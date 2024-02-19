---
tags: 
alias:
---

# 动画类型
## 视图动画
### 补间动画
![](https://gd-hbimg.huaban.com/b2837d143acae97813ab4c85a8e433cb7ebb7d8b1b253-W0NpQl)

a. 标准的动画效果
补间动画常用于视图View的一些标准动画效果：平移、旋转、缩放 & 透明度；
除了常规的动画使用，补间动画还有一些特殊的应用场景。
b. 特殊的应用场景
Activity 的切换效果（淡入淡出、左右滑动等）
Fragement 的切换效果（淡入淡出、左右滑动等）
视图组（ViewGroup）中子元素的出场效果（淡入淡出、左右滑动等）
### 逐帧动画
![](https://gd-hbimg.huaban.com/0494a928d7f1a949c8b61070e2c7e4a7944d19d422efd-6yBj6i)

## 属性动画
![](https://gd-hbimg.huaban.com/ca543f6df145ba1fad2a4ebb234c1617d6c811ea1c9a3-6o6L9m)
### ValueAnimator
-   实现动画的原理：**通过不断控制 值 的变化，再不断 手动 赋给对象的属性，从而实现动画效果**。
### ObjectAnimator类
直接对对象的属性值进行改变操作，从而实现动画效果
继承自`ValueAnimator`类，即底层的动画实现机制是基于`ValueAnimator`类
### ValueAnimator类 & ObjectAnimator 类的区别
对比ValueAnimator类 & ObjectAnimator 类，其实二者都属于属性动画，本质上都是一致的：先改变值，然后 赋值 给对象的属性从而实现动画效果。
但二者的区别在于：
ValueAnimator 类是先改变值，然后 手动赋值 给对象的属性从而实现动画；是 间接 对对象属性进行操作；
ValueAnimator 类本质上是一种 改变 值 的操作机制

ObjectAnimator类是先改变值，然后 自动赋值 给对象的属性从而实现动画；是 直接 对对象属性进行操作；

可以理解为：ObjectAnimator更加智能、自动化程度更高
### 插值器
![](https://gd-hbimg.huaban.com/84c1aa7123a729d1c70adb16731005cbf93e5f2f22757-3IpWcO)
### 估值器
![](https://gd-hbimg.huaban.com/2af637b74c963c4490294318d4fe087d8087f1e91b274-HXBLd6)



![](https://gd-hbimg.huaban.com/979ed1ff29cf5fdff2ce33bf05e9465f1e6dcbcd10aad-2ICFjB)


# 补间动画和属性动画的区别
a. 作用对象局限:View

补间动画 只能够作用在视图View上，即只可以对一个Button、TextView、甚至是LinearLayout、或者其它继承 自View的组件进行动画操作，但无法对非View的对象进行动画操。

有些情况下的动画效果只是视图的某个属性 & 对象而不是整个视图; 如，现需要实现视图的颜色动态变化，那 么就需要操作视图的颜色属性从而实现动画效果，而不是针对整个视图进行动画操作

b. 没有改变View的属性，只是改变视觉效果

补间动画只是改变了View的视觉效果，而不会真正去改变View的属性。

如，将屏幕左上角的按钮 通过补间动画 移动到屏幕的右下角

  点击当前按钮位置(屏幕右下角)是没有效果的，因为实际上按钮还是停留在屏幕左上角，补间动画只是将这个
按钮绘制到屏幕右下角，改变了视觉效果而已。

c. 动画效果单一  
补间动画只能实现平移、旋转、缩放 & 透明度这些简单的动画需求
