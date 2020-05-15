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

## ES2015 Reflect

## ES2015 静态方法

## ES2015 Set

## ES2015 Map

## ES2015 Symbol

## ES2015 for...of 循环

## ES2015 生成器

## ES2016 概述

## ES2017 概述
