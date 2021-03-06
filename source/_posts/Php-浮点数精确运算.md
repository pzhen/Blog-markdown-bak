---
title: Php - 浮点数精确运算
toc: true
date: 2017-08-03 14:55:40
share: true
tags:
 - 浮点数
 - 运算
 - 精度
categories:
 - Php
---

如果用php的+-*/计算浮点数的时候，可能会遇到一些计算结果错误的问题，所以基本上大部分语言都提供了精准计算的类库或函数库，比如php有BC高精确度函数库，下面我们介绍一下一些常用的BC高精确度函数使用。<!-- more -->

bc是Binary Calculator的缩写。bc*函数的参数都是操作数加上一个可选的 [int scale]，比如string bcadd(string $left_operand, string $right_operand[, int $scale])，如果scale没有提供，就用bcscale的缺省值。这里大数直接用一个由0-9组成的string表示，计算结果返回的也是一个 string。
bcadd — 将两个高精度数字相加 
bccomp — 比较两个高精度数字，返回-1, 0, 1 
bcdiv — 将两个高精度数字相除 
bcmod — 求高精度数字余数 
bcmul — 将两个高精度数字相乘 
bcpow — 求高精度数字乘方 
bcpowmod — 求高精度数字乘方求模，数论里非常常用 
bcscale — 配置默认小数点位数，相当于就是Linux bc中的”scale=” 
bcsqrt — 求高精度数字平方根 
bcsub — 将两个高精度数字相减

``` php
$a = 0.1;
$b = 0.7;
var_dump(($a + $b) == 0.8);
```

打印出来的值居然为 boolean false
这是为啥?PHP手册对于浮点数有以下警告信息:
Warning 
浮点数精度
显然简单的十进制分数如同 0.1 或 0.7 不能在不丢失一点点精度的情况下转换为内部二进制的格式。这就会造成混乱的结果：例如，floor((0.1+0.7)*10) 通常会返回 7 而不是预期中的 8，因为该结果内部的表示其实是类似 7.9999999999...。 
这和一个事实有关，那就是不可能精确的用有限位数表达某些十进制分数。例如，十进制的 1/3 变成了 0.3333333. . .。 
所以永远不要相信浮点数结果精确到了最后一位，也永远不要比较两个浮点数是否相等。如果确实需要更高的精度，应该使用任意精度数学函数或者 gmp 函数
那么上面的算式我们应该改写为

```php
$a = 0.1;
$b = 0.7;
var_dump(bcadd($a,$b,2) == 0.8);
```
这样就能解决浮点数的计算问题了


