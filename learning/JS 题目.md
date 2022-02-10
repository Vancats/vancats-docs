
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
**实现bind**
```js
Function.prototype.bind(obj) {
	var fun = this
	var args1 = Array.prototype.slice.call(arguments, 1)
	return function() {
		var args2 = Array.prototype.slice.call(arguments)
		var args = args1.concat(args2)
		fun.apply(obj, args2)
	}
}
```

**深拷贝**
```js
function deepClone(target, map = new Map()) {
	if(typeof target === 'object') {
		let cloneTarget = Array.isArray(target) ? [] : {}
		for(const key in target) {
			// 防止循环引用
			if(map.has(target)) {
				return map.get(target)
			}
			map.set(target, cloneTarget)
			cloneTarget[key] = deepClone(target[key], map)
		}
	} else {
		return target
	}
	return cloneTarget
}
```
###### Promise
```js
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'

class Promise {
	constructor(executor) {
		this.state = PENDING
		this.value = undefined
		this.reason = undefined

		this.onResolvedCallbacks = []
		this.onRejectedCallbacks = []

		let resolve = (value) => {
			if (value instanceof Promise) {
				return value.then(resolve, reject)
			}
			if(this.state === PENDING) {
				this.value = value
				this.state = FULFILLED
				this.onResolvedCallbacks.forEach(fn => fn())
			}
		}

		let reject = (reason) => {
			if(this.state === PENDING) {
				this.reason = reason
				this.state = REJECTED
				this.onRejectedCallbacks.forEach(fn => fn())
			}
		}
	}

	try {
		executor(resolve, reject)
	} catch (err) {
		reject(err)
	}

	then(onFulfilled, onRejected) {
		onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v
		onRejected = typeof onRejected === 'function' ? onRejected : v => v

		let promise2 = new Primise((resolve, reject) => {

			if(this.state === FULFILLED) {
				setTimemout(() => {
					try {
						let x = onFulfilled(this.value)
						resolvePromise(promise2, x, resolve, reject)
					} catch (err) {
						reject(err)
					}
				}, 0)
			}

			if(this.state === REJECTED) {
				setTimemout(() => {
					try {
						let x = onRejected(this.reason)
						reslovePromise(promise2, x, resolve, reject)
					} catch (err) {
						reject(err)
					}
				}, 0)
			}

			// 处理异步任务
			if(this.state === PENDING) {
				this.onResolvedCallbacks.push(() => {
					setTimemout(() => {
						try {
							let x = onFulfilled(this.value)
							reslovePromise(promise2, x, resolve, reject)
						} catch (err) {
							reject(err)
						}
					}, 0)
				})
				this.onRejectedCallbacks.push(() => {
					setTimemout(() => {
						try {
							let x = onRejected(this.reason)
							reslovePromise(promise2, x, resolve, reject)
						} catch (err) {
							reject(err)
						}
					}, 0)
				})
			}
		})
		return promise2
	}
}


const resolvePromise = (promise2, x, resolve, reject) => {
	if (promise2 === x) {
		return reject(new TypeError('Chaining cycle detected for promise #<Promise>'))
	}

	let called
	if ((typeof x === 'object' && x !== null) || typeof x === 'function') {
		try {
			let then = x.then
			if (typeof then === 'function') {
				then.call(x, y => {
					if (called) return
					called = true
					resolvePromise(promise2, y, resolve, reject)
				}, r => {
					if (called) return
					called = true
					reject(r)
				})
			} else {
				resolve(x)
			}
		} catch (err) {
			if (called) return
			called = true
			reject(err)
		}
	} else {
		resolve(x)
	}
}

Promise.resolve = function(value) {
	return new Primise((resolve, reject) => {
		resolve(value)
	})
}

Promise.reject = function(reason) {
	return new Primise((resolve, reject) => {
		reject(reason)
	})
}

Promise.prototype.catch = function(errCallback) {
	return this.then(null, errCallback)
}

Promise.prototype.finally = function(callback) {
	return this.then((value) => {
		return Promise.resolve((callback())).then(() => value)
	}, (reason) => {
		return Promise.resolve((callback())).then(() => { throw reason }3)
	})
}

Promise.all = function(promises) {
	if(!Array.isArray(promises)) {
		const type = typeof promises
		return new TypeError(`TypeError: ${type} ${promises} is not iterable`)
	}

	return new Promise((resolve, reject) => {
		let resultArr = []
		let orderIndex = 0
		const processResultByKey = (promise, index) => {
			resultArr[index] = promise
			if (++orderIndex === promises.length) {
				resolve(resultArr)
			}
		}

		for(let i = 0; i < promises.length; i++) {
			let promise = promises[i]
			if (value && typeof value.then === 'function') {
				value.then((value) => { 
					processResultByKey(value, i)
				}, reject)
			} else { 
				processResultByKey(value, i)
			}
		}
	})
}

Promise.race = function(promises) {
	return new Promise((resolve, reject) => {
		for(let i = 0; i < promises.length; i++) {
			let val = promises[i]
			if (val && (typeof val === 'function')) {
				val.then(resolve, reject)
			} else {
				resolve(val)
			}
		}
	})
}
```