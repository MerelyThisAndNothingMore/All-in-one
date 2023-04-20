---
tags: 
alias:
- Binder IPC
---
Binder是Android中独有的一种[[IPC|进程间通信]]方式。它底层依靠mmap,只需要一次数据拷贝，把一块物理内存同时映射到内核和目标进程的[[用户空间]]。
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
