---
tags: 
alias:
---
# 介绍
视图承载器，是一个视图的顶层窗口， 包含了View并对View进行管理, 是一个抽象类，具体的实现类为 PhoneWindow, 在PhoneWindow中有一个顶级View—DecorView，继承自FrameLayout，我们可以通过getDecorView()获得它，当我们调用Activity的setContentView时，其实最终会调用Window的setContentView，当我们调用Activity的findViewById时，其实最终调用的是Window的findViewById，这也间接的说明了Window是View的直接管理者。

但是Window并不是真实存在的，它更多的表示一种抽象的功能集合，View才是Android中的视图呈现形式，绘制到屏幕上的是View不是Window，但是View不能单独存在，它必需依附在Window这个抽象的概念上面，Android中需要依赖Window提供视图的有Activity，Dialog，Toast，PopupWindow，StatusBarWindow（系统状态栏），输入法窗口等，因此Activity，Dialog等视图都对应着一个Window。
## 创建
在Activity的attach方法中，系统会**创建Activity所属的Window对象并设置其回调接口**。
通过WindowManager创建，并通过WindowManger将DecorView添加进 来。
## 分类
1.  应用窗口（拥有自己的WindowToken） 例如：Activity与Dialog
2.  子窗口（必须依附到其他非子窗口才能存在，比如Activity等） 例如：PopupWindow
3.  系统窗口 例如：Toast

# DecorView什么时候被WindowManager添加到Window中
即使Activity的布局已经成功添加到DecorView中，DecorView此时还没有添加到Window中 
ActivityThread的 handleResumeActivity方法中，首先会调用Activity的onResume方法，接着调用Activity的makeVisible()方法 
makeVisible()中完成了DecorView的添加和显示两个过程
