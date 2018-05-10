---
layout: post
title: "关于发邮件报错535-Error-authentication-failed解决方法"
date: 2017-03-17
description: "关于发邮件报错535-Error-authentication-failed解决方法"
tag: 网络协议
---
﻿#####smtplib.SMTPAuthenticationError: (535, 'Error: authentication failed')

用163邮箱服务器来发送邮件,我们需要开启POP3/SMTP服务,这时163邮件会让我们设置客户端授权码，这个授权码替代上面代码部分的passwd即可成功发送邮件


关于发邮件报错550 Error：user has no permission解决方法

原因一：properties里是否存放了 mail.smtp.auth= true
原因二：邮箱没有开通POP3/SMTP协议

###发送邮件部分代码
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_USE_TLS = True
EMAIL_HOST = 'smtp.163.com'
EMAIL_PORT = 25
EMAIL_HOST_USER = '163_sender@163.com'
EMAIL_HOST_PASSWORD = '********'
DEFAULT_FROM_EMAIL = '163_sender@163.com'


def sendmsg(request):
	send_mail(subject, message,'163_sender@163.com',['163_recever@163.com'],fail_silently=False)  
	return HttpResponse("发送邮件成功！")

转载请注明：[化风的博客](http://xinchanghao.github.io) » [关于发邮件报错535-Error-authentication-failed解决方法](/2017/03/关于发邮件报错535-Error-authentication-failed解决方法/)  
