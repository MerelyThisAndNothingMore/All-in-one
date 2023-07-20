---
tags: 
alias:
- WindowManageService
---
##### Window 与 WindowManageService 通讯
![](https://upload-images.jianshu.io/upload_images/1785445-7124482a3e07937a.png?imageMogr2/auto-orient/strip|imageView2/2/w/692/format/webp)
#### 点击事件
WindowManageService 会接收所有的输入事件，比如 touch 事件，再通过 IWindow 通知 [[ViewRootImpl|ViewRootImpl]] 


