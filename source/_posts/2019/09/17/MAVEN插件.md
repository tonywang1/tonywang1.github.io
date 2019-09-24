---
title: MAVEN插件总览
date: 2019-09-17 20:28:36
urlname: maven_plugin
tags: [MAVEN]
categories: MAVEN
---
 
# maven插件知识总览
 
---


## 1.对于maven插件的了解
插件官网： [http://maven.apache.org/plugins/index.html](http://maven.apache.org/plugins/index.html)
 Maven 本质上是一个插件框架，它的核心并不执行任何具体的构建任务，所有这些任务都交给插件来完成。如编译源代码是由
 maven-compiler-plugin完成的，每个任务阶段对应了一个插件目标（goal），每个插件会有一个或者多个目标，如maven- compiler-plugin的compile目标用来编译位于src/main/java/目录下的主源码，testCompile目标用来编译位于src/test/java/目录下的测试源码
 　
## 2.常用插件

### 1）  maven-antrun-plugin
http://maven.apache.org/plugins/maven-antrun-plugin/

### 2） maven-archetype-plugin
http://maven.apache.org/archetype/maven-archetype-plugin/
### 3）  maven-assembly-plugin
http://maven.apache.org/plugins/maven-antrun-plugin/
### 4） maven-dependency-plugin
http://maven.apache.org/plugins/maven-dependency-plugin/

maven-dependency-plugin最大的用途是帮助分析项目依赖，dependency:list能够列出项目最终解析到的依赖列表，dependency:tree能进一步的描绘项目依赖树，dependency:analyze可以告诉你项目依赖潜在的问题，如果你有直接使用到的却未声明的依赖，该目标就会发出警告。maven-dependency-plugin还有很多目标帮助你操作依赖文件，例如dependency:copy-dependencies能将项目依赖从本地Maven仓库复制到某个特定的文件夹下面。

### 5）maven-help-plugin
官网插件地址：[http://maven.apache.org/plugins/maven-help-plugin/](http://maven.apache.org/plugins/maven-help-plugin/)

maven-help-plugin是一个小巧的辅助工具，最简单的help:system可以打印所有可用的环境变量和Java系统属性。help:effective-pom和help:effective-settings最 为有用，它们分别打印项目的有效POM和有效settings，有效POM是指合并了所有父POM（包括Super POM）后的XML，当你不确定POM的某些信息从何而来时，就可以查看有效POM。有效settings同理，特别是当你发现自己配置的 settings.xml没有生效时，就可以用help:effective-settings来验证。此外，maven-help-plugin的describe目标可以帮助你描述任何一个Maven插件的信息，还有all-profiles目标和active-profiles目标帮助查看项目的Profile。

### 6）maven-resources-plugin
> - 官网插件地址：[http://maven.apache.org/plugins/maven-resources-plugin](http://maven.apache.org/plugins/maven-resources-plugin/)
> - 作用：为了使项目结构更为清晰，Maven区别对待Java代码文件和资源文件，maven-compiler-plugin用来编译Java代码，maven-resources-plugin则用来处理资源文件。默认的主资源文件目录是src/main/resources，很多用户会需要添加额外的资源文件目录，这个时候就可以通过配置maven-resources-plugin来实现。此外，资源文件过滤也是Maven的一大特性，你可以在资源文件中使用${propertyName}形式的Maven属性，然后配置maven-resources-plugin开启对资源文件的过滤，之后就可以针对不同环境通过命令行或者Profile传入属性的值，以实现更为灵活的构建。

```xml
代码6.1 设置资源文件编码
<plugin>
    <artifactId>maven-resources-plugin</artifactId>
    <configuration>
        <encoding>UTF-8</encoding>    <!--配置资源文件编码-->
    </configuration>
</plugin>

``` 

### 7）versions-maven-plugin

 官网插件地址：[http://mojo.codehaus.org/versions-maven-plugin/](http://mojo.codehaus.org/versions-maven-plugin/)
 
很多Maven用户遇到过这样一个问题，当项目包含大量模块的时候，为他们集体更新版本就变成一件烦人的事情，到底有没有自动化工具能帮助完成这件 事情呢？（当然你可以使用sed之类的文本操作工具，不过不在本文讨论范围）答案是肯定的，versions-maven- plugin提供了很多目标帮助你管理Maven项目的各种版本信息。例如最常用的，命令 mvn versions:set -DnewVersion=1.1-SNAPSHOT 就能帮助你把所有模块的版本更新到1.1-SNAPSHOT。该插件还提供了其他一些很有用的目标，display-dependency- updates能告诉你项目依赖有哪些可用的更新；类似的display-plugin-updates能告诉你可用的插件更新；然后use- latest-versions能自动帮你将所有依赖升级到最新版本。最后，如果你对所做的更改满意，则可以使用 mvn versions:commit 提交，不满意的话也可以使用 mvn versions:revert 进行撤销。

## 8 war包相关的例子-执行打war阶段的时候自动执行

- 官网地址：[https://maven.apache.org/plugins/maven-war-plugin](https://maven.apache.org/plugins/maven-war-plugin/)
- 所有的路径都是相对于pom文件的路径

 <!-- maven-war-plugin：mvn install可以将项目打成war包 -->
```xml

<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-war-plugin</artifactId>
<configuration>
    <webResources>
        <resource>
            <filtering>true</filtering>
            <!-- 需要打包的目标文件路径 -->
            <directory>src/main/webapp</directory>
<!-- 指定build资源具体目录，默认是base directory。 -->
            <targetPath>WEB-INF</targetPath>
            <!-- 打包的文件叫什么 -->
            <includes>
                <include>**/web.xml</include>
            </includes>
        </resource>
    </webResources>
    <!-- 打包后放在这个目录下 -->
<warSourceDirectory>src/main/webapp</warSourceDirectory>
    <webXml>src/main/webapp/WEB-INF/web.xml</webXml>
</configuration>
</plugin>
```

```xml
 说明：指定打包的目录和对应的web.xml文件
<plugin>
    <artifactId>maven-war-plugin</artifactId>
    <configuration>
        <webResources>
            <resource>
                <directory>src/main/webapp</directory>
                <filtering>true</filtering>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </webResources>
    </configuration>
</plugin>

```
## 9 编译插件使用实例-执行编译阶段的时候会自动执行

- 官网地址:[https://maven.apache.org/plugins/maven-compiler-plugin/](https://maven.apache.org/plugins/maven-compiler-plugin/)
- 使用实例：
```xml
 <plugin>                 
    <!-- 指定maven编译的jdk版本,如果不指定,maven3默认用jdk 1.5 maven2默认用jdk1.3 --> 
    <groupId>org.apache.maven.plugins</groupId> 
    <artifactId>maven-compiler-plugin</artifactId>  
    <version>3.1</version>  
    <configuration>      
        <!-- 一般而言，target与source是保持一致的，但是，有时候为了让程序能在其他版本的jdk中运行(对于低版本目标jdk，源代码中不能使用低版本jdk中不支持的语法)，会存在target不同于source的情况 -->                    
        <source>1.8</source> <!-- 源代码使用的JDK版本 -->
        <target>1.8</target> 
        <!-- 需要生成的目标class文件的编译版本 --> 
        <encoding>UTF-8</encoding><!-- 字符集编码 -->
        <skipTests>true</skipTests><!-- 跳过测试 --> 
        <verbose>true</verbose>
        <showWarnings>true</showWarnings>               
    </configuration>  
</plugin>
```
## 10 spring打可执行的插件
- 官网地址:[https://docs.spring.io/spring-boot/docs/current/reference/html/build-tool-plugins-maven-plugin.html](https://docs.spring.io/spring-boot/docs/current/reference/html/build-tool-plugins-maven-plugin.html)
- spring打包实例
```xml

<plugin>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-maven-plugin</artifactId>
	<configuration>
		<mainClass>主类</mainClass>
	</configuration>
</plugin>
```

 