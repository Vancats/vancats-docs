---
date created: 2022-05-06 16:54
date updated: 2022-05-09 10:33
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

原始值，数组，对象，循环引用

```js
const mapTag = '[object Map]'
const setTag = '[object Set]'
const arrayTag = '[object Array]'
const objectTag = '[object Object]'

const boolTag = '[object Boolean]'
const numberTag = '[object Number]'
const stringTag = '[object String]'
const symbolTag = '[object Symbol]'
const dateTag = '[object Date]'
const errorTag = '[object Error]'
const regexpTag = '[object RegExp]'

const deepTag = [mapTag, setTag, arrayTag, objectTag]

const getType = function(target) {
	return Object.prototype.toString.call(target)
}

const cloneOtherType = function(target, type) {
	const Ctol = target.constructor
	switch(type) {
		case boolTag:
		case numberTag:
		case stringTag:
		case dateTag:
		case errorTag: 
			return new Ctol(target)
		case symbolTag:
			return cloneSymbol(target)
		case regexpTag:
			return cloneRegExp(target)
	}
}

function deepClone(target, map = new Map()) {
	if (!target || typeof target !== 'object')
		return target

	const type = getType(target)
	let res
	if (deepTag.includes(type)) {
		res = new target.constructor()
	} else {
		return cloneOtherType(target, type)
	}
	if (map.has(target))
		return map.get(target)
	map.set(target, res)
	if (type === setTag) {
		target.forEach(value => res.add(deepClone(value)))
		return res
	}
	if (type === mapTag) {
		target.forEach((value, key) => res.set(key, deepClone(value)))
		return res
	}
	for (let key in target) {
		if (target.hasOwnProperty(key)) {
			res[key] = deepClone(target[key], map)
		}
	}
	return res
}
```
