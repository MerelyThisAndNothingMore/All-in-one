---
tags: 
alias:
---
LayoutInflater 顾名思义，就是解析 xml ，生成相应的 view 出来，在 activity 中我们可以 findviewbyid 获取到布局文件中的 view ，若是我们想要的 view 不在 activity 的布局文件中，那么就得通过 LayoutInflater 来获取了

LayoutInflater是一个抽象类，我们通常使用它的 from(Context context) 静态方法创建一个 LayoutInflater 实例。

通过 [[Inflate]]() 方法来加载 View

```java
public View inflate(XmlPullParser parser, @Nullable ViewGroup root, boolean attachToRoot) {}
```

-   XmlPullParser，用来解析 xml 文件，
-   ViewGroup，view 所在的 viewGroup，如果没有，可以传 null
-   boolean 的 attachToRoot参数，标识是否要绑定到 根视图，就是第二个 root 中

  第一步获取 Resoureces 资源, 
  第二步通过XmlResourceParser去解析 xml




