---
tags: 
alias:
---
1.原理

JIT是运行时编译，这样可以对执行次数频繁的dex代码进行编译和优化，减少以后使用时的翻译时间， 但将dex 翻译为本地机器码也要占用时间，所以Google在4.4之后推出了ART，用来替换Dalvik。

ART的策略与Dalvik不同，在ART 环境中，应用在第一次安装的时候，字节码就会预先编译成机器码，使其成为 真正的本地应用。之后打开App的时候，不需要额外的翻译工作，直接使用本地机器码运行，因此运行速度提高。

AOT  
AOT 是静态编译，应用在安装的时候会启动 dex2oat 过程把 dex预编译成 ELF 文件，每次运行程序的时候不用

重新编译。





Dalvik，ART是Android的两种运行环境，也可以叫做Android虚拟机 JIT，AOT是Android虚拟机采用的两种不同 的编译策略

在Dalvik虚拟机上，APK中的Dex文件在安装时会被优化成odex文件，在运行时，会被JIT编译器编译成native代 码。

在ART虚拟机上安装时，Dex文件会直接由dex2oat工具翻译成oat格式的文件，oat文件中既包含了dex文件中原 先的内容，也包含了已经编译好的native代码。




