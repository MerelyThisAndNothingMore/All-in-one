---
tags: 
alias:
---
在[[Android]]系统中，[[DVM]]、应用程序进程以及运行系统的关键服务的[[system_server进程]]都是由Zygote进程来创建的，我们也将它称为孵化器。
它通过fork  (复制进程)的形式来创建应用程序进程和SystemServer进程，由于Zygote进程在启动时会创建DVM，因此通过fork而创建的应用程序进程和SystemServer进程可以在内部获取一个DVM的实例拷贝。
Zygote进程共做了如下几件事：
1.创建AppRuntime并调用其start方法，启动Zygote进程。  
2.创建DVM并为DVM注册JNI.  
3.通过JNI调用ZygoteInit的main函数进入Zygote的Java框架层。  
4.通过registerZygoteSocket函数创建服务端Socket，并通过runSelectLoop函数等待ActivityManagerService的请求来创建新的应用程序进程。  
5.启动SystemServer进程。