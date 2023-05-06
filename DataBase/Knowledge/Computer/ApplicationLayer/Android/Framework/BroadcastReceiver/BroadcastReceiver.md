# 介绍
Android 广播分为两个角色:广播发送者、广播接受者
广播接收器的注册分为两种:静态注册、动态注册。
静态广播接收者:通过AndroidManifest.xml的标签来申明的BroadcastReceiver。
动态广播接收者:通过AMS.registerReceiver()方式注册的BroadcastReceiver，动态注册更为灵活，可在不需要 时通过unregisterReceiver()取消注册。
## 广播类型
### 普通广播
通过Context.sendBroadcast()发送，可并行处理
### 系统广播
当使用系统广播时，只需在注册广播接收者时定义相关的action即可，不需要手动发送广播(网络变 化,锁屏,飞行模式)
### 有序广播
指的是发送出去的广播被 BroadcastReceiver 按照先后顺序进行接收 发送方式变为: sendOrderedBroadcast(intent);
广播接受者接收广播的顺序规则(同时面向静态和动态注册的广播接受者):按照 Priority 属性值从大-小排 序，Priority属性相同者，动态注册的广播优先。
### App应用内广播(Local Broadcast)
#### 背景 
Android中的广播可以跨App直接通信(exported对于有intent-filter情况下默认值为true)
#### 可能出现的问题:
其他App针对性发出与当前App intent-filter相匹配的广播，由此导致当前App不断接收广播并处理;
其他App注册与当前App一致的intent-filter用于接收广播，获取广播具体信息; 即会出现安全性 & 效率性的问 题。
#### 解决方案 
使用App应用内广播(Local Broadcast) App应用内广播可理解为一种局部广播，广播的发送者和接收者都同属于一个App。
相比于全局广播(普通广播)，App应用内广播优势体现在:安全性高 & 效率高

具体使用1 - 将全局广播设置成局部广播
注册广播时将exported属性设置为false，使得非本App内部发出的此广播不被接收; 在广播发送和接收时，增设相应权限permission，用于权限验证;

发送广播时指定该广播接收器所在的包名，此广播将只会发送到此包中的App内与之相匹配的有效广播接收器 中。

具体使用2 - 使用封装好的LocalBroadcastManager类 
对于LocalBroadcastManage,方式发送的应用内广播，只能通过LocalBroadcastManager动态注册，不能静态注册
## 应用场景
1. 同一 App 内部的不同组件之间的消息通信(单个进程);  
2. 不同 App 之间的组件之间消息通信; 
3. Android系统在特定情况下与App之间的消息通信，如:网络变化、电池电量、屏幕开关等。
## 注册方法
静态注册:常驻系统，不受组件生命周期影响，即便应用退出，广播还是可以被接收，耗电、占内存。
动态注册:非常驻，跟随组件的生命变化，组件结束，广播结束。在组件结束前，需要先移除广播，否则容易造成内存泄漏。

# 底层实现
Android的Broadcast本质上是一种事件的订阅和发布机制。订阅者通过`registerReceiver`或者Manifest声明订阅消息，发布者通过`sendBroadcast`发送消息，消息由`AMS(ActivityManagerService)`进行和管理和派发，AMS充当消息中心的角色。

  
