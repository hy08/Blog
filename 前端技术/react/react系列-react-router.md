# react-router知识点梳理

## react-router用处
其实在我思考react-router的用处时候，我有些愣住。  

这不是理所当然的吗！react-router可以是组件路由啊,它可以，额，它可以...  

在不清楚react-router的用处的时候，可以先假设没有react-router会怎样。那么，我们编写一个复杂应用会有什么改变吗？

由于复杂应用一般具有多个页面，在没有react-router的情况下进行页面跳转，必须自己监听history的变化，之后渲染对应组件。  

总体来说，当然可以解决问题，但是自己实现很不优雅而且很麻烦。具体情形可以参照:[react-router简介](https://react-guide.github.io/react-router-cn/docs/Introduction.html)。  

因此，我们有了一个共同的需求。于时，前端各种框架中都但是了类似的router解决方案。在react框架中，react-router诞生，目的就是解决复杂应用中的路由改变导致的组件渲染问题。  

**接下来，我们提到的react-router都是指React-Router 4。**

## react-router组成

### 组件成员
|组件名称  |介绍  |
|---------|---------|
|Router     |    路由容器组件，通常使用其中的BrowserRouter组件    |
|Route     |      路由组件，其中path规定渲染组件路径（匹配的组件都会渲染），exact规定精准匹配      |
|Switch     |     路由容器组件，但是只渲染第一个匹配的组件    |
|Redirect     |      重定向路由组件   |
|Prompt     |     提示路由组件    |
|Link     |     相当于a标签，跳转    |
|NavLink     |    相当于a标签，提供额外的样式   |
|withRouter     |     高阶组件，给需要路由参数的组件传递路由参数    |

### 特殊成员

|成员  |类型  |介绍  |
|---------|---------|---------|
|history     |    属性     |   Router组件的属性，通常指定createBrowserHistory方法生成的对象      |
|createBrowserHistory     |     方法    |     生成Browser history,    `import { createBrowserHistory } from "history";const history = createBrowserHistory();`    |
|location     |    属性     |     路由参数对象,其中有开发者定义的参数    |
|match     |     属性    |     path：match.path 是为路由编写的路径，用于匹配路径模式。用于构建嵌套的 <Route>; <br/> match.url 是浏览器 URL 中的实际路径。URL 匹配的部分。 用于构建嵌套的 <Link>;<br/> 在浏览器中访问 /users/5，那么 match.url将是 "/users/5" 而 match.path 将是 "/users/:userId";  |
|generatePath     |   方法      |    路由跳转`generatePath("/user/:id/:entity(posts|comments)", {id: 1,entity: "posts"});`     |

## 基于路由的代码分割
推荐使用react官方文档介绍的代码分割解决方案：[代码分割](https://react.docschina.org/docs/code-splitting.html)

## react-router路由守卫的实现方式
推荐在路由组件的生命周期内操作

## react-router权限路由
解决方案推荐:
1. [权限路由](https://juejin.im/post/5995a2506fb9a0249975a1a4#heading-10) 
2. [ant-design-pro相关源码](https://pro.ant.design/docs/getting-started-cn)

## 推荐阅读

1. [官方文档](https://reacttraining.com/react-router/core/guides/philosophy)
2. [[译]关于 React Router 4 的一切](https://juejin.im/post/5995a2506fb9a0249975a1a4)