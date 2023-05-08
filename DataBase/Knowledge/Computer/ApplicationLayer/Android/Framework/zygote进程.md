---
tags: 
alias:
---

![](https://gd-hbimg.huaban.com/92961f8b40f74ba18f83194343ffba14bb6082d18aae-vyGrL4)

在[[DataBase/Knowledge/Computer/ApplicationLayer/Android/Android]]系统中，[[DVM]]、应用程序进程以及运行系统的关键服务的[[system_server进程]]都是由Zygote进程来创建的，我们也将它称为孵化器。
它通过fork  (复制进程)的形式来创建应用程序进程和SystemServer进程，由于Zygote进程在启动时会创建DVM，因此通过fork而创建的应用程序进程和SystemServer进程可以在内部获取一个DVM的实例拷贝。



Zygote进程共做了如下几件事：
1.创建AppRuntime并调用其start方法，启动Zygote进程。  
2.创建DVM并为DVM注册JNI.  
3.通过JNI调用ZygoteInit的main函数进入Zygote的Java框架层。  
4.通过registerZygoteSocket函数创建服务端Socket，并通过runSelectLoop函数等待ActivityManagerService的请求来创建新的应用程序进程。  
5.启动SystemServer进程。

# 孵化应用进程这种事为什么不交给SystemServer来做，而专门设计一 个Zygote
Zygote进程是所有Android进程的母体，包括system_server和各个App进程。zygote利用fork()方法生成 新进程，对于新进程A复用Zygote进程本身的资源，再加上新进程A相关的资源，构成新的应用进程A。应 用在启动的时候需要做很多准备工作，包括启动虚拟机，加载各类系统资源等等，这些都是非常耗时的， 如果能在zygote里就给这些必要的初始化工作做好，子进程在fork的时候就能直接共享，那么这样的话效 率就会非常高

SystemServer里跑了一堆系统服务，这些不能继承到应用进程

# Zygote的IPC通信机制为什么使用socket而不采用binder
Zygote是通过fork生成进程的 
因为fork只能拷贝当前线程，不支持多线程的fork，fork的原理是copy-on-write机制，当父子进程任一方 修改内存数据时(这是on-write时机)，才发生缺页中断，从而分配新的物理内存(这是copy操作)。 
zygote进程中已经启动了虚拟机、进行资源和类的预加载以及各种初始化操作，App进程用时拷贝即可。 Zygotefork出来的进程A只有一个线程，如果Zygote有多个线程，那么A会丢失其他线程。这时可能造成死 锁。 
Binder通信需要使用Binder线程池,binder维护了一个16个线程的线程池，fork()出的App进程的binder通 讯没法用


