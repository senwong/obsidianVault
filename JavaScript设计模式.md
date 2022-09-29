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
2. 可以方便实现命令队列，

# 组合模式
集合和集合里的元素有相同的功能，我们在操纵整个集合时，只需要调用集合的功能，集合会依次调用元素的功能，使用者并不关心被调用的是集合还是元素。比如，对文件夹和文件进行复制粘贴，复制粘贴文件夹时或者文件，文件夹和文件的方法是一样的命名，使用者并不关心被复制粘贴的是文件夹还是文件。

# 模板方法（temaplate mathod）
抽象父类：抽象父类中封装了子类的算法框架，实现了一一些公共方法以及封装了子类中所有方法的执行顺序。
子类继承父类：也继承了整个算法结构。
子类把控制权交给父类，父类通知子类，子类只提供设计上的细节。

# 享元模式（flyweight)
享元模式的核心就是运用共享技术来有效支持大量细粒度的对象。
比如需要50个男模特和50个女模特来试内衣，正常做法就是生成100个模特对象。
```javascript

const Model = function(gender, underwear) {
	this.gender = gender;
	this.underwear = underwear;
}
Model.prototype.takePhoto = function() {
	console.log("gender "+ this.gender + "wearing " + this.underwear);
}

for(let i = 0; i < 50; i++) {
	const female = new Model("femal", i);
	female.takePhoto();
}

for(let i = 0; i < 50; i++) {
	const male = new Model("male", i);
	male.takePhoto();
}
```
如果使用享元模式，只需要生成1个男模特和1个女模特，再给他们设置不同的内衣，
```javascript
const female = new Model("female");
for(let i = 0; i < 50; i++) {
	female.underwear = i;
	female.takePhoto();
}

const male = new Model("male");
for(let i = 0; i < 50; i++) {
	male.underwear = i;
	male.takePhoto();
}
```

# 职责链模式
举一个购买手机的例子，购买手机有3中限制：

1. orderType: 表示选择的定金类型。1：支付了500定金，2：支付了200定金，3：普通购买
2. pay: 表示是否支付了定金，为false时，降级为普通购买
3. stock: 表示库存，如果小于0，无法购买，但是支付了定金的用户不受此限制

我们的购买函数可以写成下面这样：
```javascript

function order(orderType, pay, stock) {
	if (orderType === 1) {
		if (pay) {
			console.log("支付了500定金，购买成功")
		} else {
			// 降级为普通购买
			if (stock) {
				console.log("普通购买，购买成功")
			} else {
				console.log("普通购买，购买失败")
			}
		}
	}
	return;
	if (orderType === 2) {
		if (pay) {
			console.log("支付了200定金，购买成功")
		} else {
			// 降级为普通购买
			if (stock) {
				console.log("普通购买，购买成功")
			} else {
				console.log("普通购买，购买失败")
			}
		}
	}
	return;
	if (orderType === 3) {
		// 普通购买
		if (stock) {
			console.log("普通购买，购买成功")
		} else {
			console.log("普通购买，购买失败")
		}
	}
}
```
可以看到，这种情况下，代码里有很多if-else，对于后续的维护很不方便。可以以职责链模式重构。
```javascript
          next            next 
order500 =====> order200 =====> orderNormal
```


