---
layout: post
title: "XHR异步上传文件报错： “the request was rejected because no multipart boundary was found”"
date: 2017-12-04
description: "XHR异步上传文件报错： “the request was rejected because no multipart boundary was found”"
tag: JavaScript
---

﻿####js 使用异步上传文件时报错：

![报错描述](http://img.blog.csdn.net/20171204162220841?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

####解决方法：

![图片描述](http://img.blog.csdn.net/20171204161850704?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


```
// 开始上传
xhr.open("POST", self.url, true);
// xhr.setRequestHeader("Content-Type","multipart/form-data");
xhr.send(formData);
```

因为请求头的"Content-Type"会自动填充，不需要单独设置header，也就是说，多此一举了。

[参考]（https://stackoverflow.com/questions/17415084/multipart-data-post-using-python-requests-no-multipart-boundary-was-found)

转载请注明：[化风的博客](http://ChhXin.github.io) » [XHR异步上传文件报错-the-request-was-rejected-because-no-multipart-boundary-was-found](/2017/12/XHR异步上传文件报错-the-request-was-rejected-because-no-multipart-boundary-was-found/)  
