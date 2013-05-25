---
layout: post
title: 通过SSL Pining提供iOS SSL通信的安全
categories:
- Programming
tags:
- iOS
---
通过SSL Pining提供iOS SSL通信的安全
     使用SSL来进行网络通信成为了很多mobile app的默认选择。最近一些文章发现：一些app并没有采用“额外的措施”来保证窃听不可以发生：这个“额外的步骤“就是SSL Pinning。
    iOS上SSL连接默认是这样的，client 和server建立一个连接，server返回其 SSL证书。如果这个证书是被OS信任的证书机构签发，那么这个连接就正常进行下去。随后交换session key进行通信。这个关键点就在于“信任“。如果攻击者进行中间人攻击，mobile device很可能也会信任攻击者的证书。一般来说，攻击者有受信任的证书不太，所以通常情况下这个问题也不大。
      使用SSL Pinning可以消除这个可能。也可能用户本身故意作为攻击者来探查加密的网络流量。可能使用 Charles 或者mitmproxy来安装一个受信任的证书。你的app需要有能力通过SSL Pinning来阻止这种嗅探攻击。
SSL Pinning就是client能够把server的证书和一个已知的server的证书的拷贝做对比，通过把server的SSL 证书捆绑到程序中就可以达到目的。
在iOS中可以使用NSURLConnectionDelegate 协议中的connection:willSendRequestForAuthenticationChallenge: 来检查


SSL Pinning for Increased App Security
http://www.doubleencore.com/2013/03/ssl-pinning-for-increased-app-security/?utm_source=iOS+Dev+Weekly&utm_campaign=a11b470e23-iOS_Dev_Weekly_Issue_88&utm_medium=email