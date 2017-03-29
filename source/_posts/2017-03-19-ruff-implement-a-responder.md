---
title: '[ruff学习]按键抢答器的实现'
date: 2017-03-19 12:05:31
tags: ruff
categories: ruff
---
<img src='http://p1.bqimg.com/582279/2b0732ee0ac08cf6.jpg'>
前几天入了ruff的板子，今天撸了[官网](https://ruff.io/zh-cn/)的官方教程[SOS 求救灯](http://community.ruff.io/t/ruff-lesson-1-sos/403/8) ，还有后面的课后练习-按键抢答器。
<!--more-->

# 按键抢答器
>使用三个按键模块和两个板载LED灯。
>- 如果按键 A 按下，板载蓝灯（led-b）亮；
>- 按键 B 按下，板载红灯（led-r）亮；
>- 按键 C 用于重置抢答器，C 键按下，两灯都灭；
>- 按键 A 或 B 按下后，直到按下按键 C 重置抢答器，即使另一按键按下，两LED 灯也不会有变化。

# 效果演示

<img src='http://i2.muimg.com/582279/059c16087c2040b5.gif'>

# 实践

## 材料清单
我看了看后面的回复
>因为套件里面只有两个大按钮，就使用板载的button-k2来作为重置按钮了

- 2个大按键模块
- ruff板子
- 6根杜邦线
  {% asset_img 微信图片_20170319122115.jpg 图1 材料清单 %}

## 部署项目
在此之前你需要参看官网写的
- [起步走](https://ruff.io/zh-cn/docs/getting-started.html)
- [ruff开发的基本步骤](https://ruff.io/zh-cn/docs/development-steps.html)

我这里用的是[AP模式](https://ruff.io/zh-cn/docs/channel.html)部署

step1:新建项目文件夹
```
mkdir responder
cd responder
```
step2:项目初始化
```
rap init
```
step3:添加外设(参照[这里](https://ruff.io/zh-cn/docs/getting-started-device.html))
```
rap device add button_a
? model: CK002
```
```
rap device add button_b
? model: CK002
```
step4:硬件布局按图连线
```
rap layout --visual
```
{% asset_img layout01.png 连线示意图 %}

## 事件绑定
`src/index.js`具体的代码
`switchLed()`  根据按钮的id来选择要触发的事件
```javascript
function switchLed(id){
    switch(id){
        case 'button_a': 
            if(!$('#led-b').isOn()) $('#led-r').turnOn();
            break;
        case 'button_b': 
            if(!$('#led-r').isOn()) $('#led-b').turnOn();
            break;
        case 'button-k2': 
            $('#led-r').turnOff();
            $('#led-b').turnOff();
            break;
    }
}
```
`$.ready` 中代码逻辑
```javascript
$.ready(function (error) {
    if (error) {
        console.log(error);
        return;
    }
    $('#button_a').on('push', function(){
        console.log('Button_a pushed.');
        switchLed('button_a');
    });
    $('#button_b').on('push', function(){
        console.log('Button_b pushed.');
        switchLed('button_b');
    });
    $('#button-k2').on('push', function(){
        console.log('Button-k2 pushed.');
        switchLed('button-k2');
    });
});
```

