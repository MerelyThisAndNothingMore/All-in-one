---
tags: 
alias:
- 任务栈
---

![](https://gd-hbimg.huaban.com/b7bbadd2a0d17f9087683c9694977bd3ca186ced20f2-iYHVLL)
-   一个`ActivityRecord`对应一个`Activity`，保存了一个`Activity`的所有信息;但是一个`Activity`可能会有多个`ActivityRecord`,因为`Activity`可以被多次启动，这个主要取决于其启动模式。
-   一个`TaskRecor`d由一个或者多个`ActivityRecord`组成，这就是我们常说的任务栈，具有后进先出的特点。
-   `ActivityStack`则是用来管理`TaskRecord`的，包含了多个`TaskRecord`。


1.android任务栈又称为Task，它是一个栈结构，具有后进先出的特性，用于存放我们的Activity组件。
2.我们每次打开一个新的Activity或者退出当前Activity都会在一个称为任务栈的结构中添加或者减少一个Activity 组件， 一个任务栈包含了一个activity的集合, 只有在任务栈栈顶的activity才可以跟用户进行交互。
3.在我们退出应用程序时，必须把所有的任务栈中所有的activity清除出栈时,任务栈才会被销毁。当然任务栈也 可以移动到后台, 并且保留了每一个activity的状态. 可以有序的给用户列出它们的任务, 同时也不会丢失Activity的状态 信息。
4.对应AMS中的ActivityRecord、TaskRecord、ActivityStack(AMS中的总结)