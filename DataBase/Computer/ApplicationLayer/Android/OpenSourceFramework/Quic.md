---
tags: 
alias:
- Quick Udp Internet Connection
---

# 是什么
QUIC最重要的特点是使用[[UDP]]，且基于[[TLS1.3]]，前者保证了速度，后者保证了安全性。

QUIC多路复用的实现方案是：同域名请求共用一条UDP通道，这样不仅省去了TCP连接，也没有Tcp的重传机制，在一条QUIC连接上，多个请求之间是没有依赖的，一个请求的丢失不会影响到其他的请求。
# 为什么要引入
尽管TCP在可靠传输方面有所成就，但它的三次握手、拥塞控制等特性导致了不可避免的延迟和队头阻塞问题。
QUIC旨在解决TCP的局限性，通过结合UDP的轻量和自身的一系列创新特性来减少延迟，提高连接效率。
# 是怎么做到的？

### 可靠传输
#### Packet Number
QUIC使用了 ​​Packet Number​​​代替了 TCP的​​Sequence Number​​，并且每个 Packet Number都是严格递增。就算 Packet N丢失了，重传该Packet的Number一定是个比N大的值。
#### 流量控制


