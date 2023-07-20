---
tags: 
alias:
---
# 介绍
`SharedPreferences`是[[Android]]平台上 **轻量级的存储类**，用来保存`App`的各种配置信息，其本质是一个以 **键值对**（`key-value`）的方式保存数据的`xml`文件，其保存在`/data/data/shared_prefs`目录下。

每次读取数据时，通过解析`xml`文件，得到指定`key`对应的`value`；每次更新数据，也通过文件中`key`更新对应的`value`。

## 读操作
当`SharedPreferences`对象第一次通过`Context.getSharedPreferences()`进行初始化时，对`xml`文件进行一次读取，并将文件内所有内容（即所有的键值对）缓到内存的一个`Map`中，这样，接下来所有的读操作，只需要从这个`Map`中取就可以了。

第一次读取完毕之前，所有的get/set请求都会被卡住，等待读取完毕后再执行，所以第一次读取会有ANR风险  

虽然 **内存缓存机制** 表面上看起来好像是一种 **空间换时间** 的权衡，实际上规避了短时间内频繁的`I/O`操作对性能产生的影响，而通过良好的代码规范，也能够避免该机制可能会导致内存占用过高的副作用，所以这种设计是 **值得肯定** 的。
## 写操作
在复杂的业务中，有时候一次操作会导致多个键值对的更新，这时，与其多次更新文件，我们更倾向将这些更新 **合并到一次写操作** 中，以达到性能的优化。

因此，对于`SharedPreferences`的写操作，设计者抽象出了一个`Editor`类，不管某次操作通过若干次调用`putXXX()`方法，更新了几个`xml`中的键值对，只有调用了`commit()`方法，最终才会真正写入文件。

提交都是写入到内存和磁盘中，

apply跟commit的区别在于apply是内存同步，然后磁盘异步写入任务放到一个单线程队列中等待调用，方法无返回，即void  

commit内存同步，只不过要等待磁盘写入结束才返回，直接返回写入成功状态 true or false

## 数据更新

`xml`中数据是如何更新的？读者可以简单理解为 **全量更新** ——通过上文，我们知道`xml`文件中的数据会缓存到内存的`mMap`中，每次在调用`editor.putXXX()`时，实际上会将新的数据存入在`mMap`，当调用`commit()`或`apply()`时，最终会将`mMap`的所有数据全量更新到`xml`文件里。

`xml`中数据量的大小，的确会对 **写操作** 的成本有一定的影响，因此，设计者更建议将 **不同业务模块的数据分文件存储** ，即根据业务将数据存放在不同的`xml`文件中。

因此，不同的`xml`文件应该对应不同的`SharedPreferences`对象，如果想要对某个`xml`文件进行操作，就通过传不同的文件标识符，获取对应的`SharedPreferences`

## 线程安全
SharePreferences是线程安全的，里面的方法有大量的synchronized来保障  

SharePreferences不是进程安全的，即使你用了MODE_MULTI_PROCESS

### 加锁

为了保证`SharedPreferences`是线程安全的，`Google`的设计者一共使用了3把锁

```java
final class SharedPreferencesImpl implements SharedPreferences {
  // 1、使用注释标记锁的顺序
  // Lock ordering rules:
  //  - acquire SharedPreferencesImpl.mLock before EditorImpl.mLock
  //  - acquire mWritingToDiskLock before EditorImpl.mLock

  // 2、通过注解标记持有的是哪把锁
  @GuardedBy("mLock")
  private Map<String, Object> mMap;

  @GuardedBy("mWritingToDiskLock")
  private long mDiskStateGeneration;

  public final class EditorImpl implements Editor {
    @GuardedBy("mEditorLock")
    private final Map<String, Object> mModified = new HashMap<>();
  }
}

```

对于简单的 **读操作** 而言，我们知道其原理是读取内存中`mMap`的值并返回，那么为了保证线程安全，只需要加一把锁保证`mMap`的线程安全即可：

```java
public String getString(String key, @Nullable String defValue) {
    synchronized (mLock) {
        String v = (String)mMap.get(key);
        return v != null ? v : defValue;
    }
}
```
对于写操作而言，每次`putXXX()`并不能立即更新在`mMap`中，这是理所当然的，如果开发者没有调用`apply()`方法，那么这些数据的更新理所当然应该被抛弃掉，但是如果直接更新在`mMap`中，那么数据就难以恢复。

