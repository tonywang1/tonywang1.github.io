---
title: maven浅尝辄止1
date: 2019-09-17 20:28:36
urlname: maven1
tags: [MAVEN]
categories: MAVEN
---


# maven浅尝辄止1
 
---


## 1.什么是maven？
   Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。
[参考](https://baike.baidu.com/item/MAVEN)
 　
## 2. maven能帮我们解决什么问题 
 
> 1. 项目统一构建 - 统一生成项目结构
> 2. jar包统一管理 - 自动下载jar包、对应的jar包的依赖、jar包不同版本冲突问题、jar包版本统一管理
> 3. 项目版本统一管理 - Release和SNAPSHOT版本即稳定版本和快照版本
> 4. 项目的编译、测试、打包 、安装、发布等管理
> 5. 项目不同环境配置的管理
> 6. 不同项目之间依赖管理
 
## 3. maven中如何使用profile来解决项目不同环境的问题

### profile使用原理：通过激活某个profile来加载对应的属性，达到不同环境加载不同属性作用
> 1、命令激活：通过打包的时候使用 -p profile名称指定该profile对应的环境的变量
> 2、属性加载：不同profile定义不同属性或者加载不同的属性文件-***代码3.1***
> 3、属性引用：在2中定义的属性或者加载的属性文件的内容可以通过`${定义的属性名称}`引用
> 4、属性文件赋值：利用2中加载属性与resource中的filter连用，达到对属性文件赋值的作用，即代码2中的属性文件中所有带有`${定义的属性名称}`引用的变量，在编译阶段都会用实际的属性代替，***代码3.2***
 
``` xml 
代码3.1
<profiles>
	<profile>
		<id>dev</id>
		<!-- 默认激活开发配制，使用config-dev.properties来替换设置过虑的资源文件中的${key} -->
		<activation>
			<activeByDefault>true</activeByDefault>
		</activation>
		<build>
			<filters>
				<filter>config-dev.properties</filter>
			</filters>
		</build>
         <properties>
             <mysql.username>mysql</mysql.username>
        </properties>
	</profile>
	<profile>
		<id>test</id>
		<build>
			<filters>
				<filter>config-test.properties</filter>
			</filters>
		</build>
         <properties>
             <mysql.username>mysql</mysql.username>
        </properties>
	</profile>
	<profile>
		<id>prod</id>
		<build>
			<filters>
				<filter>config-prod.properties</filter>
			</filters>
		</build>
         <properties>
             <mysql.username>mysql</mysql.username>
        </properties>
	</profile>
</profiles>

``` 
``` xml
*** 代码3.2-这里filtering属性=true代表所有资源文件中带有placehoder的变量都会用maven的便来给你代替 ***
<resources>
    <!-- Resource Filter -->
    <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
    </resource>
</resources>
```
## 4. maven中如何使用resource打不同的资源文件来解决不同环境问题

- 默认属性文件目录处理：根据profile属性拼接并打包对应的配置文件如代码***4.1***
- 源文件目录下配置文件处理：拷贝java源文件目录下的配置文件

``` xml
代码4.1
<resources>
    <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
        <excludes>
            <exclude>application.properties</exclude>
            <exclude>application-dev.properties</exclude>
            <exclude>application-test.properties</exclude>
            <exclude>application-prod.properties</exclude>
        </excludes>
    </resource>

    <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
        <includes>
            <include>application.properties</include>
 <include>application-${profiles.active}.properties</include>
        </includes>
    </resource>

</resources>

```

``` xml
代码4.2
<resource>    
    <!-- 设定主资源目录  -->    
    <directory>src/main/java</directory>    

    <!-- maven default生命周期，process-resources阶段执行maven-resources-plugin插件的resources目标处理主资源目下的资源文件时，只处理如下配置中包含的资源类型 -->     
	 <includes>
		  <include>**/*.xml</include>
	 </includes>  
         
    <!-- maven default生命周期，process-resources阶段执行maven-resources-plugin插件的resources目标处理主资源目下的资源文件时，不处理如下配置中包含的资源类型（剔除下如下配置中包含的资源类型）-->      
	<excludes>  
		<exclude>**/*.yaml</exclude>  
	</excludes>  

    <!-- maven default生命周期，process-resources阶段执行maven-resources-plugin插件的resources目标处理主资源目下的资源文件时，指定处理后的资源文件输出目录，默认是${build.outputDirectory}指定的目录-->      
    <!--<targetPath>${build.outputDirectory}</targetPath> -->      

    <!-- maven default生命周期，process-resources阶段执行maven-resources-plugin插件的resources目标处理主资源目下的资源文件时，是否对主资源目录开启资源过滤 -->    
    <filtering>true</filtering>     
</resource> 

```
 
## 4. maven中父子项目依赖关系

### ***父子项目依赖的多模块处理作用:*** 
> 复杂项目通过合理拆分达到代码复用、方便管理

- 父项目设置-如代码4.1 ***packaging=pom***
modules中包含所有子项目，里面的名字为各个项目的*artifactId*参数
- 子项目设置-如代码4.2，里面的参数为父项目的相应值，子项目不指定*groupId*，会自动从父项目中继承
- 子项目之家可以通过*dependency*来进行引用
  

``` xml 
代码4.1
<packaging>pom</packaging>

<modules>
    <module>pojo</module>
    <module>web</module>
    <module>dao</module>
    <module>service</module>
</modules>
```

``` xml
代码4.2
<parent>
    <artifactId>online_retailers</artifactId>
    <groupId>com.hd</groupId>
    <version>1.0-SNAPSHOT</version>
</parent>

```

## 5.maven中必要节点的作用说明

- filters：定义指定filter属性的位置，例如filter元素赋值filters/filter1.properties,那么这个文件里面就可以定义name=value对，这个name=value对的值就可以在工程pom中通过${name}引用，默认的filter目录是${basedir}/src/main/fiters/
- packing：主要打什么类型的包-pom、jar(默认)、war
  > - pom - 父类型都为pom
  > - jar - 内部调用或者服务使用
  > - war - 部署的项目
- modelVersion:模型版本，暂时固定为4.0.0
- groupId： 
  > 1 项目构建的时候第一次会根据这个参数构建目录
  > 2 打包后jar位置为：*repository*后面的目录为-groupId所设置的目录 、在后面为*artifactId*，在后面为版本*version* ，所以groupId+artifactId+version决定了jar包的位置

- artifactId：项目的唯一ID
- version：版本号，分release和snapshot版本
- name:项目的显示名称，idea的maven视图中会显示这个名称


 
## 6. maven中内置的属性变量

内置属性(Maven预定义,用户可以直接使用)
> - `${basedir}`表示项目根目录,即包含pom.xml文件的目录;
> - `${version}`表示项目版本;
> - `${project.basedir}`同${basedir};
> - `${project.baseUri}`表示项目文件地址;
> - `${maven.build.timestamp}`表示项目构件开始时间;
 
 POM属性
> - `${project.build.directory}`表示主源码路径
> - `${project.build.sourceEncoding}`表示主源码的编码格式
> - `${project.build.sourceDirectory}`表示主源码路径
> - `${project.build.finalName}`表示输出文件名称
> - `${project.version}`表示项目版本,与${version}相同
 
 Java系统属性(所有的Java系统属性都可以使用Maven属性引用)
 
> - 使用mvn help:system命令可查看所有的Java系统属性;
> - System.getProperties()可得到所有的Java属性;
> - ${user.home}表示用户目录

环境变量属性(所有的环境变量都可以用以env.开头的Maven属性引用)
> -使用mvn help:system命令可查看所有环境变量;
> - `${env.JAVA_HOME}`表示JAVA_HOME环境变量的值;


## 8.maven的生命周期，以及执行规则

## 9.SNAPSHOT版本和release版本区别

## 10.依赖包的scope取值说明
 
> - compile，缺省值，适用于所有阶段，会随着项目一起发布。
> - provided，类似compile，期望JDK、容器或使用者会提供这个依赖。如servlet.jar。
> - runtime，只在运行时使用，如JDBC驱动，适用运行和测试阶段。
> - test，只在测试时使用，用于编译和运行测试代码。不会随项目发布。
> - system，类似provided，需要显式提供包含依赖的jar，Maven不会在Repository中查找它。
		
## 11.自定义属性使用

## 12.常用的maven插件有哪些，作用？



 