---
tags: 
alias:
---

# FragmentPageAdapter和FragmentStatePageAdapter区别及使用场 景
使用FragmentPagerAdapter时 页面切换，只是调用detach，而不是remove，所以只执行onDestroyView，而 不是onDestroy，不会摧毁Fragment实例，只会摧毁Fragment 的View;

使用FragmentStatePageAdapter时 页面切换，调用remove，执行onDestroy。直接摧毁Fragment。

FragmentPagerAdapter最好用在少数静态Fragments的场景，用户访问过的Fragment都会缓存在内存中，即 使其视图层次不可见而被释放(onDestroyView) 。因为Fragment可能保存大量状态，因此这可能会导致使用大量内 存。

页面很多时，可以考虑FragmentStatePagerAdapter


https://github.com/romandanylyk/PageIndicatorView