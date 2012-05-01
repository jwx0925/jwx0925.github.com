---
layout: post
title: 初体验Jndi+DataSource
description: 初体验Jndi+DataSource，Jndi+DataSource的优点，resource-ref的作用
keywords: Jndi, DataSource,resource-ref
category : J2EE
tags : [J2EE],[DataSource]
---

刚开始写web应用的是狗都是在程序里写好数据库的配置，后来先进了点，写一个db.properties文件，文件里配置好数据库的相关信息。这时候，数据库的配置和应用程序已经解耦合了。但实际上，应用程序还是和数据库连接耦合性比较高，因为你必须要了解这些：



