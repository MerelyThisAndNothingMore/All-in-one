---
tags: 
alias:
---
# 原理

MVP框架由3部分组成:View负责显示，Presenter负责逻辑处理，Model提供数据。

View: 显示数据, 并向Presenter报告用户行为。与用户进行交互(在Android中体现为Activity)。

Presenter: 逻辑处理，从Model拿数据，回显到UI层，响应用户的行为。

Model:负责存储、检索、操纵数据(有时也实现一个Model interface用来降低耦合)。

google todo-mvp加入契约类来统一管理view与presenter的所有的接口，这种方式使得view与presenter中有哪 些功能，一目了然

# 优点

1.分离视图逻辑和业务逻辑，降低了耦合，修改视图而不影响模型，不需要改变Presenter的逻辑 模型与视图完 全分离，我们可以修改视图而不影响模型;

2.视图逻辑和业务逻辑分别抽象到了View和Presenter的接口中，Activity只负责显示，代码变得更加简洁，提高 代码的阅读性。

3.Presenter被抽象成接口，可以有多种具体的实现，所以方便进行单元测试。

Presenter是通过interface与View(Activity)进行交互的，这说明我们可以通过自定义类实现这个interface来模拟 Activity的行为对Presenter进行单元测试，省去了大量的部署及测试的时间(不需要将应用部署到Android模拟器或 真机上，然后通过模拟用 户操作进行测试)

缺点  
1.那就是对 UI 的操作必须在 Activity 与 Fragment 的生命周期之内，更细致一点，最好在 onStart() 之后

onPause()之前，否则极其容易出现各种异常，内存泄漏。 2.Presenter与View之间的耦合度高，app中很多界面都使用了同一个Presenter 。一旦需要变更，那么视图需要变更了。

# MVP如何设计避免内存泄漏?
Mvp模式在封装的时候会造成内存泄漏，因为presenter层，需要做网络请求，所以就需要考虑到网络请求的取 消操作，如果不处理，activity销毁了，presenter层还在请求网络，就会造成内存泄漏。 如何解决Mvp模式造成的 内存泄漏? 只要presenter层能感知activity生命周期的变化，在activity销毁的时候，取消网络请求，就能解决这个 问题。 下面开始封装activity和presenter。


