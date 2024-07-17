---
tags: 
alias:
---
![](https://gd-hbimg.huaban.com/9698653a7d7b2f661ddb63f70c37821cfa322a405c0b3-WbUvrV_fw1200webp)
https://blog.csdn.net/aaajj/article/details/126453103
https://blog.csdn.net/zzz777qqq/article/details/110392973?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-110392973-blog-125452572.235^v36^pc_relevant_default_base3&spm=1001.2101.3001.4242.1&utm_relevant_index=3

https://blog.csdn.net/weixin_41607932/article/details/124180252
https://blog.csdn.net/qq_20798591/article/details/122120434

https://blog.csdn.net/aaajj/article/details/126453103
# 主流程介绍

[[ViewRootImpl]].performTraversals()方法是[[Android View]]绘制的起点，会依次调用performMeasure、performLayout、performDraw方法，在其方法的内部又会分别调用 View 的 [[Measure]]、[[Layout]] 和 [[Draw]] 方法。


-   performLayout : performLayout的原理其实是和performMeasure差不多,在performLayout里面调用了layout方法,然后在layout方法会调用onLayout方法,onLayout又会对所有子元素进行layout过程.由父元素向子元素传递,最终完成所有View的layout过程.确定View的4个点: left+top+right+bottom,layout完成之后可以通过getWidth和getHeight获取View的最终宽高.
-   performDraw : 也是和performMeasure差不多,从父元素从子元素传递.在performDraw里面会调用draw方法,draw方法再调用drawSoftware方法,drawSoftware方法里面回调用View的draw方法,然后再通过dispatchDraw方法分发,遍历所有子元素的draw方法,draw事件就这样一层层地传递下去.


# 为什么[[自定义View]] wrap_content不生效
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

3一般情况下在onLayout方法中使用getMeasuredWidth方法，而在除onLayout方法之外的地方用getWidth方法。

# invalidate() 和 postInvalidate() 方法的区别
requestLayout:会触发三大流程。
invalidate:触发 onDraw 流程，在 UI 线程调用。 postInvalidate:触发onDraw 流程，在非 UI 线程中调用。

1. view的invalidate递归调用父view的invalidateChildInParent，直到ViewRootImpl的 invalidateChildInParent，然后触发peformTraversals，会导致当前view被重绘,由于mLayoutRequested 为false，不会导致onMeasure和onLayout被调用，而OnDraw会被调用

2. postInvalidate(),它可以在UI线程调用，也可以在子线程中调用， postInvalidate()方法内部通过Handler发 送了一个消息将线程切回到UI线程通知重新绘制 。最终还是调用了子View的invalidate()

# Requestlayout，onlayout，onDraw，DrawChild区别与联系
requestLayout()方法 :会导致调用 measure()过程 和 layout()过程,不一定会触发OnDraw。 requestLayout会 直接递归调用父窗口的requestLayout，直到ViewRootImpl ,然后触发peformTraversals ，由于mLayoutRequested 为true，会导致onMeasure和onLayout被调用。不一定会触发OnDraw， 将会根据标志位判断是否需要onDraw。
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