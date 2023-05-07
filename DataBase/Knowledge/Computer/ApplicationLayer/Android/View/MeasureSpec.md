---
tags: 
alias:
---
MeasureSpec表示的是一个32位的整形值，它的高2位表示测量模式SpecMode，低30位表示某种测量模式下的 规格大小SpecSize。MeasureSpec是View类的一个静态内部类，用来说明应该如何测量这个View。

它由三种测量模 式，如下:

EXACTLY:精确测量模式，视图宽高指定为match_parent或具体数值时生效，表示父视图已经决定了子视图的精确大小，这种模式下View的测量值就是SpecSize的值。

AT_MOST:最大值测量模式，当视图的宽高指定为wrap_content时生效，此时子视图的尺寸可以是不超过父视 图允许的最大尺寸的任何尺寸。

UNSPECIFIED:不指定测量模式, 父视图没有限制子视图的大小，子视图可以是想要的任何尺寸，通常用于系统 内部，应用开发中很少用到。

MeasureSpec通过将SpecMode和SpecSize打包成一个int值来避免过多的对象内存分配，为了方便操作，其提 供了打包和解包的方法，打包方法为makeMeasureSpec，解包方法为getMode和getSize。