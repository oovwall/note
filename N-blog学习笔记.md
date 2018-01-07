# N-blog学习笔记

### semver 语义化版本
semver 格式：主版本号.次版本号.修订号。版本号递增规则如下：

- 主版本号：做了不兼容的 API 修改
- 次版本号：做了向下兼容的功能性新增
- 修订号：做了向下兼容的 bug 修正

### supervisor
supervisor会监听当前目录下node和js后缀文件，当这些文件发生改动时，supervisor会自动重启程序

```powershell
npm i -g supervisor
```
用supervisor代替node，如：`supervisor index.js`


### ejs模板引擎
- <% code %>：运行 JavaScript 代码，不输出
- <%= code %>：显示转义后的 HTML内容
- <%- code %>：显示原始 HTML 内容

```javascript
// 设置模板目录
app.set('views', path.join(__dirname, 'views'))
// 设置模板引擎
app.set('view engine', 'ejs')

// 设置静态文件目录
app.use(express.static(path.join(__dirname, 'public')))

// 设置模板全局常量
app.locals.blog = {
  title: pkg.name,
  description: pkg.description
}
```

```javascript
// 渲染ejs模板
res.render('posts', {
  version: pkg.version
})
```

#### includes

```ejs
<%- include('header') %>
```

### ESLint
ESLint 是一个代码规范和语法错误检查工具。使用 ESLint 可以规范我们的代码书写，可以在编写代码期间就能发现一些低级错误。

全局安装 eslint：

```powershell
npm i eslint -g
```

运行：

```powershell
eslint --init
```

初始化 eslint 配置，依次选择：

- -> Use a popular style guide
- -> Standard
- -> JSON

eslint 会创建一个 .eslintrc.json 的配置文件，同时自动安装并添加相关的模块到 devDependencies。这里我们使用 Standard 规范，其主要特点是不加分号。

### 登录中间件
```javascript
// middlewares/check.js
module.exports = {
  checkLogin: (req, res, next) => {
    if (!req.session.user) {
      req.flash('error', '未登录')
      return res.redirect('/signin')
    }
    next()
  },
  checkNotLogin: checkNotLogin = (req, res, next) => {
    if (req.session.user) {
      req.flash('error', '已登录')
      return res.redirect('back')   // 返回之前的页面
    }
    next()
  }
}

// 引用
const checkLogin = require('../middlewares/check').checkLogin

// 加入检查中间件，如果不满足条件则无法执行回调函数（自己的理解，可能并不是正确的或专业的说法）
router.get('/create', checkLogin, (req, res, next) => {
  res.send('发表文章页')
})
```

### 表单提交
form 表单要添加 `enctype="multipart/form-data"` 属性才能上传文件

接受普通文本字段：

`req.fields.NAME`     NAME为form表单的字段name属性

接受文件字段：

`req.files.NAME.path`       path为上传后文件的路径


## express 学习
console.log() 技巧

```javascript
console.log('Example app listening at http://%s:%s', host, port);
```
### express应用的本地变量（全局变量）设置
```javascript
// 引用的时侯直接用title变量即可
app.locals.title = 'My App';
app.locals.email = 'me@myapp.com';

// 例：
app.locals.blog = {
  title: pkg.name,
  description: pkg.description
}
```

## MAC清除进程方法
```bash
lsof -i:3001            # 查看什么进程占用端口
kill PID                # 杀掉占用端口的PID进程号
```



## Node.js程序在Heroku布署
### GitHub上的repository布署
1. 在Heroku的`Dashboard`上`Create new app`，填入app名称。
2. 在`Deployment method`的列表中选择`GitHub`，登入GitHub账号之后输入repository名称，点击`search`后指定repository项目。
3. 在 `Automatic deploys`对应的栏目中点击`Enable Automatic Deploys` 则可以开启自动布署。
4. 在`Manual deploy`对应的栏目中点击`Deploy Branch`经过编译，最后提示 **Your app was successfully deployed.** ，则编译成功！
5. 点击`View`键即可看到你编写Node.js程序

>  注意：Node.js程序的package.json文件中的scripts属性一定要设start属性，作为程序入口，如：`{"start": "node index"}`
