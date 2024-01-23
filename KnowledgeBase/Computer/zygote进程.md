---
tags: 
aliases:
---

![](https://gd-hbimg.huaban.com/92961f8b40f74ba18f83194343ffba14bb6082d18aae-vyGrL4)
![](https://gd-hbimg.huaban.com/bca443686e1811cd3eaea846258d53994420dc632c047-oL55vf)

在[[Android]]系统中，[[Dalvik]]、应用程序进程以及运行系统的关键服务的[[system_server进程]]都是由Zygote进程来创建的，我们也将它称为孵化器。
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

# Zygote的IPC通信机制为什么使用[[Socket]]而不采用binder
### 先后时序问题
   binder驱动是早于init进程加载的。而init进程是安卓系统启动的第一个进程。 
   安卓中一般使用的**binder引用**，都是**保存在ServiceManager进程中的**，而如果想从ServiceManager中获取到对应的binder引用，前提是需要注册。虽然Init进程是先创建ServiceManager，后创建Zygote进程的。虽然Zygote更晚创建，但是也不能保证Zygote进程去注册binder的时候，ServiceManager已经初始化好了。注册时间点无法保证，AMS无法获取到Zygote的binder引用，
### 多线程问题
Linux中，fork进程其实并不是完美的fork，linux设计之初只考虑到了主线程的fork，也就是说如果主进程中存在子线程，那么fork进程中，其子线程的锁状态，挂起状态等等都是不可恢复的，只有主进程的才可以恢复。

所以，zygote如果使用binder，会导致子进程中binder线程的挂起和锁状态不可恢复，这是原因之二。
### 效率问题
AMS和Zygote之间使用的LocalSocket，相对于网络Socket，减少了数据验证等环节，所以其实效率相对于正常的网络Socket会大幅的提升。虽然还是要经过两次拷贝，但是由于数据量并不大，所以其实影响并不明显。  
所以，LocalSocket效率其实也不低
### 安全问题
LocalSocket其实也有权限校验，并不意味着可以被所有进程随意调用
### Binder拷贝问题



