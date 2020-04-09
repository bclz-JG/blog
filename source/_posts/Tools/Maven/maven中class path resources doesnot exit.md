---
title: maven出现class path can not open beacuse it does not exits
comments: true
date: 2019-07-22 17:30:48
tags: 
 - maven
 - spring-dao
category: 
 - tools
---
### class path resources [xxxxx.class]can not open beacuse it does not exits
**原因是:** 
  配置中没有自动编译src的java目录的除Java外的文件
***
**解决办法：** 
  将IDEA maven项目中src源代码下的资源文件编译进target目录classes文件夹具体操作方法就是：配置maven的pom文件配置，在<build>节点下添加<resources>代码：
***
**也就是**
```
<build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.*</include>
                </includes>
                <excludes>
                    <exclude>**/*.java</exclude>
                </excludes>
            </resource>
        </resources>
</build>
```
***
**clean project**
解决