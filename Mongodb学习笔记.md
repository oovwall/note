# Mongodb学习笔记

## Mongodb连接失败解决办法
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