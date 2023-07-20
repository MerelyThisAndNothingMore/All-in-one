---
tags: 
alias:
---







WindowManager是一个接口，继承自只有添加、删除、更新三个方法的ViewManager接口。 它的实现类为 WindowManagerImpl，WindowManagerImpl通过WindowManagerGlobal代理实现addView， 最后调用到 ViewRootImpl的setView 使ViewRoot和Decorview相关联。如果要对Window进行添加和删除就需要使用 WindowManager， 具体的工作则由WMS来处理，WindowManager和WMS通过Binder来进行跨进程通信。


在Activity的生命周期过程中，我们知道onCreate时界面是不可见的，要等到onResume时Activity的内容才可见。究其原因是因为onCreate中仅是创建了DecorView，并没有将其展示出来。而到onResume中，才真正去将PhoneWindow中的DecorView绘制到屏幕上。onResume是在ActivityThread的handleResumeActivity()方法开始执行的。

```java
@Override
public void handleResumeActivity(IBinder token, boolean finalStateRequest, boolean isForward,
String reason) {
    //执行Activity的onResume回调
    final ActivityClientRecord r = performResumeActivity(token, finalStateRequest, reason);
    final Activity a = r.activity;
    ...
    if (r.window == null && !a.mFinished && willBeVisible) {
        r.window = r.activity.getWindow();
        //将创建好的DecorView取出来
        View decor = r.window.getDecorView();
        decor.setVisibility(View.INVISIBLE);
        //从Activity中将WindowManager取出来，这个WindowManager是在Activity的attach方法中就初始化好了的，它是WindowManagerImpl
        ViewManager wm = a.getWindowManager();
        WindowManager.LayoutParams l = r.window.getAttributes();
        a.mDecor = decor;
        l.type = WindowManager.LayoutParams.TYPE_BASE_APPLICATION;
        l.softInputMode |= forwardBit;
        if (a.mVisibleFromClient) {
            if (!a.mWindowAdded) {
                a.mWindowAdded = true;
                //将DecorView交给WindowManagerImpl中进行添加View操作
                wm.addView(decor, l);
            } else {
                a.onWindowAttributesChanged(l);
            }
        }
    }
}
```

在handleResumeActivity最后，将DecorView交给了WindowManager的实现类WindowManagerImpl进行处理。WindowManager的addView结果有两个：

1.  DecorView被渲染绘制到屏幕上显示
2.  DecorView可以接收屏幕触摸事件
