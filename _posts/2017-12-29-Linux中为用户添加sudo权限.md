---
layout: post
title: "Linux中为用户添加sudo权限"
date: 2017-12-29
description: "Linux中为用户添加sudo权限"
tag: 系统
---

﻿**1. 进入超级用户模式。**

```
$ su -   #然后输入密码
```

**2. 编辑/etc/sudoers文件**

```
$ visudo   #visudo命令是用来编辑修改/etc/sudoers配置文件
```
或者

```
$ vim /etc/sudoers
```

找到："root ALL=(ALL) ALL", 在下面添加"xxx ALL=(ALL) ALL"。(这里的xxx是你的用户名)，然后:wq保存退出

**3. 测试是否成功**

切换到xxx用户：
```
su xxx
cd ~
sudo mkdir test
```
test文件夹创建成功则表示通过。

**4. 取消sudo验证**

在刚才修改权限的位置加入Defaults:xxx timestamp_timeout=-1,runaspw

![取消sudo验证](http://img.blog.csdn.net/20171229170922185?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

timestamp_timeout=-1 只需验证一次密码，以后系统自动记忆。
runaspw需要root密码，如果不加默认是要输入普通账户的密码。

转载请注明：[化风的博客](http://ChhXin.github.io) » [Linux中为用户添加sudo权限](/2017/12/Linux中为用户添加sudo权限/)  
