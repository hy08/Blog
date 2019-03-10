---
layout: post
categories: 随笔
title: "博客的搭建"
tags: [博客,hexo,next主题]
---
**开阖之间**，就是我的博客名啦，意思也很简单。我把这里当成我的私人领地，就像一个我的屋子一样，可以随心修饰。  

接下来，我便介绍一下使用hexo框架，采用next主题+github Page搭建的全过程，以及一些注意事项......
<!-- more -->

## 框架的下载以及博客主题的选择
### 框架的下载
第一个学习的贡献者是令狐葱，他的[手把手教你使用Hexo + Github Pages搭建个人独立博客](https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/)。  

### 博客主题的选择
第二个贡献者当然就是Next主题的作者了，但我们也可以选择自己喜欢的主题，在[hexo主题](https://hexo.io/themes/)这里有丰富的博客主题可供挑选。  

这里以Next主题为例，进入[Next主题使用文档](http://theme-next.iissnan.com/)，按照主题作者的使用文档几乎可以解决所有的问题。  

但还是有一些坑是我们会遇到的。每个人的情况不同，这里我介绍一些我自己遭遇的问题和解决的办法。  

## 踩坑经历以及解决之道
### 1.乱码的问题
在我按照使用文档的过程中，已经设置了中文简体的配置后，即`language: zh-Hans`。出现了意料之外的问题，网站的标题以及博主名，还有接下来的搜索框等等都变成了乱码，但是文章部分显示正常。  

经过了解，是编码出现的问题。用***记事本***打开配置文件(无论是站点配置文件还是主题配置文件)，这是我一开始的方法，之后用***Hbuilder***(一个编辑器)打开之后，发现在这里汉字都成了乱码，经过修改则恢复正常。  

### 2.头像和favicon
这里的问题是图片路径的问题。  

我们将头像的图片以及favicon图片放置在***“主题名\source\images”***中，之后在站点配置文件中设置`avatar: /images/头像名`，在主题配置文件中设置`favicon: images/favicon.ico`  

### 3.404页面
我的404页面在部署完成后才生效，不过个人认为如果我们在站点根目录之下执行以下命令也是ok的。  
```
$ hexo g # 或者hexo generate 
$ hexo s # 或者hexo server，可以在http://localhost:4000/ 查看 
```

### 4.站点加载太慢
当一切完成之后，部署也ok之后(这部分后面会讲述)。打开网站的时候发现加载速度是在感人(***59秒***)，火狐浏览器检查一下，发现问题有两个，***字体无法加载、JavaScript 第三方库加载速度太慢***。

* [字体解决办法](http://www.jianshu.com/p/95a8a7f70457)
* JavaScript 第三方库加载速度太慢：我将[第三方库](http://theme-next.iissnan.com/advanced-settings.html)涉及到的资源全部迁移到我的[七牛云](https://www.qiniu.com/)中。

当然，这些资源都在***主题名\source\lib***。之后只需要修改下路径名即可，路径名就是七牛云中的外链。  

## 博客的部署
### 我的方法
1. 请创建自己的仓库，详情请见[如何搭建一个独立博客——简明 Github Pages与 jekyll 教程
](http://www.cnfeat.com/blog/2014/05/10/how-to-build-a-blog/)

2. 在站点配置文件中修改以下配置：
```
deploy:
  type: git
  repo: git@github.com:hy08/hy08.github.io.git
  branch: master
```
3. 在站点根目录下安装一个插件
`$ npm install hexo-deployer-git --save`

4. 在站点目录下执行以下程序：
```
$ hexo clean
$ hexo g
$ hexo d
```

## 最后
希望以上的经验能够对你起到作用。  

**最后提示在每次文章更新、需要同步到github中的时候，如果你有自己的域名，可以在`$ hexo d`之前将CNAME文件放在public文件夹下面。**