---
tags: 
alias:
---
[[ServiceManager|ServiceManager]]是[[Init进程]]负责启动的。


# 系统服务与bindService启动的服务的区别
服务可分为系统服务与普通服务，系统服务一般是在系统启动的时候，由SystemServer进程创建并注册到 ServiceManager中 例如AMS，WMS，PMS。而普通服务一般是通过ActivityManagerService启动的服务，或者说通 过四大组件中的Service组件启动的服务。不同主要从以下几个方面:

服务的启动方式 系统服务这些服务本身其实实现了Binder接口，作为Binder实体注册到ServiceManager中，被 ServiceManager管理。这些系统服务是位于SystemServer进程中

普通服务一般是通过Activity的startService或者其他context的startService启动的，这里的Service组件只是个封 装，主要的是里面Binder服务实体类，这个启动过程不是ServcieManager管理的，而是通过 ActivityManagerService进行管理的，同Activity管理类似

服务的注册与管理

系统服务一般都是通过ServiceManager的addService进行注册的，这些服务一般都是需要拥有特定的权限才能 注册到ServiceManager，而bindService启动的服务可以算是注册到ActivityManagerService，只不过 ActivityManagerService管理服务的方式同ServiceManager不一样，而是采用了Activity的管理模型

服务的请求使用方式

使用系统服务一般都是通过ServiceManager的getService得到服务的句柄，这个过程其实就是去 ServiceManager中查询注册系统服务。而bindService启动的服务，主要是去ActivityManagerService中去查找相应 的Service组件，最终会将Service内部Binder的句柄传给Client

# Activity的bindService流程
1、Activity调用bindService:通过Binder通知ActivityManagerService，要启动哪个Service

2、ActivityManagerService创建ServiceRecord，并利用ApplicationThreadProxy回调，通知APP新建并启动 Service启动起来

3、ActivityManagerService把Service启动起来后，继续通过ApplicationThreadProxy，通知APP， bindService，其实就是让Service返回一个Binder对象给ActivityManagerService，以便AMS传递给Client

4、ActivityManagerService把从Service处得到这个Binder对象传给Activity，这里是通过IServiceConnection binder实现。

5、Activity被唤醒后通过Binder Stub的asInterface函数将Binder转换为代理Proxy，完成业务代理的转换，之 后就能利用Proxy进行通信了。

# SystemServer，ServiceManager，SystemServiceManager的关系
在SystemServer进程中创建SystemServiceManager, ServiceManager是系统服务管理 者,SysytemServiceManager启动一些继承自SystemService的服务，并将这些服务的Binder注册到ServiceManager 中，对于其他的一些继承于IBinder的服务,通过ServiceMaanager的addService方法添加

SystemServer:

SystemServer是一个由zygote孵化出来的进程， 名字为system_server 。 SystemServer叫做系统服务进程， 大部分Android提供的一些系统服务都运行在该进程中,包括AMS，WMS，PMS，这些系统的服务都是以一个线程的 方式存在在SysyemServer进程中。

SystemServiceManager: 管理一些系统的服务，在SystemServer中初始化。启动各种系统服务:WMS/PMS/AMS等,调用ServerManager

的addService方，将这些Service服务注册到ServerManager里面 ServiceManager:

ServiceManager像是一个路由，Service把自己注册在ServiceManager中,客户端 通过ServiceManager查询服 务

1、维护一个svclist列表来存储service信息。  
2、向客户端提供Service的代理，也就是BinderProxy。 3、维护一个死循环，不断的查看是否有service的操作请求，如果有就读取相应的内核binder driver。