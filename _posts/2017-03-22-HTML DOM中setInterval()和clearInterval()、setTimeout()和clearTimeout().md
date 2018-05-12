---
layout: post
title: "HTML-DOM中setInterval()和clearInterval()-setTimeout()和clearTimeout()"
date: 2017-03-22
description: "HTML-DOM中setInterval()和clearInterval()-setTimeout()和clearTimeout()"
tag: JavaScript
---
﻿###**HTML DOM setInterval()和clearInterval() 方法**


**setInterval() 方法**可按照指定的周期（以毫秒计）来调用函数或计算表达式。setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。

**clearInterval() 方法**可取消由 setInterval() 设置的 timeout。clearInterval() 方法的参数必须是由 setInterval() 返回的 ID 值。

案例一：实现一个打点计时器，要求
1、从 start 到 end（包含 start 和 end），每隔 100 毫秒 console.log 一个数字，每次数字增幅为 1
2、返回的对象中需要包含一个 cancel 方法，用于停止定时操作
3、第一个数需要立即输出
```
function count(start, end) {
    if(start <=end){
        console.log(start++);
        xin=setTimeout(function(){
            count(start,end);
        },100);
    }
    return {
        cancel:function(){
            clearTimeout(xin);
        }
    }
}
```

###**HTML DOM setInterval()和clearInterval() 方法**
**setTimeout() 方法**用于在指定的毫秒数后调用函数或计算表达式。
**clearTimeout() 方法**可取消由 setTimeout() 方法设置的 timeout。


案例二：带有停止按钮的计时器

```
<html>
<head>
<script type="text/javascript">
var c=0
var t
function timedCount()
{
	document.getElementById('txt').value=c
	c=c+1
	t=setTimeout("timedCount()",1000)
}

function stopCount()
{
	clearTimeout(t)
}
</script>
</head>

<body>
<form>
<input type="button" value="开始计时！" onClick="timedCount()">
<input type="text" id="txt">
<input type="button" value="停止计时！" onClick="stopCount()">
</form>

<p>
请点击上面的“开始计时”按钮。输入框会从 0 开始一直进行计时。点击“停止计时”可停止计时。
</p>

</body>

</html>

```
转载请注明：[化风的博客](http://xinchanghao.github.io) » [HTML-DOM中setInterval()和clearInterval()-setTimeout()和clearTimeout()](/2017/03/HTML-DOM中setInterval()和clearInterval()-setTimeout()和clearTimeout()/)  
