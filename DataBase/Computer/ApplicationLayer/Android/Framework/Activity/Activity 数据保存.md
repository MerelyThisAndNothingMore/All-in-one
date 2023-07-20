---
tags: 
alias:
---
![](https://gd-hbimg.huaban.com/a601ee12826c71495877c95cb3a077b214e54bc626328-vKCmPf) 
当Activity变得不可见时（`onSaveInstanceState`和`onStop`回调之后），在应用进程这边会通过ActivityTaskManagerService的`activityStopped`方法，把刚刚在`onSaveInstanceState`中满载了数据的Bundle对象，传到系统服务进程那边！ 然后（在系统服务进程这边），会进一步将这个Bundle对象，赋值到对应ActivityRecord的`icicle`上


