---
layout: post   
title: 通过UserAgent识别移动设备		
categories:	 
- python   
tags:
- python
---	

 
[iOS Wiki(www.ios-wiki.com)][1]上的iOS技术分享需要在移动设备上做适配。
因此需要识别访问的设备是PC还是mobile。

通过判断UserAgent中的字符串就可以达到目的。如下面的代码所示：


	def is_mobile_user(request):
	    ua = request.user_agent
	    if ('iPhone' in ua) or ('iPod' in ua) 
	       or ('iPad' in ua) or ('Android' in ua):
	       return True
	    return False
        

点击[iOS技术分享第3期][2]可以查看mobile设备上访问的样式。


[1]:http://www.ios-wiki.com
[2]:http://www.ios-wiki.com/mobile/issues/3