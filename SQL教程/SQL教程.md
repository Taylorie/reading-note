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
