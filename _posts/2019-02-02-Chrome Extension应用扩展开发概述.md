---
layout: post
title: "Markdown书写模板"
date: 2018-05-05
description: "Markdown书写模板"
tag: 教程
---



> 摘要：本文主要描述关于chrome plugin开发的相关开发知识，着重讲述下popup/background/content-script三者之间的消息互通。

@[TOC](目录)
## 一、前言
在此之前，并未接触到chrome插件开发，由于最近业务需要，不得不去了解一些相关内容。在实践的过程中，总结了一些内容，一来方便自己日后查阅，二来希望给众位同僚提供一些参考。如有不对之处，还望及时指正。

## 一、Chrome Extension还是Chrome Plugin?

事实上，本文所说的插件并不是严格意义上的Chrome Plugin，从真正意义上讲，Chrome Plugin是浏览器底层的功能实现，是需要有一定的浏览器开发能力才可以接触到的层面。而我们习惯上更喜欢叫Chrome插件，但作为一个正直的developer，我们需要知道这二者是不同的东西。所以，在本文中出现的chrome extension扩展 和chrome plugin插件都是指同一个东西。
## 二、为什么选择Chrome浏览器？
众所周知，Chrome浏览器在全球都拥有可观的忠实用户，抛去其占据了浏览器市场的霸主地位不说，其具备了众多的优点，如良好的用户体验，简单的开发规范等等。

归纳为以下几点：

> 1. 市场占有率高，用户基础庞大；
> 2. 开发流程简单，方便快速上手；
> 3. 应用场景广泛，兼容webkit内核（360极速浏览器、360安全浏览器、搜狗浏览器、QQ浏览器等等）；
> 4. 可扩展性强，非weikit内核的浏览器也有一定的支持（如Firefox）
## 三、关于Chrome Extension
Chrome Extension简单定义为浏览器的功能性扩展，由html、css、js和一个描述文件manifest.json组成，在浏览器的地址栏边上显示扩展图标。本质上其实就是一个由html、css、js、图片等资源组成的一个.crx后缀的压缩包。

## 四、开发调试
##### 1. 调试弹出页(popup)
右击扩展图标->审查弹出内容即可弹出开发者面板，这个面板与网页调试面板一模一样，操作方式也是相同的。值得一提的是第一次弹出面板，会错过弹出页，初始化的脚本，可以通过在对应的面板上按F5然它重新加载进入断点
##### 2. 调试选项页(option)
右击扩展图标->选项,在选项页按F12打开调试面板
##### 3. 调试后台页(background)
点击检查视图后的超链接，就会弹出后台页相关的调试面板。
##### 4. 调试内容脚本(content script)
在内容脚本注入的网页打开开发者面板->source->Content scripts(左侧面板)

## 五、Manifest 文件
每一个扩展、可安装的WebApp、皮肤，都有一个JSON格式的manifest文件用来配置所有和扩展相关的配置，而且必须放在扩展的根目录。其中，名称（name）、版本（version）和Manifest 版本（manifest_version）这3个是必须添加的（而且manifest_version 必须为 2），描述（description）、图标位置（icons）是推荐的。

归纳一下manifest文件常见的配置项

