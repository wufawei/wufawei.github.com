---
layout: post   
title: iOS技术分享－第12期        
categories: 
- iOS   
tags:     
- iOS
---    
 

iOS技术分享第12期的主要内容精选自[iOS News][1]。   
欢迎大家访问[news.ios-wiki.com][1]查看更多内容，每天都会分享精彩好文章。


**欢迎大家注册、分享、顶好文章。
**

 

本期主要包含:

###资讯

* 如果App没有广告功能，随便用AdvertisingIdentifier可能会被拒  
二月份的时候苹果拒掉了一些用AdvertisingIdentifier但是没有广告功能的App，现在苹果开始在iTunes Connect中新加了一个部分, 提交App的时候要勾选是否使用AdvertisingIdentifier,以及使用的场景。使用下面命令可以查看你的.app文件是否包含这个方法：
otool -v -s __TEXT __objc_methname AppName | grep advertisingIdentifier


* Unflod：iOS越狱设备上的恶意代码，截取Apple id和密码  
iOS越狱设备上出现恶意代码，通过 Hook Security.framework的SSLWrite，截取Apple id和密码。在reddit上被人讨论，里面有样本，有兴趣的可以分析下。这个讨论里面还有saurik关于iOS越狱设备上的恶意软件Unflod.dylib的建议的相关链接。


* 知名iOS开发者和设计师给iOS/Mac app开发的建议  
知名iOS开发者和设计师给iOS/Mac app开发的建议：学习的最好方法是分享和讨论；每种app都已经做过几百遍了，但是仍有空间留给那些更好的app；等等

###工具

* CocoaPods Feeds    
可以订阅这个Feed来查看最近CocoaPods新增加的pods源

* Running Custom Clang Analyzer Builds  
Clang Analyzer是开源的，Xcode中集成的Clang Analyzer往往不是最新版本的。按文章中的介绍下载最新的Clang Analyzer，
然后与Xcode绑定。运行Xcode中的分析（Product > Analyze ⇧⌘B）你会得到更多的Warning信息，比如忘记调用super的view(Did/Will)(Appear/Disappear)等方法


###代码

* Synchronizing Around A Class  
在同步的时候一般都是用 @synchronized (self)，但是对类方法，特别是在基类中定义的类方法，这样写有问题，应该写成类似@synchronized ([SomeClassName class])这样   

* JavaScriptCore by Example  
JavaScriptCore相关的官方文档介绍得并不详细，本文给出了使用JavaScriptCore的好例子和代码

* GCDWebServer  
轻量级、基于GCD实现的HTTP server（for OS X & iOS）


###技巧

* 如何判断iOS App是否调用了特定的API  
文章介绍如何判断App是否调用了uniqueIdentifier，你也可以用文章介绍的方法，判断是否使用了advertisingIdentifier。
把如下命令中AppName换成你的应用名称，otool -v -s __TEXT __objc_methname AppName | grep advertisingIdentifier。

* 对iOS恶意软件Unflod的分析  
恶意代码Unflod，通过Hook Security.framework的SSLWrite，截取Apple id和密码。这篇文章详细的分析了该恶意代码。。


<br>
更多内容，请访问[iOS技术分享第12期][2]查看新一期的内容。  
手机版本，请访问[iOS技术分享第12期][3]。


<br>

###关于我
大家好，我是[@吴发伟Ted](http://weibo.com/wufawei)，我会在博客上分享自己学习的一些东西。

###好文推荐  && iOS Wiki

[好文推荐(news.ios-wiki.com)][5]和[iOS Wiki(www.ios-wiki.com)][6]是我利用业余时间建立的网站。

###微信公众账号
微信公众账号：**iOS技术分享**  
![](http://farm3.staticflickr.com/2826/10855679484_56b7429bd6_m.jpg)




<br/>

[1]:http://news.ios-wiki.com
[2]:http://www.ios-wiki.com/issues/12
[3]:http://www.ios-wiki.com/mobile/issues/12


[5]:http://news.ios-wiki.com
[6]:http://www.ios-wiki.com