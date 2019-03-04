---
layout: post
title: "javascript设计模式之new创建对象的安全模式"
date: 2017-03-23
description: "javascript设计模式之new创建对象的安全模式"
tag: JavaScript
---

﻿####对于我们初学者来说，在使用js创建对象时，不适应闭包的写法，经常容易忘记使用new关键字而犯错误。


**闭包**是有权访问另外一个函数作用域中变量的函数，即在一个函数内部创建另外一个函数。我们将这个闭包作为创建对象的构造函数，这样他既是闭包又是可实例化对象的函数，即可访问到类函数作用域中变量，如bookNum这个变量，此时这个变量叫做静态私有变量，并且checkBook()可称之为静态私有方法。当然闭包内部也有自身的私有变量以及私有方法如price,checkID()。

```
//利用闭包实现
var Book=(function(){
//静态私有变量
var bookNum = 0;
//创建私有方法
function checkBook(name){}
//创建类
function book(newId,newName,newPrice){
	//私有变量
	var name,price;
	//私有方法
	function checkID(id){}
	//特权方法
	this.getName=function(){};
	this.getPrice=function(){};
	this.setName=function(){};
	this.setPrice=function(){};
	//公有属性
	this.id=newId;
	//公有方法
	this.copy=function(){};
	bookNum++
	if(bookNum>100){
		throw new Error('没有更多书.');
	}
	this.setName(name);
	this.setPrice(price);
}
	//构建原型
	_book.prototype={
		//静态公有属性
		isJSBook:false,
		//静态共有方法
		display:function(){}
	};
	//返回类
	return _book;
})();
```


**new创建类**

```
//图书类
var Book = function(title,time,type){
	this.title = title;
	this.time = time;
	this.type = type;
}
//实例化一本书
var book = Book('javascript','2014','js');

结果：
console.log(book); //undefined
console.log(window.title); //javascript
console.log(window.time); //2014
console.log(window.type); //js
```

*new关键字作用可以看作是对当前对象的this不停地赋值，然而例子中没有用new,所以就会直接执行这个函数，而这个函数在全局作用域中执行了，所以在全局作用域中this指向的是当前对象自然就是全局变量，在页面中全局变量为window*


所以，可以使用如下安全模式：

```
//图书安全类
var Book = function(title,time,type){
//判断执行过程中this是否是当前这个对象（如果是说明是用new创建的）
	if(this instanceof Book){
		this.title = title;
		this.time = time;
		this.type = type;
		//否则重新创建这个对象
	}else{
		return new Book(title,time,type);
	}
}
var book= Book('javascript','2014','js');


//
输出结果：
console.log(book); //Book
console.log(book.title); //javascript
console.log(book.time); //2014
console.log(book.type); //js
console.log(window.title); //undefined
console.log(window.time); //undefined
console.log(window.type); //undefined
```
转载请注明：[化风的博客](http://ChhXin.github.io) » [javascript设计模式之new创建对象的安全模式](/2017/03/javascript设计模式之new创建对象的安全模式/)  
