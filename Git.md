# 概述

Git是一个免费、开源的**分布式版本控制系统**。

## 什么是版本控制

版本控制是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统。 

版本控制其实最重要的是可以记录文件修改历史记录，从而让用户能够查看历史版本， 方便版本切换。

## Git工作机制

<div align='center'>
    <img src='./imgs/git/git001.png' width='300px'>
	<br/><br/>git工作机制
</div>

## Git和代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般简称为远程库。

- 私有/个人
	- GitLab
	- gogs
- 公共
	- GitHub
	- Gitee（码云）

# Git使用

## 常用命令

| 命令名称                                 | 作用           |
| ---------------------------------------- | -------------- |
| git config --global user.name username   | 设置用户签名   |
| git config --global user.email useremail | 设置用户签名   |
| git init                                 | 初始化本地库   |
| git status                               | 查看本地库状态 |
| git add filename                         | 添加到暂存区   |
| git commit -m 'message' [filename]       | 提交到本地库   |
| git reflog                               | 查看历史记录   |
| git reset --hard versionNum              | 切换版本       |

## 分支操作

<div align='center'>
    <img src='./imgs/git/git002.png' width='800px'>
</div>

### 为什么使用分支

同时推进多个功能开发，提高开发效率。

如果某个分支开发失败，不会影响其他分支。

### 常用分支操作

| 命令名称            | 作用                         |
| ------------------- | ---------------------------- |
| git branch 分支名   | 创建分支                     |
| git branch -v       | 查看分支                     |
| git checkout 分支名 | 切换分支                     |
| git merge 分支名    | 把指定的分支合并到当前分支上 |

### 团队协作

<div align='center'>
    <img src='./imgs/git/git003.png' width='800px'>
</div>

### 跨团队协作

<div align='center'>
    <img src='./imgs/git/git004.png' width='800px'>
</div>