---
tags: 
alias:
---
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDQzNjUtZjEyNmFiMDRjZjZkYzA3MS5wbmc?x-oss-process=image/format,png)
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDQzNjUtMzk0YmQzMGRkZjg5NGNmOS5wbmc?x-oss-process=image/format,png)


## 原理  
视图层(View) 一般采用XML文件进行界面的描述，这些XML可以理解为AndroidApp的View。 控制层(Controller)  
Android的控制层的重任通常落在了众多的Activity的肩上。  
模型层(Model)

我们针对业务模型，建立的数据结构和相关的类，就可以理解为AndroidApp的Model，Model是与View无关， 而与业务相关的。对数据库的操作、对网络等的操作都应该在Model里面处理，当然对业务计算等操作也是必须放在 的该层的。

## 缺点 

**Activity责任不明、十分臃肿**  
`Activity`由于其生命周期的功能，除了担任`View`层的部分职责（加载应用的布局、接受用户操作），还要承担`Controller`层的职责（业务逻辑的处理）  
随着界面的增多 & 逻辑复杂度提高，`Activity`类的代码量不断增加，越加臃肿

