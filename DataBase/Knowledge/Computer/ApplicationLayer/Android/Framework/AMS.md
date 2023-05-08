---
tags: 
alias:
- ActivityManageService
---
ActivityManagerService 主要负责系统中四大组件的启动、切换、调度及应用进程的管理和调度等工作，其职责 与操作系统中的进程管理和调度模块类似。

ActivityManagerService进行初始化的时机很明确，就是在SystemServer进程开启的时候，就会初始化 ActivityManagerService。([[Android系统启动流程]])

如果打开一个App的话，需要AMS去通知zygote进程， 所有的Activity的生命周期AMS来控制


