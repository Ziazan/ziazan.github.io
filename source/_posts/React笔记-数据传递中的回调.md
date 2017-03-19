---
title: '[React笔记] 反向数据流'
date: 2017-03-02 12:42:24
tags: [react]
categories: [react]
---

参考链接: http://reactjs.cn/react/docs/thinking-in-react-zh-CN.html
> 在层级深处的表单组件需要更新上层级的state

<!--more-->
#### 官网解释
每当用户改变表单，就通过更新 state 来反映用户的输入。**由于组件应该只更新自己拥有的 state** ，FilterableProductTable 将会传递一个回调函数给 SearchBar ，每当 state 应被更新时回调函数就会被调用。我们可以使用 input 的 onChange 事件来接受它的通知。 FilterableProductTable 传递的回调函数将会调用 setState() ，然后应用将会被更新。

#### 理解画出的图解
我画了一个图来理解。
{% asset_img reactfunctionCallback.png react 函数回调 %}
图1  react 函数回调

#### 参考代码
<p data-height="265" data-theme-id="0" data-slug-hash="yMYwXm" data-default-tab="js" data-user="ziazan" data-embed-version="2" data-pen-title="React-demo1" class="codepen">See the Pen <a href="http://codepen.io/ziazan/pen/yMYwXm/">React-demo1</a> by ziazan (<a href="http://codepen.io/ziazan">@ziazan</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

