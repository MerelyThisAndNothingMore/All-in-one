---
tags: 
alias:
---
Init进程是Android系统中用户空间的第一个进程(pid=1)，作为第一个进程，它被赋予了很多极其重要的工作职责，比如创建zygote(孵化器)和属性服务等。init进程是由多个源文件共同组成的，这些文件位于源码目录system/core/init。
在[[Android系统启动流程]]中，[[Linux]]内核启动后，就要启动init进程。
[[Init进程|Init进程]]启动后，主要做了三件事：
1.创建一些文件夹并挂载设备  
2.初始化和启动属性服务  
3.解析init.rc配置文件并启动zygote进程


