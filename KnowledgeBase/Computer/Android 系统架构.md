---
tags: 
alias:
- Android Architecture
---
# 五层系统架构
- 应用层
- Java框架层
- 系统Native和Android运行时环境
- HAL
- Linux内核
![](https://gd-hbimg.huaban.com/1f7368a68bdac5245a0c8f7dea8e9f7f271617de18cb2-6PCoob)
# 系统启动架构
![](https://gd-hbimg.huaban.com/15332c539c3d7ad40e7ab6647eaab3a64913a04f3350d-7WRvyT)

## [[Linux]]内核层
Android平台的基础是Linux内核，比如[[ART]]最终调用底层Linux内核来执行功能。
## 硬件抽象层
HAL包含多个库模块，其中每个模块都为特定类型的硬件组件实现一组接口，比如WIFI/蓝牙模块，当框架API请求访问设备硬件时，Android系统将为该硬件加载相应的库模块。
## Android Runtime & 系统库
每个应用都在其自己的进程中运行，都有自己的虚拟机实例。ART通过执行DEX文件可在设备运行多个虚拟机，DEX文件是一种专为Android设计的字节码格式文件，经过优化，使用内存很少。ART主要功能包括：预先(AOT)和即时(JIT)编译，优化的垃圾回收(GC)，以及调试相关的支持。

这里的Native系统库主要包括init孵化来的用户空间的守护进程、HAL层以及开机动画等。启动init进程(pid=1),是Linux系统的用户进程，`init进程是所有用户进程的鼻祖`。

-   init进程会孵化出ueventd、logd、healthd、installd、adbd、lmkd等用户守护进程；
-   init进程还启动`servicemanager`(binder服务管家)、`bootanim`(开机动画)等重要服务
-   init进程孵化出Zygote进程，Zygote进程是Android系统的第一个Java进程(即虚拟机进程)，`Zygote是所有Java进程的父进程`，Zygote进程本身是由init进程孵化而来的。
## Framework层
-   Zygote进程，是由init进程通过解析init.rc文件后fork生成的，Zygote进程主要包含：
    -   加载ZygoteInit类，注册Zygote Socket服务端套接字
    -   加载虚拟机
    -   提前加载类preloadClasses
    -   提前加载资源preloadResouces
-   System Server进程，是由Zygote进程fork而来，`System Server是Zygote孵化的第一个进程`，System Server负责启动和管理整个Java framework，包含ActivityManager，WindowManager，PackageManager，PowerManager等服务。
-   Media Server进程，是由init进程fork而来，负责启动和管理整个C++ framework，包含AudioFlinger，Camera Service等服务。
## App层
-   Zygote进程孵化出的第一个App进程是Launcher，这是用户看到的桌面App；
-   Zygote进程还会创建Browser，Phone，Email等App进程，每个App至少运行在一个进程上。
-   所有的App进程都是由Zygote进程fork生成的。
## Syscall && JNI
-   Native与Kernel之间有一层系统调用(SysCall)层，见[Linux系统调用(Syscall)原理](http://gityuan.com/2016/05/21/syscall/);
-   Java层与Native(C/C++)层之间的纽带JNI，见[Android JNI原理分析](http://gityuan.com/2016/05/28/android-jni/)。
