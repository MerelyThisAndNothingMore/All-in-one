---
tags: 
alias:
---

[[Android Window]]是一个抽象类，定义了窗口的基本行为，而`PhoneWindow`则是这个抽象的一个具体实现，提供了Android系统标准窗口的大部分功能:

1. **UI呈现**：`PhoneWindow`负责创建和管理窗口的根视图（[[DecorView]]），这个根视图是所有窗口内容的容器。`PhoneWindow`通过这个根视图来呈现应用的用户界面。
    
2. **视图管理**：`PhoneWindow`管理着窗口中的视图层次结构，包括标题栏（ActionBar）、内容视图（ContentView）等，并处理视图的添加、移除和布局。
    
3. **事件分发**：`PhoneWindow`接收来自用户的输入事件（如触摸、按键事件）并将这些事件分发到视图层次结构中的适当视图。
    
4. **窗口特性**：`PhoneWindow`提供了设置和管理窗口特性的能力，如全屏模式、标题栏的显示与隐藏、窗口动画等。
    
5. **主题和样式**：`PhoneWindow`支持通过主题（Themes）和样式（Styles）来定制窗口的外观和行为。开发者可以在应用的主题中声明窗口相关的属性，`PhoneWindow`会在窗口创建时应用这些属性。

在Android应用中，每个`Activity`都有一个与之关联的`Window`对象，用于呈现该`Activity`的界面。在大多数情况下，这个`Window`对象实际上是一个`PhoneWindow`实例。当[[Activity]]被创建时，系统会为其创建一个`PhoneWindow`对象，并通过这个`PhoneWindow`来加载和显示`Activity`的布局。