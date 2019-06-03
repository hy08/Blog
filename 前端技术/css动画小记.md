# CSS 动画小记

在工作中时常需要用到动画，但是经常会遗忘动画的具体使用方法，以及对于 animation、transform、transition、translate 经常性的混淆。所以我发现还是又必要记录一下的。

## transform

> MDN: CSS 的 transform 属性允许你旋转，缩放，倾斜或平移给定元素。这是通过修改 CSS 视觉格式化模型的坐标空间来实现的。

这说明，transform 属性只是对给定元素做视觉上的静态变化，并没有动画的特性。但是在我们使用动画的时候，结合 transform 的属性可以做出很多效果，可以说该属性是组成动画的必不可少的成分。

## translate

> translate 其实是属性值，隶属于 transform 属性。通过 translate(x,y) 方法，元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数。通过 translate3d(x,y,z),实现 3d 移动

对于 transform 以及 translateZ,推荐张鑫旭大神的一篇博文:[好吧，CSS3 3D transform 变换，不过如此！](https://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/)

## transition

> MDN: transition 属性是 transition-property，transition-duration，transition-timing-function 和 transition-delay 的一个简写属性。

transition 属性，其含义是过渡，即一个属性值向另一个属性值转变。例如以下代码：

```
p{
  width:100px;
  transition:width 1s easy-in-out;
}
p:hover{
  width:200px;
}
上述代码的作用是当鼠标悬浮在p元素上，宽度完成从100px到200px的转变，耗时1s，时间曲线为easy-in-out
```

当然 transition 可以作用与很多属性，用","分割，也可指定为 all，既适用于所有属性。

transition 过渡的缺点也很明显，就是无法更细粒度的操纵元素的动画效果，另外必须要有触发的条件，例如悬浮或者 JS 等。

## animation

> MDN:CSS animation 属性是 animation-name，animation-duration, animation-timing-function，animation-delay，animation-iteration-count，animation-direction，animation-fill-mode 和 animation-play-state 属性的一个简写属性形式。
>
> animation-name:用来调用@keyframes 定义好的动画，与@keyframes 定义的动画名称一致
>
> animation-duration:指定元素播放动画所持续的时间
>
> animation-timing-function:规定速度效果的速度曲线，是针对每一个小动画所在时间范围的变换速率
>
> animation-delay:定义在浏览器开始执行动画之前等待的时间，值整个 animation 执行之前等待的时间
>
> animation-iteration-count:定义动画的播放次数，可选具体次数或者无限(infinite)
>
> animation-direction:设置动画播放方向：normal(按时间轴顺序),reverse(时间轴反方向运行),alternate(轮流，即来回往复进行),alternate-reverse(动画先反运行再正方向运行，并持续交替运行)
>
> animation-fill-mode:控制动画结束后，元素的样式，有四个值：none(回到动画没开始时的状态)，forwards(动画结束后动画停留在结束状态)，backwords(动画回到第一帧的状态)，both(根据 animation-direction 轮流应用 forwards 和 backwards 规则)，注意与 iteration-count 不要冲突(动画执行无限次)
>
> animation-play-state:控制元素动画的播放状态，通过此来控制动画的暂停和继续，两个值：running(继续)，paused(暂停)

动画的定义方式：

```
@keyframes slidein {
  from { transform: scaleX(0); }
  to { transform: scaleX(1); }
}
```
当然其中也可以使用更细力度的百分数定义动效

## css 硬件加速
目前大多数电脑显卡都支持硬件加速，是的动画更为流畅，那么具体的使用方式是：
```
 transform: translate3d(0, 0, 0);
```
