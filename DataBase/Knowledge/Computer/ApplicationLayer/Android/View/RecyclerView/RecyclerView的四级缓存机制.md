---
tags: 
alias:
---
# 缓存结构划分
-   一级缓存：mAttachedScrap 和 mChangedScrap
-   二级缓存：mCachedViews
-   三级缓存：ViewCacheExtension
-   四级缓存：RecycledViewPool
## mAttachedScrap 和 mChangedScrap
### mChangedScrap
该层缓存目的是为了当调用notifyItemChanged(pos),notifyItemRangeChanged(pos,count)后该位置信息发生改变的缓存,一般用于change动画，注意mChangedScrap并不是说存储改变的位置并直接复用，而是在预布局时存储改变的holder，重新创建新holder并绑定数据来充当改变位置的数据刷新,然后根据新老holder执行change动画。动画执行完毕后新的holder会被缓存到mRecyclerPool中。

### mAttachedScrap
该层缓存目的是在调用notfyXxx时未改变的item，以及影响RecyclerView重新绘制的情况。
### 差异分析
mChangedScrap和mAttachedScrap可以看做是一个层级，都是屏幕上可见itemView,只不过区分了状态(改变和未改变)。
Recycler 类中，我们可以看到两个单独的 scrap 容器: mAttachedScrap 和 mChangedScrap。为什么需要两个呢？

ViewHolder 只有在满足下面情况才会被添加到 mChangedScrap：当它关联的 item 发生了变化（notifyItemChanged 或者 notifyItemRangeChanged 被调用），并且 ItemAnimator 调用 ViewHolder#canReuseUpdatedViewHolder 方法时，返回了 false。否则，ViewHolder 会被添加到mAttachedScrap 中。 canReuseUpdatedViewHolder 返回 “false” 表示我们要执行用一个 view 替换另一个 view 的动画，例如淡入淡出动画。 “true”表示动画在 view 内部发生。

mAttachedScrap 在 整个布局过程中都能使用，但是mChangedScrap 只能在预布局阶段使用。 这是有道理的：在布局后，新的 ViewHolder 应该替换掉“改变了的”视图，因此 AttachedScrap 在布局后是没有用的。 更改动画执行完成后，mChangedScrap 将按预期方式转存到 mRecyclerPool 中

**可以在 3 种情况下重用更新的 ViewHolder：**

1.  setSupportsChangeAnimations(false)。
2.  notifyDataSetChanged 而不是 notifyItemChanged 或 notifyItemRangeChanged
3.  notifyItemChanged(index，anyObject)。

最后一种情况显示了一种很好的方法，当只想更改一些内部元素时，可以避免创建/绑定新的 ViewHolder。
## mCachedViews
作用在滑动，当滑进屏幕或滑出屏幕，为了避免多次bind，是一个大小为2的List。
滑动过程中的回收和复用都是先处理的这个 List，这个集合里存的 ViewHolder 的原本数据信息都在，所以可以直接添加到 RecyclerView 中显示，不需要再次重新 onBindViewHolder()。
## ViewCacheExtension
## RecycledViewPool
作用在滑动,当超过mCachedViews缓存的大小时会将mCachedViews最老的数据移除放入到mRecyclerPool中 
根据itemType拿 holder集合，该集合默认大小为5，每次从mRecyclerPool取出的holder都要重置视图信息，也就是需要从新bind。
当mRecyclerPool 找不到缓存的holder时会调用adapter的onCreateViewHolder和onBindViewHolder。
# 预测动画
为什么要调用notifyXxx后要执行两次布局呢？一次预布局，一次实际布局？

因为RecyclerView 要执行预测动画。比如有A,B,C三个itemView，其中A和B被加载到屏幕上，这时候删除B后，按照最终效果我们会看到C移动到B的位置；因为我们只知道 C 最终的位置，但是不知道 C 的起始位置在哪里(即C还未被加载)。

第一次 预先布局

将之前原状态 下的 item 都布局出来。并且根据 Adapter 的 notify 信息，我们知道哪些 item 即将变化了，所以可以加载出另外的 View。在上述例子中，因为知道 B 已经被删除了，所以可以把屏幕之外的 C 也加载出来。

第二次，实际布局，也就是变化完成之后的布局。

这样只要比较前后布局的变化，就能得出应该执行什么动画了，就称为预测动画。
#  刷新回收复用机制
前面我们知道了调用notifyXxx后会RecyclerView会进行两次布局，一次预布局，一次实际布局，然后执行动画操作。具体执行方法如下：

-   dispatchLayoutStep1 预布局
-   dispatchLayoutStep2 实际布局
-   dispatchLayoutStep3 触发动画

1.  dispatchLayoutStep1

在预布局时就会查找改变holder，并保存在mChangedScrap中；其他未改变的保存到mAttachedScrap中；在预布局中获取holder时有以下代码片段，所以mChangedScrap保存的holder信息只有预布局时才会被复用，另外预布局后会将旧的holder信息保存，用于在dispatchLayoutStep3中执行change动画

```ini
//tryGetViewHolderForPositionByDeadline
if (mState.isPreLayout()) {
    holder = getChangedScrapViewForPosition(position);
    fromScrapOrHiddenOrCache = holder != null;
}
复制代码
```

其他未改变的位置holder从mAttachedScrap中获取

2.  dispatchLayoutStep2

实际布局，此步骤会创建一个新的holder并执行绑定数据，充当改变位置的holder。因为执行这一步骤时mState.isPreLayout就为false。然后holder为空，就会重新创建holder,并绑定数据。

其他位置holder从mAttachedScrap中获取

3.dispatchLayoutStep3

获取新老holder,执行change动画，动画完后新的holder会被保存到mRecyclerPool中。

而notifyDataSetChanged会导致RecyclerView上所有可见itemView全部remove,然后缓存在mRecyclerPool中，而此时mRecyclerPool中默认情况下每种itemType最大缓存5个，所以当缓存满时，就不会被缓存。导致接下来获取缓存holder时前5个直接获取，但是需要bind,超过5个，会从新adapter的onCreateViewHolder和onBindViewHolder。

所以这也就解释了为什么notifyItemChanged(pos),notifyItemRangeChanged(pos,count) 会比notifyDataSetChanged高效。

# 滑动回收复用机制
将滑出屏幕的缓存在mCachedViews中，默认大小为2，如果mCachedViews满，则删除mCachedViews最先被缓存的holder,放入到mRecyclerPool中。为什么要先放入到mCachedViews而不是直接放入mRecyclerPool，为什么要这样做？

因为刚滑出屏幕的itemView可能会被滑动进来，所以加了一层mCachedViews缓存，而从mCachedViews中获取的holder是不需要重新bind数据的。mRecyclerPool取出的holder会被重置信息，重新bind数据的。
# 总结
mChangedScrap，mAttachedScrap 针对的是屏幕可见itemView信息发生变化时的回收与复用

mCachedViews，mRecyclerPool 针对的是滑动回收与复用

另外可以通过setItemViewCacheSize 设置mCachedViews缓存大小，可以通过 recycledViewPool.setMaxRecycledViews() 修改mRecyclerPool缓存大小


# References 
[简书](https://juejin.cn/post/6854573221702795277#heading-9)
