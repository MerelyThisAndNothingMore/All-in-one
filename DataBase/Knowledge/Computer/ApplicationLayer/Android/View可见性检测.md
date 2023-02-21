#android_development
# 背景
## 定义
View的可见性指的是用户是否可以在屏幕上看到这个View
## 概念分析
view存在于view树中，存在几种不可见的可能性。
- View本身不绘制
	- visibility不为null
	- 在屏幕外
- 被遮挡
	- 
## 使用场景
- 当view部分可见时
	- ImageView开始渲染
- 当view完全可见时
	- 上报可见性埋点
	- 播放器开始播放
# 相关方法介绍
## View.getVisibility(): Int
```kotlin
fun getVisibility(): Int

View.GONE // 不占位不渲染
View.INVISIBLE // 占位不渲染
View.VISIBLE // 占位渲染

```

这个方法用来标识view是否允许在当前层级进行渲染，当返回为View.VISIBLE时不代表真正进行了渲染。例如View在屏幕外或父view的visibility不为VISIBLE。
## View.isShown(): Boolean
递归检查所有父view的visibility是否为VISIBLE。
## View.getGlobalVisibleRect(r: Rect): Boolean
返回view是否真实可见(不能检查被同级view遮挡的情形)
同时会将可见区域的全局坐标保存在传入的Rect对象中。
## View.getLocalVisibleRect(r: Rect): Boolean
返回View是否真实可见(不能检查被同级view遮挡的情形)
同时会将可见区域的相对坐标保存在传入的Rect对象中。
