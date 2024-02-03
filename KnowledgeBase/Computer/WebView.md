---
tags: 
alias:
---

对于Android调用JS代码的方法有2种：

通过WebView的loadUrl（）
通过WebView的evaluateJavascript（）


对于JS调用Android代码的方法有3种：

通过WebView的addJavascriptInterface（）进行对象映射
通过 WebViewClient 的shouldOverrideUrlLoading ()方法回调拦截 url
通过 WebChromeClient 的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调拦截JS对话框alert()、confirm()、prompt（） 消息

https://wepie.yuque.com/we_play/hy63pl/cm8ry9bechr5gofr