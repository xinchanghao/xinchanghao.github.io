---
layout: post
title: "jQuery中filter()和find()和has()的用法区别"
date: 2017-03-12
description: "jQuery中filter()和find()和has()的用法区别"
tag: jQuery
---

﻿###filter()、find()、has()是jquery中常用得三个函数，用法以及效果截然不同，对于初学者来说，往往分不清楚，关于三个函数的用法，简单解释一下。

####filter()方法

-filter()过滤DOM元素包装集，是指操作**当前元素集**，删除不匹配的元素，得到一个新的集合。

```
<ul>
  <li class="a">list item 1</li>
  <li>list item 2
    <ul>
      <li><div><span>a</span></div>list item 2-a</li>
      <li>list item 2-b</li>
    </ul>
  </li>
  <li>list item 3</li>
  <li>list item 4</li>
</ul>


<script>
$('li').filter('.a').css('background-color', 'red');
</script>
```
![作用于自身元素](http://img.blog.csdn.net/20170312121753247?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
filter()方法与has()方法中的参数，都是过滤条件。不同的是filter()方法，条件作用于自身；has()方法条件是作用于它的后代元素中。

####has()方法
-has(selector选择器或DOM元素)   将匹配元素集合根据选择器或DOM元素为条件，检索该条件在每个元素的后代中是否存在，将符合条件的的元素构成新的结果集。

```
<ul>
  <li>list item 1</li>
  <li>list item 2
    <ul>
      <li><div><span>a</span></div>list item 2-a</li>
      <li>list item 2-b</li>
    </ul>
  </li>
  <li>list item 3</li>
  <li>list item 4</li>
</ul>
<script>
$('li').has('span').css('background-color', 'red');
</script>
```
![作用于自身元素](http://img.blog.csdn.net/20170312122725385?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

has只起判断作用。以has参数中的选择器或DOM元素做为条件，检测原结果集中的元素是否符合。去掉不符合的元素，将符合的元素构成一个新结果集。

####find()方法
-find()在当前选中元素的上下文中找到符合条件的后代，返回的是子元素

```
<ul>
  <li>list item 1</li>
  <li>list item 2
    <ul>
      <li><div><span>a</span></div>list item 2-a</li>
      <li>list item 2-b</li>
    </ul>
  </li>
  <li>list item 3</li>
  <li>list item 4</li>
</ul>
<script>
$('li').find('span').css('background-color', 'red');
</script>
```
![作用于选中元素](http://img.blog.csdn.net/20170312122944372?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
-find()方法是获得在当前结果集中每个元素的后代。参数（选择器、jquery集合或DOM元素）做为过滤条件，满足过滤条件的则保留，保留的是后代。而has()方法中，参数只做为条件，符合条件的，它的前元素加入新的结果集，而不是后代加入新的结果集。

####由上面可以看出，filter()是对**选中的元素**集合操作，得到这些元素中符合条件的元素，而find()是得到**选中元素** ，作用于后代，has()则是作为判断，作用于自身。

 - end（）方法

- end()函数用于返回最近一次"破坏性"操作之前的jQuery对象。当前jQuery对象可能是通过调用之前的jQuery对象的特定方法创建的，使用该函数可以返回之前的jQuery对象。

```
$('div').filter('.div1').end();//返回的是使用filter()之前的选择元素，即$('div')
```
- 只要调用jQuery对象的某个方法返回的是一个新创建的jQuery对象，则该操作被视为"过滤"操作或"破坏性"操作。jQuery对象的add()、 addBack()、 andSelf()、 children()、 closest()、 contents()、 eq()、 filter()、 find()、 first()、 has()、 last()、 map()、 next()、 nextAll()、 nextUntil()、 not()、 parent()、 parents()、 parentsUntil()、 prev()、 prevAll()、 prevUntil()、 siblings()、 slice()、 clone()等方法均属于"破坏性"操作。

```
<p id="n1">
    <span id="n2">A
        <span id="n3">B</span>
    </span>
    <span id="n4">C
        <label id="n5">D</label>        
    </span>
</p>


<script>
var $p = $("p");
//这是一个破坏性操作，返回一个新的jQuery对象
var $p_span = $p.find("span");
document.writeln( $p_span.end() === $p ); // true

//这不是一个破坏性操作，css()和attr()返回的都是原jQuery对象，并没有创建一个新的jQuery对象
var $me = $p.css("color", "#333").attr("uid", "12");
document.writeln( $me.end() === $p ); // false
// $me和$p是同一个jQuery对象
document.writeln( $me === $p ); // true

var $span = $("span");
// 这是一个破坏性操作，虽然没有过滤掉任何元素，但返回的是一个新的jQuery对象
var $newSpan = $span.not(".foo");
document.writeln( $newSpan.end() === $span ); // true

// 如果之前没有破坏性操作，可能返回包含document的jQuery对象或空的jQuery对象(视具体情况而定)
document.writeln( $("label").end().length ); // 1 (document对象)
document.writeln( $("#n1").end().length ); // 0
</script>
```

转载请注明：[化风的博客](http://xinchanghao.github.io) » [jQuery中filter()和find()和has()的用法区别](/2017/03/jQuery中filter()和find()和has()的用法区别/)  
