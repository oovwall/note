# ECMAScript新特性

## ES2015 let与块级作用域
### 有哪些作用域？
- 全局作用域
- 函数作用域
- 块级作用域

### var的双重循环问题
```js
for (var i = 0; i < 3; i ++) {
  for (var i = 0; i < 3; i ++) {
    console.log(i)
  }
  console.log('内层结束 i =', i)
}

// 0
// 1
// 2
// 内层结束 i = 3
```

### 循环注册事件问题
问题代码
```js
var elements = [{}, {}, {}]
for (var i = 0; i < 3; i ++) {
  elements[i].onclick = function() {
    console.log(i)
  }
}
elements[0].onclick() // 3
```

解决方法
```js
var elements = [{}, {}, {}]
for (var i = 0; i < 3; i ++) {
  elements[i].onclick = (function() {
    console.log(i)
  })(i)
}
elements[0].onclick() // 0
```

### for循环中的两层作用域
```js
for (let i = 0; i < 3; i ++) {
  let i = 'foo' // 不会报错
  console.log(i)
}

// foo
// foo
// foo
```
这个for循环等价于
```js
let i = 0

if (i < 3) {
  let i = 'foo'
  console.log(i)
}
i++

if (i < 3) {
  let i = 'foo'
  console.log(i)
}
i++

if (i < 3) {
  let i = 'foo'
  console.log(i)
}
i++
```


## ES2015 数组的解构
```js
const arr = [100, 200, 300]

const [, , baz] = arr
console.log(baz)  // 300

const [, , , more = 'default value'] = arr
console.log(more) // default value

const path = '/foo/bar/baz'
const [, rootdir] = path.split('/')
console.log(rootdir) // foo
```

## ES2015 对象的数组解构
对象解构重命名和属性的默认值
```js
const obj = { name: 'zhang', age: 17 }
const name = 'tom'

const { name: objName, gander = 'male' } = obj
const { log } = console

log(objName) // zhang
log(gander)  // male
```

## ES2015 带标签的模板字符串
```js
const name = 'zhang'
const gender = true

function myTagFunc(strings, name, gender) {
  const sex = gender ? 'male' : 'female'
  return strings[0] + name + strings[1] + sex + strings[2]
}

console.log(myTagFunc`hey, ${name} is ${gender}.`)
// hey, zhang is male.
```

## ES2015 字符串的扩展方法
用startsWith、endsWith、includes方法比indexOf更方便
```js
const message = 'Error: foo is not defined.'

console.log(message.startsWith('Error')) // true
console.log(message.endsWith('.')) // true
console.log(message.includes('foo')) // true
```

## ES2015 展开数组
```js
const arr = ['a', 'b', 'c']
console.log(...arr) // a b c
```

## ES2015 Proxy
Proxy 对象用于定义基本操作的自定义行为（如属性查找、赋值、枚举、函数调用等）。
```js
const person = {
  name: 'zhang',
  age: 17
}

const personProxy = new Proxy(person, {
  get(target, p, receiver) {
    console.log('get the property')
    return target[p]
  },
  set (target, p, value, receiver) {
    console.log(`set ${p} in ${target}`)
    target[p] = value
    return true
  },
  deleteProperty (target, p) {
    console.log(`delete the ${p}`)
    Reflect.deleteProperty(target, p)
    return true
  }
})

console.log(personProxy.name)
// get the property
// zhang

personProxy.gander = 'male'
// set gander in [object Object]

console.log(personProxy)
// { name: 'zhang', age: 17, gander: 'male' }
console.log(person)
// { name: 'zhang', age: 17, gander: 'male' }

delete personProxy.gander // delete the gander
console.log(person) // { name: 'zhang', age: 17 }

console.log(person.name) // zhang 这里没有监听输出
```

Proxy对数组的监视
```js
const list = []
const listProxy = new Proxy(list, {
  set(target, p, value, receiver) {
    console.log('set', p, value)
    target[p] = value
    return true
  }
})

listProxy.push(100) // set 0 100
```

