---
layout: post

title:  如何创建一个maven项目

date:   2016-09-25 22:32:00 +0800

categories: document

tag: 教程
---

* content
{:toc}


Java开发怎么能不知道怎么创建maven项目，博主创建maven项目遇到一些小难题，在此记下。


新建Maven项目
==
maven创建有很多种方法，

1. 打开Eclipse-->File-->New-->Maven project
2. 勾选 Create a Simple project
<img src="{{ '/styles/images/maven/maven1.png' | prepend: site.baseurl }}"/>

3. 点击Next在Packaging中选择**war**
<img src="{{ '/styles/images/maven/maven2.png' | prepend: site.baseurl }}"/>

4. 点击Finish，一个简单的Maven Java Project 就建立好了。

5. 然后我们建立Maven Web项目，随便建立一个Web项目，将其中的WebContent目录下所有文件都复制到Maven下的**src-->webapp**中
6. 新建index.jsp文件，一般这个时候index.jsp文件会报错，这个时候我们就需要在maven的配置文件**pom.xml**中加入一些配置。
<img src="/styles/images/maven/maven3.png"/>

7. 建立了maven项目之后再Java Resource 里面竟然没有任何东西，并且Java Resource 还在报红叉。

	百度了之后才发现是因为JDK版本和Maven程序的版本不一样，博主本人的版本是

		>Dynamic Web Module --> 3.0
		>Java --> 1.7
		>javaScript -->1,0
	这个根据自己情况而定，没有固定的。
8. 在建立好项目之后，还有个问题是在pom.xml中一会报错，但没有显示具体的位置在哪，这个时候一般是仓库有问题，最**简单**的方法是将仓库删除后重新下载，然后在**Maven Update** 就可以了。

9. 在pom.xml中添加完依赖后，我们就可以部署和测试这个mavenTest。
	

-->Update DateTime 2016.09.25

