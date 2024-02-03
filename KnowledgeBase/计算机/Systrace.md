---
tags: 
alias:
---
com.wejoy.weplay.jp
Systrace工具是[[Android]]平台提供的一种强大工具，用于收集系统和应用程序的许多关键性能信息。

systrace 可以从系统层面上，收集并分析设备运行时的所有进程的时间信息，它从 Android 内核中，比如：CPU 调度、磁盘活动和 app 线程中收集信息
# 使用
```shell
adb shell systrace -a com.wejoy.weplay.jp -o trace.html sched gfx view
```
python $ANDROID_HOME/platform-tools/systrace/systrace.py --app=com.wejoy.weplay.jp -o trace.html sched gfx view