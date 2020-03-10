---
title: tkmybatis集成
date: 2020-03-10 15:41:13
urlname: java-tkmybatist
tags: [JAVA]
categories: JAVA
---
 # cloud_test
 
 
 ## 描述
 spring，tk.mybatis 与PageHelper的集成
 
 參考：
 https://github.com/godlike110/tk-mybatis
 https://github.com/abel533/MyBatis-Spring-Boot
 ## 集成步骤
  - 1 添加maven
      mapper-spring-boot-starter
      pagehelper-spring-boot-starter
  - 2 添加mybatis配置MybatisConfiguration
  - 3 所有mapper继承Mapper  
  - 4 需要分页的地方调用
   PageHelper.startPage(currentPage, pageSize);
 
  - 5 分页返回的列表需要强制转换为page：
    PageInfo(list);
  - 6 实体和字段映射关系，需要在实体上添加注解 
  - 
 # 集成实例
  - [集成实例](https://github.com/tonywang1/cloud_test.git)
  
