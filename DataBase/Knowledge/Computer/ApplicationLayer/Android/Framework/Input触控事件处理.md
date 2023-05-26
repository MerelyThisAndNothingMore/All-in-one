---
tags: 
alias:
---
# 定义
`Android` 系统是由事件驱动的，而 `input` 是最常见的事件之一，用户的点击、滑动、长按等操作，都属于 `input` 事件驱动，其中的核心就是 `InputReader` 和 `InputDispatcher`。

`InputReader` 和 `InputDispatcher` 是跑在 `SystemServer`进程中的两个 `native` 循环线程，负责读取和分发 `Input` 事件。

1. InputReader 负责从 EventHub 里面把 Input 事件读取出来，然后交给 InputDispatcher 进行事件分发；
    
2. InputDispatcher 在拿到 InputReader 获取的事件之后，对事件进行包装后，寻找并分发到目标窗口；
   
3. App 响应处理 Input 事件，内部会在其界面 View 树中传递处理。

# 流程分析
## 分发到UI
从桌面点击应用图标启动应用，[[system_server进程]] 的 native 线程 InputReader 首先负责从 EventHub 中利用 linux 的 epolle 机制监听并从屏幕驱动读取上报的触控事件，然后唤醒另外一条 native 线程 InputDispatcher 负责进行进一步事件分发。

InputDispatcher 中会先将事件放到 InboundQueue 也就是 “iq” 队列中，然后寻找具体处理 input 事件的目标应用窗口，并将事件放入对应的目标窗口 OutboundQueue 也就是 “oq” 队列中，等待通过 SocketPair 双工信道发送到应用目标窗口中。

最后当事件发送给具体的应用目标窗口后，会将事件移动到 WaitQueue 也就是 “wq” 中等待目标应用处理事件完成，并开启倒计时，如果目标应用窗口在 5S 内没有处理完成此次触控事件，就会向 system_server 报应用 ANR 异常事件。
## UI消费
当 input 触控事件传递到桌面应用进程后，Input 事件到来后先通过 enqueueInputEvent 函数放入 “aq” 本地待处理队列中，并唤醒应用的 [[UI线程]]在 deliverInputEvent 的流程中进行 input 事件的具体分发与处理。

具体会先交给在应用界面 Window 创建时的 ViewRootImpl#setView 流程中创建的多个不同类型的 InputUsage 中依次进行处理（比如对输入法处理逻辑的封装 ImeInputUsage），整个处理流程是按照责任链的设计模式进行。

最后会交给 ViewpostImeInputUsage 中具体进行处理，这里面会从 View 布局树的根节点 DecorView 开始遍历整个 View 树上的每一个子 View 或 ViewGroup 界面进行事件的分发、拦截、处理的逻辑。

最后触控事件处理完成后会调用 finishInputEvent 结束应用对触控事件处理逻辑，这里面会通过 JNI 调用到 native 层 InputConsumer 的 sendFinishedSignal 函数通知 InputDispatcher 事件处理完成，从触发从 "wq" 队列中及时移除待处理事件以免报 ANR 异常。

