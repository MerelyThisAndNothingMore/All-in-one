---
tags: 
alias:
---

点击事件用 MotionEvent 来表示，当一个点击事件产生后，事件最先传递给 [[Activity|Activity]]，0. 这会调用 Activity 的 dispatchTouchEvent()方法，当然具体的事件处理工作都是交由 Activity 中的 [[PhoneWindow]] 来完成的，然后 PhoneWindow 再把事件处理工作交给[[DecorView]]，之后再由 DecorView 将事件处理 工作交给根 [[ViewGroup]]。

# 源码分析

## ViewGroup.dispatchTouchEvent()

这里首先会执行onInterceptTouchEvent()，判断是否要拦截，如果不拦截，则依次调用子View的dispatchTouchEvent()方法。

## View.则依次调用子View的dispatchTouchEvent()

首先判断OnTouchListener是否消费事件，不消费则调用onTouchEvent(event)方法。

在onTouchEvent中会判断CLICKABLE和LONG_CLICKABLE是否为true，CLICKABLE 和 LONG_CLICKABLE 代表[[Android View]]可以被点击和长按点击，可以通过 View 的 setClickable 和 setLongClickable 方法来设置，也可以 通过 View 的 setOnClickListenter 和 setOnLongClickListener 来设置

# 传递规则

```java
public boolean dispatchTouchEvent(MotionEvent ev) {
boolean result=false;
if(onInterceptTouchEvent(ev)){
	result=super.onTouchEvent(ev);
 }else{
	result=child.dispatchTouchEvent(ev);
}

return result;
```



