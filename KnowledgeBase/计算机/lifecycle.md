---
tags: 
alias:
---
# 定义

Lifecycle，顾名思义，是用于帮助开发者管理[[Activity]]和[[Fragment]] 的生命周期，它是[[LiveData]]和[[ViewModel]]的基础。

# 使用

- 1、生命周期拥有者 使用getLifecycle()获取Lifecycle实例，然后代用addObserve()添加[[观察]]者；
- 2、观察者实现LifecycleObserver，[[方法]]上使用OnLifecycleEvent注解关注对应生命周期，生命周期触发时就会执行对应方法；

# 原理

![](https://ask.qcloudimg.com/http-save/yehe-1094110/b7f047f844b431896d1e047f721b9df4.png)

Lifecycle使用两个枚举来跟踪其关联组件的生命周期状态，这两个枚举分别是Event和State。  
State指的是Lifecycle的生命周期所处的状态。  
Event代表Lifecycle生命周期对应的事件，这些事件会映射到Activity和Fragment中的回调事件中。


