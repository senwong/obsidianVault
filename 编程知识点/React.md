### react原理

react setState之后发生了什么？
setState会把更新放到队列当中，短时间内同一个状态的多次更新会合并，
批量更新

### react hook原理

### react合成事件
目的：
1. 为了兼容不同浏览器，
实现原理：
react17事件是委托在document or，react17是挂在的容器节点上，

### React concurrent mode
__react 并发模式：__
react18新增了并发模式。
__react异步更新：__ 
也叫批量更新，是react的优化措施之一，如果每次setState都去更新虚拟dom和真实dom，会造成非必要的性能浪费。批量更新指的是把同一时间端多次setState尽可能地合并为一次更新，不能在setState之后拿到更新后的值。
例外在setTimeout， promise，原生事件中的setState不会批量更新。
__为什么setTimeout不会异步更新：__
```js
function batchedUpdates(fn, a) {
  var prevExecutionContext = executionContext; // 保存原来的值
  executionContext |= EventContext;
  try {
    return fn(a); // 调用我们的合成事件逻辑onBtnClick
  } finally {
    executionContext = prevExecutionContext; // 函数执行完成恢复成原来的值

    if (executionContext === NoContext) {
      // Flush the immediate callbacks that were scheduled during this batch
      resetRenderTimer();
      flushSyncCallbackQueue();
    }
  }
}

const batchedEventUpdates = batchedUpdates;
```
参考合成事件的处理方式，在回调函数`fn`开始执行之前会设置一个`executionContext`为异步模式，`fn`执行完成后设置成同步模式，如果`fn`是一个异步函数，那么`fn`真正执行的时候，`executionContext`已经恢复成同步模式了。所以异步的时间，或者原生事件（没有经过合成事件的统一代理）是同步模式。
__react18默认开始自动批处理：__
[官方介绍](https://react.dev/blog/2022/03/08/react-18-upgrade-guide#automatic-batching)
react18开始，如果是setTimeout，promise，原生事件的回调函数也会批处理。可以用`flushSync`包一下规避批处理。

### setTransition
react18的新api。react中将用户交互分成`urgent update`紧急更新和`not urgent updates`非紧急更新，比如input输入是紧急更新，用户点击键盘后需要马上看到输入框的字符。根据输入框更新list是非紧急更新，输入完搜索词字后可以等待一下再更新list。`setTransition`就是非紧急更新。
__使用方法：__
```js
import { startTransition } from 'react';

// Urgent: Show what was typed
setInputValue(input);

// Mark any state updates inside as transitions
startTransition(() => {
  // Transition: Show the results
  setSearchQuery(input);
});
```

__和使用setTimeout的区别：__
```js
// Show what you typed
setInputValue(input);

// Show the results
setTimeout(() => {
  setSearchQuery(input);
}, 0);
```
使用throttle或者debounce类似的方案，也能实现类似的效果，区别点是setTimeout固定等待一个时间去执行回调函数，setTransition会在空闲时执行回调函数，相应时间更快。


react异步更新和并发模式的区别？
react异步更新: 
并发模式是指`setState-reander-commit`中`render-commit`阶段





## react-loadable
react-loadable也是使用的import 函数进行动态加载

## suspense-react
suspense的使用场景：
1. 如果子组件需要异步获取数据，在组件获取数据的过程中，显示fallback组件。
2. 如果子组件是一个使用`React.lazy`异步加载的组件，在子组件加载的过程中，显示fallback组件。
工作原理：
1. 子组件是异步获取数据的，`const user = use(fetchData("/api/getUser"))`，需要在`use`方法中`throw` 出一个`promise`对象，react在渲染过程捕获一个异常，异常内容是一个`promise`对象，那么可以通过`promise`对象知道子组件数据是否加载完成，未加载时展示fallback。
2. 如果子组件是异步组件，`React.lazy`返回的是一个`import()`返回的`promise`，`promise`完成后得到组件，然后加载组件。

