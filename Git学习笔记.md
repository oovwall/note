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

## 相关链接
- [常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)