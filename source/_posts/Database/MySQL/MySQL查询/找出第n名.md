---
title: 找出第n大的值
date: 2018-3-2 9:9:9
tags: [DB,sql,MySQL]
categories: Dataase
---

### 正文

---
日常工作中，我们会遇到找出一张表中第2大的值，或者第n大的值
例如在以下表中找出第二高的语文成绩：
```
exam
+----+--------+---------+
| Id | score  | lesson  |
+----+--------+---------+
| 1  | 90     | Chinese |
| 2  | 200    | Chinese |
| 3  | 300    | math    |
+----+--------+---------+
```

### 思路

- steps one

先找出所有语文成绩：
```
select *
from exam
where lesson = 'Chinese'
```
- steps two

考虑到实际情况中，成绩可能存在一样的值，所以使用distinct对成绩进行去重

- 1. ideas one

先找出最大值a，再找出小于a的最大值，就是第二大值
max(列名)可以返回该列的最大值
可以用以下sql得到语文最大值
```
select max(t.score) form exam t where t.lesson='Chinese';
```
然后再找出小于a的最大值就是第二大值
```
select max(distinct t.score) form exam t
where t.lesson='Chinese' and 
    t.score < (select max(distinct n.score) from exam n where n.lesson='Chinese';
```
- 2. ideas two 使用limit和offset

 - limit n 子句表示查询结果返回前n条数据
 - offset n 表示跳过前n条数据
 
 因此: limit y offset x 表示查询结果跳过前x条数据，读取前y条数据
 使用limit和offset，降序排列再返回第二条记录可以得到第二大的值
```
select distinct t.score
from exam t
where t.lesson='Chinese'
order by t.score desc
limit 1 offset 1
```
- 3. 特殊情况

如果没有第二高的成绩就返回空值。可以用函数`ifnull`来处理特殊情况。
> ifnull(a,b)函数解释:
如果value1不是空，结果返回a
如果value1是空，结果返回b

此处适用的sql即为：
```
select ifnull(result, null) as '2th of Chinese score';
```
result即为上面的查询结果，我们可以直接将sql填充进去
```
select ifnull(
    (select max(distinct t.score) from exam t
    where t.score < (select max(distinct n.score) from exam n where n.lesson='Chinese')
        and t.lesson='Chinese')
    , null)
as '2th of Chinese score';
```
### 总结

`limit y offset x` = `查询结果跳过前x条取y条数据`