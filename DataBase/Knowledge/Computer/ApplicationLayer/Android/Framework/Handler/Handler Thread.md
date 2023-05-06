---
tags: 
alias:
---
HandlerThread本质上是一个线程类，它继承了Thread; HandlerThread有自己的内部Looper对象，通过 Looper.loop()进行looper循环; 通过获取HandlerThread的looper对象传递给Handler对象，然后在 handleMessage()方法中执行异步任务;

优势: 
1.将loop运行在子线程中处理,减轻了主线程的压力,使主线程更流畅,有自己的消息队列,不会干扰UI线程 2. 串行执行,开启一个线程起到多个线程的作用

劣势: 
1.由于每一个任务队列逐步执行,一旦队列耗时过长,消息延时 
2.对于IO等操作,线程等待,不能并发 

我们可以使用HandlerThread处理本地IO读写操作(数据库，文件)，因为本地IO操作大多数的耗时属于毫秒级别，对于单线程 + 异步队列的形式 不会产生较大的阻塞