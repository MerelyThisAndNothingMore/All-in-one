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
# 算法题
## 典型应用1：寻找出现特定次数的数字

-   [数组中只出现1次的2个数字](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%95%B0%E7%BB%84%E4%B8%AD%E5%8F%AA%E5%87%BA%E7%8E%B0%E4%B8%80%E6%AC%A1%E7%9A%84%E4%B8%A4%E4%B8%AA%E6%95%B0%E5%AD%97%2856%29.md)
-   [数组中出现次数超过一半的数字](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%95%B0%E7%BB%84%E4%B8%AD%E5%87%BA%E7%8E%B0%E6%AC%A1%E6%95%B0%E8%B6%85%E8%BF%87%E4%B8%80%E5%8D%8A%E7%9A%84%E6%95%B0%E5%AD%97%EF%BC%8839%EF%BC%89.md)
-   [统计 数字在排序数组中出现的次数：二分法](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E7%BB%9F%E8%AE%A1%20%E6%95%B0%E5%AD%97%E5%9C%A8%E6%8E%92%E5%BA%8F%E6%95%B0%E7%BB%84%E4%B8%AD%E5%87%BA%E7%8E%B0%E7%9A%84%E6%AC%A1%E6%95%B0%EF%BC%8853%EF%BC%89.md)
-   [数组中唯一出现1次的数字、其他都出现了3次](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%95%B0%E7%BB%84%E4%B8%AD%E5%94%AF%E4%B8%80%E5%87%BA%E7%8E%B0%E4%B8%80%E6%AC%A1%E7%9A%84%E6%95%B0%E5%AD%97%EF%BC%8856-1%EF%BC%89.md)


## 典型应用2：寻找符合特定条件的数字

-   [数组中数值与下标相等的元素](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%95%B0%E7%BB%84%E4%B8%AD%E6%95%B0%E5%80%BC%E4%B8%8E%E4%B8%8B%E6%A0%87%E7%9B%B8%E7%AD%89%E7%9A%84%E5%85%83%E7%B4%A0%EF%BC%8853-3%EF%BC%89.md)
-   [获取数组中最小的k个数](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%9C%80%E5%B0%8F%E7%9A%84k%E4%B8%AA%E6%95%B0%EF%BC%8840%EF%BC%89.md)
-   [排序数组中，0~n-1中缺失的数字](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/0~n-1%E4%B8%AD%E7%BC%BA%E5%A4%B1%E7%9A%84%E6%95%B0%E5%AD%97%EF%BC%8853-2%EF%BC%89.md)
-   [打印从1到最大的n位数：大数问题](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%89%93%E5%8D%B0%E4%BB%8E1%E5%88%B0%E6%9C%80%E5%A4%A7%E7%9A%84n%E4%BD%8D%E6%95%B0%EF%BC%8817%EF%BC%89.md)
-   [数组中重复的数字（可修改 & 不可修改数组）](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%95%B0%E7%BB%84%E4%B8%AD%E9%87%8D%E5%A4%8D%E7%9A%84%E6%95%B0%E5%AD%97%EF%BC%88%E5%8F%AF%E4%BF%AE%E6%94%B9%20%26%20%E4%B8%8D%E5%8F%AF%E4%BF%AE%E6%94%B9%E6%95%B0%E7%BB%84%EF%BC%89%EF%BC%883%EF%BC%89.md)

## 典型应用3：不同类型数组的查找

-   [二维数组中的查找](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E4%BA%8C%E7%BB%B4%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E6%9F%A5%E6%89%BE%EF%BC%884%EF%BC%89.md)
-   [找出旋转数组的最小数字](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%89%BE%E5%87%BA%E6%97%8B%E8%BD%AC%E6%95%B0%E7%BB%84%E7%9A%84%E6%9C%80%E5%B0%8F%E6%95%B0%E5%AD%97%EF%BC%8811%EF%BC%89.md)

## 典型应用4：数组内元素的[排列组合](https://so.csdn.net/so/search?q=%E6%8E%92%E5%88%97%E7%BB%84%E5%90%88&spm=1001.2101.3001.7020)

-   [数组所有滑动窗口的最大值](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E7%9A%84%E6%9C%80%E5%A4%A7%E5%80%BC%EF%BC%8859%EF%BC%89.md)
-   [连续子数组的最大和](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E8%BF%9E%E7%BB%AD%E5%AD%90%E6%95%B0%E7%BB%84%E7%9A%84%E6%9C%80%E5%A4%A7%E5%92%8C%EF%BC%8842%EF%BC%89.md)
-   [把数组的所有数排成最小的数：大数问题](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%8A%8A%E6%95%B0%E7%BB%84%E7%9A%84%E6%95%B0%E5%AD%97%E6%8E%92%E6%88%90%E6%9C%80%E5%B0%8F%E6%95%B0%2845%29.md)
-   [数组中的逆序对](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E9%80%86%E5%BA%8F%E5%AF%B9%EF%BC%8851%EF%BC%89.md)
-   [调整数组顺序，使奇数位于偶数前面](https://github.com/Carson-Ho/AlgorithmLearning/blob/master/%E5%85%B7%E4%BD%93%E8%AE%B2%E8%A7%A3/%E8%B0%83%E6%95%B4%E6%95%B0%E7%BB%84%E9%A1%BA%E5%BA%8F%EF%BC%8C%E4%BD%BF%E5%A5%87%E6%95%B0%E4%BD%8D%E4%BA%8E%E5%81%B6%E6%95%B0%E5%89%8D%E9%9D%A2%EF%BC%8821%EF%BC%89.md)


