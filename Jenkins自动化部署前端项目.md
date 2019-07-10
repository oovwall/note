# Jenkins自动化部署前端项目

## 前端部署环境搭建
### Node.js插件安装
1. 打开`系统管理 > 插件管理`，安装`NodeJS Plugin`
### 安装Node.js
1. 打开`系统管理 > 全局工具配置 > NodeJS`
1. 点击NodeJS安装
  1. 方法一：自动安装
     1. 填入NodeJS别名，如：NodeJS_v10.16.0
     1. 勾选 **自动安装**
     1. 选择相应的Version版本，点击保存即可。等第一次构建项目时会自动安装Node.js。但是由于网络慢或一些其它原因导致自动安装失效的，这时建议用方法二手动安装Node.js。
  1. 方法二：手动安装  
     首先，先到Jenkins对应的主机上安装Node.js程序
     1. 下载Linux Binaries(x64)文件，这里是`node-v10.16.0-linux-x64.tar.xz`，上传到`/usr/local/lib/node`目录
     1. 解压Node.js安装包（二进制文件），分别运行以下命令行
        ```powershell
        VERSION=v10.16.0
        DISTRO=linux-x64
        sudo mkdir -p /usr/local/lib/nodejs
        sudo tar -xJvf node-$VERSION-$DISTRO.tar.xz -C /usr/local/lib/nodejs 
        ```
     1. 运行`vim /etc/profile`，编辑`profile`文件设置环境变量，按`i`键，在文件末尾处插入以下代码；按`esc`键输入`:wq`保存退出。
        ```profile
        # Nodejs
        VERSION=v10.16.0
        DISTRO=linux-x64
        export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
        ```
     1. 运行`. /etc/profile`刷新profile
     1. 运行`node -v`看Node.js是否被成功安装
     1. 在命令行依次运行以下命令行（我也不知道干嘛的）
        ```powershell
        sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/node /usr/bin/node
        
        sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/npm /usr/bin/npm
        
        sudo ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/npx /usr/bin/npx
        ```
     然后，回到Jenkins配置(`系统管理 > 全局工具配置 > NodeJS`)
     1. 填入NodeJS别名，如：Global_v10.16.0
     1. 取消勾选 **自动安装**
     1. 在安装目录填入 `/usr/local/lib/nodejs/node-v10.16.0-linux-x64`，点击保存即可。
   
## 发布配置
### Publish over SSH配置
点击 `系统管理 > 系统设置`，在`Publish over SSH`处添加SSH Servers，填入对应的值。如果需要填入密码，则点击`高级...`勾选`Use password authentication, or use a different key`在`Passphase / Password`中填入密码，点击Test Configuration提示『Success』则成功  
![](http://ww2.sinaimg.cn/large/006tNc79gy1g4utg6g3jqj30gd091q3g.jpg)

> 值得注意的是，这里的Remote Directory 是默认的SSH地址，建立构建任务的时侯以这个目录为相对的目录。

### 建立构建和部署任务
1. 点击新建任务，输入任务名称，选择`构建一个自由风格的软件项目`，点确认
1. 在 **General** 填入描述
1. 在 **源码管理** 处选择Git，填入Repository URL；在Credentials选择属于自己的凭证；在Branch Specifier 填入分支，如：`*/master`
1. 在 **构建环境** 处勾选`Add timestamps to the Console Output`  
   勾选`Provide Node & npm bin/ folder to PATH`，在NodeJS Installation处选择之前安装的Node.js对应的别名，如：Global_v10.16.0；npmrc file保持默认选项`- use system default -`，Cache location保持默认选项`Default (~/.npm or %APP_DATA% pm-cache)
1. 在 **构建** 处执行shell处填入以下命令行
   ```powershell
   node -v
   npm install -g yarn
   yarn -v
   yarn install
   yarn build
   cd dist
   rm -rf fe-admin.tar.gz           # 删除之前的打好的压缩包
   tar -zcvf fe-admin.tar.gz *      # 压缩
   cd ../
   ```
1. 点击`增加构建步骤`选择**Send files or execute commands over SSH**，如图配置：  
   ![](http://ww1.sinaimg.cn/large/006tNc79gy1g4usqioritj30pj0kzgnd.jpg)
   Name：为之前在SSH配置那里的别名
   Source files: 相对于项目构建时的目录  
   Remove prefix：文件复制时要过滤的目录  
   Remote directory：文件得到到远程机上的目录，此目录是相对于“SSH Server”中的“Remote directory”的，如果不存在将会自动创建。
   ```powershell
   cd /data/project/h5/                     # 进入对应目录
   rm -rf fe-admin                          # 删除 fe-admin目录及内容
   mkdir fe-admin                           # 创建 fe-admin目录
   tar -zxvf fe-admin.tar.gz -C fe-admin/   # 解压 fe-admin.tar.gz到fe-admin文件夹
   rm -rf fe-admin.tar.gz                   # 删除 fe-admin.tar.gz
   ```
1. 点击保存，创建任务。
