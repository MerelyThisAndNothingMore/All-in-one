---
tags: 
alias:
---
# 介绍

**[[Binder]]/Socket用于进程间通信，而Handler消息机制用于同进程的线程间通信**
Handler消息机制是由一组MessageQueue、Message、Looper、Handler共同组成的，为了方便且称之为Handler消息机制。
Handler消息机制用于同进程的线程间通信，其核心是线程间共享内存空间，而不同进程拥有不同的地址空间，也就不能用handler来实现进程间通信。
Handler默认是与当前线程的Looper绑定的，因此在子线程中new Handler时需要先调用Looper.prepare()和Looper.loop()方法来创建和启动该线程的消息循环系统，否则会抛出异常。
一个线程可以有多个Handler,只有一个Looper对象,只有一个MessageQueue对象。

## post和handleMessage
post方法的消息是在post传递的Runnable对象的 run方法中处理，而调用sendMessage方法需要重写handleMessage方法或者给handler设置callback，在callback 的handleMessage中处理并返回true
- msg.callback
- handler.mCallback
- handleMessage
