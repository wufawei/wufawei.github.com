---
layout: post   
title: 使用NSTimer需要注意的事        
categories: 
- iOS   
tags:     
- iOS
---    
 


关于NSTimer, 有一点需要特别注意：NSTimer会持有target（Remember that NSTimer Retains Its Target)。


* 一个NSTimer对象在触发时会保留目标直到计时器被显式的设置无效。（NSTimer invalidate）

* 如果调用者保存了NSTimer,在dealloc中没有调用invalidate timer的话，会有内存泄漏。

* 使用块方式可以使NSTimer对象实现打破这种循环引用的扩展。为了使这个块实现作为NSTimer的公共接口一部分，可以把这个块实现方法加到NSTimer的类扩展中。   
（[代码][1]）



所以，如果你不想实现block的方式，在dealloc中记得invalidate timer就可以了。
也可以看看这篇文章[Defeating NSTimer Retain Cycle][2]


<br/>


[1]:https://github.com/effectiveobjc/code/blob/master/chapter_7/item_52.txt
[2]:http://alexanderwong.me/post/33729599887/defeating-nstimer-retain-cycle
