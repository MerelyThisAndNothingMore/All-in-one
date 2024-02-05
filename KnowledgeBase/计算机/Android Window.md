---
tags: 
alias:
---

# 定义

所有视图（[[View]]）和视图组（[[ViewGroup]]）的容器，提供了一个可以绘制应用界面的区域。在Android UI系统中，`Window`不仅仅是指应用程序的整个屏幕，它可以是应用内的任何可视化内容区域，包括活动窗口（[[Android Activity|Activity]] Window）、对话框窗口（[[Android Dialog|Dialog]] Window）和菜单窗口（Menu Window）等。

在Android中，Window也表现为一个抽象类，定义了窗口的基本行为，而[[PhoneWindow]]则是这个抽象的一个具体实现。

## 分类

- **应用窗口（Application Window）**：这是应用中最常见的窗口类型，每个活动（Activity）都有自己的窗口来显示应用的UI。
- **子窗口（Sub Window）**：依附于应用窗口的窗口，不能单独存在，例如对话框和弹出窗口（Pop-up Window）。
- **系统窗口（System Window）**：由系统创建，位于应用窗口之上，用于显示系统级的信息或交互元素，如状态栏和导航栏。

# 实现

在应用中，你不需要直接实例化`Window`对象。当创建一个`Activity`时，Android框架会为你创建一个`Window`实例，你可以通过`Activity`的`getWindow()`方法来获取这个`Window`对象。然后，你可以使用这个`Window`对象来访问和定制标题栏、设置窗口特性（如动画、透明度）

