## 问题一

```js
var a = [];

/* 同步代码执行完毕后执行 a[6]() */
for (var i = 0; i < 10; i++) {
	a[i] = function () {
		console.log(i); // 函数内部未声明 i ，所以会顺着作用链向上寻找 i
	};
}

a[6]();
```

最后执行的结果是 `10`, 因为 for 循环中的 i 是 var 声明的，这等同于在在全局声明了一个变量 i 。当循环体执行完毕是 i 已经等于 10 了，所以结果是 `10`

## 问题二

```js
var tmp = 123;

if (true) {
	console.log(tmp); // 报错
	let tmp;
}
```

最后会报错，因为 let 声明的变量有自己的作用域, 并在作用域内形成暂时性死区，在声明前如果调用变量都会报错

## 问题三

```js
var arr = [12, 34, 32, 89, 4];
```

答案：利用 sort 方法从小到大排序然后去第一位

```js
arr.sort((x, y) => x - y)[0];
```

## 问题四

`var`、`let`、`const` 的区别

```
1. `var` 声明的变量会声明提升，可重复声明变量，可随意修改变量的值
2. `let` 声明的变量不会声明提升，不可重复声明，会形成暂时性死区，提前使用会报错
3. `const` 与 let 相似，不同的是它声明的变量的值不可修改
```

## 问题五

```js
var a = 10;

var obj = {
	a: 10,
	fn() {
		setTimeout(() => {
			console.log(this.a);
		});
	},
};

obj.fn();
```

- web: 10
- node: undefined

由于 setTimeout 内是箭头函数，而箭头函数不会改变 this 指向，它内部的 this 和当前执行上下文有关，这里 fn 的执行环境是全局
在 web 环境下的执行结果是 `10`(web 环境下，全局作用域下用 var 声明变量相当于在 window 对象上挂载属性)，node 环境下是 `undefined`（node 环境下全局环境是 {} ）

## 问题六

`Symbol` 和 `Object` 一样也是一个内置对象，`Symbol` 的值是独一无二的，可以为对象添加一个独一无二的属性也可以声明一个常量

## 问题七

浅拷贝：仅仅只是拷贝在内存中引用的地址，如果原地址中的对象被改变了，那么浅拷贝出来的对象也会被影响
深拷贝：在内存中新开辟一个地址存放拷贝出来的对象，修改原地址中的对象不会影响拷贝的对象

## 问题八

假如 JavaScript 中没有异步只有同步任务，那么我们的定时器或 ajax 请求都会阻塞 js 执行，非常影响用户体验

`Event loop` 是一种事件循环机制，js 在执行代码的时候会将同步代码直接压入执行栈中执行，而异步任务则会被分配到微任务队列或宏任务队列中等待同步任务执行完毕。当同步任务执行完毕后会检查微任务队列中是否有任务，如果有就压如执行栈中，否则就检查宏任务队列，如果有任务需要执行压入执行栈，如此循环直到所有队列清空
常见的微任务比如 Promise then 或 catch 中的回调函数
常见的宏任务比如 setTimeout 和 setInterval

## 问题九

```js
setTimeout(function () {
	var a = "hello";
	setTimeout(function () {
		var b = "lagou";
		setTimeout(function () {
			var c = "I 💕 U";
			console.log(a + b + c);
		}, 10);
	}, 10);
}, 10);
```

答案：

```js
new Promise((resolve) => {
	setTimeout(function () {
		resolve("hello");
	}, 10);
})
	.then((res) => {
		return new Promise((resolve) => {
			setTimeout(function () {
				resolve(res + "lagou");
			}, 10);
		});
	})
	.then((res) => {
		setTimeout(function () {
			console.log(res + "I 💕 U");
		}, 10);
	});
```

## 问题十

`TypeScript` 是 `JavaScript` 的超集，它提供 类型系统 和对 ES6 的支持，增强了代码的可读性和维护性

## 问题十一

1. 渐进式的，完全可以像写 JavaScript 代码一样去写 TypeScript
2. 强大的类型系统使得代码变得更加健壮，增加代码可读性与维护性减少产生 bug 的可能性
3. 缺点在于写起来比较啰嗦，会增加代码量
