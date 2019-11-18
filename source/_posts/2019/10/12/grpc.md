---
title: GRPC使用
date: 2019-10-12 15:41:13
urlname: you_know_grpc
tags: [GRPC]
categories: GRPC
---
 
# GRPC初识
---


## 1.什么是GRPC
GRPC是谷歌开源的高性能，跨语言的RPC框架，基于http2协议，交换描述语言为protobuf 


## 2.GRPC描述与说明
  GRPC的客户端可以直接调用服务端的方法，在任何的机器上，就像调用本地方法一样，因此利用GRPC可以更加方便的创建分布式的应用和服务。
 GRPC需要定义一个服务，并说明服务的方法和返回值，然后Server方实现这个接口并运行GRPC服务去处理客户端的调用，客户端可以使用stub去调用服务端的接口（方法是一样的）
## 3.什么地方适合用GBPC
- 低延时、高可用的分布式系统；
- 移动端与云服务端的通讯；
- 使用protobuf，独立于语言的协议，支持多语言之间的通讯；
- 可以分层扩展，如：身份验证，负载均衡，日志记录，监控等。
- 服务端通信用的比较多

## 4.在java中如何使用GBPC
 > 1. 使用protobuf定义接口,即proto描述文件
 > 2. 使用maven的编译插件编译proto文件，生成对应的java文件
 > 3. 启动Server端，监听指定的端口
 > 4. 启动一个或者多个Client，去调用服务端暴露的接口
 
## 5.示例代码
[实例代码](https://github.com/tonywang1/test.git)
[spring集成GRPC实例代码](https://github.com/tonywang1/test.git) 
*注意启动项目的时候需要修改spring-boot-starter-grpc对应的版本为对应模块的版本*


## 6.主要参考
- [GRPC代码库](https://github.com/grpc/grpc/blob/master/README.md)
- [GRPC文档](https://grpc.io/docs/tutorials/basic/java/)
    
 