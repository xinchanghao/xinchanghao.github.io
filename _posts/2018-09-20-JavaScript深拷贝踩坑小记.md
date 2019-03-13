---
layout: post
title: "JavaScript深拷贝踩坑小记"
date: 2018-09-20
description: "JavaScript深拷贝踩坑小记"
tag: JavaScript
---

## 一、关于深拷贝

关于深拷贝与浅拷贝的理解，这里不过多的进行描述。因为此前自己总结了一次深拷贝的常见方法，`JSON.parse(JSON.stringify(obj))，jQury的$.extend(true,{},obj)，for...in加递归`已经满足了大部分的业务需求。具体内容可见[JavaScript之深拷贝和浅拷贝](https://blog.csdn.net/haoaiqian/article/details/78041384)


## 二、然而，有坑！

```
const data = {
    arr: ["test0", "test1"],
    obj: { title: '标题', dataIndex: "name" },
    onClick: () => { console.log('this is a function') },
    date: new Date(),
}

const deepCopy = function (obj) {
    var str, newobj = obj.constructor === Array ? [] : {};
    if (typeof obj !== 'object') {
        return;
    } else if (window.JSON) {
        str = JSON.stringify(obj), //json对象转字符串，系列化
            newobj = JSON.parse(str); //还原为json对象
    } else {
        for (var i in obj) {
            newobj[i] = typeof obj[i] === 'object' ?
                deepCopy(obj[i]) : obj[i];
        }
    }
    return newobj;
};


const result1 = JSON.parse(JSON.stringify(data));
const result2 = deepCopy(data);

console.log('------------------------------------');
console.log(result1);
console.log('------------------------------------');

console.log('------------------------------------');
console.log(result2);
console.log('------------------------------------');
```
由以下输出结果，可以看出，这两个方法无法拷贝含有function，date属性的对象！！！
![结果](https://img-blog.csdn.net/20180920222628121?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 三、解决方案

### 1. 结构化克隆算法

```
//需要进一步完善处理对待各种类型；
const deepCopy = (obj, hash = new WeakMap()) => {
    let cloneObj
    let Constructor = obj.constructor
    switch (Constructor) {
        case Object:
            cloneObj = new Constructor(obj)
            break
        case RegExp:
            cloneObj = new Constructor(obj)
            break
        case Date:
            cloneObj = new Constructor(obj.getTime())
            break
        default:
            if (hash.has(obj)) return hash.get(obj)
            cloneObj = new Constructor()
            hash.set(obj, cloneObj)
    }
    for (let key in obj) {
        cloneObj[key] = obj[key].constructor === constructor ? deepCopy(obj[key], hash) : obj[key];
    }
    return cloneObj
}

const result3 = deepCopy(data);

console.log('------------------------------------');
console.log(result3);
console.log('------------------------------------');
```
参考MDN上的[结构化克隆算法](https://developer.mozilla.org/zh-CN/docs/Web/Guide/API/DOM/The_structured_clone_algorithm#%E7%9B%B8%E5%85%B3%E9%93%BE%E6%8E%A5)，下图为输出结果，达到预期效果：
![结果2](https://img-blog.csdn.net/2018092022471695?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 2. lodash中的_.cloneDeep() 方法
目前是我觉得最直接，最完整的解决方案，毕竟可以一句话解决问题，绝不多说一句（其实我就是懒）。
![lodash](https://img-blog.csdn.net/20180920225427841?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[lodash中文文档](https://www.lodashjs.com/)
 [1]: https://juejin.im/post/5b235b726fb9a00e8a3e4e88

转载请注明：[化风的博客](http://ChhXin.github.io) » [JavaScript深拷贝踩坑小记](/2018/09/JavaScript深拷贝踩坑小记/)                   
