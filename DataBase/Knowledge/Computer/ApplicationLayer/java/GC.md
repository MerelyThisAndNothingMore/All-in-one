---
tags: 
alias:
---

## GC Roots
虚拟机栈帧(栈桢中的本地变量表)引用的对象、
类静态属性引用的对象、
常量引用的对象、
Native方法引用的对象 
对象若无 GC Roots引用，则表示可以被回收。



软引用SoftRef内存不足时回收，弱引用WeakRef发生gc时(内存即便充足)回收。

  