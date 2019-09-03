# 管理MySQL

要管理MySQL，可以使用可视化图形界面MySQL Workbench。

MySQL Workbench可以用可视化的方式查询、创建和修改数据库表，但是，归根到底，MySQL Workbench是一个图形客户端，它对MySQL的操作仍然是发送SQL语句并执行。因此，本质上，MySQL Workbench和MySQL Client命令行都是客户端，和MySQL交互，唯一的接口就是SQL。

因此，MySQL提供了大量的SQL语句用于管理。虽然可以使用MySQL Workbench图形界面来直接管理MySQL，但是，很多时候，通过SSH远程连接时，只能使用SQL命令，所以，了解并掌握常用的SQL管理操作是必须的。

## 数据库
在一个运行MySQL的服务器上，实际上可以创建多个数据库（Database）。要列出所有数据库，使用命令：
```sql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| shici              |
| sys                |
| test               |
| school             |
+--------------------+
```
其中，information_schema、mysql、performance_schema和sys是系统库，不要去改动它们。其他的是用户创建的数据库。

要创建一个新数据库，使用命令：
```sql
mysql> CREATE DATABASE test;
Query OK, 1 row affected (0.01 sec)
```
要删除一个数据库，使用命令：
```sql
mysql> DROP DATABASE `test`;
Query OK, 0 rows affected (0.01 sec)
```
注意：
1. 删除一个数据库将导致该数据库的所有表全部被删除。
2. 数据库名称要使用反引号 *`* 引用

对一个数据库进行操作时，要首先将其切换为当前数据库：
```sql
mysql> USE test;
Database changed
```

## 表
列出当前数据库的所有表，使用命令：
```sql
mysql> SHOW TABLES;
+---------------------+
| Tables_in_test      |
+---------------------+
| classes             |
| statistics          |
| students            |
| students_of_class1  |
+---------------------+
```
要查看一个表的结构，使用命令：
```sql
mysql> DESC students;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | bigint(20)   | NO   | PRI | NULL    | auto_increment |
| class_id | bigint(20)   | NO   |     | NULL    |                |
| name     | varchar(100) | NO   |     | NULL    |                |
| gender   | varchar(1)   | NO   |     | NULL    |                |
| score    | int(11)      | NO   |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```
还可以使用以下命令查看创建表的SQL语句：
```sql
mysql> SHOW CREATE TABLE students;
+----------+-------------------------------------------------------+
| students | CREATE TABLE `students` (                             |
|          |   `id` bigint(20) NOT NULL AUTO_INCREMENT,            |
|          |   `class_id` bigint(20) NOT NULL,                     |
|          |   `name` varchar(100) NOT NULL,                       |
|          |   `gender` varchar(1) NOT NULL,                       |
|          |   `score` int(11) NOT NULL,                           |
|          |   PRIMARY KEY (`id`)                                  |
|          | ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 |
+----------+-------------------------------------------------------+
1 row in set (0.00 sec)
```
创建表使用CREATE TABLE语句，而删除表使用DROP TABLE语句：
```sql
mysql> DROP TABLE students;
Query OK, 0 rows affected (0.01 sec)
```
修改表就比较复杂。如果要给students表新增一列birth，使用：
```sql
ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
```
要修改birth列，例如把列名改为birthday，类型改为VARCHAR(20)：
```sql
ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
```
要删除列，使用：
```sql
ALTER TABLE students DROP COLUMN birthday;
```
## 退出MySQL
使用EXIT命令退出MySQL：
```sql
mysql> EXIT
Bye
```
注意EXIT仅仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行。