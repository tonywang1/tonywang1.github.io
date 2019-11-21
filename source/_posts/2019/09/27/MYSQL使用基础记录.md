---
title: MYSQL使用基础记录
date: 2019-11-21 19:31:12
urlname: mysql_basic
tags: [MYSQL,SQL]
categories: MYSQL
---
#MYSQL使用基础记录
 
------
## 1.变量的定义
   > - 第一种用法：set @num=1; 或set @num:=1; //这里要使用变量来保存数据，直接使用@num变量
   > - 第二种用法：select @num:=1; 或 select @num:=字段名 from 表名 where ……
   
```
	实例如：
	1）set @t_error=0; select @t_error ;
	2）select @num:=`name`  from   sys_area  where id=2  ; select @num;
```



## 2.启动事务和提交事务,回滚事务，
如果打算启动事务则，设置自动提交为0，start transaction ; commit; 

``` sql
 	# 数值1自动提交 0手动提交
	select @@autocommit;  
	#设置手动提交
	set @@autocommit=0;
	start transaction ;
		insert into dic(name ) values('1');
		insert into dic(name ) values('2');
		ROLLBACK;
	commit;
	
```
 
## 3.数据库的四个特性
特性：原子性，一致性，隔离性，持久性.
其中隔离性是不同事务之间的隔离原则，需要设置事务隔离级别来处理。

 
## 4.事务的隔离级别

 	1、 Read Uncommitted（读取未提交内容-未提交读）
    2、 Read Committed（读取提交内容-提交读）
 	3、Repeatable Read（可重读）
 	4、 Serializable（可串行化） 
 	
``` sql
	1）查看当前会话隔离级别
	select @@tx_isolation;
	2）查看系统当前隔离级别
	select @@global.tx_isolation;
	3）设置当前会话隔离级别
	set session transaction isolatin level repeatable read;
	4）设置系统当前隔离级别
	set global transaction isolation level repeatable read;
	
```
 
## 5.数据库的共享锁和排它锁

查询当前有多少事务，多少锁

```sql
 SELECT * FROM information_schema.INNODB_TRX ;
 SELECT * FROM information_schema.INNODB_LOCKS;
```

* 共享锁和排它锁不同点在于是否允许另一个事务读取被锁住的数据
* 共享锁锁住了一条id=1的数据，则其他的事务是可以读取，但不能更改
* 排它锁锁住了一条id=1的数据，其他的事务不可以读取，也不能更改，直到这个事务完成
* 如果另一个查询没有显示添加锁，则他是可以读取数据，不受上面锁影响 


***举个栗子：***
```sql
	select @@autocommit;  
	#设置手动提交
	set @@autocommit=0;
	set @t_error=0;
	select @t_error ;
	start transaction ;
		select * from dic for update ;
		insert into dic(name ) values('1');
		insert into dic(name ) values('2');
		ROLLBACK;
	commit;
```

## 6.外键删除
外键不能直接删除，必须先删除外键然后删除相应的索引

```sql
 #查看表的外键以及外键名称
 show create table  表名
 #删除外键
 alter table 表名  drop FOREIGN KEY 外键名称  ;
 #显示所有的索引，并且删除需要删除的索引
 show index from  表名称 ;
 alter table  表名称 drop  index  FK_cv98jak9 ;
```

## 7.字段删除和添加
```
 alter table   表名  drop column    cluster_num
 alter table   表名  add cluster_num int comment '数量';
```
## 8.NULL问题
    SELECT 1 = NULL, 1 <> NULL, 1 < NULL, 1 > NULL;
## 9. 显示执行的进程
show  full  processlist （状态字段的意思） 
或者（select * from  information_schema.processlist where  host like '%10.18.2.65%'）


* Sleep
通常代表资源未释放，如果是通过连接池，sleep状态应该恒定在一定数量范围内
（一般连接池会有一个参数initialSize，如果initialSize=10则初始化的时候会有10个状态为sleep连接被建立，并且一直存在），
例如：
数据查询时间为0.1秒，而网络输出需要1秒左右，原本数据连接在0.1秒即可释放，但是因为前端程序未执行close操作，
直接输出结果，那么在结果未展现在用户桌面前，该数据库连接一直维持在sleep状态

