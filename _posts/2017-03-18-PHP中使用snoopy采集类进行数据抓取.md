---
layout: post
title: "PHP中使用snoopy采集类进行数据抓取"
date: 2017-03-18
description: "PHP中使用snoopy采集类进行数据抓取"
tag: PHP
---

﻿```
include "Snoopy.class.php";
$snoopy = new Snoopy;

$snoopy->proxy_host = "http://www.url.net";
$snoopy->proxy_port = "80";

$snoopy->agent = "(compatible; MSIE 4.01; MSN 2.5; AOL 4.0; Windows 98)";
$snoopy->referer = "http://www.url.net";

$snoopy->cookies["SessionID"] = 238472834723489l;
$snoopy->cookies["favoriteColor"] = "RED";

$snoopy->rawheaders["Pragma"] = "no-cache";

$snoopy->maxredirs = 2;
$snoopy->offsiteok = false;
$snoopy->expandlinks = false;

$snoopy->user = "joe";
$snoopy->pass = "bloe";

if($snoopy->fetchtext("http://www.url.net"))
{
echo "<PRE>".htmlspecialchars($snoopy->results)."</PRE>\n";
}
else
echo "error fetching document: ".$snoopy->error."\n";
```
转载请注明：[化风的博客](http://xinchanghao.github.io) » [PHP中使用snoopy采集类进行数据抓取](/2017/03/PHP中使用snoopy采集类进行数据抓取/)  
