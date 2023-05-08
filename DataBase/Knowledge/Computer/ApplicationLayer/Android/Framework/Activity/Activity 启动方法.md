---
tags: 
alias:
---
# 显式启动和隐式启动
## 显式启动
```java
// 1. 使用构造函数 传入 Class对象
 Intent intent = new Intent(this, SecondActivity.class); 
 startActivity(intent);

// 2. 使用 setClassName（）传入 包名+类名 / 包Context+类名
 Intent intent = new Intent(); 
 // 方式1：包名+类名
 // 参数1 = 包名称
 // 参数2 = 要启动的类的全限定名称 
 intent.setClassName("com.hc.hctest", "com.hc.hctest.SecondActivity"); 

 // 方式2：包Context+类名
 // 参数1 = 包Context，可直接传入Activity
 // 参数2 = 要启动的类的全限定名称 
 intent.setClassName(this, "com.hc.hctest.SecondActivity"); 

 startActivity(intent);

// 3. 通过ComponentName（）传入 包名 & 类全名
 Intent intent = new Intent(); 
 // 参数1 = 包名称
 // 参数2 = 要启动的类的全限定名称 
 ComponentName cn = new ComponentName("com.hc.hctest", "com.hc.hctest.SecondActivity"); 
 intent.setComponent(cn); 
 startActivity(intent);
```

## 隐式启动
[隐式启动](https://www.jianshu.com/p/12c6253f1851) 
通过action、category、data来匹配Activity
![](https://gd-hbimg.huaban.com/d9bb17be5cd3f80672deb9804f3556831fa715412340e-BTnInq)
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
-   通过intent启动Activity的时候，如果intent-filter中没有添加android.intent.category.DEFAULT则会匹配失败
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