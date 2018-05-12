---
layout: post
title: "javascript之事件捕获和事件冒泡"
date: 2018-04-06
description: "javascript之事件捕获和事件冒泡"
tag: JavaScript
---

﻿#### 1. 事件阶段
**事件分为三个阶段：捕获阶段、目标阶段和冒泡阶段。**

捕获阶段：

> 事件从文档的根节点流向目标对象节点。途中经过各个层次的DOM节点，并在各节点上触发捕获事件，直到到达事件的目标节点，主要任务是建立传播路径。

目标阶段：
> 事件到达目标节点，事件就进入目标阶段。事件在目标节点上被触发，然后会逆向回流，直到传播至最外层的文档节点。

冒泡阶段：
> 事件在目标元素上触发后，并不在这个元素上终止。它会随着DOM树一层层向上冒泡，回溯到根节点

#### 2.事件捕获

```
<!-- 事件捕获 -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>事件捕获</title>
</head>

<body>
    <div>
        <input type="button" value="测试事件捕获" />
    </div>
</body>
</html>
<script>
var $input = document.getElementsByTagName("input")[0];
var $div = document.getElementsByTagName("div")[0];
var $body = document.getElementsByTagName("body")[0];

$input.addEventListener("click", function(){
    this.style.border = "5px solid red";
    console.log("red")
}, true)

$div.addEventListener("click", function(){
    this.style.border = "5px solid green";
    console.log("green")
}, true)

$body.addEventListener("click", function(){
    this.style.border = "5px solid yellow";
    console.log("yellow")
}, true)
</script>
```
![这里写图片描述](https://img-blog.csdn.net/20180406143659429?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

> 如上效果，与事件冒泡相反，依次打印出"yellow"，"green"，"red"。 这就是事件捕获，从文档最上层开始，自上而下。

#### 3. 事件冒泡
> 事件捕获指的是从document到触发事件的那个节点，即自上而下的去触发事件。相反的，事件冒泡是自下而上的去触发事件。绑定事件方法的第三个参数，就是控制事件触发顺序是否为事件捕获。true,事件捕获；false,事件冒泡。默认false,即事件冒泡。

```
<!-- 事件冒泡 -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>事件捕获和事件冒泡</title>
</head>

<body>
    <div>
        <input type="button" value="测试事件冒泡" />
    </div>
</body>
</html>
<script>
var $input = document.getElementsByTagName("input")[0];
var $div = document.getElementsByTagName("div")[0];
var $body = document.getElementsByTagName("body")[0];

$input.onclick = function(e){
      this.style.border = "5px solid red";
      var e = e || window.e;
      console.log("red");
}

$div.onclick = function(e){
      this.style.border = "5px solid green";
      console.log("green");
}

$body.onclick = function(e){
      this.style.border = "5px solid yellow";
      console.log("yellow");
}
</script>

```

![这里写图片描述](https://img-blog.csdn.net/20180406142928511?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

> 如上效果，依次打印出"red"，"green"，"yellow"。 事件冒泡就是如此，触发button这个元素，却连同父元素绑定的事件一同触发，自下而上。
#### 4.阻止事件冒泡

为什么要阻止事件冒泡？

> 有种可能是，某个DOM节点绑定了某事件监听器，本来是想当该DOM节点触发事件，才会执行回调函数。结果是该节点的某后代节点触发某事件，由于事件冒泡，该DOM节点事件也会触发，执行了回调函数，这样就违背了最初的本意了。

依旧是上述的例子，在input的时间中阻止事件冒泡，可作如下修改：
```
//阻止事件冒泡
$input.onclick = function(e){
    this.style.border = "5px solid red"
    var e = e || window.e;
    alert("red")
    e.stopPropagation();
}
```

![这里写图片描述](https://img-blog.csdn.net/20180406144334386?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


> 在正常的开发过程中，如果想要阻止事件的传播，通过一个方法实现。在IE模型中，你必须设置事件的cancelBubble的属性为true。在w3c模型中你必须调用事件的stopPropagation()方法。

```
function doSomething(e) {
  if (!e) {
    var e = window.event;
    e.cancelBubble = true;
  }
  if (e.stopPropagation) {
    e.stopPropagation();
  }
}
```


关于事件委托移步：[javascript之事件委托](https://blog.csdn.net/haoaiqian/article/details/71910802)

转载请注明：[化风的博客](http://xinchanghao.github.io) » [javascript之事件捕获和事件冒泡](/2018/04/javascript之事件捕获和事件冒泡/)  
