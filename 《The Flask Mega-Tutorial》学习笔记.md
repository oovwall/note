# 《The Flask Mega-Tutorial》学习笔记

**Flask**是一个使用 Python 编写的轻量级 Web 应用框架。其 WSGI 工具箱采用 Werkzeug ，模板引擎则使用 Jinja2 。Flask使用 BSD 授权。

[TOC]
## 安装
### 安装python
Windows直接下载安装，命令行输入python生效时，即安装成功。输入exit()退出。

### 创建项目目录
```bash
$ mkdir microblog
$ cd microblog
```

### 创建虚拟环境
创建一个名为venv的虚拟环境。
```bash
$ python -m venv venv
# 第一个“venv”是Python虚拟环境包的名称，第二个是要用于这个特定环境的虚拟环境名称。
```

### 激活虚拟环境
- Mac, Linux用这个命令激活虚拟环境
```bash
$ source venv/bin/activate
(venv) $ _
```

- Windows用这个命令激活虚拟环境
```bash
$ venv\Scripts\activate
(venv) $ _
```

### 安装Flask
```bash
(venv) $ pip install flask
```

验证是否安装成功
```bash
>>> import flask
>>> _
```
如果没有报错则安装成功


## 模板
### 引入模板
在Flask中，模板被编写为单独的文件，存储在应用程序包内的templates文件夹中(`app/templates`)。

```python
from flask import render_template
from app import app

@app.route('/')
@app.route('/index')
def index():
    user = {'username': 'Miguel'}
    return render_template('index.html', title='Home', user=user)
```
`render_template()`函数调用Flask框架原生依赖的Jinja2模板引擎。 Jinja2用render_template()函数传入的参数中的相应值替换{{...}}块。

### 条件语句
```html
{% if title %}
    <title>{{ title }} - Microblog</title>
{% else %}
    <title>Welcome to Microblog!</title>
{% endif %}
```
### 循环
```html
{% for post in posts %}
    <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
{% endfor %}
```

### 模板的继承
母板
```html
{% block content %}{% endblock %}
```

继承母板
```html
{% extends "base.html" %}

{% block content %}
    <h1>Hi, {{ user.username }}!</h1>
    {% for post in posts %}
    <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
    {% endfor %}
{% endblock %}
```