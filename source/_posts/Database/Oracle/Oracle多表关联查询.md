---
title: Oracle多表关联查询
date: 2020-3-1 22:22:22
tags: Oracle
categories: Database
---

### 正文

日常工作中，会时常遇到需要多表关联查询的地方
如题： 

>表1: Person
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     | primary key
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
表2：Address
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     | primary key
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
要求：查询结果为 FirstName,LastName,City,State

### 分析

---
通过对比两张表可以看到他们有相同的一组属性`PersonId`

### sql

---
```
select t.FirstName, t.LastName, n.City, n.State
from Person t left join Address n
on t.PersonId = n.PersonId
```
### 注

---
left join是Oracle中最常用的多表关联查询

