---
tags: 
alias:
---

# 什么是 https
简单来说， https 是 http + ssl，对 http 通信内容进行加密，是HTTP的安全版，是使用TLS/SSL加密的HTTP协议

Https的作用：
1. 内容加密 建立一个信息安全通道，来保证数据传输的安全；
2. 身份认证 确认网站的真实性
3. 数据完整性 防止内容被第三方冒充或者篡改

# https 的连接过程
![](https://s2.51cto.com/images/blog/202106/25/0742eed7fe33614cddb111dc4ec72f45.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp)

# [[HTTPS|HTTPS]] 可以被抓包吗？
HTTPS 的数据是加密的，常规下抓包工具代理请求后抓到的包内容是加密状态，无法直接查看。

但是，我们可以通过抓包工具来抓包。它的原理其实是模拟一个中间人。

通常 HTTPS 抓包工具的使用方法是会生成一个证书，用户需要手动把证书安装到客户端中，然后终端发起的所有请求通过该证书完成与抓包工具的交互，然后抓包工具再转发请求到服务器，最后把服务器返回的结果在控制台输出后再返回给终端，从而完成整个请求的闭环。