```
{
	//（必须）manifest版本，而且必须是2
	"manifest_version": 2,
	// （必须）名称
	"name": "demo",
	// （必须）版本
	"version": "1.0.0",
	// （推荐）描述
	"description": "简单的Chrome扩展demo",
	// （推荐）图标，懒加载可用一个尺寸
	"icons":
	{
	    "16": "images/icon-16.png",
	    "32": "images/icon-32.png",
	    "48": "images/icon-48.png",
	    "64": "images/icon-64.png",
	    "128": "images/icon-128.png"
	},
	// background script即插件运行的环境，会一直常驻的后台JS或后台页面
	"background":
	{
		// 2种指定方式，如果指定JS，那么会自动生成一个背景页
		"page": "background.html"
		//"scripts": ["js/background.js"]
	},
	// 浏览器右上角图标设置，browser_action、page_action、app必须三选一
	//	注意： Packaged apps 不能使用browser actions.
	"browser_action": 
	{
		"default_icon": "images/icon.png",
		"default_title": "hello", // 图标悬停时的标题，可选
		"default_popup": "popup.html"
	},
	// 当某些特定页面打开才显示的图标
	/*"page_action":
	{
		"default_icon": "images/icon.png",
		"default_title": "hello",
		"default_popup": "popup.html"
	},*/
	// 需要直接注入页面的JS
	"content_scripts": 
	[
		{
			//"matches": ["http://*/*", "https://*/*"],
			// "<all_urls>" 表示匹配所有地址
			"matches": ["<all_urls>"],
			// 多个JS按顺序注入
			"js": ["js/jquery-1.8.3.js", "js/content-script.js"],
			// JS的注入可以随便一点，但是CSS的注意就要千万小心了，因为一不小心就可能影响全局样式
			"css": ["css/custom.css"],
			// 代码注入的时间，可选值： "document_start", "document_end", or "document_idle"，最后一个表示页面空闲时，默认document_idle
			"run_at": "document_start"
		},
		// 这里仅仅是为了演示content-script可以配置多个规则
		{
			"matches": ["*://*/*.png", "*://*/*.jpg", "*://*/*.gif", "*://*/*.bmp"],
			"js": ["js/show-image-content-size.js"]
		}
	],
	// 权限申请
	"permissions":
	[
		"contextMenus", // 右键菜单
		"tabs", // 标签
		"notifications", // 通知
		"webRequest", // web请求
		"webRequestBlocking",
		"storage", // 插件本地存储
		"http://*/*", // 可以通过executeScript或者insertCSS访问的网站
		"https://*/*" // 可以通过executeScript或者insertCSS访问的网站
	],
	// 普通页面能够直接访问的插件资源列表，如果不设置是无法直接访问的
	"web_accessible_resources": ["js/inject.js"],
	// 扩展的主页 url。扩展的管理界面里面将有一个链接指向这个url。如果你将扩展放在自己的网站上，这个url就很有用了。如果你通过了Extensions Gallery和Chrome Web Store来分发扩展，主页 缺省就是扩展的页面。
	"homepage_url": "https://www.baidu.com",
	// 覆盖浏览器默认页面
	"chrome_url_overrides":
	{
		// 覆盖浏览器默认的新标签页
		"newtab": "newtab.html"
	},
	// Chrome40以前的插件选项页写法
	"options_page": "options.html",
	// Chrome40以后的插件选项页写法，如果2个都写，新版Chrome只认后面这一个
	"options_ui":
	{
		"page": "options.html",
		// 添加一些默认的样式，推荐使用
		"chrome_style": true
	},
	// 向地址栏注册一个关键字以提供搜索建议，只能设置一个关键字
	"omnibox": { "keyword" : "go" },
	// 默认语言
	"default_locale": "zh_CN",
	// devtools页面入口，注意只能指向一个HTML文件，不能是JS文件
	"devtools_page": "devtools.html"
}
```

