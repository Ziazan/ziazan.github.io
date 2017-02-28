---
title: '[回答]const 与let 以及 this'
date: 2017-02-28 17:42:37
tags: [javascript, this, const, let, es6]
categories: [javascript]
---

# 1.关于this

this 的指向是由他所在函数调用的上下文决定的，而不是由它所在的函数定义的上下文决定的。
（在函数内部，this的值取决于函数是如何调用的。）
先看这篇文章:
[走火入魔javascript — this关键字](http://yijiebuyi.com/blog/e86ed5c9cdba6ae21d2a5b06b391c3d2.html)

#### 1.1 例子1
```javascript
function  foo(){
  let name = 'su';
  this.age = 12;
	console.log(name);//su
	console.log(age);//12
	console.log(this.name);//undefined
	console.log(this.age);//12
	console.log(this);//Window,此时this 指向的是Window
}
foo();
```
#### 1.2 例子2
```javascript
function  foo(){
    let name = 'su';
    this.age = 12;
    console.log(name);//su
    console.log(age);//12
    console.log(this.name);//undefined
    console.log(this.age);//12
    console.log(this);//foo{age: 12}
}
new foo();
```

 如果和new 关键字一起用， foo 作为构造函数调用。在函数的内部，this指向新创建的对象。
 被调用的函数没有显式的 return 表达式，则隐式的会返回 this 对象 - 也就是新创建的对象。

#### 1.3 例子3
```javascript
 var foo = {
    age:12,
    printAge: function(){
        console.log(this.age);
    }
}
foo.printAge();//{age:12}
```

当以对象里的方法的方式调用函数时，它们的 this 是调用该函数的对象.

# 2.关于const 与 let的选择
* const 与 let 都是用来声明变量的
* [let](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/let): 声明变量。 在可以使用var 声明的变量都可以考虑使用let 避免变量被重复定义。
* [const](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/const) :声明常量。如果这个值在后面都不会改变，建议使用const 声明成常量。如： const PI = 3.1415926 这种。

(自己的一点点理解，以后慢慢补充)

<!-- 参考链接: [javascript秘密花园](http://bonsaiden.github.io/JavaScript-Garden/zh/#function.this) -->