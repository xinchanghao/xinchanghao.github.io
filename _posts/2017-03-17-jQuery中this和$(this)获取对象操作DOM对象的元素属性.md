---
layout: post
title: "jQuery中this和$(this)获取对象操作DOM对象的元素属性"
date: 2017-03-17
description: "jQuery中this和$(this)获取对象操作DOM对象的元素属性"
tag: jQuery
---

```
 $("#row a img").each(function(index){
            alert($(this));
            alert(this);

 }

 //可以看出来$(this)是jquery对象，而this是DOM对象：

 alert($(this));  弹出的结果是[object Object ]

 alert(this);        弹出来的是[object HTMLImageElement]
```

####如何获取$(this)子对象？find( )
```
$("#row a ").each(function(index){

         var imgurl=$(this).find('img').attr('src');

         alert(imgurl);
        }
```

*.find(element) 是返回一个用于匹配元素的DOM元素

####如何获取元素的属性或赋值？
方法一：
```
<script type="text/javascript">
$(document).ready(function(){
  $("#add").click(function(){
    $("div").data("mydata","我是data()方法");//为mydata赋值
  })
  $("#show").click(function(){
    $("div").text($("div").data("mydata"));//获取mydata属性
  })
})
</script>
```

方法二：

```
返回文档中所有图像的src属性值。

    # jQuery 代码:
    $("img").attr("src");

为所有图像设置src和alt属性。

    # jQuery 代码:
    $("img").attr({ src: "test.jpg", alt: "Test Image" });

为所有图像设置src属性。

    # jQuery 代码:
    $("img").attr("src","test.jpg");

把src属性的值设置为title属性的值。

    # jQuery 代码:
    $("img").attr("title", function() { return this.src });
```

转载请注明：[化风的博客](http://ChhXin.github.io) » [jQuery中this和$(this)获取对象操作DOM对象的元素属性](/2017/02/jQuery中this和$(this)获取对象操作DOM对象的元素属性/)
