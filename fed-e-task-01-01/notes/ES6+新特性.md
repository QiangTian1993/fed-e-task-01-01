# ES6+ 新特性

## 模板字符串

```js
var n = "World";

console.log(`Hello ${n}`); // Hello World
```

## 带标签的模板字符串

```js
const name = "IAN";
const gender = true;

const strTag = function (strings, name, gender) {
	const sex = gender ? "man" : "woman";
	return strings[0] + name + strings[1] + sex + strings[2];
};

const result = strTag`hey, ${name} is a ${gender}`;

console.log(result); // hey, IAN is a man
```

## 解构赋值

在以前为变量赋值，只能一个个的指定

```js
let a = 1;
let b = 2;
let c = 3;
```

在 ES6 中可以这么写

```js
let [a, b, c] = [1, 2, 3];
a; // 1
b; // 2
c; // 3
```

对象也可以，区别在与对象是按照变量名来结构

```js
let obj = {
	name: "张三",
	age: "18",
	city: "北京",
};
let { name, age, city } = obj;
name; // '张三'
age; // 18
city; // '北京'
```

这里要注意，因为对象是通过变量名进行结构，如果全局作用域中已经有相同的常量结构就会报错

```js
const name = "IAN";
const { name } = obj; // 报错
```

这时候就需要进行重命名

```js
const name = "IAN";
const { name: objName } = obj;
```

::: tip
结构赋值中还可以指定默认值

```js
const [, , c = 0] = [1, 2, 3];
const { name: objName = "IAN" } = obj;
```

:::

### 解构赋值的的用途

#### 1. 变换变量的值

```js
let a = "a";
let b = ("b"[(a, b)] = ["b", "a"]);
a; // 'b'
b; // 'a'
```

#### 2. 函数的传参

```js
let obj = {
	name: "张三",
	age: "18",
	city: "北京",
};

function fn({ name, age, city }) {
	console.log(name);
	console.log(age);
	console.log(city);
}
fn(obj);

// 张三
// 18
// 北京
```

#### 3. 提取 JSON 数据

```js
let jsonData = {
	id: 42,
	status: "OK",
	data: [867, 5309],
};

let { id, status, data: number } = jsonData;
```

## let

 声明语句，与 `var` 不同的是 `let` 不会声明提升而且有自己的作用域，不能重复定义同一个变量

举个例子：

```js
var elements = [{}, {}, {}];
for (var i = 0; i < elements.length; i++) {
	elements[i].onclick = function () {
		console.log(i);
	};
}

elements[2].onclick(); // 3
```

这里为什么会打印出 3 呢？
这是因为我们在调用 `elements[0]` 的 `onclick` 方法时循环已经结束，内存中的 i 已经等于 3 了，所以打印出来的是 3
那在以前呢如果想实现你想想的那样，就得使用闭包如下：

```js
var elements = [{}, {}, {}];
for (var i = 0; i < elements.length; i++) {
	elements[i].onclick = (function (i) {
		return function () {
			console.log(i);
		};
	})(i);
}

elements[2].onclick(); // 2
```

现在呢，因为 `let` 有自己的作用域，所以就不用这么麻烦了

```js
var elements = [{}, {}, {}];
for (let i = 0; i < elements.length; i++) {
	elements[i].onclick = function () {
		console.log(i);
	};
}

elements[2].onclick(); // 2
```

再看一个例子

```js
for (var i = 0; i < 3; i++) {
	for (var i = 0; i < 3; i++) {
		console.log(i);
	}
}

// 0
// 1
// 2
```

这是为什么呢，不是应该打印两次 1 2 3 吗？
这就是因为 `var` 没有自己作用域，在内层循环结束后 i 就等于 3 了，因为 3 不满足 外层循环的条件，所以只会打印一次 1 2 3

## const

`const` 与 `let` 差不多，不同之处在于 `const` 用来声明常量。用 `const` 声明的变量不可修改，他是 `只读` 的

## 剩余参数

