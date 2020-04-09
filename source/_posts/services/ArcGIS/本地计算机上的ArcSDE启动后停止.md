---
title: 本地计算机上的ArcSDE(esri_sde)启动后停止
date: 2020-3-18
tags: [ArcGIS, OS]
categories: services
---

### 正文

---
#### 问题

---
系统在查询sde地图数据的时候，查询结果全部为0，于是重启sde服务，这时重启中断弹出：
**本地计算机上的ArcSDE(esri_sde)服务启动又停止，某些服务在未由其它服务或程序使用时将自动停止**

#### 查看原因

---
该问题产生的原因可查看文件：C:\Program Files(x86)\ArcGIS\ArcSDE\ora11gexe\etc\sde_esri_sde.log
~~ps:其它安装路径的以自己的路径为准~~

文件中提示密码失效：ORA-28002: the password will expire within 3 days

日志发现，是由于Oracle密码过期引起的，找到原因就好解决了

#### 解决方法

---
解决方法：修改密码或延长密码期限。有两种方法：
 1. 通过sql语句悠
 2. 通过Oracle客户端具PL/SQL修改
 
到此，重启ArcSDE服务应该已经可以正常启动。如果仍然不能重启，参照[重新创建服务](http://blog.bclz.work/blog/services/ArcGIS/%E5%88%9B%E5%BB%BAArcSDE%E6%9C%8D%E5%8A%A1/)

### 后记

---
当sde服务出现问题时，首先应当从log中查看问题产生原因