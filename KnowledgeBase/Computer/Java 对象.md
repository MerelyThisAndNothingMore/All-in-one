---
tags: 
alias:
---
# 创建流程
![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1kY2FjYTJhYTc4NDMwMGZjLnBuZw?x-oss-process=image/format,png)


## 类加载检查

当[[Java 虚拟机]]遇到一条字节码new指令时，
首先将去检查这个指令的参数是否能在常量池中定位到 一个类的符号引用，
并且检查这个符号引用代表的类是否已被加载、解析和初始化过。
如果没有，那必须先执行相应的[[Java 类加载|类加载过程]]

## 内存分配

在类加载检查通过后，接下来虚拟机将为新生对象分配[[内存]]。对象所需内存的大小在类加载完成后便可完全确定，为对象分配空间的任务实际上便等同于把一块确定大小的内存块从[[Java 堆]]中划分出来。

分配内存时，如果内存规整，使用[[指针]]碰撞，如果不规整，则使用空闲列表。内存是否规整取决于垃圾收集器是否有空间整理能力。

对象创建在虚拟机中是非常频繁的行为，即使仅仅修改一个指针所指向的位置，在并发情况下也并不是[[线程安全]]的，这里有两种处理思路：

1. 对分配内存空间的动作进行同步处理——实际上虚拟机是采用CAS配上失败 重试的方式保证更新操作的原子性;
2. 把内存分配的动作按照线程划分在不同的空间之中进 行，即每个线程在Java堆中预先分配一小块内存，称为本地线程分配缓冲(Thread Local Allocation Buffer，TLAB)，哪个线程要分配内存，就在哪个线程的本地缓冲区中分配，只有本地缓冲区用完 了，分配新的缓存区时才需要同步锁定。

## 初始化

1. 初始化内存空间为零
2. 根据对象头中的数据，配置对象的初始信息。


# 对象的内存布局
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cc597c75fe5342cd9b3a253980f95524~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS1mOWQyNTk3NTczMjE2NGJjLnBuZw?x-oss-process=image/format,png)
# 对象的访问定位

[[Java]]程序通过栈上的引用类型数据（reference） 来访问Java堆上的对象

![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS0yZjQ5MjgxNzNlNzM0ZTNlLnBuZw?x-oss-process=image/format,png)

# Java对象如何判断存活
## ![[引用计数法|引用计数法]]
## ![[可达性分析算法|可达性分析算法]] 