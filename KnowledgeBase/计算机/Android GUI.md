---
tags: 
alias:
---
在一个典型的显示系统中，一般包括CPU、GPU、Display三个部分， CPU负责计算帧数据，把计算好的数据交给GPU，GPU会对图形数据进行渲染，渲染好后放到buffer(图像缓冲区)里存起来，然后Display（屏幕或显示器）负责把buffer里的数据呈现到屏幕上。





![](https://gd-hbimg.huaban.com/756305c2f6766259704f1d881a5983cc0399ea76129ac-vQAnhL_fw1200webp)

# 角色分析
## [[Android Window]]
[[Android Window]]是一个抽象类，通过控制 [[DecorView]] 提供了一些标准的 UI 方案，比如背景、标题、虚拟按键等，而 PhoneWindow 是 Window 的唯一实现类，在 Activity 创建后的 attach 流程中创建，应用启动显示的内容装载到其内部的 mDecor（DecorView）；

## [[DecorView]]
DecorView 是整个界面布局 View 控件树的根节点，通过它可以遍历访问到整个 View 控件树上的任意节点；

## [[Window Manager]]
WindowManager 是一个接口，继承自 ViewManager 接口，提供了 View 的基本操作方法；WindowManagerImp 实现了 WindowManager 接口，内部通过组合方式持有 WindowManagerGlobal，用来操作 View；WindowManagerGlobal 是一个全局单例，内部可以通过 ViewRootImpl 将 View 添加至窗口中；

## [[ViewRootImpl]]

ViewRootImpl 是所有 View 的 Parent，用来总体管理 View 的绘制以及与系统 WMS 窗口管理服务的 IPC 交互从而实现窗口的开辟；ViewRootImpl 是应用进程运转的发动机，可以看到 ViewRootImpl 内部包含 mView（就是 DecorView）、mSurface、Choregrapher，mView 代表整个控件树，mSurfacce 代表画布，应用的 UI 渲染会直接放到 mSurface 中，Choregorapher 使得应用请求 vsync 信号，接收信号后开始渲染流程；


