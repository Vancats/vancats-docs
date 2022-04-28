---
date created: 2022-04-27 15:08
date updated: 2022-04-28 19:41
---

### 数据类型

#### 类型判断

1. typeof 判断 function
2. 原型链
   1. [].**proto** === Array.prototype
   2. Object.getPrototype([]) === Array.prototype
   3. Array.prototype.isPrototypeOf([])
3. 构造函数
   1. [].constructor === Array
   2. [] instanceof Array
4. Object.prototype.toString.call([])
5. Array.isArray

**手写 instanceof**

```js
function myInstanceof(left, right) {
	const proto = right.prototype
	let curProto = Object.getPrototypeOf(left)
	while (curProto !== null) {
		if (curProto === proto)
			return true
		curProto = Object.getPrototypeOf(curProto)
	}
	return false
}
```

#### 类型转换

#### 其他情况

### JS 基础

#### new 一个构造函数

1. 创建一个新对象
2. 设置该对象的 **proto** 指向构造函数的 prototype
3. 构造函数的 this 指向新对象，为新对象添加属性和方法
4. 如果构造函数的返回值是一个引用类型，返回该引用类型
5. 返回新对象

**手写 new**

```js
function myNew() {
	const args = Array.prototype.slice.call(arguments)
	const instance
	let constructor = args.shift()
	if (typeof constructor !== 'function')
		throw new TypeError('TypeError!')

	// 二选一
	instance = Object.create(constructor.prototype)
	instance.__proto__ = constructor.prototype
	
	const res = constructor.apply(instance, args)
	if (res && (typeof res === 'object' || typeof res === 'function'))
		return res
	return instance
}

```

#### 箭头函数

1. 没有自身的 this，没有 prototype，所以不能 new 调用
2. 没有 arguments
3.
