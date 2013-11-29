---
layout: post 
title: Go语言内存模型   
categories: 
- Go 
tags:   
- Go
---  





“Go 内存模型是为了定义清楚变量的读写在不同执行体的可见性。理解内存模型在并发编程中非常重要，因为代码的执行顺序和书写的逻辑顺序并不会完全一致，甚至在编译期间编译器也有可能重排代码以最优化CPU执行, 另外还因为有CPU缓存的存在，内存的数据不一定会及时更新，这样对内存中的同一个变量读和写也不一定和期望一样。” 。
引用自[Go语言内存模型][1]。


读完之后，建议再读读官方文档[The Go Memory Model][2]。


[1]:http://hugozhu.myalert.info/2013/04/20/31-golang-memory-model.html
[2]:http://golang.org/ref/mem 








