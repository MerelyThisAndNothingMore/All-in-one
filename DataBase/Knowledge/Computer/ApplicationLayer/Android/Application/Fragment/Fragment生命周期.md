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
![](https://gd-hbimg.huaban.com/f0874a1383bbdbf4b744bc4cfc5e59756c2499e09128-tsT0or)
`Fragment`的生命周期与`Activity`的生命周期密切相关 `Activity`管理`Fragment`生命周期的方式是在`Activity`的生命周期方法中调用`FragmentManager`的对应方法，通过`FragmentManager`将现有的`Fragment`迁移至下一个状态，同时触发相应的生命周期函数。







# References 
