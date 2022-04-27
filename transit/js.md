---
date created: 2022-04-21 16:00
date updated: 2022-04-21 20:23
---

### 数据类型

#### 类型判断

1. typeof 可以判断函数

2. 原型链方法

   > [].**proto** == Array.prototype

   > Object.getPrototype([]) === Array.prototype

   > Array.prototype.isPrototype([])

3. 构造函数方法

   > [].constructor = Array

   > [] instanceof Array

4. 原型链的替换会导致 2, 3 判断出错
   > [].**proto** = Object.prototype

5. 万能方法

> Object.prototype.toString.call([]) === '[object Array]'

5. Array.isArray
6. 手写 instanceof

```js
function myInstance (left, right) {
	let proto = Object.getPrototypeOf(left)
	let prototype = right.prototype
	while (proto !== null) {
		if (prototype === proto)
			return true
		protp = Object.getPrototypeOf(proto)
	}
	return false
}
```

### JS 基础

#### new 一个构造函数

1. 创建一个新对象
2. 将新对象的 **proto** 指向构造函数的 prototype
3. this 指向该新对象
4. 为新对象强行赋值属性和方法
5. 返回新对象的地址，如果构造函数返回了一个引用类型，则前面无效

```js
function myNew() {
	let instance = null
	const args = Array.prototype.slice.call(arguments)
	const constructor = args.shift()
	if (constructor !== 'function') {
		return new TypeError('error')
	}
	instance.__proto__ = constructor.prototype
	const res = constructor.apply(instance, args)
	if (typeof res === 'function' || (typeof res === 'object') && res !== null)
		return res
	return instance
}
```

#### 箭头函数

1. 没有自己的 this，继承上层作用域
2. 没有 prototype 和 arguments，内部访问 arguments 实际返回的是外层的 arguments
3. 不能使用 bind apply call 修改this
4. 无法用 new 新建（prototype 和 this）
5. 对象和原型对象的方法，DOM 事件函数不能使用

#### 判断 this

1. 如果是 new 生成的，新创建的对象
2. bind apply call 绑定的内容
3. 调用，指向调用的函数或对象
4. 指向 window 或者 undefined

#### iterator
```js
const arr = [1, 2, 3]
arr[]
```