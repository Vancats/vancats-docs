---
date created: 2022-03-03 00:28
date updated: 2022-03-03 23:08
---

### 之前

###### 判断 this

1. obj.fun  this -> obj
2. new 构造函数 this -> 新创建的对象
3. 普通函数，匿名函数，回调函数 this -> window
4. DOM 事件处理函数  this -> DOM 对象  这里不可更改为箭头函数，否则 this 指向 window
5. Vue 中的 this 指向当前实例对象
6. 箭头函数中的 this 指向当前函数外最近作用域的 this
		1. 所有匿名函数都可以使用箭头函数简化
		2. 对象中的方法不可改为箭头函数，可使用 `fun() {}` 或 `fun: function(){}`
		3. 箭头函数的底层原理即为 bind，所以 call 无法更新箭头函数的指向
7. bind 永久更改 this 指向，call，apply 临时更改 this 指向

###### JS 创建对象的方法

1. 字面量
2. new Object
3. 工厂函数：无法根据原型对象判断对象的类型

```js
function createPerson(name, age) {
	var o = new Person()
	o.name = 'Lqf'
	o.age = 18
	return o
}
var person = createPerson('Lqf', 18)
```

4. 构造函数：如果内含方法，浪费内存

```js
function Person(name, age) {
	this.name = name
	this.age = age
	this.intr = function() {}
}
var person = new Person('Lqf', 18)
```

5. 原型对象方式：步骤繁琐

```js
function Person(){}
Person.prototype.nama = 'name'
Person.prototype.age = 'age'
Person.prototype.intr = function() {}
var person = new Person()
person.name = 'Lqf' // 将会在子对象中创建一个name覆盖父对象的值
```

6. 混合模式：不符合面向对象封装思想

```js
function Person(name, age) {
	this.name = name
	this.age = age
}
Person.prototype.intr = function() {}
var person = new Person('Lqf', 18)
```

7. 动态混合：语义不符

```js
function Person(name, age) {
	this.name = name
	this.age = age
	if (Person.prototype.intr === undefined) {
		Person.prototype.intr = function() {}
	}
}
var person = new Person('Lqf', 18)
```

8. 寄生构造函数：可读性差

```js
function Person(name, age) {
	this.name = name
	this.age = age
	if (Person.prototype.intr === undefined) {
		Person.prototype.intr = function() {}
	}
}

function Student(name, age, className) {
	var person = new Person(name, age)
	person.className = className
	return person
}
var student = new Student('Lqf', 18, '四班')

```

9. ES6 Class：集中保存一种类型所有子对象的属性和方法的程序结构，本质上和混合模式类似

```js
class Person {
	constructor(name, age) {
		this.name = name
		this.age = age
	}
	intr() {}
}
```

10. 稳妥构造函数：容易内存泄漏

```js
function Person(name, age) {
	var person = {}
	person.getName = function(){return name}
	person.setName = function(val){name = val}
	person.getAge = function(val) {}
}
```

###### JS 继承方式

1. 原型链继承：将父类的实例变成子类的原型 --- 创建子类时无法向父类传参

```js
function Person(name) {
	this.name = name
	this.intr = function() {}
}
Person.prototype.eat = funtion(food) {}

function Student() {}
Student.prototype = new Person()
Student.prototype.name = 'student'
var student = new Student()
```

2. 构造函数继承

```js
function Person(name) {
	this.name = name
	this.intr = function() {}
}
Person.prototype.eat = funtion(food) {}

function Student(name, age) {
	Person.call(this, name)
	this.age = age
}
var student = new Student()
```

3. 实例继承

```js
function Person(name) {
	this.name = name
	this.intr = function() {}
}
Person.prototype.eat = funtion(food) {}

function Student(name, age) {
	var person = new Person(name)
	person.age = age
	return person
}
var student = new Student()
```

4. 拷贝继承：无法获取父类不可 for in 的方法

```js
function Person(name) {
	this.name = name
	this.intr = function() {}
}
Person.prototype.eat = funtion(food) {}

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
function Person(name) {
	this.name = name
	this.intr = function() {}
}
Person.prototype.eat = funtion(food) {}

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
function Person(name) {
	this.name = name
	this.intr = function() {}
}
Person.prototype.eat = funtion(food) {}

function Student(name,age) {
	Person.call(this, name)
	this.age = age
}
 
(function(){ // 创建一个没有实例方法的类
	var Super = function(){};  
	Super.prototype = Person.prototype; // 将实例作为子类的原型 Cat.prototype = new Super();
	Student.prototype = new Super()
})()
var student = new Student()
```

