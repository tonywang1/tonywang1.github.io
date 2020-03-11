---
title: 架构整理
date: 2020-03-10 15:41:13
urlname: struct
tags: [架构]
categories: 架构
---

 # 架构知识整理
 
 
 ## 1、一些开源项目的记录
 
 - vm下载地址：http://www.downza.cn/soft/277470.html
 - 淘宝开源项目汇总：https://www.javazhiyin.com/33790.html
 - 阿里开源的jar：https://www.mvnjar.com/com.alibaba.middleware/list.html
 -  arthas项目：https://alibaba.github.io/arthas/arthas-tutorials?language=cn
 - mybatis通用mapper：https://gitee.com/free/Mapper/wikis/Home
 - sql解析器：https://github.com/JSQLParser/JSqlParser
 
 ## 2、项目与开源项目的契合点
 
 - 监控：Prometheus,alertmanager,
      puppet+zabbix，或者saltstack+zabbix
      open-falcon全监控
 
 - 日志：flink(实时分析，实时监控，ETL，告警 )、flume ，es存储
 - 网关：Kong、
 - 自动化运维：SaltStack
 - 自动部署： jenkins
 - 自建cdn：squid dns
 - 机器硬件信息采集：puppet
 - 自建dns ：powerdns（github上），smartdns(https://github.com/pymumu/smartdns/releases)
 - 切图工具：
 - 代码管理：svn、git(Webhooks机制做代码部署)
 - 存储工具：ceph 、TFS、FastDFS
 - 微服务：
  dubbo(https://github.com/dubbo)(https://github.com/apache/dubbo-spring-boot-project),
  spring cloud(https://www.springcloud.cc/)( https://github.com/spring-cloud ) , 
  SOFABoot
Service Mesh：Envoy
 - 代码质量：Sonar
 - 自动伸缩部署：容器、Istio、
 - 分布式追踪系统： SkyWalking 
 - 服务网格：Istio、Linkerd
 - 测试：jmeter
 - rpc：grpc 
 - 跨语言通信方案 Thrift、Protobuf、Avro
 - 深度学习框架：TensorFlow、Keras等
 - 服务器数据上报：puppet
 - 反编译：http://java-decompiler.github.io/
 - 网页流量统计网站：http://www.matomo.net.cn/category/tutorial/
 - AB测试灰度发布-*团服务治理也是参考这个的：https://github.com/CNSRE/ABTestingGateway.git
 - CAT ：应用系统实时监控以及告警 
 - HTTP/2：可以对数据压缩增加传输速度 
 - 数据库中间件：DBProxy(美团)，Atlas（https://github.com/Qihoo360/Atlas），mycat（http://www.mycat.io/），Zebra（美团）
 - 数据库sharding： Hibernate Shards 、 Ibatis-Sharding，TDDL（阿里）
 - ID生成策略:数据库自增、uuid、snowflake、依赖mysql生成自增（美团点评leaf） 
 - 分布式任务调度：https://www.xuxueli.com/xxl-job/（调度、单点、缓存、服务注册）
 - KMS统一密钥管理系统:参考https://github.com/ligson/kms/blob/master/README.md
 - camus：kafka数据导入到hdfs的解决方案


 - 服务通信框架、弹性负载均衡、服务治理代理（服务注册/发现、配置更新、访问控制、上报调用情况）、服务治理平台、
命名服务、配置中心、健康检查、
 - 限流：Guava的RateLimiter
 - 断路器、线程池隔离：Hystrix
 - 流量控制、熔断降级、系统负载保护：Sentinel (https://github.com/alibaba/Sentinel/wiki/%E4%BB%8B%E7%BB%8D)
 - 服务注册和发现：ZooKeeper、Eureka、Consul 、Nacos


 - 共享元数据、数据分级、审计、安全性以及数据保护：Atlas
权限控制：Apache Ranger
 - KV存储tair-redis集群（Codis）：https://github.com/alibaba/tair/wiki/%E4%B8%AD%E6%96%87%E4%B8%BB%E9%A1%B5
 - Codis：https://github.com/CodisLabs/codis


 - 全链路线上压测的目的主要有：参考：
① 了解整个系统的处理能力
② 排查性能瓶颈
③ 验证限流、降级、熔断、报警等机制是否符合预期并分析数据反过来调整这些阈值等信息
④ 发布的版本在业务高峰的时候是否符合预期
⑤ 验证系统的依赖是否符合预期
 - 美团可用性实践：https://tech.meituan.com/2018/04/19/trade-high-availability-in-action.html
 - 美团点评中间件技术介绍：https://awps-assets.meituan.net/mit-x/slide-bundle-2018/34/1-%E9%AB%98%E5%8F%AF%E7%94%A8%E4%B9%8B%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%8A%80%E6%9C%AF.pdf
 - 服务间调用解耦参考：https://tech.meituan.com/2018/07/26/sep-service-arrange.html
 - 美团规则引擎设计：https://tech.meituan.com/2017/06/09/maze-framework.html
 - 开源规则引擎drools：https://www.drools.org/
 - 字节码增强：https://tech.meituan.com/2019/09/05/java-bytecode-enhancement.html

 - 美团移动网络优化：https://tech.meituan.com/2017/03/17/shark-sdk.html
 - 美团定制化路由：https://tech.meituan.com/2018/09/06/oceanus-custom-traffic-routing.html
 - 美团权限系统：https://tech.meituan.com/2019/02/14/data-security-platform-construction-practice-jiangjunling.html
 - 美团日志收集：https://tech.meituan.com/2020/01/09/meituan-logan.html

 - mysql的binlog同步机制：https://github.com/alibaba/canal
 - mysql数据同步参考：https://github.com/hjx601496320/plumber
 - 数据库变更抓取：https://github.com/linkedin/databus/blob/master/README.md
1）主数据库和衍生数据库的同步 
2）将数据库数据同步到缓存中


 - ESB代表项目：JBOSS ESB,Mule,Camel 以及一些其他的esb项目
 - ESB和服务注册之间的区别：
1、两类开源项目侧重点不同，ESB侧重任务的编排，性能问题可通过异构的方式来进行规避。无法支持特别大的并发
2、服务注册侧重服务的治理，将各个服务颗粒化，各个子业务系统在程序逻辑上完成业务的编排。但是比较实用较大的并发量，因为dubbo类的只是存放服务地址。
 - 有zookeeper类的分布式通讯框架，能保证单点的失败不影响整个系统的业务调用，因为业务接口都是在各个提供服务的子系统中。

 
 ## 3、360开源项目
 
 - 1）类Redis存储系统 Pika，https://github.com/Qihoo360/pika
 - 2）日志搜索平台 Poseidon
 - 3）高性能分布式存储服务 HustStore
 -  4）抓包工具 MySQL Sniffer
 -  5）代码质量检测工具 GoReporter，https://github.com/wgliang/goreporter
 - 6）配置管理工具QConf，https://github.com/Qihoo360/QConf/blob/master/README_ZH.md
 
 ## 4、阿里巴巴开源项目
 > - 1 分布式应用服务开发的一站式解决方案 Spring Cloud Alibaba
 地址：https://github.com/spring-cloud-incubator/spring-cloud-alibaba
  > - 2 JDBC 连接池、监控组件 Druid
 地址：https://github.com/alibaba/druid
  > - 3 服务框架 Dubbo
 地址：https://github.com/alibaba/dubbo
  > - 4 企业级流式计算引擎 JStorm
 地址：https://github.com/alibaba/jstorm
  > - 5 分布式数据层 TDDL
 地址：https://github.com/alibaba/tb_tddl
  > - 6 Java 图片处理类库 SimpleImage
 地址：https://github.com/alibaba/simpleimage
 > -  7 开源 Java 诊断工具 Arthas
  > - 8.动态服务发现、配置和服务管理平台 Nacos
 地址：https://nacos.io/en-us/
 > -  9.Java 解析 Excel 工具 easyexcel
 地址：https://github.com/alibaba/easyexcel
  > - 10.高可用流量管理框架 Sentinel
 地址：https://github.com/alibaba/Sentinel
  > - 11.基于多维度 Metrics 的系统度量和监控中间件 SOFALookout
 地址：https://github.com/alipay/sofa-lookout
  > - 12.基于 Spring Boot 的研发框架 SOFABoot
 地址：https://github.com/alipay/sofa-boot
  > - 13 轻量级分布式数据访问层 CobarClient
 地址：https://github.com/alibaba/cobarclient

 ## 5、关键词介绍
  - CQRS：CQRS（Command Query Responsibility Segration）架构，大家应该不会陌生了。简单的说，就是一个系统，
  从架构上把它拆分为两部分：命令处理（写请求）+查询处理（读请求）。然后读写两边可以用不同的架构实现，
  以实现CQ两端（即Command Side，简称C端；Query Side，简称Q端）的分别优化。CQRS作为一个读写分离思想的架构，
  在数据存储方面，没有做过多的约束
  - MDM：Master Data Management，翻译为主数据管理或元数据管理
  高性能规则：避开网络开销（IO），避开海量数据，避开资源争夺
  
 
  
  ## 6、问题整理
 - 1 解决APP中DNS劫持和公网DNS依赖导致的问题
自己维护域名和ip对应关系表，通过检测找出速度最快的，然后通过ip进行接口访问
 - 2 APP通过长连接提升访问速度
APP通过TCP访问中间代理长连服务器（IP直连，减少公网DNS依赖），
然后代理服务器通过http短链接访问我们自己的服务器（1 建设专线提高访问速度，2自建DNS减小公网DNS的依赖）， 
 - 3 HTTP流量定制化路由，可以参考AB测试灰度发布,nginx添加lua，根据不同规则进行路由
HTTP负载均衡-LB-美团的Oceanus（可以进行拆分为：4层LB、7层LB）


  