# Express框架学习笔记
## 安装Express
```powershell
$ npm install express --save
```

## 简单的 Express 路由
路由（Routing）是由一个 URI（或者叫路径）和一个特定的 HTTP 方法（GET、POST 等）组成的，涉及到应用如何响应客户端对某个网站节点的访问。

路由的定义由如下结构组成：app.METHOD(PATH, HANDLER)。其中，app 是一个 express 实例；METHOD 是某个 HTTP 请求方式中的一个；PATH 是服务器端的路径；HANDLER 是当路由匹配到时需要执行的函数。

```javascript
// 对网站首页的访问返回 "Hello World!" 字样
app.get('/', function (req, res) {
  res.send('Hello World!');
});

// 网站首页接受 POST 请求
app.post('/', function (req, res) {
  res.send('Got a POST request');
});
```

## 利用 Express 托管静态文件
将静态资源文件所在的目录作为参数传递给 express.static 中间件就可以提供静态资源文件的访问了。

```javascript
app.use(express.static('public'));    // public 目录下面的文件就可以访问了。
app.use(express.static('files'));     // 静态资源存放在多个目录下面，可以多次调用 express.static 中间件
```

也可以通过`express.static`把文件放在虚拟目录`/static`下
```javascript
app.use('/static', express.static('public'));

// 访问
// http://localhost:3000/static/css/style.css
```

