---
tags:
- computerArchitecture
alias:
- 线程
---
Threads are an increasingly important programming model because of the requirement for concurrency in network servers, because it is easier to share data between multiple threads than between multiple processes, and because threads are typically more efficient than processes.
线程进程的一个实体，是CPU调度和分派的基本单位，他是比进程更小的能独立运行的基本单位,线 程自己基本上不拥有系统资源。在运行时，只是暂用一些计数器、寄存器和栈 。
-   一个基本的CPU执行单元 & 程序执行流的最小单元

> 1.  比进程更小的可独立运行的基本单位，可理解为：轻量级进程
> 2.  组成：线程ID + 程序计数器 + 寄存器集合 + 堆栈
> 3.  注：线程自己不拥有系统资源，与其他线程共享进程所拥有的全部资源。

-   作用  
    减少程序在并发执行时所付出的时空开销，提高操作系统的并发性能。
    
-   状态说明  
    拥有类似于进程的**就绪、阻塞、运行**3种基本状态，具体如下图：
![](https://img-blog.csdnimg.cn/img_convert/bbe530af9a30e0ca6f91cecab792ab5e.png)

![](https://img-blog.csdnimg.cn/img_convert/d6146fc2e7a3dc92753b76696faa2990.png)
