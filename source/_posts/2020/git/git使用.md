---
title: GIT使用
date: 2020-02-28 15:41:13
urlname: git-know-use
tags: [GIT]
categories: GIT
---
 
# GIT使用
---


## 1.什么是GIT
 免费开源的分布式版本控制系统，小或者大的工程使适应
## 2.GIT作用和优点
 GIT有工作区间、本地仓库、远程仓库，在没有网络的情况下依然可以用本地仓库进行版本控制。
## 3.GIT中涉及到的关键字与关键词
- HEADER 当前活跃的分支
- origin 默认远程仓库的名称

 
## 4.git工作流程： git有三个工作树，即工作目录 
 > 1）本地工作目录，它持有实际文件 
 > 2）暂存区域，像一个缓存区域，保存你的改动
 > 3）HEAD区域，指向你最后一次提交的结果。
   每次提交的时候，add文件，则文件提交到暂存区域即第二个区域，然后commit文件，则文件会被提交到HEAD区域，
这个时候其他人还看不到文件的改动，最后需要push，将文件推送到远端的服务器上 



## 5.GIT中常用的命令以及使用场景
- 创建新仓库：创建新文件夹，打开，然后执行    git init
- 检出仓库： 
   本地：git clone /path/to/repository 
   远端：git clone username@host:/path/to/repository

- 提交文件
$ git status -s
git add <filename> 
git commit -m "代码提交信息"
git  pull     #（将服务器项目与本地项目合并）
git push origin master  可以把 master 换成你想要推送的任何分支

- 如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：
 git remote add origin <server> 
（git remote add origin master或 git remote add origin git@github.com:YotrolZ/helloTest.git ）
 删除本地和远程的关联关系： git remote remove origin <server> 

- 要更新你的本地仓库至最新改动，执行：
git pull
- 在合并改动之前，你可以使用如下命令预览差异
git diff <source_branch> <target_branch>
- 获取（fetch） 并 合并（merge） 远端的改动。 要合并其他分支到你的当前分支（例如 master）
git merge <branch>
- 并可能出现冲突（conflicts）。 这时候就需要你修改这些文件来手动合并这些冲突（conflicts）。改完之后，你需要执行如下命令以将它们标记为合并成功：
git add <filename>

-  假如你操作失误（当然，这最好永远不要发生），你可以使用如下命令替换掉本地改动：
此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。
git checkout -- <filename>

- 假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
git fetch origin（当某个分支看不到的时候，可以这样进行索引更新） - 更新远程分支最新信息到本地，会将远程新创建的分支信息写到 FETCH_HEAD(所有的分支信息)文件中
git reset --hard origin/master1111

-  查看远程有什么数据仓库
 git remote -v

- git版本回退
查看所有版本号
git reflog

根据版本号恢复到某个版本
git reset --hard 6fcfc89


- $ git push origin :master
  等同于
$ git push origin --delete master

- 直接提交：
git push origin source -f

 
## 6.git的分支管理
> git分支操作在本地建立分支，然后与本地主枝合并，最终提交到服务器。有效的避免了因个人操作不当向服务器提交过多脏数据，避免频繁git clone服务器来更新本地库。

分支操作指令：

 - 建立分支
git branch AAA     #建立分支AAA
 - 分支切换
git checkout AAA    #从当前分支切换到AAA分支
 -  将分支与主枝master合并
git checkout master     #（首先切换回主枝）
git merge AAA         #（将分支AAA与主枝合并）
 -  当前分支查看
git  branch            #默认有master（也称为主枝）
git  branch –a 查看当前所有分支
 -  删除分支
git branch –d  AAA     #删除分支AAA
 - 删除所有 有远程分支的本地分支
git remote prune origin  


## 7..gitignore使用，根目录创建文件.gitignore
 忽略根目录文件：/.project
 忽略根目录下的目录：/target/
 注意：忽略的目录和文件必须是git没有追踪的（untraced）,如果文件或者目录是已经设置了跟踪，则需要先删除版本库跟踪，然后提交（git commit -m "评"），这时候文件夹或者文件的忽略跟踪才会生效

	git 删除被管理的文件 git rm —cached filePath
	git 删除被管理的文件夹 git rm -r -f —cached filePath
	git 不再追踪文件改动 git update-index --assume-unchanged filePath
	git 恢复追踪文件改动 git update-index —no-assume-unchanged filePath
  原则：文件未追踪的情况下，添加到.gitignore中，达到忽略文件提交的目的
  如果文件已经提交或者已经添加追踪，则删除提交或者删除追踪即可

## 7.elipse中使用git
> 
>   1 Add to Index ，添加文件到git追踪下
>   2 Remove from Index ，文件去掉git追踪
> 
>   3 team->commit 提交文件到本地仓库
>   4 team->Repository->Push to upstream或者push Branch Master 提交到远程共享仓库
>   5 replace -> head Revision 本地仓库最新文件
>   6 replace -> Prevision Revision   新版本
>   7 文件冲突 
    pull 将新文件下载下来，手动解决冲突，
    add 然后push新文件到线上
  
## 7.SOURCETREE修改用户名和密码
删除C:\Users\%USERNAME%\AppData\Local\Atlassian\SourceTree 目录下的passwd文件， 能移除掉保存的密码。
删除C:\Users\%USERNAME%\AppData\Local\Atlassian\SourceTree 目录下的userhosts文件， 能移除掉保存的用户名


 ## 8.可能会遇到的问题
 如果账号密码有变动，需要修改的话：
1 删除：AppData\Local\Atlassian\SourceTree\passwd 
2  git config --system --unset credential.helper
   不行的话git config --global http.emptyAuth true
 
git异常ssl权限问题：git config --global http.sslVerify false 

 
 
## 7.主要参考
- [安装包下载](https://git-scm.com/downloads)

    
 