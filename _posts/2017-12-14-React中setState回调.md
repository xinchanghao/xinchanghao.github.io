---
layout: post
title: "React中setState回调"
date: 2017-12-14
description: "React中setState回调"
tag: React
---

﻿####在使用React过程中，中可以使用this.state来访问需要的某些状态，但是需要更新或者修改state时，一般而言，我们都会使用setState()函数，从而达到更新state的目的，setState()函数执行会触发页面重新渲染UI。但是！！！setState是异步的！！！

**1. 语法：**
```
setState(updater[, callback])  //
```
updater是要改变的state对象，callback是state导致的页面重新渲染的回调，等价于componentDidUpdate

**2. 区别：**

```
//不使用回调
this.state = {preState: false};
this.setState({preState: true});
console.log(this.state.preState); // false

//使用回调
this.state = {newState: false};
this.setState(
	{
		newState: true
	}, ()=> {
	    console.log(newState); // true
	}
);
```

**3. 注意：**

>  - 刚说了，setState是异步的！不保证数据的同步。
>  - setState更新状态时可能会导致页面不必要的重新渲染，影响加载。
>  - setState管理大量组件状态也许会导致不必要的生命周期函数钩子调用。

转载请注明：[化风的博客](http://xinchanghao.github.io) » [React中setState回调](/2017/12/React中setState回调/)  
