# Javascript知识盲点扫描

[TOC]

## 数组 Array
### forEach, for...in, for, for...of 的区别
1. forEach
    - 声明式（不关心如何实现）
    - 不支持 return
```javascript
let arr = [1, 2, 3, 4, 5]
arr.b = '100'
arr.forEach((item) => {
  console.log(item)
})

// 1
// 2
// 3
// 4
// 5
```

2. for...in
    - key 会变成字符串类型，包括数组的私有属性也可以打印出来
```javascript
let arr = [1, 2, 3, 4, 5]
arr.b = '100'
for (let key in arr) {
  console.log(`${typeof key}:${key}`)
}

// string:0
// string:1
// string:2
// string:3
// string:4
// string:b
```

3. for...of
    - 支持 return
    - 只输出数组元素（不能遍历对象）
```javascript
let arr = [1, 2, 3, 4, 5]
arr.b = '100'
for (let key of arr) {
  console.log(`${typeof key}:${key}`)
}

// number:1
// number:2
// number:3
// number:4
// number:5
```

### Array.prototype.map()
将原有的数组经提供的函数处理后创建一个新的数组。
```javascript
let arr = [1, 2, 3].map((item) => `<li>${item}</li>`)
console.log(arr.join(''))

// <li>1</li><li>2</li><li>3</li>
```

### Array.prototype.includes() 和 Array.prototype.find()
- **Array.prototype.includes()**  
    判断一个数组是否包含一个指定的值，返回 true 或 false
- **Array.prototype.find()**  
    返回数组中满足提供函数的第一个元素的值（找到即停止循环，如果没有找到则返回 undefined）
    
```javascript
let arr = [1, 2, 3, 4, 55, 555]
console.log(arr.includes(5))

let result = arr.find((item) => item.toString().indexOf(5) > -1)
console.log(result)

// false
// 55
```

### Array.prototype.reduce()
reduce() 方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。

数组求和
```javascript
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));

// 10
```

将二维数组转化为一维
```javascript
const flattened = [[0, 1], [2, 3], [4, 5]].reduce(
 ( acc, cur ) => acc.concat(cur),
 []
);

// [0, 1, 2, 3, 4, 5]
```

计算数组中每个元素出现的次数
```javascript
const names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

const countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  } else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```


## 对象 Object
### Object.keys
返回一个将对象的 key 作为新的数组的方法。
```javascript
let obj = {
  school: 'mit',
  age: '22'
}
console.log(Object.keys(obj))

for (let val of Object.keys(obj)) {
  console.log(obj[val])
}

// ["school", "age"]
// mit
// 22
```

## 如何更改 this 指向
1. call, apply, bind
2. let that = this
3. =>

## Error
自定义错误范例
```javascript
function process (values) {
  if (!(values instanceof Array)) {
    throw new Error ('process(): Argument must be an array.')
  }

  values.sort()
}
```

## DOM
定义：DOM（文档对象模型）是针对HTML和XML文档的一个API（应用程序接口）。

功能：DOM描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。

<html>称之为文档元素。每个文档只有一个文档元素，在HTML页面中，文档元素始终都是<html>元素。

### Document类型
```javascript
document.documentElement          // <html>元素
document.body                     // <body>元素
document.title                    // 网页标题

document.URL                      // 取得完整的URL
document.domain                   // 取得域名，可以设置主域名
document.referrer                 // 取得来源页面的URL

document.anchors                  // 包含文档中所有带name特性的<a>元素
document.applets                  // 包含文档中所有的<applet>元素
document.forms                    // 包含文档中所有的<form>元素，document.getElementsByTagName('form')结果相同
document.images                   // 包含文档中所有的<img>元素，document.getElementsByTagName('img')结果相同
document.links                    // 包含文档中所有带href特性的<a>元素
```

#### 查找元素
元素的name属性查找
```html
<img src="myimage.gif" name="myImage" alt="">
```
```javascript
var images = document.getElementsByTagName('img')

// 只会取得一项，并不会返回HTMLCollection
var myImage = images.namedItem('myImage')
// 或
var myImage = images['myImage']
```

### Element类型
```html
<div id="myDiv" class="bd" title="Body text" lang="en" dir="ltr"></div>
```
```javascript
// HTML元素的标准特性（可赋值修改）
var div = document.getElementById('myDiv')
div.id                      // 'myDiv'
div.className               // 'bd'
div.title                   // 'Body text'
div.lang                    // 'en'
div.dir                     // 'ltr'

// 取得特性
div.getAttribute('id')                      // 'myDiv'
div.getAttribute('class')                   // 'bd'
div.getAttribute('title')                   // 'Body text'
div.getAttribute('lang')                    // 'en'
div.getAttribute('dir')                     // 'ltr'
div.getAttribute('data-my-option')          // 可以取得HTML元素自定义特性，不区分大小写

// 设置特性
div.setAttribute('id', 'someOtherId')
div.setAttribute('class', 'ft')
div.setAttribute('title', 'Some other text')
div.setAttribute('lang', 'fr')
div.setAttribute('dir', 'rtl')

// 删除特性
div.removeAttribute('class')
```

