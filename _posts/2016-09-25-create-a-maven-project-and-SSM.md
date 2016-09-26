---
layout: post

title:  如何创建一个maven项目并基于Maven创建SSM框架

date:   2016-09-25 22:32:00 +0800

categories: document

tag: 教程
---

* content
{:toc}


Java开发怎么能不知道怎么创建maven项目，博主创建maven项目遇到一些小难题，在此记下。


Update DateTime 2016.09.25
==

新建Maven项目
--
maven项目创建有很多种方法。

1. 打开Eclipse-->File-->New-->Maven project
2. 勾选 Create a Simple project<img src="{{ '/styles/images/maven/maven1.png' | prepend: site.baseurl }}"/>

3. 点击Next在Packaging中选择**war**<img src="{{ '/styles/images/maven/maven2.png' | prepend: site.baseurl }}"/>

4. 点击Finish，一个简单的Maven Java Project 就建立好了。

5. 然后我们建立Maven Web项目，随便建立一个Web项目，将其中的WebContent目录下所有文件都复制到Maven下的**src-->webapp**中
6. 新建index.jsp文件，一般这个时候index.jsp文件会报错，这个时候我们就需要在maven的配置文件**pom.xml**中加入一些配置。<img src="/styles/images/maven/maven3.png"/>

7. 建立了maven项目之后再Java Resource 里面竟然没有任何东西，并且Java Resource 还在报红叉。

	百度了之后才发现是因为JDK版本和Maven程序的版本不一样，博主本人的版本是

		>Dynamic Web Module --> 3.0
		>Java --> 1.7
		>javaScript -->1,0
	这个根据自己情况而定，没有固定的。

8. 在建立好项目之后，还有个问题是在pom.xml中一会报错，但没有显示具体的位置在哪，这个时候一般是仓库有问题，最**简单**的方法是将仓库删除后重新下载，然后在**Maven Update** 就可以了。

9. 在pom.xml中添加完依赖后，我们就可以部署和测试这个mavenTest。


Update DateTime 2016.09.26
==

创建SSM（Spring+SpringMVC+Mybatis）项目	
--

1. 先配置pom.xml文件

	将我们需要搭建的环境，全部都写在pom.xml。当然如果不会的话，可以到 [http://search.maven.org/]( http://search.maven.org/)上面去搜。在这里贴出基于maven的spring+springMVC+Mybatis框架的pom.xml文件。<a href="/styles/images/maven/pom.xml">pom.xml</a>编译完成后，直接**maven update**，所有的jar包就下载好了。

		<project xmlns="http://maven.apache.org/POM/4.0.0" 
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
		http://maven.apache.org/xsd/maven-4.0.0.xsd">
  			<modelVersion>4.0.0</modelVersion>
  			<groupId>com.test.maventest2</groupId>
  			<artifactId>maventest2</artifactId>
  			<version>0.0.1-SNAPSHOT</version>
  			<packaging>war</packaging>
  			<properties>
  			<!-- Spring 版本号 -->
  			<spring.version>4.0.2.RELEASE</spring.version>
  			</properties>

   			<dependencies>
    			<!-- Spring  核心jar包-->
    			<dependency>
    				<groupId>org.springframework</groupId>
    				<artifactId>spring-core</artifactId>
    				<version>${spring.version}</version>
    			</dependency>
    			<dependency>
    				<groupId>org.springframework</groupId>
    				<artifactId>spring-web</artifactId>
    				<version>${spring.version}</version>
    			</dependency>
    			<dependency>
    				<groupId>org.springframework</groupId>
    				<artifactId>spring-tx</artifactId>
    				<version>${spring.version}</version>
    			</dependency>
    			<dependency>
    				<groupId>org.springframework</groupId>
    				<artifactId>spring-jdbc</artifactId>
    				<version>${spring.version}</version>
    			</dependency>
    			<dependency>
            		<groupId>org.springframework</groupId>
            		<artifactId>spring-webmvc</artifactId>
            		<version>${spring.version}</version>
       	 		</dependency>
    			<dependency>
            		<groupId>org.springframework</groupId>
            		<artifactId>spring-aop</artifactId>
            		<version>${spring.version}</version>
       			</dependency>
    			<dependency>
					<groupId>org.springframework</groupId>
            		<artifactId>spring-context-support</artifactId>
            		<version>${spring.version}</version>
       	 		</dependency>
        		<dependency>
            		<groupId>org.springframework</groupId>
            		<artifactId>spring-test</artifactId>
            		<version>${spring.version}</version>
        		</dependency>
        	
        		<!-- 添加MyBatis依赖 -->
        		<dependency>
            		<groupId>org.mybatis</groupId>
            		<artifactId>mybatis</artifactId>
            		<version>3.3.0</version>
        		</dependency>
        	
        		<!-- JDBC连接依赖 -->
        		<dependency>
            		<groupId>mysql</groupId>
            		<artifactId>mysql-connector-java</artifactId>
            		<version>5.0.8</version>
        		</dependency>
    	        <!-- 阿里巴巴的数据库连接池 -->
        		<dependency>
            		<groupId>com.alibaba</groupId>
            		<artifactId>druid</artifactId>
            		<version>1.0.16</version>
        		</dependency>
        		<!-- spring结成mybatis -->
        		<dependency>
            		<groupId>org.mybatis</groupId>
            		<artifactId>mybatis-spring</artifactId>
            		<version>1.2.3</version>
        		</dependency>
        		<!-- jsp标准标签库 -->
        		<dependency>
            		<groupId>javax.servlet</groupId>
            		<artifactId>jstl</artifactId>
            		<version>1.2</version>
        		</dependency>
        		<dependency>
            		<groupId>log4j</groupId>
            		<artifactId>log4j</artifactId>
            		<version>1.2.16</version>
        		</dependency>
        		<dependency>
            		<groupId>org.slf4j</groupId>
            		<artifactId>slf4j-api</artifactId>
            		<version>1.6.1</version>
        		</dependency>
        		<dependency>
            		<groupId>org.slf4j</groupId>
            		<artifactId>slf4j-nop</artifactId>
            		<version>1.6.4</version>
        		</dependency>

        		<dependency>
            		<groupId>junit</groupId>
            		<artifactId>junit</artifactId>
            		<version>4.7</version>
            		<scope>test</scope>
        		</dependency>
  				<dependency>
  					<groupId>javax.servlet</groupId>
  					<artifactId>servlet-api</artifactId>
  					<version>3.0-alpha-1</version>
  				</dependency>
  			</dependencies>
      			<build>
        			<finalName>maven-ssm-web</finalName>
    			</build>
		</project>

2. 配置web.xml文件
	这里给出web.xml的配置文件

  
		<!-- 解决工程编码过滤器 -->
    	<filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    	</filter>
    	<filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    	</filter-mapping>
    
    	<!-- 配置文件所在位置 -->
    	<context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring.xml,classpath:mybatis-spring.xml</param-value>
    	</context-param>
    	<!-- Spring配置（监听器） -->
    	<listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    	</listener>
    
    	<!-- SpringMVC配置 -->
    	<servlet>
        <servlet-name>springDispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    	</servlet>
    	<servlet-mapping>
        <servlet-name>springDispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    	</servlet-mapping>
		</web-app>


3. 配置spring.xml文件。

4. 配置spring-mvc.xml文件
5. 配置mybatis-spring.xml文件
6. 配置mybatis-config.xml文件（可不选）
7. 配置mysqldb.properties文件
8. 完成项目框架。
	
	