7. ES6 Class extends

```js
class Person() {
	constructor() {
	}
}

class Student extends Person () {
	constructor() {
		super()
	}
}
```

### 数据类型

###### 类型判断

1. typeof：可以判断函数
2. 原型链
		1. `[].__proto__ === Array.prototype`
		2. `Object.getPrototype([]} === Array.prototype`
		3. `Array.prototype.isPrototypeOf([])`
3. 构造函数方式
		1. `[].constructor === Array`
		2. `[] instanceof Array`

**原型链的更换会引起上面两点判断的出错** `[].__proto__ = Object.prototype`
4. `Object.prototype.toString.call([]) === '[object Array]'`
5. `Array.isArray([])`
6. 手写 ==instanceof==

```js
function myInstanceOf (left, right) {
	let proto = Object.getPrototypeOf(left)
	let prototype = right.prototype
	while (true) {
		if (!proto) return false
		if (proto === prototype) return true
		proto = Object.getPrototypeOf(proto)
	}
}
```

###### 类型转换

**== 强制类型转换**

```js
'1' == true   ➡️   '1'  ==  1   ➡️   1  ==  1

		boolean->number   string->number

如果有一边是不是基本类型，转基本类型
```

**to String**

```js
null        -> 'null'
undefined   -> 'undefined'
Symbol('a') -> 'Symbol(a)' 隐式转换报错
Object      -> 调用 toString 方法
```

**to Number**

```js
null          -> 0
undefined     -> NaN
Symbol('a')   -> 报错
Object(Array) -> ToPrimitive 操作：首先通过内部操作 DefaultValue 检查是否有 valueOf，如果有并且返回的是基本值，则强制类型转换，不然就使用 toString 强制类型转换。⚠️ 只有两个方法返回的都不是基本值时，会报错！其他最多是 NaN
```

**to Boolean**

> `undefined、null、false、(+-)0、NaN、""`

**包装类型**

> `Object('abc').valueOf() === 'abc'`

==隐式转换==
JS 中的每个值都带有 ToPrimitive 方法，主要功能为将值转换为基本类型

```js
当需要转换成 number 时（大多数）
	1. 调用 valueOf 方法，如果为原始值，则返回，不然下一步
	2. 调用 toString 方法，同上
	3. 抛出 TypeError
当需要转换成 string 时，第一二步调换位置，先 toString（只有 Date 对象是这样的）

基本类型转换
+
1 + false    -> 1
1 + Symbol() -> 报错
'1' + false  -> '1false'

> <
两边字符串：比较字母表顺序
其他情况：转换为数字再比较

对象情况
eg1
var a = {}
a > 2 // false

eg2
var a = { name: 'Jake' }
var b = { name: 'Rose' }
a + b // '[object Object][object Object]'
```

###### 其他情况

1. typeof null

```js
在 JS 的第一个版本中，所有值存储在一个 32 位单元中，每个单元包含一个类型标签（1～3bit）以及真实数据，类型标签存储在低位中
000: object
  1: int 31 位有符号整数
010: double 双精度浮点数
100: string
110: boolean
null: 32 位全是 0
undefined: (-2)30 超出整数范围的数字
```

2. 0.1 + 0.2 !== 0.3：Number.EPSILON
3. isNaN：任何不能转换为数值的值都会返回true；Number.isNaN：先判断是否为数字再判断是否为NaN

### JS 基础

###### new 一个构造函数

1. 创建一个新的对象
2. 将新对象的 `__proto__` 指向构造函数的 prototype
3. 构造函数中的 this 指向该新对象，通过强制赋值，为新对象添加属性和方法
4. 返回该新对象的地址
5. 如果构造函数 return 了一个引用类型对象，直接返回该对象，前面的操作无效
6. 手写==new==

```js
function myNew () {
	let newObj = null
	let constructor = Array.prototype.slice.call(arguments)
	if (arguments !== 'function') {
		new TypeError('type error')
		return
	}
	newObj = Object.create(constructor.prototype)
	let result = constructor.apply(newObj, arguments)
	if (result && (typeof result === 'object' || typeof result === 'function')) {
		return result
	}
	return newObj
}
```

###### 箭头函数

