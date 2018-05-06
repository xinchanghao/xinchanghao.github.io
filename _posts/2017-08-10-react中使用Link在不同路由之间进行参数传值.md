---
layout: post
title: "react中使用Link在不同路由之间进行参数传值"
date: 2017-08-10
description: "react中使用Link在不同路由之间进行参数传值"
tag: React
---
﻿1.使用Link 首先需要引入Link 模块。

```
import { Link } from 'react-router'
```

2.通过 Link to设置路由跳转地址，以及需要传递的参数对象，注意，此处to 中所携带的路由和参数也是一个对象。
```
<Link to = {{
        pathname: `detail/${id}`,
        state: 'hello',
        }}>点击跳转
</Link>
```
2、to=对象，带参数跳转（pathname, query, hash, state(额外数据）），注意:这些参数都被存放到this.props.location中

```
 <li>
	 <Link to = {
		 {
			 pathname:"/jump",
			 hash:'#ahash',  
			 query:{foo: 'foo', boo:'boo'},  
			 state:{data:'hello'}   
		 }
	} activeClassName="GlobalNav-active">点击跳转
	 </Link>
 </li>
```
3、to=函数，注册到路由跳转事件中，每一次路由变化，都会执行该函数，并经最新的location作为参数。

```
<Link to={location => ({ ...location, query: { name: 'ryan' } })}>
  Hello
</Link>
```
4、不使用Link，在函数内直接操作router

旧版本：由于router只用的context传递路由信息，因此每一个组件都可以轻易的通过this.context.router获取路由

新版本：router被放置在this.props中，通过this.props.router可以获取路由

####注意：push与replace的区别，一个是添加，一个是替换，历史记录中被替换的已经不存在了，所以浏览器回退不到替换前的页面。

转载请注明：[化风的博客](http://xinchanghao.github.io) » [react中使用Link在不同路由之间进行参数传值](/2017/08/react中使用Link在不同路由之间进行参数传值/)            
