---
title: 找出第n大的值
date: 2018-3-2 9:9:9
tags: [DB,sql,Oracle]
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

- 使用minus

Oracle没有像mysql一样的limit & offset方法可用。可以利用minus
`minus`的中文意思是减去，A minus B 即表示，在结果集A中减去结果集B
从而可以这样写sql
```
(select t.score SeconHighestScore from exam t where rownum < 3)
minus
(select tt.score SeconHighestScore from exam tt where rownum <= 1 );
```
表示在小于3的集中减去小于等于1的，从而得到2.

- not in方法

此方法类似于miuns，A not in B 意义相近，用法略有不同
````
select b.score SeconHighestScore 
from (select * from exam where rownum <= 2) b 
    where b.id not in (
        select id from exam 
        where rownum < 2);
````

