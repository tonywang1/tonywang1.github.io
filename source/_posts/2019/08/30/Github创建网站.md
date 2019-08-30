---
title: Github上创建自己的网站
date: 2019-08-30 17:51:03
urlname: gen_github_page
tags: [Github]
categories: Github
---

# Github搭建自己的博客
 
---

## *为什么要搭建自己的博客*

> * 探索未知世界
> * 记录自己的成长
> * 为后来者提供借鉴
> * 梳理自己的逻辑思维
> * 整理自己的学习知识，方便以后查阅


 
## 1. github page是什么？
    
>　　GitHub页面是一种静态网站托管服务，旨在直接从GitHub存储库托管个人、组织或项目页面。

[Github Page官方详细](https://help.github.com/en/categories/github-pages-basics)

## 2. hexo是什么？

- 简单、快速、强大的Node.js静态博客框架
- [hexo源码](https://github.com/hexojs/hexo)
- [hexo官方网站](https://hexo.io/zh-cn/)

## 3. github page的设置

- [设置GitHubPage](https://help.github.com/en/articles/configuring-a-publishing-source-for-github-pages)

### 个人操作借鉴 （详细请参考上文超链接）
> - 创建一个repository，名称为（**用户名** + .github.io ）括号中的以后是你博客的默认域名
> - 切换tab到setting，并设置Repository name


## 4. nodejs、hexo安装，git安装
nodejs的安装，直接百度就可以搜索到，直接下载安装非常简单
git安装
[hexo安装参考](https://hexo.io/zh-cn/docs/#%E5%AE%89%E8%A3%85%E5%89%8D%E6%8F%90)
## 5. 创建、清理、发布、启动服务、本地访问

[文档地址](https://hexo.io/zh-cn/docs/commands)
 1. 创建静态站点 ` hexo init`
 2. 创建一个文章 `hexo new [layout] <title>`
 3. 生成静态文件 `hexo generate`
 4. 启动服务 `hexo server`
 5. 部署网站 `hexo deploy`
 6. 清除缓存和生成的静态文件 `hexo clean`
 7. 项目本地启动以后可以访问 `http://localhost:4000/`

## 6. 步骤5中创建的项目与Git关联并上传到git仓库

> - `git init`
> - `git add .`
> - `git commit '评论'`
> - 本地项目关联到git上：` git remote add origin <server> `

## 7. 更换主题
 
 1. 到 [主题源码](https://github.com/yscoder/hexo-theme-indigo) 页面fork源码到我们自己git上
 2. [主题安装](https://github.com/yscoder/hexo-theme-indigo/wiki/%E5%AE%89%E8%A3%85) 点击查看文档
 3. 修改配置文件（_config.yml）添加 theme: indigo指定主题
 
[ 我用的indigo主题源码](https://github.com/yscoder/hexo-theme-indigo)

## 8. hexo选择修改语言类型
 现在项目还是英文的，需要修改项目的语言为中文
 修改配置文件（_config.yml）中  `language: zh-CN`

## 6. hexo布局的使用

> * scaffolds文件夹中有3个文件分别为draft.md、page.md、post.md
分别对应草稿、page格式、post格式的文件模板
> * 命令 `hexo new [layout] <title>`  中的layout取值为：draft、page、post
> * 三个文件中的**Front-matter**中可以放一些变量在里面，以便生成静态页面的时候可以使用

```
这里是我自己搭建项目的模板 其中urlname这个变量设置以后生成的静态文件路径上会使用
---
title: {{ title }}
urlname: 
date: {{ date }}
tags:
categories:
---
```


## 7. hexo生成静态页面的目录的设置
配置文件_config.yml中设置下面的**permalink**参数，生成静态页面的访问路径，会根据**Front-matter**中的变量设置参数
`permalink: :year/:month/:day/:urlname/`

## 8. hexo标签和分类的使用，以及标签和分类区别

### 分类生成及使用

 > - 打开命令行，进入博客所在文件夹。执行命令
 `hexo new page categories`
 > - 会生成一个文件 ，/categories/index.md
 > - 修改分类中的设置如下，有的版本使用type有的版本使用layout变量
    ```
    ---
    title: 文章分类
    date: 2019-08-29 15:48:13
    comments: false
    type: categories
    layout: categories
    ---
    ```
    
  > - 写文章的收需要加上 categories: “文章标签名称”
  > - 原理：hexo 会找到`layout: categories`的设置，然后搜索所有文章中有categories设置的分类，生成分类列表
  
### 标签生成及使用
操作： 
 > - 打开命令行，进入博客所在文件夹。执行命令`hexo new page tags 
 > - 下面的设置和categories一样了 
 
 
