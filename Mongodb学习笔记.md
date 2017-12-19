# MongoDB学习笔记

## MongoDB安装
### windows MongoDB 安装方法
#### 下载安装
1. 安装Community Server版：
[https://www.mongodb.com/download-center?jmp=docs&_ga=2.49819809.663059071.1513651999-1371713344.1513651999#production]()默认设置安装

**安装文档**
https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/

2. 安装完成后，MongoDB需要一个文件夹储存数据文件。在C盘根目录下创建文件夹：`C:\data\db`，或用命令行创建
```powershell
mk \data\db
"C:\Program Files\MongoDB\Server\3.6\bin\mongod.exe" --dbpath d:\test\mongodb\data
# ↑这句应该是可加可不加，指定备用地址，个人理解应该是如果没有在C盘创建data存储文件夹，则可指定d:\test\mongodb\data 为数据存储目录
"C:\Program Files\MongoDB\Server\3.6\bin\mongod.exe"
```

3. 用MongoDB Compass Community连接MongoDB


## MongoDB连接失败解决办法
#### Failed to connect to 127.0.0.1:27017, in(checking socket for error after poll), reason: Connection refused

```powershell
brew services start mongodb
```
### 进入MongoDB Shell
```powershell
$ mongo
```

### MongoDB Shell下操作数据库
```sql
> use DATABASE_NAME			-- 如果数据库不存在则创建数据库，否则切换到指定数据库
> db						-- 查看当前数据库名
> db.dropDatabase()			-- 删除数据库
> show dbs					-- 查看所有数据库
```