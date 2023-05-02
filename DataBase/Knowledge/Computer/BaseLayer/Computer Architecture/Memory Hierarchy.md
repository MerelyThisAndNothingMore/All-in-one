---
tags:
- computerArchitecture 
alias: 
- 存储器层次
---
![](https://gd-hbimg.huaban.com/2a3f6b86cb49af93373d09e1c30c59e569c48a4f17144-YYrXYX) 
This notion of inserting a smaller, faster storage device (e.g., cache memory) between the processor and a larger, slower device (e.g., main memory) turns out to be a general idea. In fact, the storage devices in every computer system are organized as a memory hierarchy.
The main idea of a memory hierarchy is that storage at one level serves as a cache for storage at the next lower level.
存储器层次结构的主要思想是上一层的存储器作为低一层存储器的高速缓存。因此，寄存器文件就是Ll 的高速缓存， Ll 是L2 的高速缓存， L2 是L3 的高速缓存， L3 是主存的高速缓存，而主存又是磁盘的高速缓存。在某些具有分布式文件系统的网络系统中，本地磁盘就是存储在其他系统中磁盘上的数据的高速缓存。
