---
date created: 2022-05-06 16:54
date updated: 2022-05-06 16:59
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
2. s