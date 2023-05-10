---
tags: 
alias:
---

`Lock` 是 Java并发编程中很重要的一个接口，它要比 Synchronized 关键字更能直译"锁"的概念，Lock需要手动加锁和手动解锁，一般通过 lock.lock() 方法来进行加锁， 通过 lock.unlock() 方法进行解锁。与 Lock 关联密切的锁有 ReetrantLock 和 ReadWriteLock。

`ReetrantLock` 实现了Lock接口，它是一个可重入锁，内部定义了公平锁与非公平锁。

`ReadWriteLock` 一个用来获取读锁，一个用来获取写锁。也就是说将文件的读写操作分开，分成2个锁来分配给线程，从而使得多个线程可以同时进行读操作。ReentrantReadWirteLock实现了ReadWirteLock接口，并未实现Lock接口。




# Synchronized 和 Lock 的主要区别
-   **存在层面**：Syncronized 是Java 中的一个关键字，存在于 JVM 层面，Lock 是 Java 中的一个接口  
    
-   **锁的释放条件**：
- 1. 获取锁的线程执行完同步代码后，自动释放；
- 2. 线程发生异常时，JVM会让线程释放锁；
- Lock 必须在 finally 关键字中释放锁，不然容易造成线程死锁  
    
-   **锁的获取**: 
- 在 Syncronized 中，假设线程 A 获得锁，B 线程等待。如果 A 发生阻塞，那么 B 会一直等待。
- 在 Lock 中，会分情况而定，Lock 中有尝试获取锁的方法，如果尝试获取到锁，则不用一直等待  
    
-   **锁的状态**：
- Synchronized 无法判断锁的状态，
- Lock 则可以判断  
    
-   **锁的类型**：
- Synchronized 是可重入，不可中断，非公平锁；
- Lock 锁则是 可重入，可判断，可公平锁  
    
-   **锁的性能**：
- Synchronized 适用于少量同步的情况下，性能开销比较大。
- Lock 锁适用于大量同步阶段：  
    

-   -   Lock 锁可以提高多个线程进行读的效率(使用 readWriteLock)  
        
    -   在竞争不是很激烈的情况下，Synchronized的性能要优于ReetrantLock，但是在资源竞争很激烈的情况下，Synchronized的性能会下降几十倍，但是ReetrantLock的性能能维持常态；  
        
    -   ReetrantLock 提供了多样化的同步，比如有时间限制的同步，可以被Interrupt的同步（synchronized的同步是不能Interrupt的）等。










