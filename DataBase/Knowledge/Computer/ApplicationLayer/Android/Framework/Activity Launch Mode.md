---
tags: 
alias:
- activity启动模式
---
Activity的启动模式决定了活动启动时要进入的任务栈中已经存在的活动实例是否会被重用。
Android支持4种启动模式:
1. standard:默认模式。每次启动Activity都会在任务栈中创建一个新的实例。
2. singleTop:如果栈顶已经有该Activity的实例,那么会重用该实例,否则创建新实例入栈。
3. singleTask:每次启动该Activity都会创建一个新的任务栈进行入栈操作。
4. singleInstance:同singleTask,但新的任务栈只能容纳该Activity的实例。