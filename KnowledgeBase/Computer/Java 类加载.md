---
tags: 
alias:
---

[[Java 虚拟机]]把描述类的数据从[[Class文件]]加载到[[内存]]，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型，这个过程被称作虚拟机的类加载机制。

与那些在编译时需要进行连接的语言不同，在[[Java]]语言里面，类型的加载、连接和初始化过程都是在程序运行期间完成的，这种策略让Java语言进行提前编译会面临额外的困难，也会让类加载时稍微增加一些性能开销， 但是却为Java应用提供了极高的扩展性和灵活性，**Java天生可以动态扩展的语言特性就是依赖运行期动态加载和动态连接这个特点实现的。**

# 类加载生命周期

![[Pasted image 20240201115840.png]]

一个类型从被加载到虚拟机内存中开始,到卸载出内存为止,它的整个生命周期将会经历加载(Loading)、验证(Verification)、准备(Preparation)、解析(Resolution)、初始化(Initialization)、使用(Using)和卸载(Unloading)七个阶段,其中验证、准备、解析三个部分统称为连接(Linking)。



![](https://upload-images.jianshu.io/upload_images/944365-31072687a32f8861.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
加载 -> 验证 -> 准备 -> 解析 -> 初始化
![](https://upload-images.jianshu.io/upload_images/944365-6f1d148aa90ea914.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

# 步骤1：加载
![](https://upload-images.jianshu.io/upload_images/944365-248b92b723ae3aa6.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

# 步骤2：验证
![](https://upload-images.jianshu.io/upload_images/944365-09f423a9f9e47af7.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤3：准备
![](https://upload-images.jianshu.io/upload_images/944365-f700de8a0d2b4ef0.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤4：解析
![](https://upload-images.jianshu.io/upload_images/944365-ae7602d88369eae8.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤5：初始化
![](https://upload-images.jianshu.io/upload_images/944365-3064969a380dde0b.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)











