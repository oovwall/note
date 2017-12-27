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
