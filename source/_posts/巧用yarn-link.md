---
title: 巧用yarn link
date: 2021-04-16 11:12:29
categories: "工具"
tags:
     - 工具
---


## 一般用来调试npm package
 
1. `yarn config dir` 获取全局以来安装位置 `$yarn_global_patch`
2. `cd $yarn_global_patch/node_modules/modulex` 进入`modulex`内
3. 执行 `yarn link`
4. 执行成功了将会显示 `success Registered "modulex".`
5. `cd $project_path` 进入项目根目录
6. `yarn link modulex` 此时，项目下`node_modules` 将会生成存在 `modulex`
7. 此时 `yarn release` 可以正常运行