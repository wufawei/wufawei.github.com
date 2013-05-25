---
layout: post
title: iOS应用性能调优的4个建议和技巧
categories:
- Programming
tags:
- iOS
---
    任何一个能在用户手机屏幕中占有一席之地的iOS app都包含3个关键因素：想法好、设计出色、性能卓越。本文将分享一些iOS应用性能调优的4个建议和技巧。

**Tip #1:把图片资源压缩到最小。**  
    iOS app通常都会使用图片资源，程序运行时会把这些图片从磁盘加载到内存，如果图片很大，加载速度就会很慢。为了提高性能，需要尽量压缩图片资源。苹果公司的UIKit 开发者Andy Matuschak指出：使用resizable image是最快的方法。

**Tip#2：延迟主线程操作**  
   把不需要在主线程执行的任务放到 NSOperationQueue或者Grand Central Dispatch，这个自不用说。  
   对于需要在主线程执行的任务，需要注意不要干扰用户的操作，可以把这些任务分为以下2组：
  1）View updates：view的任何改变都需要在主线程中。在iOS中延后这些操作很容易，有个规则叫做：不要调用我，我来调用你。（do not call us, we’ll cal you）。绝对不要主动调用drawRect:，调用setNeedsDisplay，然后iOS会在view停止滚动的时候重新渲染view。 
     
  2）Processing: 有些很关键的任务不能在后台线程执行，例如保存Core Data database, 改变内存中的状态等等。可以把这些任务放到独立的代码块，并且在Default Runloop mode中执行，例如：

    [self performSelectorOnMainThread:@selector(processDataOnMainThread:)  withObject:dictionaryOfParameters
                        waitUntillDone:NO
                       modes:[NSArray arrayWithObject:NSDefaultRunLoopMode]]

     当用户开始滚动scrollview或者tableview的时候，run loop mode会被设置成 Common modes。当用户停止滚动时，它会被重置为Default Mode。因此，如果你直接使用[self processDataOnMainThread:dictionaryOfParams] ，那么函数将立即执行，而不管用户是否还在滚动view。但是如果用上面给的方法，那么iOS将在用户停止滚动view之后执行你的代码。   
       
**Tip #3: 保持内存使用平滑**  
   下面是几个内存使用的尖峰时刻：  
a) app 启动时：加载尽量少的对象，会加快启动速度。  
b)   View Controler初始化时：使用尽量少得view，或者延迟实例化。  
c) UIWebview：UIWebview非常耗内存。使用时需要注意。

**Tip #4：避免不必要的图片缓存**   
    通常我们会用imageNamed:来加载图片，但是用这个API有个问题，就是它会缓存加载的image。因此，对于那些被重用的图片，这个API很高效。但是对于那些使用很少的图片，用这个就很耗内存，那怎么才能及时的释放掉内存呢。使用initWithContentsOfFile：即可。
 NSString *path = [[NSBundle mainBundle] pathForResource:fileName ofType:fileType];
 UIImage *image = [[UIImage alloc] initWithContentsOfFile:path];
 …
 [image release];  
     举个简单的例子，程序第一次使用时，会加载很多新手引导图片， 如果用initWithContentsOfFile:，这些图片所占用内存可以很快被释放，试试就知道。

参见：  
1）https://news.ycombinator.com/item?id=4645585  
2）http://robots.thoughtbot.com/post/33427366406/designing-for-ios-taming-uibutton  
3）http://eng.pulse.me/tips-for-improving-performance-of-your-ios-application/
   

另外还有篇文章一起分享给大家：iOS应用性能调优的25个建议和技巧
英文原文：http://www.raywenderlich.com/31166/25-ios-app-performance-tips-tricks