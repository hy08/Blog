---
layout: post
categories: HTML
title: "那些被遗忘的HTML知识"
tags: [HTML,CSS,前端]
---
开始赶工了，在去找实习单位之前，我规划的七件事情能完成几件呢？正则表达式已经结束、两份设计稿还原(pc端还原可以说是失败的，还有一份手机APP的设计稿)，那么来战吧。  

在这期间，几乎被我遗忘的前端知识能够在回忆起多少呢？什么时候开始我也习惯码代码了，甚至有点享受。在这期间，我会学到多少还没掌握的知识呢？  

那么，从现在开始吧，我的工具，HBuilder，**是时候展示你的奥义了**......

<!-- more -->

-----
问题篇：
----
**问题一**：如何将一个图标放入`<input>`标签中？  

解决方案 -> 绝对定位，把图标塞进去  

---------

**问题二**：目的是将图片设置为中心居中，**并且显示出来**(因为此时背景色遮盖图标)。


解决方案 -> 思路来源-> [垂直居中](https://www.qianduan.net/css-to-achieve-the-vertical-center-of-the-five-kinds-of-methods/)
HTML:
```
<div class="camera">
      <img src="../images/icon-Camera.png" />
    </div>
```
解决->CSS:
```
     .camera{
       border-radius: 50%;
       background-color: rgb( 255, 255, 255, 0.102); /*解释请见知新2，其实呐，这还是温故啊。真实丢脸*/
       width: 6em;
       height: 6em;
       text-align: center;
      /*display:table-cell;
       vertical-align:middle; 使用table方法，与包含块的水平居中margin:0 auto;冲突*/
       position: relative;
       margin: 0 auto;
     }
     .camera img{
       width: 3em;
       height: 3em;
       position: absolute;
       top: 0;
       bottom: 0;
       left: 0;
       right: 0;
       margin: auto;
     }
```

**问题三**：还是垂直居中，这次是单行文本垂直居中，目的是让A标签文本垂直居中，可是设置**line-height=height**并没有效果！问题出自哪里呢？应该是我一百分比(**不定高**)布局的原因。

HTML：
```
<div class="bottom">
    <a id="skip">Skip</a>
</div>
```

CSS:
```
.bottom{
  background-color: red;
  width: 100%;
  height: 40%;
  margin: 0 auto;
}
#skip{
  display: block;
  width: 20%;
  height: 20%;
  line-height: 20%;
  background-color: yellow;
}
```
ps:神奇的百分比，该死的单行文本居中定位。！！！！！！！  

解决方案 -> 思路来源->[百分比div垂直居中](http://www.haorooms.com/post/css_div_juzhong)  

CSS(不兼容IE浏览器):
```
.bottom{
  background-color: red;
  width: 100%;
  height: 40%;
  margin: 0 auto;
}
#skip{
  display:-webkit-flex;
  justify-content:center; /*子元素水平居中*/
  align-items:center; /*子元素垂直居中*/
  width: 20%;
  height: 20%;
  background-color: yellow;
}
```
---

温故篇：
----
1. 单引号或者英文撇号如***Don't***的 ASCII 代码是`&#39;`
2. 图片.png24和.png32之间的区别在于后者支持透明，且ps智能导出的图片就是32位的。
3. 为图片`<img>`(行元素)设置宽高，注意不是图片的父元素（那个块）
4. 代码左尖括号“<”和右尖括号“>”的 ASCII 代码是`&#60; &#62;`
5. 有的时候需要设置body高度为屏幕的高度，可以设置`html,body{height: 100%;}`
6. css加粗属性，`font-weight:bold;`
7. **以后第一步就是清除浏览器样式！！！！！！**
8. 设置水平居中：对于行元素（`<span>,<img />`），对其父元素设置`text-align: center;`
;对于定宽的块状元素设置`margin: npx,0;`。不定宽的块状元素遇到再写。
9. CSS单位：相对单位与绝对单位。参照来源 -> [CSS尺寸和字体单位-em、px还是%](http://greengerong.com/blog/2015/10/09/css-chi-cun-dan-wei-em-px-huan-shi-percent/)



知新篇：  
------

1、**background-size(CSS3)** 属性规定背景图像的尺寸，其语法：
`background-size: length|percentage|cover|contain;`这其中的属性值介绍：[background-size](http://www.runoob.com/cssref/css3-pr-background-size.html)，**至于关键字“cover &#124; contain” 有待学习**  

2、**设置opacity元素的所有后代元素会随着一起具有透明性**，一般用于调整图片或者模块的**整体不透明度**；***如果想要设置背景透明，文字/图片不透明***，那么要使用**rbga**。参照来源 -> [CSS实现背景透明，文字不透明，兼容所有浏览器](http://www.cnblogs.com/PeunZhang/p/4089894.html)  

**3、css百分比定义高度的一个问题。**  

HTML:
```
<body>
  <div class="one">
    松岛枫松岛枫松岛枫松岛枫松岛枫松岛枫松岛枫
  </div>
</body>
```
解决->CSS:
```
<style>
  html,body{
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;  /*只有同时设置html和body的高度为100%，才有效。不过可能会导致html或者body的高度仅仅为浏览器可视区域高度，而非页面可视区域高度*/
  }
  .one{
    width:100%;
    height: 50%;
  }
</style>
```
参照来源 -> [CSS百分比定义高度的冷知识](http://www.cnblogs.com/vajoy/p/3730014.html)



总结：
---
还原移动端设计稿的进度才3/13，却遇到了太多的问题(**又是失败的作品**)。以前学的东西几乎已忘干净。现在我得**有规划的学习移动端应该怎么布局**才是最有的，可以适配各种屏幕。  

### 学习方案 ###
1. [可伸缩布局方案](https://github.com/amfe/lib-flexible)
2. [知乎_大漠：使用Flexible实现手淘H5页面的终端适配](http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html)
3. [知乎_熊汉彪：移动端 Web 开发踩坑之旅](https://zhuanlan.zhihu.com/p/26141351)