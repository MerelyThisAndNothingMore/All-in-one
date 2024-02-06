---
tags: 
alias:
---

点击事件用 MotionEvent 来表示，当一个点击事件产生后，事件最先传递给 [[Android Activity|Activity]]，0. 这会调用 Activity 的 dispatchTouchEvent()方法，当然具体的事件处理工作都是交由 Activity 中的 [[PhoneWindow]] 来完成的，然后 PhoneWindow 再把事件处理工作交给[[DecorView]]，之后再由 DecorView 将事件处理 工作交给根 [[ViewGroup]]。




