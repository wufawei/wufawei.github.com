---
layout: post  
title: 使用WebP来节省带宽  
categories:  
- iOS  
tags:
- webp
---

## 1 WebP是什么 

WebP是Google推出的一种同时提供了有损压缩与无损压缩的图片格式。目前互联网上传输的数据有65%都是图片，WebP就是出于减少数据量、加速网络传输的目的而开发的。WebP希望能够达到和JPEG同水平的图片质量，但是更小的文件，以减少图片的传送时间。

在有损压缩的状况下，WebP比同样画质的JPEG少了25% ～ 34%的文件大小 。而在无损压缩的情形中，比起用libpng产生的PNG图片，WebP少了34%的文件大小，也比用pngout再处理过的PNG图片少了26%的文件大小。

## 2 WebP的使用




webp图片压缩率会比jpeg、png更高，因此下载图片会更省流量，特别适合在mobile app上使用。
比如iOS app上，可以把webp编译生成framework，然后加入引用就可以是使用了。强烈推荐。

## 3 如何生成webp framework

a) 下载webp库   


       git init   
       git pull http://git.chromium.org/webm/libwebp.git     


b) 编译   


    `sudo ./iosbuild.sh`   
c)  安装autoconf  

    此时如果提示  **"error:"autogen.sh: line 2: exec: autoreconf: not found"**
    那就需要安装autoconf：  
    在mac os上：sudo port install automake autoconf libtool
    然后执行 步骤b)就可以生成webp.framework了。

d) 使用

这iOS解析时，可以直接使用 [WebPImage](https://github.com/darcyliu/WebP-Viewer/blob/master/WebP-Viewer/WebPImage.m)。

    NSData *imageData = [NSData dataWithContentsOfURL:url];
    UIImage *tmpImage = [[WebPImage alloc] loadWebPFromData:imageData];





    
**推荐使用**。




1) How to generate webp  
<https://code.google.com/p/webp/issues/detail?id=100>

2) <http://stackoverflow.com/questions/9575989/install-autoreconf-on-osx-lion>       

3) <https://github.com/darcyliu/WebP-Viewer/blob/master/WebP-Viewer/WebPImage.h>

4) Repository for tools for the WebP image format
<https://code.google.com/p/webp/downloads/listl>