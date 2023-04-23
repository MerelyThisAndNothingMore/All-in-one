---
tags: 
alias:
---
# 基础
| 方法         | 描述     |  
| :------------- |:-------------|   
| onCreate()      | Activity首次创建时调用,用于初始化Activity     |   
| onStart()      | Activity变为可见时调用     |  
| onResume()      |Activity变为用户可交互状态时调用     |  
| onPause()      | Activity失去焦点、进入后台时调用,用于暂停操作     |  
| onStop()      | Activity完全不可见时调用     |   
| onRestart()      | Activity由停止状态变为可见状态时调用   |  
| onDestroy()     | Activity被销毁前调用,用于清理资源     |   
| onSaveInstanceState() | Activity被销毁前调用,用于保存状态     |  
| onRestoreInstanceState() | Activity重新创建时调用,用于恢复状态 |
## 调用原理
生命周期回调由[[AMS]]通过[[Binder]]通知应用进程调用。
对于 onSaveInstanceState ，由于[[Activity]]的状态是由ActivityManager进行管理的，因此这里会通过[[Binder]]将Bundle信息传输到ActivityManager进行管理。
系统会调用ActivityThread的performStopActivity方法中掉用onSaveInstanceState， 将状态保存在mActivities 中，mActivities维护了一个Activity的信息表，当Activity重启时候，会从mActivities中查询到对应的 ActivityClientRecord。
如果有信息，则调用Activity的onResoreInstanceState方法，
在ActivityThread的performLaunchActivity方法中，统会判断ActivityClientRecord对象的state是否为空
不为空则通过Activity的onSaveInstanceState获取其UI状态信息，通过这些信息传递给Activity的onCreate方 法，
# onSaveInstanceState的调用时机
onSaveInstanceState在生命周期中的回调顺序，取决于targetSdkVersion。
| targetSdkVersion | 顺序         |
|:---------------- |:------------ |
| <11              | onPause 之前 |
| <28              | onStop 之前   |
| >28              | onStop 之后             |
onSaveInstanceState(Bundle outState)会在以下情况被调用：
1、当用户按下HOME键时。
2、从最近应用中选择运行其他的程序时。
3、按下电源按键（关闭屏幕显示）时。
4、从当前activity启动一个新的activity时。
5、屏幕方向切换时(无论竖屏切横屏还是横屏切竖屏都会调用)。
>需要说明的是，非[[Activity]]的[[Dialog]]展现时不会调用到生命周期。

# onRestoreInstanceState的调用时机
只有在[[Activity]]确实被系统回收，重新创建时才会被调用：
- 屏幕切换
- 后台程序被kill
# ActivityA打开 ActivityB时的生命周期
取决于ActivityB的[[Activity Launch Mode|启动模式]]：
- B不复用（Standard或没有可复用的实例）
	- A.onPause 
	- B.onCreate 
	- B.onStart 
	- B.onResume 
	- A.onStop 
	- 



