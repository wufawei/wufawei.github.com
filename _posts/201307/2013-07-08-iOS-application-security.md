---
layout: post  
title: iOS Application Security
categories:  
- iOS  
tags:    
- security    
---   
 
 
文章分A,B,C,D 4个部分。

###A) iOS Application Security ##
<br />
下面介绍iOS应用安全，如何分析和动态修改app。    
1）[iOS Application security Part 1 – Setting up a mobile pentesting platform](http://resources.infosecinstitute.com/ios-application-security-part-1-setting-up-a-mobile-pentesting-platform/) 
Part1介绍如何在越狱的设备上搭建用来测试iOS安全的环境。

2）[iOS Application security Part 2 – Getting class information of IOS apps](http://resources.infosecinstitute.com/ios-application-security-part-2-getting-class-information-of-ios-apps/) 
Part2介绍如何利用class-dump-z 和 Clutch 来dump类信息，利用这些信息，可以理解代码的设计和代码内部是如何工作的。  
**有没有可能我们去动态修改app呢？**  
例如有一个方法 -(BOOL)isFacebookSessionValid ，在某种情况下返回 NO，有没有方法操纵它让它返回YES呢？  
可不可以在运行时修改 instance variable变量的值呢？  
**答案是：YES**。  

3) [iOS Application security Part 3 – Understanding the Objective-C Runtime](http://resources.infosecinstitute.com/ios-application-security-part-3-understanding-the-objective-c-runtime/) 
Part3提到Method Swizzling.我之前有篇blog有过介绍，参见：[Monkey Patching iOS with Swizzling](http://wufawei.com/2013/06/Monkey-Patching-iOS-with-Swizzling/)

4) [iOS Application Security Part 4 – Runtime Analysis Using Cycript (Yahoo Weather App)](http://resources.infosecinstitute.com/ios-application-security-part-4-runtime-analysis-using-cycript-yahoo-weather-app/)

Part4介绍了用Cycript动态分析和修改app的方法。文章拿Yahoo Weather app做的例子。其中一处改动是给加上了badge number。

![](http://farm3.staticflickr.com/2823/9241398663_b80335cd23.jpg)


5）[IOS Application security Part 5 – Advanced Runtime analysis and manipulation using Cycript (Yahoo Weather App)](http://resources.infosecinstitute.com/ios-application-security-part-5-advanced-runtime-analysis-and-manipulation-using-cycript-yahoo-weather-app/)  

Part5 分享了一些高级分析技术。分析了如何获得特定类的信息（方法名，实例变量名称），并且如何在运行时修改。   

6）[IOS Application Security Part 6 – New Security Features in IOS 7](http://resources.infosecinstitute.com/ios-application-security-part-6-new-security-features-in-ios-7/)

7）[IOS Application Security Part 7 – Installing and Running Custom Applications on Device without a registered developer account](http://resources.infosecinstitute.com/ios-application-security-part-7-installing-and-running-custom-applications-on-device-without-a-registered-developer-account/)
<br />
<br />
### B）关键数据的保存。 ##
在本地保存的数据，一般来说，要么:  
1）存本地db。      
2）存plist文件。  ([NSUserDefaults standardUserDefaults] 也是一个plist文件)  
3）存入keychain。  

如果直接保存明文，即使在keychain中，也不安全。
之前我写过一篇blog，[**keychain is not safe**](http://wufawei.com/2013/06/Keychain-is-not-safe/)，里面提到了某微博，其实就是**某微博客户端**，**把用户密码存在keychain中，在一定条件下，也是可以提取出来的**。
   
 (下面图中password所在位置就是我在某微博的密码，为了演示，这里做了修改）



![](http://farm6.staticflickr.com/5446/8937211262_0a3c9a75b6.jpg)


所以，关键数据是需要加密的。Encrytion is a must for sensitive data.

   
<br />
### C）通信的安全。  
一般来说，http请求都应该要做到能够**防篡改、防重放攻击**（replay attack）等等，如果安全要求更高，还要用https，
在客户端用https，也要注意对源的校验。（[通过SSL Pining提供iOS SSL通信的安全](http://wufawei.com/2013/05/SSL-Pining/)）。

比如一些涉及到关键业务的东西，不能够直接给个URL，任何人都能从PC上就下载，一定要做身份校验，甚至要有time out值（URL里面有时间戳）。  

**比如最近大家看到的关于支付宝插件的分析相关文章(参见：[分析支付宝客户端的插件机制](http://blog.devtang.com/blog/2013/06/23/alipay-plugin-mechanism/)，[浅析支付宝钱包插件](http://imallen.com/blog/2013/06/26/inside-alipay-plugin.html)，[再谈支付宝钱包插件和说好的 Demo](http://imallen.com/blog/2013/07/06/about-alipay-plugin-and-phonegap.html))，如果是你来设计，请问会从哪些方面来加强安全呢。**

我想到这些：  
 1）**插件内容加密**。  
   这里做到加密关键部分即可。在加载插件对应功能的时候，把对应功能**解密到内存**中，文件系统中还是保持加密状态。  

 2) **下载插件的URL做身份验证**。  

 3)   **插件里面的JS做混淆**。  

 4）**关键逻辑和代码，放在native实现**。  

 5）**加入冗余代码，让代码不易被看懂**。  

<br />


### D）安全 VS 成本 ##


对于个人用户来说，尽量不要越狱，不使用免费WIFI登陆敏感网站。（很多免费WIFI传输都不加密，加密的密码很可能也是123456这种，很容易被hack；或者这个WIFI，本身就是别有企图的人提供的）。


对企业来说，就既要考虑通信的安全，也要考虑客户端的安全等等。直接把用户的密码存在本地，安全上是有风险。



**安全是相对的**，A）中分享的文章可以看到，即使把关键逻辑放在iOS native code中去实现，也是可能被hack，被破解的。但是，这个需要的技巧和能力就会要求更高。

**你的业务是什么，你打算以多大的成本来提高安全性，业务越critical，要求越高。**

对于和钱打交道的app，至少，你需要提高门槛。

**You should raise the bar.**        

<br/>
<br/>

（转载本站文章请注明作者和出处 吴发伟Ted – wufawei.com ，请勿用于任何商业用途）