---
tags: 
alias:
---
在写[[Kotlin 泛型]]参数时，有时候我们可能会想知道某个泛型参数它的具体类型是什么？这个时候就需要用reified关键字来检查了。
在代码里，通过写 if (randomLoot is T) 来对泛型进行检测，编译器会报错 “**不能检测已擦除类型的实例**”。
在通常情况下，[[Kotlin]]不允许对泛型参数 T 做类型检查，因为泛型参数类型会被[[类型擦除]]（ type  erasure）。也就是说， T 的类型信息在运行时是不可知的。
Kotlin 提供了 **reified** 关键字，它允许你在运行时保留类型信息。

https://blog.csdn.net/qq_42165012/article/details/128207426




