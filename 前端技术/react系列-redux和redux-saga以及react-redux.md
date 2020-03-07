# redux和redux-saga

> redux:JavaScript状态管理容器，与具体框架无关
> redux-saga:redux的中间件，用来管理数据的异步处理

## redux的简单实现
一个基础的redux的实现，需要生成store的函数、存储state的地方、修改state的方法、监听state修改、以及redux进行扩展的中间件。

### reducer文件和reducer函数
可以将相同业务的多个reducer创建为文件，在该文件中存储并修改数据数据。  
reducer函数是一个[纯函数](https://zh.wikipedia.org/zh-hans/%E7%BA%AF%E5%87%BD%E6%95%B0),用来生成新的state。

```
let initState = {
  count: 0
}

const counterReducer = (state, action) => {
  // 如果state没有值，那就给他初值
  if (!state) {
    state = initState;
  }
  switch (action.type) {
    case 'INCREMENT':
      return {
        ...state,
        count: state.count + 1
      };
    case 'DECREMENT':
      return {
        ...state,
        count: state.count - 1
      };
    default:
      return state;
  }
}

export default counterReducer;
```
### bindReducers
通常我们需要多个reducer，bindReducers函数用来合并多个reducer，并维护根state，这样可以保重单一的数据树。

```
//合并reducer
const combineReducer = (reducers) => {
  // reducerKeys = ['counter','info']
  const reducerKeys = Object.keys(reducers);

  //返回合并后的新的reducer函数
  return function combination(state = {}, action) {
    //生成的新的state
    const nextState = {};

    //遍历执行所有的reducer，整合成为一个新的state
    for (let index = 0; index < reducerKeys.length; index++) {
      const key = reducerKeys[index];
      const reducer = reducers[key];
      //之前key的state  
      const previousStateForKey = state[key];
      // 执行分reducer，获得新的state
      const nextStateForKey = reducer(previousStateForKey, action);

      nextState[key] = nextStateForKey;
    }
    return nextState;
  }
}

export default combineReducer;
```

### createStore
创建store函数，至少需要返回三个接口：getState、dispatch、subscribe。用来获取state，通知reducer执行变更计划，监听state的改动。
```
const createStore = (reducer, initState) => {
  let state = initState;
  let listeners = []; //监听列表

  // 订阅
  function subscribe(listener) {
    listeners.push(listener);
  }

  function dispatch(action) {
    //请按照我的计划修改state
    state = reducer(state, action);
    // 通知
    for (let index = 0; index < listeners.length; index++) {
      const listener = listeners[index];
      listener();
    }
  }

  dispatch({ type: Symbol() });

  function getState() {
    return state;
  }
  return {
    subscribe,
    dispatch,
    getState
  }
}

export default createStore;
```

### dispatch和action
dispatch:通知reducer执行变更计划，参数是action。  
action:一个必须包含type属性的对象。
```
store.dispatch({ type: 'SET_NAME', name: '前端9部' });
```

### 中间件middleware和applyMiddleware
通常我们需要对redux进行一些扩展，需要通过middleware进行操作。
```dotnetcli
const loggerMiddleware = (store) => (next) => (action) => {
  console.log('this state: ', store.getState());
  console.log('action: ', action);
  next(action);
  console.log('next state: ', store.getState());
}

export default loggerMiddleware;
```

中间件通常基于洋葱模型,即请求从外层传入，响应从核心向外层依次执行。而 applyMiddleware就是用于实现redux中间件洋葱模型的函数。其返回一个重写的createStore函数
```dotnetcli
const applyMiddleware = function (...middlewares) {
  //返回一个重写createStore的方法
  return function rewriteCreateStoreFunc(oldCreateStore) {
    // 返回重写的新的createStore
    return function newCreateStore(reducer, initState) {
      //1.生成store
      const store = oldCreateStore(reducer, initState);
      /*给每个 middleware 传下store，相当于 const logger = loggerMiddleware(store);*/
      /* const chain = [exception, time, logger]*/
      const chain = middlewares.map(middleware => middleware(store));
      /* 实现 exception(time((logger(dispatch))))*/
      let dispatch = store.dispatch;
      chain.reverse().forEach(middleware => {
        dispatch = middleware(dispatch);
      });
      /*2. 重写 dispatch*/
      store.dispatch = dispatch;
      return store;
    }
  }
}

export default applyMiddleware;
```
之后在原先的createStore中修改如下：
```dotnetcli
const createStore = (reducer, initState, rewriteCreateStoreFunc) => {
  if (typeof initState === 'function' && typeof rewriteCreateStoreFunc === 'undefined') {
    rewriteCreateStoreFunc = initState;
    initState = undefined;
  }
  //如果有rewiteCreateStoreFunc，就采用新的createStore
  if (rewriteCreateStoreFunc) {
    const newCreateStore = rewriteCreateStoreFunc(createStore);
    return newCreateStore(reducer, initState)
  }
  ...如旧
}

```

至此，一个简单的redux就可以使用了。

## 数据异步处理redux-saga
redux的中间件，由于redux对于处理异步请求很麻烦，因此出现专门解决该问的诸多方案，而redux-saga就是其中较为优秀的一个。

这里不多做介绍，具体可以查看**推荐阅读**中的redux-saga中文文档。

在redux-saga中的基本概念如下：

|基本成员  |作用  |
|---------|---------|
|call和put     |   call(阻塞调用): 创建一个 Effect 描述信息，用来命令 middleware 以参数 args 调用函数 fn(通常可用于执行xhr请求) 。  <br />put: 创建一个 Effect 描述信息，用来命令 middleware 向 Store 发起一个 action。 这个 effect 是非阻塞型的，并且所有向下游抛出的错误（例如在 reducer 中），都不会冒泡回到 saga 当中   |
|take和takeEvery     |     take: 创建一个 Effect 描述信息，用来命令 middleware 在 Store 上等待指定的 action。 在发起与 pattern 匹配的 action 之前，Generator 将暂停。如果以空参数或 '*' 调用 take，那么将匹配所有发起的 action。（例如，take() 将匹配所有 action）<br />  takeEvery(pattern, saga, ...args):在发起（dispatch）到 Store 并且匹配 pattern 的每一个 action 上派生一个 saga。 **和take函数不同之处在于可以针对action指定一个saga**  |
|takeLatest     |   在发起到 Store 并且匹配 pattern 的每一个 action 上派生一个 saga。并自动取消之前所有已经启动但仍在执行中的 saga 任务。  **即执行最新的action**    |
|fork     |  fork(fn, ...args) 创建一个 Effect 描述信息，用来命令 middleware 以 非阻塞调用 的形式执行 fn 。**返回一个Task对象**      |
|cancle     |     cancel(task)  创建一个 Effect 描述信息，用来命令 middleware 取消之前的一个分叉任务。 task: Task - 由之前 fork 指令返回的 Task 对象   。**即取消任务** |
|all和race    |     类似与promise.all 和promise.race    |



## react-redux
react-redux是将redux绑定在react库上。

### Provider组件
将store绑定到对应的组件上
```dotnetcli
import React from 'react'
import { render } from 'react-dom'
import { createStore } from 'redux'
import { Provider } from 'react-redux'
import App from './components/App'
import rootReducer from './reducers'

const store = createStore(rootReducer)

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

### connect
> 高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。
> 高阶组件是参数为组件，返回值为新组件的函数

connect方法就是一个高阶组件，用于从 UI 组件生成容器组件。connect的意思，就是将这两种组件连起来。connect有两个参数:mapStateToProps,
mapDispatchToProps。

1. mapStateToProps:  函数，可以将redux中的state和组件的props建立映射关系
2. mapDispatchToProps：函数or对象，用来建立 UI 组件的参数到store.dispatch方法的映射。也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store(来源阮大)



### 容器组件和UI组件
容器组件
1. 负责管理数据和业务逻辑，不负责 UI 的呈现
2. 带有内部状态
3. 使用 Redux 的 API

UI 组件：
1. 只负责 UI 的呈现，不带有任何业务逻辑
2. 没有状态（即不使用this.state这个变量）
3. 所有数据都由参数（this.props）提供
4. 不使用任何 Redux 的 API

## 推荐阅读

1. [语雀-redux](https://www.yuque.com/fe9/basic/cgkg56)
2. [redux-saga中文文档](https://redux-saga-in-chinese.js.org/)
3. [容器组件和展示组件](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)