---
tags: 
alias:
---

1 非静态内部类默示持有外部类的引用，如非静态handler持有activity的引用

2.接收器、监听器注册没取消造成的内存泄漏，如广播，eventsbus

3.Activity 的 Context 造成的泄漏，可以使用 ApplicationContext

4.单例中的static成员间接或直接持有了activity的引用

5.资源对象没关闭造成的内存泄漏(如: Cursor、File等)

6.全局集合类强引用没清理造成的内存泄漏(特别是 static 修饰的集合)