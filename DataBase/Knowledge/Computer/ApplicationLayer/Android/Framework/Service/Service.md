---
tags: 
alias:
---
# 原理
## 启动流程
![](https://gd-hbimg.huaban.com/6ed5ce04b6586ffa92d944d83b293535a67b87aee6fc-Cl4LRw) 
1.  Process A进程采用Binder IPC向system_server进程发起startService请求；
2.  system_server进程接收到请求后，向zygote进程发送创建进程的请求；
3.  zygote进程fork出新的子进程Remote Service进程；
4.  Remote Service进程，通过Binder IPC向sytem_server进程发起attachApplication请求；
5.  system_server进程在收到请求后，进行一系列准备工作后，再通过binder IPC向remote Service进程发送scheduleCreateService请求；
6.  Remote Service进程的binder线程在收到请求后，通过handler向主线程发送CREATE_SERVICE消息；
7.  主线程在收到Message后，通过发射机制创建目标Service，并回调Service.onCreate()方法。

# 使用
## 启动方式
startService
onCreate() -> onStartCommand() -> onDestroy()
bindService
onCreate() -> onbind() -> onUnbind()-> onDestroy()
### 差异分析
如果你只是想要启动一个后台服务长期进行某项任务，那么使用startService便可以了。如果你还想要与正在运行的Service取得联系，那么有两种方法:
一种是使用broadcast，
另一种是使用bindService。
#### 启动
- 如果服务已经开启，多次执行startService不会重复的执行onCreate()， 而是会调用onStart()和 onStartCommand()。
- 如果服务已经开启，多次执行bindService时,onCreate和onBind方法并不会被多次调用
#### 销毁
当执行stopService时，直接调用onDestroy方法
调用者调用unbindService方法或者调用者Context不存在了(如Activity被finish了)，Service就会调用 onUnbind->onDestroy
使用startService()方法启用服务，调用者与服务之间没有关连，即使调用者退出了，服务仍然运行。 
使用bindService()方法启用服务，调用者与服务绑定在了一起，调用者一旦退出，服务也就终止。
## 和Activity通信
### 通过Binder对象
1.Service中添加一个继承Binder的内部类，并添加相应的逻辑方法
2.Service中重写Service的onBind方法，返回我们刚刚定义的那个内部类实例
3.Activity中绑定服务,重写ServiceConnection，onServiceConnected时返回的IBinder(Service中的binder) 调用逻辑方法
### Service通过BroadCast广播与Activity通信

# Service 的 onStartCommand 方法有几种返回值?各代表什么意思?
START_NOT_STICKY
在执行完 onStartCommand 后,服务被异常 kill 掉,系统不会自动重启该服务。

START_STICKY
重传 Intent。使用这个返回值时,如果在执行完 onStartCommand 后,服务被异 常 kill 掉,系统会自动重启该服务 ，并且onStartCommand方法会执行,onStartCommand方法中的intent值为null。
适用于媒体播放器或类似服务。

START_REDELIVER_INTEN  
使用这个返回值时,服务被异 常 kill 掉,系统会自动重启该服务,并将 Intent 的值传入。 
适用于主动执行应该立即恢复的作业(例如下载文件)的服务。
# bindService和startService混合使用的生命周期以及怎么关闭
如果先startService，再bindService  
onCreate() -> onbind() -> onStartCommand()

如果先bindService，再startService  
onCreate() -> onStartCommand() -> onbind()  
如果只stopService Service的OnDestroy()方法不会立即执行,在Activity退出的时候，会执行OnDestroy。 如果只unbindService  
只有onUnbind方法会执行，onDestory不会执行 如果要完全退出Service，那么就得执行unbindService()以及stopService。