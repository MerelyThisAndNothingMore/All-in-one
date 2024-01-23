---
tags:
  - java
aliases:
---

# 简介

![](https://docs.gradle.org/current/userguide/img/author-gradle-5.png)

任务是构建执行的独立单元，每一个任务可以执行编译类、创建JAR、生成JavaDoc等工作。
项目中的任务来自于Gradle插件或构建脚本，通过`./gradlew tasks` 命令，可以列出所有的构建任务；`./gradlew run` 将会执行当前目录下的所有任务。

任务通常在插件中进行定义，如果要在构建脚本中定义，需要如下语法：

```groovy
tasks.register('hello') {
    doLast {
        println 'Hello world!'
    }
}
```

tasks.register()方法会生成一个继承DefaultTask类的子类，然后将这个子类注册到任务列表中。通过`./gradlew help --task hello` 命令可以查看到类名。

# 自定义

构建脚本中设置任务属性：

```groovy
4.times { counter ->
    tasks.register("task$counter") {
        doLast {
            println "I'm task number $counter"
        }
    }
}
tasks.named('task0') { dependsOn('task2', 'task3') }
```

Gradle中存在一些内置的Task([DSL](link:../dsl/org.gradle.api.plugins.antlr.AntlrTask.html))，方便开发者使用，如移动文件，语法如下：

```groovy
tasks.register("copyTask",Copy) {
    from("source")
    into("target")
    include("*.war")
}
```

通过声明Copy类获取了Copy类的相关方法：from()、intro、include()，本质上是在[[Java]]中声明了一个CopyTaskTask类来继承了内置的CopyTask类。



