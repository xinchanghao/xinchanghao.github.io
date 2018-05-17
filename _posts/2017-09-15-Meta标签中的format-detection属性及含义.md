---
layout: post
title: "Meta标签中的format-detection属性及含义"
date: 2017-09-15
description: "Meta标签中的format-detection属性及含义"
tag: 移动端web
---

﻿format-detection翻译成中文的意思是“格式检测”，顾名思义，它是用来检测html里的一些格式的，那关于meta的format-detection属性主要是有以下几个设置：

```
meta name="format-detection" content="telephone=no"
meta name="format-detection" content="email=no"
meta name="format-detection" content="adress=no"
//也可以连写：
meta name="format-detection"content="telephone=no,email=no,adress=no"
```

下面具体说下每个设置的作用：

一、telephone
你明明写的一串数字没加链接样式，而iPhone会自动把你这个文字加链接样式、并且点击这个数字还会自动拨号！想去掉这个拨号链接该如何操作呢？这时我们的meta又该大显神通了，代码如下：
telephone=no就禁止了把数字转化为拨号链接！
telephone=yes就开启了把数字转化为拨号链接，要开启转化功能，这个meta就不用写了,在默认是情况下就是开启！

二、email
告诉设备不识别邮箱，点击之后不自动发送
email=no禁止作为邮箱地址！
email=yes就开启了把文字默认为邮箱地址，这个meta就不用写了,在默认是情况下就是开启！

三、adress
adress=no禁止跳转至地图！
adress=yes就开启了点击地址直接跳转至地图的功能,在默认是情况下就是开启！

[参考](http://blog.sina.com.cn/s/blog_51048da70101cgea.html)

转载请注明：[化风的博客](http://xinchanghao.github.io) » [Meta标签中的format-detection属性及含义](/2017/09/Meta标签中的format-detection属性及含义/)  
