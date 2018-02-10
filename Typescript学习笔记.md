# Typescript学习笔记

## 快速上手
### 编译代码
```powershell
tsc greeter.ts
```

### 类型注解
TypeScript里的类型注解是一种轻量级的为函数或变量添加约束的方式。
```typescript
// 约束参数类型为string类型
function greeter(person: string) {
    return "Hello, " + person;
}

let user = 'zhangsan';

document.body.innerHTML = greeter(user);
```

### 接口
我们在实现接口时候保证包含了接口要求的结构。
```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```

### 类

## 知识点
### 参数新特性
#### 参数类型
在参数名称后面使用冒号来指定参数的类型
```typescript
const myname: string = 'zhang san';

const alias: any = 'xixi';

const age: number = 13;

const man: boolean = true;

function test(name: string) :string {
    return ''
}

class Person {
    name: string;
    age: number;
}

const zhangsan: Person = new Person();
zhangsan.name = 'zhangsan';
zhangsan.age = 18;
```

#### 默认参数
在参数声明后面用等号来指定参数的默认值
```typescript
const myname: string = 'zhai liang';

// 指定第三个参数的默认值为xixi，默认值的参数一定要放在最后面
function test (a: string, b: string, c: string = 'xixi') {
    console.log(a);
    console.log(b);
    console.log(c);
}

test('xxx', 'yyy', 'zzz');
test('xxx', 'yyy');
```

#### 可选参数
在方法的参数声明后面用问号来标明此参数为可选参数（可选参数一定要在必选参数后面）
```typescript
function test (a: string, b?: string, c: string = 'xixi') {
    console.log(a);
    console.log(b);
    console.log(c);
}

test('xxx');
```

### 函数新特性
#### Rest and Spread 操作符
声明任意数量的方法参数
```typescript
function func1(...args) {
    args.forEach(function (arg) {
        console.log(arg);
    })
}

func1(1, 2, 3);

func1(7, 8, 9, 10, 11);
```

可以反过来调用
```typescript
function func1(a, b, c) {
    console.log(a);
    console.log(b);
    console.log(c);
}

const args1 = [1, 2];
func1(...args1);

const args2 = [7, 8, 9, 10, 11];
func1(...args2);
```

#### Generator函数
控制函数的执行过程，手工暂停和恢复代码执行
```typescript
function* doSomething() {
    console.log('start');
    
    yield;
    
    console.log('finish');
}

var func1 = doSomething();

func1.next();       // 'start'

func1.next();       // 'finish'
```

#### destructuring 析构表达式
通过表达式将对象或数组拆解成任意数量的变量
```typescript
function getStock() {
    return {
        code: 'IBM',
        price: 100
    }
}

let {code, price} = getStock();     // 变量名要跟返回值的对象属性一致

console.log(code);      // 'IBM'
console.log(price);     // 100
```

```typescript
function getStock() {
    return {
        code: 'IBM',
        price: 100
    }
}

let {code: codex, price} = getStock();     // 取出code属性放到codex的变量里

console.log(codex);      // 'IBM'
console.log(price);     // 100
```

析构表达式嵌套
```typescript
function getStock() {
    return {
        aaa: 'xixi',            // 不影响析构表达式
        bbb: 'haha',            // 不影响析构表达式
        code: 'IBM',
        price: {
            price1: 200,
            price2: 400
        }
    }
}

let {code: codex, price: { price2 }} = getStock();     // 取出code属性放到codex的变量里

console.log(codex);      // 'IBM'
console.log(price2);     // 400
```

析构表达式如何从数组里取值
```typescript
const array1 = [1, 2, 3, 4];

const [number1, number2] = array1;
console.log(number1);       // 1
console.log(number2);       // 2

const [, , number3, number4] = array1;
console.log(number3);       // 3
console.log(number4);       // 4
```

```typescript
const array1 = [1, 2, 3, 4];

const [number1, number2, ...others] = array1;
console.log(number1);       // 1
console.log(number2);       // 2
console.log(others);        // [3, 4]
```

### 箭头表达式与循环
#### 箭头表达式
```typescript
const sum = arg1 => {
    // arg1 为参数，可以省略()
    console.log(arg1)
}
```
```typescript
const myAarry = [1, 2, 3, 4, 5];

// 取出myAarry的偶数
console.log(myAarry.filter(value => value % 2 === 0));
```

箭头表达式可消除传统匿名函数this指针的问题
```typescript
function getStock(name: string) {
    this.name = name;
    
    setInterval(function() {
      console.log('name is : ' + this.name);        // name is: 
    }, 1000);
}

var stock = new getStock('IBM');

function getStock2(name: string) {
    this.name = name;
    
    setInterval(() => {
      console.log('name is : ' + this.name);        // name is : 'IBM'
    }, 1001);
}

var stock2 = new getStock2('IBM');
```

#### forEach(), for in 和 for of
```typescript
const myAarry = [1, 2, 3, 4, 5];
myAarry.desc = 'four number';

// for of 不会显示数组自定义属性
for (let n of myAarry) {
    // 循环可以被打断
    if (n > 2) break;
    console.log(n);
}

// 1 2
```