* Locked
操作被锁定，通常使用innodb可以很好的减少locked状态的产生

* Copy to tmp table
索引及现有结构无法涵盖查询条件时，会建立一个临时表来满足查询要求，产生巨大的i/o压力Copy to tmp table通常与连表查询有关，
建议减少关联查询或者深入优化查询语句，如果出现此状态的语句执行时间过长，会严重影响其他操作，此时可以kill掉该操作
* Sending data
Sending data并不是发送数据，是从物理磁盘获取数据的进程，如果你的影响结果集较多，那么就需要从不同的磁盘碎片去抽取数据，
如果sending data连接过多，通常是某查询的影响结果集过大，也就是查询的索引项不够优化

* Storing result to query cache
如果频繁出现此状态，使用set profiling、“SHOW PROFILE” 分析，如果存在资源开销在SQL整体开销的比例过大（即便是非常小的开销，看比例），
则说明query cache碎片较多，使用flush query cache可即时清理，Query cache参数可适当酌情设置

-----------
1 开启profiling功能：set profiling=1;
2 显示所有记录的profile：show profiles;
3 详细输出某个profile的记录：show profile cpu ,block io for query n(profile的id)
------------

## 10.mysql日期时间函数的处理

``` sql
date_format(date, format) 函数，MySQL日期格式化函数date_format()
unix_timestamp( 时间日期) 函数 ，Mysql日期转换为unix时间戳
str_to_date(str, format) 函数 ，字符串转化日期
from_unixtime(unix_timestamp, format) 函数，MySQL时间戳格式化函数from_unixtime

实例
select DATE_FORMAT(now(),'%Y-%m-%d' ) 
select str_to_date('2017-12-08 00:00:00', '%Y-%m-%d %H:%i:%s' ) 
select unix_timestamp( DATE_FORMAT(now(),'%Y-%m-%d' )  )*1000  ;
select unix_timestamp( str_to_date('2017-12-08 00:00:00', '%Y-%m-%d %H:%i:%s' )  )*1000 ;
select from_unixtime( unix_timestamp( DATE_FORMAT(now(),'%Y-%m-%d' )  ),'%Y-%m-%d %H:%i:%s'  )  ;
数据库中的时间错：FROM_UNIXTIME(left(a.created_at,10), '%Y-%m-%d %H:%i:%S')>'2018-01-01 00:00:00' 
select step_name,FROM_UNIXTIME(left(a.start_time,10), '%Y-%m-%d %H:%i:%S') 开始时间,
FROM_UNIXTIME(left(a.end_time,10), '%Y-%m-%d %H:%i:%S')  结束时间
from task_step1 a
```

## 11.多表关联更新与删除
* 关联更新一个表的数据
```
update user a    join  user_ext b on a.uid=b.id  
set  b.name=a.name, b.age=a.age
where ifNULL(a.business_ip,'')<>ifNULL(b.new_business_ip,'')   
```
 
* 关联删除h表的数据
```
delete h.* 
from user  g   join user_ext h on g.id=h.userId
where g.id=1
```
## 12.in和exist的区别
[参照](http://www.manongjc.com/article/981.html)

## 13.数据导出和导入
> - 备份：mysqldump  数据库名称 -u'用户名' -p > 表名称.sql
mysqldump -u 用户名 -p '密码' --host=远程服务器地址  数据库名称 > 数据库名称1.sql
> - /usr/local/mysql/bin/mysqldump -q -h127.0.0.1 -udmin -p'密码' -P3306  --single-transaction  --databases 数据库名称 > /data/tmp/备份数据库名_20111019.sql

> - 导入：mysql> source /tmp/表名称.sql
> - 登陆：mysql -h mysql远程地址 -u 用户名 -p'密码'

## 14.数据库的common内容显示乱码：修改编码格式

```
show variables like 'char%'
set character_set_connection=utf8 ;
set character_set_results=utf8;
```


