---
layout: post  
title: iOS越狱程序开发（4）－ 总结 
categories:  
- iOS  
tags:    
- security  programming
---   
 
本文是iOS越狱程序开发系列文章的第4篇。
本系列包含下面4篇文章:  
[iOS越狱程序开发（1）－ 工具篇](http://wufawei.com/2013/08/iOS-jailbroken-programming-1/)        
[iOS越狱程序开发（2）－ 构建和部署 ](http://wufawei.com/2013/08/iOS-jailbroken-programming-2/)  
[iOS越狱程序开发（3）－ Your First Tweak](http://wufawei.com/2013/08/iOS-jailbroken-programming-3/)   
[iOS越狱程序开发（4）－ 总结](http://wufawei.com/2013/08/iOS-jailbroken-programming-4/)


从前面几篇文章可以看到，开发一个越狱程序是多么的简单。所以介绍这几篇文章，是为了知己知彼，只有我们知道了程序存在的risk，程序可能被hack的方式，才能使得我们在写程序时，写出更健壮，更安全的程序。

之前写过的[keychain is not safe](http://wufawei.com/2013/06/Keychain-is-not-safe/)指出了在keychain中保存的数据也不是那么的安全，[iOS Application Security](http://wufawei.com/2013/07/iOS-application-security/)中更是介绍了如何dump出app、如何动态修改app的方法。

这几篇iOS越狱开发系列文章，更是指出了hacker可以hook任意的api，获取我们的敏感信息和数据。

安全是相对的，Apple本身提供的机制相对是比较安全的，Apple也提示用户越狱存在risk，但是，作为开发者，我们开发的app，很可能也会被越狱用户使用，那在这些设备上，首先是用户相关数据的安全存在risk，比如用户名，密码，甚至是支付密码。其次是我们app本身的敏感信息，比如和server通信，我们总会有些secret，这些数据，我们是绝对是希望不被别人获取到的。

但是，大家从iOS越狱开发（3）－ Your first Tweak可以知道如何去hook任意method，如果我们本身的API、对安全的考虑不够健壮，那就只有自求多福了。

很多IM，即时聊天工具在本地聊天记录存的是明文，如Whatsapp这类采用xmpp协议与server交互的，在很长一段时间内都是传输的明文。


写这个系列，是为了让大家知道，即使是安全如iOS，如果在越狱设备上使用，也会存在极大的安全隐患。


所以，对于普通用户，**建议不要越狱**。对于开发者，要多考虑下用户和自身app的安全。


题外话，即使是没有越狱，也可能会有问题，在black hat 2013上，有人演示了如何通过充电器给iOS设备注入恶意代码。

<br/>

参考文章

[How to Hack WhatsApp Messenger | Build WhatsApp API Client](http://geeknizer.com/how-to-hack-whatsapp-messenger/)

[MACTANS: INJECTING MALWARE INTO IOS DEVICES VIA MALICIOUS CHARGERS](https://media.blackhat.com/us-13/US-13-Lau-Mactans-Injecting-Malware-into-iOS-Devices-via-Malicious-Chargers-WP.pdf)

[How To Create A MobileSubstrate Tweaks for iOS](http://ios-blog.co.uk/tutorials/how-to-create-a-mobilesubstrate-tweaks-for-ios/)

（转载本站文章请注明作者和出处 吴发伟Ted – wufawei.com ，请勿用于任何商业用途）