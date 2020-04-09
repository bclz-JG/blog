---
title: 程序包com.sun.deploy.net不存在
date: 2019-3-20
tags: [Java]
categories: Develop
---

### 正文

---
在用gradle打包代码时，失败，程序包com.sun.deploy.net不存在

### 报错信息

---
````
错误: 程序包com.sun.deploy.net不存在
import com.sun.deploy.net.URLEncoder;
                         ^
````

### 方案

---
这是由于Gradle默认加载自身的依赖包而无法找到jre中的包所造成的，解决方法有两种

1. 将打包工具切换为 IntelliJ IDEA

在IDEA中按**File -> settings -> ... -> Gradle -> buile and running use**将Gradle改成IntelliJ IDEA

2. 依旧使用Gradler打包

将**import com.sun.deploy.net.URLEncoder**包删掉, 些时类中，使用URLEncoder的地方就会出错，会提示需要导入包
此时，会有两个选项，分别是：
````
1. URLEncoder (java.net)       <1.8>(rt.jar);
2. URLEncoder (com.sun.deploy.net.URLEncoder)   <1.8>(deploy.jar);
````
显然先前是2，此时应该选择1

编译成功