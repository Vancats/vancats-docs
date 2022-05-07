---
date created: 2022-05-06 16:54
date updated: 2022-05-07 18:12
---

# 手写题

#### 1. Object.create

```js
function create(obj) {
	function F() {}
	F.prototype = obj
	return new F()
}
```

#### 2. instanceof

```js
function instanceOf(left, right) {
	const prototype = right.prototype
	const proto = Object.getPrototypeOf(left)
	while (proto !== null) {
		if (proto === prototype)
			return true
		proto = Object.getPrototypeOf(proto)
	}
	return false
}
```

#### 3. new

```js
function new() {
	const args = Array.prototype.slice.call(arguments)
	const constructor = args.shift()
	if (typeof constructor !== 'function')
		throw new TypeError('TypeError')
	const instance = Object.create(constructor.prototype)
	const res = constructor.apply(instance, args)
	if (typeof res === 'function' || (typeof res === 'object' && res !== null))
		return res
	return instance
}
```

#### 4. 防抖

```js
function debounce(fn, interval) {
	let timer = null
	return function() {
		let context = this
		if (timer) {
			clearTimeout(timer)
			timer = null
		}
		timer = setTimeout(() => {
			fn.apply(context, arguments)
		}, interval)
	}
}
```

#### 5. 节流

```js
function throttle(fn, interval) {
	let lastTime = Date.now()
	return function() {
		let context = this
		let curTime = Date.now()
		if (curTime - lastTime >= interval) {
			lastTime = Date.now()
			fn.apply(context, arguments)
		}
	}
}
```

#### 6. getType

```js
function getType(val) {
	if (val === null)
		return "null"
	if (typeof val === 'object') {
		return Object.prototype.toString.call(val).slice(8, -1)
	}
	return typeof val
}
```

#### 7. call

```js
Function.prototype.call = function() {
	if (typeof this !== 'function')
		throw new TypeError('TypeError')
	const args = [...arguments]
	const context = args.shift() || window
	context.fn = this
	const res = context.fn(...args)
	delete context.fn
	return res
}
```

#### 8. apply

```js
Funtion.prototype.apply = function() {
	if (typeof this !== 'function')
		throw new TypeError('TypeError')
	const args = [...arguments]
	const context = args.shift() || window
	context.fn = this
	const res = context.fn(args)
	delete context.fn
	return res
}
```

#### 9. bind

```js
Function.prototype.bind = function() {
	if (typeof this !== 'function')
		throw new TypeError('TypeError')
	const args = [...arguments]
	const context = args.shift()
	const fn = this
	return function Fn() {
		fn.bind(this instance Fn ? this : context, ...args, ...arguments)
	}
}
```

#### 10. 柯里化

```js
function curry(fn, ...args1) {
	let len = fn.length
	return function(...args2) {
		let curArgs = [...args1, ...args2]
		if (curArgs.length >= len) {
			return fn(...curArgs)
		} else {
			return curry.call(null, fn, ...curArgs)
		}
	}
}

function curry(fn, ...args) {
	return args.length < fn.length ? curry.bind(null, fn, ...args) : fn(...args)
}
```

#### 11. 深拷贝

```js
function deepClone(obj, map = new Map()) {
	if (!obj || typeof obj !== 'object')
		return obj

	const res = Array.isArray(obj) ? [] : {}
	if (map.has(obj))
		return map.get(obj)
	map.set(obj, res)
	for (let key in obj) {
		if (obj.hasOwnProperty(key)) {
			res[key] = deepClone(obj[key])
		}
	}
	return res
}
```
