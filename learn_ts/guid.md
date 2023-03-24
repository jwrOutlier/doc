# TS 部分

*   基础类型

```typescript
let num:number = 0
let str:string = 'hello'
let bool:boolean = true

// 数组类型的定义
let arr:number[]=[1,2,3,4] // 方式一
let arr:Array<number> = [1,2,3,4,5]

// 元组  没啥新意就是已知元素数量和类型的数组
type arrTuple =[number,string]
let arr:arrTuple=[11,'string']

// enum  枚举
enum Color {Red, Green, Blue}
let c = Color.Red  // 为零0 然后Green 为1  依次递减
// 可以为其赋值
enum Color {Red='red', Green='green', Blue:'blue'}

// any类型 和原生js 一样
// 我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。
// 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。
// 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查
let un:any = 111
un = 'hello world'
un = boolean

// unknwon 和any的用法一样但是有区别 如下例子：
let un:unknown = 111
un = 'hello world'

un.slice()  // 这个使用时 ts 是不能通过编译的 但是any可以  可以使用类型收窄 来判断 例如 typeof

let num:number = 1111
let un:unknown = num   // 报错  any不报错
```

&#x20;其他数据类型可以参考文档 <https://bosens-china.github.io/Typescript-manual/download/zh/handbook/basic-types.html#%E4%BB%8B%E7%BB%8D>

*   接口&#x20;

```typescript
// 接口定义
interface IPerson {
	name:string,
	age:number
}
//缺少 接口中的属性时会报错
let person:IPerson{
	name:'Cooper',
	age:18
}

// 可选属性
interface IPerson {
	name:string,
	age:number,
	address?:string
}
let person:IPerson{
	name:'Cooper',
	age:18,
	address:'xxxx'   // 不写也可以
}

// 只读属性
interface IPerson {
	readonly name:string,
	readonly age:number,
	address?:string
}

let person:IPerson{
	name:'Cooper',
	age:18,
	address:'xxxx'   // 不写也可以
}
person.name = 'hjahh'  // 报错

// 额外属性
interface IPerson {
	readonly name:string,
	readonly age:number,
	address?:string,
	[propName:string]:string|number|any  // 改属性必须要包含以上的类型  可以直接写any类型
}

let person:IPerson{
	name:'Cooper',
	age:18,
	address:'xxxx',   // 不写也可以
	aaa:'sssss'
}

// 接口定义函数类型
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
};

// 定义数组
interface StringArray {
  [index: number]: string;
}
let myArray: StringArray;
myArray = ["Bob", "Fred"];
let myStr: string = myArray[0];

// 定义类
interface ClockInterface {
  currentTime: Date;
}
class Clock implements ClockInterface {
  currentTime: Date = new Date();
  constructor(h: number, m: number) {}
}   // constructor 为静态属性   ts不会检查 所以在定义接口时不用定义constructor

// 接口继承
interface Shape {
  color: string;
}

interface PenStroke {
  penWidth: number;
}

interface Square extends Shape, PenStroke {
  sideLength: number;
}
```

其他语法可见文档：<https://bosens-china.github.io/Typescript-manual/download/zh/handbook/interfaces.html#%E4%BB%8B%E7%BB%8D>

*   函数

```typescript
// 为函数定义类型
function add(num1:number,num2:number):number{
	return num1 + num2
}

// 完整定义类型
type TDD = (num1:number,num2:number)=>number
let add:TDD = function(num1:number,num2:number):number{
	return num1 + num2
}

// 可选参数
function buildName(firstName: string, lastName?: string) {
    if (lastName){
		return firstName + " " + lastName;
	} else{
	    return firstName;
	}
}

// 默认参数
function buildName(firstName = "Will", lastName: string) {
    return firstName + " " + lastName;
}

// 剩余参数
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}
let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");

```

this 参数

```typescript
interface IObj{
	name:string
	sayHello(this:IObj):void
}
let obj:IObj = {
	name:'Cooper'
	sayHello(){
		console.log(this.name)
	}
}
```

*   联合类型和交叉类型

```typescript
interface IObj {
	id:string|number
}
// 伪代码
let obj：IObj  = {
	id: 111/'1111'  // 都可以
}

// 交叉类型
type Animal = {
	run():void
}

type Dog= {
	wolf():void
	run():void
}

type Obj = Animal & Dog
let obj:Obj = {
	run(){
		console.log('在跑')
	}
}
```

