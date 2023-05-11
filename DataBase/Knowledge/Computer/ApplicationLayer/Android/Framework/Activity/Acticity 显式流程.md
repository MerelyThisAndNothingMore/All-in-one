---
tags: 
alias:
---
1. Activity 的起始是 ActivityThread.performLaunchActivity() 这个方法，里面 Instrumentation new 了一个 Activity 对象出来，然后 Activity.attch 进行 初始化，最后触发 Activity.onCreate
2. 可以看到在 Activity.attach 里，创建了 PhoneWindow ，并给 PhoneWindow 绑定了管理器 WindowManage ，这里 window，WindowManage 就初始化好了
3. 下面就会执行 Activity.onCreate 方法了，onCreate 里面有什么呢，就是 setContentView ，这里进行 window UI 部分的初始化了，PhoneWindow 干了2件事，第一个就是 installDecor 把 window 窗口的根视图 DecorView new 出来，另一件事就是把我们的内容视图加载出来添加到 DecorView 里面
4. 这回视图 view 都出来了，后面会继续走 Activity 的生命周期，一直到 onResume 之后,发送一个 MSG_RESUME_PENDING 消
5. handleResumeActivity 方法里面最主要的是调用了 windowManage.addView
6. new 了 ViewRootImpl 对象出来，然后把 DocverView 和 ViewRootImpl 存到 windowManage 里面，最后调用 ViewRootImpl.setView 正式开始启动 UI 的显示逻辑了，
7. ViewRootImpl 的 setView 函数就干了这2件事
8. requestLayout 函数是给 UI 线程添加了一个 callback 任务，这个任务只有在 UI 线程空闲时才会执行，也就是说会在 ViewRootImpl 的 setView 方法执行完后才执行，这个任务就是 ViewRootImpl.performTraversals() 这个方法会测量 widnow 窗口的大小，然后请求 WindowManageService 去 SurfaceFlinger 申请显存，然后遍历 viewTree，对所有 view 进行 measure，layout，draw，这样页面就可以在屏幕上显示出来了
9. addToDisplay 最终会调用 WindowManageService 的 addWindow 函数，在 WindowManageService 端生成对应的 window 对象，获取和 SurfaceFlinger 通信的 AIDL 对象 ISurface，并分别返回给 WindowManageService 和 ViewRootImpl ，有了 ISurface 之后，我们才能通过 ISurface 和 SurfaceFlinger 通信，申请显存，才能绘制图形出来
10. requestLayout() 虽说是写在 addToDisplay() 之前的，但是他是添加了一个 handle 任务，所以是先执行 addToDisplay ，然后才是 requestLayout，
ddToDisplay 是在 WindowManageService 系统进程里执行的，这时已经不在应用进程了

应用进程把 IWindow 这个 AIDL 传进来，实现 WindowManageService 和 应用进程的通信，就会在创建一个 WindowState 对象来描述与该 IWindow 所关联的 Window 窗口状态，并且以后就通过这个 IWindow 对象来控制对应的 Window 窗口状态
每一个Activity组件在 ActivityManagerService 服务内部，都对应有一个 ActivityRecord 对象，这个 ActivityRecord 对象是 Activity 组件启动的过程中创建的，用来描述 Activity 组件的运行状态。这样，每一个 Activity 组件在应用程序进程、WindowManagerService 服务和 ActivityManagerService 服务三者之间就分别一一地建立了连接
接着说 addToDisplay ，addWindow 中创建 WindowState 对象后，会触发 WindowState .attach ,该函数会创建一个关联的 SurfaceSession 对象，以便可以用来和 SurfaceFlinger 服务通信
SurfaceSession 的构造方法内会创建和 SurfaceFlinger 服务通信的 AIDL SurfaceComposerClient，通过 SurfaceComposerClient 申请创建绘制表面 Surface 的操作






  
  









