---
tags: 
alias:
- Compare and Swap
---
CAS 全称是 compare and swap，是一种用于在多线程环境下实现同步功能的机制。CAS 操作包含三个操作数 -- 内存位置、预期数值和新值。
CAS 的实现逻辑是将内存位置处的数值与预期数值想比较，若相等，则将内存位置处的值替换为新值。若不相等，则不做任何操作。

CAS指令需要有三个操作数，分别是内存位置(在Java中可以简单地理解为变量的内存地址，用V 表示)、旧的预期值(用A表示)和准备设置的新值(用B表示)。CAS指令执行时，当且仅当V符合 A时，处理器才会用B更新V的值，否则它就不执行更新。但是，不管是否更新了V的值，都会返回V的 旧值，上述的处理过程是一个原子操作，执行期间不会被其他线程中断。


# CAS 与 Synchronized 的使用情景：　　　

1.  对于资源竞争较少（线程冲突较轻）的情况，使用synchronized同步锁进行线程阻塞和唤醒切换以及用户态内核态间的切换操作额外浪费消耗cpu资源；而CAS基于硬件实现，不需要进入内核，不需要切换线程，操作自旋几率较少，因此可以获得更高的性能。
    
2.  对于资源竞争严重（线程冲突严重）的情况，CAS自旋的概率会比较大，从而浪费更多的CPU资源，效率低于synchronized。


尽管CAS看起来很美好，既简单又高效，但显然这种操作无法涵盖互斥同步的所有使用场景，并 且CAS从语义上来说并不是真正完美的，它存在一个逻辑漏洞:如果一个变量V初次读取的时候是A 值，并且在准备赋值的时候检查到它仍然为A值，那就能说明它的值没有被其他线程改变过了吗?这 是不能的，因为如果在这段期间它的值曾经被改成B，后来又被改回为A，那CAS操作就会误认为它从 来没有被改变过。这个漏洞称为CAS操作的“ABA问题”。J.U.C包为了解决这个问题，提供了一个带有

标记的原子引用类AtomicStamp edReference，它可以通过控制变量值的版本来保证CAS的正确性。不过 目前来说这个类处于相当鸡肋的位置，大部分情况下ABA问题不会影响程序并发的正确性，如果需要 解决ABA问题，改用传统的互斥同步可能会比原子类更为高效。