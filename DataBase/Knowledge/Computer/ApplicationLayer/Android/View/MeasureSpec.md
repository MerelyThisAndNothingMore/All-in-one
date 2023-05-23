---
tags: 
alias:
---
# 定义
## 数据结构
![](https://gd-hbimg.huaban.com/301c4b422518317c0066d94778d060120cf8fbcd1088-4gWMSQ_fw1200webp)

MeasureSpec表示的是一个32位的整形值，它的高2位表示测量模式SpecMode，低30位表示某种测量模式下的 规格大小SpecSize。MeasureSpec是View类的一个静态内部类，用来说明应该如何测量这个View。

![](https://gd-hbimg.huaban.com/7b321377440c4a8463ef39ad99ea9ffb878bf74e4528-ivazJ4)


它由三种测量模 式，如下:

-   `UNSPECIFIED` 父容器对View不会有任何限制,要多大给多大,一般是用在系统内部使用,我们开发的APP用不到.
-   `EXACTLY` 这种情况对应于`match_parent`和具体数值这两种模式,父容器已经检测出View需要的精确大小.
-   `AT_MOST` 这种情况对应于`wrap_content`,父容器指定了一个最大值,View不能超过这个值.

MeasureSpec通过将SpecMode和SpecSize打包成一个int值来避免过多的对象内存分配，为了方便操作，其提 供了打包和解包的方法，打包方法为makeMeasureSpec，解包方法为getMode和getSize。

# 计算方法
![](https://gd-hbimg.huaban.com/3aafbf82bb37ee34d87bd2c98a11c402a0a10f3eb92e-02zelR)