```js
function foo(,...args){
	console.log(args)
}

foo(1,2,3,4) // [2, 3, 4]
```

## 箭头函数

```js
let a = (arguments1, arguments2) => {
	console.log(1);
	return arguments1 + arguments2;
};
a(1, 2);
// 1
// 3
```

如果只有一个参数可以不写括弧

```js
let a = (arguments) => {
	console.log(1);
	return arguments;
};
a(2);
// 1
// 2
```

如果函数体只有一句话可以不写{}

```js
let a = (arguments1, arguments2) => b + c;
a(1, 2);
// 3
```

如果需要 return 的是一个对象可以用括号括起来

```js
let a = (name) => ({ name: name });
a("IAN"); // {name: "IAN"}
```

如果只有一个参数且函数体只有一句话

```js
let a = (arguments) => arguments * 2;
a(8); //16
```

普通的函数都会接收一个 this ，箭头函数中没有 this ，所以它不会改 this 的值

## spread 运算符

> 将 `对象` 展开

```js
let arr = [1, 2, 3, 4, 5];
console.log(...obj); // 1 2 3 4 5
```

### spread 的应用

#### 插入数组

```js
let arr1 = [1,2]
let arr2 = [..obj1,3,4,5]
obj2 // [1,2,3,4,5]
```

#### 将 String 转换为数组

```js
let string = "Hello World";
let arr = [...string];
arr; //["H", "e", "l", "l", "o", " ", "W", "o", "r", "l", "d"]
```

#### 合并对象

> 如果有同名的属性就覆盖

```js
let obj1 = {
	name: "张三",
	age: "18",
};
let obj2 = {
	name: "李四",
	age: "18",
	city: "北京",
};
let obj3 = {
	...obj2,
	...obj1,
};

obj3; //{name: "张三", age: "18", city: "北京"}
```

## 对象字面量

```js
const obj = {
	name: "IAN",
	age: 18,
	[Math.random()]: 1,
};
```

## Object.is

可以判断两个对象是否相等

```js
Object.is(NAN, NAN); //true
```

## class 类

> 在 ES5 中可以用构造函数模拟 class

```js
function Person(name, age) {
	this.name = name;
	this.age = age;
}

Person.prototype.getName = () => {
	console.log(this.name);
};

function Person2(name, age) {
	Person1.call(this, name, age);
}

let fn = function () {};
fn.prototype = Person.prototype;
Person2.prtotype = new fn();
```

### 创建 class 类

```js
class Person {
	constructor(name, age) {
		this.name = name;
		this.age = age;
	}
	getName() {
		// 类里面的方法等同于定义在原型上
		console.log(this.name);
	}
}
let xiaoming = new Person("小明", 18);
xiaoming; //Person {name: "小明", age: 18}
xiaoming.getName; // 小明
```

### 静态方法

在方法前加上 static 关键字 ，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
class Person {
	constructor(name, age) {
		this.name = name;
		this.age = age;
	}
	static classMethod() {
		// 类里面的方法等同于定义在原型上
		console.log("Hello");
	}
}

let xiaoming = new Person("小明", 18);

xiaoming.classMethod(); // TypeError: foo.classMethod is not a function
Person.classMethod(); // 'hello'
```

### extends 继承

> 创建一个 Person2 让他继承 Person 的属性和方法

```js
class Person2 extends Person {
	constructor(name, age, city) {
		super(name, age); //调用父类的constructor（）
		this.city = city;
	}

	hello() {
		super.getName(); // 调用父类里的 getName 方法
	}
}
let xiaowang = new Person2("小王", 19, "北京");
xiaowang.name; //'小王'
xiaowang.age; // 19
xiaowang.hello(); // '小王'
```

## Symbol

`Symbol` 是一种新的原始数据类型， 它表示一个独一无二的值

> 因为它是独一无二的所以可以为对象添加一个独一无二的属性

```js
Symbol() == Symbol(); // false

const NAME = Symbol();

const IAN = {
	[NAME]: "IAN",
	age: 18,
};

