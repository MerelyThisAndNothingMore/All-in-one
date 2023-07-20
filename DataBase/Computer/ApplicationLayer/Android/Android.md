---
tags: 
alias:
---
# 考点介绍
[[Android 复习路径|Android 复习路径]] 
![](https://gd-hbimg.huaban.com/2e64c959a359c60889ab92745205d63e0ce8e48e688e7-PlteSl)
## 四大组件
### [[Activity|Activity]] 
#### 生命周期
[[Activity 生命周期]]
[[Fragment生命周期]]
#### 启动
[[Activity Launch Mode|activity启动模式]]
[[Activity 启动过程]]
#### 卡顿
[[Android 卡顿]]
[[Activity 启动优化]]
#### 状态保存
[[Activity 数据保存]]
### [[BroadcastReceiver]]
### [[ContentProvider]]
[[Intent]]
[[Binder]]
### [[Service]]

## 多线程
[[Handler]]
## [[自定义View]]
## [[performance]]
## [[OpenSourceFramework]]
## 进阶技术
kotlin

# Android Architecture 
 ![android 架构](https://gd-hbimg.huaban.com/0013250d2f5e5785df2cac0155578e63e95a223018c2f-WHEJyK)
#### **应用层**
系统内置的应用程序以及非系统级的应用程序都是属于应用层。负责与用户进行直接交互，通常都是用[[Java]]进行开发的。
#### **应用框架层（Java Framework)**
应用框架层为开发人员提供了可以开发应用程序所需要的API，我们平常开发应用程序都是调用的这一层所提供的API，当然也包括系统的应用。这一层的是由Java代码编写的，可以称为Java Framework。下面来看这一层所提供的主要的组件。
#### **系统运行库层（Native)**
##### **1.C/C++程序库**
C/C++程序库能被Android系统中的不同组件所使用，并通过应用程序框架为开发者提供服务
##### **2.Android运行时库**
运行时库又分为核心库和ART(5.0系统之后，Dalvik虚拟机被ART取代)。核心库提供了Java语言核心库的大多数功能，这样开发者可以使用Java语言来编写Android应用。相较于JVM，Dalvik虚拟机是专门为移动设备定制的，允许在有限的内存中同时运行多个虚拟机的实例，并且每一个Dalvik 应用作为一个独立的Linux 进程执行。独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。而替代Dalvik虚拟机的ART 的机制与Dalvik 不同。在Dalvik下，应用每次运行的时候，字节码都需要通过即时编译器转换为机器码，这会拖慢应用的运行效率，而在ART 环境中，应用在第一次安装的时候，字节码就会预先编译成机器码，使其成为真正的本地应用。
#### 硬件抽象层（HAL)
硬件抽象层是位于操作系统内核与硬件电路之间的接口层，其目的在于将硬件抽象化，为了保护硬件厂商的知识产权，它隐藏了特定平台的硬件接口细节，为操作系统提供虚拟硬件平台，使其具有硬件无关性，可在多种平台上进行移植。 从软硬件测试的角度来看，软硬件的测试工作都可分别基于硬件抽象层来完成，使得软硬件测试工作的并行进行成为可能。通俗来讲，就是将控制硬件的动作放在硬件抽象层中。
#### [[Linux内核|Linux内核]] 
Android 的核心系统服务基于Linux 内核，在此基础上添加了部分Android专用的驱动。系统的安全性、内存管理、进程管理、网络协议栈和驱动模型等都依赖于该内核。