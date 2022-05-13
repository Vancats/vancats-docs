---
date created: 2022-05-06 16:54
date updated: 2022-05-13 14:27
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

原始值，数组，对象，循环引用，Set，Map，Symbol，Date，RegExp

```js
const target = {
  field1: 1,
  field2: undefined,
  field3: {
    child: 'child'
  },
  field4: [2, 4, 8],
  empty: null,
  map: new Map().set(1, 'map'),
  set: new Set().add('set'),
  bool: Boolean(true),
  num: new Number(1),
  str: new String(1),
  date: new Date(),
  objSymbol: Object(Symbol('2')),
  [Symbol('key')]: 'symbol',
  reg: /\d+de$/gm,
  error: new Error('error'), symbol: Symbol('1'),
}
target.target = target
  
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
  
const cloneOtherType = function (target, type) {
  const Ctor = target.constructor
  switch (type) {
    case boolTag:
    case numberTag:
    case stringTag:
    case dateTag:
    case errorTag:
      return new Ctor(target)
    case regexpTag:
      return new Ctor(target.source, target.flags)
    case symbolTag:
      return Object(Symbol(target.description))
  }
}

function deepClone(target, map = new Map()) {
  if (typeof target === 'symbol')
    return Symbol(target.description)
  if (!target || typeof target !== 'object')
    return target
  if (map.has(target))
    return map.get(target)
  
  const type = Object.prototype.toString.call(target)
  let res
  if (deepTag.includes(type)) {
    res = new target.constructor()
  } else {
    return cloneOtherType(target, type)
  }
  map.set(target, res)
  if (type === setTag) {
    target.forEach(value => res.add(deepClone(value)))
    return res
  }
  if (type === mapTag) {
    target.forEach((value, key) => res.set(key, deepClone(value)))
    return res
  }
  Reflect.ownKeys(target).forEach(key => {
    res[key] = deepClone(target[key], map)
  })
  return res
}
```

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

#### 发布 - 订阅模式

```ts
class EventCenter {
  constructor() {
    this.handles = []
  }

  on(type, handle) {
    if (!this.handles[type])
      this.handles[type] = []
    this.handles[type].push(handle)
  }

  trigger(type, params) {
    if (!this.handles[type])
      return new Error('不存在该方法')
    this.handles[type].forEach(handle => {
      handle(...params)
    })
  }

  remove(type, handle) {
    if (!this.handles[type])
      return new Error('不存在该方法')
    if (!handle) {
      delete this.handles[type]
    } else {
      const ind = this.handle[type].findIndex(item => item === handle)
      if (ind < 0)
        return new Error('不存在该方法')
      this.handles[type].splice(index, 1)
      if (this.handles[type].length === 0)
        delete this.handles[type]
    }
  }
}
```

#### 双向绑定

```ts
let obj = {}
let input = document.querySelector('#input')
let span = document.querySelector('#span')

Object.defineProperty(obj, 'text', {
  get() {
    console.log('获取数据')
  },
  set(newVal) {
    console.log('更新数据')
    input.value = newVal
    span.innerHTML = newVal
  }
})

input.addEventListener('keyup', function (e) {
  obj.text = e.target.value
})
```

#### 简单路由

```ts
class Route {
  constructor() {
    this.routes = {}
    this.currentHash = ''
    this.freshRoute = this.freshRoute.bind(this)
    window.addEventListener('load', this.freshRoute, false)
    window.addEventListener('hashchange', this.freshRoute, false)
  }

  storeRoute(path, cb) {
    this.routes[path] = cb || function () { }
  }

  freshRoute() {
    this.currentHash = location.hash.slice(1) || '/'
    this.routes[this.currentHash]()
  }
}
```

#### 使用 setTimeout 实现 setInterval

```ts
function mySetInterval(fn, timeout) {
  let timer = {
    flag: true
  }

  function interval() {
    if (timer.flag) {
      fn()
      setTimeout(interval, timeout)
    }
  }

  setTimeout(interval, timeout)
  return timer
}
```

#### 封装 fetch

```
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
