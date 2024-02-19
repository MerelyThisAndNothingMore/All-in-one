---
tags: 
alias:
- Thread Pool
---

在编程中经常会使用[[线程]]来异步处理任务，但是每个线程的创建和销毁都需要一定的开销。 如果每次执行一个任务都需要开一个新线程去执行，则这些线程的创建和销毁将消耗大量的资源;并且线程都是“各自为政”的，很难对其进行控制。
这时就 需要线程池来对线程进行管理。在[[Java]]1.5中提供了Executor框架用于把任务的提交和执行解耦，任务的提交交给 Runnable 或者 Callable，而 Executor 框架用来处理任务。Executor 框架中 最核心的成员就是 ThreadPoolExecutor，它是线程池的核心实现类。

## ThreadPoolExecutor

![](https://img-blog.csdnimg.cn/20190423104753143.png)

对应参数的说明：

- corePoolSize:核心线程数。默认情况下线程池是空的，只有任务提交时才会创建线程。 如果当前运行的线程数少于 corePoolSize，则创建新线程来处理任务;如果等于或者多 于 corePoolSize，则不再创建。如果调用线程池的 prestartAllcoreThread 方法，线程池会 提前创建并启动所有的核心线程来等待任务。
    
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

-   特点：只有核心线程 & 不会被回收、线程数量固定、任务队列无大小限制（超出的线程任务会在队列中等待）
-   应用场景：控制线程最大并发数
-   具体使用：通过 _Executors.newFixedThreadPool()_ 创建

### 定时线程池（ScheduledThreadPool ）

-   特点：核心线程数量固定、非核心线程数量无限制（闲置时马上回收）
-   应用场景：执行定时 / 周期性 任务
-   使用：通过*Executors.newScheduledThreadPool()*创建

### 可缓存线程池（CachedThreadPool）
特点：只有非核心线程、线程数量不固定（可无限大）、灵活回收空闲线程（具备超时机制，全部回收时几乎不占系统资源）、新建线程（无线程可用时）
任何线程任务到来都会立刻执行，不需要等待
应用场景：执行大量、耗时少的线程任务

### 单线程化线程池（SingleThreadExecutor）
特点：只有一个核心线程（保证所有任务按照指定顺序在一个线程中执行，不需要处理线程同步的问题）

应用场景：不适合并发但可能引起IO阻塞性及影响UI线程响应的操作，如数据库操作，文件操作等

使用：通过*Executors.newSingleThreadExecutor()*创建*
# 使用场景
## IO密集型

## CPU密集型
如果是 CPU 密集型的，可以把核心线程数设置为核心数+1。

# 异常处理
#### Runable执行异常(业务异常)处理方式

由于Runable执行异常并不会影响到整个系统的运行，不会因为在线程池中执行任务报错导致真系统错误退出。所以线程池执行任务的异常处理方式通常有两种：

-   **直接不处理(也可以打印日志)**
-   **捕获异常不让异常往外抛**

#### 提交任务到任务队列已满异常处理

这个异常取决于使用的何种拒绝策略。Java内置的拒绝策略有四种：

-   **CallerRunsPolicy：在任务被拒绝添加后，会调用当前线程池的所在的线程去执行被拒绝的任务**
-   **AbortPolicy：直接抛出异常**
-   **DiscardPolicy：会让被线程池拒绝的任务直接抛弃，不会抛异常也不会执行。**
-   **DiscardOldestPolicy：当任务呗拒绝添加时，会抛弃任务队列中最旧的任务也就是最先加入队列的，再把这个新任务添加进去。**
-   **自定义策略，只要实现RejectedExecutionHandler接口**

对于提交任务到任务队列已满异常，如果不进行catch不影响整个整个系统的运行可以不进行处理，如果可能会导致系统中断。就需要对错误进行处理。

#### 2.3 线程池本身导致的异常

线程池本身导致的异常可能会导致程序的中断，如果程序必须依靠线程池才能完成对应功能，当线程池本身导致异常如上面演示的。那么是否处理异常都无关紧要。整个程序直接崩溃！但是线程池只是一个备选方案，可以将可能的异常进行捕获处理。(但是不能完全杜绝让程序崩溃的问题)

  





