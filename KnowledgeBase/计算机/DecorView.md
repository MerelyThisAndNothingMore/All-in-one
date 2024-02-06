---
tags: 
alias:
---

# 简介

在Android中，`DecorView`是每个[[Android Window|Window]]的最顶层视图，它是`Window`中所有视图的根视图。`DecorView`包裹了应用程序窗口的整个内容，包括应用的布局和系统级的UI元素，如状态栏和导航栏（如果窗口具有这些系统UI元素的话）。

### DecorView的结构和组成

`DecorView`实质上是一个`FrameLayout`，它包含以下主要的子视图：

1. **标题栏（Title Bar）/操作栏（Action Bar）**：如果应用程序窗口具有标题栏或操作栏，这些元素会作为`DecorView`的子视图之一。
    
2. **内容视图（Content View）**：这是开发者通过`setContentView()`方法设置的主要布局视图，它包含了应用程序的UI元素。
    
3. **系统UI（如状态栏、导航栏）**：如果窗口具有系统UI元素，这些元素会被整合到`DecorView`中，但它们的具体实现和表现可能因Android版本和设备制造商的定制而异。
    

### DecorView的作用

`DecorView`的主要作用是作为窗口内容的容器，它负责：

- **布局管理**：管理窗口中所有视图的布局，包括应用的内容视图和可选的系统UI元素。
- **事件分发**：接收并分发触摸事件、按键事件等到窗口中的其他视图。
- **绘制窗口**：在屏幕上绘制窗口的内容，包括应用的布局和系统UI。

# 生命周期

## 创建

DecorView在Activity.onCreate()时创建

## 绑定

DecorView创建后尚不能展示，还需要在Activity.onResume()方法里被添加到Window里后才能展示在用户屏幕上。


