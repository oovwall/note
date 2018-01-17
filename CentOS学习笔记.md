# CentOS学习笔记

## CentOS设置相关
### CentOS安装五笔输入法
1. 加开终端，输入`su root`，再输入密码。
2. 登录root后，再键入`yum remove ibus`，移除现有的输入法框架。移除过程中会弹出询问是否真的移除，输入`y`继续。
3. 移除完毕后，再键入`yum install ibus ibus-table`，回车后重新安装输入法框架。同样，在执行过程中会询问是否确认安装，键入`y`回车继续。
4. 输入`yum install ibus ibus-table-wubi`安装极点五笔输入法，并且在执行过程中键入`y`确认下载和安装。
5. 安装成功后，键入`exit`退出root帐号，然后再关闭终端窗口。
6. 在`设置 > 区域和语言`中添加输入源。如果没有出现五笔则重启CentOS。

## Hyper-v相关
虚拟机和系统之前鼠标切换用：`ctrl + alt + ← `
