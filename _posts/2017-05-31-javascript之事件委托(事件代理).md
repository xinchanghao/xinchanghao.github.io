---
layout: post
title: "javascript之事件委托(事件代理)"
date: 2017-05-31
description: "javascript之事件委托(事件代理)"
tag: JavaScript
---

﻿JavaScript高级程序设计上讲：事件委托就是利用事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。也就是：利用冒泡的原理，把事件加到父级上，触发执行效果。

**事件委托的原理：**

事件委托是利用事件的冒泡原理来实现的，何为事件冒泡呢？就是事件从最深的节点开始，然后逐步向上传播事件，举个例子：页面上有这么一个节点树，div>ul>li>a;比如给最里面的a加一个click点击事件，那么这个事件就会一层一层的往外执行，执行顺序a>li>ul>div，有这样一个机制，那么我们给最外面的div加点击事件，那么里面的ul，li，a做点击事件的时候，都会冒泡到最外层的div上，所以都会触发，这就是事件委托，委托它们父级代为执行事件。


demo1：需要触发每个li来进行事件
```
<ul id="list">
     <li id="do1">111</li>
     <li id="do2">222</li>
     <li id="do3">333</li>
     <li id="do4">444</li>
</ul>
```

```
//demo1：需要触发每个li来进行事件
window.onload = function() {
    var list = document.getElementById("list");
    list.onmouseover = function (ev) {
        //ie：window.event.srcElement
        //标准下:event.target
        var even = ev || window.event;
        var target = even.target || even.srcElement;
        switch (target.id) {
            case "do1":
                target.style.background = "red";
                break;
            case "do2":
                target.style.background = "yellow";
                break;
            case "do3":
                window.location.href = "#";
                break;
            case  "do4":
                document.title = "this is a demo"
                break;
        }
    }
}
```


demo2:动态的添加li,点击button动态添加li
```

 <input type="button" id="btn"/>
 <ul id="list">
 <li >111</li>
 <li >222</li>
 <li >333</li>
 <li >444</li>
 </ul>

```

```
//demo2:动态的添加li,点击button动态添加li
window.onload = function(){
    var oBtn = document.getElementById("btn");
    var list = document.getElementById("list");
    var listson = list.getElementsByTagName('li');
    var num = 4;

    //事件委托，添加的子元素也有事件
    list.onmouseover = function(ev){
        var ev = ev || window.event;
        var target = ev.target || ev.srcElement;
        if(target.nodeName.toLowerCase() == 'li'){
            target.style.background = "red";
        }

    };
    list.onmouseout = function(ev){
        var ev = ev || window.event;
        var target = ev.target || ev.srcElement;
        if(target.nodeName.toLowerCase() == 'li'){
            target.style.background = "#fff";
        }

    };

    //添加新节点
    oBtn.onclick = function(){
        num++;
        var newlist = document.createElement('li');
        newlisti.innerHTML = 111*num;
        list.appendChild(oLi);
    };
}
```

关于事件捕获和事件冒泡移步：[javascript之事件捕获和事件冒泡](https://blog.csdn.net/haoaiqian/article/details/79833510)


转载请注明：[化风的博客](http://ChhXin.github.io) » [javascript之事件委托(事件代理)](/2017/05/javascript之事件委托(事件代理)/)  
