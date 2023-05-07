---
tags: 
alias:
---
# 介绍
## 引入
我们知道View是通过刷新来重绘视图，系统通过发出VSSYNC信号来进行屏幕的重绘，刷新的时间间隔是16ms, 如果我们可以在16ms以内将绘制工作完成，则没有任何问题，如果我们绘制过程逻辑很复杂，并且我们的界面更新 还非常频繁，这时候就会造成界面的卡顿，影响用户体验，为此Android提供了SurfaceView来解决这一问题。他们 的UI不适合在主线程中绘制。对一些游戏画面，或者摄像头，视频播放等，UI都比较复杂，要求能够进行高效的绘 制，因此，他们的UI不适合在主线程中绘制。这时候就必须要给那些需要复杂而高效的UI视图生成一个独立的绘制表 面Surface,并且使用独立的线程来绘制这些视图UI。
## 定义
SurfaceView是View的子类，且实现了Parcelable接口，其中内嵌了一个专门用于绘制 的Surface，SurfaceView可以控制这个Surface的格式和尺寸，以及Surface的绘制位置。
可以理解为Surface就是管 理数据的地方，SurfaceView就是展示数据的地方。使用双缓冲机制，有自己的 surface，在一个独立的线程里绘 制。 
SurfaceView虽然具有独立的绘图表面，不过它仍然是宿主窗口的视图结构中的一个结点，因此，它仍然是可以 参与到宿主窗口的绘制流程中去的。从SurfaceView类的成员函数draw和dispatchDraw的实现就可以看出， SurfaceView在其宿主窗口的绘图表面上面所做的操作就是将自己所占据的区域绘为黑色，除此之外，就没有其它更 多的操作了，这是因为SurfaceView的UI是要展现在它自己的绘图表面上面的。 
## 特点
优点: 使用双缓冲机制，可以在一 个独立的线程中进行绘制，不会影响主线程，播放视频时画面更流畅 
缺点:Surface不在View hierachy中，它的显示 也不受View的属性控制，SurfaceView 不能嵌套使用。在7.0版本之前不能进行平移，缩放等变换，也不能放在其它 ViewGroup中，在7.0版本之后可以进行平移，缩放等变换。


# View 和 SurfaceView的区别
View适用于主动更新的情况，而SurfaceView则适用于被动更新的情况，比如频繁刷新界面。 View在主线程中 对页面进行刷新，而SurfaceView则开启一个子线程来对页面进行刷新。 View在绘图时没有实现双缓冲机制， SurfaceView在底层机制中就实现了双缓冲机制。
# SurfaceView、TextureView、SurfaceTexture、GLSurfaceView

SurfaceView:使用双缓冲机制，有自己的 surface，在一个独立的线程里绘制，Android7.0之前不能平移、缩放 TextureView:它不会在WMS中单独创建窗口，而是作为一个普通View，可以和其它普通View一样进行移动，旋 转，缩放，动画等变化。值得注意的是TextureView必须在硬件加速的窗口中。 SurfaceTexture:SurfaceTexture和 SurfaceView不同的是，它对图像流的处理并不直接显示，而是转为OpenGL外部纹理，因此可用于图像流数据的二 次处理(如Camera滤镜，桌面特效等)。 GLSurfaceView:SurfaceView不同的是，它加入了EGL的管理，并自带 了渲染线程。





