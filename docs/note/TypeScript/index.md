# TypeScript中文手册 笔记

[TypeScript 中文手册](https://typescript.bootcss.com/)

# 基础类型

## 布尔值 `boolean`

## 数字 `number`

## 字符串 `string`

## 数组 `{type}[]`

## 元组 `[type,…type]`

## 枚举`(enum) enum name {value}`

```tsx
enum Color {red, green, blue}
```

详细见枚举专题

## 任意值 `any`

## 空值 `void`

声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`

## null `null`

## underfined `underfined`

默认情况下`null`和`undefined`是所有类型的子类型。 就是说你可以把`null`和`undefined`赋值给`number`类型的变量。

## Never `Never`

错误或者无限循环

```tsx
// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

## 类型断言

我来确定类型，我比系统权力大

```tsx
let strLength: number = (<string>someValue).length

let strLength: number = (someValue as string).length //jsx允许 
```

# 高级类型

## 交叉类型 &

```tsx
string & number[]   //可以使用所有的字符串和数组方法
```

## 联合类型 |

```tsx
string | number[]   //可以使用字符串和数组共有的方法
```

## 类型别名 type

```tsx
type Name = string
type NameResolver = string | ()=> string;
```

### 泛型-类型别名

```tsx
type container<T> = {value: T}
```

## 字面量类型

### 字符串字面量类型

```tsx
type color = "red" | "blue" | "yellow"
```

### 数字字面量类型

使用不多

```tsx
function numberSelect():1|2|3|4|5{ } 
```

# 接口 interface

一种类型规范或者契约

```tsx
 function fn(parmasObj: {name: string}){
};

interface nameValue {
  name: string
}
function fn(paramsObj: nameValue)
```

## 可选属性 ?

```tsx
interface obj {
  width?: number;
  height?: number
}
```

### 优点

检测属性名拼写错误

```tsx
config：obj
config.clor  //error： clor属性不存在
```

### 缺点

一些不必要的错误检测

```tsx
function fn(config: obj) {}
fn({colour:""}) //error 应是color属性，而我本意是colour
```

## 只读属性 readonly

```tsx
interface position {
  readonly x: number;
  readonly y: number;
}
```

### 只读数组

## **`readonly` vs `const`**

最简单判断该用`readonly`还是`const`的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用`const`，若做为属性则使用`readonly`。

## 接口继承

# 类型推论

类型在不被定义时被提供类型

计算通用类型算法会考虑所有的候选类型，并给出一个兼容所有候选类型的类型。以下`x`的类型可能是`null`和`number`，则推断算法给出一个兼容所有类型的类型

```tsx
let x = [0,1,null]
```

推断不成功的情况,`zoo` 的类型想被推断为`Animal[]`,但成员没有这个类型的，故无法推断出

```tsx
let zoo = [new Snake(),new Elephant()]
```

## 上下文类型

> **按上下文归类会发生在表达式的类型与所处的位置相关时**

函数类型可以写一半的原因，由上下文类型推论出另一半

`mouseEvent`的类型会被推断出根据左边函数的类型

```tsx
window.onmousedown = function(mnouseEvent) {}
```

# 类型保护

联合类型使用时的问题

```tsx
fn():number | string
let x = fn()
x.map() //error
(<number>x).map() //right
```

无法判断`x`的类型，无法使用`number`或`string`独立的方法，只能通过类型断言解决。

## typeof类型保护

```
typeof s === “typename” | typeof s  !== “typename”
typename` 必须为 `number string boolean symbol
function padLeft(value: string, padding: string | number) {
    if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}
```

## instanceof 类型保护

`instanceof`要求右侧是构造函数 | 类

# 类型兼容性

# 函数 func

## 完整函数类型

参数类型和返回值类型

```tsx
let fn:(x:number, y:string) => number = 
    function(x:number, y:string): number {}; 
```

<aside> 💡 通过类型推论 写一半没问题

</aside>

# 泛型

<aside> 💡 考虑代码可复用性，使用泛型

</aside>

## 对比

不用泛型

```tsx
function identity(arg: number): number {
    return arg;
}

function identity(arg: any): any {
    return arg;
}
```

使用泛型

```tsx
function indentity<T>(arg:T):T {
  return arg;
}
let output = indentity<number>(1) //输出类型也为number
```

## 使用泛型变量

<aside> 💡 原因是使用泛型的函数体内 可能无法立刻使用一些类型的方法，因为类型是待定的，比如数组的length

</aside>

```tsx
function indentity<T>(arg:T[]):T[] {
    console.log(arg.length);
    return arg;
}
```

当函数参数`arg`是一个`number[]`类型时，`T`的类型为`number`，即`T`是类型的一部分

另一种写法

```tsx
function indentity<T>(arg: Array<T>): Array<T> {
    console.log(arg.length);  
    return arg;
}
```

## 泛型类型

### 泛型函数

前面所写叫函数声明

```tsx
let myindentity: <U>(arg: U) => U = indentity
let myindentity: {<U>(arg: U): U} = indentity
```

### 泛型接口

由泛型函数引出

```tsx
interface GenericIndentityFn {
  <T>(arg: T): T;
}
let myindentity: GenericIndentityFn = indentity
```

第二种写法

接口内其他属性都能得到参数的类型

```tsx
interface GenericIndentityFn<T> {
  (arg: T): T;
}
let myindentity: GenericIndentityFn<number> = indentity
```

## 泛型约束

约束泛型，即给泛型加一些条件，比如`length`属性，即泛型的类型必须有`length`属性才不报错

使用`interface` 和 `extends` 来添加约束

```tsx
interface lengthWise {
    length: number;
}
funcition indentity<T extends lengthWise>(args:T):T {
    console.log(args.length)
}
indentity(3)    //error,number has no length property
```

### 使用类型参数

我觉得类型参数 就是泛型

使用一个泛型 约束另一个泛型

```tsx
function getProperty<T,K extends keyof T>(obj:T,key:K) { 
    return obj[key]
}
```

# 枚举

## 最简单的枚举

```tsx
enum Direction{ 
  up,
  down,
  left,
  right,
}
Direction {0:”up”,1:”down”,2:”left”,3:“right”}
```

## 枚举成员是常数

```tsx
enum Direction{ 
  up = 1,
  down,
  left,
  right,
}
Direction {1:”up”,2:”down”,3:”left”,4:“right”}
enum Direction{ 
  up,
  down = 2,
  left,
  right,
}
Direction {0:”up”,2:”down”,3:”left”,4:“right”}
```

## 枚举成员是计算得出的值

```tsx
enum FileAccess {
  None,
  Read = 1 << 1
  Write = 1 << 2
  ReadWrite = Read | Write
  L = "123".length
}
```

## **枚举类型的结果是一个对象，且双向映射**

## 外部枚举