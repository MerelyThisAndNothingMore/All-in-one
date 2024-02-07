---
tags: 
alias:
---

主要用于确定 [[View|View]] w 在父容器中的放置位置。
先通过 [[Measure]] 测量出 [[ViewGroup]] 宽高，ViewGroup 再通过 layout 方法根据自身宽高来确定自身位置。当 ViewGroup 的位置被确定后，就开始在 onLayout 方法中调用子元素的 layout 方法确定子元素的位置。子元素如果是 ViewGroup 的子类，又开始执行 onLayout，如此循环往复，直到所有子元素的位置都被确定，整个 View 树的 layout 过程就执行完了。
