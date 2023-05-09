---
tags: 
alias:
---


![](https://img-blog.csdnimg.cn/img_convert/a6a78febe892f54380eb6bd51be9fb2b.png)
# 数据结构
`HashMap` 采用的数据结构 = **数组（主） + 单链表（副）**，具体描述如下
> 该数据结构方式也称：拉链法

![](https://img-blog.csdnimg.cn/img_convert/adba8a54613a8c4d7d1df27d9825e4cb.png)

![](https://img-blog.csdnimg.cn/img_convert/7915b5e63422b0248b22094a6a8aea1a.png)

## 存储流程
![](https://img-blog.csdnimg.cn/img_convert/02e9973d443d39bd9fb5b87001b0c6c6.png)
## 数组元素 & 链表节点的 实现类
![](https://img-blog.csdnimg.cn/img_convert/a7f49fc1aa0ab30abfe4ff420477760c.png)
1.  即 `HashMap`的本质 = 1个存储`Entry`类对象的数组 + 多个单链表
2.  `Entry`对象本质 = 1个映射（键 - 值对），属性包括：键（`key`）、值（`value`） & 下1节点( `next`) = 单链表的指针 = 也是一个`Entry`对象，用于解决`hash`冲突

# 基础知识：HashMap中的重要参数（变量）
-   在进行真正的源码分析前，先讲解`HashMap`中的重要参数（变量）
-   `HashMap`中的主要参数 = 容量、加载因子、扩容阈值
## 容量
容量范围：必须是2的幂 & < 最大容量（2^30）
初始容量：1 << 4
HashMap根据用户传入的初始化容量，利用无符号右移和按位或运算等方式计算出第一个大于该数的2的幂。

-   使数据分布均匀，减少碰撞
-   当length为2的n次方时，`h & (length-1) = h % length`，因为 位运算直接对内存数据进行操作，不需要转成十进制，在速度、效率上比直接取模要快得多

## 加载因子
![](https://img-blog.csdnimg.cn/img_convert/3d44f6c6c96c65d6e986ed96f70136fd.png)

0.75
加载因子越大，空间利用率高，但冲突的机会大

## 扩容阈值
容量 * 加载因子
# 源码分析
## 步骤1：声明1个 HashMap的对象
1.  此处仅用于接收初始容量大小（`capacity`）、加载因子(`Load factor`)，但仍无真正初始化哈希表，即初始化存储数组`table`
2.  此处先给出结论：**真正初始化哈希表（初始化存储数组`table`）是在第1次添加键值对时，即第1次调用`put（）`时。下面会详细说明**
## 步骤2：向HashMap添加数据（成对 放入 键 - 值对）
![](https://img-blog.csdnimg.cn/img_convert/234b2276535de2d7b4c3679a38e74d52.png)

### 分析1：初始化哈希表
### 分析2：当 key \=\= null时，将该 key-value 的存储位置规定为数组table 中的第1个位置，即table [0]
### 分析3：计算存放数组 table 中的位置（即 数组下标 or 索引）
![](https://img-blog.csdnimg.cn/img_convert/291a43fb80e636d1e6fbf628b31525c0.png)
### 分析4：若对应的key已存在，则 使用 新value 替换 旧value
注：当发生 `Hash`冲突时，为了保证 键`key`的唯一性哈希表并不会马上在链表中插入新数据，而是先查找该 `key`是否已存在，若已存在，则替换即可
#### 分析1：替换流程

#### 分析2：`key`值的比较

采用 `equals（）` 或 “\=\=” 进行比较，下面给出其介绍 & 与 `“==”`使用的对比
![](https://img-blog.csdnimg.cn/img_convert/0f298d8627c654a50a66652e1019ecc6.png)
### 分析5：若对应的key不存在，则将该“key-value”添加到数组table的对应位置中
#### 1.键值对的添加方式：单链表的头插法
#### 2.扩容机制
扩容resize（）过程中，在将旧数组上的数据转移到新数组上时，转移操作 = 按旧链表的正序遍历链表、
在新链表的头部依次插入，

即在转移数据、扩容后，容易出现链表逆序的情况

设重新计算存储位置后不变，即扩容前 = 1->2->3，扩容后 = 3->2->1

此时若（多线程）并发执行 put（）操作，一旦出现扩容情况，则 容易出现 环形链表，从而在获取数据、遍历链表时 形成死循环（Infinite Loop），即 死锁的状态 = 线程不安全
# 与 `JDK 1.8`的区别
`HashMap` 的实现在 `JDK 1.7` 和 `JDK 1.8` 差别较大，具体区别如下
`JDK 1.8` 的优化目的主要是：减少 `Hash`冲突 & 提高哈希表的存、取效率；
链表插入元素时，JDK7 使用`头插法`插入元素，但在多线程的环境下， `扩容`时会造成`环形链或数据丢失`，出现死循环。

-   `数组+链表`改成了`数组+链表+红黑树`
-   链表的插入方式从`头插法`改成了`尾插法`
-   扩容的时候7需要对原数组中的元素进行重新hash定位在新数组的位置，8采用更简单的判断逻辑，位置不变或索引+旧容量大小；
-   **在插入时，7先判断是否需要扩容，再插入；而 8先进行插入，插入完成再判断是否需要扩容；**


## 数据结构
1.  > 解决哈希冲突时，JDK7 只使用链表，JDK8 使用链表+红黑树，当满足一定条件，链表会转换为红黑树。
![](https://img-blog.csdnimg.cn/img_convert/bd8f3dac8d659d6ac3f7007051d5b34a.png)
## 获取数据时（获取数据 类似）
![](https://img-blog.csdnimg.cn/img_convert/1d9fb93f9fa20560ed4df61b3452f081.png)











