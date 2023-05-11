---
tags: 
alias:
---
![](http://hi.csdn.net/attachment/201202/14/0_1329225045x1gB.gif)
![](http://hi.csdn.net/attachment/201202/14/0_1329225045x1gB.gif)
https://juejin.cn/post/7144659801660719111



# 捕获crash
```java
Thread.java

public static void setDefaultUncaughtExceptionHandler(UncaughtExceptionHandler eh)
```
方式，导入一个我们自定义的实现了UncaughtExceptionHandler接口的类
