---
layout: w
title: Spring Boot框架SPEL表达式注入漏洞
date: 2017-06-29 15:42:14
tags:
---
pring是2003年兴起的轻量级Java开发框架。任何Java应用都可以使用该框架的核心特性，在Java EE平台之上有可用于构建Web应用的扩展。Spring Boot是Spring 的一个核心子项目，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置国内包括百度、阿里巴巴、腾讯等诸多知名企业都在使用该框架。
最近Spring Boot框架的SpEL表达式注入通用漏洞曝光，利用该漏洞，远程攻击者在服务器上可执行任意命令。
#### 受影响的版本
Spring Boot 1.1 1.2 1.3.0版本
#### 不受影响的版本
Spring Boot 1.3.1及以上版本
#### 漏洞分析
以下是官方给的漏洞说明：
![](/upload_image/3.png)
查看spring boot1.3.0的源文件ErrorMvcAutoConfiguration.java，发现出问题的地方主要在SpelView类中：
![](/upload_image/4.png)
经过分析得知在用户采用了Spring Boot启动Spring MVC项目后Spring Boot的默认异常模板在处理异常信息的时候递归解析SPEL表达式，可导致SPEL表达式注入并执行。主要问题是SpelView类中会对路径中或者名字中含有的表达式递归解析，所以在1.3.1修复时添加了NonRecursivePropertyPlaceholderHelper类，防止递归解析路径中或者名字中含有的表达式。
#### 漏洞利用
利用此漏洞，只需在SpringMVC的Controller中的参数触发一个异常，异常信息带有SPEL表达式就可以注入了。
POC：
http://localhost:8555/test.php?id=${new%20java.lang.String(new%20byte[]{78,115,102,111,99,117,115})}
漏洞证明：
![](/upload_image/5.png)
利用表达式成功创建了字符串Nsfocus.
#### 补丁分析
从spring-boot的github的项目中可以看到，这个漏洞是在这个commit中修复的：
https://github.com/spring-projects/spring-boot/commit/edb16a13ee33e62b046730a47843cb5dc92054e6
在修复的log里可以看到存着漏洞的1.3.0和1.3.1的差别：
![](/upload_image/6.png)
1.3.1创建了NonRecursivePropertyPlaceholderHelper类，防止递归解析路径中或者名字中含有的表达式。
#### 修复方法
升级Spring Boot版本至1.3.1或以上版本
#### 参考链接
http://www.freebuf.com/news/108665.html
http://www.wooyun.org/bugs/wooyun-2010-226888
https://github.com/spring-projects/spring-boot/issues/4763
