---
tags: 
alias:
---
# Intro 
`Context`在[[Android]]开发中具有重要的作用，它提供了访问应用程序资源、启动组件、访问系统服务以及处理资源生命周期的能力。开发者可以使用`Context`来实现各种应用程序功能和与系统环境的交互。
## context家族
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/517ecdbab4a84f069872046835021374~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)
### ContextImpl
`Context`本身是一个抽象类，主要的实现类就是`ContextImpl`，`ContextImpl`实际承担着提供应用程序资源访问、组件启动和系统服务等功能的责任。
ContextImpl里比较重要的是关于resource和Theme的实现，通过这块儿代码来获取系统资源和主题设置。
### ContextWrapper
`ContextWrapper`是一个包装类，内部包含一个`mBase： Context`成员变量，所有的实现都是调用`mBase`的方法。
### ContextThemeWrapper
相比于ContextWrapper，ContextThemeWrapper中修改了Theme获取的相关逻辑。



