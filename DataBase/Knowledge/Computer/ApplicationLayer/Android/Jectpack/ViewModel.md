---
tags: 
alias:
---

https://blog.csdn.net/c10wtiybq1ye3/article/details/89934891

https://www.jianshu.com/p/41c56570a266?utm_campaign=haruki&utm_content=note&utm_medium=seo _notes&utm_source=recommendation

https://www.jianshu.com/p/ebdf656b6dd4 https://blog.csdn.net/qq_15988951/article/details/105106867  
how  
viewmodel = ViewModelProvider(this).get(MyViewModel::class.java) what  
ViewModel 类旨在以注重生命周期的方式存储和管理界面相关的数据 why  
Activity配置更改重建时(比如屏幕旋转)保留数据  
问题

https://blog.csdn.net/u014093134/article/details/104082453

例如你的 APP 某个 Activity 中包含一个 列表，因为配置更改而重新创建 Activity 后(例如众所周知的屏幕旋转 发生后需手动保存数据在旋转后进行恢复)，新 Activity 必须重新提取列表数据，对于简单数据，Activity 可以使用 onSaveInstanceState() 方法从 onCreate() 中的捆绑包恢复数据，但这种方法仅适合可以序列化再反序列化但少量数 据，不适合数量可能较大但数据，如用户列表或位图

因为ViewModel的生命周期是比Activity还要长，所以ViewModel可以持久保存UI数据。

通常在系统首次调用 Activity 对象的 onCreate() 方法时请求 ViewModel。系统可能会在 Activity 的整个生命周 期内多次调用 onCreate()，如在旋转设备屏幕时。 所以当前Activity的生命周期不断变化，经历了被销毁重新创建， 而ViewModel的生命周期没有发生变化，Activity因为配置更改或者被系统意外回收的时候，会自动保存数 据。在 Activity重建的时候就可以继续使用销毁之前保存的数据。

源码 https://www.jianshu.com/p/ebdf656b6dd4  
ComponentActivity 中  
onRetainNonConfigurationInstance是在onStop() 和 onDestroy()之间被调用，它内部会保存ViewModel数据; 它会被ActivityThread中performDestroyActivity方法调用，它执行在onDestroy生命周期之前 Activity的attach时会调用getLastNonConfigurationInstance来恢复数据



ViewModel将一直留在内存中，直到限定其存在时间范围的Lifecycle(activity dstroy掉用clear) 永久消失:

UI组件(Activity与Fragment、Fragment与Fragment)间实现数据共享

当这两个 Fragment 各自获取 ViewModelProvider 时，它们会收到相同的 ViewModel 实例 ViewModelProvider通过ViewModelStore获取ViewModel，FragmentActivity自身是持有ViewModelStore

避免内存泄漏的发生。

https://www.jianshu.com/p/41c56570a266

引入了ViewModel和LiveData之后，可以实现vm和view的解耦，只是view引用vm，而vm是不持有view的引用 的。在activity退出之后即是还有网络在继续也不会引发内存泄漏和空指针异常

源码解析

https://blog.csdn.net/c10wtiybq1ye3/article/details/89934891

1.Factory是ViewModelProvider的一个内部接口，它的实现类是拿来构建ViewModel实例

3.get mViewModelStore.get(key) create通过newInstance(application)去实例化

ViewModelStore:和名字一样，就是存储ViewModel的，它里面定义了一个HashMap来存储ViewModel，key 值是ViewModel全路径+一个默认的前缀