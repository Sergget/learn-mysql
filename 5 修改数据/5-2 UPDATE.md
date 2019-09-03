# UPDATE
阅读: 14404
如果要更新数据库表中的记录，我们就必须使用UPDATE语句。

## update-sql

UPDATE语句的基本语法是：
```
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
```
例如，我们想更新students表id=1的记录的name和score这两个字段，先写出UPDATE students SET name='大牛', score=66，然后在WHERE子句中写出需要更新的行的筛选条件id=1：
```sql
-- 更新id=1的记录
UPDATE students SET name='大牛', score=66 WHERE id=1;
-- 查询并观察结果:
SELECT * FROM students WHERE id=1;
```
注意到UPDATE语句的WHERE条件和SELECT语句的WHERE条件其实是一样的，因此完全可以一次更新多条记录：
```sql
-- 更新id=5,6,7的记录
UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
-- 查询并观察结果:
SELECT * FROM students;
```
在UPDATE语句中，更新字段时可以使用表达式。例如，把所有80分以下的同学的成绩加10分：
```sql
-- 更新score<80的记录
UPDATE students SET score=score+10 WHERE score<80;
-- 查询并观察结果:
SELECT * FROM students;
```
其中，SET score=score+10就是给当前行的score字段的值加上了10。

如果WHERE条件没有匹配到任何记录，UPDATE语句不会报错，也不会有任何记录被更新。例如：
```sql
-- 更新id=999的记录
UPDATE students SET score=100 WHERE id=999;
-- 查询并观察结果:
SELECT * FROM students;
```
最后，要特别小心的是，UPDATE语句可以没有WHERE条件，例如：
```
UPDATE students SET score=60;
```
这时，整个表的所有记录都会被更新。所以，在执行UPDATE语句时要非常小心，最好先用SELECT语句来测试WHERE条件是否筛选出了期望的记录集，然后再用UPDATE更新。

## MySQL
在使用MySQL这类真正的关系数据库时，UPDATE语句会返回更新的行数以及WHERE条件匹配的行数。

例如，更新id=1的记录时：
```sql
mysql> UPDATE students SET name='大宝' WHERE id=1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```
MySQL会返回1，可以从打印的结果Rows matched: 1 Changed: 1看到。

当更新id=999的记录时：
```sql
mysql> UPDATE students SET name='大宝' WHERE id=999;
Query OK, 0 rows affected (0.00 sec)
Rows matched: 0  Changed: 0  Warnings: 0
```
MySQL会返回0，可以从打印的结果Rows matched: 0 Changed: 0看到。

## 小结
使用UPDATE，我们就可以一次更新表中的一条或多条记录。