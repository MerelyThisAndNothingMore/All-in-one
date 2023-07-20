---
tags: 
alias:
---
每个Handler会被添加到 Message 的target字段上面，Looper 通过调用 Message.target.handleMessage() 来 让 Handler 处理消息。
## 创建方法
1. Message msg = new Message(); 每次需要Message对象的时候都创建一 个新的对象，每次都要去堆内存开辟对象存储空间 
2. Message msg = Message.obtain(); obtainMessage能避免重复 Message创建对象。它先判断消息池是不是为空，如果非空的话就从消息池表头的Message取走,再把表头指向 next。 如果消息池为空的话说明还没有Message被放进去，那么就new出来一个Message对象。消息池使用 Message 链表结构实现，消息池默认最大值 50。消息在loop中被handler分发消费之后会执行回收的操作，将该消 息内部数据清空并添加到消息链表的表头。 
3. Message msg = handler.obtainMessage(); 其内部也是调用的obtain() 方法