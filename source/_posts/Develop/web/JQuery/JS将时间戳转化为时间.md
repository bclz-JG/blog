---
title: JS将时间戳转化为时间的方法
date: 2020-3-11 10:10:10
tags: [javaScript, web]
categories: Develop
---

### 正文

---
记录将后端传来的时间戳在前端转化为时间的两种方法：

1. new Date()方法
````
$("#time-info").text(new Date(this.list[this.inum].markTime).FormatNew("yyyy-MM-dd hh:mm"));
````

2. 转化函数方法

````
var format = function(time, format){
    var t = new Date(time);
    var tf = function(i){return (i < 10 ? '0' : '') + i};
    return format.replace(/yyyy|MM|dd|HH|mm|ss/g, function(a){
        switch(a){
            case 'yyyy':
                return tf(t.getFullYear());
                break;
            case 'MM':
                return tf(t.getMonth() + 1);
                break;
            case 'mm':
                return tf(t.getMinutes());
                break;
            case 'dd':
                return tf(t.getDate());
                break;
            case 'HH':
                return tf(t.getHours());
                break;
            case 'ss':
                return tf(t.getSeconds());
                break;
        }
    })
}
--------------------
var time = format(time,"yyyy-MM-dd HH:mm:ss");
````