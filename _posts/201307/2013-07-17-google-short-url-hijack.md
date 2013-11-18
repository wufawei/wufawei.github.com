---
layout: post  
title: Google短连接劫持分析
categories:  
- security  
tags:    
- google short url    
---   


### 一 Google搜索结果被劫持 ##
<br>
 一朋友在Google上搜索关键词“易迅网”，出现下面结果。

 图： 

![](http://farm4.staticflickr.com/3740/9306568559_38b33553d9.jpg)


点击第一个搜索结果，结果页面变成了这样：

![](http://farm8.staticflickr.com/7429/9306568847_4df0a06f10.jpg)
                      
<br/>

很奇怪，为什么先打开一个新页面（http://51buy.com/）后，google的搜索结果页会跳转到这个(http://www.ipaofu.com/book/shopping)地方呢。


<br>
### 二 分析

发生跳转的搜索结构的链接为：    
    
http://www.google.com.hk/aclk?sa=L&ai=CbQJnm5rmUd-fG6GZmQWmv4CwBP6Bj7UF5qqo3miTg8vojAEIABABUL3Sh1pgnenVgbgFyAEBqQI9jf6EPuCFPqoEIk_QwGD1zeq4ZfRROe5R2bhlFF-_aa3XIohG3_JbQbiU8_WAB8bIrDE&sig=AOD64_16YOyJJmNqcQ52_uzraIktGo4i-g&ved=0CC4Q0Qw&adurl=**http://goo.gl/T6Ysw**

**http://goo.gl/T6Ysw**是Google短链接（[Google url shortener](http://goo.gl/)）服务生成的。

**1）执行命令:curl http://goo.gl/T6Ysw**

    <HTML>
    <HEAD>
    <TITLE>Moved Permanently</TITLE>
    </HEAD>
    <BODY BGCOLOR="#FFFFFF" TEXT="#000000">
    <H1>Moved Permanently</H1>
    The document has moved <A HREF="http://youhuiqq.com/ssfm/icson/ca.icson.com/">he
    re</A>.
    </BODY>
    </HTML>

**2）curl http://youhuiqq.com/ssfm/icson/ca.icson.com/**

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
    <meta http-equiv="Refresh" content="0;URL=http://www.51buy.com" />
    <title>易讯网上商城 51buy  进入中</title>
    </head>
    <body>
    <SCRIPT> 
         if(parent.window.opener) 
             parent.window.opener.location='http://www.ipaofu.com/book/shopping'; 
    </SCRIPT>
    </body>
    </html>
    
  **这里的代码完成的功能，一目了然，除了会打开http://www.51buy.com之外，还会修改parent 窗口为:http://www.ipaofu.com/book/shopping,原来是在这里实现的跳转**。

  **如果是正常搜索结果，打开http://www.51buy.com就结束了，是不会去修改父窗口打开另一个网站的。**

**3）whois**    

  http://whois.chinaz.com/
     
 **4) 进一步分析**  
   我碰到过一次，在google上搜索京东，除了打开京东之外，还会打开另一个网站。
   打开**http://youhuiqq.com/test/test/**，这个里面还有很多东西，有兴趣的可以去看看里面会执行的跳转。
   
 ![](http://farm4.staticflickr.com/3802/9309047242_5d1ea816ab.jpg)

![](http://farm3.staticflickr.com/2860/9306263271_4aec284037.jpg)
<br>


### 三 总结

这个搜索结果会出现这个问题，从问题发现到现在一周多过去了，**发现后就跟google反馈过这个问题**，**不过他们还是没有响应**，今天就先发出来。

这个url是怎么换掉的，怎么被劫持的、劫持后有什么后果呢？

发出来大家想一想吧。


<br />

（转载本站文章请注明作者和出处，请勿用于任何商业用途）
<br />
<br />  


---------这是分割线----------

1) 如果你是因为别人的分享来到这里的，可以到微信，点击查看公共账户，添加 iosinfo(iOS技术分享)为好友，会分享iOS开发相关技巧，有时也会发一些我觉得有趣的东西。  

2) 关于我。
新浪微博: [@吴发伟Ted](http://weibo.com/wufawei)

微信公众账号：**iOS技术分享**

![](http://farm3.staticflickr.com/2826/10855679484_56b7429bd6.jpg)