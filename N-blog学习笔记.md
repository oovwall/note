# N-blog学习笔记

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

#### includes

```js
<%- include('haeder') %>
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