# 调试系列-vscode之node调试备忘录

> 缘起：今来做一个node项目([thinkjs框架](https://thinkjs.org/doc/index.html))，但是遇到了一个令人头疼的调试问题。耗费3天，耗时约4个小时。虽然最终得以解决，却令人无法高兴，因为没有找到具体原因

## 问题突显
按照[thinkjs官方文档(https://thinkjs.org/zh-cn/doc/3.0/debug.html)]配置了vscode的调试文件，一直相安无事。三天前安装并配置了electron项目。在进行另外两个node项目的调试，均报异常(Break set but not yet bound)。不得已，学习vscode项目的调试相关知识。

## 相关资料
1. vscode官方资料：[Nodejs Debugging](https://code.visualstudio.com/docs/nodejs/nodejs-debugging),这份资料详细的介绍了各个属性的含义，以及配置。

2. 掘金社区的一份资料：[nodejs调试指南](https://juejin.im/post/5b60202df265da0f8145f887)，这里介绍了--inspect和--inspect-brk的区别，摘录如下。

> --inspect: 启动debug模式，并监听9229端口（默认端口）；
> --inspect-brk： 启动debug模式，并监听9229端口（默认端口），并在开始处进行断点；

## 结论
上文我说我有两个node项目，一个是我的node案列集合：[node_demo](https://github.com/hy08/node_demo),这个项目的调试问题已经解决，尤其是在index.js文件中的断点想要生效，需要如下设置：`"runtimeArgs": ["--inspect-brk","index.js"]`。但是另一个node项目[demo-thinkjs](https://github.com/hy08/demo-thinkjs)问题遗留。

## 问题遗留
以上的资料足以使我了解vscode配置node项目的调试，但是解决了我的根本问题了吗，并没有，问题还剩下50%。

在thinkjs的项目[demo-thinkjs](https://github.com/hy08/demo-thinkjs)，异步方法内部设置断点依旧无法生效，我尝试了各种方法均告失败。绝望之下，我在异步方法内写了`debugger;`,结果调试立刻跑通，眼泪流下来啊。

可是我却找不到具体原因，忘知悉者提醒，感激不尽。

