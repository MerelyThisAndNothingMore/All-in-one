---
tags: 
alias:
---

# 定义
![](https://gd-hbimg.huaban.com/21e403c8403f48072aab06f9ff473cf169ef4b7fce96-uFmpWb)

View层包含布局,以及布局生命周期控制器(Activity/Fragment)

ViewModel与Presenter大致相同,都是负责处理数据和实现业务逻辑,但 ViewModel层不应该直接或者间接地持 有View层的任何引用

model层是数据层 Model，模型层，即数据模型，用于获取和存储数据。

View，视图，即Activity/Fragment

ViewModel，视图模型，负责业务逻辑。

MVVM 的本质是 数据驱动，把解耦做的更彻底，viewModel不持有view 。

View 产生事件，使用 ViewModel进行逻辑处理后，通知Model更新数据，Model把更新的数据给ViewModel， ViewModel自动通知View更新界面，而不是主动调用View的方法

LiveData是具有生命周期的可观察的数据持有类。理解它需要注意这几个关键字，生命周期，可观察，数据持有 类。

DataBinding用来实现View层与ViewModel数据的双向绑定，Data Binding 減輕原本 MVP 中 Presenter 要與 p 和 View 互動的職責

# MVVM的优点

核心思想是观察者模式,它通过事件和转移View层数据持有权来实现View层与ViewModel层的解耦.

1.耦合度更低，复用性更强，没有内存泄漏

2.结合jetpack，写出更优雅的代码

# 缺点

ViewModel与View层的通信变得更加困难了,所以在一些极其简单的页面中请酌情使用,否则就会有一种脱裤子放 屁的感觉,在使用MVP这个道理也依然适用.


