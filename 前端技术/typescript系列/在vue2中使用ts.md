## 介绍

文本所介绍的内容是使用 TypeScript 编写 Vue2.6.11 前端应用，具体 demo 地址可访问: [vue-ts-demo](https://github.com/hy08/all-demo/tree/master/vue-demo)。

本文总结几个月来在 ts 环境 中使用 vue 的经验，提炼一个最小可运行案例，该案例将包括：

1. 搭建 ts 项目，配置 tsconfig.json
2. 单文件组件(template 组件)的使用
3. tsx 组件的使用
4. vue-router 的 ts 方案
5. vuex 的 ts 方案
6. api 类型定义的建议

## 项目搭建与配置

ts 环境下 vue2 版本的项目直接使用官方的脚手架 vue-cli 即可，根据项目组情况判断是否需要使用 tsx、css 预处理+css module、单元测试。

项目创建完成，默认生成一份`tsconfig.json`文件。ts 配置项解释可以参考[TypeScript 官方教程](https://www.tslang.cn/docs/handbook/tsconfig-json.html)。

在`package.json`中默认安装[vue-class-component](https://github.com/vuejs/vue-class-component)包，该依赖通过装饰器模式实现了 vue 的 ts 适配，也是官方推荐的使用 ts 方式。不过笔者更建议使用[vue-property-decorator](https://github.com/kaorun343/vue-property-decorator)包，因为后者在前者基础上进行了修改与扩充。`vue-class-component`拥有的功能`vue-property-decorator`都具备，并且功能更强大，也更易于使用。

对于使用 Vuex 的项目，建议安装[vuex-module-decorators](https://github.com/championswimmer/vuex-module-decorators)包，这是在 vue 中使用 ts 的一种解决方案。

由于 vue 对 jsx 的支持问题，如果想实现如同 react 的组件 props 的智能提示，需要安装[vue-tsx-support](https://github.com/wonderful-panda/vue-tsx-support)。

## 单文件组件(template 组件)的使用

### 组件实例

vue-class-component 允许我们通过使用类语法声明 vue 组件，需要使用@Component 修饰。

```
  import { Vue, Component } from 'vue-property-decorator';

  @Component
  export default class Index extends Vue {

  }

  //相当于
  <script>
    module.export = {

    }
  </script>
```

### 生命周期

生命周期钩子的使用和原先使用的区别：在类语法中直接将生命周期生命为方法(方法名称和生命周期名称一致)。

```
  import { Vue, Component } from 'vue-property-decorator';

  @Component
  export default class Index extends Vue {
    created() {
      console.log('created');
    }
    mounted() {
      console.log('mounted');
    }
  }

  //相当于
  <script>
    module.export = {
      created() {
        console.log('created');
      }
      mounted() {
        console.log('mounted');
      }
    }
  </script>
```

### 响应式数据 data

类语法中可以直接定义为类的实例属性作为组件的响应式数据。原始类型的数据不需要定义类型，ts 可以实现类型推断，但是复杂的类型需要定义。  
其中值得注意的一点是：当数据的值是 undefined 或者只定义未赋初值，`vue-class-component`不会将该属性修饰为响应式数据！这会导致异常。推荐方案是进行赋初值，或者扩展一个 null 类型再赋值未 null。

```
  import { Vue, Component } from 'vue-property-decorator';

  type User = {
    name: string;
    age: number;
  };
  @Component
  export default class Index extends Vue {
    message = 'hello world';
    info: User = { name: 'test', age: 25 };
    //如果数据的值是undefined或者未赋初值,则不会成为响应式数据。解决方案：追加类型定义null
    count: number;
  }

  //相当于
  <script>
    module.export = {
      data:function(){
        return {
          message: 'hello world',
          info: { name: 'test', age: 25 };
        }
      }
    }
  </script>
```

### 计算属性 computed

类语法中的计算属性的实现，是通过 get 取值函数。

```
  import { Vue, Component } from 'vue-property-decorator';

  @Component
  export default class Index extends Vue {
    //computed定义
    get introduction() {
      return `姓名：${this.info.name}, 年龄：${this.info.age}`;
    }
  }

  //相当于
  <script>
    module.export = {
      computed:{
        introduction() {
          return `姓名：${this.info.name}, 年龄：${this.info.age}`;
        }
      }
    }
  </script>
```

### 数据监听 watch

类语法实现响应式的数据监听，是由`vue-property-decorator`依赖提供 Watch 装饰器来完成

```
  import { Vue, Component } from 'vue-property-decorator';

  @Component
  export default class Index extends Vue {
    //watch定义，其中Wacth装饰器第一个参数：响应式数据字符串(也可以定义为'a.b');
    //第二个参数options成员[immediate,deep]分别对应的是原生的用法
    @Watch('$route', { immediate: true })
    changeRouter(val: Route, oldVal: Route) {
      console.log('$route watcher: ', val, oldVal);
    }
  }

  //相当于
  <script>
    module.export = {
      watch:{
        '$route':function(val,oldVal) {
          console.log('$route watcher: ', val, oldVal);
        }
      }
    }
  </script>
```

### 方法 methods

在类语法实现原生 vue 的方法的方式，即通过直接定义类方法成员。

```
  import { Vue, Component } from 'vue-property-decorator';

  @Component
  export default class Index extends Vue {
    hello(){
      console.log('hello world');
    }
  }

  //相当于
  <script>
    module.export = {
      methods:{
        hello(){
          console.log('hello world');
        }
      }
    }
  </script>
```

### 引入组件

和原生写法一致，都需要先引入在注册，区别在于类语法注册在修饰器中。组件使用方式和 vue 原生一致。
顺便说一点，如果需要在`.vue`文件中

```
  import { Vue, Component } from 'vue-property-decorator';
  import Header from '../component/header/index.vue';

  @Component({
    components: {
      Header,
    },
  })
  export default class Index extends Vue {
  }

  //相当于
  <script>
    module.export = {
      components: {
        Header,
      }
    }
  </script>
```

### 组件属性 props

类语法实现组件 props 定义是通过装饰器`@Prop`实现

```
  import { Vue, Component, Prop } from 'vue-property-decorator';
  import { User } from '@/types/one';

  @Component
  export default class Header extends Vue {
    @Prop({ type: String, default: '标题' }) readonly title?: string;
    //复杂类型type参数的值为Object，默认值需要以函数形式返回
    @Prop({ type: Object, default: () => ({ name: '-', age: '-' }) }) readonly author!: User;
  }


  //相当于
  <script>
    module.export = {
      props:{
        title:{
          type: String,
          required: false,
          default: '标题'
        },
        author:{
          type: Object,
          required: true,
          default: { name: '-', age: '-' }
        }
      }
    }
  </script>
```

### 事件触发

ts 环境下 vue 的事件触发方式和 js 环境下是一致的，区别只是事件回调定义的地方不同（ts 定义为类的实例方法，js 定义在 methods 属性中）。

### ref 使用

在类语法中使用 ref 需要借助`vue-property-decorator`提供的 Ref 装饰器,使用方法如下：

```
//模板和原生vue保持一致
<template>
  <div class="container">
    <Header ref="header" title="首页" :author="info" />
  </div>
</template>

<script lang="ts">
  import { Vue, Component, Watch, Ref } from 'vue-property-decorator';
  import { Route, NavigationGuardNext } from 'vue-router';
  import Header from '../component/header/index.vue';

  @Component({
    components: {
      Header,
    },
  })
  export default class Index extends Vue {
    @Ref('header') readonly headerRef!: Header;
  }
</script>

//相当于
<script>
  export default  {
    computed():{
      headerRef:{
        cache:false,
        get(){
          return this.$refs.header as Header
        }
      }
    }
  }
</script>
```

### mixins 使用

类语法使用 mixins 需要继承`vue-property-decorator`提供的 Mixins 函数所生成的类。
Mixins 函数的参数是 Vue 实例类，正确使用会用 mixin 成员的的智能提示，使用方式如下：

```
// mixins.js
  import Vue from 'vue';
  import Component from 'vue-class-component';

  // You can declare mixins as the same style as components.
  @Component
  export class Hello extends Vue {
    /**
    *  mixin中的响应式数据
    */
    mixinText = 'Hello mixins';

    obj: { name: string } = { name: 'han' };
  }

//index.vue
<script lang="ts">
  import {  Component, Mixins, Watch, Ref } from 'vue-property-decorator';
  @Component
  export default class Index extends Mixins(Hello) {

    created(){
      console.log(this.mixinText,this.obj.name);
    }
  }
</script>

//相当于
<script>
  export default{
    mixins:{
      data(){
        return {
          mixinText:'Hello mixins',
          obj: { name: 'han' }
        }
      }
    },
    created(){
      console.log(this.mixinText,this.obj.name);
    }
  }
</script>
```

### slots 和 scopedSlots

## tsx 组件的使用

## tsx 组件和 vue 组件

在`.vue`文件中以`<script></script>`方式定义组件，文件跳转正常，但是暂未实现路径的智能提示。
在`.tsx | .ts`文件引入`.vue`,路径智能提示正常，但是会发生无法跳转到 vue 文件的情况。

## vue-router 的 ts 方案

## vuex 的 ts 方案

## api 类型定义的建议
