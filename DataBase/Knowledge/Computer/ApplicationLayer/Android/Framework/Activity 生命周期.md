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
-   **onCreate和onDestory**  
    分别代表了一个Activity的**创建和销毁**、第一个生命周期和最后一个生命周期回调，期间包裹了一个**完整**(entire lifetime)的Activity生命周期。
-   **onStart和onStop**  
    分别代表了Activity已经处于**可见状态和不可见状态**，此时的Activity未处在前台，**不可以与用户交互**，可多次被调用，期间Activity处于可见(visable lifetime)状态。
-   **onResume和onPause**  
    分别代表了Activity已经进入前台获得焦点和退出前台失去焦点，此时的Activity是可以和**用户交互的**，可多次被调用，期间的Activity处于前台(foreground lifetime)状态。
-   **onRestart**  
    表示Activity正在重新启动，正常状态下，Acitivty调用了onPause--onStop但是并没有被销毁，重新显示此Activity时，onRestart会被调用。



  
  
作者：MeloDev  
链接：https://www.jianshu.com/p/6d9d830a758d  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 调用原理
生命周期回调由[[AMS]]通过[[Binder]]通知应用进程调用。
对于 onSaveInstanceState ，由于[[Activity]]的状态是由ActivityManager进行管理的，因此这里会通过[[Binder]]将Bundle信息传输到ActivityManager进行管理。
系统会调用ActivityThread的performStopActivity方法中掉用onSaveInstanceState， 将状态保存在mActivities 中，mActivities维护了一个Activity的信息表，当Activity重启时候，会从mActivities中查询到对应的 ActivityClientRecord。
如果有信息，则调用Activity的onResoreInstanceState方法，
在ActivityThread的performLaunchActivity方法中，统会判断ActivityClientRecord对象的state是否为空
不为空则通过Activity的onSaveInstanceState获取其UI状态信息，通过这些信息传递给Activity的onCreate方 法，
## 弹出Dialog对生命周期的影响
弹出 Dialog、Toast、PopupWindow 本质上都直接是通过 WindowManager.addView 显示的（没有经过 AMS），所以不会对生命周期有任何影响。
如果是启动一个 **Theme 为 Dialog 的 Activity**, 则生命周期为：
**A.onPause -> B.onCrete -> B.onStart -> B.onResume**
注意这边没有前一个 Activity 不会回调 onStop，因为只有在 Activity 切到后台不可见才会回调 onStop；而弹出 Dialog 主题的 Activity 时前一个页面还是可见的，只是失去了焦点而已所以仅有 onPause 回调。

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
- B复用且已经在栈顶（singleTop）
	- B.onPause 
	- B.onNewIntent
	- B.onResume 
- B复用但不在栈顶(singleTask、singleInstance)
	- A.onPause 
	- B.onNewIntent 
	- B.onRestart 
	- B.onStart 
	- B.onResume 
	- A.onStop 
	- A.onDestroy (如果A被移出栈)
# OnActivityResult的调用时机
**B.onPause -> A.onActivityResult -> A.onRestart -> A.onStart -> A.onResume**
# onCreate 中的死循环会导致ANR吗
不会，死循环会阻塞主线程，在此基础上发生[[Android ANR]]中的情形就会导致ANR。

# References 
[5道生命周期面试题](https://www.sohu.com/a/402329833_611601) 




