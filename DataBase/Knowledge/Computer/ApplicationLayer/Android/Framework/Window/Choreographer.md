---
tags: 
alias:
---
https://blog.csdn.net/xfhy_/article/details/115436765?spm=1001.2014.3001.5502

[[View 绘制流程]]的开始依赖于Choreographer，翻译过来这个单词的意思是“编舞者”。


Choreographer 的引入，主要是配合系统 Vsync 垂直同步机制（Android “黄油计划” 中引入的机制之一，协调 APP 生成 UI 数据和 SurfaceFlinger 合成图像，避免 Tearing 画面撕裂的现象），给上层 App 的渲染提供一个稳定的 Message 处理的时机，也就是 Vsync 到来的时候 ，系统通过对 Vsync 信号周期的调整，来控制每一帧绘制操作的时机。Choreographer 扮演 Android 渲染链路中承上启下的角色：

1. 承上：负责接收和处理 App 的各种更新消息和回调，等到 Vsync 到来的时候统一处理。比如集中处理 Input (主要是 Input 事件的处理) 、Animation (动画相关)、Traversal (包括 measure、layout、draw 等操作) ，判断卡顿掉帧情况，记录 CallBack 耗时等；
    
2. 启下：负责请求和接收 Vsync 信号。接收 Vsync 事件回调 (通过 FrameDisplayEventReceiver.onVsync)，请求 Vsync (FrameDisplayEventReceiver.scheduleVsync) 。

[[ViewRootImpl]] 调用 [[Choreographer]] 的 postCallback 接口放入待执行的绘制消息后，Choreographer 会先向系统申请 APP 类型的 [[vsync]] 信号，然后等待系统 vsync 信号到来后，去回调到 ViewRootImpl 的 doTraversal 函数中执行真正的绘制动作（measure、layout、draw）。