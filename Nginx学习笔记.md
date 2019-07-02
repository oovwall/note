# Nginx 学习笔记
> nginx是一款轻量化的web服务器。相较于Apache具有占有内存少，并发高等优势。使用epoll模型，nginx的效率很高。并且可以热升级。

## Homebrew
Homebrew是macOS 缺失的软件包管理器。

#### Homebrew 能干什么?
使用 Homebrew 安装 Apple 没有预装但 你需要的东西。

#### 安装Homebrew
``` powershell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
#### brew 常用命令
``` powershell
brew -help				# 查看brew的帮助
brew list 				# 显示已安装的程序
brew install []			# 安装软件
brew uninstall 	[] 		# 卸载软件
brew update 			# 更新Homebrew
brew home [] 			# 用浏览器打开相关程序的页面
brew info [] 			# 显示包信息
brew outdated			# 查看那些已安装的程序需要更新
brew upgrade [] 		# 更新某个已安装的程序
brew cleanup [] 		# 删除某个程序
```

## 安装Nginx
### Mac安装Nginx
``` powershell
$ brew install nginx
```

### CentOS安装Nginx
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
3. 如果无法在终端使用`nginx`命令，则需配置环境变量
   ```powershell
   vi /etc/profile
   ```
   
   在`/etc/profile`文件末尾添加下面两行代码，按`Esc`键输入`:wq`保存退出
   ```
   PATH=$PATH:/usr/local/nginx/sbin
   export PATH
   ```
   输入`reboot`重启服务器生效。
   

## 使用
``` powershell
$ sudo nginx						# 启动 nginx
$ sudo nginx -s reload  			# 修改 nginx.conf 后，重载配置文件
$ sudo nginx -s stop 				# 停止 nginx 服务器
```

## 配置
在Finder上右键点击前往文件夹 */usr/local/etc/nginx* 可到nginx文件夹

``` nginx
# 运行用户
user www-data;    
# 启动进程,通常设置成和cpu的数量相等
worker_processes  1;

# 全局错误日志及PID文件
error_log  /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

# 工作模式及连接数上限
events {
    use   epoll;             # epoll是多路复用IO(I/O Multiplexing)中的一种方式,但是仅用于linux2.6以上内核,可以大大提高nginx的性能
    worker_connections 1024; # 单个后台worker process进程的最大并发链接数
    # multi_accept on; 
}

# 设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
    #设定mime类型,类型由mime.type文件定义
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    access_log    /var/log/nginx/access.log;

    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，对于普通应用，
    #必须设为 on,如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile        on;
    #tcp_nopush     on;

    #连接超时时间
    #keepalive_timeout  0;
    keepalive_timeout  65;
    tcp_nodelay        on;
    
    #开启gzip压缩
    gzip  on;
    gzip_disable "MSIE [1-6]\.(?!.*SV1)";

    #设定请求缓冲
    client_header_buffer_size    1k;
    large_client_header_buffers  4 4k;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;

    #设定负载均衡的服务器列表
    upstream mysvr {
      #weigth参数表示权值，权值越高被分配到的几率越大
      #本机上的Squid开启3128端口
      server 192.168.8.1:3128 weight=5;
      server 192.168.8.2:80  weight=1;
      server 192.168.8.3:80  weight=6;
    }

   # 基本设置
    server {
        #侦听80端口
        listen       80;
        #定义使用www.xx.com访问
        server_name  www.xx.com;

        #设定本虚拟主机的访问日志
        access_log  logs/www.xx.com.access.log  main;

        #默认请求
        location / {
            root   /root;      #定义服务器的默认网站根目录位置
            index index.php index.html index.htm;   #定义首页索引文件的名称

            fastcgi_pass  www.xx.com;
            fastcgi_param  SCRIPT_FILENAME  $document_root/$fastcgi_script_name; 
            include /etc/nginx/fastcgi_params;
        }

        # 定义错误提示页面
        error_page   500 502 503 504 /50x.html;  
            location = /50x.html {
            root   /root;
        }

        #静态文件，nginx自己处理
        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            root /var/www/virtual/htdocs;
            #过期30天，静态文件不怎么更新，过期可以设大一点，如果频繁更新，则可以设置得小一点。
            expires 30d;
        }
        #PHP 脚本请求全部转发到 FastCGI处理. 使用FastCGI默认配置.
        location ~ \.php$ {
            root /root;
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME /home/www/www$fastcgi_script_name;
            include fastcgi_params;
        }
        #设定查看Nginx状态的地址
        location /NginxStatus {
            stub_status            on;
            access_log              on;
            auth_basic              "NginxStatus";
            auth_basic_user_file  conf/htpasswd;
        }
        #禁止访问 .htxxx 文件
        location ~ /\.ht {
            deny all;
        }
    }
    
    # 前端开发环境配置
    server {
        listen       80;
        server_name  fe-admin.gialen.com;

        # 后端接口请求
        location /api/ {
            proxy_pass http://127.0.0.1:3000;
        }

        # 前端页面配置
        location / {
            proxy_pass http://127.0.0.1:8081;
        }
    }
}
```

## 坑
1. 在Mac上，有时更改nginx.conf文件重新启动nginx仍然无效，即便重启电脑也无效。
   解决办法：关掉编辑器中打开的nginx.conf文件，再重新打开，配置即可生效。

## 相关链接
- [80端口被占用解决方法](https://www.cnblogs.com/zhaoweidong/p/5710280.html)
- [HTTP服务无法启动的解决方案](https://blog.csdn.net/u010792238/article/details/22661767)
