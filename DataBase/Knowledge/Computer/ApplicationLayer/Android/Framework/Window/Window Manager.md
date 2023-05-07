---
tags: 
alias:
---

WindowManager是一个接口，继承自只有添加、删除、更新三个方法的ViewManager接口。 它的实现类为 WindowManagerImpl，WindowManagerImpl通过WindowManagerGlobal代理实现addView， 最后调用到 ViewRootImpl的setView 使ViewRoot和Decorview相关联。如果要对Window进行添加和删除就需要使用 WindowManager， 具体的工作则由WMS来处理，WindowManager和WMS通过Binder来进行跨进程通信。