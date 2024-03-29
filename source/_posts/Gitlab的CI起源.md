---
title: 前端项目为什么需要CI？
date: 2017-02-08 23:00:09
categories: "CI"
tags:
     - 开发流程
     - 持续部署
---

### Gitlab-Ci 的过程（采取有力措施，防止人为失误）

进来热议的“工匠精神”，实质上是关注人力资源因素对产品及质量的影响。人是质量工作中最活跃，最重要的因素。也是管理难度最大的。在质量管理体系运行中，从流程到标准，都是需要人来做对。防止人为差错已成为质量保证的当务之急。那些依赖多人对过程，应特别关注是否有防错措施。

**人为错误有两种：**
*   有意识制造差错
*   无意识制造差错


**人为错误剖析：**
*   人都不想出错
*   失败又不是什么关荣的事情，有时候还会遭到周围人责备，所以谁也不回故意出错。

<!-- more -->

**BUT**

*   事实上如果一度认为【认为错误是没有办法的】，敷衍了事地对应，错误是不回减少的

**产生错误的背景：**

*   不知道（不明白 ● 没有听 ● 忘记了）

     > 不知道要确认，没有仔细看文档

*   不会做（知道，但是能力不足）

	>猜着去做

*   忽略危机（习惯 ● 本能走捷径）
    >  |-  一直以来都是ok的，所以认为不检查没有问题
       |-  随意地判断以为这样的就没有问题了
       |-  过于相信自己

*   错觉（误解，深信）
*   紧急时慌张，疲劳，注意力散漫（慌张更容易失误）


-------

###  我们需要有效的方法，工具，流程来防止人为失误

前端代码开发发布我们一直遵循着这个步骤：

![Alt text](https://raw.githubusercontent.com/anlia260/anlia260.github.io/master/img/a1.png)

我们走的是邮件流程，所有的人为操作都会由邮件记录，这是一个公司基本流程。

**But**

_上面的开发，测试，发布流程是否满足需要？_

__或者__

_上面的流程是否容易受到人力因素的巨大影响?_



| 环节               | 因素                       | 结果  |
| -------------     |:-------------             | -----:|
| 开发阶段           | 误解需求，某些需求忘记了       | 测试或者上线时才发现少了功能，或者功能不对 |
| 发布阶段           | 人为发错版本号              |   线上功能受到影响 |


开发阶段，某些需求没有具体原型，或者口头叙述没有相关文档，并且开发者未记录，那么很容易产生测试或者上线的出现，

#  Oh , shit !

还有这么个功能，看来每次需求会议需要记录了。或者是对需求的时候仔细确认。

开发的时候我们有可能处于这个流程

![Alt text](https://raw.githubusercontent.com/anlia260/anlia260.github.io/master/img/a2.png)

**这个时候我需要上B任务：**

*  我应该发布（2300--2310），跳过了（2299-2300）属于跳版本发布
   > 正常任务流中避讳跳版本，这样会使整个版本流显得混乱。
     并且可能遗漏发布某些文件，这也属于常规人为失误。

*  或者是我直接忽略影响，直接发布（2999-2310）
   > 这样就会把错误的A任务的文件给发布线上，带来的影响也是最大的。

*  或者是我直接发布最新版本，直接发布（2999-2312）
   > 这个时候可能A任务还未测试完成，发布线上会有风险。

上面的过程很容易产生人为失误。


__我们需要解决的事情__

*   基于git分支的commit的发布    [Git工作流](http://blog.jobbole.com/76867/)
  ![](http://legendtkl.com/img/uploads/2016/git-model.png)

*   自动构建部署

![Alt text](https://raw.githubusercontent.com/anlia260/anlia260.github.io/master/img/a3.png)


***

###   持续集成过程过程（Gitlab-Ci）

![](http://upload-images.jianshu.io/upload_images/525728-4339103186d2b1c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*  配置gitlab-runner

>Gitlab 8.0 给我们提供了ci的工具，使用简单的yaml配置就可以执行我们的构建命令，并且保存每次的构建结果。那么runner就是给我们跑命令的机器环境（可以是虚拟机，docker容器.....）
 [GitLab-CI与GitLab-Runner](http://www.jianshu.com/p/2b43151fb92e)

*  项目生产环境依赖锁定

>为什么要锁定当前目录生产依赖(node_modules),因为我要确定runner的环境要与开发一致，这样构建的结果才能与开发一致。这是我需要解决的事情，但是还有一个原因希望我的项目环境依赖是固定的，方便迁移开发，不受外部原因的影响。
  [从 left-pad 事件我们可以学到什么?](https://segmentfault.com/a/1190000004700432)

***配置gitlab-runner***

runner 一般需要运维去配置，如果懂一些简单的linux系统的话。自己也是可以配置一个项目的runner。
讲一下配置的项目runner中遇到的一些事情：

*   先要在Specific Runner(```CentOs.6.8```)机器上安装gitlab-ci-multi-runner
> curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.rpm.sh | sudo bash
    yum install gitlab-ci-multi-runner

*   在Gitlab上注册你的runner机器
> gitlab-ci-multi-runner register
	注册的gitlab-runner的机器默认是以gitlab-runner身份默认登录runner机器，所以要确保相对应的用户有node,npm相关软件执行权限

***锁定项目依赖资源表***

>现有的前端大而全，都会依赖一些构建工具。前端研发部采用的是fis流程，整套流程实验证明是很有价值的。
>但是会有一部分瑕疵：无人维护、插件质量，Bug数总量，等等。但我们现有的fis流程跑的很溜的时候，

___迁移到其他开发环境是否同样顺呢？___

>不会的 ，不同的环境受到网络，```node```，```npm```版本影响，都会出现各种问题，最大的问题是可能出现，```npm```中的软件包会经常改变依赖版本，这样的话就需要我们锁定当前健康的构建依赖。

>解决方案：
>```npm shrinkwrap```锁定当前的依赖关系
> ```npm shrinkpack```下载当前依赖的```node_module/*.tar```包
> ```npm-shrinkwrap.json```&```npm_shrinkwrap/```提交至gitlab
> ```git co  & npm i```这个时候你会发现下载数据非常快，因为直接从tar包中安装解压依赖。
>
>   [从 left-pad 事件我们可以学到什么?](https://segmentfault.com/a/1190000004700432)


--------

#	文档历史
>write by Alex Lin at 2017-01-12
