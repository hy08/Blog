> react基础知识集锦，仅仅是一些基础知识，都是我们必须掌握的知识点。

## 导图镇楼
![思维脑图](https://raw.githubusercontent.com/hy08/Imgs/master/Blog/react%E6%A1%86%E6%9E%B6%E6%A2%B3%E7%90%86(%E5%9F%BA%E6%9C%AC%E6%9E%84%E6%88%90)_%E5%BD%93%E5%89%8D%E7%89%88%E6%9C%AC%EF%BC%9A16.3.png)

## 组件、生命周期与单向数据流
react组件分为function组件和class组件。function组件

### function组件
1. 接受唯一参数，即props
2. 一般只用于简单的渲染，不包含复杂的交互
### class组件
classs组件继承自React.Component，通常用于实现较为复杂的组件。

#### 常用的生命周期
挂载阶段：
|方法名称  |注意事项  |
|---------|---------|
|constructor     |        用于数据的初始化，和事件this绑定。继承的的时候，注意super(porps) |
|componentWillMount     |       之后可能被废弃，慎用。组件挂载前执行  |
|render     |        class组件必须实现的方法，作为渲染的入口，该函数需要保持纯函数 |
|componentDidMount     |      挂载之后执行的方法，通常用于请求数据、事件监听、websocket监听、dom特殊处理等   |

更新阶段：
|方法名称  |注意事项  |
|---------|---------|
|componentWillReceiveProps(nextProps)     |       props改动之后触发，必须要有条件的进行props修改调用操作，否则会导致死循环。之后会废弃，谨慎使用 |
|render     ||
|componentDidUpdate(prevProps,prevState)     |        render之后触发(首次渲染不触发)，建议1和3二选一即可。必须要有条件的进行state、props修改操作，否则会导致死循环 |

销毁阶段：
|方法名称  |注意事项  |
|---------|---------|
|componentWillUnmount     |        卸载前执行，常用于执行销毁操作，例如销毁定时器，移出事件订阅等|

错误处理阶段：
|方法名称  |注意事项  |
|---------|---------|
|static getDerivedStateFromError()     |       用于展示降级UI。注意getDerivedStateFromError() 会在渲染阶段调用，因此不允许出现副作用。 如遇此类情况，请改用 componentDidCatch()。|
|componentCatch()| 允许副作用，常用于记录错误 |


### 单向数据流
1. 每一个组件的属性都只能传递给后代组件，既不能传递给兄弟组件。子组件不能修改(不推荐)父组件数据，只能调用父组件方法修改
2. 状态提升：每一个状态应该对应唯一的数据源，如果多个组件使用了同一个数据，那么要摆该数据提升到这些组件的最高父组件中

## JXS
我对jsx的看法是升级版的组件模板的写法，这方面比vue强，当然现在vue也早支持jsx了。jsx不是传统的html，但是还是需要html的基础，毕竟web端。但是我们可以些加入逻辑控制，个别地方需要注意，例如事件注册的写法，label的for属性等待。  
一下我列出我认为需要注意的：
1. jsx中引用的自定义组件，一定要大写字母开头，否则会被解析为html标签
2. props.children可以访问子元素
3. 数组型jsx注意添加key，不推荐使用索引
4. 条件渲染 &&、? :  ，必须确保&&之前的结果一定是布尔值，如果是0，还是会渲染的之后的组件

## state和props这两个属性
在我看来，state就是改动需要重新渲染的数据集合。那么，我们需要把什么数据列入state中呢？按照官方文档，结论是：**该数据会动态改变，或者异步改变 && 该数据不是来自props && 该数据不能根据state和props计算出来**。

### this.setState()
有四个注意点：
1. 该方法调用后会触发render()函数
2. 该方法调用可以简单点理解，可以当做是异步的，所以不要调用后直接去get参数对象的属性，错误
3. `this.setState(object,[callback]) //该object和state发生浅合并`
4. `this.setState((state,props)=>{//在该函数中总是能取得最新的state}[,callback]) //callback是在设置完state之后调用`

### props
修改props需要调用到父组件对应的方法

## propsType和defaultProps(力荐这两个)
 propsType和defaultProps推荐官方文档：[使用 PropTypes 进行类型检查](https://zh-hans.reactjs.org/docs/typechecking-with-proptypes.html)

 ## 事件处理
 ```
 // 建议使用这种方式，避免在constructor中绑定this，和在jsx的方式，影响性能
 // 此语法确保 `handleClick` 内的 `this` 已被绑定。  
  handleClick = () => { 
    console.log('this is:', this); 
  }
 ```
 ## refs
 ```
 父组件在不得已情况下需要调用子组件的方法时候：
 this.refName=React.createRef(); //定义
 this.refName.current.xxx() ;// 调用
 ```
 ## 最佳实践
 1. 根据设计稿划分组件
 2. 自下而上构建组件的静态版本，既只需要render函数
 3. 确定state的最小集合，并根据接口计算其他依赖state数据
 4. 确定部分state是否需要状态提升，添加其他交互细节
 5. 添加反向数据流，也就是父组件修改自身state的方法