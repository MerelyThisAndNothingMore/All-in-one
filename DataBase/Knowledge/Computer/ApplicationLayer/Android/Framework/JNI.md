---
tags: 
alias:
- Java Native Interface
---
## 简介
定义：Java Native Interface，即 Java本地接口
作用： 使得Java 与 本地其他类型语言（如C、C++）交互
即在 Java代码 里调用 C、C++等语言的代码 或 C、C++代码调用 Java 代码

特别注意：
JNI是 Java 调用 Native 语言的一种特性
JNI 是属于 Java 的，与 Android 无直接关系

### 为什么要有 JNI

-   背景：实际使用中，`Java` 需要与 本地代码 进行交互
-   问题：因为 `Java` 具备**跨平台**的特点，所以`Java` 与 本地代码交互的能力非常弱
-   解决方案： 采用 `JNI`特性 增强 `Java` 与 本地代码交互的能力

### 实现步骤
1. 在Java中声明Native方法（即需要调用的本地方法）
2. 编译上述 Java源文件javac（得到 .class文件）
3. 通过 javah 命令导出JNI的头文件（.h文件）
4. 使用 Java需要交互的本地代码（如C++） 实现在 Java中声明的Native方法
   如 Java 需要与 C++ 交互，那么就用C++实现 Java的Native方法
5. 编译.so库文件
6. 通过Java命令执行 Java程序，最终实现Java调用本地代码
