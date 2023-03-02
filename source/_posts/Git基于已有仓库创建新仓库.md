---
title: 基于已有的仓库创建新的仓库 
date: 2022-05-16 12:10:11
categories: "实践"
tags:
     - 实践
---


背景：新小程序（`new-mp`）业务中，期望使用已有的小程序（`old-mp`）的技术结构进行实现


### （第一种做法）新建空白仓库

> 创建一个临时文件夹

> git clone old-mp

> rm -rf .git 

> git上创建一个空白仓库

> git init 

> git set remote origin ...

> git push -set-upstream ...

以上为伪代码，表示一个基于已有的旧项目，提交一个新项目

<!-- more -->


### （第二种做法）fork

> gitlab fork 已有的项目 frontend/fc/old-mp -> frontend/old-mp

> frontend/old-mp 修改项目设置，修改成新的名字 frontend/new-mp

> 使用 Transfer 功能，进行项目迁移(frontend/new-mp -> frontend/jv/new-mp) 项目设置 Advanced/Transfer project 

其实到这里已经完毕了，新项目仓库已经用用整改项目结构了。

但是由于fork的原因，其commit，branch，tag等都在。因为是新仓库，期望这些都是init的，所以我们需要把这些都清理掉。

### git checkout --orphan lastest_branch

创建孤立分支。

我们基于master创建一个孤立分支，孤立分支的意思是，这个分支所有的commit都会清空，只有最后一个`HEAD`

然后我们通过一些手段把这个孤立分支回溯到master即可。

1. push lastest_branch
2. remove all tags
3. remove all branch except lastest_branch （可能需要涉及到仓库的管理权限）
4. done


## 结束

仅仅是一个`茴`字怎么写的过程。😄 