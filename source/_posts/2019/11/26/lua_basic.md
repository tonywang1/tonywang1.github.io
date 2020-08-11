---
title: Lua语言基础
date: 2019-11-26 15:41:13
urlname: lua_basic
tags: [Lua]
categories: Lua
---
#LUA语言简介
 
------
## 1.什么是Lua语言
  Lua是一种强大，高效，轻量级，可嵌入的脚本语言

## 2.特性
它用标准C语言编写并以源代码形式开放，编译后仅仅一百余K，可以很方便的嵌入别的程序里

## 3.LUA能解决什么问题

Nginx + lua　处理http请求时，可在11个阶段插入lua脚本
redis + lua 　实现原子操作，避免多线程数据不一致的问题
 
## 3.LUA环境搭建
[搭建参照](https://www.runoob.com/lua/lua-environment.html)

## 4.基础语法记录-主要记录学习中独特的地方
1) [基础语法学习参照](https://www.runoob.com/lua/lua-basic-syntax.html)
2)逻辑运算符短路求值
```
and	逻辑与操作符。 “短路求值” 若 A 为 false，则返回 A，否则返回 B。	(A and B) 为 false。
or	逻辑或操作符。“短路求值”  若 A 为 true，则返回 A，否则返回 B。	(A or B) 为 true。
```
3)bool类型
lua中除了false和nil之外，其他所有数据，包括0、空字符串等都是true。
4) 类型判断
```
print(type("Hello world"))      --> string
print(type(10.4*3))             --> number
print(type(print))              --> function
print(type(type))               --> function
print(type(true))               --> boolean
print(type(nil))                --> nil
print(type(type(X)))            --> string

```
5)nil删除功能
对于全局变量和 table，nil 还有一个"删除"作用，给全局变量或者 table 表里的变量赋一个 nil 值，等同于把它们删掉
6)nil 作比较时应该加上双引号
``` type(X)=="nil" --- true  ```
7)字符串和数字运算
一个数字字符串上进行算术操作时，Lua 会尝试将这个数字字符串转成一个数字
8)字符串连接
```  
print("a" .. 'b')  tonumber(字符串)  -- tostring(bVar)
```
9)字符串长度
使用 # 来计算字符串的长度，放在字符串前面

10)table类型-数组、哈希、包、类- 都可以通过table实现
 
> - 1、不同于其他语言的数组把 0 作为数组的初始索引，在 Lua 里表的默认初始索引一般以 1 开始
> - 2、"关联数组"（associative arrays），数组的索引可以是数字或者是字符串。
> - 3、table 不会固定长度大小，有新数据添加时 table 长度会自动增长，没初始的 table 都是 nil
> - 4、对 table 的索引使用方括号 []。Lua 也提供了 . 操作
> - Lua table 是不固定大小的，你可以根据自己需要进行扩容
> Lua也是通过table来解决模块（module）、包（package）和对象（Object）的。 例如string.format表示使用"format"来索引table string

11)table类型是否为nil变量
```
function isTableEmpty(t)
    if t == nil or _G.next(t) == nil then
        return true
    else
        return false
    end
end
```
12)全局变量和局部变量
a = 5  -- 全局变量 
local b = 5         -- 局部变量

13)Lua 可以对多个变量同时赋值
Lua 可以对多个变量同时赋值，变量列表和值列表的各个元素用逗号分开，赋值语句右边的值会依次赋给左边的变量

14)循环
``` lua
while( true )
do
   print("循环将永远执行下去")
end

if(0)
then
    print("0 为 true")
end

for k, v in pairs(tab1) do
    print(k .. " - " .. v)
end
```

