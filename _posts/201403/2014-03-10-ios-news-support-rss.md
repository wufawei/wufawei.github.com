---
layout: post   
title: 好文推荐（http://news.ios-wiki.com）支持RSS订阅      
categories: 
- iOS   
tags:     
- iOS
---    
 
[好文推荐（http://news.ios-wiki.com）][1]上线以来，用户慢慢增多，不少用户希望有RSS的功能。

周末抽时间写了些代码，让[好文推荐][1]支持RSS。


### PyRss2Gen

一开始的方案是采用 [PyRSS2Gen][2]来生成RSS文件， PyRSS2Gen的代码位于[此][3]。
比如可以在有用户提交新帖子的时候生成一次，保存到server本地，但是有时候帖子可能会被删除，这个时候就需要在删除帖子的时候再动态生成一次。



### 自定义Feed内容

考虑到 [好文推荐][1] 这个网站的RSS Feed只用显示标题、提交者等简单信息，因此，最新版本没有用
PyRSS2Gen。查看[http://wufawei.com/feed/][4]的源代码，可以知道每条feed的格式如下：

    <item>
      <title>iOS技术分享－第6期</title>
      <link href="http://wufawei.com/2014/03/ios-wiki-issue-6/"/>
      <pubDate>2014-03-08T00:00:00-08:00</pubDate>
      <author>Ted</author>
      <guid>http://wufawei.com/2014/03/ios-wiki-issue-6</guid>
     </itme>
     
 
 用上面的格式，查询DB，生成RSS Feed。
 
 
 访问[好文推荐][1], 点击[订阅][5]可找到RSS Feed。
 
 
  欢迎使用Feedly等工具订阅[好文推荐][1]的[RSS Feed][5]。

<br>

###关于我
大家好，我是[@吴发伟Ted](http://weibo.com/wufawei)，我会在博客上分享自己学习的一些东西。

###好文推荐  && iOS Wiki

[好文推荐(news.ios-wiki.com)][1]和[iOS Wiki(www.ios-wiki.com)][5]是我利用业余时间建立的网站。

###微信公众账号
微信公众账号：**iOS技术分享**  
![](http://farm3.staticflickr.com/2826/10855679484_56b7429bd6_m.jpg)

[1]:http://www.ios-wiki.com

 
[1]:http://news.ios-wiki.com
[2]:https://pypi.python.org/pypi/PyRSS2Gen/
[3]:http://svn.python.org/projects/peps/trunk/PyRSS2Gen.py
[4]:http://wufawei.com/feed/
[5]:http://news.ios-wiki.com/feed
