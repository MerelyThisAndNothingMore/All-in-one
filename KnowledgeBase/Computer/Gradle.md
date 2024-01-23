---
tags:
  - android
aliases:
  - Gradle Build Tool
---
# 简介
![](https://docs.gradle.org/current/userguide/img/gradle-basic-1.png)

Gradle是一种 [[JVM]] 构建系统，通过声明性构建语言完成构建配置。

## 配置文件

settings.gradle/settings.gradle.kts文件是一个Gradle项目的入口，前者接受[[Groovy]] DSL语言，后者接受[[Kotlin]] DSL语言。这里可以设置项目名称，添加子项目。

## 打包脚本

每个 Gradle 构建至少包含一个构建脚本，
```
// 设置插件
plugins {
    id 'application'              
}
// 设置对应插件的属性
application {
    mainClass = 'com.example.Main'  
}
```

## 任务

任务是构建执行的独立单元，每一个任务可以执行编译类、创建JAR、生成JavaDoc等工作。
项目中的任务来自于Gradle插件或构建脚本，通过`./gradlew tasks` 命令，可以列出所有的构建任务；`./gradlew run` 将会执行当前目录下的所有任务。

任务通常在插件中进行定义，如果要在构建脚本中定义，需要如下语法：
```groovy
tasks.register<Zip>("zip-reports") {
    from 'Reports/'
    include '*'
    archiveName 'Reports.zip'
    destinationDir(file('/dir'))
}
```
构建脚本中设置任务属性：
```groovy
tasks.named("javadoc").configure {
    exclude 'app/Internal*.java'
    exclude 'app/internal/*'
    exclude 'app/internal/*'
}
```


## 插件
插件用于**扩展构建功能和自定义 Gradle**，大多数功能（例如编译 Java 代码的能力）都是通过 _插件_ 添加的。
插件可以提供有用的任务，例如运行代码、创建文档、设置源文件、发布档案等。
插件分为三类：
1. **核心插件**- Gradle 开发并维护一组[核心插件](https://docs.gradle.org/current/userguide/plugin_reference.html#plugin_reference)。
2. **社区插件**- Gradle 社区通过[Gradle 插件门户](https://plugins.gradle.org/)共享插件。
3. **本地插件- Gradle 使用户能够使用**[API](https://docs.gradle.org/current/javadoc/org/gradle/api/Plugin.html)创建自定义插件。

## 增量构建和构建缓存

为了使增量构建发挥作用，任务必须定义其输入和输出。Gradle 将确定输入或输出在构建时是否已更改。如果它们发生了变化，Gradle 将执行任务。否则，它将跳过执行。
```
// 新建properties文件
$ touch gradle.properties
// 设置配置参数
org.gradle.console=verbose
// 进行打包，此时会输出所有任务的构建情况。
./gradlew :app:clean :app:build
```

构建缓存用来处理切换分支的情况，它存储以前的构建结果并在需要时恢复它们，避免浪费周期来重新构建不受新代码更改影响的二进制文件。

```
// properties文件中设置参数以开启构建缓存
org.gradle.caching=true
```

## 依赖

通过 `./gradlew :app:dependencies` 查看项目的依赖树。

## 多项目构建

多项目构建由一个根项目和一个或多个子项目组成：
1. 根目录的`settings.gradle.kts`文件应包含所有子项目。
2. 每个子项目都应该有自己的`build.gradle.kts`文件。

## 生命周期

![](https://docs.gradle.org/current/userguide/img/build-lifecycle-example.png)
Gradle构建具有三个阶段：
1. 初始化
	- 检测到`settings.gradle(.kts)`文件。
	- 创建一个[`Settings`](https://docs.gradle.org/current/dsl/org.gradle.api.initialization.Settings.html)实例。
	- 评估设置文件以确定哪些项目（以及包含的构建）构成了构建。
	- 为每个项目创建一个[`Project`](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html)实例。
2. 配置
	- `build.gradle(.kts)`评估参与构建的每个项目的构建脚本。
	- 为请求的任务创建任务图。
3. 执行
	- 安排并执行选定的任务。
	- 任务之间的依赖关系决定了执行顺序。
	- 任务的执行可以并行发生。


# 引用
[官方文档](https://docs.gradle.org/current/userguide/getting_started_eng.html) ——这个是最好的学习资料，广度优先。