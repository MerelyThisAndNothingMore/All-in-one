---
tags: 
alias:
---
在Activity的onSaveInstanceState方法回调时，put到参数outState(Bundle)里面。outState就是 ActivityClientRecord的state。

ActivityClientRecord实例，都存放在ActivityThread的mActivities里面。