console.log(IAN[NAME]); // IAN
```

:::tip 注意：
通过 Symbol 添加的键名，无法通过遍历对对象、 Object.keys 方法等获取
如果需要获取 通过 Symbol 添加的键，可通过 Object.getOwnPropertySymbols 方法
因为这种 Symbol 的这种特性，所以它非常适合为对象添加私有属性
:::

```js
console.log(IAN[Object.getOwnPropertySymbols(IAN)[0]]); // IAN
```

## for ..of

`for ..of` 可以用来遍历具有 `iterator` 迭代器属性的对象的属性

```js
const arr = [1, 2, 3];

for (const item of arr) {
	console.log(item);
}
// 1,2,3
```

我们来看看 `iterator`是怎么样工作的

```js
const iterator = arr[Symbol.iterator]();

iterator.next(); // {value: 1, done: false}
iterator.next(); // {value: 2, done: false}
iterator.next(); // {value: 3, done: false}
iterator.next(); // {value: undefined, done: true}
```

:::tip
如果想用 for ..of 来遍历对象，可为对象添加一个 `iterator`
:::

```js
const obj = {
	name: "IAN",
	age: 18,
};

Object.defineProperty(obj, Symbol.iterator, {
	enumerable: false,
	writable: false,
	configurable: true,
	value: function () {
		const _this = this;
		let idx = 0;
		const keys = Object.keys(_this);

		return {
			next: function () {
				return {
					value: _this[keys[idx++]],
					done: idx > keys.length,
				};
			},
		};
	},
});
```

## Proxy

### 什么是 Proxy

`Proxy` 用于修改用户的默认操作或行为，比如说想在对象在调用其的属性时，希望在调用前做些校验，就可以使用 `Proxy` ；简单来说 `Proxy` 就像是一代理器
他与 `defineProperty` 区别在于 `Proxy` 可以监听到对对象的操作如 `delete` 等，更好的监视数组

### Proxy 的用法

`Proxy` 接收两个参数，第一个是要操作的对象，第二个参数是一个对象，这个对象有`setter`，`getter` 属性 `Proxy(target,handle)`

#### get

`get` 接收三个参数，第一是目标对象，第二个是属性名，第三个是 proxy 实例本身

```js
var obj = { name: "小王", age: 14 };

var proxy = new Proxy(obj, {
	get(target, key, proxy) {
		if (key in target) {
			return target[key];
		} else {
			return new Error("属性不存在");
		}
	},
});
```

#### set

`set` 接收四个参数，依次为，目标对象、属性名、属性值、proxy 实例本身

```js
var obj = { name: "小王", age: 14 };

var proxy = new Proxy(obj, {
	get(target, key, proxy) {
		if (key in target) {
			return target[key];
		} else {
			return new Error("属性不存在");
		}
	},
	set(target, key, value, proxy) {
		if (key === "age") {
			if (!Number.isInteger(value)) {
				throw new Error("age必须是number");
			}
		}
		target[key] = value;
	},
});
```

#### apply

apply 拦截函数

```js
function fn(){
    return 'hello world'
}

var proxy = new Proxy(fn,{
    return '你好'
})

proxy() // '你好'

```

```js
function(left,right){
    return left + right
}

var proxy = new Proxy(fn,{
    return Reflect.apply(...arguments)*2
})

proxy(1,2) // 6

```

## Reflect

统一对象操作

```js
const obj = {
	name: 'IAN',
	age: 18
}
// 以前删除对象中的属性
delelte obj.age

// 以前判断属性在不在对象中
'age' in obj

