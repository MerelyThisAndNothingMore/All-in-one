---
tags: 
alias:
---
对于单个Activity嵌套多个Fragment的UI架构方式， 对Fragment的管理一直是一个比较麻烦的事情。

需要通过FragmentManager和FragmentTransaction来管理Fragment之间的切换 对应用程序的App bar的管 理， Fragment间的切换动画 Fragment间的参数传递 总之，使用起来不是特别友好。

Navigation是用来管理Fragment的切换，并且可以通过可视化的方式，看见App的交互流程

# why
可以可视化的编辑各个组件之间的跳转关系

优雅的支持fragment之间的转场动画 通过第三方的插件支持fragment之间安全的参数传递

通过NavigationUI类，对菜单，底部导航，抽屉菜单导航进行方便统一的管理 支持通过deeplink直接定位到fragment


