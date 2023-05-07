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

