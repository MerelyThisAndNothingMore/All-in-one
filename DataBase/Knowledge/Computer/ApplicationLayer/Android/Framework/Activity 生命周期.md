---
tags: 
alias:
---
Activity的生命周期方法有:
| 方法         | 描述     |  
| :------------- |:-------------|   
| onCreate()      | Activity首次创建时调用,用于初始化Activity     |   
| onStart()      | Activity变为可见时调用     |  
| onResume()      |Activity变为用户可交互状态时调用     |  
| onPause()      | Activity失去焦点、进入后台时调用,用于暂停操作     |  
| onStop()      | Activity完全不可见时调用     |   
| onRestart()      | Activity由停止状态变为可见状态时调用   |  
| onDestroy()     | Activity被销毁前调用,用于清理资源     |   
| onSaveInstanceState() | Activity被销毁前调用,用于保存状态     |  
| onRestoreInstanceState() | Activity重新创建时调用,用于恢复状态 |



