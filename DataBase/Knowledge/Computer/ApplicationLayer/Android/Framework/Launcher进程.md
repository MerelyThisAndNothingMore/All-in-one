---
tags: 
alias:
---
桌面可以看做是一个程序，即Launcher，当系统开机后，Launcher随之被启动，然后显示应用图标到桌面上。
点击图标相当于点击[[Activity]]中的一个Button，Launcher收到点击事件后会调用startActivity()方法启动[[Activity]]，这里的startActivity()方法来自于Launcher父类，顶端父类是Activity。
