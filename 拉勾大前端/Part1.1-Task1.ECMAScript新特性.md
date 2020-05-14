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


## ES2015 对象的数组解构

## ES2015 带标签的模板字符串

## ES2015 Proxy

## ES2015 Reflect

## ES2015 静态方法

## ES2015 Set

## ES2015 Map

## ES2015 Symbol
