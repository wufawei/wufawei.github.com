---
layout: post
title: 使用iOS的NSRunLoop
categories:
- Programming
tags:
- iOS
---
使用iOS的NSRunLoop

1）基础知识
Run loops是线程相关的的基础框架的一部分。一个run loop就是一个事件处理的循环，用来不停的调度工作以及处理输入事件。使用run loop的目的是让你的线程在有工作的时候忙于工作，而没工作的时候处于休眠状态。
在Cocoa中，每个线程(NSThread)对象中内部都有一个run loop（NSRunLoop）对象用来循环处理输入事件。应用程序不需要显式的创建这些对象(run loop objects)；每个线程，包括程序的主线程都有与之对应的run loop object。只有辅助线程才需要显式的运行它的run loop。处理的事件包括两类，一是来自Input sources的异步事件，一是来自Timer sources的同步事件
     每个run loop可运行在不同的模式下,一个run loop mode是一个集合，其中包含其监听的若干输入事件源，定时器等。运行在一种mode下的run loop只会处理其run loop mode中包含的输入源事件，定时器事件等。
  Cocoa中的预定义模式有:
1）	Default模式。几乎包含了所有的输入源，一般情况下用此模式。
2）	Event tracking模式。user interface tracking loops时处于此种模式下，在此模式下会限制输入事件的处理。例如，当手指按住UITableView、UIScrollView拖动时就会处于此模式。
3）	Common模式
为一组run loop mode的集合。在Cocoa应用程序中，默认情况下Common Modes包含default modes,modal modes,event Tracking modes。
4）   …
2）如何解决NSURLConnection在用户滚动UIScrollView或者UITableView不执行的问题。
       当使用NSURLConnection来加载文件的时候，如下面的代码：
NSURLConnection *connection = [[NSURLConnection alloc] 
     initWithRequest:request delegate:self startImmediately:NO];
[connection start];
    NSURLConnection默认运行在default mode下，这样当用户在拖动UITableView处于UITrackingRunLoopMode模式时， NSURLConnection的数据也无法处理。
      通过下面的方法可以解决这个问题：
     [connection scheduleInRunLoop:[NSRunLoop currentRunLoop] 
     forMode:NSRunLoopCommonModes];

3）如何在后台线程运行NSURLConnection
    答案就是通过CFRunLoopRun()使得后台线程一直执行，使用CFRunLoopStop(CFRunLoopGetCurrent())结束。
参考文献：
1）http://stackoverflow.com/questions/7222449/nsdefaultrunloopmode-vs-nsrunloopcommonmodes
2）http://www.pixeldock.com/blog/how-to-avoid-blocked-downloads-during-scrolling/
3）http://stackoverflow.com/questions/1728631/asynchronous-request-to-the-server-from-background-thread
