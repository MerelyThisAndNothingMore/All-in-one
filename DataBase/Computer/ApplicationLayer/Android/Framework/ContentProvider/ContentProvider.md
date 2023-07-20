---
tags: 
alias:
---
# 介绍
ContentProvider(内容提供者)用于提供数据的统一访问格式，封装底层的具体实现。对于数据的使用者来说，无需知晓数据的来源是数据库、文件，或者网络，只需简单地使用ContentProvider提供的**SQL式**的数据操作接口，也就是增(insert)、删(delete)、改(update)、查(query)四个过程。
 ## 特性
 ### 封装
采用ContentProvider方式，其 解耦了 底层数据的存储方式，使得无论底层数据存储采用何种方式，外界对数 据的访问方式都是统一的，这使得访问简单 & 高效

如一开始数据存储方式 采用 SQLite 数据库，后来把数据库换成 MongoDB，也不会对上层数据 ContentProvider使用代码产生影响
### 提供一种跨进程数据共享的方式。
应用程序间的数据共享还有另外的一个重要话题，就是数据更新通知机制了。因为数据是在多个应用程序中共享的，当其中一个应用程序改变了这些共享数据的时候，它有责任通知其它应用程序，让它们知道共享数据被修改了，这样它们就可以作相应的处理。


`ContentProvider`基于`binder`进行进程间通信，具有较高的安全性。由于其使用共享内存传输数据，也因此具备较高的传输效率。
## ContentProvider
每一个 ContentProvider 都拥有一个公共的 URI ，这个 URI 用于表示这个 ContentProvider 所提供的数据。

ContentProvider(内容提供者)通过 uri 来标识其它应用要访问的数据。  
通过 ContentResolver(内容解析者)的增、删、改、查方法实现对共享数据的操作。 
还可以通过注册 ContentObserver(内容观察者)来监听数据是否发生了变化来对应的刷新页面

ContentProvider:管理数据，提供数据的增删改查操作，数据源可以是数据库、文件、XML、网络等。
ContentResolver:外部进程可以通过 ContentResolver 与 ContentProvider 进行交互。其他应用中 ContentResolver 可以不同 URI 操作不同的 ContentProvider 中的数据。
ContentObserver:观察 ContentProvider 中的数据变化，并将变化通知给外界。
# References 
https://juejin.im/post/6844904062173839368#heading-0 http://gityuan.com/2016/07/30/content-provider/ https://blog.csdn.net/u011733869/article/details/83958712


