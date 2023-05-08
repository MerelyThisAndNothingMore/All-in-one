---
tags: 
alias:
---
ActivityThread 在Android中它就代表了Android的主线程,它是创建完新进程之后,main函数被加载，然后执行一个loop的循环

使当前线程进入消息循环，并且作为主线程。

ApplicationThread  
ApplicationThread是ActivityThread的内部类， 是一个Binder对象。在此处它是作为IApplicationThread对象

的server端等待client端的请求然后进行处理，最大的client就是AMS。



在Activity的onSaveInstanceState方法回调时，put到参数outState(Bundle)里面。outState就是 ActivityClientRecord的state。

ActivityClientRecord实例，都存放在ActivityThread的mActivities里面。

