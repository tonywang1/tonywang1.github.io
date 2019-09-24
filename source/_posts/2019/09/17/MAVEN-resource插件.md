---
title: MAVEN插件
date: 2019-09-17 20:28:36
urlname: maven_plugin_resource
tags: [MAVEN]
categories: MAVEN
---

#RESOURCE插件说明

 
##1插件说明
 - [官方说明](http://maven.apache.org/plugins/maven-resources-plugin/resources-mojo.html)
 - resources:resources标签全名为org.apache.maven.plugins:maven-resources-plugin:3.1.0:resources
 即plugin中配置的资源的插件和resource标签的是同样的东西
 - 资源插件会拷贝对应的资源根据resource标签的设置
 
##2插件种类
Resources插件目标有三个，主要的不同点在于：拷贝资源中的一些默认值和文件的输出目录不同 

- resources:resources:拷贝main resources到main output directory,它绑定了process-resources生命周期阶段，当执行Compiler:compile插件目标前就会自动执行此阶段。
- resources:testResources：拷贝test resources到test output directory，它绑定了process-test-resources生命周期阶段，当执行surefire:test插件目标前就会执行此阶段
- resources:copy-resources：需要自己手动配置去拷贝文件，拷贝资源去输出目录。

##3为什么要有resource插件
项目中文件通常有两种：
- 1 需要编译的java文件 
- 2 不需要编译的资源文件 

这两种文件通过不同的插件分别处理，其中第二种资源文件就是需要resource插件处理



##4resource插件能解决什么问题？

- 资源文件拷贝目录的设置 - 代码4.2
- 插件读取配置的编码情况 - 代码4.1
- 过滤器的设置 - 代码4.3
- 精确控制资源拷贝- 代码4.4
- 排除过滤 - 代码4.5
- copy-resources
- 主目录中某些资源的拷贝 - 代码4.6

```xml
代码4.1
<build>
	<plugins>
		<plugin>
		<artifactId>maven-resources-plugin</artifactId>
			<version>3.0.2</version>
			<configuration>
				<encoding>UTF-8</encoding>
			</configuration>
		</plugin>
	</plugins>
</build>
```

```xml
代码4.2
<build>
	<resources>
		<resource>
			<directory>src/main/resources1</directory>
		</resource>
		<resource>
			<directory>src/main/resources2</directory>
		</resource>
	</resources>
</build>
```
```xml
代码4.3
<build>
	<filters>
		<filter>filter-values.properties</filter>
	</filters>
	<resources>
		<resource>
			<directory>src/main/resources</directory>
			<filtering>true</filtering>
		</resource>
	</resources>
</build>

```

```xml
代码4.4
  <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
              <include>**/*.txt</include>
              <include>**/*.rtf</include>
            </includes>
            <excludes>
              <exclude>**/*.bmp</exclude>
              <exclude>**/*.jpg</exclude>
              <exclude>**/*.jpeg</exclude>
              <exclude>**/*.gif</exclude>
            </excludes>
        </resource>
    <resources>
```

```xml
代码4.5
<!-- 过滤后缀为pdf和swf的文件 -->
<plugins>
	<plugin>
		<artifactId>maven-resources-plugin</artifactId>
		<version>3.0.2</version>
		<configuration>
			<encoding>UTF-8</encoding>
			<nonFilteredFileExtensions>
				<nonFilteredFileExtension>pdf</nonFilteredFileExtension>
				<nonFilteredFileExtension>swf</nonFilteredFileExtension>
			</nonFilteredFileExtensions>
		</configuration>
	</plugin>
<plugins>
```

```xml
代码4.6
  <!-- 资源目录 -->    
<resources>    
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
</resources> 


```