---
tags: 
alias:
---

# 数据结构
![](https://img-blog.csdnimg.cn/img_convert/9c5c1eb12ee54aa7db717506cba4603c.png)

## 存储流程
![](https://img-blog.csdnimg.cn/img_convert/a521254d1ab77539ac5b348a11248093.png)

## 数组元素 & 链表节点的 实现类

-   `HashMap`中的数组元素 & 链表节点 采用 `Node`类 实现

## 红黑树节点 实现类

-   `HashMap`中的红黑树节点 采用 `TreeNode` 类 实现

## 红黑树转化条件
### 数组长度
`如果数组的长度大于等于64的话，才会将该节点的 链表转换成树`。如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树，以减少搜索时间。
### 阈值
在[[链表]]个数为8时转化为[[红黑树]]，原因是泊松分布。
用随机的哈希码，容器中节点分布在 hash 桶中的频率遵循**[泊松分布](https://link.zhihu.com/?target=http%3A//en.wikipedia.org/wiki/Poisson_distribution)**，按照泊松分布的计算公式计算出了桶中元素个数和概率的对照表，可以看到链表中元素个数为 8 时的概率已经非常小，小于千万分之一概率，所以在选择链表元素个数时， 把长度 `8` 作为转化的默认转红黑树的阈值。

# 用可变类当 HashMap 的 key 有什么问题?
hashcode 可能发生改变，导致 put 进去的值，无法 get 出。

# 扩容
HashMap使用的是懒加载，构造完HashMap对象后，只要不进行put 方法插入元素，HashMap并不会去初始化或者扩容table。

首次调用put方法时，HashMap如果发现table为空然后调用`resize`方法进行初始化。

JDK8做了两处优化：
1.  JDK7 中 rehash 的时候，旧链表迁移新链表的时候，如果在新表的数组索引位置相同，则链表元素会倒置（头插法）。JDK8 不会倒置，使用`尾插法`。
2.  不需要像JDK7的实现那样重新计算hash，只需要看看原来的hash值新增的那个bit是1还是0就好了，是0的话索引没变，是1的话索引变成“原索引+oldCap”。

由于hashmap的索引是根据哈希值的后N位决定的，由于初始数组容量是16（2^4 = 16），所以是哈希码后4位决定索引位置（即当前元素要存到数组的哪个位置）。

而扩容是以2的次方扩容的（每次乘以2的次方倍），所以相当于n-1高位+1（这里是第5位0 →1），同时索引值变成哈希值后N+1位（这里是第一次扩容，所以是后5位）。

这样原先的哈希码仍然可以用作索引值。

# HashMap，HashTable，ConcurrentHash的共同点和区别
### HashMap

底层由链表+数组+红黑树实现
可以存储null键和null值
线性不安全
初始容量为16，扩容每次都是2的n次幂
加载因子为0.75，当Map中元素总数超过Entry数组的0.75，触发扩容操作.
并发情况下，HashMap进行put操作会引起死循环，导致CPU利用率接近100%
HashMap是对Map接口的实现
### HashTable

HashTable的底层也是由链表+数组+红黑树实现。
无论key还是value都不能为null
它是线性安全的，使用了synchronized关键字。
HashTable实现了Map接口和Dictionary抽象类
Hashtable初始容量为11
性能比较低
### ConcurrentHashMap

ConcurrentHashMap的底层是数组+链表+红黑树
不能存储null键和值
ConcurrentHashMap是线程安全的
ConcurrentHashMap使用锁分段技术确保线性安全
JDK8为何又放弃分段锁，是因为多个分段锁浪费内存空间，竞争同一个锁的概率非常小，分段锁反而会造成效率低。


# 为什么推荐用SparseArray代替HashMap？

并不能替换所有的HashMap。只能替换以int类型为key的HashMap。  
HashMap中如果以int为key，会强制使用Integer这个包装类型，当我们使用int类型作为key的时候系统会自用装箱成为Integer，这个过程会创建对象影响效率。  
SparseArray内部是一个int数组和一个object数组。可以直接使用int减少了自动装箱操作。

