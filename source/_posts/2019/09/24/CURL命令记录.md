---
title: CURL使用
date: 2019-09-17 20:28:36
urlname: linux_curl
tags: [LINUX,CURL]
categories: LINUX
---
 
# CURL使用
 
---


## 1.curl作用
  　curl是一个利用URL规则在命令行下工作的文件传输工具

## 2语法
 curl [option] [url]
 
 ## 3实例
 
 
 - 发送get、post请求-代码3.1
- 发送带有参数的get和post请求 - 3.2 
- 查看请求的整个请求链路-调试谁用 - 3.3
- 发送带有header请求 - 4.4
- 发送设置host的请求 -4.4


```linux
代码3.1
curl -i(返回头信息) -X POST/Get http//www.baidu.com
```

```linux
代码3.2
- 发送applicationjson数据
- curl -H "Content-Type:application/json" -X POST -d '{"user": "admin", "passwd":"12345678"}' https://proxy.mimvp1.com/login
- 普通数据提交
- curl -d "param1=value1&param2=value2" -H "Content-Type: application/x-www-form-urlencoded" -X POST http://localhost:3001/data

```

```linux
代码3.3
curl  -X POST/Get http//www.baidu.com -xvo /usr/null  | python -m json.tool
```

```linux
代码4.4
curl  'http://localhost/hadoop1/clusters/list?page=1&pageSize=50&total=0&tokenId=tokenId_a218_4ab8_8404_3ac9a4b63d2c' -H 'host:bd1prod.localhost.com

```