# Vue.js 学习笔记

### 第二课：入门

**`javascript`**

```javascript
var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello world!'
    }
})
```
**`HTML`**

```html
<div id="app">
    <h1>{{message}}</h1>
</div>
```

在控制台输入 `app.message = '你好'` 则可以改变HTML中的文字

### 第三课

**v-text v-html**

**`javascript`**

```javascript
var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello world!'
    }
})
```
**`HTML`**

```html
<div id="app">
    <h1 v-text="message"></h1>
</div>
```

**v-show** 布尔型，控制元素是否显示

**v-if v-else** 判断条件

**v-pre** 直接输出花括号内容，没有值

**v-once** 直接渲染无法更改

**v-cloak** 程序执行之后再渲染到页面

### 第四课 **v-bind** 数据绑定

**`javascript`**

```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hello world!',
      title: 'Boo ya!',
      url: 'https://cn.vuejs.org/images/logo.png'
    }
  })
```
**`HTML`**

```html
<div id="app">
    <h1 v-bind:title="title">{{message}}</h1>
    <img :src="url" alt="">
</div>
```

### 第五课 **v-for** 循环
```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hello world!',
      todos: [
        { text: 'Learn Vue'},
        { text: 'Like the video'},
        { text: 'Subscribe to DevMarketer'}
      ]
    }
  })
```
**`HTML`**

```html
<div id="app">
    <h1>{{message}}</h1>
    <ul>
        <li v-for="todo in todos">{{ todo.text }}</li>
    </ul>
</div>
```

### 第六课 **`v-model`**
实时双向绑定

```javascript
var app = new Vue({
    el: '#app',
    data: {
      message: 'Hello world!'
    }
  })
```
**`HTML`**

```html
<input type="text" v-model="message">
```

### 第七课 事件处理
**`v-on:click`**
**`@click`**
**`methods`**


### 第八课 **`computed`**

### 第九课 Getter & Setter
 