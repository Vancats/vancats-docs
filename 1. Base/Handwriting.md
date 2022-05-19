---
date created: 2022-05-06 16:54
date updated: 2022-05-19 16:46
---

#### 12. AJAX

```js
function ajax(url, method) {
	let xhr = new XMLHttpRequest()
	xhr.open(method, url, true)
	xhr.onreadystatechange = function() {
		if (this.readystate !== 4)
			return
		if (this.status === 200)
			handle(this.response)
		else
			throw new Error(this.statusText)
	}
	xhr.onerror = function() {
		console.warn(this.statusText)
	}
	xhr.responseType = 'json'
	xhr.setRequestHeader('Accept', 'application/json')
	xhr.send(null)
}

function getAjax(url, method) {
	return new Promise((res, rej) => {
	let xhr = new XMLHttpRequest()
		xhr.open(method, url, true)
		xhr.onreadystatechange = function() {
			if (this.readystate !== 4)
				return
			if (this.status === 200)
				res(this.response)
			else
				rej(new Error(this.statusText))
		}
		xhr.onerror = function() {
			rej(new Error(this.statusText))
		}
		xhr.responseType = 'json'
		xhr.setRequestHeader('Accept', 'application/json')
		xhr.send(null)
	})
}
```

#### 13. Promise

```ts
// 1. 设置状态和值
// 2. resolve 和 reject 修改值
// 3. fn 函数的错误处理
// 4. 值穿透
// 5. 任务队列和错误处理
// 6. 两个延迟执行函数队列
// 7. resolvePromise
//    1. 循环爆栈处理
//    2. 普通值和没有 then 方法的值直接 resolve
//    3. 执行函数并递归调用 resolvePromise 注意要绑定 this
const PENDING = "PENDING"
const FULFILLED = "FULFILLED"
const REJECTED = "REJECTED"
class MyPromise {
  constructor(fn) {
    if (typeof fn !== 'function')
      return new TypeError(`Promise resolver ${fn} is not a function`)

    this.PromiseState = PENDING
    this.PromiseResult = null

    this.onFulfilledCallbacks = []
    this.onRejectedCallbacks = []

    const resolve = (value) => {
      this.PromiseResult = value
      this.PromiseState = FULFILLED
      this.onFulfilledCallbacks.forEach(fn => fn())
    }

    const reject = (reason) => {
      this.PromiseResult = reason
      this.PromiseState = REJECTED
      this.onRejectedCallbacks.forEach(fn => fn())
    }

    try {
      fn(resolve, reject)
    } catch (e) {
      reject(e)
    }
  }

  then(onFulfilled, onRejected) {
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value
    onRejected = typeof onRejected === 'function' ? onRejected : reason => { throw reason }

    const promise2 = new MyPromise((resolve, reject) => {

      if (this.PromiseState === FULFILLED) {
        process.nextTick(() => {
          try {
            const x = onFulfilled(this.PromiseResult)
            resolvePromise(promise2, x, resolve, reject)
          } catch (e) {
            reject(e)
          }
        })
      }

      if (this.PromiseState === REJECTED) {
        process.nextTick(() => {
          try {
            const x = onRejected(this.PromiseResult)
            resolvePromise(promise2, x, resolve, reject)
          } catch (e) {
            reject(e)
          }
        })
      }

      if (this.PromiseState === PENDING) {
        this.onFulfilledCallbacks.push(() => {
          process.nextTick(() => {
            try {
              const x = onFulfilled(this.PromiseResult)
              resolvePromise(promise2, x, resolve, reject)
            } catch (e) {
              reject(e)
            }
          })
        })

        this.onRejectedCallbacks.push(() => {
          process.nextTick(() => {
            try {
              const x = onRejected(this.PromiseResult)
              resolvePromise(promise2, x, resolve, reject)
            } catch (e) {
              reject(e)
            }
          })
        })
      }
    })
    return promise2
  }

  catch(onRejected) {
    return this.then(undefined, onRejected)
  }

  finally(callback) {
    return this.then(callback, callback)
  }
}

const resolvePromise = (promise2, x, resolve, reject) => {
  if (promise2 === x)
    return reject(new TypeError("Chaining cycle detected for promise #<Promise>"))

  if (x && (typeof x === 'object' || typeof x === 'function')) {
    let called = false
    try {
      const then = x.then
      if (typeof then !== 'function') {
        resolve(x)
      } else {
        then.call(x,
          (value) => {
            if (called) return
            called = true
            resolvePromise(promise2, value, resolve, reject)
          },
          (reason) => {
            if (called) return
            called = true
            reject(reason)
          }
        )
      }
    } catch (e) {
      if (called) return
      called = true
      reject(e)
    }
  } else {
    resolve(x)
  }
}

MyPromise.resolve = (value) => {
  return new MyPromise((resolve, reject) => {
    if (value in MyPromise) {
      return value
    } else if (typeof value === 'object' && 'then' in value) {
      return value.then(resolve, reject)
    } else {
      return new MyPromise(resolve => resolve(value))
    }
  })
}

MyPromise.reject = (reason) => {
  return new MyPromise((resolve, reject) => { reject(reason) })
}

MyPromise.all = (promises) => {
  return new MyPromise((resolve, reject) => {
    if (!Array.isArray(promises))
      return reject(new TypeError('Arguments is not iterator'))

    const n = promises.length
    if (!n)
      return resolve(promises)

    let cnt = 0
    const res = new Array(n)
    promises.forEach((promise, index) => {
      MyPromise.resolve(promise).then(
        value => {
          cnt++
          res[index] = value
          cnt === n && resolve(res)
        },
        reason => reject(reason)
      )
    })
  })
}

MyPromise.race = (promises) => {
  return new MyPromise((resolve, reject) => {
    if (!Array.isArray(promises))
      return reject(new TypeError('Arguments is not iterator'))
    if (promises.length > 0) {
      promises.forEach(promise => {
        MyPromise.resolve(promise).then(resolve, reject)
      })
    }
  })
}

MyPromise.allSettled = (promises) => {
  return new MyPromise((resolve, reject) => {
    if (!Array.isArray(promises))
      return reject(new TypeError('Arguments is not iterator'))

    const n = promises.length
    if (!n)
      resolve(promises)

    let cnt = 0
    const res = new Array(n)
    promises.forEach((promise, index) => {
      MyPromise.resolve(promise).then(
        value => {
          cnt++
          res[index] = {
            status: 'fulfilled',
            value
          }
          cnt === n && resolve(res)
        },
        reason => {
          cnt++
          res[index] = {
            status: 'rejected',
            reason
          }
          cnt === n && reject(res)
        }
      )
    })
  })
}

MyPromise.any = (promises) => {
  return new MyPromise((resolve, reject) => {
    if (!Array.isArray(promises))
      return reject(new TypeError('Arguments is not iterator'))

    const n = promises.length
    if (!n)
      return reject(new AggregateError('All promises were iterator'))

    let cnt = 0
    const errors = new Array(n)
    promises.forEach((promise, index) => {
      MyPromise.resolve(promise).then(
        value => reject(value),
        reason => {
          cnt++
          errors[index] = reason
          cnt === n && reject(new AggregateError(errors))
        }
      )
    })
  })
}
```

