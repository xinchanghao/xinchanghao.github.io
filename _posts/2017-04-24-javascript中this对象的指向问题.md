---
layout: post
title: "javascript中this对象的指向问题"
date: 2017-04-24
description: "javascript中this对象的指向问题"
tag: JavaScript
---

﻿**1. 不像C#，this一定是指向当前对象。**

js的this指向是不确定的，也就是说是可以动态改变的。call/apply 就是用于改变this指向的函数，这样设计可以让代码更加灵活，复用性更高。

**2. this 一般情况下，都是指向函数的拥有者。**

这一点很重要！这一点很重要！这一点很重要！
   这也是一道常见的面试题，如下代码：
```
var myObject = {
    foo: "bar",
    func: function() {
        var self = this;
        console.log(this.foo);  
        console.log(self.foo);  
        (function() {
            console.log(this.foo);  
            console.log(self.foo);  
        }());
    }
};
myObject.func();

//1.第一个this.foo输出bar，因为当前this指向对象myObject。
//2.第二个self.foo输出bar，因为self是this的副本，同指向myObject对象。
//3.第三个this.foo输出undefined，因为这个IIFE(立即执行函数表达式)中的this指向window。
//4.第四个self.foo输出bar，因为这个匿名函数所处的上下文中没有self，所以通过作用域链向上查找，从包含它的父函数中找到了指向myObject对象的self。
```
为什么第二点说一般情况下this都是指向函数的拥有者，因为有特殊情况。函数自执行就是特殊情况，在函数自执行里，this 指向的是：window。所以第一个 console.log 打印的是 window 的属性 number。
   所以要加一点：

**3. 在函数自执行里，this 指向的是 window 对象。**

理解关键：方法/函数是由谁(对象) 调用 的，方法/函数内部的 this 就指向谁(该对象)；

注意：被谁调用，不是处于谁的作用域，即使在作用域

转载请注明：[化风的博客](http://xinchanghao.github.io) » [javascript中this对象的指向问题](/2017/04/javascript中this对象的指向问题/)  
