---
tags: 
alias:
---
https://blog.csdn.net/weixin_41607932/article/details/124180252

https://segmentfault.com/a/1190000022873569
大多数自定义View要么是在onDraw方法中画点东西，和在onTouchEvent中处理触摸事件。 
自定义View步骤 : 

onMeasure，可以不重写，不重写的话就要在外面指定宽高，建议重写; 
onDraw，看情况重写，如果需要画东西 就要重写; onTouchEvent，也是看情况，如果要做能跟手指交互的View，就重写; 

自定义View注意事项: 如果有 自定义布局属性的，在构造方法中取得属性后应及时调用recycle方法回收资源; onDraw和onTouchEvent方法中都 应尽量避免创建对象，过多操作可能会造成卡顿;

自定义ViewGroup步骤: 
onMeasure(必须)，在这里测量每一个子View，还有处理自己的尺寸; 
onLayout(必须)，在这里对子View进行布局; 
如有自己的触摸事件，需要重写onInterceptTouchEvent或 onTouchEvent; 

自定义ViewGroup注意事项: 
如果想在ViewGroup中画点东西，又没有在布局中设置background 的话，会画不出来，这时候需要调用setWillNotDraw方法，并设置为false; 
如果有自定义布局属性的，在构造方法 中取得属性后应及时调用recycle方法回收资源; onDraw和onTouchEvent方法中都应尽量避免创建对象，过多操作 可能会造成卡顿;


