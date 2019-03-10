---
layout: post
categories: 有意思的编程课外书
title: "变量名的力量"
tags: [代码大全]
---
  代码不好读，那可真是一件让人头疼的事情。而专业的开发人员应该意识到代码的可阅读性的重要。  

  那么从哪里开始呢？就从变量的命名开始。
<!-- more -->

## 命名的核心
  一个好的名字，应该反应的是**“What”**,而不是How。
## 命名的小细节
  你愿意看见一个长长的变量名吗，那时候你还有兴趣了解其代表什么吗？就像下面这个变量名:  
`int numberOfPeopleOnTheUsolympicTeam=100;` 很糟糕，不够优雅。那么很短的变量名又如何呢？例如 `string s=user.Name;`,不去分析上下文，你知道这代表什么吗？  

**所以，一个好的变量名，不要太长，也不能过短。字符数在9-16为宜。  **

此外，记住一些对仗词也不错，例如:  

1. begin/and  
2. first/last
3. locked/unlocked
4. old/new
5. min/max
6. 山清对水秀,柳绿对桃红...

## 布尔变量与枚举类型命名
1. 对于Boolean变量，**注意给布尔类型变量赋予隐含的'真/假'意义的名字**。慎用Is%;使用肯定的布尔变量名，用`if(found)`代替`if(notFount)`；
2. 由于c#中枚举的处理方式和类很相似(一以下为反编译结果)，不需要加前缀修饰，直接赋予变量含义即可。  
```
.namespace Variables_Naming
{
    .class public auto ansi sealed Color
        extends [mscorlib]System.Enum
    {
    }

    .class private auto ansi beforefieldinit Program
        extends [mscorlib]System.Object
    {
    }
}
```


