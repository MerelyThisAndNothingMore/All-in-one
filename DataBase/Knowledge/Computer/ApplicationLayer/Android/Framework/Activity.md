---
tags: 
- android
alias:
---
# 启动流程
[[Activity|Activity]]启动可以分为根Activity启动和普通Activity启动，即是通过桌面图标启动还是由通过另一个Activity启动。
## 跨进程启动/根Activity启动
- [[Launcher进程]]：点击App图标后，[[Launcher进程]]通过[[Binder|Binder IPC]]向[[system_server进程]]发起startActivity请求。
	- Activity.startActivity
	- Activity.startActivityForResult
	- Instrumentation.execStartActivity
	- 获取IActivityTaskManager.aidl，这里定义了[[IPC]]使用到的[[Binder]]
	- ![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/9/16daf8c05d64c40a~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp)
- [[system_server进程|system_server进程]] 收到请求后，向[[zygote进程]]进程发起创建进程请求。
	- 通过ActivityTaskManagerService实现IActivityTaskManager.Stub
	- ActivityTaskManagerService内部
- [[zygote进程]]fork出新的子进程，即[[app进程]]
- [[app进程]]通过[[Binder|Binder IPC]]向[[system_server进程]]发起attachApplication请求
- [[system_server进程]]收到请求后，进行一系列准备工作，再通过[[Binder|Binder IPC]]向[[app进程]]发送scheduleLaunchActivity请求。
- [[app进程]]的[[binder线程]]收到请求后，通过[[Handler]]向主线程发送LAUNCH_ACTIVITY消息
- 主线程收到Message后，通过反射机制创建目标Activity，并回调生命周期方法
## 进程内启动/普通Activity启动





