---
date created: 2022-04-27 15:08
date updated: 2022-05-17 14:31
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
	let instance
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

1. 没有自身的 this，this 继承上层作用域；没有 prototype；所以不能 new 调用
2. 没有 arguments，如果使用，取的是上层作用域
3. 不能使用 call bind apply 改变其 this 指向
4. 不能用作 generator，不能使用 yield
5. 对象、原型对象方法，DOM 事件函数不能使用

#### 判断 this

1. 箭头函数 上级作用域
2. new 新创建的对象
3. bind apply call
4. 调用关系，指向上层
5. 普通函数，指向 window | undefined
6. DOM 事件函数，指向 DOM 对象；箭头函数 => window

#### iterator

```js
// obj1
const obj = { a: 1, b: 2, c: 3}
function myIterator (obj) {
	let keys = Object.keys(obj)
	let count = 0
	return {
		next() {
			if (count < keys.length) {
				return { done: true, value: undefined }
			} else {
				return { done: false, value: obj[keys[count++]] }
			}
		}
	}
}

// obj2
obj[Symbol.iterator] = function*() {
	let keys = Object.keys(this)
	for (let k of keys)
		yield obj[k]
}
```
