---
title: Failed to process import candidates for configuration class
date: 2020-3-21
tags: [Java, Spring boot]
categories: Frame
---

### 正文

---
用Gradle启动项目时遇到Failed to process import candidates for configuration class……的问题，具体报错信息是这样的
````
org.springframework.beans.factory.BeanDefinitionStoreException: Failed to process import candidates for configuration class [com.augurit.MainPsFacilityApplication]; nested exception is java.lang.IllegalStateException: AutoConfigure cycle detected between org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration and org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration
	at org.springframework.context.annotation.ConfigurationClassParser.processDeferredImportSelectors(ConfigurationClassParser.java:556) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.context.annotation.ConfigurationClassParser.parse(ConfigurationClassParser.java:185) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.context.annotation.ConfigurationClassPostProcessor.processConfigBeanDefinitions(ConfigurationClassPostProcessor.java:308) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.context.annotation.ConfigurationClassPostProcessor.postProcessBeanDefinitionRegistry(ConfigurationClassPostProcessor.java:228) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.context.support.PostProcessorRegistrationDelegate.invokeBeanDefinitionRegistryPostProcessors(PostProcessorRegistrationDelegate.java:272) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.context.support.PostProcessorRegistrationDelegate.invokeBeanFactoryPostProcessors(PostProcessorRegistrationDelegate.java:92) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.invokeBeanFactoryPostProcessors(AbstractApplicationContext.java:687) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:525) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.refresh(EmbeddedWebApplicationContext.java:122) ~[spring-boot-1.5.9.RELEASE.jar:1.5.9.RELEASE]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:693) [spring-boot-1.5.9.RELEASE.jar:1.5.9.RELEASE]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:360) [spring-boot-1.5.9.RELEASE.jar:1.5.9.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:303) [spring-boot-1.5.9.RELEASE.jar:1.5.9.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1118) [spring-boot-1.5.9.RELEASE.jar:1.5.9.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1107) [spring-boot-1.5.9.RELEASE.jar:1.5.9.RELEASE]
	at com.augurit.MainPsFacilityApplication.main(MainPsFacilityApplication.java:48) [main/:na]
Caused by: java.lang.IllegalStateException: AutoConfigure cycle detected between org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration and org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration
	at org.springframework.util.Assert.state(Assert.java:70) ~[spring-core-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.boot.autoconfigure.AutoConfigurationSorter.doSortByAfterAnnotation(AutoConfigurationSorter.java:97) ~[spring-boot-autoconfigure-1.5.2.RELEASE.jar:1.5.2.RELEASE]
	at org.springframework.boot.autoconfigure.AutoConfigurationSorter.doSortByAfterAnnotation(AutoConfigurationSorter.java:100) ~[spring-boot-autoconfigure-1.5.2.RELEASE.jar:1.5.2.RELEASE]
	at org.springframework.boot.autoconfigure.AutoConfigurationSorter.sortByAnnotation(AutoConfigurationSorter.java:84) ~[spring-boot-autoconfigure-1.5.2.RELEASE.jar:1.5.2.RELEASE]
	at org.springframework.boot.autoconfigure.AutoConfigurationSorter.getInPriorityOrder(AutoConfigurationSorter.java:74) ~[spring-boot-autoconfigure-1.5.2.RELEASE.jar:1.5.2.RELEASE]
	at org.springframework.boot.autoconfigure.AutoConfigurationImportSelector.sort(AutoConfigurationImportSelector.java:241) ~[spring-boot-autoconfigure-1.5.2.RELEASE.jar:1.5.2.RELEASE]
	at org.springframework.boot.autoconfigure.AutoConfigurationImportSelector.selectImports(AutoConfigurationImportSelector.java:98) ~[spring-boot-autoconfigure-1.5.2.RELEASE.jar:1.5.2.RELEASE]
	at org.springframework.context.annotation.ConfigurationClassParser.processDeferredImportSelectors(ConfigurationClassParser.java:547) ~[spring-context-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	... 14 common frames omitted

Disconnected from the target VM, address: '127.0.0.1:51317', transport: 'socket'

Process finished with exit code 1
````
### 分析

---
可以看到，在EmbeddedWebApplicationContext中使用的spring-boot包是1.5.9版本，这与AutoConfigurationSorter注入的包是不匹配的。
### 解决

---
打开gradle.build,将spring-boot的版本号改成与AutoConfiguration相匹配的（与maven的pom.xml相似）
````
buildscript {
    ext {
        //springBootVersion = '1.5.9.RELEASE'(原来的)
        springBootVersion = '1.5.2.RELEASE'//(改过的)
    }
    repositories {
        mavenCentral()

    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }

}
````

重新Build之后发现启动正常了。
