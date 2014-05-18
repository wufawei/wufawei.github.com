---
layout: post   
title: iOS技术周报－第16期        
categories: 
- iOS   
tags:     
- iOS
---    

### iOS技术周报第16期


iOS技术周报第16期的主要内容精选自[iOS News][1]。   
欢迎大家访问[iOS技术周报(weekly.ios-wiki.com)][7]查看更多内容。


**欢迎大家注册、分享、顶好文章。**


本期主要包含:

###资讯

* 做App的成本   
制造一款App的成本，你计算过吗？ 当你推进一个移动项目的时候，就掉进了成本的无底洞里，并非"只差一个程序员了"这么简单。

###视频

* Mobile Engineering at Facebook's Scale  
Facebook介绍其移动开发经验，最后介绍了Paper中遇到的技术挑战。

* Releasing & Optimizing Mobile Apps for the World  
从专门的人各自负责Web、Android、iOS等平台，开发对应feature，到各自feature的开发人员负责所有平台的开发，Facebook介绍其移动开发之路。

###技巧

* ​Objective-C中Class Clusters设计模式  
子类太多，使用不便，可以通过类簇来很方便地搞定。可以参见[类簇在iOS开发中的应用](http://blog.leezhong.com/ios/2014/01/04/class-cluster.html)。

* Variable Argument Lists（可变参数列表）  
文章介绍了Variable Argument Lists（可变参数列表）的使用方法，内部机制和两种常用的解决方案。

* ​自动在APP图标上添加当前版本号信息  

* ​NSLog效率低下的原因及尝试lldb断点打印Log  
文章探究了一下NSLog如此之慢的原因，并尝试使用lldb断点调试器替代NSLog进行debug log。


###代码

* Controlling Animation Timing  
CABasicAnimation与CAKeyframeAnimation的基类CAAnimation和CALayer都实现了CAMediaTiming协议，文章对其各个参数如speed和timeOffset等都进行了详细介绍，可以参见[cheat sheet](http://ronnqvi.st/images/CAMediaTiming%20cheat%20sheet.pdf)。

* Mesh Transforms  
实现了网状变形的动画，推荐下载[代码](https://github.com/Ciechan/BCMeshTransformView)，运行代码看看实际运行效果。

* MTDActionSheet    
MTDActionSheet使用block作为callback，是iPad上UIActionSheet的替代类，其颜色和字体可以很方便的定制。

* RSSRead    
RSSRead是一个iOS设备上的RSS/Atom阅读器。


###工具

* Eight Terminal Utilities Every OS X Command Line User Should Know    
每个Mac OS X 命令行用户应当知道的八个终端工具，其中文翻译在[此](http://aimijia.net/2014/05/each-mac-os-x-command-line-users-should-be-aware-of-the-eight-terminal-tool/)。感谢 [@yaongphy](http://weibo.com/336525266)的提交




###设计

* Prototyping with Facebook's Origami  
介绍使用Origami进行原型设计，里面附有更多教程。  


###书籍

* 苹果在iBooks发布iOS人机交互指南  
iOS人机界面指南涵盖一般的设计和内容规则，以前只能通过苹果开发者门户网站获取，你可以在Mac或者iOS设备上阅读。  



<br>
更多内容，请访问[iOS技术分享第16期][2]查看新一期的内容。  


<br>

###关于我
大家好，我是[@吴发伟Ted](http://weibo.com/wufawei)，我会在博客上分享自己学习的一些东西。

###iOS技术周报 & 好文推荐

[iOS 技术周报(weekly.ios-wiki.com)][7] 和 [好文推荐(news.ios-wiki.com)][5]是我利用业余时间建立的网站。

###微信公众账号
微信公众账号：**iOS技术分享**  
![](http://farm3.staticflickr.com/2826/10855679484_56b7429bd6_m.jpg)



<br/>

[1]:http://news.ios-wiki.com
[2]:http://weekly.ios-wiki.com/issues/16
[3]:http://twitter.com/daveverwer
[4]:https://iosdevweekly.com/
[5]:http://news.ios-wiki.com
[6]:http://www.ios-wiki.com
[7]:http://weekly.ios-wiki.com