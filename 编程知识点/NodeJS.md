
# koa洋葱模型
洋葱模型指的是koajs中中间件的执行行为模式，多个中间件组合之后，每个中间件就像洋葱的一层一样，请求从洋葱外部到洋葱里面，再到洋葱外面，最外面的中间件中next方法之前的代码最先执行，next之后的代码最后执行。

![[onion_model.excalidraw|600]]
比如上图中3个middleware中的代码如下：
```js
app.use((ctx, next) => {
	console.log("middleware1 before");
	await next();
	console.log("middleware1 after");
})
```
那么整个执行顺序为：
```
middleware1 before => middleware2 before => middleware3 before => middleware3 after => middleware2 after => middleware1 after
```


## nodejs如何调试
使用vscode debugger terminal启动nodejs进程。
*编译型语言debugger原理*
CPU有*中断*机制，CPU执行完每条指令之后，会检查中断标记。
软中断（INT指令），硬中断（中断寄存器DR0-DR3）

*js中断原理*
v8的debugger协议，V8 Debugger Protocol，debugger就是实现了这个协议，中断/执行js代码。
nodejs的debugger协议：https://nodejs.org/en/learn/getting-started/debugging

*DAP*
适配各种语言的debugger协议：debugger adaptor protocol
