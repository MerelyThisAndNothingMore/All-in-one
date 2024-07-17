---
tags: 
aliases:
  - View
---

# 定义

**View** 是 [[Android]] 用户界面的基本构建块。它代表屏幕上的一个矩形区域，负责绘制和处理交互事件。所有的 UI 组件都是 View 的子类，例如按钮、文本框、图像等。

## 特点

1. **绘制**：
   - View 负责在屏幕上绘制自身。它提供了 `onDraw()` 方法，可以通过重写该方法来自定义绘制逻辑。
2. **事件处理**：
   - View 能够响应用户的触摸和键盘输入等事件。通过重写 `onTouchEvent()` 和 `onKeyEvent()` 方法，可以处理用户交互。
3. **布局**：
   - View 包含布局属性，用于确定其在父容器中的位置和大小。常见的布局属性包括 `layout_width`、`layout_height`、`padding`、`margin` 等。
4. **层次结构**：
   - View 可以嵌套在 ViewGroup 中，形成视图层次结构。ViewGroup 是 View 的子类，用于容纳和管理多个子 View。

5. **动画**：
   - View 支持多种动画效果，包括属性动画、视图动画和转场动画，使得 UI 更加生动和流畅。

## 相似概念辨析

1. **View vs ViewGroup**：
   - View 是一个单一的 UI 元素，而 ViewGroup 是一个容器，能够包含和管理多个子 View。常见的 ViewGroup 子类有 LinearLayout、RelativeLayout、ConstraintLayout 等。
   
2. **View vs Widget**：
   - Widget 是 View 的子类，代表具体的 UI 组件，如 Button、TextView、ImageView 等。所有的 Widget 都是 View，但并不是所有的 View 都是 Widget。

3. **View vs Fragment**：
   - Fragment 是一个 UI 模块，包含自己的布局和行为，可以嵌入到 Activity 中。Fragment 可以包含多个 View 组件。

# 原理

1. **视图树**：
   - 在 Android 中，所有的 View 和 [[ViewGroup]] 组成一个视图树。[[Activity]] 的根视图是一个 ViewGroup，所有的子视图和子 ViewGroup 形成一个树形结构。

2. **测量、布局和绘制**：
   - **测量**：视图树从根节点开始，向下递归调用 `measure()` 方法，确定每个 View 的尺寸。
   - **布局**：视图树从根节点开始，向下递归调用 `layout()` 方法，确定每个 View 的位置。
   - **绘制**：视图树从根节点开始，向下递归调用 `draw()` 方法，绘制每个 View。

3. **事件分发**：
   - 用户事件（如触摸、点击）从根视图开始，通过 `dispatchTouchEvent()` 方法向下传递，直到找到能够处理事件的 View。

# 使用

1. **创建自定义 View**：
   - 通过继承 View 类并重写 `onDraw()` 方法，可以创建自定义 View，实现自定义绘制逻辑。

   ```java
   public class CustomView extends View {
       public CustomView(Context context) {
           super(context);
       }

       @Override
       protected void onDraw(Canvas canvas) {
           super.onDraw(canvas);
           // 自定义绘制逻辑
           Paint paint = new Paint();
           paint.setColor(Color.RED);
           canvas.drawCircle(50, 50, 30, paint);
       }
   }
   ```

2. **使用布局文件**：
   - 通过 XML [[布局]]文件定义 UI 结构，将 View 和 ViewGroup 组合在一起，形成复杂的界面。

   ```xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">

       <TextView
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="Hello, World!" />

       <Button
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="Click Me" />

   </LinearLayout>
   ```

3. **事件处理**：
   - 通过设置事件监听器来处理用户交互。

   ```java
   Button button = findViewById(R.id.my_button);
   button.setOnClickListener(new View.OnClickListener() {
       @Override
       public void onClick(View v) {
           // 处理点击事件
           Toast.makeText(MainActivity.this, "Button clicked!", Toast.LENGTH_SHORT).show();
       }
   });
   ```