1. 没有自己的 this，直接继承作用域上一层上下文，并且不会改变
2. 没有 prototype
3. 不能作为构造函数，new 一个函数中第二点需要 prototype，第三点需要更改 this
4. 不能用 call，apply，bind 改变 this 指向（箭头函数底层使用 apply）
5. 没有自己的 arguments，用 ...rest 代替，箭头函数内部访问 arguments 实际上返回的是外层函数的 arguments
6. 不能用作 generator 函数，不能使用 yield 关键字
7. 函数体只有一句话，并且不要返回值，使用 void `let fn = () => void fun()`
8. 对象的方法，原型对象的方法和DOM事件函数不能用

###### for...of & iterator

```js
let obj = { a: 1, b: 2 }

// 实现 iterator 的方法
// eg1
obj[Symbol.iterator] = function() {
	let keys = Object.keys(this)
	let count = 0
	return {
		next() {
			if (count < keys.length) {
				return { value: obj[keys[count++]], done: false }
			} else {
				return { value: undefined, done: true }
			}
		}
	}
}

// eg2
obj[Symbol.iterator] = function*() {
	let keys = Object.keys(this)
	for(let k of keys) {
		yield obj[k]
	}
}
```

###### 数组的原生方法

```js
at concat copyWithin entries every fill filter find findIndex findLastIndex flat flatMap forEach
includes indexOf join keys lastIndexOf map pop push reduce reduceRight reverse shift slice some sort splice
toLocaleString toString unshift values

Array: from isArray of
```

###### 对象的原生方法

```js
hasOwnProperty isPrototypeOf propertyEnumerable toLocaleString toString valueOf

String: assign create defineProperties defineProperty entries freeze fromEntries getOwnPropertyDescriptor
getOwnPropertyDescriptors getOwnPropertyName getOwnPropertySymbols getOwnPropertyOf hasOwn is isExtensible
isFrozen isSealed keys preventExtensions seal setPropertyOf values
```

###### 字符串的原生方法

```js
anchor at big blink bold charAt charCodeAt codePointAt concat endsWith fixed fontcolor fontsize includes indexOf
italics lastIndexOf link localeCompare match matchAll normalize padEnd padStart repeat replace replaceAll search
slice small split startsWith strike sub substr substring sup toLocaleLowerCase toLocaleUpperCase toLowerCase
toString toUpperCase trim trimEnd trimLeft trimRight trimStart valueOf

String: fromCharCode fromCodePoint raw
```

###### ES6 和 CommonJS

1. 前者的 import 异步加载，后者的 require 同步加载，只适用于本地读取文件的服务端
2. 前者输出的值的引用，而后者输出的是值的浅拷贝内容
3. 前者是编译时输出接口，因此可以静态分析，tree shaking 和 scope hoisting
4. 前者遇到循环模块时，因为其变量值不缓存，可以取到最终的值，后者只会输出已经执行的部分，后续的变化不影响前面的值
5. 前者 this 指向 undefined，后者 this 指向本模块
6. 前者支持引入后者（必须整体引入，不能引入单独一个变量）；后者引入前者，要求后者使用 .mjs 后缀，或者在 package.json 中加入 { "type": "module" }，而且 mjs 文件必须和 import 对应，不能使用 require

###### AJAX

```js
const url = '/api/user'
let xhr = new XMLHttpRequest()
xhr.open('get', url, true) // 默认 true 异步

// 设置状态监听函数
xhr.onreadystatechange = function() {
	// 0: 已代理，未发 open; 1: open 2: send 3: 下载中 4: 完成
	if (this.readystate !== '4') return
	if (this.status === 200) {
		handle(this.response)
	} else {
		console.error(this.statusText)
	}
}

xhr.onerror = function() {
	console.error(this.statusText)
}

xhr.responseType = 'json'
xhr.setRequestHeader('Accept', 'application/json')

// 也可以带 data
xhr.send(null)

// 使用 Promise 封装 AJAX
function getJSON() {
	let promise = new Promise(function (resolve, reject) {
		let xhr = new XMLHttpRequest()
		xhr.open('get', url)
		xhr.onreadystatechange = function() {
			if (this.readystate !== '4') return
			if (this.status === 200) {
				resolve(this.response)
			} else {
				reject(new Error(this.statusText))
			}
		}

		xhr.onerror = function() {
			reject(new Error(this.statusText))
		}

		xhr.responseType = 'json'
		xhr.setRequestHeader('Accept', 'application/json')

		xhr.send(null)
	})
	return promise
}
```

