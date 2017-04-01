---
layout: post
title: Integrating Kubernetes and Jenkins
category: 技术
tags: Kubernetes
keywords: Jenkins Docker Kubernetes
---

## 简介（待整理）

环境描述

||node1|node2|node3|
|-|-|-|-|
|ip|192.168.3.57|192.168.3.58|192.168.3.59|
|role|node|node|master|
|subnet|172.17.10.0/24|172.17.20.0/24|172.17.30.0/24|

private registry:`192.168.3.56:5000`



## 各种网络方案的特点

||物理机如何访问pod|缺点|其它|
|-|-|-||
|路由方式|物理机直接以pod ip访问pod|pod ip可能会改变|ip易改变的解决方案：容器网络使用service，外部网络使用 ingress + nginx|
|etcd + flannel/ovs|物理机以physical ip + service node port的方式访问pod|同上|同上|
|docker overlay||container ip可指定||

## k8s网络选择

采用直接路由方式，以node1为例，路由表要加上：

    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    172.17.30.0     192.168.3.59    255.255.255.0   UG    20     0        0 em1
    172.17.20.0     192.168.3.58    255.255.255.0   UG    20     0        0 em1

也可以使用quagga之类的软件动态添加。

## 项目结构


![kubernetes_demo_project_structure.png](/public/upload/kubernetes/kubernetes_demo_project_structure.png)

k8s-web-demo是一个很简单的j2ee示例程序，运行成功后，可以访问`/hello`，返回`hello world`

主要有以下几个点：

1. Dockerfile在项目根目录。以为Dockerfile所在的目录作为docker build容器的root目录，必须包含build时所有必要的资源（这里主要是target目录下的war包）。
2. 在k8s目录下创建项目的rc文件和service文件。

## jenkins

jenkins涉及到的配置如下

### git
	
    git@xxx:bert/k8s-web-demo.git
    
### maven

	clean install -Ptest-out -Dmaven.test.skip=true 
    
### shell

	#!/bin/bash
	set +e
    REGISTRY_ADDRESS=192.168.3.56:5000
    IMAGE_NAME=$JOB_NAME
    FULL_IMAGE_NAME=$REGISTRY_ADDRESS/$IMAGE_NAME
    /usr/bin/docker build -t $FULL_IMAGE_NAME $WORKSPACE | tee Docker_build_result.log
    echo ">>>docker push image"
    /usr/bin/docker push $FULL_IMAGE_NAME 
    ## delete old image and container in cluster
    ## delete old if exist or just update image,rolling update
    echo ">>>k8s create rc"
    /usr/local/bin/kubectl create -f $WORKSPACE/k8s/rc.yaml
    ## if not exist create
    echo ">>>k8s create service"
    /usr/local/bin/kubectl create -f $WORKSPACE/k8s/svc.yaml

