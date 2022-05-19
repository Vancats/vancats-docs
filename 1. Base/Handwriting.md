---
date created: 2022-05-06 16:54
date updated: 2022-05-19 19:38
---

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
