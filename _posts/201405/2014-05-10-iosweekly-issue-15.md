---
layout: post   
title: iOS技术周报－第15期        
categories: 
- iOS   
tags:     
- iOS
---    

### iOS技术周报第15期


iOS技术周报第15期的主要内容精选自[iOS News][1]。   
欢迎大家访问[iOS技术周报(weekly.ios-wiki.com)][7]查看更多内容。


**欢迎大家注册、分享、顶好文章。**


本期主要包含:

###资讯

* objc.io第12期  
这一期对iOS动画进行了详细的介绍，从UIView animations, 到Core Animation，介绍了UIViewController以及Collection View的动画等等。


###技巧

* ​6种有效的iOS团队开发技巧  
使用cocoapods，feature分支开发，使用Xcode的Configurations 和 Preprocessor Marcos区分具体环境信息等6个开发技巧。

* 如何删掉所有subView  
常用方法就是遍历view的subviews，然后一个个移除，这里介绍的[someNSView setSubviews:[NSArray array]] 和 [[someUIView subviews] makeObjectsPerformSelector:@selector(removeFromSuperview)]都能达到同样的目的。

* ​unrecognized selector sent to instance 问题快速定位的方法  
在Debug菜单中选择 Breakpoints -> Create Symbolic Breakpoint，在Symbol中填写如下方法签名：-[NSObject(NSObject) doesNotRecognizeSelector:]，然后再运行，错误时断点会停在真正导致崩溃的地方。

* ​iOS 7上给View截图的方法  
iOS 7上UIView上提供了drawViewHierarchyInRect:afterScreenUpdates:来截图, 速度比renderInContext:快15倍。


###代码

* ClusterPrePermissions  
在 iOS技术周报第13期 的［小细节大作为：提高iOS访问权限通过率有这几个办法］中介绍了原生应用Cluster是如何提高iOS访问权限通过率的。申请访问权限的第一次很关键。开发者要在第一次提问时取得用户的允许。Cluster开源了相关的代码，非常值得学习。

* Viewfinder开源  
移动支付公司Square将去年收购的照片应用Viewfinder开源了，包含服务端、iOS 和 Android 应用代码。


###工具

* Github的文本编辑器Atom    
Atom已经开源，不需邀请码即可下载使用。


###设计

* subjc.com － a study of innovative iOS interfaces  
如果说 objc.io 专注于 iOS 开发中用户看不见的那部分，那么 subjc.com 是为了用户中看的见的交互而出现的，有动画实例，分析，还有实现的代码。  



<br>
更多内容，请访问[iOS技术分享第15期][2]查看新一期的内容。  


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
[2]:http://weekly.ios-wiki.com/issues/15
[3]:http://twitter.com/daveverwer
[4]:https://iosdevweekly.com/
[5]:http://news.ios-wiki.com
[6]:http://www.ios-wiki.com
[7]:http://weekly.ios-wiki.com