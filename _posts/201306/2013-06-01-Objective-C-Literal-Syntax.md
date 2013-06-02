---
layout: post  
title: Objective-C Literal Syntax  
categories:  
- Programming  
tags:
- iOS
---

**1 Objective-C Literal Syntax**  
从Xcode 4.5开始，可以在Objective-C中以literal Syntax来定义NSNumber, NSArray和NSDictionary。Literal Syntax，简单说就是以@@字符开始的方式简单地创建数组、字典、NSNumber常量。

定义一个NSString对象，最简单的方法就是用下面的方法：

NSString *aString = @"This is a test string";

定义NSNumber, NSArray和NSDictionary也可以用类似的方法了。

NSNumber literals:

    NSNumber *n1 = @1000;  // [NSNumber numberWithInt:1000] 
    NSNumber *n2 = @3.1415926; // [NSNumber numberWithDouble:3.1415926]
    NSNumber *anotherInteger = @42; //NSNumber *anInteger = [NSNumber numberWithInt:42];


NSArray literals:

    // before
    NSArray *words = [NSArray arrayWithObjects:@"array", @"of", @"words", nil];
    // after (array with some strings and numbers)
    NSArray *words = @[@"array", @"of", @"words", @1, @1.4];




NSDictionary literals


    //Old way
    NSDictionary *oldDictionary = [NSDictionary dictionaryWithObjectsAndKeys:@1, @"key1", @2, @"key2"];
    
    //New way to do it
    NSDictionary *newDictionary = @{ @”key1” : @1,  @”key2”: @2 };
    

**2 copy vs mutablecopy**  
NSMutableDictionary的对象copy之后，是不可变（immutable）的。
比如：       
NSMutableDictionary *demoMutable = [NSMutableDictionary alloc] init];  
id copyResult = [demoMutable copy];  
copyResult是不可变的。可以打印其class查看。

​http://stackoverflow.com/questions/1768937/how-do-i-convert-nsmutablearray-to-nsarray

**3 小技巧，如何打印frame**  
一般会用：  
`    NSLog(@"Frame is %g, %g, %g, %g", frame.origin.x, frame.origin.y, frame.size.width, frame.size.height);`  
更方便：  
    `NSLog(@"Frame is %@", NSStringFromCGRect(frame));`

**4 WWDC 2013即将开幕，期待**

Have Fun!






