---
layout: post
categories: 编程
title: "学无止尽"
tags: [JavaScript,HTML,.Net]
---
一些知识点，加强一下记忆。
1. JS中变量的提升、函数的提升、还有闭包  
2. JQuery的data()方法  
3. input的autocomplete属性  
4. .Net的里氏替换  
<!-- more -->

变量的提升
--
先前看阮一峰大大的[JavaScript 标准参考教程](http://javascript.ruanyifeng.com/ "JavaScript 标准参考教程（alpha）")，才算接触了这个知识点。**所说的变量提升，也就是在JS运行的时候由var命名的变量会在一开始就执行。注意只是变量的声明提前，而不是整个赋值语句的提前**  

```
console.log(a); //执行结果：undefined  
var a=4;

//上面两行代码等同于：
var a;
console.log(a);
a=4;
```

**但是，如果变量不是由var声明的，则该变量不会被提升，而是作为全局变量，充当window对象的属性。**  
```
console.log(a); //执行结果： Uncaught ReferenceError: a is not defined
a=4;
```

函数的提升
--
函数也存在提升的现象，相当于在调用之前已经声明。  
```
ff(); //执行结果： 弹出1
function ff(){
  alert(1);
}

//上面代码相当于：
function ff(){
  alert(1);
}
ff();
```

**最后一句话：变量的提升，与函数的提升，和变量的作用域(关于作用域，要看的是定义时候的作用域，而不是调用时所在的作用域。这几乎是阮大大的原话啦)更配哦！**


闭包
--
由于变量作用域的限制，函数内部可以访问外部变量，但是在函数外部则无法读取到内部的变量。为了解决这一问题，出现了闭包的运用。  
```
function a(){
  var n=10;
  var m=20;
  //现在怎么在外部获取n、m这两个变量呢？
  //答案是：返回一个包括这两个变量的对象
  return {
    n:n,
    m:m
  }
}
var b=a();
console.log(b.n); //执行结果： 10
console.log(b.m); //执行结果： 20
```

**最后一句话：详情请见 -> [阮大大的闭包介绍](http://javascript.ruanyifeng.com/grammar/function.html#toc23 "阮大大的闭包介绍")**

Jquery的data()方法
--
最近在学习一个有意思的UI框架:LayUI,我想知道里面动画的实现方式。结果在js中找到了这么几行代码：  
```
$('.layui-anim').on('click',function () {
	var othis=$(this),oanim=othis.data('anim');
	othis.removeClass(oanim);
	setTimeout(function () {
		othis.addClass(oanim);
	});
});
//这几行代码的含义是：获取当前触发事件的对象，并获取其data-anim自定义属性的值；
//然后对其类名进行增减以操控样式。
```

**由此可知,data方法的作用就是获取自定义属性data-\*的属性值。**  

最后一句话：有关data()方法更清楚的解释请见[这里](https://segmentfault.com/a/1190000005770912 "这里")

input的autocompelete属性
--
其属性值只有两个：off/on,前者代表取消input的自动完成功能，后者正好相发。

.Net的里氏替换
--
人生真奇妙，我现在是.Net开发实习生。从未想过这一天的，本来一直觉得会从事前端的。哪想得....  

算了，感叹完毕。

至于里氏替换，总结起来就是父类对象可以存储子类对象，如果父类对象存储子类对象，那么使用is或者as转换+判断，可以进行类型转换。  

另外重要的一点运用就是，当我们在方法参数传递得时候，如果形参是父类对象，我们可以传一个子类对象过去。