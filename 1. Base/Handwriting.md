---
date created: 2022-05-06 16:54
date updated: 2022-05-19 20:06
---

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
t
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
