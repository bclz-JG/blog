---
title: 创建ArcSDE服务
date: 2020-3-19
tags: [ArcGIS, Oracle]
categories: services
---

### 正文

---
在windows上使用post installation wizard或者sdeservice命令创建服务后，实际上是在操作系统的注册表写了一些ArcSDE启动时候所需要的内容，其中一项就包括数据库的密码(当然该密码是以加密的方式存在的)，使用windows的服务管理器启动ArcSDE服务的时候读取的是注册表中的内容，数据库库的密码发生改变后，注册表中的密码信息没有更新，因此也就无法启动了。

### 解决办法

- 一、ArcSDE和数据库安装在一台机器上

1. 停止ArcSDE服务。

2. 使用sdeservice –o delete –d ORACLE –i esri_sde 删除ArcSDE 服务

3. 使用sdeservice –o create –d ORACLE –p password重新创建服务。

- 二、ArcSDE和数据库是分别部署在两台机器上

1.  停止ArcSDE服务。

2. 使用sdeservice –o delete –d ORACLE –i esri_sde 删除ArcSDE 服务

3. 使用Sdeservice –o create –d ORACLE,netservicename –i esri_sde –p sde –n

4.  编辑$SDEHOME/etc/dbinit.sde，修改文件为set LOCAL=netservicename(原来的可能是set ORACLE_SID=netservicename).
其中netservicename为您配置的oracle的网络服务名。

重启SDE服务即可。