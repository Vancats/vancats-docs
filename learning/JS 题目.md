---
date created: 2022-03-03 00:34
date updated: 2022-05-16 11:16
---

```js
 var a = 2
 var obj = {
	 a: 4,
	 fn1: (function () {
		 this.a *= 2
		 var a = 3
		 return function () {
			 this.a *= 2
			 a *= 3
			 console.log(a)
		 }
	 })()
 }
 var fn1 = obj.fn1
 console.log(a) // 4
 fn1() // 9
 obj.fn1() // 27
 console.log(a) // 8
 console.log(obj.a) // 8
```

```js
for(var i = 0; i < 4; i++) {
	(function(i) {
		btns[i].onclick = function() {
			alert(i)
		}
	})(i)
}
```

```js
function fun(n, o) {
	console.log(o)
	return {
		fun: function(m) {
			return fun(m, n)
		}
	}
}

var a = fun(0)  // undefined
	a.fun(1)    // 0
	a.fun(2)    // 0
	a.fun(3)    // 0
var b = fun(0)  // undefined
		.fun(1) // 0
		.fun(2) // 1
		.fun(3) // 2
var c = fun(0)  // undefined
		.fun(1) // 0
	c.fun(2)    // 1
	c.fun(3)    // 1
```

```js
// 地址相关
var a = { n: 1 }
var b = a
a.x = a = { n: 2 }
console.log(a) // { n: 2 }
console.log(b) // { n: 1, x: { n: 2 } }
a.n = 3
console.log(b) // { n: 1, x: { n: 3 } }
```

```js
var a = {}
var b = {key: 'a'}
var c = {key: 'c'}
a[b] = '123' // b -> "[object Object]"  -> a["[object Object]"] = '123'
a[c] = '456' // a["[object Object]"] = '456'
console.log(a[b]) // '456'
```

```js
function Foo() {
	Foo.a = function() { console.log(1) }
	this.a = function() { console.log(2) }
}
Foo.a = function() { console.log(4) }
Foo.prototype.a = function() { console.log(3) }
Foo.a() // 4
var foo = new Foo()
foo.a() // 2
Foo.a() // 1
```

```js
function A() {}
function B() { return new A() }
A.prototype = new A()
B.prototype = new B()
var a = new A()
var b = new B()
console.log(a.__proto__ === b.__proto__) // true
```

```js
function Foo() {
	// 修改 window 的 getName 
	getName() {
		console.log(1)
	}
	return this // this -> window
}
Foo.getName = function() {
	console.log(2)
}
Foo.prototype.getName = function() {
	console.log(3)
}
var getName = function() {
	console.log(4)
}
function getName() {
	console.log(5)
}

Foo.getName()             // 2
getName()                 // 4
Foo().getName()           // 1
getName()                 // 1
new Foo.getName()         // 2
new Foo().getName()       // 3
new new Foo().getName()   // 3
```

#### qq

```js
eg1
// 在块内执行函数，即为将当前的内容映射到全局
console.log(foo) // undefined
{
	console.log(foo) // foo {}
	function foo () { }
	foo = 1
	console.log(foo) // 1
}
console.log(foo) // foo {}


eg2
console.log(foo) // undefined
{
	console.log(foo) // foo {2}
	function foo () { 1 } // 将当前的 foo 映射到全局   [G]:foo = foo {2}
	console.log(foo) // foo {2}
	foo = 1
	console.log(foo) // 1
	function foo () { 2 } // 将当前的 foo 映射到全局    [G]:foo = 1
	console.log(foo) // 1
}
console.log(foo) // 1
  

eg3
// 这是正常情况，只有 Local 和 Global 两层作用域
var a = 1
function func (a, b = function anonymous1 () { a = 2 }) { // [G]:a = 1 [Local]:a = 5, b = fun1
	a = 3 // [G]:a = 1 [Local]:a = 3, b = fun1
	b() // [G]:a = 1 [Local]:a = 2 b = fun1
	console.log(a) // 2
}
func(5)
console.log(a)


eg4
// 当使用了形参赋值默认值 并且函数体有 var let const 时，会创建一个 Block，存放里面的所有内容
var a = 1 [G]:a = 1
function func (a, b = function anonymous1 () { a = 2 }) { // [G]:a = 1 [Local]:a = 5, b = fun1
	var a = 3 // [G]:a = 1 [Local]:a = 5, b = fun1 [Block]:a = 3
	b() // [G]:a = 1 [Local]:a = 2, b = fun1 [Block]:a = 3
	console.log(a) // 3
}
func(5)
console.log(a) // 1


eg5
var a = 1
function func (a, b = function anonymous1 () { a = 2 }) {
	var a = 3 // [G]:a = 1 [Local]:a = 5, b = fun1 [Block]:a = 3
	b = function anonymous2 () { a = 4 } // [G]:a = 1 [Local]:a = 2, b = fun2 [Block]:a = 3
	b() // [G]:a = 1 [Local]:a = 5, b = fun2 [Block]:a = 4
	console.log(a) // 4
}
func(5)
console.log(a) // 1
```
