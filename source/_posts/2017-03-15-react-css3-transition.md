---
title: '[React笔记]动画:CSS3 Transition'
tags: react
categories: react
date: 2017-03-15 16:32:24
---
在[汇智网](http://www.hubwiz.com)学习react,今天学习，react中的[CSS3 Transiton](http://www.hubwiz.com/class/552762019964049d1872fc88)  按照例子做了一个仪表盘
![Markdown](http://p1.bqimg.com/582279/61793dd9d7d35c2at.jpg)
<!--more-->
# 知识点
>CSS3 Transition需要DOM属性的变化才能触发，所以我们需要将属性改变后的React 元素**重新渲染到真实DOM上**，才可以触发过渡效果。
>react中使用 CSS3的步骤:
>1. 为元素声明transition的样式
>2. 设置属性的初始值，第一次渲染元素
>3. 设置属性目标值，第二次渲染元素


# 实践
## 代码
>1. 使用了一个内部状态mounted 来实现第二次渲染：
>2. 当初次渲染完成功后，通过setState()方法我们触发 了再次渲染
>3. window.getComputedStyle() 这个方法的目的是刷新DOM的样式，以便确保之前设置的样式已经被应用到DOM 上了。

**代码1:**
<p data-height="265" data-theme-id="dark" data-slug-hash="qrXvPN" data-default-tab="js,result" data-user="ziazan" data-embed-version="2" data-pen-title="react CSS3 Transition-clock" class="codepen">See the Pen <a href="http://codepen.io/ziazan/pen/qrXvPN/">react CSS3 Transition-clock</a> by ziazan (<a href="http://codepen.io/ziazan">@ziazan</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

---
## 遇到的问题
### 问题1
```
React.findDOMNode(this.refs.ptr)
```
报错:
```
Uncaught TypeError: React.findDOMNode is not a function
```
在[这里](http://stackoverflow.com/questions/33031516/reactjs-finddomnode-and-getdomnode-are-not-functions)找到了答案。正解是:在最新的react版本中，`React.findDOMNode` 改为`ReactDOM.findDOMNode` 修改之后没有报错了。

### 问题2
我想，既然是 要让指针转动，我设置指针的角度，不断地改变角度值不是就可以了吗？
所以我做了下面的修改
**代码2**
```javascript
getInitialState: function() {
    return {
      value: 0, //速度值
      mounted: false, //是否已经挂到dom
      degree: -201
    };
```
```javascript
render: function() {
    if (this.state.mounted) {
       this.setState({degree:(this.props.value / 220) * 265 - 201});
      //确保之前的样式生效
 window.getComputedStyle(ReactDOM.findDOMNode(this.refs.ptr)).transform;
    }
    var style = {
      transform: 'rotate(' + this.state.degree + 'deg)'
    }
    return (
     ...
    )
  }
```
无情报错:
```
Uncaught RangeError: Maximum call stack size exceeded
    at h._updateDOMProperties (react-dom.min.js:13)
    at h.updateComponent (react-dom.min.js:13)
    at h.receiveComponent (react-dom.min.js:13)
    at Object.receiveComponent (react-dom.min.js:14)
    at Object.updateChildren (react-dom.min.js:13)
    at h._reconcilerUpdateChildren (react-dom.min.js:14)
    at h._updateChildren (react-dom.min.js:14)
    at h.updateChildren (react-dom.min.js:14)
    at h._updateDOMChildren (react-dom.min.js:13)
    at h.updateComponent (react-dom.min.js:13)
    at h.receiveComponent (react-dom.min.js:13)
    at Object.receiveComponent (react-dom.min.js:14)
```
解决方案
我在render方法中打`console.log('渲染第'+(++i)+ '次');`发现，**代码2**渲染输出了N多次，直到`Maximum call stack size exceeded`。而**代码1**中只渲染输出两次
[stackoverflow](http://stackoverflow.com/questions/32716885/maximum-call-stack-exceeded-error-in-reactjs-can-someone-help-explain-whats-go)
>You are calling `this.props.incCount` in `componentWillReceiveProps` which sets the state of the parent component and the effect will be that `AddingTheFriend` is rendered again, and this.props.incCount is called again. Hence the stack overflow.

这让我想起了
>React要求我们使用 `setState()`方法来进行状态设置。这是因为，`setState()`方法会自动 地重新渲染组件。

由此我猜测试 效果过渡时，设置了state,引起`ClockComp`的多次渲染，然后就溢出了。

---
## 第二种实现方式（多余）
但是我还是觉得这个方案是可行的，然后我再看了看代码。发现好像逻辑怪怪的。
最后进行了修改。
```javascript
render: function() {
    console.log('渲染第'+(++i)+ '次');
    console.log('时钟的角度'+this.state.degree);
    if (this.state.mounted) {
      //根据属性值计算旋转角度
      if(this.state.degree < (this.props.value / 220) * 265 - 201){
        this.setState({degree: ++ this.state.degree });
      }
      //确保之前的样式生效
  window.getComputedStyle(ReactDOM.findDOMNode(this.refs.ptr)).transform;
    }
    return (
    ...
    )
  }
```
效果是可以的，但是看日志发现，渲染了243次=.= ，应为是一次一次加的。再想想，看到了自己写的css 
```css
.pointer{
  position:absolute;
  left:150px;
  top:105px;
  transform-origin : 1px 45px;
  transition : transform 2s; /*注意这一行*/
}
```
发现自己完全没必要去 这么一点一点的增加角度来实现效果。(⊙o⊙)… 哎,我想多了。
傻逼的代码如下：
<p data-height="265" data-theme-id="dark" data-slug-hash="gmGjrm" data-default-tab="js,result" data-user="ziazan" data-embed-version="2" data-pen-title="react clock demo" class="codepen">See the Pen <a href="http://codepen.io/ziazan/pen/gmGjrm/">react clock demo</a> by ziazan (<a href="http://codepen.io/ziazan">@ziazan</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>