## ES2015 Reflect
Reflect 是一个内置的对象，它提供拦截 JavaScript 操作的方法。Reflect 是一个内置的对象，它提供拦截 JavaScript 操作的方法。
```js
const obj = {
  name: 'zhang',
  age: 17
}

console.log('name' in obj)
console.log(Reflect.has(obj, 'name'))

console.log(delete obj.name)
console.log(Reflect.deleteProperty(obj, 'name'))

console.log(Object.keys(obj))
console.log(Reflect.ownKeys(obj))
```

## ES2015 静态方法
```js
class Person {
  constructor(name) {
    this.name = name
  }
  say () {
    `hi, my name is ${this.name}`
  }
  
  static create (name) {
    // 静态方法中的this不是指向该类的实例，而是直接指向该类
    return new Person(name)
  }
}

const tom = Person('tom')
tom.say()
```


## ES2015 Set
```js
const s = new Set()
s.add(1).add(2).add(3).add(4) // s的add方法返回实例本身，所以可以链式调用

console.log(s) // Set { 1, 2, 3, 4 }

s.forEach(val => console.log(val)) // 1 2 3 4

for (let val of s) {
  console.log(val)
} // 1 2 3 4

console.log(s.size) // 4
console.log(s.has(100)) // false
console.log(s.delete(3)) // true
console.log(s) // Set { 1, 2, 4 }
```

Set实现数组去重
```js
const arr = [1, 2, 3, 3, 2, 1]
console.log(Array.from(new Set(arr))) // [ 1, 2, 3 ]
console.log([...new Set(arr)]) // [ 1, 2, 3 ]
```

## ES2015 Map
Map相比于对象来说，可以用任意类型的数据作为Map的键值，而对象的key只能是string类型或Symbol类型
```js
const m = new Map()

const tom = { name: 'tom' }
m.set(tom, 90)
console.log(m.get(tom)) // 90
console.log(m.delete(tom)) // true
m.clear()

console.log(m) // Map {}
```

## ES2015 Symbol
Symbol最主要的作用就是为对象添加一个独一无二的属性名
```js
const s = Symbol()
console.log(s) // Symbol()
console.log(typeof s) // symbol
console.log(Symbol() === Symbol()) // false
console.log(Symbol() == Symbol()) // false
console.log(Symbol('a')) // Symbol(a)
console.log(Symbol('b')) // Symbol(b)

const obj = {
  [Symbol()]: 123
}
obj[Symbol()] = 456
console.log(obj) // { [Symbol()]: 123, [Symbol()]: 456 }
```

Symbol还可以实现对象的私有成员
```js
const name = Symbol()
const person = {
  [name]: 'zhang',
  say () {
    console.log(`my name is ${this[name]}`)
  }
}

person.say() // my name is zhang
```

Symbol相关补充：
  - 使用给定的key搜索现有的symbol，如果找到则返回该symbol。否则将使用给定的key在全局symbol注册表中创建一个新的symbol。
    ```js
    console.log(Symbol.for('foo') === Symbol.for('foo')) // true
    console.log(Symbol.for(true) === Symbol.for('true')) // true， 这里的标识符会自动转换为字符串
    ```
    
  - 对象里的symbol属性无法被for...in遍历
  - Object.keys()方法无法列出对象中的symbol属性
  - 获得对象里的symbol属性可以通过Object.getOwnPropertySymbols()方法

## ES2015 for...of 循环
可以遍历所有类型的数据结构
```js
const arr = [100, 200, 300, 400]

// for...of可以使用break关键字中断循环，forEach()不能
for (const item of arr) {
  if (item > 200) {
    break
  }
  console.log(item)
} // 100 200

const s = new Set([1, 2, 3])
for (const item of s) {
  console.log(item)
} // 1 2 3

const m = new Map()
m.set('foo', 123)
m.set('baz', 456)

for (const [key, val] of m) {
  console.log(key, val)
}
// foo 123
// baz 456
```

## ES2015 可迭代接口
`for...of`循环内部的工作原理
```js
const s = new Set([1, 2, 3])

const interator = s[Symbol.iterator]()

console.log(interator.next()) // { value: 1, done: false }
console.log(interator.next()) // { value: 2, done: false }
console.log(interator.next()) // { value: 3, done: false }
console.log(interator.next()) // { value: undefined, done: true }
```

