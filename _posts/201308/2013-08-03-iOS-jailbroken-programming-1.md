---
layout: post  
title: iOS越狱开发（1）－ 工具篇  
categories:  
- iOS  
tags:    
- security  programming
---   
 
本文是iOS越狱开发系列文章的第1篇。

本系列包含下面4篇文章:  
iOS越狱开发（1）－ 工具篇        
iOS越狱开发（2）－ 构建和部署   
iOS越狱开发（3）－ Your first Tweak   
iOS越狱开发（4）－ 应用篇


开发越狱程序和日常开发的iOS程序很相似，不过，越狱程序能做更强大的事情。你的设备越狱之后，你就能够hook进Apple提供的几乎所有的class，来控制iPhone/iPad的功能。
本文将介绍写越狱程序需要的工具。

#### **Theos**

@DHowett的theos大幅简化了编写越狱程序的流程。DHowett介绍了如何再Mac和Linux上开发iOS越狱程序，本文将只介绍如何在Mac上开发。

#### Step1:安装iOS SDK。 ####

<http://developer.apple.com/devcenter/ios/index.action>

#### Step2:设置环境变量


建议把theos安装在/opt/theos， 打开terminal然后输入
    
    export THEOS=/opt/theos

通过在命令行执行 echo $THEOS可以看到这个变量是否正确设置。每次你打开terminal都需要重新设置一下。


#### Step3:下载theos


在ternimal中输入：

    svn co http://svn.howett.net/svn/theos/trunk $THEOS

会把theos下载到Step2所设置的目录中，会提示你输入admin的密码。

#### Step4:下载ldid

ldid的作用是模拟给iPhone签名的流程，使得你能够在真实的设备上安装越狱的apps/hacks。

你可以在很多地方都找得到这个tool，不过DHowett在他的dropbox中给大家存了一份。

通过下面的命令下载：

    curl -s http://dl.dropbox.com/u/3157793/ldid > ~/Desktop/ldid
    chmod +x ~/Desktop/ldid
    mv ~/Desktop/ldid $THEOS/bin/ldid


先是下载到桌面，然后改执行权限，然后移动到指定目录。

你可以尝试下看看直接下载是否ok：  

    curl -s http://dl.dropbox.com/u/3157793/ldid > $THEOS/bin/ldid; chmod +x $THEOS/bin/ldid


由于伟大的墙，下载这个你需要自备梯子。




#### Step5:安装dkpg

Dpkg能够把你的app打包成Debian Package,可以分发的Cydia的存储目录中。

    sudo port install dpkg.


#### Step6:创建新的项目

theos使用一个叫做nic(new instance tool)的工具来创建新的工程。执行下面的命令：

    $THEOS/bin/nic.pl

就可以开始创建。下面是一个创建jailbroken 应用程序的例子：

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

简单这样的命令，就创建了一个基本的越狱程序firtdemo,它除了常规的文件外，还包含了Makefile，以及control文件（当在Cydia中时，显示的关于程序的信息）。



本文介绍了如何创建一个jailbroken app/tweak/hack的工具和步骤。


下面将继续介绍如何构建和部署到真实设备上去。


<br/>

参考文献：

[Beginning Jailbroken iOS Development – Getting The Tools](http://brandontreb.com/beginning-jailbroken-ios-development-getting-the-tools)

<br/>


（转载本站文章请注明作者和出处 吴发伟Ted – wufawei.com ，请勿用于任何商业用途）


---------这是分割线----------
<br />

1) 如果你是因为别人的分享来到这里的，可以到微信，点击查看公共账户，添加
   iosinfo(iOS技术分享)为好友，会分享iOS开发相关技巧，有时也会发一些我觉得有趣的东西。

  
2)  关于我。  

新浪微博: [@吴发伟Ted](http://weibo.com/wufawei)

**微信公众账号：iOS技术分享**

 **点击     **<http://t.cn/zHpnevd>**   查看所有的历史消息。**（微信上打开此URL）

                 
![](http://farm3.staticflickr.com/2861/8836295022_023774dd2f_m.jpg)