---
layout: post
categories: HTML
title: "为Hbuilder设置PHP运行环境"
tags: [Xampp,HBuilder]
---
前言
--
以我的能力艰难完成大作业之后，总算得空可以继续学习前端知识了，**主动学习有一种让人沉迷的味道啊**。  

至少半年前曾经尝试过给Hbuilder配置PHP的运行环境，可惜那时是被动学习，终究以失败告终，导致我对AJAX和JSON的运用几乎是0的，没有去解决问题。今天可以弥补一下以前的遗憾了。  

因此我写下如何为Hbuilder设置PHP运行环境，如果能够帮助到其他人，那真是一件让人开心的事情。  
<!-- more -->
开始之前再唠叨一些
---------
我的专业偏计算机，在电脑上安装过很多编程环境，因此可能会出现不少不清楚的问题。这里我提一个事情，可能和我以前配置失败有关联。***如果你也设置失败***，或许也是IIS服务器没有关闭！具体解决办法可以参照这篇文档：  

[win8系统确认关闭IIS服务器](http://www.win8.net/jiaocheng/20170318/7178.html)， 该文档中的Internet信息服务，在你的电脑上可能是英文形式的！

开始配置PHP运行环境
-----------
### 第一步：安装Xampp集成环境 ###
**下载地址：http://www.xampps.com/**  

**如何安装、安装之后出现的问题以及解决方案**：http://boke112.com/3878.html ，内有VC9运行库，最好提前安装(**x64和x86都要安装**)  

安装顺序， VC9 运行库 -> Xampp集成环境 ->以管理员身份运行此程序。  

如果需要修改Apache的端口号，则可以参照这篇文档：[PHP XAMPP配置PHP环境和Apache80端口被占用解决方案](http://blog.csdn.net/eastmount/article/details/11823549)，不过**修改之后进入admin页面也需要在localhost:+端口号**  

ok，现在配置成功，打开localhost:8081(我的端口号修改为8081)，显示如下：
![xampp正常](http://oltqt8zyb.bkt.clouddn.com/xwapp-min.png)

### 第二步：HBuilder Web服务器配置 ###

具体的可以参照这篇文档：http://www.cnblogs.com/laonniudajiangtang/p/5973267.html 

### 注意点： ###
1. 涉及到php项目存放目录：xampp软件目录的htdocs目录下，以我为例就是在（D:\xampp\htdocs）
2. 测试：在HBuilder新建php文件，代码如下
```
<?php
print "请求成功!";
?>
```
在浏览器中显示如图：![phpSuccessful](http://oltqt8zyb.bkt.clouddn.com/phpSuccessful-min.png)  

大功告成！！  

可以愉快的写东西了！！！