---
tags:
  - AndroidJetPack
aliases:
---
# 定义
LiveData 是一种可观察的数据存储器类。与常规的可观察类不同，LiveData 具有生命周期感知能力，意指它遵循其他应用组件（如 Activity/Fragment）的生命周期。这种感知能力可确保 LiveData 仅更新处于活跃生命周期状态的应用组件观察者。

- LiveData是一个数据持有者，给源数据包装一层。
- 源数据使用LiveData包装后，可以被observer观察，数据有更新时observer可感知。
- 但 observer的感知，只发生在（Activity/Fragment）活跃生命周期状态（STARTED、RESUMED）。

## 特点
- **确保界面符合数据状态**，当生命周期状态变化时，LiveData通知Observer，可以在observer中更新界面。观察者可以在生命周期状态更改时刷新界面，而不是在每次数据变化时刷新界面。
- **不会发生内存泄漏**，observer会在LifecycleOwner状态变为DESTROYED后自动remove。
- **不会因 Activity 停止而导致崩溃**，如果LifecycleOwner生命周期处于非活跃状态，则它不会接收任何 LiveData事件。
- **不需要手动解除观察**，开发者不需要在onPause或onDestroy方法中解除对LiveData的观察，因为LiveData能感知生命周期状态变化，所以会自动管理所有这些操作。
- **数据始终保持最新状态**，数据更新时 若LifecycleOwner为非活跃状态，那么会在**变为活跃时接收最新数据**。例如，曾经在后台的 Activity 会在返回前台后，observer立即接收最新的数据。
# 使用
# 原理


