
# 自定义class实现`for in`
第一种方法：实现__iter__和__next__方法
第二种方法：实现使用yeild生成一个生成器函数
```py

第一种
class MyCls:
	def __init__(self, data):
		self.data = data;
	def __iter__(self):
		self.current = 0
		return self
	def __next__(self):
		if self.current + 1 < len(self.data):
			return self.data[self.current++]
		else:
			raise StopIteration

```

```py
第二种
class MyCls:
	def __init__(self, data):
		self.data = data;
	def __iter__(self):
		self.current = 0
		while self.current + 1 < len(self.data):
			yield self.data[self.current]
			self.current++

```


# 自定义class实现`len`

```py
class MyList: 
	def __init__(self, data):
		self.data = data 
	def __len__(self):
		return len(self.data)
```

