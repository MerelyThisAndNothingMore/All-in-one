---
tags: 
alias:
---
# 介绍
![](https://gd-hbimg.huaban.com/a3005ced87fdb78f746f96ca53ffa6fbae8bf0d9ec87-qQPZ1q)
`Fragment`的生命周期函数很多，但其实`Fragment`中只定义了6种状态

```arduino
static final int INITIALIZING = 0;     // Not yet created.
static final int CREATED = 1;          // Created.
static final int ACTIVITY_CREATED = 2; // The activity has finished its creation.
static final int STOPPED = 3;          // Fully created, not started.
static final int STARTED = 4;          // Created and started, not resumed.
static final int RESUMED = 5;          // Created started and resumed.
```

`Fragment`的整个生命周期一直在这6个状态中流转，调用对应的生命周期方法然后进入下一个状态，如下图
![](https://gd-hbimg.huaban.com/c261cc9e3e38fb2656d86f3d236b0dc2f3265f61644c-889rg7)
# References 
