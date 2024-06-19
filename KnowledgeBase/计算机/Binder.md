---
tags:
  - Android_framework
aliases:
  - Binder IPC
---
# 特性
从[[进程间通信]]的角度看，Binder 是一种进程间通信的机制;

从 Server 进程的角度看，Binder 指的是 Server 中的 Binder 实体对象(Binder类 IBinder);

从 Client 进程的角度看，Binder 指的是对 Binder 代理对象，是 Binder 实体对象的一个远程代理

从传输过程的角度看，Binder 是一个可以跨进程传输的对象;Binder 驱动会自动完成代理对象和本地对象之间 的转换。

从Android Framework角度来说，Binder是ServiceManager连接各种Manager和相应ManagerService的桥梁 Binder跨进程通信机制:基于C/S架构，由Client、Server、ServerManager和Binder驱动组成。

  进程空间分为用户空间和内核空间。用户空间不可以进行数据交互;内核空间可以进行数据交互，所有进程共用
一个内核空间

Client、Server、ServiceManager均在用户空间中实现，而Binder驱动程序则是在内核空间中实现的;

Binder是Android中独有的一种[[进程间通信|进程间通信]]方式。它底层依靠mmap,只需要一次数据拷贝，把一块物理内存同时映射到内核和目标进程的[[用户空间]]。
在[[Android]]中，Binder机制包含三个部分，
Java Binder、
[[Native Binder]]、
Kernel Binder.
Binder 本身是为了进程间频繁-灵活的通信所设计的, 并不是为了拷贝大量数据
## 优缺点
1、效率:传输效率主要影响因素是内存拷贝的次数，拷贝次数越少，传输速率越高。从Android进程架构角度 分析:对于消息队列、Socket和管道来说，数据先从发送方的缓存区拷贝到内核开辟的缓存区中，再从内核缓存区拷 贝到接收方的缓存区，一共两次拷贝。

一次数据传递需要经历:用户空间 –> 内核缓存区 –> 用户空间，需要2次数据拷贝，这样效率不高。 而对于Binder来说，数据从发送方的缓存区拷贝到内核的缓存区，而接收方的缓存区与内核的缓存区是映射到同

一块物理地址的，节省了一次数据拷贝的过程 : 共享内存不需要拷贝，Binder的性能仅次于共享内存。

2、稳定性:上面说到共享内存的性能优于Binder，那为什么不采用共享内存呢，因为共享内存需要处理并发同步问题，容易出现死锁和资源竞争，稳定性较差。 Binder基于C/S架构 ，Server端与Client端相对独立，稳定性较好。

3、安全性:传统Linux IPC的接收方无法获得对方进程可靠的UID/PID，从而无法鉴别对方身份;而Binder机制 为每个进程分配了UID/PID，且在Binder通信时会根据UID/PID进行有效性检测。
# 原理

## 组件视角
![](https://gd-hbimg.huaban.com/d3dc2d1f8be06fbe167fb964a68e22b128c7481a8678-DGedSi)
Binder通信采用C/S架构

可以看出无论是注册服务和获取服务的过程都需要ServiceManager，需要注意的是此处的Service Manager是指Native层的ServiceManager（C++），并非指framework层的ServiceManager(Java)。

## 内核视角
[[Binder|Binder]]基于[[内存映射]]来实现，
![](https://gd-hbimg.huaban.com/f8e4c8baaa8d8ff599f15e3b396a716de35fe8fe5355-pyUyP2)
![](https://gd-hbimg.huaban.com/0af80ecc18d3c0eb2c6f64499b7c99b3c4b1b4ee1d93f-sHlBkn)

1.Binder驱动在[[内核空间]]创建一个数据接收缓存区。  
2.在内核空间开辟一块内核缓存区，建立内核缓存区和数据接收缓存区之间的映射关系，以及数据接收缓存区和接收进程[[用户空间]]地址的映射关系。  
3.发送方进程通过copy_from_user()函数将数据拷贝 到内核中的内核缓存区，由于内核缓存区和接收进程的用户空间存在内存映射，因此也就相当于把数据发送到了接收进程的用户空间，这样便完成了一次进程间的通信。
![](https://gd-hbimg.huaban.com/0af80ecc18d3c0eb2c6f64499b7c99b3c4b1b4ee1d93f-sHlBkn)

# 一次Binder通信
![](https://gd-hbimg.huaban.com/c51429c96bbe0f1968372af8cf995bb4909a67723b914-aA7tyw)
1.  Server 进程向 Binder 驱动发送 Binder 实体请求注册服务， Binder 驱动将请求转发到 ServiceManager 注册后把名字和 Binder 实体等信息填入查找表中。
2.  Client 进程在 Binder 驱动的帮助下通过名字从 ServiceManager 中获取到Server Binder 实体的引用，至此，Client与Server总算是勾搭上了。
3.  与此同时，Binder 驱动开始创建数据缓冲区并通过 ServiceManager 提供的 Server 进程信息地址使用 mmap 函数将 Server 进程用户空间地址映射到创建好的缓冲区上，为跨进程通信做好了准备。
4.  Client 进程调用 copy_from_user 函数将数据拷贝到数据缓冲区后 Binder 驱动再通知 Server 进程进行解包。
5.  Server 进程收到通知后进行解包和方法调用，并将结果写到自己的有映射的内存中。
6.  由于内核缓冲区与 Server进程用户空间地址存在映射关系的，这就相当于目标方法的结果直接传递到了内核数据缓冲区，Binder驱动通知 Client 进程调用 copy_to_user 函数将缓冲区的目标结果拷贝到自己的用户空间中。
# 简单示例
在服务端App中定义Binder行为，在客户端App中通过[[Service]]绑定获取Binder实例并执行。
## 服务端提供Binder
```java
public class GradeService extends Service {
    public static final int REQUEST_CODE=1000;
    private final Binder mBinder = new Binder() {
        @Override
        protected boolean onTransact(int code, @NonNull Parcel data, @Nullable Parcel reply, int flags) throws RemoteException {
            if (code == REQUEST_CODE) {
                // set data in 'reply'
                reply.writeInd(0)
                return true;
            }
            return super.onTransact(code, data, reply, flags);
        }
    };

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return mBinder;
    }
}
```
## 客户端获取Binder
```java
public class BinderActivity extends AppCompatActivity {
    // 远程服务的Binder代理
    private IBinder mRemoteBinder;

    private final ServiceConnection mServiceConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
            // 获取远程服务的Binder代理
            mRemoteBinder = iBinder;
        }

        @Override
        public void onServiceDisconnected(ComponentName componentName) {
            mRemoteBinder = null;
        }
    };

    // 绑定远程服务
    private void bindGradeService() {
        String action = "android.intent.action.server.gradeservice";
        Intent intent = new Intent(action);
        intent.setPackage(getPackageName());
        bindService(intent, mServiceConnection, BIND_AUTO_CREATE);
    }

    private int getStudentGrade(String name) {
        Parcel data = Parcel.obtain();
        Parcel reply = Parcel.obtain();
        int grade = 0;
        data.writeString(name);
        try {
            if (mRemoteBinder == null) {
                throw new IllegalStateException("Need Bind Remote Server...");
            }
            mRemoteBinder.transact(REQUEST_CODE, data, reply, 0);
            grade = reply.readInt();
        } catch (RemoteException e) {
            e.printStackTrace();
        }
        return grade;
    }
```
# References 
[Binder与AIDL](https://juejin.cn/post/6994057245113729038) 
[Binder系列-开篇](http://gityuan.com/2015/10/31/binder-prepare/)


