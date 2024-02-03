---
tags: 
alias:
---
[[Android]]开发中，开发架构就是描述 **视图层**、**逻辑层**、**数据层** 三者之间的关系和实施：

- 视图层：用户界面，即界面的展示、以及交互事件的响应。
- 逻辑层：为了实现系统功能而进行的必要逻辑。
- 数据层：数据的获取和存储，含本地、server。

[[Android|Android]] 中常见的架构包括：[[MVC]]、[[MVP]]、[[MVVM]]。



# MVC与MVP区别

View与Model并不直接交互，而是通过与Presenter交互来与Model间接交互。而在MVC中View可以与Model直 接交互。MVP 隔离了MVC中的 M 与 V 的直接联系后，靠 Presenter 来中转，所以使用 MVP 时 P 是直接调用 View 的接口来实现对视图的操作的，