**Fetch**
基于 Promise 设计，使用原生 js，而不是 ajax 的进一步的封装，没有使用 XMLHttpRequest
优点：语法简洁，基于 Promise，支持 async/await
缺点：

1. 只对网络请求报错，对 400，500 都当作成功请求
2. fetch 默认不发 cookie，需要配置项 fetch( url, { credentials: 'include' })
3. fetch 不支持 abort，不支持超时控制，使用 setTimeout 与 Promise.reject 实现的超时控制不能阻止请求过程继续在后台运行
4. fetch 没有办法原生监测的请求的进度

**Axios**
基于 Promise 封装

1. 浏览器端发起 XMLHttpRequest 请求，node 端发起 http 请求
2. 支持 Promise API
3. 监听请求和返回，对请求和返回进行转化
4. 可以取消请求
5. 自动转换 json，fetch 需要 res.json()
6. 客户端支持抵御 XSRF 攻击

###### Map

1. 意外的键：新建的 map 不包含任何键，object 中有原型，新键可能和原型的冲突
2. 键值：map 的键可以是任意内容，object 的键必须是 string 或者 symbol
3. 键的顺序：map 的键的顺序严格按照插入顺序，object 无序
4. Size：map 默认有 size 属性，object 只能遍历得到数量
5. 迭代：map 是 iterator 的，可以直接迭代，object 使用 Object.keys
6. 性能：map 在频繁增删键值对是性能好，object 无优化

WeakMap 的键必须是引用类型，引用的对象都是弱引用，不计入垃圾回收机制，一旦不需要会自动消失

###### JSON

1. JSON.parse：JSON -> JS 如果不合规范，报错
2. JSON.stringify

```js
JSON.stringify(value[, replacer[, space]])

replacer
function: 每个属性都会经过该函数的处理
array: 只有包含在这个数组中的才会被序列化

space: 指定缩进的空白符
数字是几就是有多少空格，上限为 10，小于 1 没有空格
如果是字符串，字符串被当作空格，取前十个

特性：
1. null、undefined、Symbol 出现在非数组对象中直接忽略，在数组中返回 null，被 replacer 单独转换时返回 undefined
2. 基础值包装对象自动转换成原始值 new Number(1) -> 1
3. 任何以 Symbol 为键值的都会被忽略
4. NaN、Infinity、null 都转换成 null
5. 如果值有 toJSON 方法，直接返回 return 值
6. Date 自动调用了 toJSON，返回时间字符串
7. 其他类型的对象，如 Map、WeakMap、Set、WeakSet，仅序列化可枚举属性
8. 包含循环引用或 BigInt，抛出错误
```

###### Unicode 与编码方式

**Unicode**：为每种语言的每个字符设定了统一并且唯一的二进制编码，实现方式（编码方式）有 UTF-8/16/32

**UTF-8**

1. 对于单子节字符，首位为0，其余 7 位为这个字符的 unicode 编码，因此英文字母的 ASCII 和 Unicode 一样
2. 对于 n 字节字符，第一个字节的前 n 位都是 1，第 n + 1 位是 0，其他字节前两位都是 10

| 编码范围（编号对应的十进制数）             | 二进制格式                               |
| :-------------------------- | ----------------------------------- |
| 0x00—0x7F （0-127）           | 0xxxxxxx                            |
| 0x80—0x7FF （128-2047）       | 110xxxxx 10xxxxxx                   |
| 0x800—0xFFFF  （2048-65535）  | 1110xxxx 10xxxxxx 10xxxxxx          |
| 0x10000—0x10FFFF  （65536以上） | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |

编码规则

1. 找到 Unicode 所在编码范围，进而找到相应的二进制格式
2. 将 Unicode 编码转换成二进制（去掉最高位的 0）
3. 将二进制数从右到左一次填入二进制格式的 x 中，如果有 x 没填，设置为 0

eg
“马”字的 Unicode 编码是 0x9A6C，整数编号是 39532

1. 该字在第三个范围内，二进制格式是 1110xxxx 10xxxxxx 10xxxxxx
2. 39532 的二进制编码是 1001 1010 0110 1100
3. 因此是 11101001 10101001 10101100

**UTF-16**
以 "**𡠀**" 字为例，它的 Unicode 码点为 0x21800，该码点超出了基本平面的范围，因此需要用四个字节来表示，步骤如下：

