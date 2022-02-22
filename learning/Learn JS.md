###### 名称含义
- 函数作用域(Actived Object) js引擎在调用函数时创建一个作用域对象,保存函数的局部变量,函数调用完就释放
- JS 中一切皆关联数组
- 闭包形成: 外层的函数调用之后,外层函数的**作用域对象**,被内层函数的**作用域链**引用无法释放

构造函数中含有方法，那么每一次调用时都会重新创建方法，浪费内存，需要把方法放入原型对象中
- 原型对象：替所有子对象集中保存属性值和方法的父对象
- 原型对象只能强行赋值：Student.prototype.fun = fun(){}

数字下标 for of
字符串下标 对象 for in

###### 箭头函数缺点
1. undefined、构造函数不能用
2. 对象方法不能用
3. 原型对象不能用
4. DOM 事件处理函数不能用
5. 无法使用 bind，apply，call
6. 不支持 arguments，使用 ...rest
7. 箭头函数没有 prototype

###### new 一个构造函数
1. 创建一个新对象
2. 让子对象继承构造函数的原型对象
3. 将构造函数中的 this 强行指向刚创建的对象，通过强行赋值的方式，为新对象添加属性和方法
4. 返回新对象的地址
5. 如果构造函数 return 了一个引用类型对象，则返回该对象，前面的操作失效 

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

###### 深拷贝*
1. `JSON.parse(JSON.stringify(obj1))`：无法深拷贝 undefined 以及内嵌函数
2. 自定义
```js
```




###### 判断类型
1. typeof：可以判断 function，无法判断 object 类型，如 Date，Array，null
2. 原型链方式
	1. `[1, 2].__proto__ === Array.prototype`
	2. `Object.getPrototypeOf([1, 2]) === Array.prototype`
	3. `Array.prototype.isPrototypeOf([1, 2])`
3. 构造函数方式：父级原型对象的 constructor
	1. `([1, 2]).constructor === Object`
	2. `[1, 2] instanceof Array`
**如果原型链被更改，则2，3点无法判断** `[1, 2].__proto__ = Object.protoype`
4. `Object.prototype.toString.call([1, 2]) === [object Array]` 最准确的方法
5. `Array.isArray([1, 2])`

###### 链接
[[JS 题目]]

