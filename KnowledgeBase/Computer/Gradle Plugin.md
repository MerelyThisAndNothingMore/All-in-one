---
tags: 
alias:
---

# 简介

插件用于**扩展构建功能和自定义 [[Gradle]]**，大多数功能（例如编译 [[Java]] 代码的能力）都是通过 _插件_ 添加的。
插件可以提供有用的任务，例如运行代码、创建文档、设置源文件、发布档案等。
插件分为三类：

1. **核心插件**- Gradle 开发并维护一组[核心插件](https://docs.gradle.org/current/userguide/plugin_reference.html#plugin_reference)。
2. **社区插件**- Gradle 社区通过[Gradle 插件门户](https://plugins.gradle.org/)共享插件。
3. **本地插件- Gradle 使用户能够使用**[API](https://docs.gradle.org/current/javadoc/org/gradle/api/Plugin.html)创建自定义插件。

# 插件引用

Gradle插件有两种存在形式，二进制和脚本，一般而言，一个插件首先是脚本编写的，当它的功能稳定和泛用后，会被转化为二进制形式，方便共享。

在使用时，插件需要被解析并添加到对应的Project中，这一点通过执行对应的DSL来进行声明。

## plugins闭包

```groovy
// 主Project引入
plugins {
    id 'com.example.hello' version '1.0.0' apply false
    id 'com.example.goodbye' version '1.0.0' apply false
}

// 子Project使用
plugins {
    id 'my-plugin'
}
```

plugins闭包必须放在构建脚本的最上方。

## 从BuildSrc引用

```groovy
// 主Project引入
gradlePlugin {
    plugins {
        myPlugins {
            id = 'my-plugin'
            implementationClass = 'my.MyPlugin'
        }
    }
}
// 子Project使用
plugins {
    id 'my-plugin'
}
```

BuildSrc用来存放本地插件

## buildscript{}

如果要在build.gradle文件中使用到插件能力，需要在buildscript闭包下进行声明。

```groovy
import org.apache.commons.codec.binary.Base64

buildscript {
    repositories {  // this is where the plugins are located
        mavenCentral()
        google()
    }
    dependencies { // these are the plugins that can be used in subprojects or in the build file itself
        classpath group: 'commons-codec', name: 'commons-codec', version: '1.2' // used in the task below
        classpath 'com.android.tools.build:gradle:4.1.0' // used in subproject
    }
}

tasks.register('encode') {
    doLast {
        def byte[] encodedString = new Base64().encode('hello world\n'.getBytes())
        println new String(encodedString)
    }
}
```

# 自定义插件

通过自定义插件可以更好地组织构建逻辑，任何实现了Plugin接口的类都可以称为是插件。

```kotlin
import org.gradle.api.Plugin
import org.gradle.api.Project

abstract class SamplePlugin : Plugin<Project> {
    override fun apply(project: Project) {
        project.tasks.create("SampleTask") {
            println("Hello world!")
        }
    }
}
```


https://docs.gradle.org/current/userguide/writing_plugins.html
## 约定插件

## 二进制插件