- 首先计算超出部分的结果：0x21800 - 0x10000
- 将上面的计算结果转为 20 位的二进制数，不足20位就在前面补0，结果为：0001000110 0000000000
- 将得到的两个 10 位二进制数分别对应到两个区间中
- U+D800 对应的二进制数为 1101100000000000，将 0001000110 填充在它的后 10  个二进制位，得到 1101100001000110，转成 16 进制数为 0xD846。同理，低位为 0xDC00，所以这个字的 UTF-16 编码为 0xD846 0xDC00

**总结**

- UTF-16 使用变长码元序列的编码方式，相较于定长码元序列的 UTF-32 算法更复杂，甚至比同样是变长码元序列的 UTF-8 也更为复杂，因为其引入了独特的**代理对**这样的代理机制；
- UTF-8 需要判断每个字节中的开头标志信息，所以如果某个字节在传送过程中出错了，就会导致后面的字节也会解析出错；而 UTF-16 不会判断开头标志，即使错也只会错一个字符，所以容错能力教强；
- 如果字符内容全部英文或英文与其他文字混合，但英文占绝大部分，那么用`UTF-8`就比 UTF-16 节省了很多空间；而如果字符内容全部是中文这样类似的字符或者混合字符中中文占绝大多数，那么 UTF-16 就占优势了，可以节省很多空间；

encodeURI：对整个 URI 进行转义，但是一些对于 URI 有特殊意义的字符不转义
encodeURIComponent：对 URI 的组成部分转义，包括特殊字符
escape：和 encodeURI 一样，但是Unicode 大于 0xff 字符，直接前面加 %u，而 encodeURI 转成 UTF-8 后前面加 %

### 原型与闭包

###### 对原型链的理解

**原型**：JS 中对象是通过构造函数构建，每个构造函数都拥有一个 `prototype`，属性值是一个对象，该对象包含了可以由这个构造函数的所有实例共享的属性和方法。构造函数创建的对象中，会有一个指针（`__proto__`）直接指向该 `prototype`，这个指针被称为原型。ES5中新建了一个 `Object.getPrototypeOf()` 来获取这个属性。另外还会有一个 `constructor` 属性指向构造函数。

**原型链**：当访问对象属性的时候，如果本身没有，就会顺着原型往上找，直到 `Object.prototype` 为止

**原型方法**：构造函数的方法如果放在函数中，每次调用都会重新创建方法，需要放到 `prototype` 上

**原型重写**

```js
function Person(name){ this.name = name }
Person.prototype = { getName(){} }
var p = new Person('hello')
p.__proto__ === Person.prototype // true
p.__proto__ === p.constructor.prototype // false
p.__proto__ === Person.prototype // { getname(){} }
p.constructor // Object 如果没有改写的话是 Person
p.constructor.prototype // Object.prototype

// 正常情况下
p.__proto__ // Person.prototype
Person.prototype.__proto__ // Object.prototype
p.__proto__.__proto__ // Object.prototype
p.__proto__.constructor.prototype.__proto__ // Object.prototype
Person.prototype.constructor.prototype.__proto__ // Object.prototype
p1.__proto__.constructor // Person
Person.prototype.constructor // Person
```

![image](https://cdn.nlark.com/yuque/0/2021/png/1500604/1615475711487-c474af95-b5e0-4778-a90b-9484208d724d.png)

###### 作用域链 / 闭包

**闭包**：有权访问另一个函数作用域中变量的函数。用途：1. 从外部访问内部变量 2. 保留对象的引用防止回收

**作用域链作用**：保证对执行环境有权访问的所有变量和函数的有序访问

###### 执行上下文

- 全局执行上下文，函数执行上下文，eval 执行上下文；有执行上下文栈（调用栈）
- 函数上下文除了变量定义、函数声明，还有 this，arguments

**创建执行上下文**：

1. 创建阶段

- this 绑定
- 创建词法环境组件
	- 是一种有 标识符-变量 映射的数据结构
	- 内部有两个组件：1. 环境记录器：储存变量/函数的具体位置 2. 外部环境的引用：访问父级作用域
- 创建变量环境组件：也是一个词法环境，环境记录器存储变量的绑定关系

2. 执行阶段：完成对变量的分配，最后执行代码

###### 垃圾回收机制
JS 具有自动回收机制，找到不再使用的变量释放内存

**标记清除**
- 变量进入执行环境时，会被标记“进入环境”，当“离开环境”时，内存被释放
- 垃圾收集器运行时给内存中所有变量打上标记，然后去掉环境中的变量的标记，ci hou

**引用计数**