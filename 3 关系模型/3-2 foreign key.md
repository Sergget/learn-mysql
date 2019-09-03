# 外键

当我们用主键唯一标识记录时，我们就可以在students表中确定任意一个学生的记录：
```
id	name	other columns...
1	小明	...
2	小红	...
```
我们还可以在classes表中确定任意一个班级记录：
```
id	name	other columns...
1	一班	...
2	二班	...
```
但是我们如何确定students表的一条记录，例如，id=1的小明，属于哪个班级呢？

由于一个班级可以有多个学生，在关系模型中，这两个表的关系可以称为“一对多”，即一个classes的记录可以对应多个students表的记录。

为了表达这种一对多的关系，我们需要在students表中加入一列class_id，让它的值与classes表的某条记录相对应：
```
id	class_id	name	other columns...
1	1	小明	...
2	1	小红	...
5	2	小白	...
```
这样，我们就可以根据class_id这个列直接定位出一个students表的记录应该对应到classes的哪条记录。

例如：
*
小明的class_id是1，因此，对应的classes表的记录是id=1的一班；
小红的class_id是1，因此，对应的classes表的记录是id=1的一班；
小白的class_id是2，因此，对应的classes表的记录是id=2的二班。
*
在students表中，通过class_id的字段，可以把数据与另一张表关联起来，这种列称为外键。

外键并不是通过列名实现的，而是通过定义外键约束实现的：
```
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);
```
其中，外键约束的名称`fk_class_id`可以任意，`FOREIGN KEY (class_id)`指定了`class_id`作为外键，`REFERENCES classes (id)`指定了这个外键将关联到`classes`表的id列（即`classes`表的主键）。

通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果`classes`表不存在`id=99`的记录，`students`表就无法插入`class_id=99`的记录。

由于外键约束会降低数据库的性能，大部分互联网应用程序为了追求速度，并不设置外键约束，而是仅靠应用程序自身来保证逻辑的正确性。这种情况下，class_id仅仅是一个普通的列，只是它起到了外键的作用而已。

要删除一个外键约束，也是通过ALTER TABLE实现的：
```
ALTER TABLE students
DROP FOREIGN KEY fk_class_id;
```
注意：删除外键约束并没有删除外键这一列。删除列是通过DROP COLUMN ...实现的。

## 多对多
通过一个表的外键关联到另一个表，我们可以定义出一对多关系。有些时候，还需要定义“多对多”关系。例如，一个老师可以对应多个班级，一个班级也可以对应多个老师，因此，班级表和老师表存在多对多关系。

多对多关系实际上是通过两个一对多关系实现的，即通过一个中间表，关联两个一对多关系，就形成了多对多关系：
```
teachers表：

id	name
1	张老师
2	王老师
3	李老师
4	赵老师
```
```
classes表：

id	name
1	一班
2	二班
```
中间表teacher_class关联两个一对多关系：
```
id	teacher_id	class_id
1	1	1
2	1	2
3	2	1
4	2	2
5	3	1
6	4	2
```
通过中间表teacher_class可知teachers到classes的关系：

id=1的张老师对应id=1,2的一班和二班；
id=2的王老师对应id=1,2的一班和二班；
id=3的李老师对应id=1的一班；
id=4的赵老师对应id=2的二班。

同理可知classes到teachers的关系：

id=1的一班对应id=1,2,3的张老师、王老师和李老师；
id=2的二班对应id=1,2,4的张老师、王老师和赵老师；
因此，通过中间表，我们就定义了一个“多对多”关系。

## 一对一
一对一关系是指，一个表的记录对应到另一个表的唯一一个记录。

例如，students表的每个学生可以有自己的联系方式，如果把联系方式存入另一个表contacts，我们就可以得到一个“一对一”关系：
```
id	student_id	mobile
1	1	135xxxx6300
2	2	138xxxx2209
3	5	139xxxx8086
```
有细心的童鞋会问，既然是一对一关系，那为啥不给students表增加一个mobile列，这样就能合二为一了？

如果业务允许，完全可以把两个表合为一个表。但是，有些时候，如果某个学生没有手机号，那么，contacts表就不存在对应的记录。实际上，一对一关系准确地说，是contacts表一对一对应students表。

还有一些应用会把一个大表拆成两个一对一的表，目的是把经常读取和不经常读取的字段分开，以获得更高的性能。例如，把一个大的用户表分拆为用户基本信息表user_info和用户详细信息表user_profiles，大部分时候，只需要查询user_info表，并不需要查询user_profiles表，这样就提高了查询速度。

## 小结
关系数据库通过外键可以实现一对多、多对多和一对一的关系。外键既可以通过数据库来约束，也可以不设置约束，仅依靠应用程序的逻辑来保证。