---
tags: 
alias:
---

Surface 对应了一块屏幕缓冲区，是要显示到屏幕的内容 的载体。每一个 Window 都对应了一个自己的 Surface 。这里说的 window 包括 Dialog, Activity, Status Bar 等。 SurfaceFlinger 最终会把这些 Surface 在 z 轴方向上以正确的方式绘制出来(比如 Dialog 在 Activity 之上)。 SurfaceView 的每个 Surface 都包含两个缓冲区，而其他普通 Window 的对应的 Surface 则不是。
