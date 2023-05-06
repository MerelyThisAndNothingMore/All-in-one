---
tags: 
alias:
- 任务栈
---
1.android任务栈又称为Task，它是一个栈结构，具有后进先出的特性，用于存放我们的Activity组件。
2.我们每次打开一个新的Activity或者退出当前Activity都会在一个称为任务栈的结构中添加或者减少一个Activity 组件， 一个任务栈包含了一个activity的集合, 只有在任务栈栈顶的activity才可以跟用户进行交互。
3.在我们退出应用程序时，必须把所有的任务栈中所有的activity清除出栈时,任务栈才会被销毁。当然任务栈也 可以移动到后台, 并且保留了每一个activity的状态. 可以有序的给用户列出它们的任务, 同时也不会丢失Activity的状态 信息。
4.对应AMS中的ActivityRecord、TaskRecord、ActivityStack(AMS中的总结)