#### 14. 红绿灯打印

```ts
function red() {
  console.log('red')
}
function green() {
  console.log('green')
}
function yellow() {
  console.log('yellow')
}

function task(color, timeout, callback) {
  setTimeout(() => {
    if (color === 'red') {
      red()
    } else if (color === 'green') {
      green()
    } else {
      yellow()
    }
    callback()
  }, timeout)
}

function task1(color, timeout) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (color === 'red') {
        red()
      } else if (color === 'green') {
        green()
      } else {
        yellow()
      }
      resolve()
    }, timeout)
  })
}

const step = () => {
  task('red', 1000, () => {
    task('green', 1000, () => {
      task('yellow', 1000, step)
    })
  })
}

const step1 = () => {
  task1('red', 1000)
    .then(() => task1('green', 1000))
    .then(() => task1('yellow', 1000))
    .then(step1)
}

const step2 = async () => {
  await task1('red', 1000)
  await task1('green', 1000)
  await task1('yellow', 1000)
  step2()
}
step2()
```

#### 15. 图片异步加载

```ts
let imgAsync = (url) => {
  return new Promise((resolve, reject) => {
    const img = new Image()
    img.src = url
    img.onload = () => {
      console.log('123')
      resolve(img)
    }
    img.onerror = (err) => {
      console.log('456')
      reject(err)
    }
  })
}

imgAsync('https://img0.baidu.com/it/u=2862534777,914942650&fm=253&fmt=auto&app=138&f=JPEG?w=889&h=500').then(() => {
  console.log('加载成功')
}).catch(err => {
  console.log('err: ', err)
  console.log('加载失败')
})
```

