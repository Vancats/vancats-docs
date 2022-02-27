#### 之前
###### 名称含义
- 函数作用域(Actived Object) js引擎在调用函数时创建一个作用域对象,保存函数的局部变量,函数调用完就释放
- JS 中一切皆关联数组
- 闭包形成: 外层的函数调用之后,外层函数的**作用域对象**,被内层函数的**作用域链**引用无法释放

构造函数中含有方法，那么每一次调用时都会重新创建方法，浪费内存，需要把方法放入原型对象中
- 原型对象：替所有子对象集中保存属性值和方法的父对象
- 原型对象只能强行赋值：Student.prototype.fun = fun(){}

数字下标 for of
字符串下标 对象 for in

###### 面向对象
1. 封装
	1. 只创建一个对象：{}
	2. 创建多个重复对象，使用构造函数
2. 继承：所有子对象共用的属性和方法，会放在构造函数的原型对象中
3. 多态：重写父对象继承来的成员

######  判断 this
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



#### 数据类型
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
>`undefined、null、false、(+-)0、NaN、""`

**包装类型**
>`Object('abc').valueOf() === 'abc'`

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

#### JS 基础
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

###### 数组的原生方法


###### Map
1. 意外的键：新建的 map 不包含任何键，object 中有原型，新键可能和原型的冲突
2. 键值：map 的键可以是任意内容，object 的键必须是 string 或者 symbol
3. 键的顺序：map 的键的顺序严格按照插入顺序，object 无序
4. Size：map 默认有 size 属性，object 只能遍历得到数量
5. 迭代：map 是 iterator 的，可以直接迭代，object 使用 Object.keys
6. 性能：map 在频繁增删键值对是性能好，object 无优化

WeakMap 的键必须是引用类型，引用的对象都是弱引用，不计入垃圾回收机制，一旦不需要会自动消失

###### ~~JSON~~
1. JSON.parse：JSON -> JS 如果不合规范，报错
2. JSON.stringify：JS -> JSON 如果不合规范，会对值特殊处理

###### unicode 与编码方式
Unicode：为每种语言的每个字符设定了统一并且唯一的二进制编码，实现方式（编码方式）有 UTF-8/16/32

UTF-8
1. 对于单子节字符，首位为0，其余 7 位为这个字符的 unicode 编码，因此英文字母的 ASCII 和 Unicode 一样
2. 对于 n 字节字符，第一个字节的前 n 位都是 1，第 n + 1 位是 0，其他字节前两位都是 10

｜ 

| 编码范围（编号对应的十进制数）  | 二进制格式                          |
| ------------------------------- | ----------------------------------- |
| 0x00—0x7F （0-127）             | 0xxxxxxx                            |
| 0x80—0x7FF （128-2047）         | 110xxxxx 10xxxxxx                   |
| 0x800—0xFFFF  （2048-65535）    | 1110xxxx 10xxxxxx 10xxxxxx          |
| 0x10000—0x10FFFF  （65536以上） | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |
