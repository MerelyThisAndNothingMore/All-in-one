---
tags: 
alias:
---
地址： https://github.com/bytedance/Fastbot_Android/tree/main
操作手册： https://github.com/bytedance/Fastbot_Android/blob/main/handbook-cn.md

运行命令模板
```shell
adb -s 设备号 shell CLASSPATH=/sdcard/monkeyq.jar:/sdcard/framework.jar:/sdcard/fastbot-thirdpart.jar exec app_process /system/bin com.android.commands.monkey.Monkey -p 包名 --agent reuseq --running-minutes 遍历时长 --throttle 事件频率 -v -v
```

实际使用命令
```shell
cd ~/Desktop/Project/Fastbot_Android

adb push *.jar /sdcard
adb push libs/* /data/local/tmp/


adb -s 5PD6SCZHCQEEPREQ shell CLASSPATH=/sdcard/monkeyq.jar:/sdcard/framework.jar:/sdcard/fastbot-thirdpart.jar exec app_process /system/bin com.android.commands.monkey.Monkey -p com.wejoy.weplay.jp --agent reuseq --running-minutes 900 --throttle 500 -v -v



```
# 结果
Crash:/sdcard/crash-dump.log
ANR:/sdcard/oom-traces.log
