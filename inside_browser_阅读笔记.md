>原文链接：https://developer.chrome.com/blog/inside-browser-part1


# Chrome浏览器架构

## Browser Process
- 说明：浏览器进程处理浏览器相关的事务，比如地址栏，书签，前进/后退按钮，网络请求和文件读取等
- 
## Renderer  Process
- 说明：渲染进程负责处理每个tab页的显示

## GPU Process
- 说明：GPU进程负责处理页面到屏幕的显示

## Plugin Process
- 说明：负责处理网站使用的插件，比如Flash。

## 特别说明
 - 在一些低性能的设备上，Chrome会把多个进程的功能合并到一个线程里处理。
 - 每个iframe都是单独的一个渲染进程。


# 渲染过程
## style
- 说明：根据dom节点和css节点，计算每个节点的style属性，即每个元素的`computed style`。
- 进程：main process

## Layout
- 说明：计算每个元素的几何属性，生成`layout tree`。
- 进程：main process


## Paint
- 说明：计算每个元素的先后渲染顺序，生成`paint records`。
- 进程：main process

## Compositing
- 说明：把整个页面分层，生成`layer tree`，`main thread` 把`layer tree`和`paint records`交给`compositor thraed`，`compositor thraed`把每个layer分成小的tile，交给`raster thread`，`raster thread`把tile光栅化之后，把数据放到GPU内存，然后GPU显示图像。
- 进程：main process . main thread --> compositer thread

# 事件处理
## 处理过程
事件处理的开始是`compositor thread`把input event给`main thread`，如果一块区域注册了事件，这块区域被标记为`non-fast scrollable region`，如果事件触发了，`compositor thread`等待`main thread`执行完事件处理函数，执行完毕之后`compositor thread`再合成新的一帧。

## 事件聚合
一些连续事件触发是非常频繁的，比如`touchemove`事件，触发频率肯定超过了屏幕的刷新率，这种情况下，如果每次触发时都去执行`main thread`的处理函数，肯定会造成页面卡顿，所以chrome会把连续事件聚合到一起，在下次`requestAnimationFrame`之前触发处理函数。  
在这种情况下，可以使用`getCoalescedEvents`方法获取连续事件
```
window.addEventListener('pointermove', event => {

	const events = event.getCoalescedEvents();
	
	for (let event of events) {
		
		const x = event.pageX;
		
		const y = event.pageY;
		
		// draw a line using x and y coordinates.
	
	}

});

```
## 特别注意
- 注册事件时避免注册到全局或者大的区域元素上，因为会把整块区域标记为`non-fast scrollable region`，每次合成时都要等待`main thread`处理事件函数，降低了合成速度，所以要尽量在小的元素上注册事件。
- 注册事件时，传入`passive: true`参数，可以让`compositor thread`不等待`main thread`。（但是`composite layer`合成`layer tree`是在`main thread`上进行的，事件处理函数还是会影响渲染速度）。
- 