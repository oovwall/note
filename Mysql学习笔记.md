# MySQL 学习笔记

## 安装：
使用Homebrew安装MySQL
安装MySQL

``` powershell
$ brew update # 这是一个好习惯
$ brew install mysql
```

在使用 MySQL 前，我们需要做一些设置：

``` powershell
$ unset TMPDIR
$ bash mysql_install_db --verbose --user=root --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
```

## 使用

启动 MySQL 服务，运行 mysql.server

``` powershell
$ mysql.server start
```

关闭 MySQL，运行：

``` powershell
$ mysql.server stop
```

你可以了解更多 mysql.server 的命令，运行：

``` powershell
$ mysql.server --help
```

登录 MySQL, 运行:

``` powershell
$ mysql -u root
```

> 默认情况下，MySQL 用户 root 没有密码，这对本地开发没有关系，但如果你希望修改密码，你可以运行:

``` powershell
$ mysql admin -u root password 'new-password'
```

### 常用SQL命令

```sql
-- 查看所有数据库
> SHOW DATABASES;
> USE db_name

-- 查看
> SHOW TABLES;

-- 创建数据表
> CREATE TABLE IF NOT EXISTS `amount`(
   `id` INT UNSIGNED AUTO_INCREMENT,
   `total_amount` BIGINT NOT NULL,
   PRIMARY KEY ( `id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 查看表结构
> DESCRIBE table_name;
> DESC table_name;
> SHOW COLUMNS FROM table_name;
> SHOW CREATE TABLE table_name;

-- 删除数据表
> DROP TABLE table_name;

-- 表中插入数据
> INSERT INTO amount VALUES (1, 50000000);
> INSERT INTO  amount  (total_amount) VALUES (70000000);

-- 修改字段类型
> ALTER TABLE amount MODIFY total_amount BIGINT;

-- 更新表中记录
> UPDATE amount SET total_amount=55550000 where id=1;
> UPDATE amount SET total_amount = total_amount + 5 WHERE id = 1

-- 删除表中记录
> DELETE FROM table_name [WHERE Clause]

-- 更改数据库密码
> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('新密码');  
```



