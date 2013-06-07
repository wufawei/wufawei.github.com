---
layout: post  
title: Block retain cycle
categories:  
- programming
tags:
- iOS
---

今天这里讨论下使用block导致的retain  cycle的问题，简化为一句话就是 object A拥有 block，而block又retain了 object A。

1）在没有使用ARC之前，
  如果在block里面引用了对象，只需要在前面加一个__\_\_block__就可以了。

2）使用ARC之后，光在对象前面加个\_\_block已经不够了，因为对象的默认修饰符是\_\_strong, 在block内部使用时其引用计数还是会＋1。

怎么办呢？使用 __\_\_weak__就可以了。如果在iOS 5.0之前，那就使用__\_\_unsafe\_unretained。__


下面的objective-c-blocks-quiz 可以测试下你对block用法


<http://blog.parse.com/2013/02/05/objective-c-blocks-quiz/>


使用block和operation而导致的retain cycle的详细分析例子可以通过点击下面链接查看。

**Blocks, Operations, and Retain Cycles**

<http://conradstoll.com/blog/2013/1/19/blocks-operations-and-retain-cycles.html>

