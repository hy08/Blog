# vue系列-vuex
注册vuex
```dotnetcli
// index.js
import store from "./store";
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount("#app");

//store.js
import Vue from "vue";
import Vuex from "vuex";
import mutations from "./mutation.js";
import actions from "./action.js";

Vue.use(Vuex);
const state = {};
export default new Vuex.Store({
  state,
  mutations,
  actions
});
```

## vuex基础成员
### store实例
```dotnetcli
const store = new Vuex.Store({
  state,
  mutations,
  actions
})
```
注册完vuex之后，组件可以通过this.$store来访问实例

### state状态对象
```dotnetcli
const state={};
```
在组件内访问state：
1. `this.$store.state.xxx`, 不推荐
2. 使用 mapState 辅助函数帮助我们生成计算属性. **推荐**

mapState 函数返回的是一个对象，可以使用对象扩展运算符来导入数据
```dotnetcli
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

### getter
有点意思

1. 相当于store中的计算属性，当getter返回对象的时候，可以被缓存，但是getter还可以返回方法，但是结果不缓存，好处是可以传参啊，查询的时候尤其有用。
2. mapGetters 辅助函数。类似于mapState函数

### mutation（变动）
mutation其实是redux的reducer的翻版。用来执行state的变动。

发送变动的commit就是redux中的dispatch的翻版。dispatch发送action（action是一个对象，必须由type属性）。而commit直接发送一个字符串(该字符串就是redux中action的type值)就好了。当然了，也可以提交payload
```dotnetcli
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
store.commit('increment', {
  amount: 10
})
//建议这样使用commit
store.commit({
  type: 'increment',
  amount: 10
})
```

**Mutation 必须是同步函数!! 毕竟是reducer**
组件中想要使用mutation，需要使用mapMutation来将mutation注入到methods中

### action
1. 相当于redux中的effect。
2. 分发action用dispatch，而不是commit
3. mapActions注入action到methods

## 复杂vuex
定义modules,使用map系列方法的时候指定命名空间就好了或者通过`createNamespacedHelpers方法来操作`。

## 和redux的比较
类似于redux中的单项数据流的实现，毕竟都是基于Flux模式。

1. 都需要创建唯一的store（数据仓库实例）以及一个state(状态树)
2. 在redux中store需要由dispatch发送action，由内部reducer执行数据变更。之后通知订阅方
3. 在vuex中store需要由commit发送同步请求，通过dispatch发送异步请求，再有内部方法执行数据变更，之后通知订阅方。