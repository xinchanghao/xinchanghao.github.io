---
layout: post
title: "javascript中的parseInt()-toString()进制转换问题"
date: 2017-03-26
description: "javascript中的parseInt()-toString()进制转换问题"
tag: JavaScript
---
﻿parseInt方法可以将其它进制转换为十进制，只需要给该方法传入需要转换的字符串和该字符串的进制表示两个参数即可。 注意！！！***parseInt方法的可选参数是操作数的进制说明，不是目标的进制。***数字转字符用toString()，字符转数字用parseInt() or parse(float).

**1.   toString()函数**
toString()函数用于将当前对象以字符串的形式返回

该方法属于Object对象,所有的对象都是Object的实例,所以,所有几乎所有的实例对象都有toString()方法,而有所区别.

**用法**

object.toString()

**返回值**

toString()函数的返回值都是String 类型,返回当前对象的字符串形式.

**类型 	         行为描述**

Array 	将 Array 的每个元素转换为字符串，并将它们依次连接起来，两个元素之间用英文逗号作为分隔符进行拼接。
Boolean 	如果布尔值是true,则返回"true",否则返回"false"
Date  	返回日期的文本表示
Error 	返回一个包含相关错误信息的字符串
Function 	返回如下格式的字符串,"function functionname() { [native code] }"
Number 	返回数值的自负产表示,参数表示转化成几进制,toString(2)返回二进制的字符串
String 	返回String对象的值
Object(默认) 	返回"[object ObjectName]"，其中 ObjectName 是对象类型的名称。


**1.   parseInt(string, radix)函数**

string 是必须的，要被解析的字符串。
radix  可选。表示要解析的数字的基数。该值介于 2 ~ 36 之间。

**用法**

如果省略该参数或其值为 0，则数字将以 10 为基础来解析。

如果它以 “0x” 或 “0X” 开头，将以 16 为基数。

如果该参数小于 2 或者大于 36，则 parseInt() 将返回 NaN。

**parseInt(number,type)这个函数后面如果不跟第2个参数来表示进制的话，默认是10进制。**
比如说parseInt("010",10)就是10进制的结果：10;  parseInt("010",2)就是2进制的结果：2;    parseInt("010",8)就是8进制的结果：8;   parseInt("010",16)就是16进制的结果：16


**如果是第一位不是0就遇到字母就停止解析，并把字母前面的值作为10进制去解析，如果第一个就是字母那么值就是空，空成了NaN,比如:**
parseInt("a")==>parseInt("",10)==>NaN.

parseInt("10a")==>parseInt("10")==>parseInt("10",10)==>10;

**如果第一位是0,且第2位不是x也和上面一样遇到字母就停止解析，并把字母前面的值作为8进制去解析,比如:**

parseInt("0a")==>parseInt("0")==>parseInt("0",10)==>0.

*PS:这个有点特殊，因为0a被解析成了0，还不具备看做是8进制的结构，下面那个就明显了。*

parseInt("010a")==>parseInt("010")==>parseInt("10",8)==>8;

**如果第一位是0,且第2位是x那后面也和上面一样遇到字母就停止解析，并把字母前面的值作为16进制去解析,比如:**

parseInt("0xt")==>parseInt("",16)==>NaN.

parseInt("0x12t")==>parseInt("12",16)==>18.

转载请注明：[化风的博客](http://xinchanghao.github.io) » [javascript中的parseInt()-toString()进制转换问题](/2017/03/javascript中的parseInt()-toString()进制转换问题/)  
