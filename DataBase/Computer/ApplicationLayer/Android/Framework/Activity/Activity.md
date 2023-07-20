---
tags: 
- android
alias:
---
# 启动流程
[[Activity|Activity]]启动可以分为根Activity启动和普通Activity启动，即是通过桌面图标启动还是由通过另一个Activity启动。
Activity实际上是在Instrumentation类的newActivity方法中被反射创建的。
注：Android P中创建新Activity由以前的scheduleLaunchActivity方法变成mH中EXECUTE_TRANSACTION消息执行ClientTransaction类型任务(实际为LaunchActivityItem类型)，继而执行client.handleLaunchActivity。 client实际类型为ClientTransactionHandler，而在Android P中，ActivityThread extends ClientTransactionHandler，而ClientTransactionHandler封装了handlexxxActivity的方法。
因此Android P中最后也是执行ActivityThread中的handleLaunchActivity方法执行创建Activity。 EXECUTE_TRANSACTION消息由ActivityThread中sendActivityResult方法调用
mAppThread.scheduleTransaction(clientTransaction)
--> ActivityThread.this.scheduleTransaction(transaction) 
--> ClientTransactionHandler(隐藏抽象类，ActivityThread是其子类)中scheduleTransaction方法
--> sendMessage(ActivityThread.H.EXECUTE_TRANSACTION, transaction);
简单列以下Android P的流程：
1. startActivity
   --> Activity(startActivity-> startActivityForResult)
   --> Instrumentation(execStartActivity)
   --> ActivityManager(getService.startActivity)
   --> ActivityManagerService(startActivity)
   --> ActivityStartController(obtainStarter工厂方法模式)
   --> ActivityStarter(execute--> startActivityMayWait--> startActivity--> startActivityUnchecked) 
2.  --> ActivityStackSupervisor(resumeTopActivityUncheckedLocked)
   --> ActivityStack(resumeTopActivityUncheckedLocked--> resumeTopActivityInnerLocked)
   --> ActivityStartSupervisor(startSpecificActivityLocked--> realStartActivityLocked)
   --> ClientLifecycleManager(scheduleTransation)
   --> ClientTransation(schedule) 
3.  --> ActivityThread(ApplicationThread(scheduleTransation)--> scheduleTransation)
   --> ClientTransationHandler(scheduleTransation--> sendMessage(ActivityThread.H.EXECUTE_TRANSATION))
   --> ActivityThread(H(handleMessage))
   --> TransationExceutor(execute)
   --> LaunchActivityItem(excute)
   --> ClientTransationHandler(handleLaunchActivity) 
4. (最后使用反射创建Activity). 
   --> ActivityThread(handleLaunchActivity--> performLaunchActivity)
   --> Instrumentation(newActivity--> getFactory(pkg))
   --> ActivityThread(peekPackageInfo)
   --> LoadedApk(getAppFactory)
   --> AppComponentFactory(instantiateActivity(cl, className, intent)--> (Activity) cl.loadClass(className).newInstance())
   --> Activity(performCreate--> onCreate)

## 跨进程启动/根Activity启动
### Launcher进程调用到systemServer进程
 ![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/10/9/16daf8c05d64c40a~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp)
点击App图标后，[[Launcher进程]]获取到[[system_server进程]]的[[Binder|Binder IPC]]，通过这个Binder将启动任务交由[[system_server进程]]执行。
- Activity.startActivity
- Activity.startActivityForResult
- Instrumentation.execStartActivity
- 获取到服务线程的IActivityTaskManager.aidl，这里定义了[[IPC]]使用到的[[Binder]]
### [[system_server进程|system_server进程]] 调用Process.start
[[AMS]]:解析Activity信息、处理启动参数
### Process.start 调用 startViaZygote
与Zygote进程建立连接并发送参数。



- [[system_server进程|system_server进程]] 收到请求后，向[[zygote进程]]进程发起创建进程请求。[[Launcher进程]]
	- 通过ActivityTaskManagerService实现IActivityTaskManager.Stub
	- ActivityTaskManagerService内部
- [[zygote进程]]fork出新的子进程，即[[app进程]]
- [[app进程]]通过[[Binder|Binder IPC]]向[[system_server进程]]发起attachApplication请求
- [[system_server进程]]收到请求后，进行一系列准备工作，再通过[[Binder|Binder IPC]]向[[app进程]]发送scheduleLaunchActivity请求。
- [[app进程]]的[[binder线程]]收到请求后，通过[[Handler]]向主线程发送LAUNCH_ACTIVITY消息
- 主线程收到Message后，通过反射机制创建目标Activity，并回调生命周期方法
## 进程内启动/普通Activity启动
请求进程A：
startActivity—(hook插入点1)—(AMP，ActivityManager代理对象)
——> system_server进程：AMS(ActivityManagerService) 解析Activity信息、处理启动参数、scheduleLaunchActivity/mH中EXECUTE_TRANSACTION消息处理(Android P)
--> 回到请求进程A：ApplicationThread 
--> ActivityThread -(hook插入点2)
-> Activity生命周期


## References 
[Activity启动分析](https://juejin.cn/post/6844903959581163528#heading-1) 
# 拒绝服务漏洞
## 定义
App A的ActivityA启动App B的ActivityB时，传过来一个Bundle数据，此数据是一个被Serializable修饰的类SerializableA。 
若App B中没有SerializableA这个类，只要App B的ActivityB中访问了Intent的Extra(getIntent().getExtras())则就会发生类找不到异常。此种情况就是服务漏洞
## 处理
try{}catch(Exception e){}
# 如何解决Activity参数的类型安全及接口繁琐的问题？
设置值：intent.putExtra("id", 0); 
获取值：String id = getIntent().getStringExtra("id");
参数类型需要人工约定。
# Activity常用的标记位Flags
FLAG_ACTIVITY_NEW_TASK
此标记位作用是为Activity指定“singleTask”启动模式，其效果和在XML中指定相同 android:launchMode="singleTask"
FLAG_ACTIVITY_SINGLE_TOP
此标记位作用是为Activity指定“singleTop”启动模式，其效果和在XML中指定相同 android:launchMode="singleTop"
FLAG_ACTIVITY_CLEAR_TOP 
具有此标记位的Activity，当它启动时，在同一个任务栈中位于它上面的Activity都要出栈。此标记位一般会和singleTask启动模式一起出现，此情况下，若被启动的Activity实例存在，则系统会调用它的onNewIntent。


