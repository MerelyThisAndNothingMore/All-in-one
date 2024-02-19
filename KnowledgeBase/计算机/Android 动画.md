---
tags: 
alias:
---

### 1. 帧动画（Frame Animation）

帧动画是最简单的动画形式之一，它通过顺序播放一系列图像来创建动画效果，类似于传统的动画片。在[[Android]]中，帧动画通常通过在`drawable`资源文件夹中定义一个XML文件来实现，该文件指定了一系列的图片资源和每帧的持续时间。

- **优点**：实现简单，适用于简单的动画效果。
- **缺点**：图片资源可能会占用大量[[内存]]，不适合复杂的动画效果。

### 2. 补间动画（Tween Animation）

补间动画可以在视图上创建移动、旋转、缩放和淡入淡出效果。这类动画不会改变视图的属性，而是创建视图属性变化的幻觉。补间动画同样可以通过XML文件定义，也可以在代码中动态创建。

- **优点**：相比帧动画，补间动画可以创建更加平滑和复杂的动画效果，且不需要大量的图片资源。
- **缺点**：不能对视图的实际属性做出改变，只是视觉上的变化。

```xml
<?xml version="1.0" encoding="utf-8"?>
<rotate
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="1000"
    android:fromDegrees="0"
    android:pivotX="50%"
    android:pivotY="50%"
    android:toDegrees="360" />

```

```java
ImageView imageView = findViewById(R.id.imageView);
Animation rotateAnimation = AnimationUtils.loadAnimation(this, R.anim.rotate_animation);
imageView.startAnimation(rotateAnimation);
```

### 3. 属性动画（Property Animation）

属性动画是Android 3.0（API级别11）引入的，它提供了一种更加强大和灵活的方式来动态改变对象的属性。属性动画系统允许开发者定义动画来改变任何对象的任何属性，不仅仅限于视图对象。属性动画可以在指定时间内平滑地改变属性值，还可以自定义插值器和动画监听器。

- **优点**：功能强大，可以应用于任何对象，支持自定义动画效果。
- **缺点**：相对复杂，需要更多的代码来实现特定的动画效果。

### 4. 布局动画（Layout Animations）

布局动画用于布局容器中的视图，当视图在容器中出现或消失时，可以自动应用动画效果。这可以通过在XML资源文件中定义布局动画来实现，也可以在代码中动态设置。

### 实现方法

- **XML资源文件**：通过在`res/anim`目录下创建XML文件来定义动画。这种方式使得动画可重用且易于修改。
- **Java代码**：动画也可以直接在Java代码中创建和配置。这种方式提供了更高的灵活性，可以根据应用的运行时状态动态创建动画。

```xml
<?xml version="1.0" encoding="utf-8"?>
<layoutAnimation xmlns:android="http://schemas.android.com/apk/res/android"
    android:delay="0.5"
    android:animationOrder="normal"
    android:animation="@anim/slide_up">
</layoutAnimation>

```

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromYDelta="100%"
        android:toYDelta="0%"
        android:duration="300"/>
    <alpha
        android:fromAlpha="0.0"
        android:toAlpha="1.0"
        android:duration="300"/>
</set>

```

```xml
<LinearLayout
    android:id="@+id/linearLayout"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:layoutAnimation="@anim/layout_animation">
    <!-- 子视图 -->
</LinearLayout>

```