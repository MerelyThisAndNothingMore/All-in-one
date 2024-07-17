---
tags: 
alias:
---
https://mp.weixin.qq.com/s/b6-leHKQZkuxkjll-1109A

它是GUI管理系统与GUI呈现系统之间的桥梁。**每一个ViewRootImpl关联一个Window， ViewRootImpl 最终会通过它的setView方法绑定Window所对应的View，并通过其performTraversals方法对View进行布局、测量和绘制**。

[[ViewRootImpl|ViewRootImpl]] 是所有[[Android View]]的Parent，用来总体管理 View 的绘制以及与系统 [[WMS]] 窗口管理服务的 IPC 交互从而实现窗口的开辟；

ViewRootImpl 是应用进程运转的发动机，可以看到 ViewRootImpl 内部包含 mView（就是 [[DecorView]]）、mSurface、Choregrapher，

mView 代表整个控件树，mSurfacce 代表画布，应用的 UI 渲染会直接放到 mSurface 中，Choregorapher 使得应用请求 vsync 信号，接收信号后开始渲染流程；

# setView()分析
1. requestLayout () 通过一系列调用触发界面绘制（measure、layout、draw）动作
    
2. 通过 Binder 调用访问系统窗口管理服务 WMS 的 addWindow 接口，实现添加、注册应用窗口的操作，并传入本地创建 inputChannel 对象用于后续接收系统的触控事件，这一步执行完我们的 View 就可以显示到屏幕上了。
    
3. 创建 WindowInputEventReceiver 对象，封装实现应用窗口接收系统触控事件的逻辑；
    
4. 执行 view.assignParent (this)，设置 DecorView 的 mParent 为 ViewRootImpl。所以，虽然 ViewRootImpl 不是一个 View, 但它是所有 View 的顶层 Parent。

1. 从界面 View 控件树的根节点 DecorView 出发，递归遍历整个 View 控件树，完成对整个 View 控件树的 measure 测量操作，由于篇幅所限，本文就不展开分析这块的详细流程；
    
2. 界面第一次执行绘制任务时，会通过 Binder IPC 访问系统窗口管理服务 WMS 的 relayout 接口，实现窗口尺寸的计算并向系统申请用于本地绘制渲染的 Surface “画布” 的操作（具体由 SurfaceFlinger 负责创建应用界面对应的 BufferQueueLayer 对象，并通过内存共享的方式通过 Binder 将地址引用透过 WMS 回传给应用进程这边），由于篇幅所限，本文就不展开分析这块的详细流程；
    
3. 从界面 View 控件树的根节点 DecorView 出发，递归遍历整个 View 控件树，完成对整个 View 控件树的 layout 测量操作；
    
4. 从界面 View 控件树的根节点 DecorView 出发，递归遍历整个 View 控件树，完成对整个 View 控件树的 draw 测量操作，如果开启并支持硬件绘制加速（从 Android 4.X 开始谷歌已经默认开启硬件加速），则走 GPU 硬件绘制的流程，否则走 CPU 软件绘制的流程；



