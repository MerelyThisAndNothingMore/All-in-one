---
tags: 
- softwareEngineering 
alias:
- 依赖倒置原则
---
# Introduction
High level modules should not depend upon low level modules. Both should depend upon abstractions. Abstractions should not depend upon details. Details should depend upon abstractions.
- 每个类尽量都有接口或抽象类，或者抽象类和接口两者都具备
- 变量的表面类型尽量是接口或者是抽象类
- 任何类都不应该从具体类派生
