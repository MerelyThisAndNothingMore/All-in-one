---
tags: 
alias:
---
# 介绍
## 定义
SurfaceFlinger服务负责绘制Android应用程序的UI，它运行在[[Android]]系统的[[system_server进程]]中，它负责管理Android系统的帧缓冲区（Frame Buffer）。

Android设备的显示屏被抽象为一个帧缓冲区，而Android系统中的SurfaceFlinger服务就是通过向这个帧缓冲区写入内容来绘制应用程序的用户界面的。

Surface 对应了一块屏幕缓冲区，是要显示到屏幕的内容 的载体。每一个 Window 都对应了一个自己的 Surface 。这里说的 window 包括 Dialog, Activity, Status Bar 等。 SurfaceFlinger 最终会把这些 Surface 在 z 轴方向上以正确的方式绘制出来(比如 Dialog 在 Activity 之上)。 SurfaceView 的每个 Surface 都包含两个缓冲区，而其他普通 Window 的对应的 Surface 则不是。
## 流程
### 启动
在SurfaceFlinger服务启动的过程中会自动创建两个线程：

其中一个线程用于监控控制台事件；
另外一个线程则用于渲染系统的UI，
### 绑定应用
每一个Android应用程序与SurfaceFlinger服务都有一个连接，这个连接都是通过一个类型为Client的Binder对象来描述的。
这些Client对象是Android应用程序连接到SurfaceFlinger服务的时候由SurfaceFlinger服务创建的，而当Android应用程序成功连接到SurfaceFlinger服务之后，就可以获得一个对应的Client对象的Binder代理接口了。有了这些Binder代理接口之后，Android应用程序就可以通知SurfaceFlinger服务来绘制自己的UI了。
ISurface 就是需要的和 SurfaceFlinger 通信的 AIDL ，每个窗口都持有一个 ISurface ，具体来说 WindowManageService 持有的 ISurface 用来进行窗口大小改变，动画等操作，ViewRootImpl 持有的 ISurface 用来进行 UI 的绘制，所以 canvas 才有用武之地
### 数据传输
Android应用程序在通知SurfaceFlinger服务来绘制自己的UI的时候，需要将UI元数据传递给SurfaceFlinger服务，例如，要绘制UI的区域、位置等信息。
一个Android应用程序可能会有很多个窗口，而每一个窗口都有自己的UI元数据，因此，Android应用程序需要传递给SurfaceFlinger服务的UI元数据是相当可观的。
在这种情况下，通过Binder进程间通信机制来在Android应用程序与SurfaceFlinger服务之间传递UI元数据是不合适的，这时候Android系统的匿名共享内存机制（Anonymous Shared Memory）就派上用场了。
