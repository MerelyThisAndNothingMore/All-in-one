---
tags: 
alias:
---
# 主流程介绍
## Measure
主要用于确定 View 的测量宽/高。
主要包含两个步骤：
1.  求取 View 的测量规格[[MeasureSpec]]。
2.  依据上一步求得的[[MeasureSpec]]，对 View 进行测量，求取得到 View 的最终测量宽/高。
`LayoutParams`会受到父容器的`MeasureSpec`的影响，测量过程会依据两者之间的相互约束最终生成子View 的`MeasureSpec`，完成 View 的测量规格。

简而言之，View 的`MeasureSpec`受自身的`LayoutParams`和父容器的`MeasureSpec`共同决定（`DecorView`的`MeasureSpec`是由自身的`LayoutParams`和屏幕尺寸共同决定，参考后文）。也因此，如果要求取子View 的`MeasureSpec`，那么首先就需要知道父容器的`MeasureSpec`，层层逆推而上，即最终就是需要知道顶层View（即`DecorView`）的`MeasureSpec`，这样才能一层层传递下来，这整个过程需要结合`Activity`的启动过程进行分析。
![](https://gd-hbimg.huaban.com/855ca61cf63d564022c99601d05350779a77b22f2d8a6-BK7djg)

### DecorView 的MeasureSpec
**`DecorView`的布局参数为`MATCH_PARENT`**
-   当`DecorView`的`LayoutParams`为`MATCH_PARENT`时，说明`DecorView`的大小与屏幕一样大，而又由于屏幕大小是确定的，因此，其 SpecMode 为`EXACTLY`，SpecSize 为`windowSize`，；
-   当`DecorView`的`LayoutParams`为`WRAP_CONTENT`时，说明`DecorView`自适应内容大小，因此它的大小不确定，但是最大不能超过屏幕大小，故其 SpecMode 为`AT_MOST`，SpecSize 为`windowSize`；
-   其余情况为`DecorView`设置了具体数值大小或`UNSPECIFIED`，故以`DecorView`为主，其 SpecMode 为`EXACTLY`，SpecSize 就是自己设置的值，即`rootDimension`；

由于`DecorView`的`LayoutParams`为`MATCH_PARENT`，因此，`DecorView`的`MeasureSpec`最终为：`MeasureSpec.makeMeasureSpec(windowSize, MeasureSpec.EXACTLY)`
### 单一View
measure() 
-> onMeasure() 
-> getDefaultSize() 计算View的宽/高值 
-> setMeasuredDimension存储测量后的View宽 / 高 其实就是将 View 的最终测量宽/高设置到`View.mMeasuredWidth`/`View.mMeasuredHeight`属性中，完成测量过程。
###  ViewGroup
-> measure() 
-> 需要重写onMeasure( ViewGroup 没有定义测量的具体过程，因为ViewGroup是一个抽象类，其测量过程的onMeasure方法需要各个子类去实现。 如:LinearLayout、RelativeLayout、FrameLayout等等，这些控件的特性都是不一样的，测量规则自然也都不一 样。)遍历测量ViewGroup中所有的View 
-> 根据父容器的MeasureSpec和子View的LayoutParams等信息计算子 View的MeasureSpec 
-> 合并所有子View计算出ViewGroup的尺寸 
-> setMeasuredDimension 存储测量后的宽 / 高 从顶层父View向子View的递归调用view.layout方法的过程，即父View根据上一步measure子View所得到的布局大小 和布局参数，将子View放在合适的位置上。

## Layout
主要用于确定 View 在父容器中的放置位置。
先通过 measure 测量出 ViewGroup 宽高，ViewGroup 再通过 layout 方法根据自身宽高来确定自身 位置。当 ViewGroup 的位置被确定后，就开始在 onLayout 方法中调用子元素的 layout 方法确定子元素的位置。子元素如果是 ViewGroup 的子类，又开始执行 onLayout，如此循环往复，直到所有子元素的位置都被确定，整个 View 树的 layout 过程就执行完了。
## Draw
ViewRoot创建一个Canvas对象，然后调用OnDraw()。六个步骤:
1、绘制视图的背景; 
2、保存画布的图层(Layer);
3、绘制View的内容;
4、绘制View子视图，如果没有就不用;
5、还原图层 (Layer);
6、绘制View的装饰(例如滚动条等等)。

# 为什么自定义View wrap_content不生效
## 原因分析
### `wrap_content`起到与`match_parent`相同的作用
在onMeasure()中的getDefaultSize（）的默认实现中，当View的测量模式是AT_MOST或EXACTLY时，View的大小都会被设置成子View MeasureSpec的specSize。

因为AT_MOST对应wrap_content；EXACTLY对应match_parent，所以，默认情况下，wrap_content和match_parent是具有相同的效果的。
### 为什么是填充父容器的效果呢
因为在计算子View MeasureSpec的getChildMeasureSpec()中，子View MeasureSpec在属性被设置为wrap_content或match_parent情况下，子View MeasureSpec的specSize被设置成parenSize = 父容器当前剩余空间大小

所以：wrap_content起到了和match_parent相同的作用：等于父容器当前剩余空间大小

# getWidth()方法和getMeasureWidth()区别
1getMeasuredWidth方法获得的值是setMeasuredDimension方法设置的值，它的值在measure方法运行后就会确定

2getWidth方法获得是layout方法中传递的四个参数中的mRight - mLeft，它的值是在layout方法运行后确定的

3一般情况下在onLayout方法中使用getMeasuredWidth方法，而在除onLayout方法之外的地方用getWidth方 法。

# invalidate() 和 postInvalidate() 方法的区别
requestLayout:会触发三大流程。
invalidate:触发 onDraw 流程，在 UI 线程调用。 postInvalidate:触发onDraw 流程，在非 UI 线程中调用。

1. view的invalidate递归调用父view的invalidateChildInParent，直到ViewRootImpl的 invalidateChildInParent，然后触发peformTraversals，会导致当前view被重绘,由于mLayoutRequested 为false，不会导致onMeasure和onLayout被调用，而OnDraw会被调用

2. postInvalidate(),它可以在UI线程调用，也可以在子线程中调用， postInvalidate()方法内部通过Handler发 送了一个消息将线程切回到UI线程通知重新绘制 。最终还是调用了子View的invalidate()

# Requestlayout，onlayout，onDraw，DrawChild区别与联系
requestLayout()方法 :会导致调用 measure()过程 和 layout()过程,不一定会触发OnDraw。 requestLayout会 直接递归调用父窗口的requestLayout，直到ViewRootImpl,然后触发peformTraversals，由于mLayoutRequested 为true，会导致onMeasure和onLayout被调用。不一定会触发OnDraw， 将会根据标志位判断是否需要ondraw。
onLayout()方法(如果该View是ViewGroup对象，需要实现该方法，对每个子视图进行布局) 
onDraw()方法:绘制视 图本身 (每个View都需要重载该方法，ViewGroup不需要实现该方法)。
drawChild():去重新回调每个子视图的 draw()方法。
# LinearLayout、FrameLayout 和 RelativeLayout 哪个效率高
简单布局 FrameLayout>LinearLayout>RelativeLayout 
复杂布局 RelativeLayout>LinearLayout>FrameLayout 

(1)FrameLayout是从上到下的一个堆叠的方式布局的，那当然是绘制速度最快，只需要将本身绘制出来即可，但是由 于它的绘制方式导致在复杂场景中直接是不能使用的，所以工作效率来说FrameLayout仅使用于单一场景

(2) RelativeLayout会让子View调用2次onMeasure，LinearLayout 在有weight时，也会调用子View 2次onMeasure。 由于RelativeLayout需要在横向和纵向分别进行一次measure过程。而LinearLayout只进行纵向或横向的测量，所以 measure的时间会比RelativeLayout少很多。但是如果设置了 weight,在测量的过程中，LinearLayout会将设置过 weight的和没设置的分别测量一次，这样就导致measure两次。
(3)在不影响层级深度的情况下,使用 LinearLayout和FrameLayout而不是RelativeLayout，复杂布局使用RelativeLayout 简单布局:在DecorView自己是 FrameLayout但是它只有一个子元素是属于LinearLayout。因为DecorView的层级深度是已知而且固定的，上面一个 标题栏，下面一个内容栏。采用RelativeLayout并不会降低层级深度，所以此时在根节点上用LinearLayout是效率最 高的。 复杂布局:RelativeLayout 在性能上更好，使用 LinearLayout 容易产生多层嵌套的布局结构，这在性能上是 不好的。 而 RelativeLayout 通常层级结构都比较扁平，很多使用LinearLayout 的情况都可以用一个 RelativeLayout 来替代，以降低布局的嵌套层级，优化性能。
# LinearLayout的绘制流程
onMeasure():
1>:把 ViewRootImpl 的测量模式 传递给 DecorView，然后 DecorView 把测量模式 传递给 LinearLayout，遍历子元素并对每个子元素执行measureChildBeforeLayout方法，这个方法内部会调用子元素的 measure方法，这样各个子元素就开始依次进入measure过程。 
2.LinearLayout类的measureVertical方法会遍历每 一个子元素并且执行LinearLayout类的measureChildBeforeLayout方法对子元素进行测量，LinearLayout类的 measureChildBeforeLayout方法内部会执行子元素的measure方法。
在代码中，变量mTotalLength会是用来存放 LinearLayout在竖直方向上的当前高度，每遍历一个子元素，mTotalLength就会增加 
onLayout(): 其中 会遍历调用每个子View的setChildFrame方法为子元素确定对应的位置 其中会遍历调用每个子View的setChildFrame 方法为子元素确定对应的位置。其中的childTop会逐渐增大，意味着后面的子元素会被放置在靠下的位置。