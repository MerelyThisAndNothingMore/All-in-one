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

## 1. 加载

![](https://upload-images.jianshu.io/upload_images/944365-248b92b723ae3aa6.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

1)通过一个类的全限定名来获取定义此类的二进制字节流。
2)将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构。
3)在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口。

加载阶段既可以使用[[Java 虚拟机]]里内置的引导[[Java 类加载器]]来完成，也可以由用户自定义的类加载器去完成，开发人员通过定义自己的类加载器去控制字节流的获取方式(重写一个类加载器的findClass()或loadClass()方法)，实现根据自己的想法来赋予应用程序获取运行代码的动态性。

加载阶段结束后，Java 虚拟机外部的二进制字节流就按照虚拟机所设定的格式存储在[[Java 方法区]]之中 了，方法区中的数据存储格式完全由虚拟机实现自行定义，《Java虚拟机规范》未规定此区域的具体数据结构。类型数据妥善安置在方法区之后，会在[[Java 堆]]内存中实例化一个java.lang.Class类的对象， 这个[[对象]]将作为程序访问方法区中的类型数据的外部接口。



# 步骤2：验证
![](https://upload-images.jianshu.io/upload_images/944365-09f423a9f9e47af7.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤3：准备
![](https://upload-images.jianshu.io/upload_images/944365-f700de8a0d2b4ef0.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤4：解析
![](https://upload-images.jianshu.io/upload_images/944365-ae7602d88369eae8.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)
# 步骤5：初始化
![](https://upload-images.jianshu.io/upload_images/944365-3064969a380dde0b.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)











