---
tags: 
alias:
---
# 原理

规定线程每次修改变量副本后立刻同步到主内存中，用于保证其它线程可以看到自己对变量的修改

规定线程每次使用变量前，先从主内存中刷新最新的值到工作内存，用于保证能看见其它线程对变量修改的最新值

为了实现可见性内存语义，编译器在生成字节码时，会在指令序列中插入内存屏障来防止指令重排序。









![](https://gd-hbimg.huaban.com/037a6f26772aac082a3abc806e76f4d3655359c36640-4ZTaWF_fw1200webp)

![](https://gd-hbimg.huaban.com/a0bf8fdd9ba4fe096b88c5b90174b3f3dee64cbabac8-NX2dTV)
