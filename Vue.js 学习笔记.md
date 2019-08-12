# Vue.js 学习笔记

## Vue CLI相关笔记
### @vue/cli 3.x
#### 安装
```powershell
npm install -g @vue/cli
```

#### 初始化项目
```powershell
vue create project-name
```

#### 现有项目中安装插件（如ESLint, vue-router, vuex）等
```powershell
vue add @vue/eslint
vue add router
vue add vuex
```

#### package.json里的ESLint配置
```json
{
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "@vue/standard"
    ],
    "rules": {
      "generator-star-spacing": "off",
      "no-debugger": "off",
      "no-console": "off",
      "indent": 0
    },
    "parserOptions": {
      "parser": "babel-eslint"
    }
  }
}
```

### Vue CLI相关故障解决
- 浏览器报`Invalid Host/Origin header`问题，见1
  > 这是webpack本身出于安全考虑，因为不检查主机的应用程序容易受到DNS重新绑定攻击。但是，在我们的开发环境下，可以禁用掉disableHostCheck这一配置项。
- 如果想在serve的时候在console处输出你配置的域名信息，可在package.json里修改改serve命令，如：`vue-cli-service serve --public vue-admin.oovwall.com`，或更改devServer配置，见2
  ```
  devServer: {
    contentBase: resolve(__dirname, '../dist'),
    host: '0.0.0.0',
    port: 4000,
    disableHostCheck: true, //  1. 新增该配置项
    public: 'vue-admin.oovwall.com',    // 2. console面板上会输出：- Network: http://vue-admin.oovwall.com/
  },
  ```

### 项目架构相关内容
#### 散装知识
- router-link 设置tag属性
```vue
<router-link tag="li" :to="/url"></router-link>
```

- vue监听路由变化
```vuejs
new Vue({
  watch: {
    $route () {
      // code
    }
  }
})
```

- 判断是否为空对象
```javascript
Object.keys(obj).length > 0
```

- map() 新用法
```javascript
let books = ['vue', 'react', 'angularjs']
books.map(item => {
  if (item === 'angularjs') {
    return 'angular'      // 查到相同条件的item，替换该条件的内容
  }
  return item             // 返回此item
})

/* 输出
["vue", "react", "angular"]
*/
```

- 多个接口同时发请求，用axios.all()
```javascript
// 执行多个并发请求
// api
export let getAll = () => {
  return axios.all([func1(), func2()])
}

// 调用页面
async getData () {
  let [v1, v2] = await getAll()
  
  // 操作
  this.v1 = v1
  this.v2 = v2
}
```

- keep-alive 页面缓存应用
```javascript
const routes = [
  {
    path: '/main',
    component: Main,
    meta: {
      keepAlive: true
    }
  }
]
```

```vue
<keep-alive>
    <!--缓存的router-->
    <router-view v-if="$route.meta.keepAlive"/>
</keep-alive>
<router-view v-if="!$route.meta.keepAlive"/>
```

- 为每个页面更改网页<title>；路由拦截
```javascript
const routes = [
  {
    path: '/main',
    component: Main,
    meta: {
      title: '首页'
    }
  }
]

router.beforeEach((to, from, next) => {
  // 为每个页面更改网页<title>
  document.title = to.meta.title
  
  // 路由拦截
  if (to.path === '/list') {
    next({ path: '/add' })
  } else {
    next()
  }
})
```

- Element UI传统方式提交带文件表单
  1. 方法一：自写方法用axios发送提交表单
      ```vue
      <template>
        <el-upload
          ref="upload"
          action="/api/cgFinance/cgt/user/saveFile"
          :limit="1"
          :on-change="selectFile"
          :auto-upload="false">
          <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
          <div slot="tip" class="el-upload__tip">只能上传zip文件</div>
        </el-upload>
      </template>

      <script>
      export default {
        methods: {
          selectFile (file) {
            this.form2.file = file.raw
          },
          async postFileToBank () {
            let formData = new FormData()
            formData.append('userId', this.form2.userId)
            formData.append('email', this.form2.email)
            formData.append('file', this.form2.file)

            let { data } = await postFileToBank(formData)
            if (data.code === 200) {
              this.$notify.success({
                message: data.msg
              })
            } else if (data.code === 401 || data.code === -100) {
              redirectLogin(this, data.msg)
            } else {
              this.$notify.error({
                message: data.msg
              })
            }
          }
        }
      }
      </script>
      ```

  1. 方法二：用`el-upload`组件自带的小form带上其它属性值一起提交。
      ```vue
      <el-upload
        ref="file"
        action="/api/cgFinance/cgt/user/saveFile"
        :headers="{
          token: token
        }"
        :data="{
          email: this.form2.email,
          userId: this.form2.userId
        }"
        :on-success="success"
        :limit="1"
        :file-list="form2.fileList"
        :auto-upload="false">
        <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
        <div slot="tip" class="el-upload__tip">只能上传zip文件</div>
      </el-upload>

      <script>
      export default {
        methods: {
          submit () {
            this.$refs.file.submit()
          },
          success (res, file, fileList) {
            // 请求成功的回调
          }
        }
      }
      </script>
      ```
  
- 判断传入的节点是否为该节点的后代节点
> 一般用来做弹出窗时点击外部关闭，在弹出窗内不关闭，弹出窗外点击页面关闭，这也是js中的Node节点知识
```vue
<script>
// this.$refs.picker.contains(e.target)
export default {
  methods: {
    clickOutSide (e) {
      if (this.pickerVisible && !this.$refs.picker.contains(e.target)) {
          this.pickerVisible = false
      }
    }
  },
  created() {
    document.addEventListener('mouseup', this.clickOutSide)
  },
  destoryed() {
    document.removeEventListener('mouseup', this.clickOutSide)
  }
}
</script>
```

## Vue.js Fundamentals
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

## 其它课程笔记
### 列表删除某一项元素
```vuejs
new Vue({
  el: '#app',
  method: {
    remove(todo) {
      this.todos = this.todos.filter(item => item !== todo)
    }
  }
})
```

### 自定义指令
```vue
<template>
  <div id="app">
    <div v-color="flag">变色</div>
    <div v-drag>拖我</div>
  </div>
</template>

<script>
let app = new Vue({
  directives: {
    color(el, bindings) {
      el.style.background = bindings.value
    },
    drag(el) {
      el.onmousedown = function(e) {
        let disx = e.pageX - el.offsetLeft
        let disx = e.pageY - el.offsetTop
        
        document.onmousemove = function(e) {
          el.style.left = e.pageX - disx + 'px'
          el.style.top = e.pageY - disy + 'px'
        }
        
        document.onmouseup = function() {
          document.onmousemove = document.onmouseup = null
        }
        
        e.preventDefault()
      }
    }
  },
  data: {
    flag: 'red'
  }
})
</script>

<style scoped>
  .flag {
    position: absolute;
    width: 100px;
    height: 100px;
    color: lightseagreen;
  }
</style>
```

