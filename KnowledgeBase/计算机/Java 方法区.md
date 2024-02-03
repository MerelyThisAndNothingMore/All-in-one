---
tags: 
aliases:
  - Java Method Area
---
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDQzNjUtZTllMDU4MWM0NTFkMzA2ZC5wbmc?x-oss-process=image/format,png)


方法区(Method Area)与[[Java 堆]]一样，是各个[[线程]]共享的[[内存]]区域，它用于存储已被虚拟机加载的类型信息、常量、静态变量、即时编译器编译后的代码缓存等数据。

从《Java虚拟机规范》所定义的概念模型来看，所有Class相关的信息都应该存放在方法区之中， 但方法区该如何实现，《Java虚拟机规范》并未做出规定，这就成了一件允许不同虚拟机自己灵活把 握的事情。JDK 7及其以后版本的[[HotSpot]]虚拟机选择把静态变量与类型在Java语言一端的映射Class对象存放在一起，存储于Java堆之中

## 运行时常量池

运行时常量池(Runtime Constant Pool)是方法区的一部分。[[class文件]]中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池表(Constant Pool Table)，用于存放编译期生成的各种字面量与符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。


