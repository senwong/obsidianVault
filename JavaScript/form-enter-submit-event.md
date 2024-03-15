form的回车触发submit事件不是总是触发，下面是几种特殊情况

# form下面只有1个input

form下只有1个input时，没有submit input 或者submit button时，enter会触发submit事件

```
<form >
	<input  />
</form>
```


# form下面多个input
form下有多个input，但是没有submit 时，不会触发submit事件

```
<form>
	<input />
	<input />
</form>
```

# form下面多个input+submit
给多个input加上submit时，会触发submit元素的click事件+submit事件和
```
<form>
	<input />
	<input />
	<input type="submit" />
</form>
```

# form下面多个input+多个submit
form下有多个submit元素时，回车触发第一个submit元素的click事件+submit事件
```
<form>
	<input />
	<input />
	<input type="submit" />
	<input type="submit" />
</form>
```

# form下click和submit事件
form下submit元素的click事件会触发submit元素的disabled时，click会触发但是不会触发submit事件。
```

const [loading, setLoading] = useState(false);
  const handleClick1 = async () => {
    console.log("handleClick1");
    setLoading(true);
    await sleep(2);
    setLoading(false);
  };
<form>
	<input />
	<input />
	<input type="submit" disabled={loading} onClick={handleClick} />
</form>
```
form的submit事件和click事件的执行顺序：click -> submit。

# demo
demo code: https://replit.com/@WongRichard/form-submit-event

# 测试环境
系统：Mac OS 14.3.1 
浏览器：Chrome 122.0.6261.128，Firefox 123.0.1，Safari 17.3.1 。

# html Specification

参考：https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#implicit-submission