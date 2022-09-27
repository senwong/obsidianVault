# 多态
多态就是把做什么和谁去做分开，避免写无数个if_else，增加扩展性。
举一个例子：电影开拍，演员开始背台词，灯光师开始打光，道具师开始撒花。如果是使用面向过程的方式来写代码，就要写如果是演员，演员要干嘛，如果是灯光师，灯光师要干嘛，如果是道具师，道具师要干嘛。这种方式每次新增一个种类时都要去增加if_else，违反开放封闭原则。
下面以一个类似的代码说明这种情况：
```js
const googleMap = {
	show() {
		console.log("渲染google地图");
	}
};

const baiduMap = {
	show() {
		console.log("渲染baidu地图");
	}
};

const renderMap = (type) {
	if (type === "google") {
		googleMap.show();
	} else if (type === "baidu") {
		baiduMap.show();
	}
};
renderMap("google");
renderMap("baidu");

// 修改后
const renderMap = (map) {
	if (map && typeof map.show === "function") {
		map.show();
	}
};

renderMap(googleMap);
renderMap(baiduMap);

// 这样新增加一个地图时，不用修改renderMap函数。

```


# 切片AOP
```js

Function.prototype.before = function(beforeFn) {
	const self = this;
	return function() {
		beforeFn();
		return self.apply(self, arguments);
	}
}

Function.prototype.after = function(afterFn) {
	const self = this;
	return function() {
		const ret = self.apply(self, arguments);
		afterFn();
		return ret;
	}
}


const fn = () => console.log(2);

const fn2 = fn.bfore(() => console.log(1)).after(() => console.log(3));
fn2();


```

# 命令模式
命令模式就是把对象和执行函数代码分开。
新增加一个Command对象，command的receiver是原对象，execute是原来的执行函数，
命令模式的优点：
1. 可以很方便的执行撤销操作，在命令初始化时传入对象的初始状态，执行撤销操作就是再恢复初始状态。