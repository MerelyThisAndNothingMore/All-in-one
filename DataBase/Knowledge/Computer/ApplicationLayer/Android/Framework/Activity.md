---
tags: 
- android
alias:
---

# 启动流程
## 跨进程启动/根Activity启动
- 点击App图标后，[[Launcher进程]]通过[[Binder IPC]]向[[system_server进程]]发起startActivity请求。
- [[system_server进程|system_server进程]] 收到请求后，向[[zygote进程]]进程发起创建进程请求。
- [[zygote进程]]fork出新的子进程，即[[app进程]]
- [[app进程]]通过[[Binder IPC]]向[[system_server进程]]发起attachApplication请求
- [[system_server进程]]收到请求后，进行一系列准备工作，再通过[[Binder IPC]]向[[app进程]]发送scheduleLaunchActivity请求。
- [[app进程]]的[[binder线程]]收到请求后，通过[[Handler]]向主线程发送LAUNCH_ACTIVITY消息
- 主线程收到Message后，通过反射机制创建目标Activity，并回调生命周期方法
## 进程内启动/普通Activity启动





