---
title: 两个markLine只显示一个的问题
date: 2020-3-18
tags: [web,echarts,javaScript]
categories: Develop
---

### 正文

---
先记一下今天遇到的一个奇奇怪怪的问题

- 这是echarts的option
````
series: [
    {
        name: "流量"，
        type: "value",
        symbolType: "cicle",
        markLine: {
            data: [{yAxix: marlValue, name: "警界值"}]
        }
    }
    {
        name: "流量"，
        type: "value",
        symbolType: "cicle",
        markLine: {
            data: [{yAxix: marlValue, name: "警界值"}]
        }
    }
]
````
展示效果就是只能看到series[1]的markline，看不到series[0]的
我找了好久找不到原因，问同事也找不到原因。
最后我们无耐地说，这两名字改不一样的试试
结果，真就可以了

- 结论

**还真是名字相同引起的只能显示一个markLine**
!!!!!!

### 后记

---
也不知道是什么原理，但细微之处，要学的东西还很多