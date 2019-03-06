---
layout: post
title: "WebApp中Meta标签属性及含义"
date: 2017-09-15
description: "WebApp中Meta标签属性及含义"
tag: 移动端web
---
﻿
###一、 meta 中的viewport
```
   <meta content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport">
```

> viewport即可视区域，对于桌面浏览器而言，viewport指的就是除去所有工具栏、状态栏、滚动条等等之后用于看网页的区域。代码的意思是，让viewport的宽度等于物理设备上的真实分辨率，不允许用户缩放。
>
>  1. width : 控制viewport的大小，可以指定一个值，如600，或者特殊的值，如device-width为设备的宽度（单位是缩放为100%的CSS的像素）
>  2. height : 和width相对应，指定高度
>  3. initial-scale : 初始缩放比例，页面第一次加载时的缩放比例
>  4. maximum-scale : 允许用户缩放到的最大比例，范围从0到10.0
>  5. minimum-scale : 允许用户缩放到的最小比例，范围从0到10.0
>  6. user-scalable : 用户是否可以手动缩放，值可以是：①yes、 true允许用户缩放；②no、false不允许用户缩放

###二、meta 中的name

**name 属性**

 1. ＜meta name="Generator" contect=""＞用以说明生成工具（如Microsoft FrontPage 4.0）等
 2. ＜meta name="keywords" contect=""＞向搜索引擎说明你的网页的关键词
 3. ＜meta name="Description" contect=""＞告诉搜索引擎你的站点的主要内容
 4. ＜meta name="Author" contect="你的姓名"＞告诉搜索引擎你的站点的制作的作者
 5. ＜meta name="Robots" contect="all | none | index | noindex |
    follow | nofollow"＞

> 其中的属性说明如下：
>
> 设定为all：文件将被检索，且页面上的链接可以被查询；
>
> 设定为none：文件将不被检索，且页面上的链接不可以被查询；
>
> 设定为index：文件将被检索；
>
> 设定为follow：页面上的链接可以被查询；
>
> 设定为noindex：文件将不被检索，但页面上的链接可以被查询；
>
> 设定为nofollow：文件将不被检索，页面上的链接可以被查询


###三、webapp里主要的mate用途

`<meta name="apple-touch-fullscreen" content="yes">`

> 添加到主屏幕后，全屏显示。

`<meta name="apple-mobile-web-app-capable" content="yes" />`

> 这meta的作用就是删除默认的苹果工具栏和菜单栏。content有两个值”yes”和”no”,当我们需要显示工具栏和菜单栏时，这个行meta就不用加了，默认就是显示。

`<meta name=”apple-mobile-web-app-status-bar-style” content=black” />`

> 默认值为default（白色），可以定为black（黑色）和black-translucent（灰色半透明）。 注意： 若值为“black-translucent”将会占据页面px位置，浮在页面上方（会覆盖页面20px高度–iphone4和itouch4的Retina屏幕为40px）。

`<meta name="apple-mobile-web-app-capable" content="yes">`  `<meta name="apple-mobile-web-app-status-bar style"content="black-translucent">`

> 在iOS中有两个meta值，apple-mobile-web-app-capable和apple-mobile-web-app-status-bar-style，这两个会让网页内容以应用程序风格显示，并使状态栏透明。
>

> 说明：这个link就是设置web app的放置主屏幕上icon文件路径。 图片尺寸可以设定为57*57（px）或者Retina可以定为114*114（px），ipad尺寸为72*72（px）

`<meta content="telephone=no" name="format-detection" />`

> 将不识别邮箱，告诉设备忽略将页面中的数字识别为电话号码

`<link rel="apple-touch-icon" href="/static/images/identity/HTML5_Badge_64.png" />`  
`<link rel="apple-touch-iconprecomposed" href="/static/images/identity/HTML5_Badge_64.png" />`

> iOS用rel="apple-touch-icon",android
> 用rel="apple-touch-icon-precomposed"。这样就能在用户把网页存为书签时，在手机HOME界面创建应用程序样式的图标。

`<meta name="sharecontent" data-msg-img="缩略图地址" data-msg-title="标题" data-msg-content="简介" data-msg-callBack="" data-line-img="缩略图地址" data-line-title="标题" data-line-callBack=""/>`  

>微信分享页面设置

[参考](http://www.cnblogs.com/aimyfly/p/4432121.html)

转载请注明：[化风的博客](http://ChhXin.github.io) » [WebApp中Meta标签属性及含义](/2017/09/WebApp中Meta标签属性及含义/)  
