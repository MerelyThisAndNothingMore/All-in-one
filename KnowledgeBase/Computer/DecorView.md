---
tags: 
alias:
---

[[Android Activity]]所对应的视图其实是在其对应的PhoneWindow中管理着的，它就是鼎鼎大名的DecorView。它是什么时候创建的呢？平时我们使用Activity时，设置一个布局是通过setContentView来完成的，它内部代码如下

