---
tags:
- dataStructure
alias: 
- 数组
---
# Introduction 
A common programming task is to keep track of an ordered sequence of related values or objects.
数组是一种顺序存储的线性表，所有元素的**内存地址是连续**的

## Array Elements and Capacities
Each value stored in an array is called an element of that array. Since the length of an array determines the maximum number of things that can be stored in the array, we will sometimes refer to the length of an array as its capacity.
# Implementation 
```java
public interface IArrayList<E> {
	public int size();
	public boolean isEmpty();
	public int indexOf(E element);
	public boolean contains(E element);
	public void add(E element);
	public void add(int index, E element);
	public E get(int index);
	public E set(int index, E element);
	public E remove(int index);
	public void clear();
	public void ensureCapacity(int capacity);
}
```
