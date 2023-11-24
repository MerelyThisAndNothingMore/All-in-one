---
tags: 
alias:
---

在[[Kotlin]]中，如果你在一个[[内联函数]]中传递了一个[[Lambda表达式]]作为参数，这个lambda表达式可以直接使用非局部返回。即，它可以直接从包含它的函数中返回，而不仅仅是从lambda自身返回。

使用`crossinline`修饰符的主要目的是禁止在传递给内联函数的lambda表达式中使用非局部返回。

```kotlin
inline fun runOperation(crossinline operation: () -> Unit) {
    // ... 一些其他代码 ...
    val thread = Thread {
        operation() // 如果lambda中有return语句，crossinline会阻止非局部返回
    }
    thread.start()
    // ... 一些其他代码 ...
}

```
