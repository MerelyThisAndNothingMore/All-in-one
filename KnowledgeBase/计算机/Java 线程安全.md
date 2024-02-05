---
tags: 
alias:
---

![[线程安全#定义]]
# 实现手段

## [[互斥同步]]

在[[Java]]里面，最基本的互斥同步手段就是[[Synchronized]]关键字，这是一种块结构(Block Structured)的同步语法。synchronized关键字经过Javac编译之后，会在同步块的前后分别形成 monitorenter和monitorexit这两个字节码指令。这两个字节码指令都需要一个reference类型的参数来指明要锁定和解锁的对象。如果Java源码中的synchronized明确指定了对象参数，那就以这个对象的引用作 为reference;如果没有明确指定，那将根据sy nchroniz ed修饰的方法类型(如实例方法或类方法)，来 决定是取代码所在的对象实例还是取类型对应的Class对象来作为线程要持有的锁。