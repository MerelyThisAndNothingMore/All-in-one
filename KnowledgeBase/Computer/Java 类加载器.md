---
tags: 
alias:
- ClassLoader
---

通过一个类的全限定名来获取描述该类的二进制字节流

> 比较两个类是否“相等”，只有在这两个类是由同一个类加载器加载的前提下才有意义，否则，即使这两个类来源于同一个[[Class文件]]，被同一个[[Java 虚拟机]]加载，只要加载它们的类加载器不同，那这两个类就必定不相等。

## 启动类加载器

启动类加载器(Bootstrap Class Loader):这个类加载器负责加载存放在 <JAVA_HOME>\\lib目录，或者被-Xbootclasspath参数所指定的路径中存放的，而且是Java虚拟机能够 识别的(按照文件名识别，如rt .jar、t ools.jar，名字不符合的类库即使放在lib目录中也不会被加载)类库加载到虚拟机的内存中。启动类加载器无法被Java程序直接引用，用户在编写自定义类加载器时，如果需要把加载请求委派给引导类加载器去处理，那直接使用null代替即可

## 扩展类加载器

扩展类加载器(Extension Class Loader):这个类加载器是在类sun.misc.Launcher$ExtClassLoader 中以Java代码的形式实现的。它负责加载<JAVA_HOM E>\\lib\\ext目录中，或者被java.ext.dirs系统变量所 指定的路径中所有的类库。根据“扩展类加载器”这个名称，就可以推断出这是一种Java系统类库的扩 展机制，JDK的开发团队允许用户将具有通用性的类库放置在ext目录里以扩展Java SE的功能，在JDK 9之后，这种扩展机制被模块化带来的天然的扩展能力所取代。由于扩展类加载器是由Java代码实现 的，开发者可以直接在程序中使用扩展类加载器来加载Class文件。

## 应用程序类加载器

应用程序类加载器(Application Class Loader):这个类加载器由  sun.misc.Launcher$Ap p ClassLoader来实现。由于应用程序类加载器是ClassLoader类中的getSy stem- ClassLoader()方法的返回值，所以有些场合中也称它为“系统类加载器”。它负责加载用户类路径 (ClassPath)上所有的类库，开发者同样可以直接在代码中使用这个类加载器。如果应用程序中没有 自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。
