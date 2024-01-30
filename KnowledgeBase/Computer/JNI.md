---
tags: 
alias:
- Java Native Interface
- Java本地接口
---
https://juejin.cn/post/6844904192780271630#heading-1
# 简介
JNI 允许[[Java 虚拟机]]内运行的[[Java]]代码与[[C]]、[[C++]]和汇编等其他编程语言编写的应用程序和库进行互操作。

特别注意：
JNI是 Java 调用 Native 语言的一种特性
JNI 是属于 Java 的，与 Android 无直接关系

## 为什么要有 JNI

-   背景：实际使用中，`Java` 需要与 本地代码 进行交互
-   问题：因为 `Java` 具备**跨平台**的特点，所以`Java` 与 本地代码交互的能力非常弱
-   解决方案： 采用 `JNI`特性 增强 `Java` 与 本地代码交互的能力

## 实现步骤
1. 在Java中声明Native方法（即需要调用的本地方法）
2. 编译上述 Java源文件javac（得到 .class文件）
3. 通过 javah 命令导出JNI的头文件（.h文件）
4. 使用 Java需要交互的本地代码（如C++） 实现在 Java中声明的Native方法
   如 Java 需要与 C++ 交互，那么就用C++实现 Java的Native方法
5. 编译.so库文件
6. 通过Java命令执行 Java程序，最终实现Java调用本地代码
## 原理
Java语言的执行环境是Java虚拟机（JVM），JVM其实是主机环境中的一个进程，每个JVM都在本地环境中有一个JavaVM结构体，该结构体在创建虚拟机的时候被返回。在JNI环境下创建JVM的函数为JNI_CreateJavaVM。

JINEnv是当前Java线程的执行环境，一个JVM对应一个JavaVM结构体，一个JVM中可能创建多个Java线程，每个线程对应一个JNIEnv结构，它们保存在线程本地存储TLS中。因此不同的线程JNIEnv不同，而不能相互共享使用。

JavaEnv结构也是一个函数表，在本地代码通过JNIEnv函数表来操作Java数据或者调用Java方法。也就是说，只要在本地代码中拿到了JNIEnv结构，就可以在本地代码中调用Java代码。


# JNI开发
