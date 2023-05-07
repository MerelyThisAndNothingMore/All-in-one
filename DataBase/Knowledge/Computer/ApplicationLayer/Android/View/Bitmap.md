---
tags: 
alias:
---
# Bitmap 内存占用的计算
占用的内存大小 = 像素总数量(图片宽x高)x 每个像素的字节大小
# getByteCount() & getAllocationByteCount()的区别
如果被复用的Bitmap的内存比待分配内存的Bitmap大 getByteCount()获取到的是当前图片应当所占内存大小 getAllocationByteCount()获取到的是被复用Bitmap真实占用内存大小 在复用Bitmap的情况下，getAllocationByteCount()可能会比getByteCount()大。


# 缓存
## LruCache & DiskLruCache原理
### LRU(Least Recently Used)

LruCache的核心思想就是维护一个缓存对象列表，从表尾访问数据，在表头删除数据。对象列表的排列方式是 按照访问顺序实现，就是 当访问的数据项在链表中存在时，则将该数据项移动到表尾，否则在表尾新建一个数据 项。当链表容量超过一定阈值，则移除表头的数据。

利用LinkedHashMap数组+双向链表的数据结构来实现的。其中双向链表的结构可以实现访问顺序和插入顺序， 使得LinkedHashMap中的accessOrder设置为true则为访问顺序，为false，则为插入顺序。
写入缓存

1插入元素，并相应增加当前缓存的容量。

2调用trimToSize()开启一个死循环，不断的从表头删除元素，直到当前缓存的容量小于最大容量为止。

读取缓存

调用LinkedHashMap的get()方法，注意如果该元素存在，这个方法会将该元素移动到表尾.

