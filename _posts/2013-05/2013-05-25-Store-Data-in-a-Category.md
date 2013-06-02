---
layout: post
title: Store Data in a Category
categories:
- Programming
tags:
- iOS
---
当需要给一个已有的类增加方法的时候，我们会使用iOS的category。比如NSString中的各种drawing 方法，都是category methods,声明在UIKit中的UIStringDrawing.h。
使用category的时候有个不足，不能申明实例变量。 Objecttive-C Programming Guide这样说：

***“Note that a category can’t declare additional instance variables for the class; it includes only methods.”***

**解决方法：Associated References**

简单说就是通过2个API：

    void objc_setAssociatedObject(id object, void *key, id value, objc_AssociationPolicy policy)；
    void objc_getAssociatedObject(id object, void *key, id value, objc_AssociationPolicy policy)；

这里的要注意：key不是一个string，它是一个内存中固定的地址。需要保证设置和获取value的时候使用的是一个同样的地址，使用static varable可以保证。
 
一般像这样定义：
static char someKey;
	
Have Fun!

1）[Monkey-Patching iOS with Objective-C Categories Part II: Adding Instance Properties](http://blog.carbonfive.com/2012/11/27/monkey-patching-ios-with-objective-c-categories-part-ii-adding-instance-properties/)    
2）[iOS Recipes:Tips and Tricks for Awesome iPhone and iPad Apps.](http://www.amazon.com/iOS-Recipes-Awesome-Pragmatic-Programmers/dp/1934356743)

