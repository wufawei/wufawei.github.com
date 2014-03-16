---
layout: post   
title: 升级到iOS 7.1之后，你需要注意的一些事情     
categories: 
- iOS   
tags:     
- iOS
---    
 
 

iOS 7.1正式版本发布后，升级速度非常快，目前已经达到33%的占有率。同步更新的还有Xcode5.1，那么在升级到Xcode5.1（iOS 7.1）之后，你需要注意哪些事情呢？



###企业包部署需要HTTPS

部署企业包需要用HTTPS，在[iOS技术分析第4期][1]的一篇文章就提到:


`stackoverflow上有人反映iOS 7.1 beta要使用HTTPS才能部署企业包，很可能iOS 7.1会有此要求。`

可以参见这篇文章：[Enterprise app deployment doesn't work on iOS 7.1][2]


###编译不通过的问题
在升级到Xcode5.1之后，很可能你会遇到编译不通过的情况，因为Xcode默认会加上64位编译
第三方库很多都不支持 arm64。


这个时候，只用把工程设置中的Targets—>Build Settings—>Architectures暂时去掉arm64就可以了。

参见这篇文章:[Xcode 5 and iOS 7: Architecture and Valid architectures][3]

 

<br>

###关于我
大家好，我是[@吴发伟Ted](http://weibo.com/wufawei)，我会在博客上分享自己学习的一些东西。

###好文推荐  && iOS Wiki

[好文推荐(news.ios-wiki.com)][5]和[iOS Wiki(www.ios-wiki.com)][6]是我利用业余时间建立的网站。

###微信公众账号
微信公众账号：**iOS技术分享**  
![](http://farm3.staticflickr.com/2826/10855679484_56b7429bd6_m.jpg)


 
[1]:http://www.ios-wiki.com/issues/4
[2]:http://stackoverflow.com/questions/20276907/enterprise-app-deployment-doesnt-work-on-ios-7-1
[3]:http://stackoverflow.com/questions/18913906/xcode-5-and-ios-7-architecture-and-valid-architectures
[5]:http://news.ios-wiki.com/newest
[6]:http://www.ios-wiki.com
