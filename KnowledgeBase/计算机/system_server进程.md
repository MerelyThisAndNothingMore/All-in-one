---
tags: 
alias:
---
`system_server` 进程托管了许多核心系统服务，例如 Activity Manager Service (AMS)、Window Manager Service (WMS)、Package Manager Service (PMS)、Power Manager、Notification Manager 等。



SyetemServer在启动时做了如下工作：  
1. 启动Binder线程池，这样就可以与其他进程进行通信。  
2. 创建SystemServiceManager用于对系统的服务进行创建、启动和生命周期管理。  
3. 启动各种系统服务。
   ServiceManager用来管理系统中的各种Service，用于系统C/S架构中的Binder机制通信：Client端要使用某个Service，则需要先到ServiceManager查询Service的相关信息，然后根据Service的相关信息与Service所在的Server进程建立通讯通路，这样Client端就可以使用Service了。
