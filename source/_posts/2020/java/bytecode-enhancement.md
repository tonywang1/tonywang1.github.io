---
title: 字节码增强
date: 2020-01-20 15:41:13
urlname: java-bytecode-enhancement
tags: [JAVA]
categories: JAVA
---
 
# JAVA字节码增强
---


## 1.什么是字节码
 java源代码编译以后的class文件为字节码

## 2.如何对字节码进行修改
 asm、javassist等类库进行修改
## 3.字节码修改以后如何动态替换运行时JVM中对应的信息
pid-> javaagaent(agent加载到运行时jvm上) -> Instrumentation(Transformer替换那个class文件)  
-> ClassFileTransformer(字节码对方法进行修改) ->  


## 4.字节码增强运用场景
- 热部署：不部署服务而对线上服务做修改，可以做打点、增加日志等操作。
- 性能诊断工具：比如bTrace就是利用Instrument，实现无侵入地跟踪一个正在运行的JVM，监控到类和方法级别的状态信息。
- AOP
 
## 5.idea插件jclasslib
 查看字节码工具

## 6.主要参考
- [美团字节码增强总结文章](https://tech.meituan.com/2019/09/05/java-bytecode-enhancement.html)

    
 