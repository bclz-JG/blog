---
title: idea远程代码实时同步
date: 2019-4-5
categories: tools
tags: idea
---

## 正文

---

开发时一般都会使用windows平台，完成开发后，又要将项目打包发布在Linux平台上面去。打包工作繁琐又不方便进行调试，如果能将windows开发环境与Linux服务器环境同步起来是一件多么完美的事。
其次对于自已搭建的博客环境，在windows下写好文章提交到服务器上还要手动重启服务实在太麻烦。需要一种更方便的操作。
下面介绍一种windows主机与Linux虚拟机代码同步的方式。

### 前提

---
在windows与linux中有同一份代码。

### 步骤

---
1. 打开windows主机上的idea,工具栏tools->deployment->brower remote host
2. 点击右上方`...`新建一个连接
```
连接类型：sftp
填写虚拟机地址，username,password等信息
root path选择linux上项目的所在目录
切换到mapping
选择deployment path为根路径（可选择先配置local path）
确定
```
3. 工具栏tools->deployment->auto upload前在打✔

### 提示

此方法可以很好的利用windows的图形化界面优势。
所有Jetbrains全家桶都支持此操作
但，必须是**professional**版本