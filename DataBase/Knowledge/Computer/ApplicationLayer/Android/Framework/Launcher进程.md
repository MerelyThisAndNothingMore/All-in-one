---
tags: 
alias:
- Launcher
---
[[Android系统启动流程]]的最后一步是启动一个Home应用程序，这个应用程序用来显示系统中已经安装的应用程序，即Launcher。
当系统开机后，Launcher随之被启动，然后显示应用图标到桌面上。
[[system_server进程]]在启动的过程中会启动PackageManagerService，PackageManagerService启动后会将系统中的应用程序安装完成。在此前已经启动的ActivityManagerService会将Launcher启动起来。

点击图标相当于点击[[Activity]]中的一个Button，Launcher收到点击事件后会调用startActivity()方法启动[[Activity]]，这里的startActivity()方法来自于Launcher父类，顶端父类是Activity。
