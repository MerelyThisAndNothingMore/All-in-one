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

EXACTLY:精确测量模式，视图宽高指定为match_parent或具体数值时生效，表示父视图已经决定了子视图的精确大小，这种模式下View的测量值就是SpecSize的值。

AT_MOST:最大值测量模式，当视图的宽高指定为wrap_content时生效，此时子视图的尺寸可以是不超过父视 图允许的最大尺寸的任何尺寸。

UNSPECIFIED:不指定测量模式, 父视图没有限制子视图的大小，子视图可以是想要的任何尺寸，通常用于系统 内部，应用开发中很少用到。

MeasureSpec通过将SpecMode和SpecSize打包成一个int值来避免过多的对象内存分配，为了方便操作，其提 供了打包和解包的方法，打包方法为makeMeasureSpec，解包方法为getMode和getSize。

# 计算方法
![](https://gd-hbimg.huaban.com/3aafbf82bb37ee34d87bd2c98a11c402a0a10f3eb92e-02zelR)