15)函数
> - Lua函数可以返回多个结果值，比如string.find，其返回匹配串"开始和结束的下标"（如果不存在匹配串返回nil）。
> - Lua 函数可以接受可变数目的参数，和 C 语言类似，在函数参数列表中使用三点 ... 表示函数有可变的参数
> - 我们也可以通过 select("#",...) 来获取可变参数的数量
> - 有时候我们可能需要几个固定参数加上可变参数，固定参数必须放在变长参数之前
> - 通常在遍历变长参数的时候只需要使用 {…}，然而变长参数可能会包含一些 nil，那么就可以用 select 函数来访问变长参数了：select('#', …) 或者 select(n, …)
select('#', …) 返回可变参数的长度
select(n, …) 用于访问 n 到 select('#',…) 的参数


 
 
 
## 5.Lua在nginx中的使用
一、[阶段执行流程](https://github.com/openresty/lua-nginx-module#directives)
二、[Nginx API for Lua](https://github.com/openresty/lua-nginx-module#nginx-api-for-lua)

三、常用阶段
> - content_by_lua 是内容处理器，接受请求并输出响应，适用于location、location if。 
> - access_by_lua 在请求访问阶段处理，用于访问控制，适用于http、server、location、location if。
> - rewrite_by_lua 测试只有在重定向的时候才会调用 
> - set_by_lua
> - set_by_lua_file执行Nginx外部的lua脚本，可以避免在配置文件中使用大量的转义

四、使用实例1
```  
location / {
	rewrite_by_lua '
	local res = ngx.location.capture("/check-spam")
	if res.body == "spam" then
		ngx.redirect("/terms-of-use.html")
	end
	'; fastcgi_pass ...;
}

location /echo {
	default_type text/plain;
	echo hello lua;
}

location /lua {
	default_type text/plain;
	content_by_lua 'ngx.say("hello world")';
}

```
五、使用实例2-ip禁止功能
```
location @client{
	proxy_pass  http://www.baidu.com;
}
location ~  /test {
	default_type  text/html;
	content_by_lua 'ngx.say("this is  ruifengyun.com!")';
	access_by_lua '
		if ngx.var.remote_addr == "101.2.20.110" then
			ngx.exit(ngx.HTTP_FORBIDDEN)
		end
		if ngx.var.remote_addr == "101.21.20.112" then
			ngx.exec("@client")
		end
	';
}
```

六、重定向转发-获取重定向的参数
```

location /foo {
 content_by_lua_block {
     ngx.exec("/bar", "a=goodbye");
 }
}


location /bar {
 default_type text/plain;
 content_by_lua_block {
     local args = ngx.req.get_uri_args()
     for key, val in pairs(args) do
	 if key == "a" then
	     ngx.say(val)
	 end
     end
 }
}
```
七、nginx定义的变量 lua中可以获取
```
 location /foo {
     set $my_var ''; # this line is required to create $my_var at config time ngx.arg[1]可以获取定义的参数
     content_by_lua_block {
         ngx.var.my_var = 123;
         ...
     }
 }
 
 删除变量方法：ngx.var.args = nil如：ngx.var.name = nil 删除nginx中name的变量 
```

八、ngx.ctx-一个生命周期中共享变量
```
 location /test {
     rewrite_by_lua_block {
         ngx.ctx.foo = 76
     }
     access_by_lua_block {
         ngx.ctx.foo = ngx.ctx.foo + 3
     }
     content_by_lua_block {
         ngx.say(ngx.ctx.foo)
     }
 }

```

九 几个常用的api地址
> - [方法常量](https://github.com/openresty/lua-nginx-module* #http-method-constants)
> - [状态常量](https://github.com/openresty/lua-nginx-module#http-status-constants)
> - [日志级别常量](https://github.com/openresty/lua-nginx-module#nginx-log-level-constants)
> - [nginx头处理](https://github.com/openresty/lua-nginx-module#ngxheaderheader)
> - [重定向](https://github.com/openresty/lua-nginx-module#ngxreqset_uri)

十、常用API记载
```
 ngx.req.set_uri
 ngx.req.set_uri("/foo", true)
ngx.req.get_uri_args
ngx.req.set_uri_args
local args, err = ngx.req.get_post_args()
ngx.req.get_headers
ngx.req.set_header
ngx.req.clear_header
ngx.req.read_body
ngx.req.get_body_file

ngx.req.set_body_data

ngx.redirect
ngx.print 没有回车 
ngx.say 有回车 

ngx.flush
ngx.exit(501) 返回状态吗，并直接退出 
ngx.sleep
ngx.today
ngx.time
ngx.now
ngx.update_time
ngx.utctime
ngx.re.match
ngx.re.find
ngx.re.gmatch
ngx.re.sub
ngx.re.gsub
ngx.shared.DICT
local ok, err = ngx.timer.at(2, mylog ) 延时器，只会执行一次 

--写响应头  
ngx.header.a = "1"  
--多个响应头可以使用table  
ngx.header.b = {"2", "3"}  
--输出响应  
ngx.say("a", "b", "<br/>")  
ngx.print("c", "d", "<br/>")  
--200状态码退出  
return ngx.exit(200)  

必须调用exit给出状态码
ngx.say("api is offline!")
ngx.exit(200)

获取消息体，先打开获取消息体的开关：
ngx.req.read_body()
local data = ngx.req.get_body_data()

local request_uri = ngx.var.request_uri or ""
resty.iputils 工具类使用：https://github.com/hamishforbes/lua-resty-iputils

```


## 6.主要参考
* 1) [Lua官网](http://www.lua.org/about.html)
* 2) [luajit官网](https://luajit.org/)
* 3）[openresty](https://openresty.org/cn/)
* 4）[lua-nginx-module](https://github.com/openresty/lua-nginx-module)
