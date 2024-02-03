---
tags: 
alias:
---

对象优先在Eden分配
大对象直接进入老年代
长期存活的对象将进入老年代
动态对象年龄判定
HotSpot虚拟机并不是永远要求对象的年龄必须达到- XX:M axTenuringThreshold才能晋升老年代，如果在Survivor空间中相同年龄所有对象大小的总和大于 Survivor空间的一半，年龄大于或等于该年龄的对象就可以直接进入老年代，无须等到-XX:  
M axTenuringThreshold中要求的年龄。
