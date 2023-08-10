# 1.数据库与SQL

## 1.数据库

1. 
2. 层次数据库
3. 关系数据库
4. 面向对象数据库
5. XML 数据库
6. 键值存储数据库

---

## 2.数据库结构

1. 根据 SQL 语句的内容返回的数据同样必须是二维表的形式

---

## 3.SQL概要

### 1.SQL 语句及其种类

SQL 用关键字，表名和列名等组合组成的一条语句

1. DDL 操作数据库和表 create drop alter
2. DML 操作数据 select insert update delect
3. DCL 数据控制

---

## 4.alter 语句

```sql
alter table <> add column <> -- 新增列 在除 mysql 外不需要 column
```

```sql
alter table <> drop column <> -- 删除列 Oracle 不需要 column
```

```sql
alter table <> rename to <> -- oracle 修改表名
alter table <> to <> -- mysql 修改表名
alter '<>','<>' -- sqlserver
```

```sql
alter table <> change column <> <> varchar(128) not null -- mysql 修改列名
```

---

# 2.查询基础

## 1.where 查询

先通过 where 字句查询出符合条件的记录，再使用 select 选择指定列

---

## 2.null

包含 null 的计算，结果都是 null，哪怕是除以0

---

## 3.真值

sql 中除了包含 false 和 true 以外还有 unknow 这样的不确定真值

---

# 3.聚合和排序

## 1.对表进行聚合查询

### 1.count

count 函数中的参数如果是 * 那么输出结果会包含 null

---

### 2.sum

sum 计算对应列和的时候如果包含 null ,则会将 null 的部分排除掉而不是包含计算视为 0 

---

### 3.avg

与 sum 相同会排除 null 的数据，但是分母不会，计算个数的时候依旧包含 null ，默认是不会视为 0的

---

### 4.max & min

与 sum 和 avg 不同，原则上可以适用任何数据类型，不仅限于数值类型

也就是说，只要能够排序的数据，那肯定会有最大值和最小值

---

### 5.distinct

想要计算值的种类时，可以在 count 函数的参数中使用 distinct

所有的聚合函数都是可以使用 distinct

---

## 2.对表进行分组

### 1.group by

就像对表进行切割，其中 null 的部分会单独分为一组

>目前 SQL 书写顺序
>
>```sql
>SELECT - FROM - WHERE - GROUP BY
>```

>目前 SQL 执行顺序
>
>```sql
>FROM - WHERE - GROUP BY - SELECT
>```

---

#### 1.常见错误

在 SELECT 字句中写了多余的列，不能在写聚合键之外的列名

>对数据分组后，一行对应了一组，如果出现了聚合键之外的列名，聚合键是无法与多出来的列一一对应的
>
>

---

在Group By 子句中写了列的别名

>在聚合之前 SELECT 还没执行，所以无法得知别名

---

Group By 的结果不能排序，是随机的

---

在 WHERE 字句中不能使用聚合函数

---

## 3.为聚合结果指定条件

### 1.having

>目前 SQL 书写顺序
>
>``` sql
>SELECT - FROM - WHERE - GROUP BY - HAVING
>```

---

构成要素

1. 常数
2. 聚合函数
3. Group By 子句中指定的列名

---

如果 where 和 having 都能达到相同的结果，那么优先使用 where ，因为使用 where 会剔除不符合条件的列这样计算量就会减少

---

## 4.对结果进行排序

排序键如果包含 null ，会在开头或末尾进行汇总

order by 字句是可以使用 select 字句中的别名

>目前 SQL 执行顺序
>
>```sql
>FROM - WHERE - GROUP BY - HAVING - SELECT - ORDER BY
>```

---

order by 也可以使用不在 select 字句中的列，也可以使用聚合函数

不要使用列序号来排序

----

# 4.数据更新

## 1.insert

```sql
INSERT INTO <表名> (列1，列2，...) VALUES (值1，值2，...)
```

将列名和值用逗号隔开，分别括在括号内，这种形式称之为 `清单` ，分别可以称为列清单，值清单。

列清单和值清单的数量必须保持一致

原则上执行一次 insert 语句会插入一行数据，因此需要插入多行的时候，需要循环执行相应次数的 insert 语句

如果支持多行 insert 只需要在 values 字句后用逗号隔开，继续写一个 values 字句即可

oracle 需要使用 insert all into ，在values 字句后不能直接书写 values ，需要加上前缀 into <表名> values

如果需要插入默认值只需要填入 `DEFAULT` 即可，也可以在列清单和值清单中直接全部省略对应的值即可

>建议使用关键字的方式，这样更易懂

如果需要从其他表直接复制数据可以使用 insert ... select 语句

```sql
INSERT INTO xxx SELECT xxx FROM xxx
```

可以使用 where 进行筛选

