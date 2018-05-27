---
layout: post
title: "Python中使用NLTK库解决错误LookupError_from_nltk_book_import"
date: 2017-04-18
description: "Python中使用NLTK库解决错误LookupError_from_nltk_book_import"
tag: Python
---
﻿**什么是词干提取？**
在语言形态学和信息检索里，词干提取是去除词缀得到词根的过程─—得到单词最一般的写法。对于一个词的形态词根，词干并不需要完全相同；相关的词映射到同一个词干一般能得到满意的结果，即使该词干不是词的有效根。从1968年开始在计算机科学领域出现了词干提取的相应算法。很多搜索引擎在处理词汇时，对同义词采用相同的词干作为查询拓展，该过程叫做归并。

使用pip安装NLTK

```
sudo pip install nltk
```

```
>>> import nltk
#没有报错即安装成功
```

使用过程中，报错；错误代码为：
Traceback (most recent call last):
  File "E:\python\lib\site-packages\nltk\corpus\util.py", line 80, in __load
    try: root = nltk.data.find('{}/{}'.format(self.subdir, zip_name))
  File "E:\python\lib\site-packages\nltk\data.py", line 648, in find
    raise LookupError(resource_not_found)
LookupError:
**********************************************************************
  Resource 'corpora/brown.zip/brown/' not found.  Please use the
  NLTK Downloader to obtain the resource:  >>> nltk.download()


查过资料后知道，有人遇到过此类问题，并给出了解决办法：
![老外给的解决方案](http://img.blog.csdn.net/20170418221445900?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```
>>> nltk.download()
```
然后就跳出了所谓的下载界面：
![这里写图片描述](http://img.blog.csdn.net/20170418221637749?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGFvYWlxaWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

接下来就可以了。。。。

转载请注明：[化风的博客](http://xinchanghao.github.io) » [Python中使用NLTK库解决错误LookupError_from_nltk_book_import](/2018/04/Python中使用NLTK库解决错误LookupError_from_nltk_book_import/)                   
