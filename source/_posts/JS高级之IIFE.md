---
title: JS基础之IIFE
date: 2017-03-09 23:26:32
categories: "JS"
tags:
     - JS高级
     - 前端进阶
---

> 什么是自执行（Immediately-Invoked Function Expression）？

在JavaScript里，任何function在执行的时候都会创建一个执行上下文，因为为function声明的变量和function有可能只在该function内部，这个上下文，在调用function的时候，提供了一种简单的方式来创建自由变量或私有子function。

```
// 由于该function里返回了另外一个function，其中这个function可以访问自由变量i
// 所有说，这个内部的function实际上是有权限可以调用内部的对象。

function makeCounter() {
    // 只能在makeCounter内部访问i
    var i = 0;

    return function () {
        console.log(++i);
    };
}

// 注意，counter和counter2是不同的实例，分别有自己范围内的i。

var counter = makeCounter();
counter(); // logs: 1
counter(); // logs: 2

var counter2 = makeCounter();
counter2(); // logs: 1
counter2(); // logs: 2

alert(i); // 引用错误：i没有defind（因为i是存在于makeCounter内部）。
```

很多情况下，我们不需要makeCounter多个实例，甚至某些case下，我们也不需要显示的返回值，OK，往下看。

<!--more-->

#### 问题的核心

当你声明类似function foo(){}或var foo = function(){}函数的时候，通过在后面加个括弧就可以实现自执行，例如foo()，看代码：

```
// 因为想下面第一个声明的function可以在后面加一个括弧()就可以自己执行了，比如foo()，
// 因为foo仅仅是function() { /* code */ }这个表达式的一个引用

var foo = function(){ /* code */ }

// ...是不是意味着后面加个括弧都可以自动执行？

function(){ /* code */ }(); // SyntaxError: Unexpected token (
//
```
上述代码，如果甚至运行，第2个代码会出错，因为在解析器解析全局的function或者function内部function关键字的时候，默认是认为function声明，而不是function表达式，如果你不显示告诉编译器，它默认会声明成一个缺少名字的function，并且抛出一个语法错误信息，因为function声明需要一个名字。

#### 旁白：函数(function)，括弧(paren)，语法错误(SyntaxError)

有趣的是，即便你为上面那个错误的代码加上一个名字，他也会提示语法错误，只不过和上面的原因不一样。在一个表达式后面加上括号()，该表达式会立即执行，但是在一个语句后面加上括号()，是完全不一样的意思，他的只是分组操作符。

```
// 下面这个function在语法上是没问题的，但是依然只是一个语句
// 加上括号()以后依然会报错，因为分组操作符需要包含表达式

function foo(){ /* code */ }(); // SyntaxError: Unexpected token )

// 但是如果你在括弧()里传入一个表达式，将不会有异常抛出
// 但是foo函数依然不会执行
function foo(){ /* code */ }( 1 );

// 因为它完全等价于下面这个代码，一个function声明后面，又声明了一个毫无关系的表达式：
function foo(){ /* code */ }

( 1 );
```

>自执行函数表达式

要解决上述问题，非常简单，我们只需要用大括弧将代码的代码全部括住就行了，因为JavaScript里括弧()里面不能包含语句，所以在这一点上，解析器在解析function关键字的时候，会将相应的代码解析成function表达式，而不是function声明。

```
// 下面2个括弧()都会立即执行

(function () { /* code */ } ()); // 推荐使用这个
(function () { /* code */ })(); // 但是这个也是可以用的

// 由于括弧()和JS的&&，异或，逗号等操作符是在函数表达式和函数声明上消除歧义的
// 所以一旦解析器知道其中一个已经是表达式了，其它的也都默认为表达式了
// 不过，请注意下一章节的内容解释

var i = function () { return 10; } ();
true && function () { /* code */ } ();
0, function () { /* code */ } ();

// 如果你不在意返回值，或者不怕难以阅读
// 你甚至可以在function前面加一元操作符号

!function () { /* code */ } ();
~function () { /* code */ } ();
-function () { /* code */ } ();
+function () { /* code */ } ();

// 还有一个情况，使用new关键字,也可以用，但我不确定它的效率
// http://twitter.com/kuvos/status/18209252090847232

new function () { /* code */ }
new function () { /* code */ } () // 如果需要传递参数，只需要加上括弧()
```

上面所说的括弧是消除歧义的，其实压根就没必要，因为括弧本来内部本来期望的就是函数表达式，但是我们依然用它，主要是为了方便开发人员阅读，当你让这些已经自动执行的表达式赋值给一个变量的时候，我们看到开头有括弧(，很快就能明白，而不需要将代码拉到最后看看到底有没有加括弧。


> credit

* [立即调用的函数表达式](http://www.cnblogs.com/TomXu/archive/2011/12/31/2289423.html)

* [立即调用的函数表达式存在的意义](https://www.zhihu.com/question/35832285?sort=created)

* [IFEE](http://lipeng1667.github.io/2016/12/20/IIFE-in-js/)
