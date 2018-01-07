[TOC]
# Javascript知识盲点扫描

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
