---
tags:
- computerArchitecture 
alias:
- 缓存
banner_y: 0
---
A system spends a lot of time moving information from one place to another. A major goal for system designers is to make these copy operations run as fast as possible.
根据机械原理，较大的存储设备要比较小的存储设备运行得慢，而快速设备的造价远高于同类的低速设备。一个典型的寄存器文件只存储几百字节的信息，而主存里可存放几十亿字节。然而，处理器从寄存器文件中读数据比从主存中读取几乎要快100 倍。
针对这种处理器与主存之间的差异，系统设计者采用了更小更快的存储设备，称为高速缓存存储器(cache memory, 简称为cache 或高速缓存），作为暂时的集结区域，存放处理器近期可能会需要的信息。
[[Memory Hierarchy]]






