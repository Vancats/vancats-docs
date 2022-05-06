---
date created: 2022-05-06 16:54
date updated: 2022-05-06 17:03
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

#### new

```js
function new() {
	const args = Array.prototype.slice.call(arguments)
	const constructor = args.shift()
	if (typeof constructor !== 'function')
		throw new Error('TypeError')
	const instance = Object.create(constructor.prototype)
	const res = 
}
```