创建元素
```javascript
var div = document.createElement('div')
div.id = 'myNewDiv'
div.className = 'box'
```

### Text类型
创建一个完整的<div>元素并添加到页面
```javascript
var div = document.createElement('div')
var textNode = document.createTextNode('Hello world!')
div.appendChild(textNode)
document.body.appendChild(div)
```
### DocumentFragment类型
```javascript
var fragment = document.createDocumentFragment()
var textNode = document.createTextNode('Hello world!')
fragment.appendChild(textNode)
document.body.appendChild(fragment)
```

## 事件
### 事件冒泡
事件由最具体的元素接收，然后逐级向上传播到不具体的节点（文档）。
### 事件捕获
不太具体的节点更早接收到事件，而具体的节点最后接收到事件。
### 事件处理程序
响应某个事件的函数叫`事件处理程序`

#### HTML事件处理程序
```html
<input type="button" onclick="showMessage()">
```
#### DOM0级事件处理程序
通过Javascript指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。
```javascript
var btn = document.getElementById('myBtn')
btn.onclick = function() {
  alert('Clicked')
}
```
> 使用DOM0级方法指定的事件处理程序被认为是元素的方法。因此，这时侯的事件处理程序是在元素的作用域中运行的，程序中的this引用当前元素。

#### DOM2级事件处理程序
DOM2级事件定义了两个方法，用于处理指定和删除事件处理程序的操作：addEventListener()和removeEventListener()。所有DOM节点都包含这两个方法，并且他们接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值（true: 在捕获阶段调用事件处理程序，false: 在冒泡阶段调用事件处理程序）。
```javascript
var btn = document.getElementById('myBtn')
btn.addEventListener('click', function () {
  alert(this.id)
}, false)
```
使用removeEventListener()来移除事件处理程序时，必须为同一个函数名（不能是匿名函数）

#### IE事件处理程序
IE实现了与DOM中类似的两个方法：attachEvent()和detachEvent()。这两个方法接受两个相同的参数：事件处理程序名称与事件处理程序函数。添加的事件都会被添加到冒泡阶段。
```javascript
var btn = document.getElementById('myBtn')
btn.attachEvent('onclick', function () {
  alert('Clicked')
})
```
> 注意：
> 1. 在使用attachEvent()方法的情况下，事件处理程序会在全局作用域中运行。因此`this === window`。
> 2. 与DOM方法不同的是，这些事件处理程序不是以添加它们的顺序执行，而是以相反的顺序被触发。

## ES6 Promise
> 所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步的操作）结果。

示例 
```javascript
let promise = new Promise((resolve, reject) => {
  // 构造函数中的参数是一个函数，该函数生成成功或失败的状态，由实例promise去设定成功或失败的回调函数

  console.log('Promise')
  resolve()     // 如果替换成reject()则输出failure
  
})

promise.then(() => {
  console.log('success')
}, () => {
  console.log('failure')
})

console.log('HI')
```

一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。
```javascript
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```
一般总是建议，Promise 对象后面要跟catch方法，这样可以处理 Promise 内部发生的错误。catch方法返回的还是一个 Promise 对象，因此后面还可以接着调用then方法。

## CommonJS规范
CommonJS每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

CommonJS规范规定，每个模块内部，`module`变量代表当前模块。这个变量是一个对象，它的`exports`属性（即`module.exports`）是对外的接口。加载某个模块，其实是加载该模块的`module.exports`属性。
```javascript
var x = 5;
var addX = function (value) {
  return value + x;
};

// 通过module.exports输出变量x和函数addX。
module.exports.x = x;
module.exports.addX = addX;
```
CommonJS模块的特点:
1. 所有代码都运行在模块作用域，不会污染全局作用域。
2. 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
3. 模块加载的顺序，按照其在代码中出现的顺序。

### module对象

> * module.id 模块的识别符，通常是带有绝对路径的模块文件名。
> * module.filename 模块的文件名，带有绝对路径。
> * module.loaded 返回一个布尔值，表示模块是否已经完成加载。
> * module.parent 返回一个对象，表示调用该模块的模块。
> * module.children 返回一个数组，表示该模块要用到的其他模块。
> * module.exports 表示模块对外输出的值。 

#### module.exports属性
module.exports属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取module.exports变量。

#### exports变量
对外输出模块接口时，可以向exports对象添加方法。
```javascript
exports.area = function (r) {
  return Math.PI * r * r;
};

exports.circumference = function (r) {
  return 2 * Math.PI * r;
};

// 不能和module.exports混用，因为module.exports会被重新赋值。
```
~~module.exports = function (x){ console.log(x);};~~

### require命令
#### 删除模块缓存
```javascript
// 删除指定模块的缓存
delete require.cache[moduleName];

// 删除所有模块的缓存
Object.keys(require.cache).forEach(function(key) {
  delete require.cache[key];
})
```
