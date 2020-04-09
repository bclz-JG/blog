---
title: spring boot中的时间控件daeTimePicker
date: 2020-4-8
tags: [Spring Boot, javaScript]
categories: Frame
---

### 正文

---
学习一个好用的时间选择控件`dateTimePicker`,由于他是spring boot的一个控件，因些在spring boot项目中使用十分方便

### 使用前提

---
添加query.min.js、datetimepicker.main.js和datetimepicker.css到页面中
````
<link rel="stylesheet" type="text/css" href="jquery.datetimepicker.css"/ >
<script src="jquery.main.js"></script>
<script src="jquery.datetimepicker.main.js"></script>
````

### 使用方法

---
通常都当作时间选择器，作为输入框的一种

````
<input type="text" value="2020-05-15 21:05" id="datetimepicker">
$('#datetimepicker').datetimepicker({
    format: 'yyyy-mm-dd hh:ii'
});
````
也可以作为组件使用
````
<div class="input-append date" id="datetimepicker" data-date="12-02-2012" data-date-format="dd-mm-yyyy">
    <input size="16" type="text" value="12-02-2012" readonly>
    <span class="add-on"><i class="icon-th"></i></span>
</div>
$('#datetimepicker').datetimepicker();
````
还有其它使用形式，大同小异
重要的是
>$(#元素id).datetimepicker();

剩下的就是在datetimepicker的属性（也可以叫“参数”）上作文章

### 属性设置方法

---
简单介绍一下它的属性设置的方法
````
$("#startTime").datetimepicker({
    language: 'zh-CN',  //设置语言，默认为en
	format: 'yyyy-mm-dd hh:ii',
	autoclose:true,
	pickerPosition:'bottom-right', // 样式
	minView: 0,    // 显示到小时
	initialDate: new Date(),  // 初始化日期
	todayBtn: true  //默认显示今日按钮
});
````
总之就是`属性名:属性值`用`,`隔开

### 属性介绍

----
datetimepicker有众多属性，介绍几种常用的

1. formate      -格式
---
String类型 
默认值: ‘mm/dd/yyyy’ 
这个是最重要最常用的属性之一了。控制显示格式就是固定值得个性化搭配。 

比如，显示 2016-04-21 19：21
````
$("#demo").datetimepicker({
    format: 'yyyy-mm-dd hh:ii'
});
````
如果你喜欢反人类的格式也可以
````
$("#demo").datetimepicker({
    format: 'hh-yyyy-ii mm:dd'
});
````
开个玩笑，意思是你可以根据不同的组全设计自己想要的样子

符号	|      意义
————————————————————————————————————————
P	    |   12小时制且大写(‘AM’ or ‘PM’)
s	    |   秒，前面不补0
ss	    |   秒，前面补0
i	    |   分，前面不补0
ii	    |   分，前面补0
h	    |   时，24小时制，前面不补0
hh	    |   时，24小时制，前面补0
H	    |   时，12小时制，前面不补0
HH	    |   时，12小时制，前面补0
d	    |   日，前面不补0
dd	    |   日，前面补0
m	    |   月，数字表示，前面不补0 如：4
mm	    |   月，数字表示，前面补0 如：04
p	    |   12小时制且小写(‘am’ or ‘pm’)
M	    |   月，缩写表示，前面补0 如：Apr
MM	    |   月，全称表示，前面补0 如：April
yy	    |   年，后两位 如：16
yyyy    |   年，全部 如：2016

2. weekStart    -一周从哪天开始
---
Integer类型 
默认值：0 
0（星期日）到6（星期六） 
````
$("#demo").datetimepicker({
    weekStart: 3
});
````

3. daysOfWeekDisabled       — 一周的周几不能选
---
String, Array类型 
默认值：”, []
0（星期日）到6（星期六） 
可以有多个。
````
$("#demo").datetimepicker({
    daysOfWeekDisabled: [0,4,6]
    //daysOfWeekDisabled: '0,4,6'
});
````
两种写法都可以

4. startView    — 选完时间首先显示的视图
---
Number, String类型 
默认值：2, ‘month’
日期时间选择器打开之后首先显示的视图。

值	 |  意义
————————————————————————
0	 |  从小时视图开始，选分
1	 |  从天视图开始，选小时
2	 |  从月视图开始，选天
3	 |  从年视图开始，选月
4	 |  从十年视图开始，选年

5. minView      — 最精确的时间
---
Number, String类型 
默认值：0, ‘hour’
日期时间选择器所能够提供的最精确的时间选择视图。

值	 |  意义
——————————————————————————
0	 |  从小时视图开始，选分
1	 |  从天视图开始，选小时
2	 |  从月视图开始，选天
3	 |  从年视图开始，选月
4	 |  从十年视图开始，选年

还有其它属性可以参考官方文档->[biu~](https://www.bootcss.com/p/bootstrap-datetimepicker/#options)