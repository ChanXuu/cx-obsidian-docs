## 1.接口返回的是大数据的字符串
#### JSON.parse()
	这个方法使用时得保证返回的字符串是标准格式的JSON字符串
#### eval('(' + data + ')')

	1. eval()会将传入的字符串转化成js代码进行执行
	2. 如果传入的是String,会转化成为js 进行执行
	3. 如果传入非String，则会直接返回该值。
	4. 使用时需要加上括号 `eval('(' + data + ')')`，让eval执行表达式，而不是把data当成语句执行
	5. 永远不要使用`eval`，因为它不安全（容易被脚本注入）和有性能问题（调用js解析器，会进行冗余的变量名查找、确定位置、设置值等一系列操作）。
#### new Function()
使用`new Function()`替代`eval` 
`new Function()`只能接受对象字符串
不需要使用括号`()`进行转义
```javascript
// 转化json字符串
new Function('return' + '{}')()  // {}

// 转化array字符串
new Function('return' + '[1, 2]')()  // [1, 2]

// 转化function字符串
new Function("return function foo() { console.log(123)}")()() // 123

// 转化对象
new Function('return' + {})()  // error: Unexpected identifier 'Object'

// 转化字符串
new Function('return' + '1+1')()  // error: return1 is not defined
```
