---
title: GO语言基础
date: 2019-10-12 15:41:13
urlname: go_basic
tags: [GO]
categories: GO
---
#GO语言简介
个人从学习从入手不到一周基本可以开始开发：
1 go语法基础看了一遍 
2 经常用到的难点突破一下
3 beego网路开发官方文档主要的部分看一遍

------
## 1.什么是GO语言
Go（又称 Golang）是 Google 开源的一种静态强类型、编译型语言

## 2.什么要用GO语言

> - 简单高效
> - 开发速度快
> - 容易开发并行性代码
> - 部署简单



## 3.GO如何安装
1. 下载go-https://golang.org/dl/
2. 安装并设置go的环境变量-go的bin目录添加的环境变量path中 ，C:\Go\bin
3. 执行go version ，打印go的版本，安装完成 
4. go evn，打印go的所有相关变量

## 4.基础语法记录
[基础语法学习参考](https://www.runoob.com/go/go-tutorial.html)

1) 指针、对象、值传递和引用传递

  > - go普通变量:变量直接指向存在内存中的值
  > - go指针变量**:变量指向值存储的地址，实际的内存地址
  > - new(T) 返回 T 的指针 *T， 并指向 T 的零值。-- 指针
  > - make(T) 返回的初始化的 T的引用，只能用于 slice，map，channel。 - 引用
  > - City{Id: id} == 返回对象引用
  > - new(City)    == 返回对象指针
  > - 函数的参数两种：值传递（数值的拷贝）、引用传递（指针）
   
2) 类型转换
  > - strconv - 字符串和各种数据类型的转换
  > - 整数之间转换
    整数之间强转：int := int(int32)
  > - 整数转为字符串：strconv.Itoa( ) 
  > - boolean转为字符串：strconv.FormatBool(isDebug) 
  > - 字节与字符串的转换：
    var a = []byte("hello boy")
    var b = string(a)
  > - strings:字符串的工具类
  
3) 命名规范
  > - golang的命名推荐使用驼峰命名法，必须以一个字母（Unicode字母）或下划线开头，后面可以跟任意数量的字母、数字或下划线
  > - golang中根据首字母的大小写来确定可以访问的权限。无论是方法名、常量、变量名还是结构体的名称，如果首字母大写，则可以被其他的包访问；如果首字母小写，则只能在本包中使用
  可以简单的理解成，首字母大写是公有的，首字母小写是私有的
  > - 结构体中属性名的大写-小写的json不能解析

4) json使用
  > - 类型断言:interface.(int)
  > - 反序列化，指定对应的struct，只有key的第一个字母大写才能被反序列化
  > - 序列化key必须是字符串，值为指针，则序列化为对应的值
  
5) simplejson使用
    simplejson：解析复杂json，解析时候最好从根部解析。
    参照：https://blog.csdn.net/chenghuan1990/article/details/75423325

6) struct tags修改json序列化和反序列化的对应
  > - 改变对应序列化和反序列化后的名称
  > - 指定空值得处理
  > - 是否忽略某个字段
  > - 类型判断：switch vv := v.(type) 

7)io.Reader/Writer常用
  > - 字节缓冲区，字节、字符串、Reader之间的操作
  > - net.Conn, os.Stdin, os.File: 网络、标准输入输出、文件的流读取
  > - strings.Reader: 把字符串抽象成Reader
  > - bytes.Reader: 把[]byte抽象成Reader
  > - bytes.Buffer: 把[]byte抽象成Reader和Writer
  > - bufio.Reader/Writer: 抽象成带缓冲的流读取（比如按行读写）

8) http使用
> - restclient: https://github.com/ylywyn/restclient
> - 标准库：http

 
## 5.IDEA中开发GO项目

* 1 设置GOROOT-项目相关的包都是从这个目录开始查找的
* 2 设置GOPATH-添加gopath到设置中
* 3 Setting的设置GoModule ，允许go module集成 ，设置Proxy=https://goproxy.cn
* 4 生成依赖管理文件go.mod:  go mod init -- 项目使用modules管理，并生成管理文件go.mod
  每次build会将项目中的包信息，放到这个文件中
  go mod download -- 会将项目依赖包放到go安装目录的pkg目录中，所有项目共享
  go.mod文件：为项目的依赖包，描述项目的名称和依赖的第三方的包
  go.sum文件：为项目的依赖分析文件，记录每个依赖库的版本和哈希值
* 5 将依赖包生成到本地:  go mod vendor - 将依赖包放到项目中的vendor目录中，本项目独有，idea就可以识别了 
* 6 启动项目 ： bee run -gendoc=true -downdoc=true

**注意：**
* 1 项目要用go的module形式进行管理 go mod init
* 2 项目中引入的所有包进入到包管理文件： go build（会将包信息写到go.mod中）
* 3 下载对应的包到项目的vendor目录中：go mod vendor


## 6.主要参考
* 1) [https://www.runoob.com/go/go-tutorial.html](https://www.runoob.com/go/go-tutorial.html)
* 2) [beego官网](https://beego.me/docs/install/bee.md)
* 3）[go官网](https://golang.google.cn/)
