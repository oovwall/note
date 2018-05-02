# 《HTTP权威指南》读书笔记

## 第一部分 HTTP： Web 的基础
### 第 1 章 HTTP 概述
**MIME类型：**（Multipurpose Internet Mail Extension， 多用途因特网邮件扩展、数据格式标签）MIME 类型是一种文本标记， 表示一种主要的对象类型和一个特定的子类型， 中间由一条斜杠来分隔。*如：Content-type: image/jpeg*

**URI：** 服务器资源名被称为统一资源标识符（Uniform Resource Identifier），URI 有两种形式， 分别称为 URL 和 URN。

**URL：** 统一资源定位符（URL）

**URN：** URI 的第二种形式就是统一资源名（URN）。 URN 是作为特定内容的唯一名称使用的， 与目前的资源所在地无关。 

**HTTP 报文**是由一行一行的简单字符串组成的。 HTTP 报文都是纯文本， 不是二进
  制代码， 所以人们可以很方便地对其进行读写。 
  ![C8sRmT.png](https://s1.ax1x.com/2018/04/28/C8sRmT.png)

**TCP/IP**
用网络术语来说， HTTP 协议位于 TCP 的上层。 HTTP 使用 TCP 来传输其报文数据。 与之类似， TCP 则位于 IP 的上层。
![C86UIg.png](https://s1.ax1x.com/2018/04/28/C86UIg.png)

**IP** 网际协议（Internet Protocol， IP）

- Web的结构组件
    - 代理
        - 代理位于客户端和服务器之间， 接收所有客户端的 HTTP 请求， 并将这些请求转发给服务器（可能会对请求进行修改之后转发）。 
    - 缓存
        - Web 缓存（Web cache） 或代理缓存（proxy cache） 是一种特殊的 HTTP 代理服务器， 可以将经过代理传送的常用文档复制保存起来。 下一个请求同一文档的客户端就可以享受缓存的私有副本所提供的服务了
    - 网关
        - 网关（gateway） 是一种特殊的服务器， 作为其他服务器的中间实体使用。 通常用于将 HTTP 流量转换成其他的协议。
    - 隧道
        - 隧道（tunnel） 是建立起来之后， 就会在两条连接之间对原始数据进行盲转发的HTTP 应用程序。 HTTP 隧道通常用来在一条或多条 HTTP 连接上转发非 HTTP 数据， 转发时不会窥探数据。
    - Agent代理
        - 用户 Agent 代理（或者简称为 Agent 代理） 是代表用户发起 HTTP 请求的客户端程序。
        
### 第 2 章 URL与资源
大多数 URL 方案的 URL 语法都建立在这个由 9 部分构成的通用格式上：
<scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>

### 第 3 章 HTTP报文
#### HTTP报文分类
1. 请求报文（request message）  
    请求报文的格式
    ```
    <method> <request-URL> <version>
    <headers>
    
    <entity-body>
    ```
    
    - 方法（method）  
    客户端希望服务器对资源执行的动作。 是一个单独的词， 比如 GET、 HEAD 或POST。 本章稍后将详细介绍方法。
    - 请求URL（request-URL）  
    命名了所请求资源， 或者 URL 路径组件的完整 URL。 如果直接与服务器进行对话， 只要 URL 的路径组件是资源的绝对路径， 通常就不会有什么问题——服务器可以假定自己是 URL 的主机 / 端口。
    - 版本（version）  
    报文所使用的 HTTP 版本， 其格式看起来是这样的：
    ```
    HTTP/<major>.<minor>
    ```
    其中主要版本号（major） 和次要版本号（minor） 都是整数。 
    - 首部（header）  
    可以有零个或多个首部， 每个首部都包含一个名字， 后面跟着一个冒号（:）， 然后是一个可选的空格， 接着是一个值， 最后是一个 CRLF。 首部是由一个空行（CRLF） 结束的， 表示了首部列表的结束和实体主体部分的开始。 有些 HTTP 版本， 比如 HTTP/1.1， 要求有效的请求或响应报文中必须包含特定的首部。 
    - 实体的主体部分（entity-body）  
    实体的主体部分包含一个由任意数据组成的数据块。 并不是所有的报文都包含实体的主体部分， 有时， 报文只是以一个 CRLF 结束。 

1. 响应报文（response message）  
    响应报文的格式
    
    ```
    <version> <status> <reason-phrase>
    <headers>
    
    <entity-body>
    ```
    
    - 状态码（status-code）  
    这三位数字描述了请求过程中所发生的情况。 每个状态码的第一位数字都用于描述状态的一般类别（“成功”、“出错” 等）。
    - 原因短语（reason-phrase）  
    数字状态码的可读版本， 包含行终止序列之前的所有文本。  原因短语只对人类有意义， 因此， 比如说， 尽管响应行 HTTP/1.0 200 NOT OK 和 HTTP/1.0 200 OK 中原因短语的含义不同， 但同样都会被当作成功指示处理。

​    

## 第二部分 HTTP 结构
## 第三部分 识别、 认证与安全
## 第四部分 实体、 编码和国际化
## 第五部分 内容发布与分发
## 第六部分 附录
