---
tags: 
alias:
---
-   `Java`虚拟机在运行`Java`程序时，会管理着一块内存区域：运行时数据区
-   在运行时数据区里，会根据用途进行划分：
    1.  [[Java 虚拟机栈]]
    2.  [[本地方法栈]]
    3.  [[Java 堆]]
    4.  [[方法区]]
    5.  程序计数器


成员变量:生命周期伴随类对象,类对象回收时回收 存在[[Java 堆]]里 

静态变量:不回收 在方法区 随着类的[[Java 类加载|加载]]而加载，随着类的消失而消失，由于类需要非常长时间的不使用，不利用，不关联，才有可能会被回收机制回收， 所以静态成员变量的生命周期特别长，除非是共享数据，否则不建议使用静态； 

局部变量:方法调用时创建 方法结束时被标记为可回收 存在[[Java 虚拟机栈]]里。



![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1mMzcwYjQ2ZjBkYjA3YmVlLnBuZw?x-oss-process=image/format,png)


![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDQzNjUtMWM2Njk1MzIwMGU4MjUzZC5wbmc?x-oss-process=image/format,png)





