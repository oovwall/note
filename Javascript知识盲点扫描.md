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
```html
<input type="button" onclick="showMessage()">
```