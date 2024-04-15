---
tags: 
alias:
---
https://blog.csdn.net/luo_boke/article/details/106004778
# 介绍
ART 是 [[Java 虚拟机|JVM]]的一种，在4.4(L)之后推出，用来替换[[Dalvik]]。
## 原理
JIT是运行时编译，这样可以对执行次数频繁的dex代码进行编译和优化，减少以后使用时的翻译时间， 但将dex 翻译为本地机器码也要占用时间，所以Google在4.4(L)之后推出了ART，用来替换Dalvik。

ART的策略与Dalvik不同，在ART 环境中，应用在第一次安装的时候，字节码就会预先编译（Ahead-Of-Time，AOT）成机器码，使其成为 真正的本地应用。之后打开App的时候，不需要额外的翻译工作，直接使用本地机器码运行，因此运行速度提高。

AOT  
AOT 是静态编译，应用在安装的时候会启动 dex2oat 过程把 dex预编译成 ELF 文件，每次运行程序的时候不用重新编译。




# ART 和 [[Dalvik]]
Dalvik，ART是Android的两种运行环境，也可以叫做Android虚拟机 

JIT，AOT是Android虚拟机采用的两种不同 的编译策略
## JIT
在Dalvik虚拟机上，APK中的Dex文件在安装时会被优化成odex文件，在运行时，会被JIT编译器编译成native代 码。
## AOT
在ART虚拟机上安装时，Dex文件会直接由dex2oat工具翻译成oat格式的文件，oat文件中既包含了dex文件中原先的内容，也包含了已经编译好的native代码。
7.0后系统采用的是结合使用 AOT、即时 (JIT) 编译和配置文件引导型编译，一种混合模式。
**优点**：
1. 占用较少的CPU资源，消耗更少的电量 ；
2. 减少启动时间；
3. 不用即时翻译，可省电 ；
4. 改善垃圾回收器；
5. 应用运行更快，直接执行机器码，.dex文件已经在安装时翻译好了  
**缺点**：
1. 安装时需要AOT,安装时间更久; 
2. 需要额外的空间存储编译的机器码



