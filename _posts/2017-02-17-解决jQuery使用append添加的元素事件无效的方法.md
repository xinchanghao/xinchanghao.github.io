---
layout: post
title: "解决jQuery使用append添加的元素事件无效的方法"
date: 2017-02-17
description: "解决jQuery使用append添加的元素事件无效的方法"
tag: jQuery
---

###jquery api官方的例子在新增的元素上添加事件

```

$(document).on("click",'#lyysb a',function(){
  if(!$(this).hasClass('cur')){
    $(this).addClass('cur');
  } else {
    $(this).removeClass('cur');
  }
});
```
on() 方法在被选元素及子元素上添加一个或多个事件处理程序。

自 jQuery 版本 1.7 起，on() 方法是 bind()、live() 和 delegate() 方法的新的替代品。
注意：使用 on() 方法添加的事件处理程序适用于当前及未来的元素（比如由脚本创建的新元素）。

提示：如需移除事件处理程序，请使用 off() 方法。

提示：如需添加只运行一次的事件然后移除，请使用 one() 方法。


*把事件绑定在docunmet就和原来的live方法没有区别了。原先的live()方法，处理函数是默认绑定在document对象上不能变的，如果DOM嵌套结构很深，事件冒泡通过大量祖先元素会导致较大的性能损失。而使用.on()方法，事件只会绑定到$()函数的选择符表达式匹配的元素上，因此可以精确地定位到页面中的一部分，而事件冒泡的开销也可以减少。


例如我会在zkdiv中动态添加多个class="zk"的dom节点，也想对动态增加的节点绑定相同的事件则可以通过以下代码实现

```
<div id="zkdiv">
  <input type="button" value="展开" id="zk" class="zk"/> <br>
</div>
```
//展开按钮点击触发事件
```
$("#zkdiv").on("click",".zk",function(){
  console.log("on 点击一次");
});
var html2 = "<input type='button' class='zk' value='新生成的展开' />";
$("#zkdiv").append(html2);
```
*这样一来处理函数就绑定到#zkdiv的选择器上去了，事件冒泡导致的性能损失会大大降低（使用该方法时要确保.on前面的选择器能选择到对象 否则不起作用）

####click是点击事件，但是在页面加载完之后，jquery事件新添加的元素，用click的话是无法获取元素的，这个时候要用on去获取元素事件，简单的说页面加载完成时候页面显示的元素可以用on，也可以用click，但是页面加载完成之后后期再追加的元素只能用on。

转载请注明：[化风的博客](http://ChhXin.github.io) » [解决jQuery使用append添加的元素事件无效的方法](/2017/02/解决jQuery使用append添加的元素事件无效的方法/)
