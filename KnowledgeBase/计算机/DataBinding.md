---
tags: 
alias:
---
https://blog.csdn.net/LucasXu01/article/details/103807451

https://juejin.cn/post/6844903494831308814#heading-6

https://www.jianshu.com/p/c56a987347ff
1.APT预编译方式生成ActivityMainBinding和ActivityMainBindingImpl

2.处理布局的时候生成了两个xml文件

activity_main-layout.xml(DataBinding需要的布局控件信息)

activity_main.xml(Android OS 渲染的布局文件)

Model是如何刷新View

1.DataBindingUtil.setContentView方法将xml中的各个View赋值给ViewDataBinding，完成findviewbyid的任 务

2.当VM层调用notifyPropertyChanged方法时，最终在ViewDataBindingImpl的executeBindings方法中处理逻 辑

View是如何刷新Model

ViewDataBindingImpl的executeBindings方法中在设置了双向绑定的控件上，为其添加对应的监听器，监听其 变动，如:EditText上设置TextWatcher，具体的设置逻辑放置到了TextViewBindingAdapter.setTextWatcher里

当数据发生变化的时候，TextWatcher在回调onTextChanged()的最后，会通过textAttrChanged.onChange()回 调到传入的mboundView2androidTextAttrChanged的onChange()。

使用


https://juejin.cn/post/6844903872520011784#heading-0