因此，`Editor`本身也应该持有一个`mEditorMap`对象，用于存储数据的更新；只有当调用`apply()`时，才尝试将`mEditorMap`与`mMap`进行合并，以达到数据更新的目的。

因此，这里我们还需要另外一把锁保证`mEditorMap`的线程安全，笔者认为，不和`mMap`公用同一把锁的原因是，在`apply()`被调用之前，`getXXX`和`putXXX`理应是没有冲突的。

```java
public final class EditorImpl implements Editor {
  @Override
  public Editor putString(String key, String value) {
      synchronized (mEditorLock) {
          mEditorMap.put(key, value);
          return this;
      }
  }
}
```

而当真正需要执行`apply()`进行写操作时，`mEditorMap`与`mMap`进行合并，这时必须通过2把锁保证`mEditorMap`与`mMap`的线程安全，保证`mMap`最终能够更新成功，最终向对应的`xml`文件中进行更新。

文件的更新理所当然也需要加一把锁：

```java
// SharedPreferencesImpl.EditorImpl.enqueueDiskWrite()
synchronized (mWritingToDiskLock) {
    writeToFile(mcr, isFromSyncCommit);
}

```

最终，我们一共通过使用了3把锁，对整个写操作的线程安全进行了保证。

## 进程安全

由于没有使用跨进程的锁，`SharedPreferences`是进程不安全的，在跨进程频繁读写会有数据丢失的可能

## apply 导致ANR

`apply()`方法设计的初衷是为了规避主线程的`I/O`操作导致`ANR`问题的产生

在`apply()`方法中，首先会创建一个等待锁，根据源码版本的不同，最终更新文件的任务会交给`QueuedWork.singleThreadExecutor()`单个线程或者`HandlerThread`去执行，当文件更新完毕后会释放锁。

但当`Activity.onStop()`以及`Service`处理`onStop`等相关方法时，则会执行 `QueuedWork.waitToFinish()`等待所有的等待锁释放，因此如果`SharedPreferences`一直没有完成更新任务，有可能会导致卡在主线程，最终超时导致`ANR`。

## 文件损坏 & 备份机制

由于不可预知的原因（比如内核崩溃或者系统突然断电），`xml`文件的 **写操作** 异常中止，`Android`系统本身的文件系统虽然有很多保护措施，但依然会有数据丢失或者文件损坏的情况。

作为设计者，如何规避这样的问题呢？答案是对文件进行备份，`SharedPreferences`的写入操作正式执行之前，首先会对文件进行备份，将初始文件重命名为增加了一个`.bak`后缀的备份文件：

这之后，尝试对文件进行写入操作，写入成功时，则将备份文件删除。

通过文件备份机制，我们能够保证数据只会丢失最后的更新，而之前成功保存的数据依然能够有效。

# 优化方向
-   建议在Application中初始化，重写attachBaseContext()，参数context直接传入Application对象即可。
-   最好使用单例，不必每次都从系统获取Sp对象，减少开销。
-   如果项目中使用了MultiDex，存在分包，请在分包前即MultiDex.install()之前或者在multidex执行的这段时间初始化。因为这时cpu是利用不满的，我们没有办法充分利用CPU的原因，是因为如果我们在Multidex之前执行一些操作，我们很有可能因为这样一些操作的类或者是相关的类不在我们的主dex当中，在四点几版本中会直接崩溃，但是由于sharePreference不会产生这种崩溃，它是系统的类。
-   请不要使用SharedPreference存储大文件及存储大量的key和value，这样的话会造成界面卡顿或者ANR比较占内存，记住它是简单存储，如果有类似的需求请考虑数据库、磁盘文件存储等等。
-   推荐使用apply进行存储，这也是官方推荐，当读入内存后，因为它是异步写入磁盘的，所以效率上会比commit好，如果你需要存储状态或者即存即用的话还是尽量使用commit。
-   请不要频繁使用apply与commit，如果存在这样的问题，请合并一次性apply与commit,可以参考封装一个map的结合，一次性提交，因为SharedPreference可能会存在IO瓶颈和锁性能差的问题。
-   尽量不要存放Json及html，数据少可以，无需担心，大量的话请放弃。
-   不要所有的数据都存在一个文件，不同类型的文件或者数据可以分开多个文件存储，避免使用一个大文件，这样可提高读取速度。
-   跨进程操作不要使用MULTI_PROCESS标志，而是使用contentprovide等进程间通信的方式。



