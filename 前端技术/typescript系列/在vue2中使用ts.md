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

slots 和 scopedSlots 的使用方式和原生 vue 保持一致。

## tsx 组件的使用

如果在项目中需要使用 jsx，默认 vue-cli 创建项目会提示是否支持 jsx，但是由于 vue 对 jsx 的支持不完善，导致在使用不像 react 那样可以提示组件 props 的类型定义，使用上非常难受。因此引入`vue-tsx-support`解决该问题。详情请见：[vue-tsx-support(github 文档)](https://github.com/wonderful-panda/vue-tsx-support)

至于 在 vue 中如何使用 jsx，推荐[在 Vue 中使用 JSX 的正确姿势](https://zhuanlan.zhihu.com/p/37920151)，该文详细介绍了 vue 实现 jsx 的原理以及几种 props 的区别和使用。

tsx 组件的很多地方和 template 组件使用方式一致，但是 props 定义、scopedSlots 定义和使用，以及引入第三方组件之后的处理方式有差异。其他地方例如生命周期、data、computed、watch、methods、事件触发、ref 使用都是一致的。

### 配置

下载完`vue-tsx-support`，我们需要配置`tsconfig.json`，设置内容如下：

```
 "compilerOptions": {
    "jsx": "preserve",
    "jsxFactory": "VueTsxSupport",
    "...": "..."
  },
```

之后，我们需要在项目入口处引入`import 'vue-tsx-support/enable-check';`。

现在 tsx 组件的 props 智能提示开始生效。

### 组件定义的方式

`vue-tsx-support`支持的 tsx 组件定义方式可以使用类似与原生 vue 的对象的写法，或者类语法编写。笔者更推荐使用类语法编写组件，这样和模板写法也更相近。

如果喜欢接近原生 vue 的对象风格，可以参考：[官方文档](https://github.com/wonderful-panda/vue-tsx-support#writing-components-by-object-style-api-like-vueextend)。

使用类语法编写组件有两种方式：

1. 通过继承`vue-tsx-support`提供的 Component 类来编写
2. 通过继承 Vue 类并且声明\_tsx 成员

笔者一直在使用前者，但是最近总结经验，发现后者更好些。主要是继承 Component 之后使用 mixins 想要有智能提示的话，需要将定义挂载在 Vue 上，不够友好。因此推荐使用：**通过继承 Vue 类并且声明\_tsx 成员**，下文都是争对该方案的说明。

### 组件实例

声明 tsx 组件,文件后缀必须为`.tsx`，这点和 react 不同，react 在 ts 文件中也是可以使用 jsx 的，但是 vue 不可以。如果一定要在`.ts`文件中，可以使用初始定义 jsx 的方式，具体可参照[vue 官网:](https://cn.vuejs.org/v2/guide/render-function.html#%E5%AE%8C%E6%95%B4%E7%A4%BA%E4%BE%8B)。

在 tsx 文件中，声明组件的方式和 template 组件是一致的。

```
import { Vue, Component, Prop } from 'vue-property-decorator';
import * as tsx from 'vue-tsx-support';

@Component
export default class Header extends Vue {
}
```

### dataProps 定义

首先我们需要和 template 组件一样将所有的 props 定义好。

然后根据情况，如果可以将所有 data 数据、computed 方法、方法定义设置为私有，这样可以使用`vue-tsx-support`提供的`AutoProps别名`，来声明 Props。如果有成员需要设置为 public，可以使用 tsx 提供的`PickProps别名`。

```
//AutoProps
import { Vue, Component, Prop } from 'vue-property-decorator';
import * as tsx from 'vue-tsx-support';
import { User } from '@/types/one';
import styles from './index.less';

@Component
export default class Header extends Vue {
  _tsx!: tsx.DeclareProps<tsx.AutoProps<Header>>;

  @Prop({ type: String, default: '标题' }) readonly title?: string;
  @Prop({ type: Object, default: () => ({ name: '-', age: '-' }) }) readonly author!: User;

  private goAboutMe() {
    this.$router.push('/about');
  }

  render() {
    return (
      <div class={styles.header}>
        <div class={styles.title}>
          <h1>{this.title}</h1>
          <span onClick={this.goAboutMe}>
            作者：
            <span>{this.author.name}</span>
          </span>
        </div>
      </div>
    );
  }
}

//PickProps
import { Vue, Component, Prop } from 'vue-property-decorator';
import * as tsx from 'vue-tsx-support';
import { User } from '@/types/one';
import styles from './index.less';

@Component
export default class Header extends Vue {
  _tsx!: tsx.DeclareProps<tsx.PickProps<Header, 'title' | 'author'>>;

  @Prop({ type: String, default: '标题' }) readonly title?: string;
  @Prop({ type: Object, default: () => ({ name: '-', age: '-' }) }) readonly author!: User;

   goAboutMe() {
    this.$router.push('/about');
  }

  render() {
    return (
      <div class={styles.header}>
        <div class={styles.title}>
          <h1>{this.title}</h1>
          <span onClick={this.goAboutMe}>
            作者：
            <span>{this.author.name}</span>
          </span>
        </div>
      </div>
    );
  }
}
```

### eventProps 定义

`_tsx`成员的类型可以定义为交叉类型，将事件类型定义混入到`_tsx`中就可以了

```
import * as tsx from 'vue-tsx-support';
import { Vue, Component, Prop } from 'vue-property-decorator';
export default class Header extends Vue {
  _tsx!: tsx.DeclareProps<tsx.AutoProps<Header>> & tsx.DeclareOnEvents<{ onClick: string }>;
  render(){
    return <div></div>
  }
}

```

### scopedSlotsProps 定义

vue 中的 scopedSlots 相当于 react 中的 renderProp。

tsx 组件中定义如下：

```
import * as tsx from 'vue-tsx-support';
import { Vue, Component, Prop } from 'vue-property-decorator';
export default class Header extends Vue {
  //这样就声明了两个scopedSlot，默认的scopedSlot参数类型为空，header参数类型为string
  $scopedSlots!: tsx.InnerScopedSlots<{ default?: void,header?:string }>;
  render(){
    return <div></div>
  }
}

```

### mixins 使用

mixins 使用和 template 组件保持一致

### 第三方组件 props 推断

由于 vue 实现的 jsx 没有参数类型提示，因此引入第三方组件也是没有 props 提示。所有我们需要使用`vue-tsx-support`来进行 jsx 支持。

这里我创建一份`propsCovert.ts`文件，使用`vue-tsx-support`提供的 ofType 方法来对第三方组件的 props 进行定义推导。

递归第三方组件的 dataProps，并将其类型推导出。eventProps 定义为索引类型，参数类型定义为 any。scopedSlotsProps 同样定义为索引类型，参数类型定义为 any。

之后没此使用第三方组件，只要用 antdPropsConvert 方法包装下即可在使用时得到 props 的智能提示。

```
//propsConvert.ts

import { ofType } from 'vue-tsx-support';

type PowerPartial<T> = {
  // 如果是 object，则递归类型
  [U in keyof T]?: T[U] extends Function ? Function : T[U] extends object ? PowerPartial<T[U]> : T[U];
};
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
type OmitVue<T> = PowerPartial<Omit<T, keyof Vue>>;

interface AnyEvent {
  [key: string]: any;
}
interface AnyScopedSlots {
  [key: string]: any;
}

function antdPropsConvert<T extends Vue>(componentType: new (...args: any[]) => T) {
  return ofType<OmitVue<T>, AnyEvent, AnyScopedSlots>().convert(componentType);
}
export { antdPropsConvert };


// sider.tsx
import { Vue, Component } from 'vue-property-decorator';
import * as tsx from 'vue-tsx-support';
import { Button as AButton } from 'ant-design-vue';
import styles from './index.less';
import { antdPropsConvert } from '@/utils/propsConvert';

const Button = antdPropsConvert(AButton);

@Component
export default class Sider extends Vue {
  _tsx!: tsx.DeclareOnEvents<{ onClick: string }>;

  $scopedSlots!: tsx.InnerScopedSlots<{ default?: void }>;

  render() {
    return (
      <div class={styles.sider}>
        {this.$scopedSlots.default && this.$scopedSlots.default()}
        <div>
          <Button
            type="primary"
            onClick={() => {
              this.$emit('click', '事件触发参数');
            }}
          >
            事件触发
          </Button>
        </div>
      </div>
    );
  }
}

```

### 事件修饰

> [modifiers](https://github.com/wonderful-panda/vue-tsx-support#modifiers)

## 遗留问题

在单文件组件模式中，文件跳转正常(ctrl+鼠标点击可以跳转到定义)，但是暂未实现路径的智能提示。
在`.tsx | .ts`文件引入`.vue`,路径智能提示正常，但是会发生无法跳转到 vue 文件的情况。

## vue-router 的 ts 方案

### 路由定义和引用

### 路由钩子函数定义

## vuex 的 ts 方案

## api 类型定义的建议
