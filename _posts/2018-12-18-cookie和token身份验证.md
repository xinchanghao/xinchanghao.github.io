---
layout: post
title: "cookie和token身份验证"
date: 2018-12-18
description: "cookie和token身份验证"
tag: JavaScript
---

HTTP Cookie（也叫Web Cookie或浏览器Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息成为了可能。

Cookie主要用于以下三个方面：

> 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息） 
> 个性化设置（如用户自定义设置、主题等）
> 浏览器行为跟踪（如跟踪分析用户行为等）

Cookie曾一度用于客户端数据的存储，因当时并没有其它合适的存储办法而作为唯一的存储手段，但现在随着现代浏览器开始支持各种各样的存储方式，Cookie渐渐被淘汰。由于服务器指定Cookie后，浏览器的每次请求都会携带Cookie数据，会带来额外的性能开销（尤其是在移动环境下）。新的浏览器API已经允许开发者直接将数据存储到本地，如使用 [Web storage API （本地存储和会话存储）](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API)或 [IndexedDB 。](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API)

### 1. cookie身份验证

>  1. 用户输入登陆凭据； 
>  2. 服务器验证凭据是否正确，并创建会话，然后把会话数据存储在数据库中；
>  3. 具有会话id的cookie被放置在用户浏览器中；
>  4. 服务器验证凭据是否正确，并创建会话；
>  5. 在后续请求中，服务器会根据数据库验证会话id，如果验证通过，则继续处理； 
>  6. 一旦用户登出，服务端和客户端同时销毁该会话在后续请求中，服务器会根据数据库验证会话id，如果验证通过，则继续处理；


![在这里插入图片描述](https://img-blog.csdnimg.cn/20181218143040425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==,size_16,color_FFFFFF,t_70)

### 2. token身份验证

>  1. 用户输入登陆凭据； 
>  2. 服务器验证凭据是否正确，然后返回一个经过签名的token；  
>  3. 客户端负责存储token，可以存在localstorage，或者cookie中
>  4. 对服务器的请求带上这个token； 
>  5. 服务器对JWT进行解码，如果token有效，则处理该请求；
>  6. 一旦用户登出，客户端销毁token。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2018121814305772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==,size_16,color_FFFFFF,t_70)

### 3. 二者特性对比
####  3.1 cookie

> 用户登录成功后，会在服务器存一个session，同时发送给客户端一个cookie
> 
> 数据需要客户端和服务器同时存储
> 
> 用户进行操作时，需要带上cookie，在服务器进行验证
> 
> cookie是有状态的

 

#### 3.2 token

> 用户进行任何操作时，都需要带上一个token
> 
> token的存在形式有很多种，header/requestbody/url 都可以
> 
> 这个token只需要存在客户端，服务器在收到数据后，进行解析
> 
> token是无状态的

#### 3.3总结

> Token 完全由应用管理，所以它可以避开同源策略。
> 
> Token 可以避免 CSRF 攻击。
> 
> Token 可以是无状态的，可以在多个服务间共享。


参考链接：
http://www.jianshu.com/p/ce9802589143
https://blog.csdn.net/xiaoxinshuaiga/article/details/80766378
[JSON Web Token 入门教程-阮一峰](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)


转载请注明：[化风的博客](http://xinchanghao.github.io) » [cookie和token身份验证](/2018/12/cookie和token身份验证/)                   
