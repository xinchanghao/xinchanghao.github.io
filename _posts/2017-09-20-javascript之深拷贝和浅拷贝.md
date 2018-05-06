---
layout: post
title: "javascript之深拷贝和浅拷贝"
date: 2017-09-20
description: "javascript之深拷贝和浅拷贝"
tag: JavaScript
---

JS 中的浅拷贝与深拷贝，只是针对复杂数据类型（Object，Array）的复制问题。浅拷贝与深拷贝都可以实现在已有对象上复制新的出来。对象的实例是存储在堆内存中然后通过一个引用值去操作对象，也就是说，浅拷贝和深拷贝的区别也就是：拷贝引用和拷贝实例。


### 浅拷贝

拷贝原对象的引用

```
var a = {c:1};
var b = a;
console.log(a === b); // 输出true。
a.c = 2;
console.log(b.c); // 输出 2
```

### 深拷贝

深拷贝后，两个对象内部的元素互不干扰。常见方法JSON.parse(),JSON.stringify()，jQury的$.extend(true,{},obj)，lodash的_.cloneDeep和_.clone(value, true)。
```
//深拷贝 对象和数组
var deepCopy = function(obj){
    var str, newobj = obj.constructor === Array ? [] : {};
    if(typeof obj !== 'object'){
        return;
    } else if(window.JSON){
        str = JSON.stringify(obj), //json对象转字符串，系列化
        newobj = JSON.parse(str); //还原为json对象
    } else {
        for(var i in obj){
            newobj[i] = typeof obj[i] === 'object' ?
            deepCopy(obj[i]) : obj[i];
        }
    }
    return newobj;
};
```

转载请注明：[化风的博客](http://xinchanghao.github.io) » [javascript之深拷贝和浅拷贝](/2017/09/javascript之深拷贝和浅拷贝/)            
