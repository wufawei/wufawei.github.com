---
layout: post  
title: How to Install Jekyll on Windows  
categories:  
- 其他  
tags:
- Jekyll
---
## 
## 1 从一个问题说起 ##
"I have a jekyll blog on the github. The problem is after I updated my old post, committed and pushed it, the blog post doesn't show any changes, but I see that the commit has been pushed successfully."  
**<http://stackoverflow.com/questions/12000316/jekyll-and-github-how-to-update-the-blog-post>**

我也碰到类似的情况，怎么办呢。通过在本地搭建Jekyll测试环境，可以发现并解决问题。

## 2 如何在Windows上搭建Jekyll环境 ##

a)下载Ruby installer for windows。  
b) Ruby Development Kit。  
c) gem install Jekyll。

在步骤c时速度有点慢，请耐心等待。

详细过程参见：**How to Install Jekyll on Windows**  
<http://sstude.com/blog/2013/02/12/how-to-install-jekyll-on-windows/>

## 3 本地开启Jekyll ##

命令行下： Jekyll server ./       (github项目位于当前文件夹)

这个时候，出现了报错：“invalid byte sequence in GBK error jekyll”

怎么解决呢：
命令行下执行 chcp 65001，然后再执行Jekyll server ./即可。

参考文献：
**Jekyll 本地调试之若干问题**  
<http://chxt6896.github.io/blog/2012/02/13/blog-jekyll-native.html>