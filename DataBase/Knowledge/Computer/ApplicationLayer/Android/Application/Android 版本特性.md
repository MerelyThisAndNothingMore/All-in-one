---
tags: 
alias:
---
https://www.jianshu.com/p/50ab0a1b0cd9
# 5.0 新特性—2014年（Lollipop）
-   全新的Material Design设计风格。
-   支持64位ART虚拟机。
    1.  放弃了之前一直使用的Dalvik虚拟机，改用了ART虚拟机，实现了真正的跨平台编译。（todo：弄懂为何）
        -   [https://www.cnblogs.com/ganchuanpu/p/9321682.html](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fganchuanpu%2Fp%2F9321682.html)
-   引入RecyclerView（todo：它的优点）。
    1.  [Android ListView与RecyclerView对比浅析](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fimport_sadaharu%2Farticle%2Fdetails%2F81323801)
-   新增悬挂式Notification。
    1.  相较于普通式和折叠式Notification需要拉下通知中心才可以查看的交互，悬挂式直接显示在屏幕上方，并且焦点不变，仍然在用户操作的界面上，不会打断用户的操作，过几秒会消失。
    2.  Android 5.0 支持对Notification设置显示等级的能力。
-   引入更加灵活的Toolbar，取代ActionBar。
# _**Android6.0 2015**_
2015年5月28日
## **1.主要有运行时权限（最主要的更新）**
-   targetSdkVersion >= 23。
    
-   分位Normal Permissions和Dangerous Permissions。
    
-   ActivityCompat.checkSelfPermissions()请求，低于6.0的版本，次方法默认返回值为PackManager.PERMISSION_GRANTED。
    
-   onRequestPermissionsResult()回调结果。
    
-   如果用户选择了『不在询问』，下次则不会弹框，而是直接处理拒绝后的逻辑。
    
## **2.取消支持Apache HTTP客户端**

# _**Android 7.0 2016**_ 

多窗口模式（分屏模式）

1.  进入多窗口的Activity生命周期变化，会先onDestroy销毁，随后重建，停在onPause状态。
2.  推出多窗口的Activity生命周期变化，接着上面onPause->onDestroy，随后正常重建。
3.  禁用多窗口模式：在manifest.xml中配置`android:resizeableActivity="false"`

**1.低电耗模式**

**2.系统权限更改**

# _**Android 8.0 2017**_

**1．通知渠道**

1.  **【重点】通知中心**
    1.  所有通知都必须分到一个渠道，即新增NotificationChannel。

**2.画中画模式(PIP)**

1.  后台执行限制
    1.  后台service限制。
    2.  广播限制：除了有限的例外情况，应用无法使用清单注册隐式广播。

# _**Android 9.0 2018（派）**_
1.  全面支持全面屏
    1.  通过DisplayCutout类可以确定非功能区域的位置和形状，这些区域不应显示内容。
**1.利用wifi-RTT进行室内定位**

**2.显示屏缺口支持**

**3.前台服务**

**4.启动Activity**

**5.Http请求**

**6.Apache HTTP客户端弃用**

# _**Android 10 2019**_

**1.用户存储权限的变更**

**2.用户的定位权限的变更**

**3.设备唯一标识符的变更**
1.  ART优化，
    1.  添加了一种垃圾回收机制，节省垃圾回收的时间，帮助在低版本设备上顺畅运行。
# _**Android 11 2020**_
2020年9月
系统主要增强了聊天气泡，安全性和隐私性的保护，电源菜单，可以更好的支持瀑布屏，折叠屏，双屏和 Vulkan 扩展程序等。（注意：Android11 有一个沙盒机制，有兴趣的可以看一下，这个机制对外部设备的个人信息或者数据等等来说增强了很大的安全机制，如果你需要调用第三方外部设备 那可能你得避免这个沙盒机制去做到高授权了。）
1.  权限和隐私
    1.  在Android10的用户隐私基础上，新增了位置、麦克风和摄像头的一次性权限许可。
**1.短信更新改进**

**2.电话号码相关权限**

**3.现在需要 APK 签名方案 v2**

# _**Android12**_

**1.设置页面被重新设计**
1.  Activity/BroadcastReciver/Service 声明了Filter，则必须显示设置`android:exported`属性。
# _**Android13**_







