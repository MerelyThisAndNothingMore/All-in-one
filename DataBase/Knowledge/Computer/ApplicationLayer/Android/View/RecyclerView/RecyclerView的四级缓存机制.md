---
tags: 
alias:
---
# 缓存结构划分
-   一级缓存：mAttachedScrap 和 mChangedScrap
-   二级缓存：mCachedViews
-   三级缓存：ViewCacheExtension
-   四级缓存：RecycledViewPool
## mAttachedScrap
```java
final ArrayList<ViewHolder> mAttachedScrap = new ArrayList<>(); 
```

