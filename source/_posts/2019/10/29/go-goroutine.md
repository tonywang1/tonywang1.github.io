---
title: GO并发处理
date: 2019-10-12 15:41:13
urlname: go_routine
tags: [GO]
categories: GO
---
#GO并发处理
 
------
## 1.协程 
 
## 2.channal

- 概念：一个channel可以理解为一个先进先出的消息队列，有长度和容量，类似一个数组
- 作用：channel用来在协程[goroutine]之前传递数据，准确的说，是用来传递数据的所有权
- 设计：一个设计良好的程序应该确保同一时刻channel里面的数据只会被同一个协程拥有，这样就可以避免并发带来的数据不安全问题
- 类型：
  > - chan T是双向channel类型，编译器允许对双向channel同时进行发送和接收。
  > -  chan<- T是只写channel类型，编译器只允许往channel里面发送数据。
  > - <-chan T是只读channel类型，编辑器只允许从channel里面接收数据。

- 定义：每一种channel类型都对应着一种简单的数据类型，进入这个channel的数据必须是这个数据类型
用法：

- For-Range：for-range语法可以用到通道上。循环会一直接收channel里面的数据，直到channel关闭

- channel阻塞：
接收方会一直阻塞直到有数据到来。如果channel是无缓冲的，发送方会一直阻塞直到接收方将数据取出。
如果channel带有缓冲区，发送方会一直阻塞直到数据被拷贝到缓冲区；如果缓冲区已满，则发送方只能在接收方取走数据后才能从阻塞状态恢复
 
## 3.select 关键字
[参考](https://www.jianshu.com/p/2a1146dc42c3)

#### 1) 执行原理：
  > - 如果有一个或多个IO操作可以完成，则Go运行时系统会随机的选择一个执行，
  > - 否则的话，如果有default分支，则执行default分支语句，
  > - 如果连default都没有，则select语句会一直阻塞，直到至少有一个IO操作可以进行

#### 2) select基本用法
```
select {
case <- chan1:
// 如果chan1成功读到数据，则进行该case处理语句
case chan2 <- 1:
// 如果成功向chan2写入数据，则进行该case处理语句
default:
// 如果上面都没有成功，则进入default处理流程
```
#### 3)作用
select就是用来监听和channel有关的IO操作，当 IO 操作发生时，触发相应的动作
#### 4) select中的break
可以中途终止case中的操作
#### 5)阻塞流程-***无default***
如果没有default时，而且所有的case中的channal都没有接收到成功操作，则协程阻塞，这个可以控制协程处理流程



## 4.定时器NewTimer、NewTicker
- NewTimer 设定一个时间间隔，到设定时间则会将当前时间传到channal中
- NewTicker 设定一个时间间隔，每隔设定时间间隔则将当前时间传到channal中

***使用***：定时器中如果时间到达与设定时间，则对应的channal会返回当时的时间，如果需要执行自定义函数需要，利用channal阻塞等待数据返回的特性实现定时器功能，执行自定义功能

```go
func TestTimer(){
	//初始化定时器
	t := time.NewTimer(5 * time.Second)
	//当前时间
	now := time.Now()
	fmt.Printf("Now time : %v.\n", now)
	for  {
		//使用 channel阻塞特性，实现定时器 定时返回值 的功能
		expire := <- t.C
		fmt.Printf("Expiration time: %v.\n", expire)
		//重置定时间时间，重新计数
		t.Reset( 3*time.Second )
	}
}
```

```go1.2
func TestTicker(){
	//初始化定时器
	t := time.NewTicker(1 * time.Second)
	//当前时间
	now := time.Now()
	fmt.Printf("Now time is : %v.\n", now)
	i:=0
	for  {
		//使用 channel阻塞特性，实现定时器 定时返回值 的功能
		expire := <- t.C
		fmt.Printf("Expiration time is : %v.\n", expire)
		i++
		if i==4{
			//结束的时候调用关闭
			t.Stop()
			return
		}
	}
}
```

## 5.WaitGroup
控制多个goroutine同时完成

## 6.协程退出处理
- chan通知退出、
- WithTimeout 超时自动取消方法、
-  WithCancel 手动取消方法：
 
**退出最终都是通过channel通知了channel来结束协程的**

