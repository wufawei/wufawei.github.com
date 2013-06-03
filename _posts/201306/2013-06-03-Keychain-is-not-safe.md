---
layout: post  
title: Keychain is not safe  
categories:  
- programming  
tags:
- iOS
---

## 1 Keychain ##

一般来说mobile app都需要在本地保存一些较为敏感的数据。如何安全的保存这些数据就是一个值得深入探讨的问题。

Mac OS 可以利用Keychain保存各应用中用户的账号密码，让用户不用重复输入，在iOS中也有Keychain，也可以在应用之间共享数据，只是有些限制，用户无法通过手动控制。

KeyChain中的所有数据都以key－value的形式进行存储，可以对其进行add、update、get、delete操作。

如果需要在应用里使用keyChain，需要导入Security.framework，keychain的操作接口声明在头文件SecItem.h里。直接使用SecItem.h里方法操作keychain，需要写的代码较为复杂，可以使用已经封装好了的工具类SFHFKeychainUtils，见：https://github.com/ldandersen/scifihifi-iphone/tree/master/security 


对每个应用来说，Keychain都有两个访问区，私有区和公共区。私有区是一个sandbox，本程序存储的任何数据都对其他程序不可见。Keychain中保存的信息是用app unique签名了的，默认只有自己能够访问。



keychain的access group的概念。  

**a)app保持的信息是用一个app unique 签名了的，默认只有自己能够访问**。

" Each application on an iOS device has a unique “application-identifier” that is cryptographically signed into the application before being submitted to the Apple App store.  The keychain service restricts which data an application can access based on this identifier.  By default, applications can only access data associated with their own application-identifier。By default, when no access group is specified, the application will use the unique application-identifier as the access group (thus limiting access to the application itself)"

**b）不同app之间可以通过access_group共享**

app1的group是 app1.accessgroup.item1,

app2在entitlements中加入这个item就可以访问了。

**c） 在不同app之间共享，只能在同一个公司内部的app共享**。  
因为keychain access group 所在的文件entitlements.plist，需要添加到code_sign_entitlements.

这个文件的路径要配置在 Project->build setting->Code Signing Entitlements里，否则公共区无效，配置好后，须用你正式的证书签名编译才可通过，否则xcode会弹框告诉你code signing有问题。所以，苹果限制了你只能同公司的产品共享KeyChain数据，别的公司访问不了你公司产品的KeyChain。


很多app都这样保存用户密码，可是，**这样就安全了吗？**

## 2 Keychain is not safe ##

对于没有越狱的设备，上述做法确实很安全。但是，**对于jail break之后的设备，风险就很大了**。

通过上面的分析，我们知道要访问keychain里面的数据，需要


1）和app同样的证书

jail break 相当于对苹果做签名检查的地方打了个补丁，使得不是苹果颁发的证书签名的文件，甚至伪造的签名文件签名的app也能正常使用。所以这个可以轻松绕过。  
2）获得access group的名称  
[Keychain Dumper Updated for iOS 5](http://labs.neohapsis.com/2012/01/25/keychain-dumper-updated-for-ios-5/) 介绍了如何获得acess group。

其实也可以不必获得access group，因为access的匹配是可以用正则表达式的，也就是用*就可以匹配所有group了。
例子如下：


     <dict> <key>keychain-access-groups</key>
     <array> <string>*</string>  </array> </dict></plist>


3）在设备上执行2)中介绍的keychain dumper，就可以得到所有的相关信息。
但是，要在社保上执行keychain dumper，就需要用chmod 777设置其权限，需要root权限，而jail break之后的默认密码是：alpine。


最后可以获得好几个文件，下面是里面的2个例子。（密码都被我用password字串替换）

a) 是家里的WIFI信息  
![](http://farm6.staticflickr.com/5446/8937211262_0a3c9a75b6.jpg)

b）是某知名微博

![](http://farm6.staticflickr.com/5446/8937211262_0a3c9a75b6.jpg)

**我在越狱之后的iOS 5.1的iPhone，iPad, iOS 6.1.2的iPad上都测试过，都可以获得如上信息。**
实际中的例子远不止这2个。很多应用都是直接存用户的明文密码的。


## 3 个人如何防止信息泄露 ##
 a）修改root的默认密码。  
 b) 安装能信任的jail break app。

## 4 对开发者和公司 ##
 **不要保存用户的明文密码。**  
 **Encryption is a must for sensitive data。**


**Have Fun!**

1 http://iosdeveloper.diandian.com/post/2012-09-05/40038606930

2 [Keychain dumper usage explained](http://www.securitylearn.net/2012/03/27/keychain-dumper-usage-explained/)

3 [iOS security](http://images.apple.com/ipad/business/docs/iOS_Security_May12.pdf)

4 [“Researchers steal iPhone passwords in 6 minutes”…true…but not the whole story"](http://labs.neohapsis.com/2011/02/28/researchers-steal-iphone-passwords-in-6-minutes-true-but-not-the-whole-story/)

5 [Keychain Dumper Updated for iOS 5](http://labs.neohapsis.com/2012/01/25/keychain-dumper-updated-for-ios-5/)

     