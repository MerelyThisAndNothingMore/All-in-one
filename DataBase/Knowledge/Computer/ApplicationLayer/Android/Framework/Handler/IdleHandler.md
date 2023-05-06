---
tags: 
alias:
---
[[Handler]]提供的一种，可以在 Looper 事件循环的过程中，当出现空闲的时候，允许我们执行任务的一种机制. 
IdleHandler在looper里面的message处理完了的时候去调用
怎么使用 
IdleHandler 被定义在 MessageQueue 中，它是一个接口. 定义时需要实现其 queueIdle() 方法。返回值为 true 表示是一个持久的 IdleHandler 会重复使 用，返回 false 表示是一个一次性的 IdleHandler。
IdleHandler 被 MessageQueue 管理，对应的提供了 addIdleHandler() 和 removeIdleHandler() 方法。将其存入将其存入 mIdleHandlers 这个 ArrayList 中。 
什么时候掉用 
就在MessageQueue的 next方法里面。 
MessageQueue 为空，没有 Message; 
MessageQueue 中最近待处理的 Message，是一个延迟消息(when>currentTime)，需要滞后执行; 
使用场景 

1.Activity启动优化:onCreate，onStart，onResume中 耗时较短但非必要的代码可以放到IdleHandler中执行，减少启动时间

2.想要在一个View绘制完成之后添加其他依赖于这个View的View，当然这个用View#post()也能实现，区别就是 前者会在消息队列空闲时执行 
优化页面的启动,较复杂的view填充 填充里面的数据界面view绘制之前的话，就会出现 以上的效果了，view先是白的，再出现. app的进程其实是ActivityThread,performResumeActivity先回调 onResume ， 之后 执行view绘制的measure, layout, draw,也就是说onResume的方法是在绘制之前，在 onResume中做一些耗时操作都会影响启动时间 把在onResume以及其之前的调用的但非必须的事件(如某些界面 View的绘制)挪出来找一个时机(即绘制完成以后)去调用即可。