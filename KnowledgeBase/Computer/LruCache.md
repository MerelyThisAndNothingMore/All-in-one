---
tags: 
alias:
---

androidx.collection.LruCache初始化的时候, 会限制缓存占据内存空间的总容量maxSize;  
# 原理
底层维护的是 LinkedHashMap, 使用 LruCache 最好要重写 sizeOf 方法, 用于计算每个被缓存的对象, 在内存中存储时, 占用多少空间;
## 结点添加
在 put 操作时, 首先计算新的缓存对象, 占多少空间, 再根据key, 移除老的对象,占用内存大小 = 之前占用的内存大小 + 新对象的大小 - 老对象的大小;  

put 操作最后总会根据 maxSize, 在拿到LinkedHashMap.EntrySet 中链表的头节点, 循环判断, 只要当前缓存对象占据内存超出 maxSize, 就移除一个头节点, 一直到符合要求;

## lruCache 和 LinkedHashMap 的关系:
LinkedHashMap 中维护一个双向链表, 并维护 head 和tail 指针, lruCache 使用了 LinkedHashMap 

accessOrder为 true 的属性, 只要访问了某个 key,包括 get 和 put, 就把当前这个 entry 放在链表的尾节点,所以链表的头节点, 是最老访问的节点, 尾节点是最新访问的节点,  

所以, lruCache 就很巧妙的利用了这个特点, 完成了 LeastRecently Used 的需求;
