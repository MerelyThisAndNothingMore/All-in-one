---
tags: 
alias:
- 垂直同步
- Vertical Synchronization
---
它利用VBI时期出现的vertical sync pulse（垂直同步脉冲）来保证双缓冲在最佳时间点才进行交换。另外，交换是指各自的内存地址，可以认为该操作是瞬间完成。

**VSync同步使得CPU/GPU充分利用了16.6ms时间，减少jank。**

