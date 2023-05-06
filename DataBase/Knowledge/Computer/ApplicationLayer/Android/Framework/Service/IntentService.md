---
tags: 
alias:
---
# 介绍
## 特性
IntentService 是 Service 的子类，默认开启了一个工作线程HandlerThread，使用这个工作线程逐一处理所有 启动请求，在任务执行完毕后会自动停止服务。只要实现一个方法 onHandleIntent，该方法会接收每个启动请求的 Intent，能够执行后台工作和耗时操作。
可以启动 IntentService 多次，而每一个耗时操作会以队列的方式在 IntentService 的 onHandlerIntent 回调方 法中执行，并且，每一次只会执行一个工作线程，执行完第一个再执行第二个。并且等待所有消息都执行完后才终止 服务。
## 原理
1.创建一个名叫 ServiceHandler 的内部 Handler 
2.把内部Handler与HandlerThread所对应的子线程进行绑定  
3.HandlerThread开启线程 创建自己的looper  
4.通过 onStartCommand() intent，依次插入到工作队列中，并发送给 onHandleIntent()逐个处理 
可以用作后台下载任务 静默上传
## 优点
IntentService会创建独立的worker线程来处理所有的Intent请求 Service主线程不能处理耗时操 作,IntentService不会阻塞UI线程，而普通Serveice会导致ANR异常。
为Service的onBind()提供默认实现，返回null;onStartCommand提供默认实现，将请求Intent添加到队列中。 所有请求处理完成后，IntentService会自动停止，无需调用stopSelf()方法停止Service。





