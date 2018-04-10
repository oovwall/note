# CentOS学习笔记

## yum
Yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装

## Mac终端使用SSH远程连接到Linux服务器
1. 在Dock的终端上点击右键 > 新建远程连接... 
2. 选择安全Shell（ssh），在右边的方框下点击+号输入服务器的IP，如图：
![](https://ws2.sinaimg.cn/large/006tNc79gy1fo91axi8iwj30ne0nmtbu.jpg?aN4syttKemFMRef)
3. 点击确认后点击下面的连接按钮，在终端窗口弹出的提示中输入yes，然后输入密码，连接成功！


## CentOS部署Node.js程序
### 安装Node.js
#### 用yum安装gcc
```powershell
[sudo] yum -y install gcc make gcc-c++ openssl-devel wget
```

#### 安装Node.js v9.4.0
1. 开始安装Node.js，先进入`/usr/src`文件夹，这个文件夹通常用来存放软件源代码:
```powershell
cd /usr/src
```

2. 从Node.js的站点中获取压缩档源代码
```powershell
wget https://nodejs.org/dist/v9.4.0/node-v9.4.0.tar.gz
```

3. 然后解压提取文件
```powershell
tar zxvf node-v9.4.0.tar.gz         # tar -xvfz node-v9.4.0.tar.gz 解压一个gzip格式的压缩包
```

4. 执行后会生成node-v9.4.0文件夹，cd进入,里面有个configure文件，配置并编译#这步执行得比较久，可以先去喝杯咖啡
```powershell
cd node-v9.4.0
./configure
make
```

5. 安装
```powershell
make install
```

6. 最后使用`node -v`检查是否安装成功。

### 安装MongoDB
1. 配置包管理系统（yum）：创建一个`/etc/yum.repos.d/mongodb-org-3.6.repo`文件，这样你就可以直接使用yum来安装MongoDB。
```powershell
cd /etc/yum.repos.d
yum -y install vim              # 安装vim，如果已完装则跳过该步骤
vi mongodb-org-3.6.repo         # 进入vi的一般模式
# 在一般模式之中，只要按下 i, o, a 等字符就可以进入输入模式了！
# 在编辑模式当中，你可以发现在左下角状态栏中会出现 –INSERT- 的字样，那就是可以输入任意字符的提示。
```
把下列内容保存至`mongodb-org-3.6.repo`中
```powershell
[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```
按`ESC`键回到vi一般模式后，按`:wq`保存退出

2. 安装MongoDB服务器：
```powershell
[sudo] yum install mongodb-org -y
```
安装完成用`mongod --version`查看版本

3. MongoDB的基本操作：
```powershell
[sudo] service mongod start            # 启动MongoDB
[sudo] service mongod stop             # 停止MongoDB
[sudo] service mongod restart          # 重启MongoDB
mongo                                  # MongoDB shell
```

## CentOS部署vue-cli构建工具程序
### 部署前准备
1. 安装必要的软件，参考上面的[CentOS部署Node.js程序](#CentOS部署Node.js程序)
2. 安装Node.js
3. 安装Git
    ```powershell
    $ yum install git
    ```
4. 用`git clone`项目文件
### 启动服务
#### 用`http-server`启动
1. 安装 `http-server`
    ```powershell
    $ npm install http-server -g
    ```
2. 进入项目文件的dist目录下，运行`http-server`启动
    ```powershell
    $ cd /home/auto-test-vue/dist
    $ http-server -p 8098
    ```
3. 这时可以通过 http://127.0.0.1:8098 访问。
    如果其它机器不能访问则需关闭防火墙
    ```powershell
    $ systemctl stop firewalld.service          #停止firewall
    $ systemctl disable firewalld.service       #禁止firewall开机启动
    $ firewall-cmd --state      # 查看默认防火墙状态（关闭后显示notrunning，开启后显示running）
    ```
    
### 用`nginx`启动
1. 安装`nginx`
    ```powershell
    $ cd /usr/src 
    $ wget http://nginx.org/download/nginx-1.13.11.tar.gz       # 下载nginx
    $ tar zxvf nginx-1.13.11.tar.gz                             # 解压
    $ cd nginx-1.13.11
    $ ./configure               # 配置
    $ make                      # 编译
    $ make install              # 安装
    ```
2. 启动并校验`nginx`安装情况
   ```powershell
   $ cd /usr/local/nginx/sbin
   $ ./nginx                    # 启动
   $ ps -ef | grep nginx        # 查看nginx进程，如果存在则安装成功
   ```
3. 配置`nginx`静态文件访问服务
   ```powershell
   $ cd /usr/local/nginx/conf   # 找到配置文件
   $ vim nginx.conf             # 修改nginx.conf文件
   ```
   
   ```nginx
   server {
       listen       80;
       server_name  localhost;

       # ...

       location / {
           root   /home/auto-test-vue/dist;   # 修改root指向
           index  index.html index.htm;
       }
       
       # ...
   }
   ```
   
4. 按`Esc`键，输入`:wq`保存文件后，运行
   ```powershell
   $ cd /usr/local/nginx/sbin
   $ ./nginx -s reload                        # 重启nginx服务器
   ```
   
- nginx常用命令
  ```powershell
  $ cd /usr/local/nginx/sbin
  $ ./nginx                         # 启动
  $ ./nginx -s stop                 # 强制关闭
  $ ./nginx -s reload               # 重启
  ```


## CentOS设置相关
### CentOS安装五笔输入法
1. 加开终端，输入`su root`，再输入密码。
2. 登录root后，再键入`yum remove ibus`，移除现有的输入法框架。移除过程中会弹出询问是否真的移除，输入`y`继续。
3. 移除完毕后，再键入`yum install ibus ibus-table`，回车后重新安装输入法框架。同样，在执行过程中会询问是否确认安装，键入`y`回车继续。
4. 输入`yum install ibus ibus-table-wubi`安装极点五笔输入法，并且在执行过程中键入`y`确认下载和安装。
5. 安装成功后，键入`exit`退出root帐号，然后再关闭终端窗口。
6. 在`设置 > 区域和语言`中添加输入源。如果没有出现五笔则重启CentOS。

### CentOS网络工具
#### netstat
##### 安装
```powershell
[sudo] yum install net-tools -y
```

##### 用例
列出所有端口
```powershell
netstat -ntlp
```

#### lsof
##### 安装
```powershell
[sudo] yum install lsof -y
```

##### 用例
查看80端口占用情况使用
```powershell
lsof -i tcp:80
```


## Hyper-v相关
虚拟机和系统之前鼠标切换用：`ctrl + alt + ← `

## 相关链接
- [CentOS 7部署Node.js+MongoDB：在VPS上从安装到Hello world](http://blog.csdn.net/azureternite/article/details/52349326)
- [CentOS 新建、删除、移动、复制等命令](http://blog.csdn.net/lpdx111/article/details/16877725)
- [vim常用命令总结](https://www.cnblogs.com/yangjig/p/6014198.html)
