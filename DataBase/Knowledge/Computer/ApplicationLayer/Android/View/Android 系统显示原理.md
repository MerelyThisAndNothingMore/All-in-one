---
tags: 
alias:
---
Android应用程序把经过测量、布局、绘制后的surface缓存数据、通过SurfaceFlinger把数据渲染到显示屏幕上，通过Android的刷新机制来刷新数据。
也就是说应用层负责绘制，系统层负责渲染，通过进程间通信把应用层需要绘制的数据传递到系统层服务，系统层服务通过刷新机制把数据更新到屏幕。

# 绘制原理
## 应用层
在Android的每个View都会经过Measure和Layout来确定当前需要绘制的View所在的大小和位置，然后，再通过Draw绘制到surface上。在Android系统中整体的绘制源码是在ViewRootImpl类的performTraversals()方法，通过这个方法可以看出Measure和Layout都是递归来获取View的大小和位置，并且以深度作为优先级。显然，层级越深，元素越多，耗时就越长。

## 系统层
将数据渲染到屏幕上是通过系统级进程中的SurfaceFlinger服务来实现的，它的主要工作流程如下：

-   1、响应客户端事件，创建Layer与客户端的Surface建立连接。
-   2、接收客户端数据和属性，修改Layer属性，如尺寸、颜色、透明度等。
-   3、将创建的Layer内容刷新到屏幕上。
-   4、维持Layer的序列，并对Layer的最终输出做裁剪计算。

其中，SurfaceFlinger系统进程和应用进程使用了匿名共享内存SharedClient，并且，每一个应用和SurfaceFlinger之间都会创建一个SharedClient，在每个SharedClient中，最多可以创建31个SharedBufferStack，每一个SharedBufferStack对应一个Surface，即一个window。
（其中包含了两个（小于4.1版本）或者三个（4.1及以上版本）缓冲区）

因此，从上可知，**一个Android应用程序最多可以包含31个窗口**。最后，显示的整体流程如下：

-   1、**应用层绘制到缓冲区**。
-   2、**SurfaceFlinger把缓冲区数据渲染到屏幕，其中使用了Android匿名共享内存SharedClient缓存需要显示的数据来达到目的**。









