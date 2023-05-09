---
tags: 
alias:
---

# 非UI线程更新UI的异常是从哪抛出的呢？ 
答：android.view.ViewRootImpl#checkThread，因此刷新View时只要不触发checkThread()就不会抛出异常。 

# UI线程是什么？ 
答：Android的App进程是由zygote进程fork出的新进程，此进程会执行Android的入口函数ActivityThread#main()方法，并开启Kooper#loop()，此时应用就启动并运行于前台。
因此UI线程即main函数运行的线程。 
# 主线程如何工作？ 
答：主线程通过Looper#loop开启死循环，一直轮询从Handler发送到MessageQueue消息队列中的消息，然后再分发给Handler进行处理，来保证程序一直运行于前台。  
# UI为什么不设计成线程安全的？ 
1. UI具有可变性，甚至是高频可变性 
2. UI对响应时间的敏感性要求UI操作必须高效 
3. UI组件必须批量绘制来保证效率 
4. 若设计成线程安全，则需要频繁的加锁，开销太大

# 非UI线程一定不能更新UI吗？ 
场景：IO线程(网络请求) UI线程(刷新UI) 
正常操作：
网络请求回来后，通过Handler#post/sendMessage发送消息，然后主线程Handler中处理View刷新 

间接在非UI线程刷新：
调用View#postInvalidate 

ViewRootImpl未初始化前在非UI线程更新：
ViewRootIml是View的根类，是在onResume生命周期创建，因此在onCreate和onStart生命周期中在子线程(线程不能睡眠)里可以更改UI。 

通过Looper实现在子线程使用Toast： 
```java
new Thread(new Runnable() { 
@Override 
public void run() {
try { 
Thread.sleep(2000); 
} catch (InterruptedException e) { e.printStackTrace(); } 


Looper.prepare(); 
Toast.makeText(mContext, "子线程弹Toast", Toast.LENGTH_SHORT).show(); 
Looper.loop(); 
} 
}).start();
  

```


SurfaceView非UI线程刷新及绘制：
SurfaceView一方面可以实现复杂而高效的UI，另一方面又不会导致用户输入得不到及时响应。常用于画面内容更新频繁的场景，比如游戏、视频播放和相机预览。 
使用SurfaceView的三步骤: 
1、获取SurfaceHolder对象，其是SurfaceView的内部类。添加回调监听Surface生命周期。 mSurfaceHolder = getHolder(); mSurfaceHolder.addCallback(this); 
2、surfaceCreated 回调后启动绘制线程。只有当native层的Surface创建完毕之后，才可以调用lockCanvas()，否则失败。 @Override public void surfaceCreated(SurfaceHolder holder) { mDrawThread = new DrawThread(); mDrawThread.start(); } 
3、绘制 Canvas canvas = mSurfaceHolder.lockCanvas(); // 使用canvas绘制内容 mSurfaceHolder.unlockCanvasAndPost(canvas); 

使用SurfaceView不显示问题：
发生这种问题的原因是多层嵌套被遮挡 
setZOrderOnTop(boolean onTop) // 在最顶层，会遮挡一切view 
setZOrderMediaOverlay(boolean isMediaOverlay)// 如已绘制SurfaceView则在surfaceView上一层绘制。 

黑色背景问题：mHolder.setFormat(PixelFormat.TRANSPARENT); //设置背景透明

  




