---
tags: 
alias:
- Thread Pool
---

在编程中经常会使用[[线程]]来异步处理任务，但是每个线程的创建和销毁都需要一定的开销。 如果每次执行一个任务都需要开一个新线程去执行，则这些线程的创建和销毁将消耗大量的资源;并且线程都是“各自为政”的，很难对其进行控制。
这时就 需要线程池来对线程进行管理。在[[Java]]1.5中提供了Executor框架用于把任务的提交和执行解耦，任务的提交交给 Runnable 或者 Callable，而 Executor 框架用来处理任务。Executor 框架中 最核心的成员就是 ThreadPoolExecutor，它是线程池的核心实现类。

![[Pasted image 20240219195259.png]]

## ThreadPoolExecutor

![](https://img-blog.csdnimg.cn/20190423104753143.png)

对应参数的说明：

- corePoolSize:核心线程数。默认情况下线程池是空的，只有任务提交时才会创建线程。 如果当前运行的线程数少于 corePoolSize，则创建新线程来处理任务;如果等于或者多于 corePoolSize，则不再创建。如果调用线程池的 prestartAllcoreThread 方法，线程池会 提前创建并启动所有的核心线程来等待任务。
    
- maximumPoolSize:线程池允许创建的最大线程数。如果任务队列满了并且线程数小于 maximumPoolSize 时，则线程池仍旧会创建新的线程来处理任务。
    
- keepAliveTime:非核心线程闲置的超时时间。超过这个时间则回收。如果任务很多， 并且每个任务的执行事件很短，则可以调大keepAliveTime来提高线程的利用率。另外， 如果设置 allowCoreThreadTimeOut 属性为 true 时，keepAliveTime 也会应用到核心线程 上，
    
- TimeUnit:keepAliveTime 参数的时间单位。可选的单位有天(DAYS)、小时(HOURS)、 分钟(MINUTES)、秒(SECONDS)、毫秒(MILLISECONDS)等。
    
- workQueue:任务队列。如果当前线程数大于 corePoolSize，则将任务添加到此任务队 列中。该任务队列是 BlockingQueue 类型的，也就是阻塞队列。这在前面已经介绍过了， 这里就不赘述了。
    
- ThreadFactory:线程工厂。可以用线程工厂给每个创建出来的线程设置名字。一般情 况下无须设置该参数。
    
- RejectedExecutionHandler:饱和策略。这是当任务队列和线程池都满了时所采取的应 对策略，默认是 AbordPolicy，表示无法处理新任务，并抛出 RejectedExecutionException 异常。此外还有 3 种策略，它们分别如下。
    
    (1)CallerRunsPolicy:用调用者所在的线程来处理任务。此策略提供简单的反馈控制 机制，能够减缓新任务的提交速度。
    
    (2)DiscardPolicy:不能执行的任务，并将该任务删除。 (3)DiscardOldestPolicy:丢弃队列最近的任务，并执行当前的任务。


## 常见的4类功能线程池
![](https://img-blog.csdnimg.cn/img_convert/8c39dcc0786d030dce0453cb5967e5bb.png)
### 定长线程池（FixedThreadPool）

```java
public static ExecutorService newFixedThreadPool(int nThreads) {  
    return new ThreadPoolExecutor(nThreads, nThreads,  
                                  0L, TimeUnit.MILLISECONDS,  
                                  new LinkedBlockingQueue<Runnable>());  
}
```

### 定时线程池（ScheduledThreadPool ）

```java
public ScheduledThreadPoolExecutor(int corePoolSize) {  
    super(corePoolSize, Integer.MAX_VALUE,  
          DEFAULT_KEEPALIVE_MILLIS, MILLISECONDS,  
          new DelayedWorkQueue());  
}
```

### 可缓存线程池（CachedThreadPool）

```java
public static ExecutorService newCachedThreadPool() {  
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,  
                                  60L, TimeUnit.SECONDS,  
                                  new SynchronousQueue<Runnable>());  
}
```

### 单线程化线程池（SingleThreadExecutor）

```java
public static ExecutorService newSingleThreadExecutor() {  
    return new FinalizableDelegatedExecutorService  
        (new ThreadPoolExecutor(1, 1,  
                                0L, TimeUnit.MILLISECONDS,  
                                new LinkedBlockingQueue<Runnable>()));  
}
```



  