从Chrome 18版本开始，Manifest V1就开始进入了淘汰的过程。Chrome内核对 Manifest V1 的支持计划具体可参见 Google 开发者网站上的日程表：[manifest version 1 support schedule](http://developer.chrome.com/extensions/manifestVersion.html)。

## 六、content-script文件

> content-script文件是嵌入到匹配的网页中的脚本，但是又与页面中的脚本隔离开。虽然可以操纵页面上的DOM元素，但却不能够使用页面脚本的API。也就是运行环境与页面的脚本是隔离开的。

content-script和原始页面共享DOM，但是不共享JS，如要访问页面JS（例如某个JS变量），只能通过injected js来实现。content-scripts不能访问绝大部分chrome.xxx.api，除了下面这4种：

> chrome.extension(getURL , inIncognitoContext , lastError , onRequest , sendRequest)
chrome.i18n
chrome.runtime(connect , getManifest , getURL , id , onConnect , onMessage , sendMessage)
chrome.storage


重要！！！除了可以在manifest中配置需要注入的页面以外，还可以动态的注入到页面中。

```
    //直接注入代码
    chrome.browserAction.onClicked.addListener(function(tab) {
      chrome.tabs.executeScript({
        code: 'document.body.style.backgroundColor="red"'
      });
    });
    //注入脚本文件
    chrome.tabs.executeScript(null, {file: "content_script.js"});

```
需要在manifest文件中配置权限：

```
    "permissions": [
      "activeTab"
    ],
```

## 七、background文件

> background 可以使扩展常驻后台，比较常用的是指定子属性scripts，表示在扩展启动时自动创建一个包含所有指定脚本的页面。它的生命周期是插件中所有类型页面中最长的，它随着浏览器的打开而打开，随着浏览器的关闭而关闭，所以通常把需要一直运行的、启动就运行的、全局的代码放在background里面。

background的权限非常高，几乎可以调用所有的Chrome扩展API（除了devtools），而且它可以无限制跨域，也就是可以跨域访问任何网站而无需要求对方设置CORS。
> 
> 经过测试，其实不止是background，所有的直接通过chrome-extension://id/xx.html这种方式打开的网页都可以无限制跨域。

## 八、popup文件
如果browser action拥有一个popup，popup 会在用户点击图标后出现。popup 可以包含任意你想要的HTML内容，并且会自适应大小。所以一般用来做临时性的交互。

在你的browser action中添加一个popup，创建弹出的内容的HTML文件。 修改browser_action的manifest中default_popup字段来指定HTML文件， 或者调用setPopup()方法。

> 在权限上，它和background非常类似，它们之间最大的不同是生命周期的不同，popup中可以直接通过chrome.extension.getBackgroundPage()获取background的window对象。

## 九、消息通信机制

废话不多说，都在代码里了。[源代码](https://github.com/ChhXin/chrome-message)

![消息交互图](https://img-blog.csdnimg.cn/20190202163044757.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhb2FpcWlhbg==,size_16,color_FFFFFF,t_70)

> 	-_-，图略丑陋，将就看一下，手动微笑。

##### 1. popup.html文件

```
<!doctype html>
<html lang="zh">
<head>
	<meta charset="UTF-8">
</head>
<body style="width: 300px">	
    <div width="200px">
      <button style="margin:5px 5px 5px 5px" id="con">popup发送消息给content_scripts</button>
      <button style="margin:5px 5px 5px 5px" id="bg">popup调用background的js函数</button>
      <button style="margin:5px 5px 5px 5px" id="bgtocon">background发送消息给content_scripts</button>
    </div>
</body>
</html>

<script type="text/javascript" src="js/jquery.js"></script>
<script type="text/javascript" src="js/popup.js"></script>
```
##### 2. popup.js文件

```

// popup调用background的js函数
$('#bg').click(() => {
	//alert("调用background的js函数");
	var bg = chrome.extension.getBackgroundPage();
	console.log(123123, bg)
	bg.bgtest();
});

 // popup主动发消息给content-script
$('#con').click(() => {
	alert("popup发送消息给content-script");
	sendMessageToContentScript('你好，我是popup！', (response) => {
		if(response) alert('收到来自content-script的回复：'+response);
	});
});

// popup调用background的js函数
$('#bgtocon').click(() => {
	var bg = chrome.extension.getBackgroundPage();
	bg.TT();
});

// 获取当前选项卡ID
function getCurrentTabId(callback)
{
	chrome.tabs.query({active: true, currentWindow: true}, function(tabs)
	{
		if(callback) callback(tabs.length ? tabs[0].id: null);
	});
}

// 向content-script主动发送消息
function sendMessageToContentScript(message, callback)
{
	getCurrentTabId((tabId) =>
	{
		chrome.tabs.sendMessage(tabId, message, function(response)
		{
			if(callback) callback(response);
		});
	});
}
```

##### 3. background.js文件

```
function bgtest()
{
	alert("background的bgtest函数！");
}

// 监听来自content-script的消息
chrome.runtime.onMessage.addListener(function(request, sender, sendResponse)
{
	console.log('收到来自content-script的消息：');
	console.log(request, sender, sendResponse);
	sendResponse('我是background，我已收到你的消息：' + JSON.stringify(request));
});

// backgrond向context_scripts发送消息
function TT(){
	
	sendMessageToContentScript('context_scripts你好，我是backgrond！', (response) => {
	if(response) alert('backgrond收到来自content-script的回复：'+response);
	});
}

// 获取当前选项卡ID
function getCurrentTabId(callback)
{
	chrome.tabs.query({active: true, currentWindow: true}, function(tabs)
	{
		if(callback) callback(tabs.length ? tabs[0].id: null);
	});
}

// 向content-script主动发送消息
function sendMessageToContentScript(message, callback)
{
	getCurrentTabId((tabId) =>
	{
		chrome.tabs.sendMessage(tabId, message, function(response)
		{
			if(callback) callback(response);
		});
	});
}
```

##### 4. content-script文件

```
//常驻后台，并且会注入到页面中
alert("content-script.js 已经注入");

//直接调用注入的其他的js函数 注入的js可以有多个在mainfest中配置
aa();

// 接收来自后台的消息
chrome.runtime.onMessage.addListener(function(request, sender, sendResponse)
{
	console.log('收到来自 ' + (sender.tab ? "content-script(" + sender.tab.url + ")" : "popup或者background") + ' 的消息：', request);
	tip(JSON.stringify(request));
	sendResponse('我收到你的消息了：'+JSON.stringify(request));

});


 //与后端background进行消息交互	 执行6秒
var startTime = new Date().getTime(); 
var interval = setInterval(function ()
{ 
	if(new Date().getTime() - startTime > 6000)
	{
		clearInterval(interval);
		return;
	}
  chrome.runtime.sendMessage
  (
	{
		doc: "yes",
		data:"123",
	},
	function(response) 
	{
		
   		tip(JSON.stringify("content-script向background 发送消息"));	
   		tip('收到来自background的回复：' + response);
	}
  );
},500);




var tipCount = 0;
// 简单的消息通知
function tip(info) {
	info = info || '';
	var ele = document.createElement('div');
	ele.className = 'chrome-plugin-simple-tip slideInLeft';
	ele.style.top = tipCount * 70 + 20 + 'px';
	ele.innerHTML = `<div>${info}</div>`;
	document.body.appendChild(ele);
	ele.classList.add('animated');
	tipCount++;
	setTimeout(() => {
		ele.style.top = '-100px';
		setTimeout(() => {
			ele.remove();
			tipCount--;
		}, 4000);
	}, 5000);
}

```

## 十、参考资料
[360安全浏览器开发文档（推荐）](http://open.se.360.cn/open/extension_dev/overview.html)
[360极速浏览器Chrome扩展开发文档](http://open.chrome.360.cn/extension_dev/overview.html)
[chrome扩展开发官方文档](https://developer.chrome.com/extensions)
[chrome API支持](https://developer.chrome.com/extensions/api_index)
[【干货】Chrome插件(扩展)开发全攻略](http://blog.haoji.me/chrome-plugin-develop.html#popup-he-background)（很详细，文中部分内容来源于此）
 [Chrome扩展开发概述](https://juejin.im/entry/59887ab06fb9a03c50228ca0)


转载请注明：[化风的博客](http://ChhXin.github.io) » [Chrome Extension应用扩展开发概述](/2019/02/Chrome Extension应用扩展开发概述/)                   
