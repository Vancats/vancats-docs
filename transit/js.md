---
date created: 2022-04-27 15:08
date updated: 2022-04-28 18:43
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
6. 手写 instanceof

```js
function myInstanceof(left, right) {
	const proto = right.prototype
	const curProto = left.prototype
	while (curProto !== null) {
		if (curProto )
	}
}
```

#### 类型转换

#### 其他情况

```
```
