---
tags: 
- softwareEngineering 
alias:
- 里氏替换原则
---
# Introduction
[[Function|Function]]s that use pointers or references to base
[[class|class]]es must be able to use [[对象|对象]]s of derived classes without knowing it.

1. 子类必须完全实现父类的方法
2. 子类可以有自己的个性
3. 覆盖或实现父类的方法时输入参数可以被放大
   子类的入参可以是父类入参的父类
4. 覆写或实现父类的方法时输出结果可以被缩小
   子类的返回值可以是父类返回值的子类