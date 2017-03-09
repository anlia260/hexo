---
title: JS基础之圆括号
date: 2017-03-09 23:14:34
categories: "JS"
tags:
     - JS基础
     - 前端进阶
---

#### 运算符

> 圆括号运算符( ) 用来控制表达式中的运算优先级.

```
var a = 1;
var b = 2;
var c = 3;

// default precedence
a + b * c     // 7
// evaluated by default like this
a + (b * c)   // 7

// now overriding precedence
// addition before multiplication   
(a + b) * c   // 9

// which is equivalent to
a * c + b * c // 9

```

<!--more-->

#### 分组运算符

> Return the result of evaluating Expression. This may be of type Reference.

>返回评估括号中的表达式的结果。结果可能是Reference类型。

抛开像Reference类型这种词汇，这里的一个关键词应当是“ 评估 ”（对evaluate的翻译一直把握不好，姑且这么叫吧），但是关于分组运算符，又有一个很重要的下文：

>This algorithm does not apply GetValue to the result of evaluating Expression.

>这个算法不会对估算的结果使用GetValue。

有很多专用的名词，看起来确实复杂，简而言之，使用括号运算符本身不会让括号中的代码立即执行，只有当括号包含的这个“分组”参与其他运算时，才会执行。因此，(function(){})()这个语句，其实是首先用分组运算符评估了一个函数表达式，随后参与“函数调用”。而(function(){}())这个语句，则是用分组运算符评估了一个函数调用，随后由于语句的结束而被执行。从语句上来说有细微的差距，当然就结果而言是一样的，


>credit


* [分组运算符](https://www.zhihu.com/question/20292224)
