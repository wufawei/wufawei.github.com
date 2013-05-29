---
layout: post
title: Life-Before-ARC(2)
categories:
- Programming
tags:
- iOS
---


**三. 自动释放池的实现原理**  


通过调用autorelease,可以创建并返回一个对象，但是不拥有其所有权。Autorelease提供了一种机制，当对象的生命周期结束时候，它会释放掉对象。

下图是release和autorelease的区别:    
![](http://farm6.staticflickr.com/5339/8867321717_2257ec65c7_m.jpg)


下面我们先看看C语言中的自动变量，然后看看GNUstep的源码以及Apple的实现来理解autorelease是如何工作的。

**Automatic Variables**

    {
       int a;
    }

当程序离开这个范围的时候，a就会被自动处理。

使用autorelease，你可以像使用自动变量的方式来使用对象。

    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
    id obj = [[NSObject alloc] init];
    [obj autorelease];
    [pool drain];

当[pool drain]调用时，所有的autoreleas的对象的release方法都会被调用。


下图是应用的 main loop, NSAutoreleasePool在NSRunLoop中被创建，使用和回收，      

 ![](http://farm6.staticflickr.com/5339/8879737754_9da3a9788b_b.jpg)
 

但是，当有太多的autorelease对象的时候，内存可能会吃紧，这种情况下就应该创建和回收NSAutoreleasePool。

    for (int i = 0; i < numberOfImages; ++i) {
       NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
       /* loading images, etc, 
        * Too many autoreleased objects exist.
       */
       [pool drain];
    }

在Cocoa中，有很多返回autoreleased对象的例子，例如arrayWithCapacity。

    id array = [NSMutableArray arrayWithCapacity:1];
    // 上述代码等效于：
    id array = [[[NSMutableArray alloc] initWithCapacity:1] autorelease];


**GNUstepd中的实现**

    in NSObject.m:
    - (id) autorelease
    {
       [NSAutoreleasePool addObject:self];
    }
    
    in NSAutoreleasePool.m:
    // class method
    + (void) addObject: (id)anObj
    {
      NSAutoreleasePool *pool = getting active NSAutoreleasePool;
      if (pool != nil) {
        [pool addObject:anObj]; //调用了instance method
      } else {
        NSLog(@"autorelease is called without active NSAutoreleasePool.");
      }
    }

    - (id) addObject: (id)anObj
    {
       [array addObject:anObj];
    }

当[pool drain]调用时：
     
    - (void) drain
    {
       [self dealloc];
    }

    - (void) dealloc
    {
       [self emptyPool];
       [array release];
    }

    - (void) emptyPool
    {
        for (id obj in array) {
            [obj release];
        }
    }


**Apple中的实现**
autorelease相关的源码在此文件中 http://opensource.apple.com/source/objc4/objc4-493.9/runtime/objc-arr.mm
简单示例见下图：
![](http://farm8.staticflickr.com/7457/8879739154_511ed73f14.jpg)



[**Pro Multithreading and Memory Management for iOS and OS X**:with ARC, Grand Central Dispatch, and Blocks](http://book.douban.com/subject/10536953/)

![](http://img5.douban.com/lpic/s9086939.jpg)



























