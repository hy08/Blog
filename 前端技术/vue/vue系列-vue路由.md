# vue系列-vue路由

1. 使用：vue全局注册vue-router插件,定义路由配置文件
2. 全局路由守卫
3. 组件内部路由监听或者组件内路由守卫。
4. 路由改变滚动到最上方。
```dotnetcli
 const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}    
```

## 基础组件
1. `<router-link>`。 当 `<router-link>` 对应的路由匹配成功，将自动设置 class 属性值 .router-link-active

```dotnetcli
<router-link to="/foo">Go to Foo</router-link>
```

2. `<router-view>`。`<router-view>` 组件是一个 functional 组件，渲染路径匹配到的视图组件。以可以配合 <transition> 和 <keep-alive> 使用。如果两个结合一起用，要确保在内层使用 <keep-alive>
```dotnetcli
<transition>
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</transition>
```
## 常用对象

1. this.$router，路由实例，每个vue组件都可以访问，可以调用push等方法
2. this.$route，路由信息对象，只读，可以读取读取path和params等属性

## router常用的属性和方法
1. push、replace、go、back、forward
2. 一些路由守卫的方法：beforeEach、beforeRouteUpdate、beforeEnter、beforeRouteEnter

## 动态路由
定义：`/user/:username`，使用`this.$router.push('/user/abc')`。

## 路由嵌套
定义:路由对象中追加children数组，父路由追加`<router-view>`

## 命名路由和命名视图
1. 命名路由，和push和`<router-link>`跳转相关
2. 命名视图, 和路由布局相关

## 重定向
在路由对象中添加redirect属性

##  路由守卫和路由元信息
结合二者可以做权限校验

## 路由加载中状态/路由过渡
1. [加载中](https://www.jianshu.com/p/6631752c5d7a)
2. 过渡 `<router-view> 是基本的动态组件，所以我们可以用 <transition> 组件给它添加一些过渡效果`


## 路由懒加载
`const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')`