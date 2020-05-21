# TypeScript 语言

## Typescript 快速上手
### 安装
```bash
yarn add typescript --dev
```

### 使用
```bash
yarn tsc file.ts
```

## Typescript 配置文件
### 创建配置文件 tsconfig.json
```bash
yarn tsc --init
```

## Typescript 原始类型
```typescript
const a: string = 'foobar'
const b: number = 100 // NaN Infinity
const c: boolean = true
const e: void = undefined
const f: null = null
const g: undefined = undefined
const h: symbol = Symbol()
```

## Typescript Object 类型
```typescript
const i: object = {} // function () {}, []
const j: {} = {} // function () {}, []
```

## Typescript 数组类型
```typescript
const k: Array<number> = [1, 2]
const l: number[] = [1, 2]

// 用法
function sum (...args: number[]) {
    return args.reduce((prev, current) => prev + current, 0)
}

sum(1, 2, 3) // 6
```

## Typescript 元组类型
```typescript
const tuple: [number, string] = [1, '1']

console.log(tuple[0])
console.log(tuple[1])
```

## Typescript 枚举类型
```typescript
enum PostStatus {
    Draft = 0,
    Unpublished, // 默认+1，为1
    Published // 默认+1，为2
}

// 枚举会编译为双向的键值对对象
console.log(PostStatus.Draft) // 0
console.log(PostStatus[0]) // 'Draft'
```
编译为：
```js
"use strict";
var PostStatus;
(function (PostStatus) {
    PostStatus[PostStatus["Draft"] = 0] = "Draft";
    PostStatus[PostStatus["Unpublished"] = 1] = "Unpublished";
    PostStatus[PostStatus["Published"] = 2] = "Published";
})(PostStatus || (PostStatus = {}));
console.log(PostStatus.Draft);
console.log(PostStatus[0]);
```

> 如果不用索引器的方式查询枚举名称则可在声明枚举时加const，`const enum PostStatus`，这样编译出来的js会省略枚举的定义，只保留使用时的枚举定义注释。

## Typescript 函数类型
```typescript
function fun1 (a: number, b?: number): string {
    return 'func1'
}

console.log(fun1(100, 200));
console.log(fun1(100));
```

## Typescript 隐式类型推断
```typescript
let a = 1
a = 'string' // 错误，a已经隐式推断为number

let b // b隐式推断为any
b = 1
b = 'string'
```

## Typescript 类型断言
类型断言是代码编译时的概念，类型转换是代码运行时的概念
```typescript
const nums = [100, 200, 300]

const res = nums.find(item => item > 100)

const num1 = res * res // 报错，res有可能为undefined

const result = res as number // 方式1：类型断言为number
const num2 = result * result
console.log(num2);

const result2 = <number>res // 方式2：类型断言为number，Jsx不能使用
```


## Typescript 接口
接口用来约束对象的结构
```typescript
interface Post {
  title: string
  subTitle?: string // 可选成员
  content: string
  readonly summary: string // 只读
}

function printPost(post: Post) {

}
```

## Typescript 类
### 类的基本
```typescript
class Person {
  name: string  // 在typescript中，类的属性一定要初始值，要么在这赋值，要么在构造函数中初始化
  private age: number // 类的实例不能访问此属性
  protected readonly gender: boolean = true  // 类的实例不能访问此属性，允许子类访问，readonly不管是内部和外部以后都不能修改

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  sayHi (msg: string): void {
    console.log(`I am ${this.name}, ${msg}`)
    console.log(this.age)
  }

}

class Student extends Person {
  private constructor (name: string, age: number) { // 类的constructor加上private修饰符表示该类不能被实例化，也不能被继承
    super(name, age)
    console.log(this.gender)
  }

  static create (name: string, age: number) {
    return new Student(name, age)
  }
}

const tom = new Person('tom', 13)
console.log(tom.name) // tom, 可以访问

const jack = Student.create('jack', 17)
```

### 类与接口
```typescript
interface Eat {
  eat (food: string): void
}

interface Run {
  run (distance: number): void
}

class Person implements Eat, Run {
  eat (food: string): void {
    console.log(`优雅的进餐：${food}` )
  }

  run (distance: number): void {
    console.log(`直立行走${distance}`)
  }
}

class Animal implements Eat, Run {
  eat (food: string): void {
    console.log(`啃：${food}` )
  }

  run (distance: number): void {
    console.log(`爬行${distance}`)
  }
}
```

### 抽象类


## Typescript 泛型
