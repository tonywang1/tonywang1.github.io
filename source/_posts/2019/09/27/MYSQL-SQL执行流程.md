---
title: mysql-sql执行流程
date: 2019-09-17 20:28:36
urlname: linux_cron
tags: [MYSQL,SQL]
categories: MYSQL
---
 
# MYSQL-SQL执行流程
 
---
 
## 1.SQL的执行流程
 词法分析->语法分析-解析树-预处理-检查权限-新解析树-查询优化器-执行计划-存储引擎获取数据
 
## 1.SQL 的关键字执行的流程如何
 1）执行的SQL语句-每一步执行的输出，是下一步的输入
 
```SQL
 SELECT DISTINCT
    < select_list >
FROM
    < left_table > < join_type >
JOIN < right_table > ON < join_condition >
WHERE
    < where_condition >
GROUP BY
    < group_by_list >
HAVING
    < having_condition >
ORDER BY
    < order_by_condition >
LIMIT < limit_number >
```
2)实际执行流程

```sql
    FROM <left_table>
    ON <join_condition>
    <join_type> JOIN <right_table>
    WHERE <where_condition>
    GROUP BY <group_by_list>
    HAVING <having_condition>
    SELECT 
    DISTINCT <select_list>
    ORDER BY <order_by_condition>
    LIMIT <limit_number>
```
## 3 执行顺序实例

1）准备工作
```
CREATE DATABASE /*!32312 IF NOT EXISTS*/`test1db` /*!40100 DEFAULT CHARACTER SET utf8 */;
USE `test1db`;
CREATE TABLE `table1` (
  `uid` varchar(10) NOT NULL,
  `name` varchar(10) NOT NULL,
  PRIMARY KEY (`uid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert  into `table1`(`uid`,`name`) values ('aaa','mike'),('bbb','jack'),('ccc','mike'),('ddd','mike');

CREATE TABLE `table2` (
  `oid` int(11) NOT NULL AUTO_INCREMENT,
  `uid` varchar(10) DEFAULT NULL,
  `old` int(11) DEFAULT NULL COMMENT '年龄',
  PRIMARY KEY (`oid`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;

insert  into `table2`(`oid`,`uid`,`old`) values (1,'aaa',2),(2,'aaa',1),(3,'bbb',3),(4,'bbb',4),(5,'bbb',5),(6,'ccc',6),(7,NULL,7);

```

2）最终的执行结果
```
SELECT
    a.uid,
    count(b.oid) AS total 
FROM
    table1 AS a
LEFT JOIN table2 AS b ON a.uid = b.uid
WHERE
    a. NAME = 'mike'
GROUP BY
    a.uid
HAVING
    count(b.oid) < 2
ORDER BY
    total DESC
LIMIT 1;

```
3）执行FROM关键字-笛卡尔积
```
select * 
from table1  a 
left  join table2 b on a.uid=b.uid 
```
4) ON关键字过滤-v-4
```
select * from table1  a join table2 b on a.uid=b.uid ; 
```
5)如果使用了外连接(LEFT,RIGHT,FULL)，主表（保留表）中的不符合ON条件的列也会被加入到这一步的执行结果中-生成新的虚拟表v-5

6）WHERE条件过滤 -v-6
对于上面的执行结果，满足WHERE条件的结果集的数据，会生成新的虚拟表 v-6
```
select * 
from table1  a 
left  join table2 b on a.uid=b.uid 
where a.name='mike'
```
**注意**：ON和where条件过滤的区别
> 1 在inner join的时候 ，两者查询的结果一样
> 2 在外连接(LEFT,RIGHT,FULL)查询的时候，on对关联表进行条件过滤，然后在与主表进行关联，这个过程中这个查询条件对主表不会有影响
> 3 在外连接(LEFT,RIGHT,FULL)查询的时候，where 条件过滤，是对连接后的整个临时表进行过滤，不分主从

```sql
查询示例：
select * from table1  a left  join table2 b on a.uid=b.uid and b.oid='1' ; 
select * from table1  a left  join table2 b on a.uid=b.uid where  b.oid='1' ; 
```
**总结**
> 主表进行过滤必须放到WHERE条件后，从表过滤如果先过滤后链接则条件放到on后面，如果先链接在过滤则放到WHERE条件后

7）GROUP BY 关键字进行过滤
 这个会对表 v-6中的表的某些字段进行分组，**他对于后面的SELECT,和HAVING所用到的列必须包含在GROUP BY 中，对于没有出现的必须进行聚合运算**
 
 对于mysql上面的限制条件可以忽略，但是select字段中没有出现在group by中的字段，会随机选择一个值。
 
 
8)HAVING 关键字
作用：对分组后的数据进行过滤，满足条件的数据放到下一个虚拟表中-v-8
```sql
SELECT
    a.uid,
    count(b.oid) AS total 
FROM
    table1 AS a
LEFT JOIN table2 AS b ON a.uid = b.uid
WHERE
    a. NAME = 'mike'
GROUP BY
    a.uid
HAVING
    count(b.oid) < 1
```
9)SELECT 对select子句进行处理
1 - 计算select子句的表达式
2 - 如果有 DISTINCT，则进行去重

10）ORDER BY-根据ORDER BY 子句的条件对结果进行排序
**唯一可使用SELECT中别名的地方**
```
SELECT
    a.uid,
    count(b.oid) AS total 
FROM
    table1 AS a
LEFT JOIN table2 AS b ON a.uid = b.uid
WHERE
    a. NAME = 'mike'
GROUP BY
    a.uid
HAVING
    count(b.oid) < 1
ORDER BY
    total DESC
```
11) LIMIT-子句从上一步得到的虚拟表中选出从指定位置开始的指定行数据


## 2 SQL中关联表on后面的条件与where后面条件有什么不同
详细见上面的6
  
##3.同一个字段不同值的统计处理
查询一个用户有多少条年龄数据：
```
select  name,
 sum(case when (old is not null)  then  1 else 0 end  ) '个数'
from table1  a 
left  join table2 b on a.uid=b.uid 
group by a.name ;

```


 