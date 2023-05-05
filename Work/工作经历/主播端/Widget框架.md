---
tags: 
alias:
---
> 好的设计是将实现做成配置。

Widget是一种带生命周期的功能模块。
[[主播直播间]]整体包含两层Fragment：播放层(LivePlayFragment)和交互层(InteractiveFragment)。播放层来展示视频流，交互层处理业务逻辑。
为了保障解耦，将交互层业务逻辑抽象为更小的单元：Widget。
# 设计
## 视图
由WidgetManager负责将widget视图加载到view树中。同步或异步。

