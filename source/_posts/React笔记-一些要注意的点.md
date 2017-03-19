---
title: '[React笔记]一些要注意的点'
date: 2017-03-07 14:36:25
tags: [react]
categories: [react]
---
![Markdown](http://p1.bqimg.com/582279/ed220c3d21dd7467s.png)
在React中要注意在componentWillUpadate 中不能用 this.setState() 方法。
<!--more-->
## componentWillUpadate
### 问题的出现 
在学习的时候，有一个例子的要求是，
> 使用componentWillUpdate()方法修改示例代码，使时钟在秒为0时显示为红色字体！

我一开始在**componentWillUpadate()**中 写的是
```javascript
...
 componentWillUpadate:function(){
   if(this.state.time.split(':')[2] === '00'){
        this.state. = 'red';
    }
 }
...
```
程序报bug了。

我去找原因：
>You cannot use this.setState() in componentWillUpadate . If you need to update state in response to a prop change, use componentWillReceiveProps instead.

代码如下：
<p data-height="265" data-theme-id="0" data-slug-hash="LWbYKY" data-default-tab="js" data-user="ziazan" data-embed-version="2" data-pen-title="LWbYKY" class="codepen">See the Pen <a href="http://codepen.io/ziazan/pen/LWbYKY/">LWbYKY</a> by ziazan (<a href="http://codepen.io/ziazan">@ziazan</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

官网上上说的是在**componentWillUpadate** 中不能用 this.setState() 方法。
## 设置组件style 
设置组件中的style属性时，要双大括号：
```javascript
<div className="ez-digi-clock"  style={{color:this.state.color}}>
    {this.state.time}
</div>
```
>这是因为 React 组件样式是一个对象，所以第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象
