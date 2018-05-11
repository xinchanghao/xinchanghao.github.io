---
layout: post
title: "Django中CSRF防护原理及使用"
date: 2017-05-29
description: "Django中CSRF防护原理及使用"
tag: Python
---

####django为用户实现防止跨站请求伪造的功能，通过中间件 django.middleware.csrf.CsrfViewMiddleware 来完成。而对于django中设置防跨站请求伪造功能有分为全局和局部。

**全局：**

中间件 django.middleware.csrf.CsrfViewMiddleware

**局部：**

@csrf_protect，为当前函数强制设置防跨站请求伪造功能，即便settings中没有设置全局中间件。
@csrf_exempt，取消当前函数防跨站请求伪造功能，即便settings中设置了全局中间件。

**Django中CSRF防护原理：**

在用户访问django的可信站点时，django反馈给用户的表单中有一个隐含字段csrftoken，这个值是在服务器端随机生成的，每一次提交表单都会生成不同的值。当用户提交django的表单时，服务器校验这个表单的csrftoken是否和自己保存的一致，来判断用户的合法性。当用户被csrf攻击从其他站点发送精心编制的攻击请求时，由于其他站点不可能知道隐藏的csrftoken字段的信息这样在服务器端就会校验失败，攻击被成功防御，这样就能避免被 CSRF 攻击。

1. 在返回的 HTTP 响应的 cookie 里，django 会为你添加一个 csrftoken 字段，其值为一个自动生成的 token
2. 在所有的 POST 表单时，必须包含一个 csrfmiddlewaretoken 字段 （只需要在模板里加一个 tag， django 就会自动帮你生成，见下面）
3. 在处理 POST 请求之前，django 会验证这个请求的 cookie 里的 csrftoken 字段的值和提交的表单里的 csrfmiddlewaretoken 字段的值是否一样。如果一样，则表明这是一个合法的请求，否则，这个请求可能是来自于别人的 csrf 攻击，返回 403 Forbidden.
4. 在所有 ajax POST 请求里，添加一个 X-CSRFTOKEN header，其值为 cookie 里的 csrftoken 的值

**Django中使用 CSRF 防护**

1. GET 请求不要用有副作用。任何处理 GET 请求的代码对资源的访问都一定要是“只读“的。
2. 启用 django.middleware.csrf.CsrfViewMiddleware 中间件
3. 使用POST 表单元素时，加上{% csrf_token %}
4. 渲染模块使用 RequestContext。RequestContext 会处理 csrf_token ,  从而自动为表单添加一个名为 csrfmiddlewaretoken 的 input

**Django中CSRF防护解析：**

csrftoken的生成方式如下：

```
md5_constructor("%s%s" % (randrange(0, _MAX_CSRF_KEY), settings.SECRET_KEY)).hexdigest()
```

设置csrftoken:

```
response.set_cookie(settings.CSRF_COOKIE_NAME,request.META["CSRF_COOKIE"], max_age = 60 * 60 * 24 * 7 * 52, domain=settings.CSRF_COOKIE_DOMAIN)
```

csrftoken与cookie中的token比较：

```
if not constant_time_compare(request_csrf_token, csrf_token):
```
转载请注明：[化风的博客](http://xinchanghao.github.io) » [Django中CSRF防护原理及使用](/2017/05/Django中CSRF防护原理及使用/)  
