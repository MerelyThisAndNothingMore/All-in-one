---
tags: 
alias:
---
![](https://gd-hbimg.huaban.com/be208186d15151d85cb07c72df8d13727a6e1b7d6650-oIyfCw)

# 定义

在 Android 中，事件分发机制是指将用户的触摸事件（如点击、滑动等）从根视图传递到特定的视图，以便进行相应的处理。事件分发机制涉及到三个主要方法：`dispatchTouchEvent()`、`onInterceptTouchEvent()` 和 `onTouchEvent()`。

下面详细介绍这三个方法以及它们在事件分发中的作用。

## 1. `dispatchTouchEvent(MotionEvent event)`

`dispatchTouchEvent` 方法是事件分发的起点。每当触摸事件发生时，系统会首先调用 `dispatchTouchEvent` 方法。这个方法存在于 `Activity`、`ViewGroup` 和 `View` 中。

- **[[Activity]]**：`Activity` 的 `dispatchTouchEvent` 将事件分发给根视图（即 [[DecorView]]）。
- **ViewGroup**：`ViewGroup` 的 `dispatchTouchEvent` 会先调用 `onInterceptTouchEvent` 判断是否拦截事件，然后决定是交给子视图处理还是自己处理。
- **[[Android View]]**：`View` 的 `dispatchTouchEvent` 直接调用 `onTouchEvent` 处理事件。

## 2. `onInterceptTouchEvent(MotionEvent event)`

`onInterceptTouchEvent` 方法存在于 `ViewGroup` 中，用于决定是否拦截事件。

- 返回 `true`：拦截事件，不再分发给子视图，而是由 `ViewGroup` 自己处理。
- 返回 `false`：不拦截事件，事件将继续分发给子视图处理。

## 3. `onTouchEvent(MotionEvent event)`

`onTouchEvent` 方法是事件的最终处理者。这个方法存在于 `View` 和 `ViewGroup` 中，负责处理具体的触摸事件。

- 返回 `true`：事件被处理，后续事件继续由当前视图处理。
- 返回 `false`：事件未被处理，事件将返回给父视图的 `onTouchEvent` 处理。

## 事件分发流程

1. **Activity**：
   - 触摸事件首先到达 `Activity` 的 `dispatchTouchEvent`。
   - `Activity` 将事件传递给 `Window`，然后传递给 `DecorView`。

2. **DecorView**：
   - `DecorView` 作为根视图，调用自己的 `dispatchTouchEvent`。

3. **ViewGroup**：
   - `ViewGroup` 调用 `dispatchTouchEvent`。
   - 调用 `onInterceptTouchEvent` 判断是否拦截事件。
     - 如果拦截 (`true`)，则调用自己的 `onTouchEvent` 处理。
     - 如果不拦截 (`false`)，则将事件传递给子视图的 `dispatchTouchEvent`。

4. **View**：
   - `View` 直接调用 `dispatchTouchEvent`。
   - 调用 `onTouchEvent` 处理事件。

# 示例代码

下面是一个简单的示例，展示如何在 `ViewGroup` 中拦截触摸事件，以及在 `View` 中处理触摸事件。

## 自定义 ViewGroup 拦截触摸事件

```java
public class CustomViewGroup extends ViewGroup {
    public CustomViewGroup(Context context) {
        super(context);
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        // 拦截所有触摸事件
        return true;
    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        // 继续分发事件
        return super.dispatchTouchEvent(ev);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        // 处理触摸事件
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                Log.d("CustomViewGroup", "onTouchEvent: ACTION_DOWN");
                return true;
            case MotionEvent.ACTION_MOVE:
                Log.d("CustomViewGroup", "onTouchEvent: ACTION_MOVE");
                return true;
            case MotionEvent.ACTION_UP:
                Log.d("CustomViewGroup", "onTouchEvent: ACTION_UP");
                return true;
        }
        return super.onTouchEvent(event);
    }

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        // 布局子视图
    }
}
```

## 自定义 View 处理触摸事件

```java
public class CustomView extends View {
    public CustomView(Context context) {
        super(context);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        // 处理触摸事件
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                Log.d("CustomView", "onTouchEvent: ACTION_DOWN");
                return true;
            case MotionEvent.ACTION_MOVE:
                Log.d("CustomView", "onTouchEvent: ACTION_MOVE");
                return true;
            case MotionEvent.ACTION_UP:
                Log.d("CustomView", "onTouchEvent: ACTION_UP");
                return true;
        }
        return super.onTouchEvent(event);
    }
}
```

# 总结

在 Android 中，事件分发机制通过 `dispatchTouchEvent`、`onInterceptTouchEvent` 和 `onTouchEvent` 方法实现。`Activity` 将触摸事件传递给 `DecorView`，`DecorView` 进一步分发给子视图。`ViewGroup` 可以通过 `onInterceptTouchEvent` 方法拦截事件，而最终的事件处理由 `onTouchEvent` 方法完成。理解事件分发机制对于开发复杂的用户交互界面至关重要。

# Q & A

## view的onTouchEvent，OnClickListerner和OnTouchListener的 onTouch方法 三者优先级

**dispatchTouchEvent->onTouch->onInterceptTouchEvent->onTouchEvent**

1.dispatchTouchEvent
onTouchListener的onTouch方法优先级比onTouchEvent高，会先触发。 
2.假如 onTouch方法返回false会接着触发onTouchEvent，返回true, onTouchEvent方法不会被调用。 
3.onClick事件是在 onTouchEvent的MotionEvent.ACTION_UP事件通过performClick() 触发的。 
OnTouchListener中onTouch方法如 果返回true，则不会执行view的onTouchEvent方法，也就更不会执行view的onClickListener的onClick方法,
返回 false，则两个都会执行。
## onTouch 和onTouchEvent 的区别
onTouch方法是View的 OnTouchListener借口中定义的方法。 当一个View绑定了OnTouchLister后，当有

touch事件触发时，就会调用onTouch方 onTouchEvent 处理点击事件在dispatchTouchEvent中掉用

onTouchListener的onTouch方法优先级比onTouchEvent高，会先触发。 假如onTouch方法返回false，会接着 触发onTouchEvent，反之onTouchEvent方法不会被调用。 内置诸如click事件的实现等等都基于onTouchEvent，假 如onTouch返回true，这些事件将不会被触发

## ACTION_CANCEL什么时候触发

1.如果在父View中拦截ACTION_UP或ACTION_MOVE，在第一次父视图拦截消息的瞬间，父视图指定子视图不 接受后续消息了，同时子视图会收到ACTION_CANCEL事件。 
2.如果触摸某个控件，但是又不是在这个控件的区域上 抬起(移动到别的地方了)，就会出现action_cancel。
## 点击事件被拦截，但是想传到下面的View，如何操作

重写子类的requestDisallowInterceptTouchEvent()方法返回true就不会执行父类的onInterceptTouchEvent()，

可将点击事件传到下面的View, 剥夺了父view 对除了ACTION_DOWN以外的事件的处理权。
## 如何解决View的事件冲突
外部拦截法:指点击事件都先经过父容器的拦截处理，如果父容器需要此事件就拦截，否则就不拦截。具体方法:需要重写父容器的onInterceptTouchEvent方法，在内部做出相应的拦截。 

内部拦截法:指父容器不拦截任何事件，而将所有的事件都传递给子容器，如果子容器需要此事件就直接消耗，否则就交由父容器进行处理。具体方法: 需要配合requestDisallowInterceptTouchEvent方法。
## 在 ViewGroup 中的 onTouchEvent 中消费 ACTION_DOWN 事件， ACTION_UP事件是怎么传递
一个事件序列只能被一个View拦截且消耗。因为一旦一个元素拦截了此事件，那么同一个事件序列内的所有事 件都会直接交给它处理(即不会再调用这个View的拦截方法去询问它是否要拦截了，而是把剩余的ACTION_MOVE、 ACTION_DOWN等事件直接交给它来处理)。












