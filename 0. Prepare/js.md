---
date created: 2022-04-27 15:08
date updated: 2022-05-17 14:31
---

### 数据类型
#### 类型判断
1. typeof
2. 原型链
	1. Array.prototype.isPrototypeOf([])
	2. Object.getPrototypeOf([]) === Array.prototype
3. 构造函数
	1. [].constructor = Array
	2. [] instanceof Array
4. Object.prototype.toString.call([])