# vue系列-生命周期
![vue lifecycle](https://cn.vuejs.org/images/lifecycle.png)

vue和react 一个很大的不同是vue在方法和生命周期的钩子中自动绑定了this，因此不需要使用箭头函数。

## beforeCreate
在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。

基本用不到

## created
1. 数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前尚不可用。

2. 常用于请求数据和数据初始化。此时可以访问data和计算属性以及方法了。

## beforeMount
在挂载开始之前被调用：相关的 render 函数首次被调用。


基本不用。


## mounted

**注意 mounted 不会保证所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以在 mounted 内部使用 vm.$nextTick：**


*常用于获取VNode信息和操作，ajax请求*

## beforeUpdate
数据更新时调用，发生在虚拟 DOM 打补丁之前。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。

## updated
当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。

注意 updated 不会保证所有的子组件也都一起被重绘。如果你希望等到整个视图都重绘完毕，可以在 updated 里使用 vm.$nextTick

**和react一个很大的不同是，vue的数据变化是直接操纵data或者计算数据或者watch，而不是通过`this.setState`,因此也不存在什么prevProp和this.prop,prevState和this.state**

## activated 
被 keep-alive 缓存的组件激活时调用。

## deactivated 
被 keep-alive 缓存的组件停用时调用。

## beforeDestroy
实例销毁之前调用。在这一步，实例仍然完全可用。
常用于销毁定时器、解绑全局事件、销毁插件对象等操作

## destroy
实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。

## errorCaptured
当捕获一个来自子孙组件的错误时被调用。

## 扩展
1. [vue 生命周期深入](https://juejin.im/entry/5aee8fbb518825671952308c)