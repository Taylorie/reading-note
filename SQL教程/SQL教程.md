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

当前日期函数

```sql
CURRENT_DATE -- MySQL ORACLE 
CAST(CURRENT_TIMESTAMP AS DATE) -- SQL Server
```

---

当前时间函数

```sql
CURRENT_TIME -- MySQL ORACLE 
CAST(CURRENT_TIMESTAMP AS TIME) -- SQL Server
```

---

当前日期和时间

```sql
CURRENT_TIMESTAMP -- MySQL SQL Server Oracle
```

---

截取日期函数

```SQL
EXTRACT(x FROM x)
```

获取指定的日期时间元素

x  YEAR MONTH DAY 等等

x CURRENT_TIMESTAMP

---

### 4.转换函数

```sql
CAST(X AS X)
```

转换前的值

想要转换的元素类型

---

```sql
COALEACE()
NVL(X,X) -- Oracle 简化版，第二的参数为如果第一个为 null 则应该为的值
```

将 NULL 转换为其他值

返回第一个不是 NULL 的值

---

## 2.谓词

需要满足特定条件的函数，该条件的返回值为真值

1. LIKE
2. BETWEEN
3. IS NULL
4. IS NOT NULL
5. IN
6. NOT IN -- 如果参数包含 null 则返回空
7. EXISTS

第七个有点特殊，只要能匹配上就能为真，在参数的 SELECT 字句中可以用常数

---

## 3.CASE 表达式

```sql
CASE WHEN <求值表达式> THEN <表达式>
 	 WHEN <求值表达式> THEN <表达式>
 	 WHEN <求值表达式> THEN <表达式>
...
     ELSE <表达式>
END
```

如果 when 为真，则 then 

如果都不符合则 else

end 必须存在

else 可以省略，但不建议省略

---

简单CASE

```SQL
CASE 匹配列 
	 WHEN <求值> THEN <表达式>
 	 WHEN <求值> THEN <表达式>
 	 WHEN <求值> THEN <表达式>
...
     ELSE <表达式>
END
```

只要 匹配列和求值能匹配上就直接返回，无需写表达式，但是这种方式不灵活

---

```sql
DECODE()
```

Oracle 简化版本

第一个参数为匹配列，从后面开始，每两个为一个匹配，第一个匹配上，则返回第二个值

最后一个单个为 ELSE

---

# 7.集合运算

## 1.表的加减法

集合运算在数据库领域表示 记录的集合

具体来说，表、视图和查询的执行结果都是记录的集合

用来进行集合运算的运算符称为 `集合运算符`

---

### 1.表的加法 UNION

取出两张表所有的内容，会去除重复的行

```sql
SELECT * FROM 
UNION
SELECT * FROM
```

---

### 2.注意事项

1. 作为运算对象的记录的列数必须相同

2. 作为运算对象的记录中的列的类型必须一致

   >有些 DBMS 不会做硬性要求，会进行隐式的转换

3. 可以使用任何 SELECT 语句，但是 ORDER BY 字句只能在最后的语句中使用

   >可以使用 where group by having 等等

---

### 3.ALL

使用 ALL 就可以选出重复的行记录

在 UNION 之外的集合运算符都可以使用

---

### 4.交集运算 INTERSECT

```sql
SELECT * FROM 
INTERSECT
SELECT * FROM
```

---

### 5.差集运算 EXCEPT

```SQL
SELECT * FROM 
EXCEPT
SELECT * FROM
```

在 oracle 中请使用 minus 

注意是去除左表中，和右表相交的部分

---

### 6.运算顺序

在 SQL 中，集合运算符的运算顺序是由运算符的优先级来确定的。以下是常见的集合运算符及其优先级，从高到低排列：

1. **括号**: 括号具有最高的优先级，可以用于控制运算的顺序。
2. **UNION、UNION ALL**: UNION 和 UNION ALL 运算符用于合并两个或多个结果集。UNION 会移除重复行，而 UNION ALL 则保留所有行，包括重复的行。
3. **INTERSECT**: INTERSECT 运算符用于获取两个结果集的交集，即共同的行。
4. **EXCEPT (MINUS)**: EXCEPT 运算符用于从一个结果集中排除另一个结果集中存在的行，也称为差集操作。
5. **INNER JOIN、LEFT JOIN、RIGHT JOIN、FULL JOIN**: 这些是用于连接多个表的运算符，它们在优先级中处于较低位置。它们的运算顺序会受到连接条件的影响。

需要注意的是，虽然有这些优先级，但在编写复杂的 SQL 查询时，最好使用括号来明确指定想要进行操作的顺序，以避免歧义和错误。使用括号可以确保查询按照预期的方式执行。

以下是一个示例查询，演示了如何使用括号来控制集合运算符的运算顺序：

```sql
SELECT column1 FROM table1
UNION
SELECT column1 FROM table2
INTERSECT
SELECT column1 FROM table3
```

在上面的示例中，首先会执行 UNION 运算，然后再执行 INTERSECT 运算，确保查询的顺序是正确的。

---

## 2.联结

和集合运算不同，联结是针对列而言，集合运算是针对行而言的

---

### 1. 内联结 INNER JOIN

```sql
SELECT * FROM
A INNER JOIN B ON A.x=B.x
```

---

### 2.外联结 OUTER JOIN 

```SQL
SELECT * FROM
A <LEFT/RIGHT> OUTER JOIN B ON A.x=B.x
```

与内联结不同，外联结会取出一张表中所有的内容，对于不能匹配上的另一张表会用 NULL 代替

那个是主表这是根据 LEFT/RIGHT 来看

---

### 3.多表联结

```sql
SELECT * FROM
A INNER JOIN B ON A.x=B.x
B INNER JOIN C ON B.x=C.x
```

---

### 4.交叉联结 CROSS JOIN

```SQL
SELECT * FROM
A INNER JOIN B ON A.x=B.x
```

取出所有组合，行数的两表行数相乘，没有联结条件

---

### 5.过时语法

使用 where  来进行联结虽然可行，但是这属于过时语法，不建议使用

---

# 8.SQL 高级处理

## 1.窗口函数

窗口函数也成为 OLAP 函数 Online Analytical Processing

```sql
<窗口函数> OVER ([PARTITION BY <列清单>] ORDER BY <排序用列清单>)
```

---

用来计算记录排序的函数

```sql
select product_id,product_name,product_type,sale_price,
       rank() over (
           order by sale_price
           ) as rank_
from product;
```

PARTITION BY 能够是设定排序的对象范围 --横向上对表进行分组

ORDER BY能指定按照哪一列，何种顺序进行排序 --决定了纵向排序的规则

RANK 不会减少原表中的记录行数

窗口函数兼具分组和排序两种功能，窗口表示的是范围，通过 PARTITION BY 分组后的集合称之为窗口

---

PARTITION BY 不是必选项，去掉后会将查询到的数据视作为整个窗口

---

RANK 在遇到相同次位的记录，会跳过之后的次位

>1 - 1 - 3 - 3 - 5

不希望跳过可以使用

DENSE_RANK

>1 - 1 - 2 - 2 - 3

希望赋予唯一的不重复的可以用

ROW_NUMBER

>1 - 2 - 3 - 4 - 5

---

这些函数都是不需要参数的

---

原则上窗口函数只能在 SELECT 字句中使用

在其他地方使用是没有意义的

---

也可以用聚合函数来当作窗口函数使用

---

SUM 函数会作为累计的情况出现，比如第一列会计算第一列的数值，第二列就会计算包括它之前所有指定列的数据

AVG 函数也是，分子累加，分母为包括自己前面的列数

---

