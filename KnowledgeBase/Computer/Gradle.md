---
tags:
  - android
aliases:
---
# 简介
![](https://docs.gradle.org/current/userguide/img/gradle-basic-1.png)
## 配置文件
settings.gradle/settings.gradle.kts文件是一个Gradle项目的入口，前者接受Groovy DSL语言，后者接受Kotlin DSL语言。这里可以设置项目名称，添加子项目。
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

## 插件
插件用于

# 引用
[官方文档](https://docs.gradle.org/current/userguide/getting_started_eng.html)