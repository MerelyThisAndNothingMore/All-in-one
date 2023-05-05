---
tags: 
alias:
---
> 好的设计是将实现做成配置。

Widget是一种带生命周期的功能模块，可以理解为轻量级的Fragment，承载view展示过程，同时继承了LifecycleOwner，可以感知和分发生命周期。
[[主播直播间]]整体包含两层Fragment：播放层(LivePlayFragment)和交互层(InteractiveFragment)。播放层来展示视频流，交互层处理业务逻辑。
为了保障解耦，将交互层业务逻辑抽象为更小的单元：Widget。
# 设计
## 视图
由WidgetManager负责将widget视图加载到view树中。同步或异步。并将自己的生命周期分发给widget。
> 异步加载，创建一个子线程将加载任务添加到阻塞队列中，保证异步加载顺序




