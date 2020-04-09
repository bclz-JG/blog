---
title: 设置echarts图表的值·标准线·颜色
date: {{date}}
tags: [web,echarts,javaScript]
categories: Develop
---

### 正文

---
前端开发中，经常需要用echarts图表制作一些统计图，关于大多属性在网上随处可见，此处记录一下时常用到却又不好找的几个属性：

- 标准线markLine

记得第一次不知道标准线这个属性的名字叫markLine,查了很资料后才发现有这个属性，基本的内容有以下这些
```
markLine : {
    itemStyle: {
        normal: {
            lineStyle: {
                type: 'solid', color: '#f00' //设置markLine颜色,此处的颜色只针对标准线，不影响数据线的颜色
            }
        },
    },
    data : [
        {yAxis:maxValue, name: '警界值'}
    ]
},
```
markLine这个属性属于echarts的一个大属性`serious`，在option中的位置如下:
```
series : [
    {
        name:'name',
        type:'line',
        symbol:'circle',
        symbolSize:'8',
        data:[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        markLine : {
　　       data : [
　　　　       {yAxis:markValue, name: '警界值'}
　　       ]
        }
    }
]
```
