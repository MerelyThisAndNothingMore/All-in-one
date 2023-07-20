---
tags: 
alias:
---
# 原理
Activity之间切换交互，需要system_server进程作为中介，而两者之间的交流是通过Binder机制，而Binder机制依赖于Android内核中的Binder缓冲区，因此参数传递的大小依赖于Binder缓冲区的大小并且数据必须是可序列化的。
# Activity之间传递数据的方式[[Intent]]是否有大小限制，如果传递的数据量偏大，有哪些方案
## 存在大小限制
startActivity
->startActivityForResult
->Instrumentation.execStartActivity 
->ActivityManger.getService().startActivity
intent中携带的数据要从[[app进程]]传输到[[AMS]]进程，再由AMS进程传输到目标Activity所在进程。这一过程通过[[Binder]]实现，数据存放在[[Binder]]的事务缓冲区内，而事务缓冲区存在大小限制，一般不超过1M。
[[Binder]] 本身就是为了进程间频繁-灵活的通信所设计的, 并不是为了拷贝大量数据
## 更多方案
### 非IPC
单例,eventBus, Application, sqlite、shared preference、file 都可以;
### [[IPC]]
[[共享内存]]

