---
tags: 
alias:
---
# 特性
scheme是一种页面内跳转协议，是一种非常好的实现机制，通过定义自己的scheme协议，可以非常方便跳转 app中的各个页面
APP根据URL跳转到另外一个APP指定页面： 
- 可以通过h5页面跳转app原生页面 
- 服务器可以定制化跳转app页面
# 定义
```
scheme://host/path?query

hr://test:8080/goods?goodsId=8897&name=test

```
hr代表Scheme协议名称
test代表Scheme作用的地址域 
8080代表改路径的端口号 
/goods代表的是指定页面(路径) 
goodsId和name代表传递的两个参数
