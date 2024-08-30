

## 闭包

特点：可以访问创建闭包的函数的局部变量，即使创建它的函数已经从stack中弹出了
实现原理：
每个函数有自己的函数作用域，或者叫context，创建函数执行完成之后，返回的函数对象，函数对象存放在堆上，函数对象会关联闭包对象，所以即使创建函数已经执行完毕，函数仍然能访问捕获的变量。

## 原型
![[prototype.excalidraw|900]]

每个对象的`__proto__`指向其原型对象，原型对象的`constructor`指向其构造函数，构造函数的`prototype`指向原型对象。

## new
new \[function\]时发生了什么？
1. 创建一个新的实例对象，把this指向实例对象。
2. 把实例对象的`__proto__`指向函数的`prototype`，如果函数的`prototype`不是对象，指向`Object.prototype`。

## for-of和for-in
for-of 是针对iterable object, 比如String, Array, Map, Set, Arguments
for-in是针对对象的可以枚举的字符串键。

## 事件循环

![[event_loop.png]]
1. 首先会执行同步代码
2. 同步代码执行完毕之后，会检查并执行微任务队列，比如Promise, Mutation Observer
3. 微任务队列清空之后，会检查宏任务队列，比如setTimeout, setInterval, xhr等，执行完一个宏任务之后，会检查并执行微任务队列，微任务执行完成后，再执行下一个宏任务，所有宏任务执行完成后，再次回到callStack。

参考：https://www.lumin.tech/articles/javascript-event-loop/




## Number.MAX_SAFE_INTERGER
为什么是2^53 -1?
安全数的定义：数字n能被精确的表示，并且n+1的的表示不能是n的表示
超过2^53-1之后，会有n === n+1的现象
因为JS中Number是IEEE754规范，数字是64位双精度浮点数组成，1 + 11 + 52组成1位符号数，11位指数，52位


# addEventListener Capture
addEventListener函数的第第三个参数useCapture: boolean表示什么意思？先看下mdn上的说明
>
A boolean value indicating whether events of this type will be dispatched to the registered `listener` _before_ being dispatched to any `EventTarget` beneath it in the DOM tree. Events that are bubbling upward through the tree will not trigger a listener designated to use capture. Event bubbling and capturing are two ways of propagating events that occur in an element that is nested within another element, when both elements have registered a handle for that event. The event propagation mode determines the order in which elements receive the event. See [DOM Level 3 Events](https://www.w3.org/TR/DOM-Level-3-Events/#event-flow) and [JavaScript Event order](https://www.quirksmode.org/js/events_order.html#link4) for a detailed explanation. If not specified, `useCapture` defaults to `false`.

一个布尔值，表示这个类型的时间是否在dom树下面的其他EventTarget触发之前，触发注册的listener。如果useCapture为true，则在冒泡上去的事件不会被捕获。冒泡和捕获是两种繁殖事件的方式在嵌套的元素之间。
dom事件在触发是会经历2个阶段（Phase）先是捕获阶段，再是冒泡阶段。useCapture: true表示时间在捕获阶段触发，false表示事件在冒泡阶段触发。




## cjs实现esm



## Stream API
参考：https://developer.mozilla.org/en-US/docs/Web/API/Streams_API


# DevTools

## memory
使用memory记录页面内存占用情况
*参考资料：*
https://v8.dev/blog/fast-properties
https://developer.chrome.com/docs/devtools/memory-problems/get-started#object_sizes


### 重要概念
一个对象有两种形式的内存占用，对象自身占用的和引用其他对象时占用的内存。
*Shallow Size*: 
*Retained Size*: 

### v8如何存储JS对象
#### name properties
v8在存储对象时，有两种表示方式。第一种，对象key和value都是array，第二种，字典类型。v8尽量避免使用字典存放对象，因为不好做inline cache 。
very fast属性：有的属性直接存放在object里，叫in-object property。获取属性时不用去property store里找。
快属性：去property store里找，属性和值以array的形式存放。获取属性时，现根据属性name去descriptor array找到index，再根据index找到实际的值。
慢属性
#### indexed properties





