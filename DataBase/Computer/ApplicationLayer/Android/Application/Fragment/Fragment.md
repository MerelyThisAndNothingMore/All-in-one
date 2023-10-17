---
tags: 
alias:
---


### Fragment为什么不能添加有参数的构造方法？
虽然Fragment可以通过new的方式创建，但是若涉及Activity状态的保存和恢复则可能会出问题。比如：Activity A可能由于长时间处于不可见而被杀死，则此时就涉及Activity状态的保存和恢复问题，而Activity中的FragmentManager会在Activity被销毁时，将所有Fragment按照android:fragments为key的数据里存储现在有哪些fragment显示、顺序、位置如何等等，当Activity需要恢复时则还是通过反射创建所以根本不知道需要构造参数如何赋值，因此无法给Activity或者Fragment添加有参数的构造方法，若fragment存在有参构造则最好有默认值处理。
### 为什么使用Fragment.setArguments(Bundle)传递参数
Activity.onCreate(Bundle saveInstance)->Fragment.instantitate() 当再次重建时会通过空参构造方法反射出新的fragment。并且给mArgments初始化为原先的值，而原来的

Fragment实例的数据都丢失了。

Activity重新创建时，会重新构建它所管理的Fragment，原先的Fragment的字段值将会全部丢失，但是通过 Fragment.setArguments(Bundle bundle)方法设置的bundle会保留下来，并在重建时恢复。所以，尽量使用 Fragment.setArguments(Bundle bundle)方式来进行参数传递。
