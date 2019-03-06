---
layout: post
title: "原生javascript实现ajax异步请求"
date: 2017-05-15
description: "原生javascript实现ajax异步请求"
tag: JavaScript
---

﻿```
<script type="text/javascript">
 //格式化post参数,此为post请求操作时使用,get无需使用
            function formatParams(jsonData) {
                var arr = [];
                for (var name in jsonData) {
                    arr.push(encodeURIComponent(name) + "=" + encodeURIComponent(jsonData[name]));
                }
                arr.push(("v=" + Math.random()).replace(".",""));
                return arr.join("&");
            }

    //创建XHR对象
    function createXMLHttp() {
        var XmlHttp;
        if (window.ActiveXObject) {
            //IE6及其之前的版本中，XHR对象是通过MSXML库中的一个ActiveX对象实现的。
            // 《js高级程序设计中》细化了IE中此类对象的不同版本，即MSXML2.XMLHttp、MSXML2.XMLHttp.3.0 和 MSXML2.XMLHttp.6.0；
            // !!!可以直接使用下面的语句创建： var XmlHttp=new ActiveXObject(’Microsoft.XMLHTTP’);
            var arr = ["MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.5.0", "MSXML2.XMLHttp.4.0", "MSXML2.XMLHttp.3.0", "MSXML2.XMLHttp", "Microsoft.XMLHttp"];
            for (var i = 0; i < arr.length; i++) {
                try {
                    XmlHttp = new ActiveXObject(arr[i]);
                    return XmlHttp;
                }
                catch (error) { }
            }
        }
        else {
            try {
                XmlHttp = new XMLHttpRequest();
                return XmlHttp;
            }
            catch (otherError) { }
        }
    }
    //js原生ajax请求方法
    window.onload=function xmlPost() {
        var xmlHttp = createXMLHttp();
        //POST请求
        //var data = formatParams(jsondata);
        var url = "www.example.com";
        xmlHttp.open('get', url, true);
        xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        //如果open方法用的是post方式，相应的参数要写到send方法里
        xmlHttp.send();
        xmlHttp.onreadystatechange = function () {
            if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
                var data =eval("(" + xmlHttp.responseText + ")");
                return data;
            }
        }
    }
</script>

```
转载请注明：[化风的博客](http://ChhXin.github.io) » [原生javascript实现ajax异步请求](/2017/05/原生javascript实现ajax异步请求/)  
