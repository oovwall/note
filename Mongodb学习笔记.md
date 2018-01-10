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

> 为了方便起见，不用每次在命令行输入一大截的mongod.exe所在的文件的路径，把`C:\Program Files\MongoDB\Server\3.6\bin\`加入环境变量，这样每次启动MongoDB只需要在命令行直接输入：`mongod`即可


## Mac MongoDB连接失败解决办法
#### Failed to connect to 127.0.0.1:27017, in(checking socket for error after poll), reason: Connection refused

```powershell
brew services start mongodb
```


## 命令行操作MongoDB
### 启动MongoDB
```powershell
$ mongod
```

### 进入MongoDB Shell
```powershell
$ mongo
```

### MongoDB Shell下操作数据库
```sql
> use DATABASE_NAME         -- 如果数据库不存在则创建数据库，否则切换到指定数据库
> db                        -- 查看当前数据库名
> db.dropDatabase()         -- 删除数据库
> show dbs                  -- 查看所有数据库

> db.COLLECTION_NAME.find()             -- 查看集合
> db.COLLECTION_NAME.find().pretty()    -- 查看集合
> db.COLLECTION_NAME.drop()             -- 删除集合
```

#### 插入文档
MongoDB 使用 insert() 或 save() 方法向集合中插入文档，语法如下：
```sql
> db.COLLECTION_NAME.insert(document)
```
插入文档也可以使用 db.COLLECTION_NAME.save(document) 命令。如果不指定 _id 字段 save() 方法类似于 insert() 方法。如果指定 _id 字段，则会更新该 _id 的数据。

#### 查询文档
MongoDB 查询数据的语法格式如下：
```sql
> db.COLLECTION_NAME(query, projection)
> -- query ：可选，使用查询操作符指定查询条件
> -- projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。
```
**`MongoDB 与 RDBMS Where 语句比较`**

|   操作  |   格式  |   范例  |   RDBMS中的类似语句     |
| ---- | ---- | ---- | ---- |
|  等于 |  {<key>:<value>} |  db.col.find({"by":"菜鸟教程"}).pretty() |   where by = '菜鸟教程'    |
|  小于 |  {<key>:{$lt:<value>}} | db.col.find({"likes":{$lt:50}}).pretty() |   where likes < 50   |
|  小于或等于 |  {<key>:{$lte:<value>}} | db.col.find({"likes":{$lte:50}}).pretty() |   where likes <= 50   |
|  大于 |  {<key>:{$gt:<value>}} | db.col.find({"likes":{$gt:50}}).pretty() |   where likes > 50   |
|  不等于 |  {<key>:{$ne:<value>}} | db.col.find({"likes":{$ne:50}}).pretty() |   where likes != 50   |

#### 删除文档
```sql
db.COLLECTION_NAME.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)

> -- query ：（可选）删除的文档的条件。
> -- justOne : （可选）如果设为 true 或 1，则只删除一个文档。
> -- writeConcern :（可选）抛出异常的级别。
```

## mongodb
### Node.js连接MongoDB & 操作方法

连接范例
```javascript
const MongoClient = require('mongodb').MongoClient
const assert = require('assert')

// Connection URL
const url = 'mongodb://localhost:27017'

// Database Name
const dbName = 'myjavascript'

// Use connect method to connect to the server
MongoClient.connect(url, function (err, client) {
  assert.equal(null, err)
  console.log('成功连接到MongoDB服务器')

  const db = client.db(dbName)

  // 插入记录
  // insertDocuments(db, function (result) {
  //   console.log(result)
  // })

  // 查找记录
  // findDocuments(db, function (docs) {
  //   console.log(docs)
  // })

  // 更新单条记录
  // updateDocument(db, function (result) {
  //   console.log(result)
  // })

  // 更新多条记录
  // updateDocuments(db, function (result) {
  //   console.log(result)
  // })

  // 删除单条记录
  // removeDocument(db, function (result) {
  //   console.log(result)
  // })

  // 删除多条记录
  // removeDocuments(db, function (result) {
  //   console.log(result)
  // })

  client.close()
})
```

操作方法
```javascript
/**
 * 插入记录
 * @param db
 */
const insertDocuments = function (db, callback) {
  // 获取名为documents的Collection
  const collection = db.collection('documents')

  collection.insertMany([
    {a: 1, b: 4}, {a: 2, c: 5}, {a: 3, d: 6}
  ], function (err, result) {

    // 测试结果
    assert.equal(err, null)
    assert.equal(3, result.result.n)
    assert.equal(3, result.ops.length)
    console.log('Inserted 3 documents into the collection')
    callback(result)
  })
}

/**
 * 查找记录
 * @param db
 * @param callback
 */
const findDocuments = function (db, callback) {
  // 获取名为documents的Collection
  const collection = db.collection('documents')

  collection.find({ a: 1}).toArray(function (err, docs) {
    assert.equal(err, null)
    console.log('Found the following records')
    callback(docs)
  })
}

/**
 * 更新单条记录
 * @param db
 * @param callback
 */
const updateDocument = function (db, callback) {
  // 获取名为documents的Collection
  const collection = db.collection('documents')

  collection.updateOne({ a: 1 }
    , { $set: { b: 2 } }, function (err, result) {
      assert.equal(err, null)
      assert.equal(1, result.result.n)
      console.log('Updated the document with the field a equal to 2')
      callback(result)
    })
}

/**
 * 更新多条记录
 * @param db
 * @param callback
 */
const updateDocuments = function (db, callback) {
  // 获取名为documents的Collection
  const collection = db.collection('documents')

  collection.updateMany({ a: 1 }
    , { $set: { b: 5 } }, function (err, result) {
      assert.equal(err, null)
      console.log('Updated the document with the field a equal to 5')
      callback(result)
    })
}

/**
 * 删除单条记录
 * @param db
 * @param callback
 */
const removeDocument = function (db, callback) {
  // 获取名为documents的Collection
  const collection = db.collection('documents')

  collection.deleteOne({ a: 1 }, function (err, result) {
    assert.equal(err, null)
    assert.equal(1, result.result.n)
    console.log('已删除指定Collection中符合条件的第一条字段')
    callback(result)
  })
}

/**
 * 删除多条记录
 * @param db
 * @param callback
 */
const removeDocuments = function (db, callback) {
  // 获取名为documents的Collection
  const collection = db.collection('documents')

  // 如果第一个参数为空对象，则删除所有记录
  collection.deleteMany({ a: 1 }, function (err, result) {
    assert.equal(err, null)
    console.log('已删除指定Collection中符合条件的所有字段')
    callback(result)
  })
}
```

