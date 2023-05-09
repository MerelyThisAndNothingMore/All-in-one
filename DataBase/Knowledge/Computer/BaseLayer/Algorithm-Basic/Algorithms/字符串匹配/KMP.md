---
tags: 
alias:
---


KMP 是 Knuth–Morris–Pratt 的简称（取名自3位发明人的名字），于1977年发布

# 原理
KMP 会预先根据模式串的内容生成一张 next 表（一般是个数组）：
![](https://img-blog.csdnimg.cn/20200427194138979.png)