---
tags: 
alias:
---
# 定义

MeasureSpec 是 [[View]] 的内部类，其封装了一个 View 的规格尺寸，包括 View 的宽和高的信 息，它的作用是在 [[Measure]] 流程中，系统会将 View 的 LayoutParams 根据父容器所施加的规则 转换成对应的 MeasureSpec，然后在 onMeasure 方法中根据这个 MeasureSpec 来确定 View 的宽和高。

在 View 的测量流程中，通过 makeMeasureSpec 来保存宽和高的信息。通过 getMode 或 getSize 得到模式和宽、高。

## 数据结构
![](https://gd-hbimg.huaban.com/301c4b422518317c0066d94778d060120cf8fbcd1088-4gWMSQ_fw1200webp)

MeasureSpec表示的是一个32位的整形值，它的高2位表示测量模式SpecMode，低30位表示某种测量模式下的规格大小SpecSize。

![](https://gd-hbimg.huaban.com/7b321377440c4a8463ef39ad99ea9ffb878bf74e4528-ivazJ4)


它由三种测量模式，如下:

-   `UNSPECIFIED` 父容器对View不会有任何限制,要多大给多大,一般是用在系统内部使用,我们开发的APP用不到.
-   `EXACTLY` 这种情况对应于`match_parent`和具体数值这两种模式,父容器已经检测出View需要的精确大小.
-   `AT_MOST` 这种情况对应于`wrap_content`,父容器指定了一个最大值,View不能超过这个值.

## 计算方法

![](https://gd-hbimg.huaban.com/3aafbf82bb37ee34d87bd2c98a11c402a0a10f3eb92e-02zelR)




