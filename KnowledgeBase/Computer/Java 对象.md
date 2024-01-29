---
tags: 
alias:
---
# 创建流程
![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1kY2FjYTJhYTc4NDMwMGZjLnBuZw?x-oss-process=image/format,png)


## 类加载检查

当[[Java 虚拟机]]遇到一条字节码new指令时，
首先将去检查这个指令的参数是否能在常量池中定位到 一个类的符号引用，
并且检查这个符号引用代表的类是否已被加载、解析和初始化过。
如果没有，那必须先执行相应的[[Java 类加载|类加载过程]]

## 内存分配

在类加载检查通过后，接下来虚拟机将为新生对象分配[[内存]]。对象所需内存的大小在类加载完成后便可完全确定，为对象分配空间的任务实际上便等同于把一块确定大小的内存块从[[Java 堆]]中划分出来。

分配内存时，如果内存规整，使用[[指针]]碰撞，如果不规整，则使用空闲列表。内存是否规整取决于垃圾收集器是否有空间整理能力。

对象创建在虚拟机中是非常频繁的行为，即使仅仅修改一个指针所指向的位置，在并发情况下也并不是[[线程安全]]的，这里有两种处理思路：

1. 对分配内存空间的动作进行同步处理——实际上虚拟机是采用CAS配上失败 重试的方式保证更新操作的原子性;
2. 把内存分配的动作按照线程划分在不同的空间之中进 行，即每个线程在Java堆中预先分配一小块内存，称为本地线程分配缓冲(Thread Local Allocation Buffer，TLAB)，哪个线程要分配内存，就在哪个线程的本地缓冲区中分配，只有本地缓冲区用完 了，分配新的缓存区时才需要同步锁定。

## 初始化

1. 初始化内存空间为零
2. 根据对象头中的数据，配置对象的初始信息。


# 对象的内存布局
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cc597c75fe5342cd9b3a253980f95524~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

-   问题：在 `Java` 对象创建后，到底是如何被存储在Java内存里的呢？
-   答：在`Java`虚拟机（`HotSpot`）中，对象在 `Java` 内存中的 存储布局 可分为三块：
1.  对象头（Header）
2.  实例数据（Instance Data）
3.  对齐填充（Padding）
![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1mOWQyNTk3NTczMjE2NGJjLnBuZw?x-oss-process=image/format,png)
# 对象的访问定位
问：建立对象后，该如何访问对象呢？
实际上需访问的是 对象类型数据 & 对象实例数据

答：Java程序 通过 栈上的引用类型数据（reference） 来访问Java堆上的对象
由于引用类型数据（reference）在 Java虚拟机中只规定了一个指向对象的引用，但没定义该引用应该通过何种方式去定位、访问堆中的对象的具体位置

所以对象访问方式取决于虚拟机实现。目前主流的对象访问方式有两种：

句柄 访问
直接指针 访问
![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS0yZjQ5MjgxNzNlNzM0ZTNlLnBuZw?x-oss-process=image/format,png)

# Java对象如何判断存活
## 引用计数法
### 方式描述
给 Java 对象添加一个引用计数器
每当有一个地方引用它时，计数器 +1；引用失效则 -1；
### 判断对象存活准则
当计数器不为 0 时，判断该对象存活；否则判断为死亡（计数器 = 0）。

### 优点
实现简单
判断高效
### 缺点
无法解决 对象间相互循环引用 的问题
即该算法存在判断逻辑的漏洞
## 引用链法（可达性分析法）
![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS0xYTkxYTgzMWM0ZmNmYjgwLnBuZw?x-oss-process=image/format,png)

-   很多主流商用语言（如`Java`、`C#`）都采用 **引用链法** 判断 `Java`对象是否存活。
-   含3个步骤：
    1.  可达性分析
    2.  第一次标记 & 筛选
    3.  第二次标记 & 筛选
### 可达性分析
将一系列的 `GC Roots` 对象作为起点，从这些起点开始向下搜索。

>    可作为 `GC Root` 的对象有：  
>     1.`Java`虚拟机栈（栈帧的本地变量表）中引用的对象  
>     2.本地方法栈 中 `JNI`引用对象  
>     3.方法区 中常量、类静态属性引用的对象
>   向下搜索的路径 = 引用链

可达性分析 仅仅只是判断对象是否可达，但还不足以判断对象是否存活 / 死亡
当在 可达性分析 中判断不可达的对象，只是“被判刑” = 还没真正死亡
不可达对象会被放在”即将回收“的集合里。

要判断一个对象真正死亡，还需要经历两个阶段：
第一次标记 & 筛选
第二次标记 & 筛选

### 第一次标记 & 筛选
对象 在 可达性分析中 被判断为不可达后，会被第一次标记 & 准备被筛选
a. 不筛选：继续留在 ”即将回收“的集合里，等待回收；
b. 筛选：从 ”即将回收“的集合取出

筛选的标准：该对象是否有必要执行 finalize()方法
若有必要执行（人为设置），则筛选出来，进入下一阶段（第二次标记 & 筛选）；
若没必要执行，判断该对象死亡，不筛选 并等待回收
当对象无 finalize()方法 或 finalize()已被虚拟机调用过，则视为“没必要执行”
### 第二次标记 & 筛选
当对象经过了第一次的标记 & 筛选，会被进行第二次标记 & 准备被进行 筛选

a. 方式描述
该对象会被放到一个 F-Queue 队列中，并由 虚拟机自动建立、优先级低的Finalizer 线程去执行队列中该对象的finalize()
finalize()只会被执行一次
但并不承诺等待finalize()运行结束。这是为了防止 finalize()执行缓慢 / 停止 使得 F-Queue队列其他对象永久等待。
b. 筛选标准
在执行finalize()过程中，若对象依然没与引用链上的GC Roots 直接关联 或 间接关联（即关联上与GC Roots 关联的对象），那么该对象将被判断死亡，不筛选（留在”即将回收“集合里） 并 等待回收





