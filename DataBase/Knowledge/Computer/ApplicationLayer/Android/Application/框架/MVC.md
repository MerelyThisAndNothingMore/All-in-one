---
tags: 
alias:
---

原理  
视图层(View) 一般采用XML文件进行界面的描述，这些XML可以理解为AndroidApp的View。 控制层(Controller)  
Android的控制层的重任通常落在了众多的Activity的肩上。  
模型层(Model)

我们针对业务模型，建立的数据结构和相关的类，就可以理解为AndroidApp的Model，Model是与View无关， 而与业务相关的。对数据库的操作、对网络等的操作都应该在Model里面处理，当然对业务计算等操作也是必须放在 的该层的。

缺点 随着界面及其逻辑的复杂度不断提升，Activity类的职责不断增加，以致变得庞大臃肿。

