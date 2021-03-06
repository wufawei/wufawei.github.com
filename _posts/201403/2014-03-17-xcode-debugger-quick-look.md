---
layout: post   
title: Xcode5.1调试器之Quick Look     
categories: 
- iOS   
tags:     
- iOS
---    
 
 
###Quick Look之显示系统类
Xcode 5.0中引入并在Xcode 5.1中增加进一步支持的一个有趣功能就是显示变量的Quick Look功能。比如如下图所示，你可以点击眼睛图标来显示调试器中的变量，如下图所示，你可以看到这个UIColor的颜色。

![](http://farm4.staticflickr.com/3810/13231223974_d2ce69b543_b.jpg)


其他支持的系统类（system classes）包括 UIImage, UIImageView, CIImage, UIColor,
UIBezierPath, CLLocation, UIView, NSString, NSAttributedString, NSData, NSURL

举2个例子：

显示UIView的子类的例子如下：

![](http://farm4.staticflickr.com/3788/13230582493_2194af8616.jpg)

显示URL的例子如下：

![](http://farm3.staticflickr.com/2872/13230670844_4fafb22ce1_b.jpg)



<br>

###Quick Look之显示自定义类

除了上述的系统类之外，自定义的类也可以通过类似的方式在Debug的时候用Quick Look显示。


方法就是: **在自定义的类里面实现 - (id)debugQuickLookObject**



  
    - (id)debugQuickLookObject {
        
        /*return @"在自定义的类里面实现 - (id)debugQuickLookObject  就可以在Debug的时候现实Quick Look信息，方便调试";*/ 
        
        
        return [[CLLocation alloc] initWithCoordinate:CLLocationCoordinate2DMake(116.35405, 39.84671)
                                             altitude:1000
                                   horizontalAccuracy:1000
                                     verticalAccuracy:1000
                                               course:100
                                                speed:1
                                            timestamp:[NSDate date]];//上面的CLLocation
    }
 
 
 这样就可以在自定义类中显示自定义的信息。
 
 上面的代码既可以显示文本信息，也可以显示CLLocation信息。

显示文本信息的例子如下：

  ![](http://farm4.staticflickr.com/3772/13230823724_a938eeb60d_b.jpg)



 
 
 显示CLLocation的例子如下图：

![](http://farm8.staticflickr.com/7237/13230392323_a032147890_b.jpg)

 
 也可以显示自定义类的NSAttributedString信息，如下面的代码所示：
 
     - (id)debugQuickLookObject
    {
      NSAttributedString *cr = [[NSAttributedString alloc] initWithString:@"\n"];
      NSMutableAttributedString *result = [[NSMutableAttributedString alloc] initWithAttributedString:self.numberLabel.attributedText];
      [result appendAttributedString:cr];
      [result appendAttributedString:self.lineLabel.attributedText];
      return result;
    }

 


<br>

参见：[xcode 5.1 Xcode Debugger Quick Look ][1]

<br>

###特别注意

Quick Look功能对于UIView和UIColor来说，并不是每次都能正常显示，这个时候只用把对应的变量类型改为id，就可以显示了。如下图所示：

![](http://farm4.staticflickr.com/3810/13231223974_d2ce69b543_b.jpg)



<br>

###关于我
大家好，我是[@吴发伟Ted](http://weibo.com/wufawei)，我会在博客上分享自己学习的一些东西。

###好文推荐  && iOS Wiki

[好文推荐(news.ios-wiki.com)][5]和[iOS Wiki(www.ios-wiki.com)][6]是我利用业余时间建立的网站。

###微信公众账号
微信公众账号：**iOS技术分享**  
![](http://farm3.staticflickr.com/2826/10855679484_56b7429bd6_m.jpg)


 
[1]:http://news.ios-wiki.com/item?id=164

[5]:http://news.ios-wiki.com/newest
[6]:http://www.ios-wiki.com
