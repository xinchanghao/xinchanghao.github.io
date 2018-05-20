---
layout: post
title: "javascript中使用迭代操作数组替代for循环map-filter-some-every-reduce-find"
date: 2017-11-22
description: "javascript中使用迭代操作数组替代for循环map-filter-some-every-reduce-find"
tag: JavaScript
---

**1.Map**

map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

语法：

```
let new_array = arr.map(function callback(currentValue, index, array) {
    // Return element for new_array
}[, thisArg])
```

参数：

> callback 生成新数组元素的函数，使用三个参数：
> 	> currentValue callback 的第一个参数，数组中正在处理的当前元素。
> 	> index callback 的第二个参数，数组中正在处理的当前元素的索引。
> 	> array callback 的第三个参数，map方法被调用的数组。
>
> thisArg 可选的。执行 callback 函数时 使用的this 值

示例：
```
var kvArray = [{key: 1, value: 10},
               {key: 2, value: 20},
               {key: 3, value: 30}];

var reformattedArray = kvArray.map(function(obj) {
   var rObj = {};
   rObj[obj.key] = obj.value;
   return rObj;
});

// reformattedArray 数组为： [{1: 10}, {2: 20}, {3: 30}],

// kvArray 数组未被修改:
// [{key: 1, value: 10},
//  {key: 2, value: 20},
//  {key: 3, value: 30}]
```

**2.Filter**

filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。

语法:

```
var new_array = arr.filter(callback[, thisArg])
```

参数:

> callback 用来测试数组的每个元素的函数。调用时使用参数 (element, index, array)。返回true表示保留该元素（通过测试），false则不保留。
> thisArg 可选。执行 callback 时的用于 this 的值。

示例：

```
function isBigEnough(element) {
  return element >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
// filtered is [12, 130, 44]
```

```
/*filter实现数组去重*/
r=arr.filter(function(item,index,self){
    return self.indexOf(item) == index;
});
```

**3.Some**

some 为数组中的每一个元素执行一次 callback 函数，直到找到一个使得 callback 返回一个“真值”（即可转换为布尔值 true 的值）。如果找到了这样一个值，some 将会立即返回 true。否则，some 返回 false。

语法：

```
arr.some(callback[, thisArg])
```

参数：

> callback 用来测试每个元素的函数。
> thisArg 执行 callback 时使用的 this 值。

示例：
```
//检测数组中是否有元素大10
function isBigEnough(element, index, array) {
  return (element >= 10);
}
var passed = [2, 5, 8, 1, 4].some(isBigEnough);
// passed is false
passed = [12, 5, 8, 1, 4].some(isBigEnough);
// passed is true
```

**4.Every**

every 方法为数组中的每个元素执行一次 callback 函数，直到它找到一个使 callback 返回 false（表示可转换为布尔值 false 的值）的元素。如果发现了一个这样的元素，every 方法将会立即返回 false。否则，callback 为每一个元素返回 true，every 就会返回 true。callback 只会为那些已经被赋值的索引调用。不会为那些被删除或从来没被赋值的索引调用。

语法:

```
arr.every(callback[, thisArg])
```

参数:

> callback 用来测试每个元素的函数。
> thisArg 执行 callback 时使用的 this 值。

示例：

```
//检测数组中的所有元素是否都大于 10。
function isBigEnough(element, index, array) {
  return (element >= 10);
}
var passed = [12, 5, 8, 130, 44].every(isBigEnough);
// passed is false
passed = [12, 54, 18, 130, 44].every(isBigEnough);
// passed is true
```

**5.Reduce**

reduce为数组中的每一个元素依次执行callback函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：accumulator 、currentValue 、currentIndex 、array回调函数第一次执行时，accumulator 和currentValue的取值有两种情况：调用reduce时提供initialValue，accumulator取值为initialValue，currentValue取数组中的第一个值；没有提供 initialValue，accumulator取数组中的第一个值，currentValue取数组中的第二个值。

语法:

```
arr.reduce(callback[, initialValue])
```

参数:

> callback 执行数组中每个值的函数，包含四个参数：
> >  accumulator 累加器累加回调的返回值; 它是上一次调用回调时返回的累积值，或initialValue（如下所示）。
> >  currentValue 数组中正在处理的元素。
> >  currentIndex 数组中正在处理的当前元素的索引。如果提供了initialValue，则索引号为0，否则为索引为1。
> >  array 调用reduce的数组

> initialValue [可选]用作第一个调用 callback的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。

示例：

```
var total = [0, 1, 2, 3].reduce(function(sum, value) {
  return sum + value;
}, 0);
// total is 6

var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
  return a.concat(b);
}, []);
// flattened is [0, 1, 2, 3, 4, 5]
```

**6.find和findIndex**

 find() 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。

findIndex()方法返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。

```
function isBigEnough(element) {
  return element >= 15;
}

[12, 5, 8, 130, 44].find(isBigEnough); // 130
```

```
function isBigEnough(element) {
  return element >= 15;
}

[12, 5, 8, 130, 44].findIndex(isBigEnough); // 3
```

转载请注明：[化风的博客](http://xinchanghao.github.io) » [javascript中使用迭代操作数组替代for循环map-filter-some-every-reduce-find](/2017/11/javascript中使用迭代操作数组替代for循环map-filter-some-every-reduce-find/)  