## ES2015 实现可迭代接口
```js
const obj = {
  store: ['a', 'b', 'c'],
  [Symbol.iterator]: function () {
    let index = 0
    const self = this
    return {
      next: function () {
        index++
        return {
          value: self.store[index - 1],
          done: index > self.store.length ? true : false
        }
      }
    }
  }
}

for (const item of obj) {
  console.log(item)
} // a b c
```

## ES2015 迭代器设计模式
迭代器模式的核心是对外提供统一遍历接口，让外部不用关心数据的内部结构是怎样的
```js
const todos = {
  life: ['吃饭', '睡觉', '打豆豆'],
  learn: ['语文', '数学', '英语'],
  work: ['写代码'],
  [Symbol.iterator]: function () { // 迭代器实现
    let index = 0
    const list = [...this.life, ...this.learn, ...this.work]

    return {
      next () {
        return {
          value: list[index],
          done: index++ >= list.length
        }
      }
    }
  }
}

for (const item of todos) {
  console.log(item)
}
// 吃饭 睡觉 打豆豆 语文 数学 英语 写代码
```

## ES2015 生成器
引入生成器是为了在复杂的代码中避免异步编程中回调嵌套过深的问题，从而提提更好的异步编程解决方案。
```js
function * foo () {
  console.log('111')
  yield 100
  console.log('222')
  yield 200
  console.log('333')
  yield 300
}

const generator = foo() // 生成器对象

console.log(generator.next()) // 111 { value: 100, done: false }
console.log(generator.next()) // 222 { value: 200, done: false }
console.log(generator.next()) // 333 { value: 300, done: false }
console.log(generator.next()) // { value: undefined, done: true }
```

## ES2016 生成器的应用
```js
// 案例1：发号器
function * createIdMaker () {
  let id = 1
  while (true) {
    yield id++
  }
}

const idMaker = createIdMaker()

console.log(idMaker.next().value)
console.log(idMaker.next().value)
console.log(idMaker.next().value)
console.log(idMaker.next().value)

// 案例2：使用Generator函数实现 iterator方法
const todos = {
  life: ['吃饭', '睡觉', '打豆豆'],
  learn: ['语文', '数学', '英语'],
  work: ['写代码'],
  [Symbol.iterator]: function * () { // 迭代器实现
    const list = [...this.life, ...this.learn, ...this.work]

    for (const item of list) {
      yield item
    }
  }
}

for (const item of todos) {
  console.log(item)
}
```

## ES2016 概述
```js
// Array.prototype.includes
console.log([1, 2, 3, 3].includes(3)) // true
console.log([1, 2, 3, NaN].includes(NaN)) // true

// Math.pow()
console.log(Math.pow(2, 3)) // 8
```

## ES2017 概述
- Object.values()
- Object.entries() 以二维数组的方式返回键值对
    ```js
    // 将对象转换成Map对象
    const person = {
      name: 'zhang',
      age: 17
    }
    
    console.log(new Map(Object.entries(person))) // Map { 'name' => 'zhang', 'age' => 17 }
    ```
- Object.getOwnPropertyDescriptors() 方法用来获取一个对象的所有自身属性的描述符。
    ```js
    const person = {
      name: 'zhang',
      age: 17
    }
    
    const person2 = Object.defineProperties({}, Object.getOwnPropertyDescriptors(person))
    
    person2.name = 'li'
    
    console.log(person) // { name: 'zhang', age: 17 }
    console.log(person2) // { name: 'li', age: 17 }
    ```
- String.prototype.padStart和String.prototype.padEnd，主要用作格式化输出
    ```js
    const books = {
      html: 3,
      css: 15,
      javascript: 138
    }
    
    for (const [key, val] of Object.entries(books)) {
      console.log(key, val)
    }
    // html 3
    // css 15
    // javascript 138
    
    for (const [key, val] of Object.entries(books)) {
      console.log(key.padEnd(16, '-'), val.toString().padStart(3, '0'))
    }
    // html------------ 003
    // css------------- 015
    // javascript------ 138
    ```
- 允许函数参数末尾加`,`，如：function (a, b,)
