---
date created: 2022-05-06 16:54
date updated: 2022-05-06 18:50
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
		throw new Error('TypeError')
	const instance = Object.create(constructor.prototype)
	const res = constructor.apply(instance, args)
	if (typeof res === 'function' || (typeof res === 'object' && res !== null))
		return res
	return instance
}
```

#### 4. 防抖

```js
function debounce(fn, interval = 300) {
	let timer = null
	return function() {
		let context = this
		if (timer) {
			clearTimeout(timer)
			timer = null
		}
		timer = setTimeout(function() {
			fn.apply(context, arguments)
		}, interval)
	}
}
```

#### 5. 节流

```js
function throttle(fn, interval = 300) {
	let lastTime = Date.now()
	return function () {
		let context = this
		let curTime = Date.now()
		if (curTime - lastTime >= interval) {
			lastTime = curTime
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
