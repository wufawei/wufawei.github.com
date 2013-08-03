---
layout: post  
title: iOS越狱开发（2）－ 构建和部署  
categories:  
- iOS  
tags:    
- security  programming
---   
 
本文是iOS越狱开发系列文章的第2篇。

本系列包含下面4篇文章:  
[iOS越狱开发（1）－ 工具篇](http://wufawei.com/2013/08/iOS-jailbroken-programming-1/)        
[iOS越狱开发（2）－ 构建和部署 ](http://wufawei.com/2013/08/iOS-jailbroken-programming-2/)  
[iOS越狱开发（3）－ Your first Tweak](http://wufawei.com/2013/08/iOS-jailbroken-programming-3/)   
[iOS越狱开发（4）－ 总结](http://wufawei.com/2013/08/iOS-jailbroken-programming-4/)



开发越狱程序和日常开发的iOS程序很相似，不过，越狱程序能做更强大的事情。你的设备越狱之后，你就能够hook进Apple提供的几乎所有的class，来控制iPhone/iPad的功能。
本文将介绍如何将于开发出来的越狱程序构建和部署到真实设备上去，建议先看系列文章的第一篇-工具篇。


#### **介绍**

工具篇中介绍了如何创建一个新的工程。下面是一个创建jailbroken 应用程序的例子：

    author$ $THEOS/bin/nic.pl
    NIC 1.0 - New Instance Creator
    ——————————
      [1.] iphone/application
      [2.] iphone/library
      [3.] iphone/preference_bundle
      [4.] iphone/tool
      [5.] iphone/tweak
    Choose a Template (required): 1
    Project Name (required): firstdemo
    Package Name [com.yourcompany.firstdemo]: 
    Author/Maintainer Name [Author Name]: 
    Instantiating iphone/application in firstdemo/…
    Done.


这将会创建一个新的目录firstdemo，并且有以下文件。
1. control: 包含applicaton/tweak的信息，当你从Cydia安装时，你可以看到这些信息，包括名字，作者，版本，等等。
2. main.m，这个不用多说。
3. [applicationName]Application.mm:appdelegate文件。
4. Makefile：包含必要的编译命令
5. Resources：包含info.plist文件等
6. RootViewController.h/mm

#### **Makefile**

这里重点介绍下：  

    include theos/makefiles/common.mk  
    APPLICATION_NAME = firstdemo  
    [applicationName]_FILES = main.m firstdemoApplication.mm RootViewController.mm  
    [applicationName]_FRAMEWORKS = UIKit Foundation QuartzCore AudioToolbox CoreGraphics


#### **设置环境变量**

打开terminal然后输入

    export THEOS=/opt/theos/
    export SDKVERSION=7.0
    export THEOS_DEVICE_IP=xxx.xxx.xxx.xxx

第二行定义你当前的SDK版本，我本机装的是7.0，最后一行定义你的设备的ip地址。


#### **构建工程**


**第一个命令:make**

    $ make
    Making all for application firstdemo…
     Compiling main.m…
     Compiling firstdemoApplication.mm…
     Compiling RootViewController.mm…
     Linking application firstdemo…
     Stripping firstdemo…
     Signing firstdemo…


**第二个命令：make package**


    make package
    Making all for application firstdemo…
    make[2]: Nothing to be done for ‘internal-application-compile’.
    Making stage for application firstdemo…
     Copying resource directories into the application wrapper…
    dpkg-deb: building package ‘com.yourcompany.firstdemo’ in ‘/Users/author/Desktop/firstdemo/com.yourcompany.firstdemo_0.0.1-1_iphoneos-arm.deb’.


**第三个命令：make install**


    $ make package install
    Making all for application firstdemo…
    make[2]: Nothing to be done for `internal-application-compile’.
    Making stage for application firstdemo…
     Copying resource directories into the application wrapper…
    dpkg-deb: building package ‘com.yourcompany.firstdemo’ in ‘/Users/author/Desktop/firstdemo/com.yourcompany.firstdemo_0.0.1-1_iphoneos-arm.deb’.
    ...
    root@ip’s password: 
    ...

   
   这个过程会提示你输入几次iphone或者ipad的密码。默认是:**alpine**.



本文介绍了如何创建一个jailbroken app/tweak/hack的工具，然后编译和上传到设备的方法。


下面将继续介绍如何编写hack程序。




参考文献：

[Beginning Jailbroken iOS Development – Building And Deployment](http://brandontreb.com/beginning-jailbroken-ios-development-building-and-deployment)


（转载本站文章请注明作者和出处 吴发伟Ted – wufawei.com ，请勿用于任何商业用途）

<br />


---------这是分割线----------
<br />

1) 如果你是因为别人的分享来到这里的，可以到微信，点击查看公共账户，添加
   iosinfo(iOS技术分享)为好友，会分享iOS开发相关技巧，有时也会发一些我觉得有趣的东西。

  
2)  关于我。  

新浪微博: [@吴发伟Ted](http://weibo.com/wufawei)

**微信公众账号：iOS技术分享**

 **点击     **<http://t.cn/zHpnevd>**   查看所有的历史消息。**（微信上打开此URL）

                 
![](http://farm3.staticflickr.com/2861/8836295022_023774dd2f_m.jpg)