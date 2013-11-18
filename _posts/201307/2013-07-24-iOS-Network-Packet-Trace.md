---
layout: post  
title: iOS Network Packet Trace   
categories:  
- iOS  
tags:    
- tcpdump  
---   
 
<br/>
### 1 远程虚拟接口(remote virtual interface ,RVI) ###
<br/>
在iOS5中新引入了“远程虚拟接口(remote virtual interface ,RVI)“的特性,可以在Mac中建立一个虚拟网络接口来作为iOS设备的网络栈，这样所有经过iOS设备的流量都会经过此虚拟接口，此虚拟接口代替了iOS设备本身的协议栈,但并没有将网络流量中转到Mac本身的网络连接上，这样所有网络连接都是iOS设备本身的，Mac电脑本身连不联网或者连接的网络类型都没有关系，而iOS设备本身可以为任意网络类型,2G/3G/WiFi等。

在Mac电脑上使用任意抓包工具抓取RVI接口上的数据包就可以看到iOS设备上的所有网络数据。


<br/>
### 2 具体步骤： ###
<br/>
a.通过USB口将iPhone连接到Mac上。 

b.使用Xcode的organizer工具获取到iPhone的UDID

c.创建RVI接口
   $ rvictl -s 

d.在Mac上使用任意抓包工具tcpdump、wireshark等，监听创建的rvi接口即可。

e.使用完之后需要将创建的虚拟接口移除 $ rvictl -x

<br/>
### 3检查是否含有特定字段的过程： ###
<br/>
a)   通过USB连接iPad/iPhone.   
b)   取得UDID   
c) 创建RVI接口。    rvictl -s   
d)  sudo tcpdump -n -t -i rvi0 -q -A -v tcp | grep "somestring“





参考文献：

1 [Technical Q&A QA1176 Getting a Packet Trace](http://developer.apple.com/library/mac/#qa/qa1176/_index.html) 

2 [Analysing iOS App Network Performances on Cellular/Wi-Fi]( http://blog.manbolo.com/2013/02/22/analysing-ios-app-network-performances-on-cellularwifi)

3 [TCPDUMP](http://www.tcpdump.org/tcpdump_man.html).

4 [未越狱ios设备的抓包方法](http://fanliugen.com/?p=351) by 樊留根.

<br/>
<br/>

（转载本站文章请注明作者和出处 吴发伟Ted – wufawei.com ，请勿用于任何商业用途）

---------这是分割线----------


<br>
微信公众账号：**iOS技术分享**

![](http://farm3.staticflickr.com/2826/10855679484_56b7429bd6.jpg)