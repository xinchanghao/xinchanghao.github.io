---
layout: post
title: "javascript中数据类型值的存储方式和位置"
date: 2017-06-01
description: "javascript中数据类型值的存储方式和位置"
tag: JavaScript
---

﻿###JavaScript有两种类型的值，内存图如下：

栈：原始数据类型（Undefined，Null，Boolean，Number、String）

 堆：引用数据类型（对象、数组和函数）

 ![栈和堆的存储方式](http://img.blog.csdn.net/20170601193306176?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

###**区别：**

两种类型的区别是：存储位置不同；

###**原始数据类型**

 原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；

###**引用数据类型**

 1. 引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；

 2. 引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体

 转载请注明：[化风的博客](http://ChhXin.github.io) » [javascript中数据类型值的存储方式和位置](/2017/06/javascript中数据类型值的存储方式和位置/)  
