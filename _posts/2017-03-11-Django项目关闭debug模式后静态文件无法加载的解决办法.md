---
layout: post
title: "Django项目关闭debug模式后静态文件无法加载的解决办法"
date: 2017-03-11
description: "Django项目关闭debug模式后静态文件无法加载的解决办法"
tag: Python
---

﻿###Django框架仅在开发模式下提供静态文件服务。开启DEBUG模式时，Django内置的服务器是提供静态文件的服务的，所以css等文件访问都没有问题，但是关闭DEBUG模式后，Django便不提供静态文件服务了。


~1.将静态文件由apache提供文件服务(类似正式部署)：

 编辑/etc/apache2/sites-available/horizon文件：

    #Alias /media /opt/stack/horizon/openstack_dashboard/static
Alias /static /opt/stack/horizon/openstack_dashboard/static

建立静态文件链接：

     ln -sv /opt/stack/horizon/openstack_dashboard/static /opt/stack/horizon

重启apache:

     sudo service apache2 restart

~2.使用django.views.static.serve()方法。在URLconf中添加：

     (r'^site_media/(?P<path>.*)$', 'django.views.static.serve',{'document_root': '/path/to/media'}),

####官方文档中评价这种办法：“The big, fat disclaimer”。

3.伪造404页面：使用正确的URL链接404页面模板；

4.改变项目运行方式：

     python manage.py runserver --insecure

[参考出处](https://my.oschina.net/zyzzy/blog/173262)
转载请注明：[化风的博客](http://xinchanghao.github.io) » [Django项目关闭debug模式后静态文件无法加载的解决办法](/2017/03/Django项目关闭debug模式后静态文件无法加载的解决办法/)  
