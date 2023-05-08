---
tags: 
alias:
---
https://liuwangshu.cn/application/jetpack/3-lifecycle-theory.html https://juejin.cn/post/6844903784166998023  
Lifecycle使用  
LifecycleObserver:是一个空方法接口，用于标识观察者，对这个 Lifecycle 对象进行监听 LifecycleOwner: 是一个接口，持有方法Lifecycle getLifecycle()。

LifecycleRegistry 类用于注册和反注册需要观察当前组件生命周期的 LifecycleObserver 1.实现LifecycleOwner重写getLifecycle 返回mLifecycleRegistry，mLifecycleRegistry不同生命周期markState 2.继承LifecycleObserver  
3.getLifecycle.addObserver注册LifecycleObserver
