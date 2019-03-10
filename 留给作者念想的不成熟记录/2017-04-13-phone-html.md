layout: post
categories: HTML
title: "移动端的那些坑(学习篇)"
tags: [前端,移动端,HTML]
---
![移动端的那些坑](http://oltqt8zyb.bkt.clouddn.com/keng.png)  

了解移动端的坑，在以后的开发中可以更加得心应手。  

以前觉得移动端的布局应该比在pc端更加容易才对，可是现实往往会**温柔的抚摸你的脸颊**。  

啪！ 醒醒，吃药啦！！！
<!-- more -->
前言：
---
正如我在上一篇：[那些被遗忘的HTML知识](/2017/03/24/psd-recover/)中提过，我先前学的前端知识由于不复习（主要是学了之后几乎处于无所事事阶段）忘得差不离了。不论是PC端或是移动端我都学过相应的技术，但是终究还是不足的，这是我的个人原因。  

就移动端来说，解决的方法与相应的框架有不少。以前学过**BootStrap框架**，除了学习的时候有些了解，现在也只能停留在**有疑问去**[Bootstrap中文文档](http://v3.bootcss.com/getting-started/#grunt)的水准了。  

现在了解下淘宝的[使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)，好的方面是有相应的配套练习（虽说是我自己找的，但也不错了。）  

一些知识点：
------
### 详情可见： ###
1. [移动端网站开发要点](http://www.jianshu.com/p/739d7ce9c6fe)  
2. [使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)

### 下面是我自己的小抄(大家可以直接看上面两个连接)： ###

#### 1.视窗viewport   ####
该属性常用于设计移动端网页，相当于移动端的浏览器窗口。**是meta标签中name属性的一个值，这里列出几个移动端常见的值：format-detection、apple-mobile-web-app-capable、apple-mobile-web-app-status-bar-style、apple-touch-fullscreen、apple-mobile-web-app-capable、apple-touch-startup-image，详情见[移动端网站开发要点](http://www.jianshu.com/p/739d7ce9c6fe) **，其属性有：  

1. width:viewport的宽度（范围从200到10,000，默认为980像素）
2. height:viewport的高度（范围从223到10,000）
3. initial-scale:初始的缩放比例（范围从>0到10）
4. minimum-scale:允许用户缩放到的最小比例
5. maximum-scale:允许用户缩放到的最大比例
6. user-scalable:用户是否可以手动缩放

在浏览器中页面将以原始大小显示，并不允许缩放  
```
<metaname="viewport"content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
```

#### 2.几个不同的像素知识   ####
**物理像素(physical pixel)**  

物理像素又被称为设备像素，他是显示设备中一个最微小的物理部件。每个像素可以根据操作系统设置自己的颜色和亮度。正是这些设备像素的微小距离欺骗了我们肉眼看到的图像效果。  

----------

**设备独立像素(density-independent pixel) ** 

设备独立像素也称为密度无关像素，可以认为是计算机坐标系统中的一个点，这个点代表一个可以由程序使用的虚拟像素(比如说CSS像素)，然后由相关系统转换为物理像素。  

----------
**CSS像素**  

CSS像素是一个抽像的单位，主要使用在浏览器上，用来精确度量Web页面上的内容。一般情况之下，CSS像素称为与设备无关的像素(device-independent pixel)，简称DIPs。  

----------
**设备像素比(device pixel ratio)**  

设备像素比简称为dpr，其定义了物理像素和设备独立像素的对应关系。它的值可以按下面的公式计算得到：  

设备像素比 ＝ 物理像素 / 设备独立像素  

在JavaScript中，可以通过window.devicePixelRatio获取到当前设备的dpr。而在CSS中，可以通过-webkit-device-pixel-ratio，-webkit-min-device-pixel-ratio和 -webkit-max-device-pixel-ratio进行媒体查询，对不同dpr的设备，做一些样式适配(这里只针对webkit内核的浏览器和webview)。  

dip或dp,（device independent pixels，设备独立像素）与屏幕密度有关。dip可以用来辅助区分视网膜（Retina）设备还是非视网膜设备。  

----------

## 开始练习 ##
按照**[使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)的案例实战**一步一步来。**[移动端重构系列](https://www.w3cplus.com/mobile/mobile-terminal-refactoring-preparatory-work.html)**也很值得一看。  

### 创建HTML模板 ###
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">  
        <meta content="yes" name="apple-mobile-web-app-capable">
        <meta content="yes" name="apple-touch-fullscreen">
        <meta content="telephone=no,email=no" name="format-detection">
        <script src="http://g.tbcdn.cn/mtb/lib-flexible/0.3.4/??flexible_css.js,flexible.js"></script>
        <link rel="apple-touch-icon" href="favicon.png"><!--在用户把网页存为书签时，在手机HOME界面创建应用程序样式的图标，换句话就是：这个link就是设置webapp的放置主屏幕上icon文件路径-->
        <link rel="Shortcut Icon" href="favicon.png" type="image/x-icon"> <!--想了解这行代码的意思，可见“维基百科”(https://zh.wikipedia.org/wiki/Favicon)的“指导”部分.需翻墙！-->
        <title>HOME</title>
    </head>
    <body>
        <!-- 页面结构写在这里 -->
    </body>
</html>
```
