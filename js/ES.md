---
date created: 2022-03-03 00:35
---

### 数组去重

1. Set去重: Arrary.from(new Set(arr))；[…new Set(arr)]

2. for+for+splice：if(arr[i]==arr[j]){ arr.splice(j,1); j--;}

3. 新建空数组加indexOf：if (array .indexOf(arr[i]) === -1) { array .push(arr[i])}

4. sort加相邻元素对比是否相等

5. 新建数组加includes，类似于indexOf

6. filter：arr.filter(function(item, index, arr) {return arr.indexOf(item, 0) === index;

7. hasOwnProperty

8. 递归去重

9. Map去重

10. reduce+includes：arr.reduce((prev,cur) => prev.includes(cur) ? prev : [...prev,cur],[])

### Map（parseInt）

parseInt(string[, radix])    array.map(function callback(currentValue[, index[, array]]){}[, thisArg]) arr.map(parseInt)

### JSONP优缺点

**优点**

- 跨越同源策略
- 兼容性好
- 可以调用 callback 回传结果

**缺点**

- 只支持 GET
- 只支持跨域 HTTP 请求，不能解决不同域页面间 JS 的调用
- 调用失败不会返回状态码
- 安全性不高

### ES5 和 ES6 继承区别

ES5的继承时通过prototype或构造函数机制来实现。ES5的继承实质上是先创建子类的实例对象，然后再将父类的方法添加到this上（Parent.apply(this)）。

ES6的继承机制完全不同，实质上是先创建父类的实例对象this（所以必须先调用父类的super()方法），然后再用子类的构造函数修改this。

具体的：ES6通过class关键字定义类，里面有构造方法，类之间通过extends关键字实现继承。子类必须在constructor方法中调用super方法，否则新建实例报错。因为子类没有自己的this对象，而是继承了父类的this对象，然后对其进行加工。如果不调用super方法，子类得不到this对象。

ps：super关键字指代父类的实例，即父类的this对象。在子类构造函数中，调用super后，才可使用this关键字，否则报错。

### ES6 class 特性

所有方法不可枚举

内部严格模式，也无法重写类名

必须用new调用（内部方法不能用new）

声明提升但是不赋值，有暂时性死区

