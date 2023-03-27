
## 关系模型

行：记录 Record
列：字段 Column

一对多 多对一等等关系

### 主键

能够通过某个字段唯一区分出不同的记录，这个字段被称为**主键**。

一般来说命名为 `id`:
- 自增整数：BIGINT
- GUID：STRING

#### 联合主键

关系数据库实际上还允许通过多个字段唯一标识记录，即两个或更多的字段都设置为主键，这种主键被称为联合主键。

没有必要的情况下，我们尽量不使用联合主键，因为它给关系表带来了复杂度的上升。

### 外键

表达一对多的关系

![[Pasted image 20230307144927.png]]

#### 多对多

中间表

#### 一对一

有一些应用会把一个大表拆成两个一对一的表，目的是把经常读取和不经常读取的字段分开，以获得更高的性能。例如，把一个大的用户表分拆为用户基本信息表`user_info`和用户详细信息表`user_profiles`，大部分时候，只需要查询`user_info`表，并不需要查询`user_profiles`表，这样就提高了查询速度。

## 索引

索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。

索引的效率取决于索引列的值是否散列，即该列的值如果越互不相同，那么索引效率越高。

对于主键，关系数据库会自动对其创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一。

## 查询

```sql
SELECT * FROM TABLE_NAME;

SELECT 1; -- 一般用于测试数据库连接
```

### 条件查询

```sql
SELECT * FROM TABLE_NAME WHERE CONDITION

SELECT * FROM TABLE_NAME WHERE CONDITION1 AND CONDITION2

SELECT * FROM TABLE_NAME WHERE CONDITION1 OR CONDITION2

SELECT * FROM TABLE_NAME WHERE NOT CONDITION
```

condition: 

```sql
score >= 80
gender = 'F'
name LIKE '小%'
score BETWEEN 60 AND 90 -- [60,90]
score in (60,90) -- {60,90} 集合
```

### 投影查询

```sql
SELECT id, score, name FROM students;
SELECT id, score, name FROM students WHERE gender = 'M';
SELECT id, score alias, name FROM students;
```

### 排序

```sql
SELECT id, name, gender, score FROM students ORDER BY score;

-- 倒序 DESC
SELECT id, name, gender, score FROM students ORDER BY score DESC;

-- 多关键字排序
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;

-- 如果有`WHERE`子句，那么`ORDER BY`子句要放到`WHERE`子句后面
SELECT id, name, gender, score FROM students WHERE class_id = 1 ORDER BY score DESC, gender;
```

### 分页

`LIMIT ... OFFSET ...`

```sql
-- 最多显示3项，从第0号记录开始
SELECT id, name, gender, score FROM students ORDER BY score DESC LIMIT 3 OFFSET 0;
```

### 聚合查询

```sql
-- COUNT()
SELECT COUNT(*) alias FROM students;
SELECT COUNT(*) alias FROM students WHERE gender = 'M';

-- SUM()
SELECT SUM(score)/count(*) from students;

-- AVG()
SELECT AVG(score) from students;

-- MAX()
SELECT MAX(score) from students;

-- MIN()
SELECT MIN(score) from students;
```

如果聚合查询的`WHERE`条件没有匹配到任何行，`COUNT()`会返回0，而`SUM()`、`AVG()`、`MAX()`和`MIN()`会返回`NULL`

#### 分组聚合

```sql
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;
```

先将`class_id`相同的列分组，再分别进行聚合

```sql
-- 根据两列进行分组
select class_id, gender, avg(score) from students group by gender, class_id;
```

### 多表查询

笛卡儿乘积

```sql
SELECT * FROM students, classes;

-- 修改显示顺序
SELECT
    students.id sid,
    students.name,
    students.gender,
    students.score,
    classes.id cid,
    classes.name cname
FROM students, classes;

-- 为表设置别名
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;

```

### 连接查询

```sql
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id;
```

将`students`的`class_id`作为查询键

`INNER JOIN` 只返回同时存在于两张表的行数据

`RIGHT OUTER JOIN` 返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以`NULL`填充剩下的字段。

`LEFT OUTER JOIN` 则返回左表都存在的行。

`FULL OUTER JOIN` 会把两张表的所有记录全部选择出来，并且，自动把对方不存在的列填充为NULL

```sql
SELECT ... FROM tableA ... JOIN tableB ON tableA.column1 = tableB.column2;
```

![[Pasted image 20230308155240.png]]

## 增改删

## INSERT

```sql
INSERT INTO TABLE_NAME ( var1, var2, ... ) VALUES 
( value1, value2, ... ), 
( ... );
```

### UPDATE

```sql
UPDATE TABLE_NAME SET var1=value1, var2=value2, ... WHERE ...;
```

更新字段时可以使用表达式

### DELETE

```sql
DELETE FROM TABLE_NAME WHERE ...;
```

## MySQL 语句

```mysql
show databases;
create database NAME;
drop database NAME;
use DATABASE_NAME;

show tables;
desc TABLE_NAM; -- 表的结构
create table TABLE_NAM;
drop table TABLE_NAM;
-- 修改表
-- 新增列
alter table TABLE_NAM add column COL VARCHAR(10) NOT NULL;
-- 修改列
alter table TABLE_NAM change column COL VARCHAR(20) NOT NULL;
-- 删除列
alter table TABLE_NAM drop column COL;
```

```mysql
-- 插入或替换
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);

-- 插入或更新
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;

-- 插入或忽略
INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);

```

```mysql
-- 快照（复制）
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
```

```mysql
-- 创建表
CREATE TABLE statistics (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    average DOUBLE NOT NULL,
    PRIMARY KEY (id)
);

-- 查询结果写入表
INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
```

## 事务

把多条语句作为一个整体进行操作的功能，被称为数据库事务。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

```mysql
BEGIN;
...
COMMIT;

BEGIN;
...
ROLLBACK;
```

![[Pasted image 20230308165358.png]]

### 隔离级别

`Read Uncommitted` 是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）。


`Read Committed`隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。
不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

`Repeatable Read` 隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。
幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

`Serializable` 是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。
虽然Serializable隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用Serializable隔离级别。

#### 默认隔离级别

如果没有指定隔离级别，数据库就会使用默认的隔离级别。在MySQL中，如果使用InnoDB，默认的隔离级别是Repeatable Read。