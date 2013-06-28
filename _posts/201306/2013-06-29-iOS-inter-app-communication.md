---
layout: post  
title: iOS app之间共享数据的几种方法
categories:  
- iOS  
tags:    
- UIPasteboard    
---   
 
 
iOS app之间要共享数据不是那么容易，因为每个app都是sandbox。然后，有时候又不得不跟其它app之间共享数据，那应该怎么办呢？

下面是一些常用方法的总结：

* UIDocumentInteractionController   
 Availability: iOS 3.2+  
  具体用法参见：http://mobile.tutsplus.com/tutorials/iphone/previewing-and-opening-documents-with-uidocumentinteractioncontroller/

* UIActivityViewController    
 Availability: iOS 6.0+

* Shared Keychain Access  
  这个要求app之间用的是同样的证书

* Custom URL Scheme  
  通过构造URL，把数据作为参数传递过去。 本地测试过，传递10000个字符都可以，不过不要太长，内存可能吃不消。
* Web Service 
  通过dropbox或者其他第三方的服务来共享数据。  

*  UIPasteboard + URL Scheme
  通过URL scheme传递UIPasteboard的名称，然后通过UIPasteboard共享数据。  
  微信iOS SDK应该采用的就是这种方式。  
  不过在iOS 7上，这种方法会存在问题，如果采用这种方案，得赶紧想办法解决。



1) <http://enharmonichq.com/sharing-data-locally-between-ios-apps/>

2) <http://stackoverflow.com/questions/17080074/ios7-beta-doesnt-allow-inter-app-communication-by-uipasteboard>       

