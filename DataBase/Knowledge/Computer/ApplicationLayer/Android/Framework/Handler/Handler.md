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
# 消息屏障
在next()方法中，有一个屏障的概念(message.target \=\= null为屏障消息), 遇到target为null的Message，说明是 同步屏障，循环遍历找出一条异步消息，然后处理。 在同步屏障没移除前，只会处理异步消息，处理完所有的异步 消息后，就会处于堵塞 当出现屏障的时候，会滤过同步消息，而是直接获取其中的异步消息并返回, 就是这样来实现 「异步消息优先执行」的功能
1、Handler构造方法中传入async参数，设置为true，使用此Handler添加的Message都是异步的; 
2、创建Message对象时，直接调用setAsynchronous(true) 3、removeSyncBarrier() 移除同步屏障:

