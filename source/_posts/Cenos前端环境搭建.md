---
title: Cenos前端环境搭建.md
date: 2017-10-21 21:30:43
categories: "系统"
tags:
     - linux
     - 前端环境
---


## 查询机器版本，位数

```shell
uname -a                      **查询所有信息
cat /etc/redhat-releae      **查询版本
```

## 更新kernel

1. 安装 ELRepo到centos系统中

  ```shell
  rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
  ```

2. 查询列出目前的内核版本

  ```shell
  yum --disablerepo="*" --enablerepo="elrepo-kernel" list available
  ```

3. 内核版本更新

  ```shell
  yum --enablerepo=elrepo-kernel install kernel-ml
  
  ```

  <!-- more -->

## 更换Repo

1. 首先备份/etc/yum.repos.d/CentOS-Base.repo

  ```shell
  mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
  ```

2. 下载对应版本repo文件, 放入/etc/yum.repos.d

  ```
  curl http://mirrors.163.com/.help/CentOS6-Base-163.repo -o CentOS6-Base-163.repo
  yum clean all   **清除缓存
  yum makecache    **生成缓存
  yum repolist    **查看生效repolist
  ```

## 安装NVM

​ `nvm(v0.33.0)` 是 `Nodejs` 版本管理工具。[nvm](https://github.com/creationix/nvm/blob/master/README.md) 是安装node最简单的方式。

​ 使用`Git`方式安装，有助于自定义`nvm`安装目录。一般安装在 `/usr/local` 全局目录。

​ 需要使用（v0.33.0）版本，最新（v0.33.2）实测有问题。

## 安装 Ci Runner

1. 安装 [gitlab-ci-multi-runner](https://docs.gitlab.com/runner/install/linux-repository.html)

2. 配置`Specific Runner`，[Config Function](https://gitlab.stnts.com/help/ci/runners/README.md)

> 其他

[快速搭建 Node.js 开发环境以及加速 npm](https://cnodejs.org/topic/5338c5db7cbade005b023c98)

​
