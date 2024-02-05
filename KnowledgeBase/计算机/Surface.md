---
tags: 
alias:
---

一个Surface就是一个对象，该对象持有一群像素（pixels），这些像素是要被组合到一起显示到屏幕上的。你在手机屏幕上看到的每一个[[Android Window]]（如对话框、全屏的activity、状态栏）都有唯一一个自己的surface，window将自己的内容（content）绘制到该surface中。
[[SurfaceFlinger]]根据各个surface在Z轴上的顺序（Z-order）将它们渲染到最终的显示屏上。




