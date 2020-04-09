---
title: 找出重复having的用法
date: 2020-4-3
tags: [sql]
categories: Database
---

### 正文

---
在数据库开发中having的使用频率并不高，但是它能极大的节约资源成本，提高sql的执行效率

1. 首先介绍下什么是`having`
>having 是常跟在group by分组后面使用的，它出现的原因就是where关键字无法与合计函数一起使用，强化某些过滤条件

2. 除了group by分组还能与什么使用？
>最能说明与where不同的就是它能与聚合函数（sum, avg, max……等）进行过滤

3. 一定要使用having吗？
>不一定

举个例子：如题
````
编写一个 SQL 查询，查找 Person 表中所有重复的电子邮箱。
Person:
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
````
- 首先我们使用常规方法解决

````
select t.email from (
    select a.email, count(a.email) num from Person a
    group by a.email
) t where t.num > 1
````
执行结果是：
````
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
````
但是明显此方法用到两次查询，执行效率并不高（1544ms左右），下来我们使用having

- 进阶版方法

````
select t.email from person t
group by t.email
having count(t.email)>1
````
我们看到写法上也更简单一些，执行时间是（600ms左右），不止写法简单，效率也高了很多

### 结尾

---
除了找出重复之外，也可以将having使用在找出比平均成绩高的成绩，这样的用法中，请看->[biu~](http://blog.bclz.work/blog/Database/composite/%E6%89%BE%E5%87%BA%E5%A4%A7%E4%BA%8E%E5%B9%B3%E5%9D%87%E5%80%BC%E7%9A%84%E6%95%B0%E6%8D%AE/)