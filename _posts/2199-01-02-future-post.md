---
title: 'Hive SQL 笔记'
date: 2022-3-1
permalink: /posts/HQL/
tags:
  - SQL
  - Hive
  - 笔记
  - 学习
---

> 笔记存档，完善中……

**目录**

- [SQL 入门笔记📕](#sql-入门笔记)
  - [distinct 去重展示](#distinct-去重展示)
  - [WHERE 子句](#where-子句)
  - [ORDER BY 排序](#order-by-排序)
  - [INSERT INTO 插入](#insert-into-插入)
    - [AUTO INCREMENT 自增+1](#auto-increment-自增1)
  - [UPDATE 更新内容](#update-更新内容)
  - [DROP 删除、撤销](#drop-删除撤销)
  - [ALTER 修改](#alter-修改)
  - [DELETE](#delete)
  - [SELECT TOP 取前几](#select-top-取前几)
  - [IN 多项之中](#in-多项之中)
  - [BETWEEN ... AND ...](#between--and-)
  - [AS 别名](#as-别名)
- [进阶语句🚀](#进阶语句)
  - [where LIKE ：搜索列中的指定模式](#where-like-搜索列中的指定模式)
    - [通配符 % _ [charlist]](#通配符--_-charlist)
  - [JOIN （连接 两个或多个表的行）](#join-连接-两个或多个表的行)
    - [`INNER JOIN` 内连接](#inner-join-内连接)
    - [`LEFT JOIN` 左（外）连接](#left-join-左外连接)
    - [`RIGHT JOIN`](#right-join)
    - [FULL OUTER JOIN](#full-outer-join)
  - [UNION 合并多个查询结果](#union-合并多个查询结果)
  - [SELECT INTO FROM 复制到](#select-into-from-复制到)
  - [CREAT](#creat)
  - [约束（constraint）](#约束constraint)
    - [FOREIGN KEY 外键](#foreign-key-外键)
    - [CHECK 条件约束](#check-条件约束)
    - [DEFAULT 约束](#default-约束)
  - [INDEX 索引](#index-索引)
- [SQL 函数](#sql-函数)
- [HiveSQL 🐘](#hivesql-)
- [SQL 优化](#sql-优化)


# SQL 入门笔记📕

## distinct 去重展示
展示不同的 country 值
```sql
select distinct country from tablename;
```
## WHERE 子句
where后支持的运算符

* = ，注意对于string字段 ='Tom'
* <>，同 !=
* 大于、小于、大于等于、小于等于
* between 100 and 200 ，范围内，包含上下界

* 逻辑判断：
  * and or not
  * is null，为空
  * IN 指定针对某列的多个可能值。where number in (150,250,350)

* LIKE ，搜索模式/模糊查询
  * where name like 'M%' 
  * %表示多个字符，_表示一个


## ORDER BY 排序
```sql
select * from tablename
where xxxxx
order by country desc,number;
```
* 可以order 多列，先按第一列排序，再依次后面
* 不写规则默认ASC，DESC为降序。


## INSERT INTO 插入
```sql
INSERT INTO table_name
VALUES (value1,value2,value3,...);
插入一行，需要列出每一列的value；

也可以指定列插入：
INSERT INTO Websites (name, url, country)
VALUES ('stackoverflow', 'http://stackoverflow.com/', 'IND');

嵌套使用：
insert into table_backup select * from table_name where neza='neza';

可用 select * into 改写（有差异）：
select *  into table_backup from table_name where neza='neza'；
```

### AUTO INCREMENT 自增+1
```sql
CREATE TABLE Persons
(
ID int NOT NULL AUTO_INCREMENT, --定义ID为自增，默认从1计数。
...
)
INSERT INTO Persons (FirstName,LastName) VALUES ('Lars','Monsen'); --当插入新纪录时，ID列会自动赋唯一值。
```


## UPDATE 更新内容
```sql
UPDATE table_name
SET column1=value1,column2=value2,...
WHERE columnx=valuex;

注意检查，如果没有where 将更新全表！
```

## DROP 删除、撤销
```sql
DROP INDEX index_name; --撤销索引
DROP TABLE table_name；
DROP DATABASE database_name；
```

## ALTER 修改
```sql
ALTER TABLE table_name ADD(DROP) column_name datatype; --增加&删除列
ALTER TABLE table_name MODIFY column_name datatype; --修改列属性
```


## DELETE
```sql
DELETE FROM table_name
WHERE some_column=some_value;

如果没有where，将清空表中所有行，但保留表结构&索引。
```

## SELECT TOP 取前几
```sql
select top 5 columnname1 from tablename; 取前五条
select top 5 columnname1 from tablename order by id desc; 取末五条
select top 50 percent columnname1 from tablename; 取前50%
```

> Mysql 中支持 limit 语句
> ```sql
> select columnname1 from tablename limit 5; 取前五
> ```
> Oracle 支持 rownum 条件
>  ```sql
> select columnname1 from tablename where rownum <= 5; 取前五
> ```

## IN 多项之中
```sql
SELECT column_name(s) FROM table_name
WHERE column_name IN (value1,value2,...);
```

## BETWEEN ... AND ...
选取介于两个值之间的数据范围内的值。可以是 **数值、文本**或者**日期**。
> 关于between的上下边界，不同数据库支持有差异！

```sql
SELECT * FROM tablename WHERE cname NOT BETWEEN 'A' AND 'H'; 字符
WHERE date BETWEEN '2016-05-10' AND '2016-05-14'; 日期
```

## AS 别名
为**表名称**或**列名称**指定别名，增加可读性




# 进阶语句🚀


## where LIKE ：搜索列中的指定模式
### 通配符 % _ [charlist]
* % 表示任意多个字符（含0个）
  * abcde LIKE '%bc%'
  
* _ 表示一个字符，__表示两个
  * abcde LIKE 'a__d%'



## JOIN （连接 两个或多个表的行）
![join](https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png)

> A `inner join` B 取交集。
> 
> A `left join` B 取 A 全部，B 没有对应的值为 null。
> 
> A `right join` B 取 B 全部 A 没有对应的值为 null。
> 
> A `full outer join` B 取并集，彼此没有对应的值为 null。

* JOIN的结果可在逻辑上看作是由SELECT语句**指定的列组成的新表**。
* 左连接与右连接的左右指的是以两张表中的**哪一张为基准**，它们都是外连接。
* 外连接就好像是为非基准表添加了一行全为空值的万能行，用来与基准表中找不到匹配的行进行匹配。假设两个没有空值的表进行左连接，左表是基准表，**左表的所有行都出现在结果中**，右表则可能因为**无法与基准表匹配**而出现是**空值**的字段。

### `INNER JOIN` 内连接
```sql
SELECT column_names FROM table1 (INNER) JOIN table2
ON table1.column_namei = table2.column_namej;
```

### `LEFT JOIN` 左（外）连接
从左表（table1）返回所有的行，如果右表（table2）中没有匹配，则该行中右表字段的结果为 NULL。
* 某些数据库中，写作 `LEFT OUTER JOIN`
```sql
website
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
access_log
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
+-----+---------+-------+------------+

SELECT Websites.name, access_log.count, access_log.date
FROM Websites LEFT JOIN access_log
ON Websites.id=access_log.site_id
ORDER BY access_log.count DESC;
```
![](https://www.runoob.com/wp-content/uploads/2013/09/left-join1.jpg)


### `RIGHT JOIN`
与上相反 

### FULL OUTER JOIN 
FULL OUTER JOIN 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行。结合了 LEFT JOIN 和 RIGHT JOIN 的结果。
> MySQL 不支持

```sql
SELECT Websites.name, access_log.count, access_log.date
FROM Websites FULL OUTER JOIN access_log
ON Websites.id=access_log.site_id;
```


## UNION 合并多个查询结果
```sql
SELECT column_name(s) FROM table1 WHERE ...
UNION (ALL)
SELECT column_name(s) FROM table2 WHERE ...
ORDER BY column_name;
```
* 不带 ALL，则返回去重结果；带 ALL，允许重复值；
* UNION 各select子句 必须是**相同**的数据类型、数量、顺序；
* ORDER BY 是对 UNION 后的结果排序，置于最后。


## SELECT INTO FROM 复制到

```sql
SELECT column_name(s) INTO newtable 
FROM table1 (WHERE xxx);

--Tips：可以用 SELECT * 来复制整张表;
```

> MySQL 数据库不支持 SELECT INTO 语句；
> 但支持 `INSERT INTO` ... SELECT 。
> 
> 区别：前者要求目标newtable不存在，会在插入时创建；后者要求目标表2存在。
>
> ```sql
> INSERT INTO table2 SELECT * FROM table1;
> ```


## CREAT
```sql
CREATE DATABASE dbname; --创建库

CREATE TABLE tablename(  --创建表
  column_names data_type(size) constraint_type,
  ...
  ); 
```
> [各数据库-各数据类型](https://www.runoob.com/sql/sql-datatypes.html)

## 约束（constraint）
|constraint_type| 约束含义 | 备注 |
|---|---|---|
`NOT NULL` | 该列不能为空|
`UNIQUE` | 保证该列的每行必须是唯一的值 | 不可重复
`PRIMARY KEY` | 主键 | 仅有一个，可视为 NOT NULL + UNIQUE
`FOREIGN KEY` | 外键 | 保证该表中的数据匹配另一个表中的值的参照完整性。
`CHECK` | 保证列中的值符合指定的条件|
`DEFAULT` | 设定没有给列赋值时的默认值| 当没有设定插入值时

* 创建表时，可直接定义列的约束；
* 修改列的约束：ALTER TABLE table_name MODIFY Age int NOT NULL;
* 为某一约束命名：`CONSTRAINT` *uc_PersonID* UNIQUE (P_Id,LastName)
```sql
CREATE TABLE Persons(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
);  --创建名为pk_PersonID的主键约束。
--该主键是唯一的，但它是由两个列（P_Id 和 LastName）组成的！

修改主键：ADD CONSTRAINT pk_PersonID PRIMARY KEY (P_Id)
删除主键：DROP CONSTRAINT pk_PersonID
```

### FOREIGN KEY 外键
**一个表（多的）中的 FOREIGN KEY 指向 另一个表（少的）中的 UNIQUE KEY（通常是主键）。**
```sql
CREATE TABLE Orders(
O_Id int NOT NULL, --本表主键
OrderNo int NOT NULL,
P_Id int,
PRIMARY KEY (O_Id),
CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
-- P_Id 是 Orders 表的外键；P_Id 是 Persons 表的主键。
);
```
### CHECK 条件约束
```sql
CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
```

### DEFAULT 约束
如果没有规定值，将默认值添加到记录中
```sql
CREATE TABLE Persons
(
    ...
    City varchar(255) DEFAULT 'Beijing',
    OrderDate date DEFAULT GETDATE()
)
```

## INDEX 索引
> 索引使得数据库应用程序可以不读取全表，更快地查找数据。
> 
> 当更新表时，索引也需要更新，从而带来额外的时间消耗。因此，应仅在常被搜索的列上创建索引。

```sql
CREATE INDEX pIndex ON Persons (LastName, FirstName); --创建多列索引 pIndex
CREATE UNIQUE INDEX index_name ON table_name (column_name);--创建唯一索引（两个记录行不能拥有相同的索引值）
```








# SQL 函数

# HiveSQL 🐘
> Hive 是基于 Hadoop 构建的一套数据仓库分析系统，它提供了丰富的SQL查询方式来分析存储在 Hadoop 分布式文件系统中的数据.
> ### 设计准则
> * 尽量把表设计为**分区表**，以分割数据方便操作，避免全表搜索。建议用时间 `ds` 作为表的天分区，实现按天存储检索数据。
> * 尽量把表的存储格式设计为压缩格式，可以减少磁盘空间的占用和查询时间。orc格式
> * 执行select的时候充分利用过滤条件，尽量按照分区进行筛选。（因为where条件从左至右生效，分区筛选条件最好紧跟在where后面）
> * 尽量用小表关联大表，重复key少的表放前面。



# SQL 优化 
not in -> not exist 提升查询效率

查询时加分区：  
where ds = ${run_date}

正则表达式 代替筛选
