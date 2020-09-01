## 介绍

文本所介绍的内容是使用 TypeScript 编写 Vue2.6.11 前端应用，具体 demo 地址可访问: [vue-ts-demo](https://github.com/hy08/all-demo/tree/master/vue-demo)。

本文总结几个月来在 vue 中使用 ts 的经验，提炼一个最小可运行案例，该案例将包括：

1. 搭建 ts 项目，配置 tsconfig.json
2. 单文件组件(template 组件)的使用
3. tsx 组件的使用
4. vue-router 的 ts 方案
5. vuex 的 ts 方案
6. api 类型定义的建议

## 项目搭建与配置

vue2 版本的 ts 项目直接使用官方的脚手架 vue-cli 即可，根据项目组情况判断是否需要使用 tsx、css 预处理+css module、单元测试。

项目创建完成，默认生成一份`tsconfig.json`文件。ts 配置项解释可以参考[TypeScript 官方教程](https://www.tslang.cn/docs/handbook/tsconfig-json.html)。

在`package.json`中默认安装[vue-class-component](https://github.com/vuejs/vue-class-component)包，该依赖通过装饰器模式实现了 vue 的 ts 适配，也是官方推荐的使用 ts 方式。不过笔者更建议使用[vue-property-decorator](https://github.com/kaorun343/vue-property-decorator)包，因为后者在前者基础上进行了修改与扩充。`vue-class-component`拥有的功能`vue-property-decorator`都具备，并且功能更强大，也更易于使用。

对于使用 Vuex 的项目，建议安装[vuex-module-decorators](https://github.com/championswimmer/vuex-module-decorators)包，这是在 vue 中使用 ts 的一种解决方案。

由于 vue 对 jsx 的支持问题，如果想实现如同 react 的组件 props 的智能提示，需要安装[vue-tsx-support](https://github.com/wonderful-panda/vue-tsx-support)。

## 单文件组件(template 组件)的使用

vue-class-component 允许我们通过使用类语法申明 vue 组件，需要使用@Component 修饰。

```
  import { Vue, Component } from 'vue-property-decorator';
  //引入组件
  @Component
  export default class Index extends Vue {

  }
```

### 组件实例

### 生命周期

### 响应式数据 data

### 计算属性 computer

### 属性监听 watch

### 组件属性 props

### 方法 methods

### 事件触发

### ref 使用

## tsx 组件的使用

## vue-router 的 ts 方案

## vuex 的 ts 方案

## api 类型定义的建议
