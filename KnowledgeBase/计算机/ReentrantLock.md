---
tags: 
aliases:
  - 重入锁
---

重入锁(ReentrantLock)是[[Java Lock接口]]最常见的一种实现，顾名思义，它与[[Synchronized]]一样是可重入的。在基本用法上，ReentrantLock也与synchronized很相似，只是代码写法上稍有区别而已。不过，ReentrantLock与synchronized相比增加了一些高级功能，主要有以下三项:

1. 等待可中断:是指当持有锁的线程长期不释放锁的时候，正在等待的线程可以选择放弃等待，改为处理其他事情。可中断特性对处理执行时间非常长的同步块很有帮助。
2. 公平锁:是指多个线程在等待同一个锁时，必须按照申请锁的时间顺序来依次获得锁;而非公平锁则不保证这一点，在锁被释放时，任何一个等待锁的线程都有机会获得锁。synchronized中的锁是非 公平的，Reent rantLock在默认情况下也是非公平的，但可以通过带布尔值的构造函数要求使用公平 锁。不过一旦使用了公平锁，将会导致ReentrantLock的性能急剧下降，会明显影响吞吐量。
3. 锁绑定多个条件:是指一个ReentrantLock对象可以同时绑定多个Condition对象。在synchronized中，锁对象的wait()跟它的notify()或者notifyAll()方法配合可以实现一个隐含的条件，如果要和多于一个的条件关联的时候，就不得不额外添加一个锁;而ReentrantLock则无须这样做，多次调用 newCondition()方法即可。

# 使用建议

尽管在JDK 5时代ReentrantLock曾经在性能上领先过synchronized，但这已经是十多年之前的胜利 了。从长远来看，[[Java 虚拟机]]更容易针对synchronized来进行优化，因为Java虚拟机可以在[[线程]]和[[对象]]的元数据中记录synchronized中锁的相关信息，而使用J.U.C中的Lock的话，Java虚拟机是很难得知具体 哪些锁对象是由特定线程锁持有的。

