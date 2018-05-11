---
layout: post
title: "Django中post提交表单时错误-CSRF-verification-failed"
date: 2017-05-27
description: "Django中post提交表单时错误-CSRF-verification-failed"
tag: Python
---
**错误描述：**

> Help Reason given for failure: CSRF cookie not set.

最近用python建站时，每次我用到CSRF（Cross Site Request Forgeries）的时候，都会报错，总结下来错误提示一般会有这么几个：

> 1、CSRF cookie not set.
> 2、 CSRF token missing or incorrect.

**其他错误大致都是这样，而导致错误的原因如下：**

 1. CSRF的饼干未设置。
 2. 当Django的CSRF的机制还没有正确使用。 对于POST表单，您需要确保：*该视图功能使用模板RequestContext的，而不是断章取义。
 3. 在模板中，没有csrftoken标志
 4. setting.py中加入'django.middleware.csrf.CsrfViewMiddleware'；

*注意：很多教程说要加入'django.middleware.csrf.CsrfResponseMiddleware',，然而当你加入时，你就会发现报错；*

**解决办法如下：**

 1. 表单里加上csrftoken

 2. 在Settings里的MIDDLEWARE_CLASSES增加配置：'django.middleware.csrf.CsrfViewMiddleware'；

 3. 在view文件中加入装饰器@csrf_exempt如下：
```
 from django.views.decorators.csrf import csrf_exempt

 @csrf_exempt
 def contact(request):
```
转载请注明：[化风的博客](http://xinchanghao.github.io) » [Django中post提交表单时错误-CSRF-verification-failed](/2017/05/Django中post提交表单时错误-CSRF-verification-failed/)  
