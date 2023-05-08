---
tags: 
alias:
---
https://carsonho.blog.csdn.net/article/details/82992269
## 定义
Java中的1个关键字

## 作用
保证同一时刻最多只有1个线程执行 被Synchronized修饰的方法 / 代码

其他线程 必须等待当前线程执行完该方法 / 代码块后才能执行该方法 / 代码块

## 应用场景
保证线程安全，解决多线程中的并发同步问题（实现的是阻塞型并发），具体场景如下：

修饰 实例方法 / 代码块时，（同步）保护的是同一个对象方法的调用 & 当前实例对象
修饰 静态方法 / 代码块时，（同步）保护的是 静态方法的调用 & class 类对象
## 原理

1.  依赖 `JVM` 实现同步
2.  底层通过一个监视器对象`（monitor）`完成， `wait（）`、`notify（）` 等方法也依赖于 monitor 对象

> 监视器锁（monitor）的本质 依赖于 底层操作系统的互斥锁（Mutex Lock）实现

# 具体使用

`Synchronized` 用于 修饰 代码块、类的实例方法 & 静态方法
![](https://img-blog.csdnimg.cn/img_convert/05330773ba468a588fa4e8f42dd7a7e9.png)

![](https://img-blog.csdnimg.cn/img_convert/713d266272dd5fba98702699e752e7c5.png)

![](https://img-blog.csdnimg.cn/img_convert/c6707ccca6925053b7d4199e9cb6e628.png)


# 特点
![](https://img-blog.csdnimg.cn/img_convert/a05fa6bf660e8677211be883f947472d.png)
![](https://img-blog.csdnimg.cn/img_convert/2233b1a1eaa2fcdbbf68061726970854.png)
# 其他控制并发 / 线程同步方式

## Lock、ReentrantLock

-   简介
![](https://img-blog.csdnimg.cn/img_convert/06262ae3c9ea1f5c11d1d1452c7f32d5.png)

![](https://img-blog.csdnimg.cn/img_convert/29980bef734d7a169e5f490d58a0a38c.png)

## CAS
### 定义

`Compare And Swap`，即 比较 并 交换，是一种解决并发操作的乐观锁

> `synchronized`锁住的代码块：同一时刻只能由一个线程访问，属于悲观锁

### 优点

资源耗费少：相对于`synchronized`，省去了挂起线程、恢复线程的开销

> 但，若迟迟得不到更新，死循环对`CPU`资源也是一种浪费

### 具体实现方式

-   使用CAS有个“先检查后执行”的操作
-   而这种操作在Java中是典型的不安全的操作，所以 `CAS`在实际中是**由`C++`通过调用CPU指令实现的**


### 典型应用：`AtomicInteger`

对 i++ 与 i–，通过`compareAndSet` & 一个死循环实现


