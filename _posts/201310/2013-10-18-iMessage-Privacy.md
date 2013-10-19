---
layout: post  
title: iMessage Privacy
categories:  
- iOS  
tags:    
- security
---   
 
**iMessage安全吗？**

Apple说它非常安全，从发送方到接收方采用了端到端的加密方案。那么iMessage真的安全吗，苹果能不能查看你的聊天内容呢？

苹果是这样说的：
“ There are certain categories of information which we do not provide to law enforcement or any other group because we choose not to retain it. For example, conversations which take place over iMessage and FaceTime are protected by end-to-end encryption so **no one but the sender and receiver** can see or read them.**Apple cannot decrypt that data**.”


事实并不是这么简单。
所有与苹果服务器的通信都是通过SSL的，然后，通过**MITM**可以拦截数据，这说明没有进行SSL Pinning（[通过SSL Pining提供iOS SSL通信的安全](http://wufawei.com/2013/05/SSL-Pining/)），更让人惊讶的是，竟然发送明文密码。

下面是POST包：   

     POST /WebObjects/VCProfileService.woa/wa/authenticateUser
    'content-length': 223,
    'accept-language': en-us,
    'accept-encoding': gzip,
    'content-encoding': gzip,
    'host': service.ess.apple.com,
    'accept': */*,
    'user-agent': com.apple.invitation-registration [Mac OS X,10.8.3,12D78,Macmini4,1],
    'connection': keep-alive,
    'x-protocol-version': 7,
    'content-type': application/x-apple-plist,
    'x-ds-client-id': t:3A5DC02C47249FC50EF0FF1B8CF3073C9EBD0668

接收到的数据：

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
    <key>password</key>
    <string>Icandoaproperkeynote</string>
    <key>username</key>
    <string>tim_c@icloud.com</string>
    </dict>
    </plist>

更详细的分析，参见：[iMessage Privacy](http://blog.quarkslab.com/imessage-privacy.html)