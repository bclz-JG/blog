---
title: Cannot instantiate interface org.springframework.boot.SpringApplicationRunListener
date: 2020-3-21
tags: [Java, Spring Boot]
categories: Frame
---

### 正文

---
Spring boot项目启动时，遇到这样一个问题：
````
Connected to the target VM, address: '127.0.0.1:61674', transport: 'socket'
Exception in thread "main" java.lang.IllegalArgumentException: Cannot instantiate interface org.springframework.boot.SpringApplicationRunListener : org.springframework.boot.context.event.EventPublishingRunListener
	at org.springframework.boot.SpringApplication.createSpringFactoriesInstances(SpringApplication.java:423)
	at org.springframework.boot.SpringApplication.getSpringFactoriesInstances(SpringApplication.java:402)
	at org.springframework.boot.SpringApplication.getRunListeners(SpringApplication.java:388)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:302)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1162)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1151)
	at com.augurit.MainPsFacilityApplication.main(MainPsFacilityApplication.java:48)
Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.springframework.boot.context.event.EventPublishingRunListener]: Constructor threw exception; nested exception is java.lang.NoSuchMethodError: org.springframework.aop.framework.AopProxyUtils.getSingletonTarget(Ljava/lang/Object;)Ljava/lang/Object;
	at org.springframework.beans.BeanUtils.instantiateClass(BeanUtils.java:154)
	at org.springframework.boot.SpringApplication.createSpringFactoriesInstances(SpringApplication.java:419)
	... 6 more
Caused by: java.lang.NoSuchMethodError: org.springframework.aop.framework.AopProxyUtils.getSingletonTarget(Ljava/lang/Object;)Ljava/lang/Object;
	at org.springframework.context.event.AbstractApplicationEventMulticaster.addApplicationListener(AbstractApplicationEventMulticaster.java:105)
	at org.springframework.boot.context.event.EventPublishingRunListener.<init>(EventPublishingRunListener.java:55)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
	at org.springframework.beans.BeanUtils.instantiateClass(BeanUtils.java:142)
	... 7 more
Disconnected from the target VM, address: '127.0.0.1:61674', transport: 'socket'

Process finished with exit code 1
````
Cannot instantiate interface org.springframework.boot.SpringApplicationRunListener直译的意思是无法实例化xxx接口，经确定是由于依赖包冲突导致的

### 解决

---
在debug中进行问题定位，结果idea提示有两个可定位的jar包，此时问题明了
