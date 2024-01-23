---
tags: 
alias:
---

插件用于**扩展构建功能和自定义 [[Gradle]]**，大多数功能（例如编译 [[Java]] 代码的能力）都是通过 _插件_ 添加的。
插件可以提供有用的任务，例如运行代码、创建文档、设置源文件、发布档案等。
插件分为三类：
1. **核心插件**- Gradle 开发并维护一组[核心插件](https://docs.gradle.org/current/userguide/plugin_reference.html#plugin_reference)。
2. **社区插件**- Gradle 社区通过[Gradle 插件门户](https://plugins.gradle.org/)共享插件。
3. **本地插件- Gradle 使用户能够使用**[API](https://docs.gradle.org/current/javadoc/org/gradle/api/Plugin.html)创建自定义插件。

# 自定义插件

Gradle插件有两种存在形式，二进制和脚本，一般而言，一个插件首先是脚本编写的，当它的功能稳定和泛用后，会被转化为二进制形式，方便共享。

在使用时，插件需要被解析并添加到对应的Project中，这一点通过执行Plugin.apply(T)完成。


