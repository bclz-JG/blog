---
title: nginx配置路径正确但是访问404
date: 2020-3-17
tags: [Nginx, Linux]
categories: services
---

### 正文

---
今天重新搭建blog的时候，将以前配置好的nginx.conf复制进来，启动nginx后，并没有抑Get 502类似的错误 ，
而是Can not Get/blog，再三检查配置确实没有什么问题
但是network中所有的请求都是404,然后，不知道为什么就突然想到执行权限这个问题，
给*.conf都加上的执行权限
````
#chome +x nginx.conf
````
再然后，就真的可以了……

### 后记

---
Linux，真的很再意权限，要时刻铭记