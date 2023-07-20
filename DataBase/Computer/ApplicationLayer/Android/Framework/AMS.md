---
tags: 
alias:
- ActivityManageService
---
http://liuwangshu.cn/framework/ams/1-ams.html

# 定义
AMS是系统的引导服务，应用进程的启动、切换和调度、四大组件的启动和管理都需要AMS的支持。

# 启动流程




ActivityManagerService进行初始化的时机很明确，就是在SystemServer进程开启的时候，就会初始化 ActivityManagerService。([[Android系统启动流程]])

AMS运行在SystemServer进程中。SystemServer进程启动时，会通过 SystemServer.startBootstrapServices()来创建一个AMS的对象;

AMS通过ActivityStackSupervisor来管理Activity。AMS对象只会存在一个，在初始化的时候，会创建一个唯 一的ActivityStackSupervisor对象;

ActivityStackSupervisor中维护了显示设备的信息。当有新的显示设备添加时，会创建一个新的 ActivityDisplay对象;

ActivityStack与显示设备的绑定。ActivityStack的创建时在Launcher启动时候进行的， AMS还未有非 Launcher的ActivityStack。后面的App启动时就会创建Launcher的ActivityStack，

通过ActivityStackSupervisor来创建ActivityRecord 在ActivityStack上创建TaskRecord 每一个ActivityRecord都需要找到自己的宿主TaskRecord

如果打开一个App的话，需要AMS去通知zygote进程， 所有的Activity的生命周期AMS来控制


