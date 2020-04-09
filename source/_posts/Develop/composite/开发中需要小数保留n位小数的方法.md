---
title: 小数保留n位小数的方法
date: 2020-3-5 10:10:10
tags: [Oracle, Web]
categories: Develop
---

### 正文

---

在日常开发中时常会遇到取出的小数数据小数点后有很多的小数位，在一般情况展示的时候不需要这么多的小数位。通常需要保留2或3位小数就可以。
以下推荐两种方法适合大多场合（四舍五入）。

- 方法一：通过sql在取出数据时就操作

在oracle中有一个`round(munber, n)`的方法number表示数据，n表示要保留的位数
```
select round(t.date, 3) date from table t;
```
- 方法二：在前端用js来控制

`data.toFixed(n)`方法,n表示保留n位小数

```
//a = 12.347264826342 
var data = a.toFixed(3);
//data = 12.347
```
实际使用时可根据两种方法哪种更方便。