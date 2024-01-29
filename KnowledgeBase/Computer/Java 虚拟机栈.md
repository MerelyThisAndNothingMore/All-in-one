---
tags:
  - java
aliases:
  - Java Virtual Machine Stack
---
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDQzNjUtOWQ0ZTM1OTcxMjllNDBkYS5wbmc?x-oss-process=image/format,png)

[[Java 虚拟机栈|Java 虚拟机栈]] 是线程私有的，它的生命周期与线程相同。虚拟机栈描述的是Java方法执行的线程内存模型: 每个方法被执行的时候，[[Java 虚拟机]]都会同步创建一个栈帧用于存储局部变量表、操作数栈、动态连接、方法出口等信

息。每一个方法被调用直至执行完毕的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。