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