# Q & A

1. **什么是 View？**
   - View 是 Android 中用户界面的基本构建块，负责绘制和处理交互事件。

2. **View 和 ViewGroup 有什么区别？**
   - View 是一个单一的 UI 元素，而 ViewGroup 是一个容器，能够包含和管理多个子 View。

3. **如何创建自定义 View？**
   - 通过继承 View 类并重写 `onDraw()` 方法，可以创建自定义 View，实现自定义绘制逻辑。

4. **如何在布局文件中使用 View？**
   - 通过 XML 布局文件定义 UI 结构，将 View 和 ViewGroup 组合在一起，形成复杂的界面。

5. **如何处理 View 的用户交互事件？**
   - 通过设置事件监听器（如 `OnClickListener`、`OnTouchListener`）来处理用户交互事件。

通过理解 Android 中 View 的定义、特点、原理和实际应用，可以更好地设计和实现用户界面，提高应用的用户体验和交互效果。


# 在Activity中获取某个View的宽高有几种方法
1. Activity/View#onWindowFocusChanged:此时View已经初始化完毕，当Activity的窗口得到焦点和失去焦 点时均会被调用一次，如果频繁地进行onResume和onPause，那么onWindowFocusChanged也会被频繁 地调用。  
2. view.post(runnable): 通过post将runnable放入ViewRootImpl的RunQueue中，RunQueue中runnable 最后的执行时机，是在下一个performTraversals到来的时候，也就是view完成layout之后的第一时间获取 宽高。 ViewTreeObserver#addOnGlobalLayoutListener:当View树的状态发生改变或者View树内部的View的可 见性发生改变时，onGlobalLayout方法将被回调。
3. View.measure(int widthMeasureSpec, int heightMeasureSpec): 
	1. match_parent 直接放弃，无法 measure出具体的宽/高。原因很简单，根据view的measure过程，构造此种MeasureSpec需要知道 parentSize，即父容器的剩余空间，而这个时候我们无法知道parentSize的大小，所以理论上不可能测量处 view的大小。
	2. wrap_content int widthMeasureSpec = View.MeasureSpec.makeMeasureSpec((1<<30)-1, View.MeasureSpec.AT_MOST); int heightMeasureSpec = View.MeasureSpec.makeMeasureSpec((1<<30)-1, View.MeasureSpec.AT_MOST); v_view1.measure(widthMeasureSpec, heightMeasureSpec);
	3. 具体的数值(dp/px) 这种模式下，只需要使用具体数值去measure即可，比如宽/高都是100px: int widthMeasureSpec = View.MeasureSpec.makeMeasureSpec(100, View.MeasureSpec.EXACTLY); int heightMeasureSpec = View.MeasureSpec.makeMeasureSpec(100, View.MeasureSpec.EXACTLY); v_view1.measure(widthMeasureSpec, heightMeasureSpec);
# 为什么onCreate获取不到View的宽高
Activity在执行完oncreate，onResume之后才创建ViewRootImpl,ViewRootImpl进行View的绘制工作 
调用链 startActivity
->ActivityThread.handleLaunchActivity
->onCreate
->完成DecorView和Activity的创建
->handleResumeActivity
->onResume()
->DecorView添加到WindowManager
->ViewRootImpl.performTraversals() 方法，测量(measure),布局(layout),绘制(draw), 从DecorView自上而下遍历整个View树。
# View#post与[[Handler]]#post的区别
对于View#post
当View已经attach到window，直接调用UI线程的Handler发送runnable。
如果View还未attach 到window，将runnable放入ViewRootImpl的RunQueue中，而不是通过MessageQueue。
RunQueue的作用类似 于MessageQueue，只不过这里面的所有runnable最后的执行时机，是在下一个performTraversals到来的时候，也 就是view完成layout之后的第一时间获取宽高，
MessageQueue里的消息处理的则是下一次loop到来的时候。
