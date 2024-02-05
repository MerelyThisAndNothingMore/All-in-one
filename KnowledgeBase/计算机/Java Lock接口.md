---
tags: 
alias:
---

自JDK 5起(实现了JSR 166)，[[Java]]类库中新提供了java.util.concurrent包(下文称J.U.C包)，其中的java.util.concurrent.locks.Lock接口便成了Java的另一种全新的[[互斥同步]]手段。

基于Lock接口，用户能够以非块结构(Non-Block Structured)来实现互斥同步，从而摆脱了语言特性的束缚，改为在类库层面 去实现同步，这也为日后扩展出不同调度算法、不同特征、不同性能、不同语义的各种锁提供了广阔的空间。

