---
title: 'android webview localStorage 操作无效'
date: 2017-03-22 09:41:59
tags: [localStorage,android,webview,cookie]
categories: [问题集]
---

# 问题
项目时一个APP的外壳，在WebView中嵌入html5页面。我在h5中操作localStorage 来缓存数据，但是程序一到`localStorage.getItme()`就报错执行不下去。

# 寻找线索
在`localStorage.getItme()`进行了`try catch`看错误信息:
`Cannot call method 'getItem' of null`  似乎是不支持`localStorage`的操作。但是其他页面的`localStorage` 操作没有问题呀？是不是安卓那边的原因？

# 原因
后来在[stackoverflow](http://stackoverflow.com)找到[答案](http://stackoverflow.com/questions/5822256/error-web-console-uncaught-typeerror-cannot-call-method-getitem-of-null-at-h)
是安卓那边没有给当前的这个webview开启缓存权限。坑爹( ⊙ o ⊙ )啊！
```java
WebSettings settings = webView.getSettings();
settings.setDomStorageEnabled(true);
```
# 改用cookie
没办法，最后改用`cookie`来存数据。
[cookie的操作方法](http://www.cnblogs.com/fishtreeyu/archive/2011/10/06/2200280.html)