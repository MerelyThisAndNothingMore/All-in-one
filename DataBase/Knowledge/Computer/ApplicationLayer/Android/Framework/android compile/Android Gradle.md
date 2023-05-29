---
tags: 
alias:
---
https://blog.csdn.net/zhaoyanjun6/article/details/77678577
# 介绍
[[Android]]项目使用 Gradle 作为构建框架，Gradle 又是以[[Groovy]]为脚本语言。

## 关键概念

### Project

每次构建（build）至少由一个project构成，一个project 由一到多个task构成。

目结构中的每个build.gradle文件代表一个project，在这编译脚本文件中可以定义一系列的task；

### Tasks

task 本质上又是由一组被顺序执行的Action对象构成，Action其实是一段代码块，类似于Java中的方法。

## 生命周期

每次构建的执行本质上执行一系列的Task。某些Task可能依赖其他Task。哪些没有依赖的Task总会被最先执行，而且每个Task只会被执行一遍。每次构建的依赖关系是在构建的配置阶段确定的。

### Initialization: 初始化阶段

这是创建Project阶段，构建工具根据每个build.gradle文件创建出一个Project实例。初始化阶段会执行项目根目录下的settings.gradle文件，来分析哪些项目参与构建。

```groovy
include ':app'
include ':libraries:someProject'
```

### Configuration:配置阶段

这个阶段，通过执行构建脚本来为每个project创建并配置Task。配置阶段会去加载所有参与构建的项目的build.gradle文件，会将每个build.gradle文件实例化为一个Gradle的project对象。然后分析project之间的依赖关系，下载依赖文件，分析project下的task之间的依赖关系。

### Execution:执行阶段

这是Task真正被执行的阶段，Gradle会根据依赖关系决定哪些Task需要被执行，以及执行的先后顺序。
task是Gradle中的最小执行单元，我们所有的构建，编译，打包，debug，test等都是执行了某一个task，一个project可以有多个task，task之间可以互相依赖。