for of 还可以打印字符串的每一个字符
```typescript
for (let n of 'four number') {
    console.log(n);
}

// f o u r   n u m b e r
```

### 面向对象特性
#### 类
类是Typescript的核心，使用Typescript开发时，大部分代码都是写在类里面的。
##### 类的定义
```typescript
class Person {
    name;
    // 直接类里声明属性或方法，等价于 public name
    // private name：则只能在类的内部访问和使用，无法在类的外部访问和使用
    // protected name：则在类和子类的内部访问和使用
    
    eat() {
        console.log(`${this.name}, i'm eating`);
    }
}

const p1 = new Person();
p1.name = 'batman';
p1.eat();

const p2 = new Person();
p2.name = 'superman';
p2.eat();
```

##### 类的构造函数
```typescript
class Person {
    // constructor 是类的一个特殊方法，只有在类被实例化的时侯被调用
    // 这个方法不能在类的外部访问
    constructor() {
        console.log('haha');
    }

    name;
    
    eat() {
        console.log(`${this.name}, i'm eating`);
    }
}

const p1 = new Person();
p1.name = 'batman';
p1.eat();

const p2 = new Person();
p2.name = 'superman';
p2.eat();
```

实例化类的时侯必须为其指定一个名字
```typescript
class Person {
    constructor(public name: string) {
        console.log('haha');
    }
    
    // 等价于下面的写法
    // name;
    // 
    // constructor(name: string) {      // 没有加public访问控制，那么name就没有声明，name不能被外部访问
    //     console.log('haha');
    // }
    
    eat() {
        console.log(`${this.name}, i'm eating`);
    }
}

const p1 = new Person('batman');
p1.eat();

const p2 = new Person('catlady');
p2.name = 'superman';
p2.eat();

// haha
// batman, i'm eating
// haha
// superman, i'm eating
```

##### 类的继承
```typescript
class Person {
    constructor(public name: string) {
        console.log('haha');
    }
    
    eat() {
        console.log(`${this.name}, i'm eating`);
    }
}

class Employee extends Person {
    constructor(name: string, code: string) {
        super(name);
        this.code = code;
        console.log('a employee created!')
    }
    
    code: string;
    
    work() {
        super.eat();
        console.log('im working now')
    }
    
}

const e1 = new Employee('zhangsan', '1');
e1.work();

// haha
// a employee created!
// zhangsan, i'm eating
// im working now
```

#### 泛型
```typescript
class Person {
    constructor(public name: string) {
        console.log('haha');
    }
    
    eat() {
        console.log(`${this.name}, i'm eating`);
    }
}

class Employee extends Person {
    constructor(name: string, code: string) {
        super(name);
        this.code = code;
        console.log('a employee created!')
    }
    
    code: string;
    
    work() {
        super.eat();
        this.doWork();
    }
    
    private doWork() {
        console.log('im working now')
    }
}

// 泛型：指定数组只能放某一个类型的元素
var workers: Array<Person> = [];
workers[0] = new Person('lisi');                // 可以放Person类
workers[1] = new Employee('wangwu', '3');       // 可以放Person的子类

const e1 = new Employee('zhangsan', '1');
e1.work();
```

#### interface & implements
##### interface
用来建立某种代码约定，使得其它开发者在调用某个方法或创建新的类时必须遵循接口所定义的代码约定
```typescript
interface IPerson {
    name: string;
    age: number;
}

class Person {
    constructor(public config: IPerson) {
        
    }
}

// 规范传入对象的合法性
var p1 = new Person({
    name: 'zhangsan',
    age: 18
})
```

##### implements
```typescript
interface Animal {
    eat();
}

class Sheep implements Animal {
    eat() {
        console.log('i eat grass!')
    }
}

class Tiger implements Animal {
    eat() {
        console.log('i eat meat!')
    }
}
```

#### 模块(Module)
模块可以帮助开发者将代码分割为可重用的单元。开发者可以自己决定将模块中的哪些资源（类、方法、变量）暴露出去供外部使用，哪些资源只在模块内使用。
> 模块在typescript里就是一个文件，一个文件是一个模块，在模块的内部有两个关键字来支撑模块的特性export和import
**a.ts**
```typescript
export const prop1 = 1;
const prop2 = 2;

export function func1() {}
function func2() {}

export class Class1 {}
class Class2 {}
```

**b.ts**
```typescript
import {prop1, func1, Class1} from './a';

console.log(prop1);
func1();
new Class1();

export function func3() {}
```

#### 注解(annotation)
FIXME: 这里回头还要自己找资料多看看
```typescript
import {Comonent} from '@angular/core';

@Comment({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
})
```

#### 类型定义文件(*.d.ts)
类型定义文件用来帮助开发者在TypeScript中使用已有的JavaScript的工具包，如：JQuery
> 类型定义文件从 [https://github.com/DefinitelyTyped]() 中找到，这里有几乎所有的常用的Javascript框架的类型定义文件

## 相关链接
- [Typescript手册指南](https://www.tslang.cn/docs/handbook/basic-types.html)