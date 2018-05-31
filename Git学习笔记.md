# Git学习笔记
## Git配置文件config用户名和邮箱
```powershell
# 查看git设置列表信息
$ git config --list

# 查看用户名
$ git config user.name

# 设置用户名
$ git config --global user.name "yourname"

# 设置用户邮箱
$ git config --global user.email myemail@qq.com
```

## Git逻辑
![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)
- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

## Git常用命令
```powershell
$ git clone https://github.com/libgit2/libgit2 mylibgit     # 克隆 Git 的可链接库 libgit2，变更名字为：mylibgit

$ git checkout .            # 本地所有修改的。没有的提交的，都返回到原来的状态

$ git reset --hard          # 重置暂存区与工作区，与上一次commit保持一致
```

## Git仓库迁移
```powershell
$ git remote -v             # 显示所有远程仓库
---------
origin  https://github.com/oovwall/LSD.git (fetch)
origin  https://github.com/oovwall/LSD.git (push)

$ git remote rm origin      # 删除origin
---------

$ git remote add origin http://git.newaddress.com/LSD.git          # 添加新的origin
---------

$ git remote -v             # 显示远程仓库信息
---------
origin     http://git.newaddress.com/LSD.git (fetch)
origin     http://git.newaddress.com/LSD.git (push)

$ git remote set-url origin http://git.newaddress.com/LSD.git      # 指向新的地址

$ git push origin master    # 将本地git仓库push到server

```

## 相关链接
- [常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)