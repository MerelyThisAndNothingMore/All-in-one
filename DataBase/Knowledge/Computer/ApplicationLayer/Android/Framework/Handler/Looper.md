---
tags: 
alias:
---

首先Looper,prepare()方法在本线程中保存了一个Looper实例,然后该实例中保存一个MessageQueue对象;
因为Looper.prepare()在一个线程中只能调用一次,所以MessageQueue在一个线程中只会存在一个.

Looper.loop()会让当前的线程进入一个无限循环,不断地从MessageQueue的实例中读取消息,然后回调,msg.target.dispatchMessage(msg)方法.

Handler的构造方法,会首先得到当前线程中保存的Looper实例,进而与Looper实例的MessageQueue相关联.

Handler的sendMessage()方法,会给msg的target赋值为handler自身,然后加入MessageQueue中.

在构造Handler实例时,我们会重写handlerMessage方法.也就是msg.target,dispatchMessage(msg)最终调用的方法.




