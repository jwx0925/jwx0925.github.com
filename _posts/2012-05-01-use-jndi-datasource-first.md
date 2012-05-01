---
layout: post
title: 初体验Jndi DataSource
description: 初体验Jndi DataSource,Jndi DataSource的优点,resource_ref的作用
keywords: Jndi,DataSource
category: J2EE,DataSource
tags: [J2EE,DataSource]
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

	<Resource name="jdbc/testDs" 
              auth="Container" 
              type="javax.sql.DataSource"  
              maxActive="100" 
              maxIdle="30"    
              maxWait="10000"   
              username="root"       
              password="root"
              driverClassName="com.mysql.jdbc.Driver"
              validationQuery="select 1" 
              testOnBorrow="true"
              testWhileIdle="true"
              url="jdbc:mysql://localhost:3306/test" />

其中：
　　name 表示指定的jndi名称
　　auth 表示认证方式，一般为Container
　　type 表示数据源床型，使用标准的javax.sql.DataSource
　　maxActive 表示连接池当中最大的数据库连接
　　maxIdle 表示最大的空闲连接数
　　maxWait 当池的数据库连接已经被占用的时候，最大等待时间
　　username 表示数据库用户名
　　password 表示数据库用户的密码
　　driverClassName 表示JDBC DRIVER
　　url 表示数据库URL地址

然后在代码中，就可以通过jndi获取DataSource了：

	Context initCtx = new InitialContext();
	Context envCtx = (Context) initCtx.lookup("java:comp/env");// "java:/comp/env/"是固定写法
	DataSource ds = (DataSource) envCtx.lookup("jdbc/testDs");//后面接的是context.xml中的Resource中name属性的值
　　Connection conn = ds.getConnection();

	//……

在web.xml中添加如下代码：

	<resource-ref>
		<description>DB Connection</description>
		<res-ref-name>jdbc/testDs</res-ref-name>
		<res-type>javax.sql.DataSource</res-type>
		<res-auth>Container</res-auth>
	</resource-ref>
		
其实我一直很好奇为什么要在web.xml中配置这么一段，google了一下，国内的回答基本都是水的。结果还是在StackOverflow上找到了答案:
http://stackoverflow.com/questions/2887967/what-is-resource-ref-in-web-xml-used-for
http://stackoverflow.com/questions/9078511/resource-ref-usage-in-web-xml-with-tomcat-5-5-and-spring

You can always refer to resources in your application directly by their JNDI name as configured in the container, but if you do so, essentially you wiring the container-specific name into your code. This has some disadvantages, for example, if you'll ever want to change the name later for some reason, you'll need to update all the references in all your applications, and then rebuild and redeploy them.

\<resource-ref\>introduces another layer of indirection: you specify the name you want to use in the web.xml, and depending on the container, provide a binding in a container-specific configuration file.

So here's what happens: let's say you want to lookup the java:comp/env/jdbc/primaryDB name. The container finds that web.xml has a <resource-ref> element for jdbc/primaryDB, so it will look into the container-specific configuration, that contains something similar to the following:

	<resource-ref>
	  <res-ref-name>jdbc/primaryDB</res-ref-name>
	  <jndi-name>jdbc/PrimaryDBInTheContainer</jndi-name>
	</resource-ref>

Finally, it returns the object registered under the name of jdbc/PrimaryDBInTheContainer.

The idea is that specifying resources in the web.xml has the advantage of separating the developer role from the deployer role. In other words, as a developer, you don't have to know what your required resources are actually called in production, and as the guy deploying the application, you will have a nice list of names to map to real resources.

Developers choose a local name for their datasource, and set this name in web.xml resource-ref. The deployer chooses to deploy on Tomcat, and creates or edits the context.xml file with a Resource or a ResourceLink. If he chooses to deploy on JBoss, WebLogic, or any other JEE container, he'll use what the container provides to associate the resource-ref with the actual data source.

在tomcat的官方文档中也能找到相应的文档：
http://tomcat.apache.org/tomcat-5.5-doc/jndi-resources-howto.html#JDBC_Data_Sources

什么意思呢？简单来说，就是在context.xml中配置的是global名称，而web.xml中配置的是app的本地jndi名称。web.xml中的配置起到了隔离本地和全局的作用。文中举了个例子；比如，你在同一台服务器上部署了2个同样的应用，但是是不同的版本，如果jndi的name相同，他们就会连接相同的DataSource，显然这是我们不想看到的。但是如果在web.xml配置一下<jndi-name>……</jndi-name>，这样话，虽然程序中的Jndi名称是一样的，但是实际连接的DataSource却是不同的。

另外，文中提下，Tomcat,JBoss, WebLogic的配置可能会有不同。
