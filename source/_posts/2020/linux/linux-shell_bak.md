---
title: linux中脚本介绍
date: 2020-04-11 15:41:13
urlname: linux_shell
tags: [LINUX]
categories: LINUX
---

# 日志清理脚本
```shell
#!/bin/bash
#统计一定数量的日志，然后根据时间删除
logpath="/data/tomcat/logs"
count=`find  /data/tomcat/logs  -name "*.log" -o -name "*.txt" -type f -mtime +3 | wc -l`
echo  "$count";
if [ "$count" -lt "20" ];then
    echo "file is less 20. no file is removed."
    exit 0;
fi

#find $logpath  -name "*.log" -o -name "*.txt" -type f -mtime +5 -exec rm {} \; > /dev/null 2>&1 ;
find  /data/tomcat/logs  -name "*.log"  -mtime +3 -exec rm {} \; 
find  /data/tomcat/logs  -name "*.txt"  -mtime +3 -exec rm {} \;

count2=` find  /data/tomcat/logs  -name "*.log" -o -name "*.txt" -type f -mtime +10 | wc -l`
echo "$count2"

```
 
 
## 2、JAVA项目启动脚本
```
#!/bin/bash
echo "杀掉springboot线程"
pgrep -f 项目名称|xargs kill -9
echo "项目启动"
nohup  java -server -Xms1096m -Xmx1096m -jar  项目的.jar > out.log 2>&1 &
tail -f -n500 out.log 


```
  
  


