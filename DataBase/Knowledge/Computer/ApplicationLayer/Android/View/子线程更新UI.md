---
tags: 
alias:
---
刷新UI， 都会调用到ViewRootImpl.Android每次刷新UI的时候，最终根布局ViewRootImpl.checkThread()来检 验线程是否是View的创建线程。 
ViewRootImpl创建的第一个地方，从Acitivity声明周期handleResumeActivity会被优先调用到，也就是说在OnResume后ViewRootImpl就被创建，这个时候无法在在子线程中访问UI了，上面子线程 延迟了一会，handleResumeActivity已经被调用了，所以发生了崩溃 不延迟在creae里直接设置不会崩溃 线程更新 UI也行，但是只能更新自己创建的View


# 为什么Android系统不建议子线程访问UI
在android中子线程可以有好多个，但是如果每个线程都可以对ui进行访问，我们的界面可能就会变得混乱不 堪，这样多个线程操作同一资源就会造成线程安全问题，当然，需要解决线程安全问题的时候，我们第一想到的可能 就是加锁，但是加锁会降低运行效率，所以android出于性能的考虑，并没有使用加锁来进行ui操作的控制。


# Android中为什么主线程不会因为Looper.loop()里的死循环卡死?
他不阻塞的原因是epoll机制，他是linux里面的，在native层会有一个读取端和一个写入端，当有消息发送过来的时候会去唤醒读取端，然后进行消息发送与处理，没消息的时候是处于休眠状态，所以他不会阻塞他。






