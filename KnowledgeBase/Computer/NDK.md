---
tags: 
alias:
- Native Development Kit
---
## 简介
### 定义
> 在Android开发中，Google提供了两种开发包，[[Android SDK]]和NDK，其中SDK是用来[[Java]]开发的，而NDK是用来c/c++开发的，即[[JNI]]编程方式，第三方应用可以通过JNI调用自己的c动态库，这就是NDK。

NDK是 [[Android]]的一个工具开发包
NDK是属于 Android 的，与[[Java]]并无直接关系

### 作用
快速开发C、 C++的动态库，并自动将so和应用一起打包成 APK
即可通过 NDK在 Android中 使用 JNI与本地代码（如C、C++）交互

应用场景：在Android的场景下 使用JNI
即 Android开发的功能需要本地代码（C/C++）实现


# NDK与JNI关系
![](https://gd-hbimg.huaban.com/a84780b6cf0a766cd34b8b93d05eac56c23c452417da1-AfQ42f)


