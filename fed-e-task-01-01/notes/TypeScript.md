# 初识 TypeScript

## 静态类型与动态类型

- 静态类型：在变量声明时就能确定类型，且在声明过后类型不允许修改
- 动态类型：在运行时才能确定变量类型，且变量类型可随时发生变化

从类型安全来讲，可将类型分为 `强类型` 和 `弱类型`。区别在于是否能够随意进行隐式类型转换
从类型检查来讲，可将类型分为 `静态类型` 和 `动态类型`。区别在是否可以随时修改变量的类型

> 强类型不等同于静态类型，弱类型也不等同于动态类型

## 基本类型

```js
let a: number = 2; // 声明一个只能为 number 类型的变量
let b: string = 2; // 声明一个只能为 string 类型的变量
let c: boolean = false; // 声明一个只能为 boolean 类型的变量
let d: void;
let e: undefined;
let f: null;
```

## 引用类型

```js
let h: string[] = ["1"]; // 声明一个所有项都是 string 类型的数组
let i: object = {}; // function | []
```

数组

```js
let arr1: [string, number] = ["1", 1]; // 元组
let arr2: string[] = ["1"];
let arr3: Array<string> = ["1"];
```

对象

```js
var tomo: { name: string, age: number } = { name: 'IAN', age: 18 }

// ----------------------

// 定义接口
interface PType {
    name: string
    age: number
    city?: string | number // 可选参数
}

var tomo: PType = {
    name: '1',
    age: 18,
    city: 2
}
```

```js
type PType = {
    name: string
    age: number
    city?: string | number // 可选参数
}

var tomo: PType = {
    name: '1',
    age: 18,
    city: 2
}
```

为对象添加只读属性

```js
interface PType {
	name: string
	age: number
	readonly city: string
}
```

动态接口

```js
interface PType {
	[prop]:string: string
}

const tomo: PType = {
	name: 'tomo',
	age: '18'
}
```

联合类型

```js
interface P2Type {
	gender: "男" | "女";
}

type P = PType & P2Type;

var tomo: P = {
	name: "tomo",
	age: 18,
	city: 2,
	gender: "男",
};
```

继承

```js
interface Person {
	name: string
	age: number
}

interface Tom extends Person {
	city: string
}

const tom: Tom = {
	name: 'tom',
	age: 18,
	city: 'x'
}
```

函数

```js
function fn1(a: number, b: number): number{ return a + b }

const fn2: (a: number, b: number) => number = (a: number, b: number): number => a + b

type Fn4Type = {
    a: number
    b: number
}

const fn4 = ({ a = 1, b }: Fn4Type): number => a + b

// 剩余参数
function fn5(a: string, ...b:string[]){
    return a + '' + b.join('')
}
```

## 断言

```js
interface obj1Type {
    name: string,
    sayName(): void
}

interface obj2Type {
    name: string,
    run(): void
}


function fn5(v: obj1Type | obj2Type) {
    if (typeof (v as obj1Type).sayName === 'function') {
        console.log(1)
    }
}
```

## 枚举

> 相比简单对象定义的 key-value，只能通过 key 去访问 value，不能通过 value 访问 key。但是在 enum 当中，正反都可以当做 key 来用。

```ts
enum EnumObj {
	"a" = 1,
	"b" = 2,
	"c" = 3,
	"d" = 4,
}

console.log(EnumObj["a"], EnumObj[1]);
```

## 泛型

> 定义接口或类的时候没有定义具体的类型，等到具体使用的时候再定义具体的类型，这种现象就叫做泛型

```ts
// 创建指定长度的数组，并用指定的值填充数据
function createArr<T>(length: number, value: T): Array<T> {
	let result: T[] = [];
	for (let i = 0; i < length; i++) {
		result[i] = value;
	}
	return result;
}

createArr<number>(10, 1);
```

```ts
interface Person<T, U> {
	name: T;
	age: U;
}

interface Tom extends Person<string, number> {
	city: string;
}

const tom: Tom = {
	name: "tom",
	age: 18,
	city: "x",
};
```

> 泛型类似函数的形参

## 类的访问修饰符

```ts
class Person {
	readonly name: string; // 只读属性不可修改
	private age: number; // private 私有属性，不可被实例与子类访问
	public city: string; // 共用属性，默认修饰符
	protected gender: string; // 受保护的，不可被实例访问，与 private 的却别在于 protected 可以被子类访问

	constructor(name: string, age: number) {
		this.name = name;
		this.age = age;
		this.gender = "男";
	}
}
```

## 类与接口

```ts
interface Eat {
	eat(food: string): void;
}
interface Run {
	run(distance: number): void;
}

class Person implements Eat, Run {
	eat(food: string): void {
		console.log(`优雅的就餐 ${food}`);
	}
	run(distance: number): void {
		console.log(`直立行走 ${distance}`);
	}
}

class Animal implements Eat, Run {
	eat(food: string): void {
		console.log(`喂食 ${food}`);
	}
	run(distance: number): void {
		console.log(`爬行 ${distance}`);
	}
}
```

## 抽象类

`abstract` 关键字可可以定义抽象类，抽象类不可以直接使用 `new` 的方法创建实例，它只能被继承
`abstract` 也可以实现一个抽象方法，定义了抽象方法后，子类中必须实现此方法

```js
abstract class Animal {
	eat(food: string): void {
		console.log(`喂食 ${food}`);
	}

	abstract run(distance: number): void
}

class Dog extends Animal {
	run(distance: number): void {
		console.log(`爬行 ${distance}`);
	}
}
```

## 类型声明

> 如果在一个模块声明的时候没有定义类型，在使用的时候可以使用 `declare` 声明

```ts
import { camelCase } from "lodash";
declare function camelCase(input: string);
```