#### 20. 封装 fetch

```js
(async () => {
  class HttpRequestUtil {
    async get(url) {
      const res = await fetch(url)
      const data = await res.json()
      return data
    }

    async post(url, data) {
      const res = await fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      })
      const result = await res.json()
      return result
    }

    async put(url, data) {
      const res = await fetch(url, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      })
      const result = await res.json()
      return result
    }

    async delete(url, data) {
      const res = await fetch(url, {
        method: 'DELETE',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      })
      const result = await res.json()
      return result
    }

  }
  const httpRequestUtil = new HttpRequestUtil()
  const res = await httpRequestUtil.get('http://golderbrother.cn/')
  console.log(res)
})()
```

#### 创建对象的方法

1. 字面量
2. new Object
3. 工厂函数：不能根据原型对象判断对象类型

```js
function createPerson(name) {
  var o = new Person()
  o.name = name
  return o
}
var person = createPerson('Lqf')
```

4. 构造函数：如果内含方法，每次调用都会产生，浪费内存

```js
function Person(name) {
	this.name = name
	this.intr = function(){}
}
var person = new Person('Lqf')
```

5. 原型对象方式：步骤繁琐

```js
function Person(){}
Person.prototype.name = 'hello'
Person.prototype.intr = function(){}
var person = new Person()
person.name = 'Lqf' // 覆盖父对象
```

6. 混合模式：不符合面向对象封装思想

```js
function Person(name) {
	this.name = name
}
Person.prototype.intr = function(){}
var person = new Person('Lqf')
```

7. 动态混合：语义不符

```js
function Person(name) {
	this.name = name
	// 初次调用才会生成，不会产生浪费
	if (Person.prototype.intr === undefined) {
		Person.prototype.intr = function(){}
	}
}
var person = new Person('Lqf')
```

8. 寄生构造函数：可读性差

```js
function Person(name) {
	this.name = name
	// 初次调用才会生成，不会产生浪费
	if (Person.prototype.intr === undefined) {
		Person.prototype.intr = function(){}
	}
}

function Student(name, age) {
	var person = new Person(name)
	person.age = age
	return person
}

var student = new Student('Lqf', 18)
```

9. ES6 Class：本质上和混合模式类似

```js
class Person {
	constructor(name) {
		this.name = name
	}
	intr() {}
}
```

10. 稳妥构造函数：容易内存泄漏

```js
function Person(name) {
	var person = {}
	person.getName = function() { return name }
	person.setName = function(val) { name = val }
	return person

	// 这种方式不用加 new 调用
	this.getName = function() { return name }
	this.setName = function(val) { name = val }
}
var person = new Person('Lqf')
```

#### 继承的方法

0. 父类方法

```js
function Person(name) {
	this.name = name
	this.intr = function(){}
}
Person.prototype.eat = function(){}
```

1. 原型链继承：父类实例变成子类的原型（无法向父类传参）

```js
function Student(){}
Student.prototype = new Person()
Student.prototype.name = 'Lqf'
var student = new Student()
```

2. 构造函数继承

```js
function Student(name, age) {
	Person.call(this, name)
	this.age = age
}
var student = new Student('Lqf', 18)
```

3. 实例继承

```js
function Student(name, age) {
	var person = new Person(name)
	person.age = age
	return person
}
var student = new Student('Lqf', 18)
```

4. 拷贝继承：无法获取父类不可 for in 的方法

```js
function Student(name, age) {
	var person = new Person(name)
	for(var p in person) {
		Student.prototype[p] = person[p]
	}
	this.age = age
}
var student = new Student()
```

5. 组合继承

```js
function Student(name, age) {
	Person.call(this, name)
	this.age = age
}
Student.prototype = new Animal()
Student.prototype.constructor = Student
var student = new Student()
```

6. 寄生组合继承

```js
function Student(name, age) {
	Person.call(this, name)
	this.age = age
}
(function () { // 创建一个没有实例方法的类
	var Super = function(){}
	Super.prototype = Person.prototype  // 将实例作为子类的原型 Cat.prototype = new Super();
	Student.prototype = new Super
})()
var student = new Student()
```

7. ES6 Class extends

```js
class Person() {
	constructor(){}
}
class Student extends Person() {
	constructor() {
		super()
	}
}
```
