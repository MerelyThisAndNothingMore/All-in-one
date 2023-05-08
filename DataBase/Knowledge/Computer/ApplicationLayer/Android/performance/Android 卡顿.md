---
tags: 
alias:
---

![](https://gd-hbimg.huaban.com/ed85ec89ad65e9c60159c9c8b295c463d3cbfb1157d6-j9ZmnT)

# 分类
## UI卡顿
### 系统负载
CPU、GPU、内存、功耗、发热
### UI帧卡顿
### 阻塞帧卡顿
## UI、流相互影响
### UI引起的流卡顿
### 流引起的UI卡顿
# 优化方案

## UI布局优化
overdraw：指定绘制区域、选择合理的控件、去掉不必要的背景
inflate：异步Inflate、X2C
layer层级：合理的控件选择、Merge标签
按需更新：SeekBar更新
Litho：异步渲染
按需加载：ViewStub、动态添加
高性能组件：定制TextView、
## 动画刷新优化
绘制泄漏：不可见，取消动画
绘制频率：降低动画更新
绘制范围：ClipRect、Invalidate。
SurfaceView：复杂动画（弹幕）
## 主线程耗时消息优化
长耗时：按需加载、异步处理、IPC结果缓存

## 后台耗时任务优化

异步、打散、预热、复用、方案优化。

