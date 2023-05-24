---
tags: 
alias:
---
https://www.jianshu.com/p/37370c1d17fc


`Android` 系统是由事件驱动的，而 `input` 是最常见的事件之一，用户的点击、滑动、长按等操作，都属于 `input` 事件驱动，其中的核心就是 `InputReader` 和 `InputDispatcher`。`InputReader` 和 `InputDispatcher` 是跑在 `SystemServer`进程中的两个 `native` 循环线程，负责读取和分发 `Input` 事件。

