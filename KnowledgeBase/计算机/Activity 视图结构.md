---
tags: 
alias:
---
![](https://gd-hbimg.huaban.com/2121f8986755dfc78a61b39d56e5ae23272600781ec2-0ECymJ)
`Activity`中，实际承载视图的组件是[[Android Window]]（更具体来说为`PhoneWindow`），顶层View 是`DecorView`，它是一个`FrameLayout`，`DecorView`内部是一个`LinearLayout`，该`LinearLayout`由两部分组成（不同 Android 版本或主题稍有差异）：`TitleView`和`ContentView`，其中，`TitleView`就是标题栏，也就是我们常说的`TitleBar`或`ActionBar`，`ContentView`就是内容栏，它也是一个`FrameLayout`，主要用于承载我们的自定义根布局，即当我们调用`setContentView(...)`时，其实就是把我们自定义的布局设置到该`ContentView`中。

