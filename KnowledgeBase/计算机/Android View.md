---
tags: 
aliases:
  - View
---
# 介绍


# 在Activity中获取某个View的宽高有几种方法
1. Activity/View#onWindowFocusChanged:此时View已经初始化完毕，当Activity的窗口得到焦点和失去焦 点时均会被调用一次，如果频繁地进行onResume和onPause，那么onWindowFocusChanged也会被频繁 地调用。  
2. view.post(runnable): 通过post将runnable放入ViewRootImpl的RunQueue中，RunQueue中runnable 最后的执行时机，是在下一个performTraversals到来的时候，也就是view完成layout之后的第一时间获取 宽高。 ViewTreeObserver#addOnGlobalLayoutListener:当View树的状态发生改变或者View树内部的View的可 见性发生改变时，onGlobalLayout方法将被回调。
3. View.measure(int widthMeasureSpec, int heightMeasureSpec): 
	1. match_parent 直接放弃，无法 measure出具体的宽/高。原因很简单，根据view的measure过程，构造此种MeasureSpec需要知道 parentSize，即父容器的剩余空间，而这个时候我们无法知道parentSize的大小，所以理论上不可能测量处 view的大小。
	2. wrap_content int widthMeasureSpec = View.MeasureSpec.makeMeasureSpec((1<<30)-1, View.MeasureSpec.AT_MOST); int heightMeasureSpec = View.MeasureSpec.makeMeasureSpec((1<<30)-1, View.MeasureSpec.AT_MOST); v_view1.measure(widthMeasureSpec, heightMeasureSpec);
	3. 具体的数值(dp/px) 这种模式下，只需要使用具体数值去measure即可，比如宽/高都是100px: int widthMeasureSpec = View.MeasureSpec.makeMeasureSpec(100, View.MeasureSpec.EXACTLY); int heightMeasureSpec = View.MeasureSpec.makeMeasureSpec(100, View.MeasureSpec.EXACTLY); v_view1.measure(widthMeasureSpec, heightMeasureSpec);
# 为什么onCreate获取不到View的宽高
Activity在执行完oncreate，onResume之后才创建ViewRootImpl,ViewRootImpl进行View的绘制工作 
调用链 startActivity
->ActivityThread.handleLaunchActivity
->onCreate
->完成DecorView和Activity的创建
->handleResumeActivity
->onResume()
->DecorView添加到WindowManager
->ViewRootImpl.performTraversals() 方法，测量(measure),布局(layout),绘制(draw), 从DecorView自上而下遍历整个View树。
# View#post与[[Handler]]#post的区别
对于View#post
当View已经attach到window，直接调用UI线程的Handler发送runnable。
如果View还未attach 到window，将runnable放入ViewRootImpl的RunQueue中，而不是通过MessageQueue。
RunQueue的作用类似 于MessageQueue，只不过这里面的所有runnable最后的执行时机，是在下一个performTraversals到来的时候，也 就是view完成layout之后的第一时间获取宽高，
MessageQueue里的消息处理的则是下一次loop到来的时候。
