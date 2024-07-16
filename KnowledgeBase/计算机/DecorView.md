---
tags: 
alias:
---

# 定义

`DecorView` 是 Android 应用程序窗口中的顶层视图（View）。它包含了应用程序的所有可视元素，包括状态栏、标题栏（如果有）以及应用程序的内容区域。`DecorView` 是 `FrameLayout` 的子类，由 `Window` 对象管理，用于组织和管理窗口中的视图层次结构。

## 特点

1. **顶层容器**：
   - `DecorView` 是应用窗口中的最顶层视图，包含所有子视图，包括状态栏、标题栏和内容视图。

2. **管理窗口装饰**：
   - `DecorView` 管理窗口的装饰元素，如标题栏、状态栏等。它是应用界面的框架。

3. **处理窗口背景**：
   - `DecorView` 处理窗口的背景绘制、窗口动画等，提供了一些视觉效果和交互能力。

4. **布局和测量**：
   - 负责测量和布局子视图，确保子视图正确显示在屏幕上。

# 原理

`DecorView` 是由 `Window` 对象创建并管理的。在 Android 中，每个 `Activity` 都有一个 `Window` 对象，该对象负责管理和显示视图层次结构。当 `Activity` 创建时，`Window` 会创建一个 `DecorView` 并将其作为内容视图的顶层容器。`DecorView` 的结构通常如下：

- **状态栏**：显示系统状态信息。
- **标题栏**（可选）：显示应用的标题。
- **内容视图**：显示应用的主要 UI 元素。

# 使用

`DecorView` 在 Android 系统中是由框架自动管理的，开发者通常不需要直接操作 `DecorView`。但了解 `DecorView` 的结构和功能有助于更好地理解 Android 界面的构建和布局。

## 获取 `DecorView`

可以通过 `Activity` 的 `getWindow().getDecorView()` 方法获取当前窗口的 `DecorView`。例如：

```java
DecorView decorView = getWindow().getDecorView();
```

## 自定义窗口装饰

通过获取 `DecorView`，开发者可以自定义窗口装饰，例如隐藏状态栏或标题栏：

```java
// 隐藏状态栏
getWindow().getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_FULLSCREEN);

// 隐藏标题栏
requestWindowFeature(Window.FEATURE_NO_TITLE);
```

## 修改 `DecorView` 中的视图

可以通过获取 `DecorView` 并对其子视图进行操作，例如添加或移除视图：

```java
DecorView decorView = getWindow().getDecorView();

// 添加自定义视图
TextView customView = new TextView(this);
customView.setText("Hello, World!");
((ViewGroup) decorView).addView(customView);

// 移除自定义视图
((ViewGroup) decorView).removeView(customView);
```

## 示例

以下是一个简单的示例，展示如何使用 `DecorView` 隐藏状态栏并添加一个自定义视图：

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // 隐藏状态栏
        getWindow().getDecorView().setSystemUiVisibility(View.SYSTEM_UI_FLAG_FULLSCREEN);
        // 隐藏标题栏
        requestWindowFeature(Window.FEATURE_NO_TITLE);

        setContentView(R.layout.activity_main);

        // 获取 DecorView
        View decorView = getWindow().getDecorView();

        // 添加自定义视图
        TextView customView = new TextView(this);
        customView.setText("Hello, World!");
        ((ViewGroup) decorView).addView(customView);
    }
}
```

# 总结

`DecorView` 是 [[Android]] 应用窗口中的顶层视图，包含了所有的窗口装饰元素和内容视图。它是 `Window` 对象的一部分，负责管理和显示窗口中的视图层次结构。通过理解和使用 `DecorView`，开发者可以更好地控制和自定义应用的界面元素和布局。