// 以前获取对象中的所有属性名
Object.keys(obj)
```

感觉很乱，操作对象一会调用 Object 的方法，一会使用操作符

现在都统一使用 `Reflect` 对象下的方法

```js
Reflect.deleteProperty(obj, "age"); // 删除属性
Reflect.has(obj, "name"); // 判断属性在不在对象中
Reflect.ownKeys(obj); // 获取对象中的所有属性名
```

更多 `Reflect API` 见 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

## Promise

### 什么是 Promise

`Promise` 是一个构造函数，它可以获取一个异步操作的最终状态，

Promise 实例 一共有三种状态

- `pending` 代表异步操作未完成
- `Fulfiled` 代表异步操作成功
- `rejected` 代表异步操作失败

Promise 构造函数的第一参数，接收一个函数，而这个函数是处理 Promise 状态变化的

这个函数的第一个参数是将状态改为 resolved，第二个参数是将状态变更为 rejected

```js
function fn(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			if (n <= 10) {
				// 如果 n 小于等于 10 就将状态变为 resolved
				resolve("n小于10");
			} else {
				// 如果 n 大于等于 10 就将状态变为 rejected
				reject("n大于于10");
			}
		}, 1000);
	});
}
fn(11)
	.then((resolve) => {
		console.log("resolve", resolve);
	})
	.catch((rejected) => {
		console.log("rejected", rejected);
	});

// rejected n大于于10
```

:::tip
如果 Promise 中存在多个 resolve 或 reject，会以第一个为主
Promise 中的代码是同步的
:::

### then()

Promise 的 then 方法可以接受异步操作的状态， then 方法的接收两个函数作为参数，第一个函数是处理 `resolved` 状态，第二个参数是处理 `rejected` 状态

then 方法也会返回 Promise 对象，所以可以对 then 进行链式操作

:::tip
下一轮的 then 会处理上一轮 then 返回的 Promise 状态，如果上一轮没有返回会继续向上找
then 中的回调方法是微任务
:::

```js
Promise.resolve(1).then(2).then(3).then(console.log);

//   1
```

### catch()

catch 方法与 then 的第二产生作用相同，都是用来处理 `rejected` 状态的

### Promise.all()

Promise.all 方法接收一个 Promise 集合的数组，当所有的的是 Promise 都是 `resolved` 状态是才会进入到 `then` 方法，否则进入 `catch`

:::tip
Promise 还可以通过注册全局事件 `unhandledrejection` 来处理异常
:::

```js
window.addEventListener("unhandledrejection", (event) => {
	console.warn(`UNHANDLED PROMISE REJECTION: ${event.reason}`);
});
```

```js
function fn(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			if (n <= 10) {
				// 如果
				resolve("n小于10");
			} else {
				reject("n大于于10");
			}
		}, 1000);
	});
}

function fn2(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			if (n <= 10) {
				// 如果
				resolve("n小于10");
			} else {
				reject("aa");
			}
		}, 1000);
	});
}

function fn3(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			if (n <= 10) {
				// 如果
				resolve("n小于10");
			} else {
				reject("xx");
			}
		}, 1000);
	});
}

Promise.all([fn(3), fn2(4), fn3(5)])
	.then(
		(r) => {
			console.log(r);
		},
		(e) => {
			console.log(e);
		}
	)
	.catch((e) => {
		console.log(e);
	});

// ["n小于10", "n小于10", "n小于10"]
```

### Promise.race()

Promise.race 方法的参数也是接收一个 Promise 集合的数组，不同的是它看谁先的到结果，然后去处理它的结果

```js
function fn(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			if (n <= 10) {
				// 如果
				resolve("n小于10");
			} else {
				reject("n大于于10");
			}
		}, 3000);
	});
}

function fn2(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			if (n <= 10) {
				// 如果
				resolve("n小于10");
			} else {
				reject("aa");
			}
		}, 2000);
	});
}

function fn3(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			if (n <= 10) {
				// 如果
				resolve("n小于10");
			} else {
				reject("xx");
			}
		}, 1000);
	});
}

Promise.race([fn(3), fn2(4), fn3(5)])
	.then(
		(r) => {
			console.log(r);
		},
		(e) => {
			console.log(e);
		}
	)
	.catch((e) => {
		console.log(e);
	});

// n小于10
```

这里 fn3 最快，所以 then 方法处理的 fn3 的结果
