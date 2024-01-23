---
tags: 
alias:
---
1.  Service TimeOut: service 未在规定时间执行完成：前台服务 20s，后台 200s
2.  BroadCastQueue TimeOut: 未在规定时间内未处理完广播：前台广播 10s 内, 后台 60s 内
3.  ContentProvider TimeOut: publish 在 10s 内没有完成
4.  Input Dispatching timeout: 5s 内未响应键盘输入、触摸屏幕等事件

我们可以看到， Activity 的生命周期回调的阻塞并不在触发 ANR 的场景里面，所以并不会直接触发 ANR。 只不过死循环阻塞了主线程，如果系统再有上述的四种事件发生，就无法在相应的时间内处理从而触发 ANR。