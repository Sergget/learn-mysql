# 排序

我们使用SELECT查询时，细心的读者可能注意到，查询结果集通常是按照id排序的，也就是根据主键排序。这也是大部分数据库的做法。如果我们要根据其他条件排序怎么办？可以加上`ORDER BY`子句。例如按照成绩从低到高进行排序：
```sql
-- 按score从低到高
SELECT id, name, gender, score FROM students ORDER BY score;
```
如果要反过来，按照成绩从高到底排序，我们可以加上DESC表示“倒序”：
```sql
-- 按score从高到低
SELECT id, name, gender, score FROM students ORDER BY score DESC;
```
如果score列有相同的数据，要进一步排序，可以继续添加列名。例如，使用ORDER BY score DESC, gender表示先按score列倒序，如果有相同分数的，再按gender列排序：
```sql
-- 按score, gender排序:
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
```
默认的排序规则是ASC：“升序”，即从小到大。ASC可以省略，即ORDER BY score ASC和ORDER BY score效果一样。

如果有WHERE子句，那么ORDER BY子句要放到WHERE子句后面。例如，查询一班的学生成绩，并按照倒序排序：
```sql
-- 带WHERE条件的ORDER BY:
SELECT id, name, gender, score
FROM students
WHERE class_id = 1
ORDER BY score DESC;
```
这样，结果集仅包含符合WHERE条件的记录，并按照ORDER BY的设定排序。