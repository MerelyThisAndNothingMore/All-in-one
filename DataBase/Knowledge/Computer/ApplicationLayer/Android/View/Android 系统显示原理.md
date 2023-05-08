---
tags: 
alias:
---
Android应用程序把经过测量、布局、绘制后的surface缓存数据、通过SurfaceFlinger把数据渲染到显示屏幕上，通过Android的刷新机制来刷新数据。
也就是说应用层负责绘制，系统层负责渲染，通过进程间通信把应用层需要绘制的数据传递到系统层服务，系统层服务通过刷新机制把数据更新到屏幕。

# 绘制原理
1.在 App 进程中创建PhoneWindow 后会创建ViewRoot。ViewRoot 的创建会创建一个 Surface壳子，请求 WMS填充Surface，WMS copyFrom() 一个 NativeSurface。

2.响应客户端事件，创建Layer(FrameBuffer)与客户端的Surface建立连接。 
3.copyFrom()的同时创建匿名共享内存SharedClient(每一个应用和SurfaceFlinger之间都会创建一个SharedClient)  

4.当客户端 addView() 或者需要更新 View 时，App 进程的SharedBufferClient 写入数据到共享内存ShareClient中,SurfaceFlinger中的 SharedBufferServer 接收到通知会将 FrameBuffer 中的数据传输到屏幕上。
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

-   1、应用层绘制到缓冲区。
-   2、SurfaceFlinger把缓冲区数据渲染到屏幕，其中使用了Android匿名共享内存SharedClient缓存需要显示的数据来达到目的.
绘制的过程首先是 
1.CPU准备数据，通过Driver层把数据交给CPU渲染，其中CPU主要负责Measure、Layout、Record、Execute的数据计算工作，
2.GPU负责Rasterization（栅格化）、渲染。因为图形API不允许CPU直接和GPU通信，所以要通过一个图形驱动的中间层来进行连接，在图形驱动里面维护了一个队列，CPU把display list（待显示的数据列表）添加到队列中，GPU从这个队列中取出数据进行绘制，最终才在显示屏上显示出来。
3.Display(屏幕或显示器)屏幕会以一定的帧率刷新，每次刷新的时候，就会从缓存区将图像数据读取显示出 来,如果缓存区没有新的数据，就一直用旧的数据，这样屏幕看起来就没有变

Android系统每隔16ms发出VSYNC信号，触发对UI进行渲染，如果每次渲染都成功，这样就能够达到流畅画面所需的60FPS。
# 刷新机制




# 渲染流程
[[VSync]]调度
消息调度：doFrame消息调度
Input处理：触摸事件的处理
动画处理：animator动画执行，对动画降帧或降复杂度
View处理：View遍历，降低页面层级
Measure
Layout
Draw
DisplayList更新：
OpenGL指令转换：绘制指令转换为OpenGL指令
指令buffer交换：OpenGL的指令交换到GPU内部执行
GPU处理：GPU对数据的处理过程
Layer合成：surface buffer合成屏幕显示buffer的流程
光栅化：将矢量图转换为位图
Display:显示控制
Buffer切换：切换屏幕显示的帧Buffer




