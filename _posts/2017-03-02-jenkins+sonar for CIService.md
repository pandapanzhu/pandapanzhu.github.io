---

layout: post

title:  在Windows下对Jenkins集成maven+subversion+sonar有感

date:   2017-03-02 02:32:00 +0800

categories: document

tag: 学习笔记
---

*   content
{:toc}

好久不写博文，本来是已经放弃了的，但是最近的确是被Jenkins整疯了，所以准备写一篇博文记录一下。本文将详细的介绍Jenkins+Subversion+maven+SonarQube集成CI服务器的过程，当然主要是以Windows操作系统作为基础。

# Windows下安装Jenkins和sonar

## Jenkins安装

1.  Jenkins安装方法大致分为两种，一是下载对应的安装包，双击安装包后进行安装。二是下载war包，将war包放在tomcat服务器下**（推荐）**。
2.  Jenkins一般在官网下载就好了，官网地址是：[https://jenkins.io/index.html](https://jenkins.io/index.html)。可直接下载war包。
3.  SVN、maven、MySQL这些东西就不再赘述了。可自行安装。
4.  SonarQube安装共有两个，博主安装的时候很智障的以为Jenkins能自动启动SonarQube的服务，所以只安装了Jenkins的SonarQube的插件。插件安装有两种方法，一是：在Jenkins启动之后，点击“系统管理”--&gt;"插件管理"--&gt;“可选插件”中搜索**SonarQube Scanner for Jenkins**，勾选后点击直接安装即可。但这种方法一般需要翻墙。能不能安装上完全看个人运气。
第二种方法是离线安装**（推荐）**：首先下载SonarQube插件：[ https://wiki.jenkins-ci.org/display/JENKINS/SonarQube+plugin](https://wiki.jenkins-ci.org/display/JENKINS/SonarQube+plugin)。
如下图所示：&lt;img alt="sonar" src="/styles/images/CIServer/sonar.png"&gt;
然后在Jenkins的“系统管理”--&gt;“插件管理”--&gt;“高级”中点击上传插件，将下载好的sonar插件上传上去（一般下载后的插件名字为：sonar.hpi）。重启服务器，在插件管理--&gt;“已安装”中可查看 SonarQube Plugin 是否存在。&lt;img alt="sonarPlgin" src="/styles/images/CIServer/sonarPlugin.png"&gt;

5.  SonarQube插件安装好了，需要在“系统管理”--&gt;“系统设置”--&gt;"SonarQube"进行一系列的设置。如下图：
&lt;img src="/styles/images/CIServer/sonarconfig.png"&gt;
&lt;img src="/styles/images/CIServer/sonarconfig2.png"&gt;

**注意：**
当时博主天真的认为Jenkins只要安装SonarQube的插件就能启动SonarQube。所以很想找到不安装SonarQube的方法，但现实证明我失败了，还需继续安装SonarQube才行。

## SonarQube安装

1.  sonarQube安装不分操作系统。直接访问[SonarQube官网:](https://www.sonarqube.org/downloads/)[https://www.sonarqube.org/downloads/](https://www.sonarqube.org/downloads/)选择版本下载就是。
2.  下载完成，解压之后，还需修改sonar.properties文件，在安装目录的**config**文件中。博主用的是mysql数据库，所以修改大致如下：

         sonar.jdbc.username=sonar     sonar.jdbc.password=sonar     sonar.jdbc.url=jdbc:mysql: //127.0.0.1:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance

3.  在安装目录中找到\bin\windows-x86-64\StartSonar.bat。运行即可。
&lt;img src="/styles/images/CIServer/sonarRun.jpg"&gt;

4.为了方便起见，我们可以安装Windows服务，让SonarQube跟随系统启动。（不推荐）

**注意：**在启动SonarQube服务时，需先启动MySQL服务，不然SonarQube服务时启动不了的。

# Jenkins配置

运行Jenkins之后，点击“系统管理”--&gt;"Global Tool Configuration" 去进行一系列的插件设置

## Maven配置

不说了，直接上图：&lt;img src="/styles/images/CIServer/maven1.png"&gt;&lt;img src="/styles/images/CIServer/maven2.png"&gt;

## JDK配置

&lt;img src="/styles/images/CIServer/jdk.png"&gt;

## SonarQube Runner配置

博主为了偷懒，（确切的是没有找到SonarQube Runner的安装地址） 所以直接用的自动安装。
&lt;img src="/styles/images/CIServer/sonarRunner.png"&gt;

邮件通知等相关信息博主是用的163的邮箱：

*   SMTP服务器：smtp.163.com
*   用户默认邮件后缀：@163.com

# 构建项目

## 1. 点击新建

输入项目名称和项目类型点击OK,进入项目配置页面
&lt;img src="/styles/images/CIServer/goujian.png"&gt;

## 2. 配置项目

1.  源码管理我是用的SVN来管理，如下图：
&lt;img src="/styles/images/CIServer/goujian.png"&gt;

2.  构建触发器**（什么时候down源代码到本地）**：
&lt;img src="/styles/images/CIServer/chufaqi.png"&gt;

      第一个的意思是远程触发构建，就是访问url的方式触发构建

     第二个的意思是另一个项目构建完成后，进行构建

     第三个的意思是定时去构建（不论源代码是否有新的commit）

     第四个略过

     第五个定期去构建（有新的commit，才会触发构建）

     第三个和第五个可以自己度娘一下表达式的书写。

3.  Build
&lt;img src="/styles/images/CIServer/build.png"&gt;

4.  构建设置中，邮件发送看自己。

5.  构建后操作
&lt;img src="/styles/images/CIServer/afterbuild.png"&gt;

# 参考文献

1.  [SonarQube和Jenkins的集成](http://itindex.net/detail/55522-sonarqube-jenkins)
2.  [静态代码扫描平台SonarQube简介](http://blog.csdn.net/wuxuehong0306/article/details/50847893)