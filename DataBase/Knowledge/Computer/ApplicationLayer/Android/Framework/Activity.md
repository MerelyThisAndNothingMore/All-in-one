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
# 显式启动和隐式启动
## 显式启动
1. intent(compnent)
2. intent.setComponent(componentName)
3. intent(className)
## 隐式启动
[隐式启动](https://www.jianshu.com/p/12c6253f1851) 
通过action、category、data来匹配Activity
### Action的匹配规则
-   action在Intent-filter可以设置多条
-   intent中必须指定action否则匹配失败且intent中action最多只有一条
-   intent中的action和intent-filter中的action必须完全一样时（包括大小写）才算匹配成功
-   intent中的action只要与intent-filter其中的一条匹配成功即可

```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>

    <intent-filter>
        <action android:name="com.jrmf360.action.ENTER"/>
        <action android:name="com.jrmf360.action.ENTER2"/>
        <category android:name="android.intent.category.DEFAULT"/>
    </intent-filter>
</activity>
```

下面的两个intent都可以匹配上面MainActivity的action规则

```cpp
Intent intent = new Intent("com.jrmf360.action.ENTER");
startActivity(intent);

Intent intent2 = new Intent("com.jrmf360.action.ENTER2");
startActivity(intent2);
```
### category的匹配规则
-   category在intent-filter中可以有多条
-   category在intent中也可以有多条
-   intent中所有的category都可以在intent-filter中找到一样的（包括大小写）才算匹配成功
-   通过intent启动Activity的时候如果没有添加category会自动添加android.intent.category.DEFAULT，如果intent-filter中没有添加android.intent.category.DEFAULT则会匹配失败
### data的匹配规则
```kotlin
<data 
  android:host="string"
  android:mimeType="string"
  android:path="string"
  android:pathPattern="string"
  android:pathPrefix="string"
  android:port="string"
  android:scheme="string"/>
```
```ruby
scheme://host:port/path|pathPrefix|pathPattern
jrmf://jrmf360.com:8888/first
```
scheme：主机的协议部分，如jrmf  
host：主机部分，如jrmf360.com  
port： 端口号，如8888  
path：路径，如first  
pathPrefix：指定了部分路径，它会跟Intent对象中的路径初始部分匹配，如first  
pathPattern：指定的路径可以进行正则匹配，如first  
mimeType：处理的数据类型，如image/*

-   intent-filter中可以设置多个data
-   intent中只能设置一个data
-   intent-filter中指定了data，intent中就要指定其中的一个data
-   setType会覆盖setData，setData会覆盖setType，因此需要使用setDataAndType方法来设置data和mimeType
### startActivity权限校验
-   同一个application下
-   Uid相同
-   permission匹配
-   目标Activity的属性Android:exported=”true”
-   目标Activity具有相应的IntentFilter，存在Action动作或其他过滤器并且没有设置exported=false
-   启动者的Pid是一个系统服务（System Server）的Pid【也就是系统服务前来调用普通App的Activity等】
-   启动者的Uid是一个System Uid（Android规定android.system.uid=1000，具有该Uid的application，我们称之为获得Root权限）
# 启动外部Activity的方式
## 共享uid的App(应用于系统级应用或者"全家桶"应用)，
示例：
```xml
<?xml version="1.0" encoding="utf-8"?> 
<manifest 
xmlns:android="http://schemas.android.com/apk/res/android" 
xmlns:tools="http://schemas.android.com/tools" 
package="com.android.customwidget" 
android:sharedUserId="com.demo"> 
... 
</manifest> 
```

共享uid不仅能启动其Activity，系统对于流量的计算等等都是共享的。
## 使用exported，
示例：
```xml
 <activity android:name=".BActivity" android:exported="true"/> 
```
## 使用IntentFilter，配置action等 <activity android:name=".BActivity" android:permission="com.demo.b"> <intent-filter> <action android:name="com.demo.intnet.Test"/> <category android:name="android.intent.category.DEFAULT"/> </intent-filter> </activity> 
使用：
```java
 Intent it = new Intent(); it.setAction("com.demo.intnet.Test"); startActivity(it);
```
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



