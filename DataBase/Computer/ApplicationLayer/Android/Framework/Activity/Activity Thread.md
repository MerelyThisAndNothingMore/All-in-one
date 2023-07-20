---
tags: 
alias:
---

# 定义
ActivityThread就是我们常说的主线程或UI线程，ActivityThread的main方法是整个APP的入口

## 初始化

ActivityThread的main方法是一个APP的真正入口

主线程的Handler以及MainLooper的初始化时机都是在ActivityThread创建的时候




ActivityThread 在Android中它就代表了Android的主线程,它是创建完新进程之后,main函数被加载，然后执行一个loop的循环
从消息队列中取消息可能会阻塞，取到消息会做出相应的处理。如果某个消息处理时间过长，就可能会影响UI线程的刷新速率，造成卡顿的现象。

使当前线程进入消息循环，并且作为主线程。

ApplicationThread  
ApplicationThread是ActivityThread的内部类， 是一个Binder对象。在此处它是作为IApplicationThread对象

的server端等待client端的请求然后进行处理，最大的client就是AMS。



在Activity的onSaveInstanceState方法回调时，put到参数outState(Bundle)里面。outState就是 ActivityClientRecord的state。

ActivityClientRecord实例，都存放在ActivityThread的mActivities里面。

# 主线程的死循环一直运行是不是特别消耗CPU资源呢？

其实不然，这里就涉及到Linux pipe/epoll机制，简单说就是在主线程的MessageQueue没有消息时， 便阻塞在loop的queue.next()中的nativePollOnce()方法里，此时主线程会释放CPU资源进入休眠状态，直到下个消息到达或者有事务发生， 通过往pipe管道写端写入数据来唤醒主线程工作。
这里采用的epoll机制，是一种IO多路复用机制，可以同时监控多个描述符， 当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，本质同步I/O，即读写是阻塞的。 
所以说，主线程大多数时候都是处于休眠状态，并不会消耗大量CPU资源。


