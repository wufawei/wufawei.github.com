---
layout: post   
title: iOS技术分享－第3期        
categories: 
- iOS   
tags:     
- iOS
---    
 
因为最近比较忙，昨天在公司加班到晚上才回家。
所以到今天晚上才在[iOS Wiki(www.ios-wiki.com)][1]上放出iOS技术分享第3期的内容。


####iOS技术分享

今天也抽了些时间对iOS Wiki的后台做了些优化，之前为了方便，每一期直接用的html文件，内容直接编辑进去。今天改成了用模版，数据存入数据库，每次用户请求来的的时候从数据库query出数据，然后填到模版中显示。

####数据录入
今天也写了用于编辑新一期内容的接口，以后直接在代码中写入内容，调用后台API就可以把数据插入数据库，这个插入代码是用golang写的，用python写也很简单，调用下后台RESTful API就可以了。



请大家到iOS Wiki查看新一期的内容。


<br/>

[1]:http://www.ios-wiki.com