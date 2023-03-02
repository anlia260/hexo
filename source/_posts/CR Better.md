---
title: 如何更好的Code Review？
date: 2021-02-08 23:00:09
categories: "CR"
tags:
     - 质量
     - CR
---


本文会从 code review 对流程，意义，工具，执行规范四个范畴来进行阐述

## 意义

1. 发现错误：人都会不可避免的出现一些纰漏，而这些纰漏在另一个人眼中也许显而易见。
2. 健壮性检查：代码是否健壮，是否有潜在安全、性能风险。代码是否可以回滚。
3. 质量保证：保证进入稳定版本的代码是经过层层检查的
4. 统一风格：对于整个团队来说，代码风格的统一很重要。风格统一除了人 Review，我们也引入了静态代码检查，不符合团队风格的代码，是无法通过 CI 的。完善注释：包括 commit message。
5. 交流与学习


## 范围

### code review

    - 函数实现复杂性
    - 可阅读性
    - 无用的code（可减少的变量声明）
    - 组件设计优化


### logic review
    
    针对某个逻辑进行debug式的review，这种情况是否属于 code review 无法界定，
    理论上来讲，在做复杂模块设计的时候，会有设计文档生成，设计文档也会经过一遍审查，此时的审查就是复杂逻辑的设计。

<!-- more -->

## 执行要点（流程） 

### 何时开始code review？

    理论上在把代码送给测试之前，进行code review，保证送测代码经过开发人员审查通过。

### 没有修复的issue是否可以送测

    在code review的时候，评审人员会根据影响范围程度决定是否必要在送测之前修复掉。如果影响甚微，可以在下个迭代的需求中修复掉。

### 如何优化 code review 的会议时间

    1. 提前做好code review的`discussion`，这样最大化利用开发者碎片化时间
    2. 会议的时候 优先review这些问题，剩下的快速过。并且把相对应的`discussion`转化成`issues`，并标记重要程度   

## 工具

`Gitlab MR` 

{% plantuml %}
    start
    :Before Test;
    :Create branch T-xx;
    repeat :Submit MR for project;
    :To discussion about MR and take advantage of fragmentation time with everyone;
    :Code Review Meeting;
    backward :fixed code;
    repeat while (create issues with discussion?) is (important) not (Yes);
    :Close code review!;

    if (CI check) then (sonarqube)
      :sonarqube scanner;
    else if (pre build?) then (yes)
          :merged;
    else (no)
          :refixed;
    endif

    stop
{% endplantuml %}
