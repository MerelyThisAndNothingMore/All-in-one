---
tags: 
alias:
---
ViewRootImpl，ViewRoot和View关系 
ViewRoot对应ViewRootImpl类，它是连接WindowManager和DecorView的纽带， View的三大流程（measure,layout,draw）均是通过ViewRoot来完成的。
ViewRootIml是View的根类，其控制着View的测量、绘制等操作 
ActivityThread中，当Activity对象被创建完毕后，会将DecorView添加到Window中，同时创建ViewRootImpl对象并和DecorView建立联系。 DecorView作为顶级的View一般情况下它内部会包含一个竖向的LinearLayout，这个LinearLayout里面有上下两个部分（具体情况和Android版本及主题有关），上面是标题栏，下面是内容栏。

