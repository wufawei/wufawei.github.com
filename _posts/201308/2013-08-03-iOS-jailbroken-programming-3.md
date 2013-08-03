---
layout: post  
title: iOS越狱程序开发（3）－ Your First Tweak  
categories:  
- iOS  
tags:    
- security  programming
---   
 

本文是iOS越狱程序开发系列文章的第3篇。

本系列包含下面4篇文章:  
[iOS越狱程序开发（1）－ 工具篇](http://wufawei.com/2013/08/iOS-jailbroken-programming-1/)        
[iOS越狱程序开发（2）－ 构建和部署 ](http://wufawei.com/2013/08/iOS-jailbroken-programming-2/)  
[iOS越狱程序开发（3）－ Your First Tweak](http://wufawei.com/2013/08/iOS-jailbroken-programming-3/)   
[iOS越狱程序开发（4）－ 总结](http://wufawei.com/2013/08/iOS-jailbroken-programming-4/)


本文是iOS越狱开发系列文章的第3篇。确保你已经读过前面两篇文章。今天的文章将介绍如何给任意的Apple提供的方法打补丁。在这个demo中，我们将要hook Springboard的init方法，然后在iphone启动时显示一个UIAlertView。这个demo不是最酷的，但是这里所使用的方法和模式，可以用来给任何class的任何method打补丁。

<br/>
#### **1 准备工作**

你首先还需要下载Saurik的libsubstrate.dylib，然后copy到/opt/theos/lib

[下载libsubsrate.dylib](http://www.mediafire.com/?2upm53uzzj0488u)

<br/>
#### **2 iOS 头文件**

很可能theos本身就自带了你所需要的头文件，但是，如果你编译程序的时候提示你头文件相关的问题，那你就需要准备相关的头文件了。要么从设备上dump头文件，要么google，建议你先google一下，看其他人有没已经提供了这些头文件。

一旦你有这些头文件，记得把它们放在/opt/theos/include。

<br/>
#### **3 创建项目**

执行
$THEOS/bin/nic.pl

    author$ $THEOS/bin/nic.pl
    NIC 1.0 - New Instance Creator
    ——————————
      [1.] iphone/application
      [2.] iphone/library
      [3.] iphone/preference_bundle
      [4.] iphone/tool
      [5.] iphone/tweak


这里，需要选择5，demo例子如下：

    NIC 1.0 - New Instance Creator
    ——————————
      [1.] iphone/application
      [2.] iphone/library
      [3.] iphone/preference_bundle
      [4.] iphone/tool
      [5.] iphone/tweak
    Choose a Template (required): 5
    Project Name (required): WelcomeWagon 
    Package Name [com.yourcompany.welcomewagon]: 
    Author/Maintainer Name [Brandon Trebitowski]: 
    MobileSubstrate Bundle filter [com.apple.springboard]: 
    Instantiating iphone/tweak in welcomewagon/…
    Done.

<br/>
#### **4 The Tweak File**

一旦你创建了项目，你会发现Theos生成了一个叫做Tweak.xm的文件，这是个特殊的文件，hook的相关代码就将写在这个文件。

默认的所有代码都是被注释起来的。

**%hook 和 %end**

    %hook Springboard
    // overwrite methods here
    %end

%hook后面跟的是你要hook的类名称，以一个%end结尾。上面的代码说明我们会hook Springboard类里面的method。

**%orig**

当在一个method内部的时候，%orig会调用原来的方法（original method)。你甚至可以给原来的method传递参数，例如：%orig(arg1,arg2)。如果你不调用%orig，原来的方法就绝对不会被调用。所以，如果你hook了SpringBoard的init方法，但是没有调用%orig。那么你的iphone就将不可用，除非你通过ssh删除你的app。


<br/>
#### 5  Hooking into Springboard ##

打开Tweak.xm，然后加上代码：


    #import <SpringBoard/SpringBoard.h>
    
    %hook SpringBoard
    
    -(void)applicationDidFinishLaunching:(id)application {
    %orig;
    
    UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Welcome" 
    message:@"Welcome to your iPhone Brandon!" 
    delegate:nil 
    cancelButtonTitle:@"Thanks" 
    otherButtonTitles:nil]
    [alert show];
    [alert release];
    }
    
    %end


首先，import头文件 Springboard.h，这可以让我们可以访问springboard。然后，我们告诉预处理器hook Springboard class。

这里覆盖的method是applicationDidFinishLaunching：方法，当Springboard启动时，就会被执行。注意我们调用了%orig。

最后，显示一个UIAlertView。


<br/>
#### 6 添加Framework

如果你直接编译，，会得到如下的提示信息：

    Tweak.xm: In function ‘objc_object* $_ungrouped$SpringBoard$init(SpringBoard*, objc_selector*)’:
    Tweak.xm:6: error: declaration of ‘objc_object* self’ shadows a parameter


那是因为我们依靠UIKit framework来显示alert,所以需要在Makefile中加上如下一行：

WelcomeWagon_FRAMEWORKS = UIKit


<br/>
#### 7 Building, Packaging, Installing.

在前面的系列中介绍了如何编译，打包和安装，依次执行下面的命令即可。

**make, make package, make install**

安装之后，你将看到这个：

![](http://f.cl.ly/items/3J1W0j0c1F3y081P2D2i/IMG_0001.PNG)

本文介绍了如何hook Springboard并显示Alert的方法和步骤。
通过Tweak可以做任何你想做的事情，因为你可以**hook任何你感兴趣的class的method**。


<br/>


参考文献：

[Beginning Jailbroken iOS Development – Your First Tweak](http://brandontreb.com/beginning-jailbroken-ios-development-your-first-tweak)

<br/><br/>
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