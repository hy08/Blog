# 防抖和节流的实现和场景应用

> 防抖函数再在工作中用了很多次，但是很少关心他的实现，也不怎么分析其使用场景。
> 前些日子，前端leader问我知道防抖和节流的区别吗。那一刻，我懵了懵，是时候真正的了解这俩货了。

## 防抖
所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。  

### 源码

```dotnetcli
/**
 * 防抖debounce:所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，
 * 如果在 n 秒内又触发了事件，则会重新计算函数执行时间。
 * 
 * @param {Function} func
 * @param {Number} wait 毫秒数
 * @param {Boolean} immediate 是否立刻执行
 * @returns {Function} 添加防抖效果之后的函数
 */
function debounce(func, wait, immediate = true) {
  let timeoutId = null;
  return function () {
    const context = this;
    const args = arguments;
    if (!!timeoutId) {
      clearTimeout(timeoutId);
    }
    if (immediate) {
      let canCall = !timeoutId;
      timeoutId = setTimeout(() => {
        timeoutId = null;
      }, wait);
      if (canCall) {
        func.call(context, ...args);
      }
    } else {
      timeoutId = setTimeout(() => {
        func.apply(context, args);  //给func绑定this以及参数
      }, wait);
    }
  }
}
```
这里需要注意定时器一定要考虑清除，否则存在内存泄漏问题。另外就是关于this的绑定和调用方参数的传递，此参数传递和func的参数传递不同。

### 使用场景
适用于：防止频繁的离散型操作（例如按钮点击，例如键盘输入），希望约束函数执行次数

## 节流
节流throttle:所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数。
节流会稀释函数的执行频率。

### 源码
```dotnetcli
/**
 * 节流throttle:所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数。
 * 节流会稀释函数的执行频率。
 * @param {Function} func 执行函数
 * @param {Number} wait 毫秒数
 * @param {boolean} [immediate=true] 是否立刻执行
 * @returns
 */
function throttle(func, wait, immediate = true) {
  let prevTime = 0;
  let first = true;
  const execu = ({ currentTime, context, args }) => {
    if (currentTime >= prevTime) {
      func.apply(context, args);
      first = true;
    }
  };
  const reset = (currentTime) => {
    if (first) {
      prevTime = currentTime + wait;
      first = false;
    }
  }
  return function () {
    const context = this;
    const args = arguments;
    const currentTime = Date.now();
    if (immediate) {
      execu({ currentTime, context, args });
      reset(currentTime);
    } else {
      reset(currentTime);
      execu({ currentTime, context, args });
    }
  }
}
```
这里需要注意的是重置prevTime与first的条件和时机。

### 使用场景
适用于：防止频繁的连续型操作（例如滑动|滚动、鼠标移动事件）

## 小问题
那么怎么向调用的函数（源码中的func）中传递参数呢？方法不止一个，这个大家可以发散下。

## 推荐阅读

1. [防抖和节流](https://www.jianshu.com/p/c8b86b09daf0)