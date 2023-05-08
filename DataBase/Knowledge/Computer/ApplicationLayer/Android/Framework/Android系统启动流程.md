---
tags: 
alias:
---
# 介绍
Android系统架分为应用层，framework层，系统运行库层(Native)，Linux内核层 启动按照一个流程: Loader->kernel->framework->Application来进行的
![](https://gd-hbimg.huaban.com/977e7143e32ad1e0dc4254a3c4b3b288c194c1dd9928-EuMXvg) 
Android系统底层基于[[Linux内核|Linux Kernel]], 当Kernel启动过程会创建[[Init进程]], 该进程是所有用户空间的鼻祖, init进程会启动[[ServiceManager]](binder服务管家), [[zygote进程]](Java进程的鼻祖). Zygote进程会创建[[system_server进程]]以及各种app进程.


**1.启动电源以及系统启动**  
当电源按下时，引导芯片代码 从 ROM (4G)开始执行。Bootloader引导程序把操作系统映像文件拷贝到 RAM中去，然后跳转到它的入口处去执行,启动Linux内核。
**2.引导程序Bootloader**  
引导程序是在Android操作系统开始运行前的一个小程序，它的主要作用是把系统OS拉起来并运行。  
**3.linux内核启动**  
内核启动时，设置缓存、被保护存储器、计划列表，加载驱动。当内核完成系统设置，它首先在系统文件中寻找”init”文件，然后启动root进程或者系统的第一个进程。  
**4.[[Init进程]]启动**  
初始化和启动属性服务，并且启动Zygote进程。  
fork 出 ServerManager 子进程。 ServerManager主要用于管理我们的系统服务，他内部存在一个server 服务列表，这个列表中存储的就是那些已经注册的系统服务。  
解析 init.rc 配置文件并启动 Zygote 进程
**5.[[zygote进程]]启动**  
创建JavaVM并为JavaVM注册JNI，创建服务端Socket，启动SystemServer进程。  
孵化其他应用程序进程，所有的应用的进程都是由zygote进程fork出来的。 通过创建服务端Socket,等待 AMS的请求来创建新的应用程序进程。 
创建SystemServer进程,在Zygote进程启动之后,会通过ZygoteInit的main方法fork出SystemServer进程
**6.[[system_server进程]]启动**  
启动Binder线程池和SystemServiceManager，并且启动各种系统服务。  
创建SystemServiceManager，它用来对系统服务进行创建、启动和生命周期管理。 
ServerManager.startService启动各种系统服务:WMS/PMS/AMS等，调用ServerManager的addService 方法，将这些Service服务注册到ServerManager里面  
启动桌面进程，这样才能让用户见到手机的界面。
**7.[[Launcher进程]]启动**  
被SystemServer进程启动的ActivityManagerService会启动Launcher，Launcher启动后会将已安装应用的快捷图标显示到界面上。
