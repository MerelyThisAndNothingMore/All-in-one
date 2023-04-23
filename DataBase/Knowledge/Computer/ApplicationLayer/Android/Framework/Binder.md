---
tags: 
alias:
- Binder IPC
---
Binder是Android中独有的一种[[IPC|进程间通信]]方式。它底层依靠mmap,只需要一次数据拷贝，把一块物理内存同时映射到内核和目标进程的[[用户空间]]。
在[[Android]]中，Binder机制包含三个部分，
Java Binder、
[[Native Binder]]、
Kernel Binder.
# 原理
## 组件视角
Binder通信采用C/S架构

可以看出无论是注册服务和获取服务的过程都需要ServiceManager，需要注意的是此处的Service Manager是指Native层的ServiceManager（C++），并非指framework层的ServiceManager(Java)。


[[Binder|Binder]]基于[[内存映射]]来实现，
1.Binder驱动在[[内核空间]]创建一个数据接收缓存区。  
2.在内核空间开辟一块内核缓存区，建立内核缓存区和数据接收缓存区之间的映射关系，以及数据接收缓存区和接收进程[[用户空间]]地址的映射关系。  
3.发送方进程通过copy_from_user()函数将数据拷贝 到内核中的内核缓存区，由于内核缓存区和接收进程的用户空间存在内存映射，因此也就相当于把数据发送到了接收进程的用户空间，这样便完成了一次进程间的通信。
# 一次Binder通信
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
