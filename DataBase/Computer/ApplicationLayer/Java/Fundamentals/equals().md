---
tags: 
alias:
---

# 重写 equals 为什么一定要重写 hashCode？
如果只重写了 equals 方法，那么默认情况下，[[Set]]进行去重操作时，会先判断两个对象的 hashCode 是否相同，此时因为没有重写 hashCode 方法，所以会直接执行 Object 中的 hashCode 方法，而 Object 中的 hashCode 方法对比的是两个不同引用地址的对象，所以结果是 false，那么 equals 方法就不用执行了，直接返回的结果就是 false：两个对象不是相等的，于是就在 Set 集合中插入了两个相同的对象。
但是，如果在重写 equals 方法时，也重写了 hashCode 方法，那么在执行判断时会去执行重写的 hashCode 方法，此时对比的是两个对象的所有属性的 hashCode 是否相同，于是调用 hashCode 返回的结果就是 true，再去调用 equals 方法，发现两个对象确实是相等的，于是就返回 true 了，因此 Set 集合就不会存储两个一模一样的数据了，于是整个程序的执行就正常了。
这是因为在使用集合类时，集合类内部会使用哈希码来对元素进行快速定位和比较。如果两个相等的对象具有不同的哈希码，则集合类可能会将它们视为不同的元素，这可能会导致一些意外的行为。






