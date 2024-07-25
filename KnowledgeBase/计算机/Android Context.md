---
tags: 
aliases:
  - Context
---
# 定义

在 [[Android]] 开发中，Context 是一个重要的[[类]]，它提供了应用程序环境的全局信息，允许访问应用的资源和类特定的信息。Context 类的主要作用是提供应用程序运行时的全局上下文，使得应用的各个组件能够彼此通信和访问系统服务。

## 特点

1. **资源访问**：可以通过 Context 访问应用的资源，如字符串、样式、图像等。
2. **系统服务**：可以通过 Context 获取系统级别的服务，如 [[LayoutInflater]]、ActivityManager、ClipboardManager 等。
3. **组件之间的通信**：Context 提供了启动 [[Activity]]、发送广播、启动服务等功能，使得应用的各个组件能够相互通信。
4. **应用全局信息**：Context 提供了访问应用级别信息和环境信息的接口，如获取应用包名、文件目录等。

## Context 的类型

在 Android 中，Context 有不同的类型，每种类型的 Context 都有特定的用途：

### 1. Application Context

Application Context 是整个应用程序的上下文，生命周期与应用程序相同。它通常用于需要全局访问的场景，如[[Singleton Pattern|单例模式]]和全局配置。

### 2. Activity Context

Activity Context 是 Activity 的上下文，生命周期与 Activity 相同。它用于与当前 Activity 相关的操作，如启动 Activity 和访问当前 Activity 的资源。

### 3. Service Context

Service Context 是 [[Service]] 的上下文，生命周期与 Service 相同。它用于与当前 Service 相关的操作，如启动服务和访问服务的资源。

### 4. Base Context

Base Context 是一个抽象的上下文，可以通过继承和实现自定义的上下文行为。

# 使用场景

Context 在 Android 开发中有广泛的应用，以下是一些常见的使用场景：

### 1. 访问资源

通过 Context 访问应用的资源，如字符串、图像和样式等。

```java
String appName = context.getString(R.string.app_name);
Drawable icon = context.getDrawable(R.drawable.icon);
```

### 2. 启动 Activity

通过 Context 启动新的 Activity。

```java
Intent intent = new Intent(context, NewActivity.class);
context.startActivity(intent);
```

### 3. 获取系统服务

通过 Context 获取系统级别的服务。

```java
ClipboardManager clipboard = (ClipboardManager) context.getSystemService(Context.CLIPBOARD_SERVICE);
```

### 4. 发送广播

通过 Context 发送广播。

```java
Intent intent = new Intent("com.example.broadcast.MY_NOTIFICATION");
context.sendBroadcast(intent);
```

### 5. 文件操作

通过 Context 进行文件操作，如读取和写入文件。

```java
FileOutputStream fos = context.openFileOutput("example.txt", Context.MODE_PRIVATE);
fos.write("Hello, World!".getBytes());
fos.close();
```

## 注意事项

1. **避免内存泄漏**：在使用 Context 时，要避免持有长生命周期[[对象]]对短生命周期 Context 的引用，尤其是在使用 Activity Context 时，要特别注意避免[[内存泄漏]]。
2. **选择合适的 Context**：在不同的场景中选择合适的 Context，如全局操作使用 Application Context，与 UI 相关的操作使用 Activity Context。
3. **生命周期管理**：注意 Context 的生命周期，避免在销毁的 Context 上执行操作。

## 示例代码

以下是一个简单的示例，演示了如何在 Android 中使用 Context：

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // 使用 Application Context
        Context appContext = getApplicationContext();
        String appName = appContext.getString(R.string.app_name);
        Toast.makeText(appContext, "Application Name: " + appName, Toast.LENGTH_SHORT).show();

        // 使用 Activity Context
        Context activityContext = this;
        Intent intent = new Intent(activityContext, SecondActivity.class);
        activityContext.startActivity(intent);

        // 获取系统服务
        ClipboardManager clipboard = (ClipboardManager) appContext.getSystemService(Context.CLIPBOARD_SERVICE);

        // 发送广播
        Intent broadcastIntent = new Intent("com.example.broadcast.MY_NOTIFICATION");
        appContext.sendBroadcast(broadcastIntent);
    }
}
```

在这个示例中，我们演示了如何使用不同类型的 Context 来访问资源、启动 Activity、获取系统服务和发送广播。

# Q & A

**Q1: 什么是 Context？**

A1: Context 是 Android 中提供应用程序运行时环境的类，允许访问应用资源、系统服务和组件之间的通信。

**Q2: Context 有哪些常见类型？**

A2: 常见的 Context 类型包括 Application Context、Activity Context、Service Context 和 Base Context。

**Q3: 如何选择合适的 Context？**

A3: 选择合适的 Context 取决于具体的使用场景。全局操作使用 Application Context，与 UI 相关的操作使用 Activity Context。

**Q4: 如何避免 Context 引起的内存泄漏？**

A4: 避免持有长生命周期对象对短生命周期 Context 的引用，特别是避免在非静态内部类、匿名类或静态变量中持有 Activity Context。

**Q5: Context 的生命周期管理应该注意什么？**

A5: 注意 Context 的生命周期，避免在已经销毁的 Context 上执行操作。例如，在 Activity 销毁后，不要继续使用该 Activity 的 Context。

# Intro 
`Context`在[[Android]]开发中具有重要的作用，它提供了访问应用程序资源、启动组件、访问系统服务以及处理资源生命周期的能力。开发者可以使用`Context`来实现各种应用程序功能和与系统环境的交互。
## context家族
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/517ecdbab4a84f069872046835021374~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)
### ContextImpl
`Context`本身是一个抽象类，主要的实现类就是`ContextImpl`，`ContextImpl`实际承担着提供应用程序资源访问、组件启动和系统服务等功能的责任。
ContextImpl里比较重要的是关于resource和Theme的实现，通过这块儿代码来获取系统资源和主题设置。
### ContextWrapper
`ContextWrapper`是一个包装类，内部包含一个`mBase： Context`成员变量，所有的实现都是调用`mBase`的方法。
### ContextThemeWrapper
相比于ContextWrapper，ContextThemeWrapper中修改了Theme获取的相关逻辑。



