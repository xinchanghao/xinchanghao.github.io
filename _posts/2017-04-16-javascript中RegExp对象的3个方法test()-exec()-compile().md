---
layout: post
title: "javascript中RegExp对象的3个方法test()-exec()-compile()"
date: 2017-04-16
description: "javascript中RegExp对象的3个方法test()-exec()-compile()"
tag: JavaScript
---

﻿**1. test() 方法**
test() 方法用来检测一个字符串是否匹配某个正则表达式，如果匹配成功，返回 true ，否则返回 false。

语法：

```
    RegExpObject.test(string)    //必需参数。要检测的字符串。
```

**2. exec() 方法**
exec() 方法用来检索字符串中与正则表达式匹配的值。

exec() 方法返回一个数组，其中存放匹配的结果。如果未找到匹配的值，则返回 null。

语法：

```
    RegExpObject.exec(string)//必需参数。要检索的字符串。
```

与 test() 方法相比，exec() 方法更加强大，功能也更加复杂。

当 exec() 找到了匹配的文本时，会返回一个结果数组。否则，返回 null。此数组的第 0 个元素是与正则表达式相匹配的文本，第 1 个元素是与 RegExpObject 的第 1 个子表达式相匹配的文本（如果有的话），第 2 个元素是与 RegExpObject 的第 2 个子表达式相匹配的文本（如果有的话），以此类推。

除了数组元素和 length 属性之外，exec() 方法还返回两个属性。index 属性声明的是匹配文本的第一个字符的位置。input 属性则存放的是被检索的字符串 string。

但是，当 RegExpObject 是一个全局正则表达式（带有 g 修饰符）时，exec() 的行为就稍微复杂一些，它会在 RegExpObject 的 lastIndex 属性指定的字符处开始检索字符串 string。当 exec() 找到了与表达式相匹配的文本时，在匹配后，它将把 RegExpObject 的 lastIndex 属性设置为匹配文本的最后一个字符的下一个位置。

也就是说，可以通过反复调用 exec() 方法来遍历字符串中的所有匹配文本。当 exec() 再也找不到匹配的文本时，它将返回 null，并把 lastIndex 属性重置为 0。

```
var str = "Itxueyuan's domain is www.itxueyuan.org. Welcome to itxueyuan !";
var pattern = new RegExp("itxueyuan", "ig");
var i=1;
var result;
while(result=pattern.exec(str)){
    alert(
        "第 "+i+" 次匹配的字符串："+result[0]+"\n"+
         "所匹配的字符的起始位置："+result.index+"\n"+
         "第 "+(++i)+" 次匹配的的起始位置："+pattern.lastIndex
    );
}
```
**3.compile() 方法**
compile() 方法可以在脚本执行过程中编译正则表达式，也可以改变已有表达式。

语法：

```
    RegExpObject.compile(regexp,modifier)
```

参数说明：

```
regexp 	//正则表达式;
modifier 	//规定匹配的类型。"g" 用于全局匹配，"i" 用于区分大小写，"gi" 用于全局区分大小写的匹配。
```

例如，在字符串中全局搜索 "man"，并用 "person" 替换。然后通过 compile() 方法，改变正则表达式，用 "person" 替换 "man" 或 "woman"，：

```
var str="Every man in the world! Every woman on earth!";
patt=/man/g;
str2=str.replace(patt,"person");
document.write(str2+"<br />");

patt=/(wo)?man/g;
patt.compile(patt);
str2=str.replace(patt,"person");
document.write(str2);
```
转载请注明：[化风的博客](http://xinchanghao.github.io) » [javascript中RegExp对象的3个方法test()-exec()-compile()](/2017/04/javascript中RegExp对象的3个方法test()-exec()-compile()/)
