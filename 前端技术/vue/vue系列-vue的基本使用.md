# vue系列-vue的基本使用

## vue实例
每个应用都通过一个Vue函数创建一个新的vue实例开始的

```dotnetcli
import Vue from "vue";
import App from "./App.vue";
import router from "./router";
import store from "./store";

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount("#app");
```

vue实例向外暴露了一系列的[属性](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E5%B1%9E%E6%80%A7)、[方法](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95-%E6%95%B0%E6%8D%AE)、[事件](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95-%E4%BA%8B%E4%BB%B6)、[生命周期](https://cn.vuejs.org/v2/api/#%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)

## vue组件
假装有内容...
### 渲染
假装有内容...
#### template
1. vue的模板语法
2. 基于html
3. 需要绑定数据和绑定事件
4. 自定义组件最好使用 kebab-case，例如`<my-component-name>`
5. 只有一个根节点

常用指令
1. v-if、v-else、v-else-if 。控制是否渲染元素
2. v-show . v-show 只是简单地切换元素的 CSS 属性 display
3. v-for . v-for='(value,name,index) in object(对象) | item of items(数组)'。一定要指定key值 :key 。v-for优先于v-if
4. v-bind | :
4. v-on | @
5. v-model 用于表单数据绑定

#### render
1. 使用jsx(在vue中使用jsx还有些麻烦)
2. React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合
3. React 并没有采用将标记与逻辑进行分离到不同文件这种人为地分离方式，而是通过将二者共同存放在称之为"组件"的松散耦合单元之中，来实现关注点分离。


#### 总结
从个人角度看我更愿意使用render模式，因为我更认同UI和渲染逻辑不适合做分离，二者是一个整体。  
但是，从团队角度看，当然是少数服从多数啦🙈🙉🙊...

### 样式
假装有内容...
#### class类名
`:class="classObject"`，类似于react中的classnames组件使用。

#### css
可以直接在style中定义样式，scoped约束样式的作用域，防止样式穿透

#### less
第三方的样式预处理器需要按照vue cli要求进行配置

#### scoped样式和css Module
[Vue: scoped 样式与 CSS Module 对比](https://juejin.im/post/5b9556446fb9a05d1b2e3613)

#### 使用全局样式
[使用全局样式](https://juejin.im/entry/5aa4bc316fb9a028d7003a64)

### 脚本
假装有内容...
#### data(基础数据)
vue实例的data必须是纯粹的对象 (含有零个或多个的 key/value 对)。
vue组件的data必须声明为返回一个初始数据对象的函数。因为组件可能被用来创建多个实例。

#### computed(计算属性)
1. computed是一个对象，一般将属性值定义为一个函数。可以和data一样使用。
2. 计算属性默认只有 getter，不过在需要时你也可以提供一个 setter。
3. 作用：基于data中的属性进行复杂计算。
4. 和method的区别：method每次都会执行，浪费性能。computed属性会根据响应式依赖进行缓存。
5. 通常使用计算属性，除非监听到data变化需要进行异步操作和开销较大的时候，使用watch

#### methods(方法)
methods是一个对象，可以在其中定义方法，当事件触发时可以调用

#### watch(侦听属性)
当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

#### Event

example:
```dotnetcli
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) {
      event.preventDefault()
    }
    alert(message)
  }
}
```
1. $event：获取event事件对象
2. @eventName.修饰符

## 组件
假装有内容...
### 组件声明和注册
组件声明简单理解就是创建一个vue文件(template、css、js)。
在其他组件中想要使用自定义组件，需要在components对象中注册组件
```dotnetcli
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
```
### props
1. props需要先进行声明，之后再使用
2. [有类似react propType校验](https://cn.vuejs.org/v2/guide/components-props.html#Prop-%E9%AA%8C%E8%AF%81)
3. prop属性在template中需要用kebab-case (短横线分隔命名)
4. 使用的时候，使用`v-bind:prop='value' | :prop='value'`进行赋值
5. 和react类似，遵从单项数据流原则
6. 开发模式，我觉得react的最佳实践很好

```dotnetcli
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})

<!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```
### 插槽(slot)
1. 类似与react中的children，使用和children一样，在定义的地方改为`<slot></slot>`
2. 注意作用域问题，slot的作用域是定义的组件实例，怎么传参看文档.

### 动态组件和异步组件
1. 动态组件:`v-bind:is='组件对象或者组件名'`,keep-alive保持状态
2. 异步组件，需要配合vue-router和webpack

### 组件通信
假装有内容...
#### 事件通知模式（父子）
1. 始终使用 kebab-case 的事件名
2. $emit(eventName,[eventPrama1,eventPrama2,...])触发事件,v-on监听事件
3. 注意表单组件(v-model)比较特殊
4. 绑定原生事件不奏效的时候看文档：[将原生事件绑定到组件](https://cn.vuejs.org/v2/guide/components-custom-events.html#%E5%B0%86%E5%8E%9F%E7%94%9F%E4%BA%8B%E4%BB%B6%E7%BB%91%E5%AE%9A%E5%88%B0%E7%BB%84%E4%BB%B6)
5. `update:myPropName | .sync`,修改某些简单的属性


#### [vue Bus模式（父子+兄弟）](https://juejin.im/post/5a4353766fb9a044fb080927)

不推荐，建议直接上vuex

#### vuex（父子+兄弟）
见扩展4

### ref和依赖注入
1. 直接就是定义字符串，具体实现看[文档](https://cn.vuejs.org/v2/guide/components-edge-cases.html#%E8%AE%BF%E9%97%AE%E5%AD%90%E7%BB%84%E4%BB%B6%E5%AE%9E%E4%BE%8B%E6%88%96%E5%AD%90%E5%85%83%E7%B4%A0)
2. [依赖注入有点意思](https://cn.vuejs.org/v2/guide/components-edge-cases.html#%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5),类似于react中的Context

### 常用的特殊Api
1.nextTick，由于修改dom之后不能立刻获取更新后的dom实例，调用nextTick可以解决这个问题

## 扩展

1. [vue系列-生命周期](https://github.com/hy08/Blog/blob/master/%E5%89%8D%E7%AB%AF%E6%8A%80%E6%9C%AF/vue%E7%B3%BB%E5%88%97-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.md)
2. [vue系列-数据双向绑定和MVVM原理](https://github.com/liuxiaodeng/Mvue)
3. [vue系列-vue路由](https://github.com/hy08/Blog/blob/master/%E5%89%8D%E7%AB%AF%E6%8A%80%E6%9C%AF/vue%E7%B3%BB%E5%88%97-vue%E8%B7%AF%E7%94%B1.md)
4. [vue系列-vuex](https://github.com/hy08/Blog/blob/master/%E5%89%8D%E7%AB%AF%E6%8A%80%E6%9C%AF/vue%E7%B3%BB%E5%88%97-vuex.md)
5. [vue系列-动画](https://github.com/hy08/Blog/blob/master/%E5%89%8D%E7%AB%AF%E6%8A%80%E6%9C%AF/vue%E7%B3%BB%E5%88%97-%E5%8A%A8%E7%94%BB.md)
6. [vue系列-mixin混入(省市区demo)](https://github.com/hy08/Blog/blob/master/%E5%89%8D%E7%AB%AF%E6%8A%80%E6%9C%AF/vue%E7%B3%BB%E5%88%97-mixin%E6%B7%B7%E5%85%A5(%E7%9C%81%E5%B8%82%E5%8C%BAdemo).md)
7. [vue系列-运用typescript(vue2.x)](https://github.com/hy08/Blog/blob/master/%E5%89%8D%E7%AB%AF%E6%8A%80%E6%9C%AF/vue%E7%B3%BB%E5%88%97-%E8%BF%90%E7%94%A8typescript(vue2.x).md)