可以使用 order by 字句但是无意义，因为不能保证顺序

使用  group by 字句时需要注意列名之间的对应

---

## 2.delete

>drop table 会将整个表删除，包括结构
>
>delete 只会删除数据不会删除表结构

```SQL
DELETE FROM <表名>
```

delete 和 insert 一样是以记录为基本单位进行操作的

可以在表名后面添加 where 字句限定删除条件

---

>还有一个 truncate 删除语句，这个语句删除效率会比 delete 高很多，但是不能指定 where 条件
>
>oracle 中会默认进行事务提交

----

## 3.update

```sql
UPDATE <表名>
	SET <列名> =<表达式>
```

  和 delete 语句一样也是可以添加 where 字句限定搜索范围

如果更新两处记录，第二条更新会受到第一条更新的影响

---

## 4.事务

>需要在同一个处理单元中执行的一系列更新处理的集合

事务开始语句

```sql
SQL SERVER POSTGRESQL
BEGIN TRANSACTION

MYSQL
START TRANSACTION

ORACLE DB2
无
```

结束均为 commit

取消 ROLLBACK

---

事务的特性

ACID 特性

1. 原子性 atomicity

   要么全部执行成功，要么全部执行失败

2. 一致性 consistency

   满足预先设定的数据库约束

3. 隔离性 isolation

   事务与事务之间不会受到干扰

4. 持久性 durability

   事务不论在提交还是回滚，数据都会永久保存

---

# 5.复杂查询

## 1.视图

视图是保存在硬盘上面的 SQL 语句，本身不存储数据

---

优点

1. 视图不会保存数据，所以可以节省存储设备的空间
2. 可以将频繁使用的 SELECT 语句保存为视图

---

创建视图

```sql
CREATE VIEW 视图名称
AS
<SELECT 语句>
```

视图列的顺序和 select 字句的列顺序相同

---

使用视图查询通常需要执行两条以上的 SELECT 语句，因为因为视图是可以内嵌一个视图的，但是不建议这么做，多重视图会降低 SQL 的性能

---

视图的限制

定义视图的时候不能使用 order by 字句，因为数据没有顺序

对视图进行更新

如果满足以下条件就可以被更新

1. SELECT 字句没有使用 DISTINCT 
2. FROM 字句只有一张表
3. 没有使用 GROUP BY 字句
4. 没有使用 HAVING 字句

---

删除视图

```sql
DROP VIEW 视图名称
```

---

## 2.子查询

子查询可以说是一次性视图，查询结束后就会消失

子查询就是将保存的视图语句直接放在 FROM 字句中

执行顺序是先执行内层，然后逐层执行到外层

和视图一样子查询也是可以无限嵌套的

原则上子查询都是要命名的

---

### 1.标量子查询

标量就是单一的意思，必须而且只能返回一行一列的结果，也就是一个单元格的值

因为是单一值所以可以用在比较运算符中

---

```sql
...
where price > avg(price)
```

这个语句不能执行，因为 where 字句中是不能使用聚合函数的，所以这个时候我们就可以使用标量子查询

```sql
where price > (select avg(price) from xxx)
```

---

能够使用常数或者列名的地方，无论是 select 字句，group by 字句，having 字句，还是 order by 字句，几句所有的地方都能使用

---

标量子查询绝对不能返回多行结果

---

## 3.关联子查询

在细分的组内进行比较时，就需要使用关联子查询

关联子查询也是用来对集合进行切分的

结合条件一定要卸载子查询中

---

# 6.函数、谓词、CASE 表达式

## 1.各种各样的函数

### 1.算术函数

四则运算符也属于函数

```sql
ABS() 
```

绝对值函数，将数值转换为绝对值

---

```sql
MOD() 
```

求余

SQL Server 不支持，需要使用 %

---

```sql
ROUND(,)
```

四舍五入，两个参数，第一个是对象数值，第二个是保留小数的位数

---

### 2.字符串函数

``` sql
||
```

```sql
+
```

```sql
CONCAT()
```

第一个 Oracle DB2 PostgreSQL 使用

第二个 SQL Server

第三个 MySQL 可以多个

---

```sql
LENGTH()
LEN()
```

字符串求长度

第二个为 SQL Server

MySQL 中还有一个函数 CHAR_LENGTH()

---

```SQL
LOWER()
```

转小写字母

---

```sql
REPLACE()
```

字符串替换

参数

1. 字符串对象
2. 要替换的字符串
3. 替换后的字符串

---

```SQL
SUBSTRING(X FROM X FOR X)
```

字符串截取

参数

1. 对象字符串
2. 截取起始位置
3. 截取的字符串数量

位置从 0 开始

SQL Server Oracle 中没有 from for ，用逗号代替

---

```sql
UPPER()
```

小写字母转大写

---

### 3.日期函数

当前日期
