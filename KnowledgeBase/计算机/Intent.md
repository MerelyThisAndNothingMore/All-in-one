---
tags: 
alias:
---
# 介绍
[[Activity]]、[[Service]]、[[BroadcastReceiver]]之间的通信载体 = `Intent`
# 能力
## 指定当前组件要完成的动作

[[Activity 启动方法]]
## 不同组件间 传递数据
### 使用方法
putExtra（）、Bundle方式

### 可传递的数据类型
a. 8种基本数据类型（boolean byte char short int long float double）、String
b. Intent、Bundle
c. Serializable对象、Parcelable及其对应数组、CharSequence 类型
d. ArrayList，泛型参数类型为：<Integer>、<\? Extends Parcelable>、<Charsequence>、<String>

### 区别
Bundle 意为 捆绑 的意思，更多适用于：

连续传递数据
若需实现连续传递：Activity A -> B -> C；若使用putExtra（），则需写两次intent = A->B先写一遍 + 在B中取出来 & 再把值重新写到Intent中再跳到C；若使用 Bundle，则只需取出 & 传入 Bundle对象即可

可传递的值：对象
putExtra（）无法传递对象，而 Bundle则可通过 putSerializable传递对象

而 `putExtra（）`更多使用于单次传递、传递简单数据类型的应用场景



https://blog.csdn.net/carson_ho/article/details/51762131

