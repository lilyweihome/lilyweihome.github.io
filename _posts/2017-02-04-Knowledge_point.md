---
layout: post
title:  JS小知识点记录 
subtitle: "JS小知识点"
tags:
    - JS 
    - JS小知识点 
    - 前端  
---



# JS小知识点记录

## 一、undefined与null的区别

undefined与null均表示无的意思，用到的地方不一样。

null表示"没有对象"，即该处不应该有值。典型用法是：

>（1） 作为函数的参数，表示该函数的参数不是对象。<br />
>（2） 作为对象原型链的终点。

undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。典型用法是：

>（1）变量被声明了，但没有赋值时，就等于undefined。<br />
>（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。<br />
>（3）对象没有赋值的属性，该属性的值为undefined。<br />
>（4）函数没有返回值时，默认返回undefined。

```
var i;
i // undefined

function f(x){console.log(x)}
f() // undefined

var  o = new Object();
o.p // undefined

var x = f();
x // undefined


typeof null ==>"object"
typeof undefined ==> "undefined"
```


## 二、






