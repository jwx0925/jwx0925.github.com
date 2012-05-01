---
layout: post
title: 初体验Jndi+DataSource
description: 初体验Jndi+DataSource，Jndi+DataSource的优点，<resource-ref>的作用
keywords: Jndi, DataSource,resource-ref
category : J2EE
tags : [J2EE],[DataSource]
---

刚开始写web应用的是狗都是在程序里写好数据库的配置，后来先进了点，写一个db.properties文件，文件里配置好数据库的相关信息。这时候，数据库的配置和应用程序已经解耦合了。但实际上，应用程序还是和数据库连接耦合性比较高，因为你必须要了解这些：

你肯定要链接数据库 
那么你肯定要用户名和密码 
正式的数据库和应用服务器应该是单独的人员管理，而不是开发人员 
密码会定期修改 
如果链接数据库是各自书写代码和配置，则运行环境的密码修改将会是一个噩梦，一不小心就忘记一个 
所以，大家全部到一个数据源那里获取连接。管理员只需要修改数据源的配置，而无需修改应用的配置 
如果数据库的地址变更，则同样不会影响到应用，也只是修改数据源 
开发人员无需知道正式数据库的密码

到公司后，发现成熟商业产品都是使用Jndi+DataSource，那么这样方式有什么好处呢？
对开发人员屏蔽数据库细节，只要通过JNDI取得数据源就可以了，无需关心数据库连接是如何建立的；数据源通常都提供了数据库连接池的功能。
数据库连接是一种关键的有限的昂贵的资源，而且数据库连接的建立和关闭也是很耗费系统资源的。

DataSource貌似是和应用服务器绑定的，比如tomcat、Jboss等都有自己的DataSource实现。
我试验了下tomcat(6.0)下的DataSource使用：
首先在tomcat的配置文件context.xml中的<Context></Context>间加上以下代码:


