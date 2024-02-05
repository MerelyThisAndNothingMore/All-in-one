---
tags: 
alias:
---

`WindowManager`是一个系统服务，负责管理应用窗口的创建、销毁、显示和更新等操作。它是Android窗口系统的核心组成部分，为窗口([[Android Window]])之间的交互和窗口的布局提供了基础支持。

### WindowManager的主要功能

1. **窗口管理**：`WindowManager`允许应用程序添加、移除和更新窗口。每个窗口都是通过`WindowManager`创建的`Window`对象的实例，它可以是应用窗口、对话框、系统窗口等。
    
2. **布局和层级**：`WindowManager`负责窗口的Z顺序，即窗口在屏幕上的堆叠顺序。这决定了哪个窗口显示在前面，哪个显示在后面。
    
3. **输入事件分发**：`WindowManager`接收用户的输入事件（如触摸、按键事件），并将这些事件分发到适当的窗口和视图。
    
4. **窗口参数和特性**：通过`WindowManager.LayoutParams`类，`WindowManager`允许开发者为窗口设置多种参数和特性，如大小、位置、透明度、动画等。
    

### WindowManager的使用

在Android应用开发中，通常不需要直接与`WindowManager`交互来管理窗口。大部分窗口管理任务，如创建活动窗口（`Activity` Window）和显示对话框（`Dialog`），都是由Android框架自动处理的。但在某些特定场景下，开发者可能需要直接使用`WindowManager`，例如：

- **自定义悬浮窗（Floating Window）**：在需要在其他应用或桌面上显示悬浮窗口时，可以使用`WindowManager`来创建和控制这种窗口。
- **系统级UI的交互**：在开发系统应用或具有系统级权限的应用时，可能需要使用`WindowManager`来管理系统窗口。

### 获取WindowManager实例

可以通过`Context.getSystemService(Context.WINDOW_SERVICE)`方法获取`WindowManager`的实例，例如：

```java
WindowManager windowManager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
```

### WindowManager.LayoutParams

`WindowManager.LayoutParams`是一个内嵌类，定义了窗口的布局参数。这些参数包括窗口的宽度、高度、类型（如系统窗口或应用窗口）、标志（如是否模糊背景）等。通过设置这些参数，可以控制窗口的外观和行为。

### 注意事项

- **权限**：直接使用`WindowManager`添加或修改窗口可能需要特定的权限，特别是对于系统窗口或在其他应用上方显示窗口。例如，显示悬浮窗可能需要`android.permission.SYSTEM_ALERT_WINDOW`权限。
- **资源管理**：使用`WindowManager`创建的窗口需要适当的管理，以确保窗口在不需要时被移除，避免内存泄漏。


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