其他知识点请访问：[联合类型和交叉类型 | TypeScript手册 (bosens-china.github.io)](https://bosens-china.github.io/Typescript-manual/download/zh/handbook/unions-and-intersections.html#%E4%BB%8B%E7%BB%8D)

*   类

```typescript
// 公共成员修饰符public  在类外和类内都可以访问
// 私有成员修饰符private 只可以在当前类的内部使用
// 受保护成员修饰符protected 不能在类的外部使用，但是可以在派生类中使用

// 类的类型定义可以参考接口定义类
```

注意：构造函数也可以被标记成 `protected` 。 这意味着这个类不能在包含它的类外被实例化，但是能被继承

类的其他知识点不熟悉可以参考&#x20;

[Class 的基本语法 - ECMAScript 6入门 (ruanyifeng.com)](https://es6.ruanyifeng.com/#docs/class)

[类 | TypeScript手册 (bosens-china.github.io)](https://bosens-china.github.io/Typescript-manual/download/zh/handbook/classes.html)

*   泛型

为什么要使用泛型：软件工程中，我们不仅要创建一致的定义良好的 API，同时也要考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。

在像 C# 和 Java 这样的语言中，可以使用 `泛型` 来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件（官方的定义，呃呃简直说的就不是人话）

举个例子：体会一下泛型解决的问题

```typescript
// 刚开始需求是这样的
function print(arg:string):string {
    console.log(arg)
    return arg
}

// 现在需求变了，我还需要打印 number 类型，怎么办?
// 当然我们可以使用联合类型 但是这种方式很笨 后面如果还要添加别的类型怎么办？ 难道继续往后面加？
function print(arg:string|number):string|number {
    console.log(arg)
    return arg
}

// 也可以使用any 这样是比较灵活 但是我们在ts中尽量不推荐使用any类型
function print(arg:string|number):string|number {
    console.log(arg)
    return arg
}

// 这个时候我们可以使用泛型
// 泛型的基本使用
function print<T>(arg:T):T {
    console.log(arg)
    return arg
}
print<string>('hello') // 其中string 可以不写 通过类型推断来获得类型

// 其中的T 可以理解为一个变量 当我们显示定义<string> 时，好比把string传了过去

// 另外接口 定义时我们可以这样写
interface Iprint<T> {
    (arg: T): T
}
function print<T>(arg:T) {
    console.log(arg)
    return arg
}
const myPrint: Iprint<number> = print

// 我们也可以为泛型定义一个默认的参数
function print<T = number>(arg:T):T {
    console.log(arg)
    return arg
}
print(221)


// 处理多个函数参数
function swap<T, U>(tuple: [T, U]): [U, T]{
    return [tuple[1], tuple[0]]
}
const res = swap(['Cooper',18])

// 约束泛型
function printLength<T>(arg: T): T {
    console.log(arg.length)
    return arg
}
// 我们不能确定是否有length属性 所以这段代码会报错 这时候就需要添加约束
interface ILength {
    length: number
}
function printLength<T>(arg: T extends ILength): T {
    console.log(arg.length)
    return arg
}  // 这样所传的值就必须具有length属性

// 泛型约束类
// 定义一个栈
class Stack<T> {
	private data:T[] = []
	push(this:Stack,item:T){
		return this.data.push(item)
	}
	pop():T | undefined {
        return this.data.pop()
    }
}
const s1 = new Stack<number>()
s1.push(111)
s1.push('hello')  // ts 报错

// 泛型约束接口
interface IKeyVal<T,U>{
	key:T,
	name:U
}
const k1:IKeyVal<number, string> = { key: 18, value: 'Cooper'}
const k2:IKeyVal<string, number> = { key: 'Cooper', value: 18}

// 定义泛型数组
// 之前这样的定义
const arr: number[] = [1,2,3]
// 泛型
const arr: Array<number> = [1,2,3]
```

*   实用的工具类型：

```typescript
// 将所有的属性值变为可选类型   Partial<Type>
interface IPerson {
	name:string
	age:number
}
let person:Partial<IPerson> = {
	name:'Cooper'
}

// Record<Keys, Type>   构造一个类型，其属性名的类型为 K ，属性值的类型为 T 。这个工具可用来将某个类型的属性映射到另一个类型上
interface PageInfo {
    title: string;
}
type Page = 'home' | 'about' | 'contact';
const x: Record<Page, PageInfo> = {
    about: { title: 'about' },
    contact: { title: 'contact' },
    home: { title: 'home' },
};

// Pick<Type, Keys> 从类型 Type 中挑选部分属性 Keys 来构造类型。
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Pick<Todo, 'title' | 'completed'>;

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
};
// ------> Pick 源码  keyof 来获取类型的所有键 返回一个联合类型
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};
// keyof T  = 'title'|'description'|'completed'  P in K：in表示遍历语法关键字;P 是类型变量，代表每次遍历出来的成员类型;K 指的是要遍历的类型

//------------------------------->
// Omit<Type, Keys>  从类型 Type 中获取所有属性，然后从中剔除 Keys 属性后构造一个类型
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}
type TodoPreview = Omit<Todo, 'description'>;
const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
};
```

还有很多类型这里就不一一举例，详情请查看&#x20;

[实用工具类型 | TypeScript手册 (bosens-china.github.io)](https://bosens-china.github.io/Typescript-manual/download/zh/reference/utility-types.html#%E7%9B%AE%E5%BD%95)
