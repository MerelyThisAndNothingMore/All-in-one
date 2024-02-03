---
tags: 
alias:
---
# 介绍

**[[Binder]]/Socket用于进程间通信，而Handler消息机制用于同进程的线程间通信**
Handler消息机制是由一组MessageQueue、Message、Looper、Handler共同组成的，为了方便且称之为Handler消息机制。
Handler:负责消息的发送和处理 
Message:消息 对象，类似于链表的一个结点; 
MessageQueue:消息队列，用于存放消息对象的数据结构; Looper:消息队列的处理者 (用于轮询消息队列的消息对象)
Handler发送消息时调用MessageQueue的enqueueMessage插入一条信息到 MessageQueue,Looper不断轮询调用MeaasgaQueue的next方法 如果发现message就调用handler的 dispatchMessage，dispatchMessage被成功调用，接着调用handlerMessage()。
Handler消息机制用于同进程的线程间通信，其核心是线程间共享内存空间，而不同进程拥有不同的地址空间，也就不能用handler来实现进程间通信。
Handler默认是与当前线程的Looper绑定的，因此在子线程中new Handler时需要先调用Looper.prepare()和Looper.loop()方法来创建和启动该线程的消息循环系统，否则会抛出异常。
一个线程可以有多个Handler,只有一个Looper对象,只有一个MessageQueue对象。
因为Handler 的构造方法中，会通过 Looper.myLooper()获取looper对象，如果为空，则抛出异常， 主线程则因为已在入口处ActivityThread的main方 法中通过 Looper.prepareMainLooper()获取到这个对象， 并通过 Looper.loop()开启循环，在子线程中若要使用 handler，可先通过Loop.prepare获取到looper对象，并使用Looper.loop()开启循环
# [[Message|Message]]发送方式
## sendMessage
sendMessage(Message msg) 
sendMessageDelayed(Message msg, long uptimeMillis)
sendMessageAtTime(Message msg,long when)
## post
post(Runnable r)
postDelayed(Runnable r, long uptimeMillis)
## 比较
post方法的消息是在post传递的Runnable对象的 run方法中处理，而调用sendMessage方法需要重写handleMessage方法或者给handler设置callback，在callback 的handleMessage中处理并返回true
- msg.callback
- handler.mCallback
- handleMessage
[[消息屏障]]
# 使用
当在A线程中创建handler的时候，同时创建了MessageQueue与Looper，Looper在A线程中调用loop进入一个 无限的for循环从MessageQueue中取消息，当B线程调用handler发送一个message的时候，会通过 msg.target.dispatchMessage(msg);将message插入到handler对应的MessageQueue中，Looper发现有message 插入到MessageQueue中，便取出message执行相应的逻辑，因为Looper.loop()是在A线程中启动的，所以则回到 了A线程，达到了从B线程切换到A线程的目的。

# Handler发送消息的delay可靠吗？
不可靠
## 主线程压力过大(待处理消息过多) 
若发送的消息过多，主线程处理较慢，导致堆积很多待处理消息，会导致主线程卡顿

handler.postDelayed(run, delay)的消息，调用时间非delay 

MessageQueue如何处理消息： 
Working Thread(工作线程)
-->enquequeMessage(MessageQueue，入队一条消息)
-->wake(Native层：NativeMessageQueue，唤醒)
-->write(mWakeEventFd，写入消息)
--↓(唤醒mEpollFd) Looper
-->next(MessageQueue，处理下一条消息)
-->pollOnce(Native层：NativeMessageQueue，轮询一条)
-->epoll_wait(mEpollFd，若此时消息队列中无消息，则在此等待，唤醒后返回一条消息)

# [[内存泄漏|内存泄漏]]分析
https://www.cnblogs.com/jimuzz/p/14187408.html

## 发送延迟消息
`Handler`导致内存泄漏一般发生在发送延迟消息的时候，当`Activity`关闭之后，延迟消息还没发出，那么主线程中的`MessageQueue`就会持有这个消息的引用，而这个消息是持有`Handler`的引用，而`handler`作为匿名内部类持有了`Activity`的引用，所以就有了以下的一条引用链:

主线程 
—> threadlocal 
—> [[Looper]] 
—> [[Message Queue]] 
—> [[Message]] 
—> [[Handler]] 
—> [[Android Activity|Android Activity]] 

  所以这次引用的`头头`就是`主线程`，主线程肯定是不会被回收的，只要是`运行中的线程`都不会被[[Java 虚拟机|JVM]]回收，跟`静态变量`一样被JVM特殊照顾。
## 子线程运行没结束


