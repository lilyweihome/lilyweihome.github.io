---
layout: post
title: css3动画事件
subtitle: "css3动画事件"
tags:
    - JS 
    - 动画事件 
    - 前端  
---



# css3动画事件

前段时间级别评审的时候，同事提出一个问题，怎么样能控制执行完一个动画后接着执行另一个动画，当时回答的不好，现在总结一下这个问题的解决思路及方法。

## 思路一：计时器

样式中设置两个动画，A、B，设置同A动画一样时长的 time，过了time时间后setTimeout一个动画，执行动画B


----------


## 思路二：css3动画事件

css3提供了animation来实现动画，在JS中也有对应的 animation事件---开始、迭代、结束。(话说之前都没有关注过这个事件，罪过罪过)

格式如下：

``` 
animationstart	     // 开始
animationend	     // 结束
animationiteration  // 迭代	
``` 

**copy 过来的优点：**

 >1. 能够非常容易地创建简单动画，你甚至不需要了解JavaScript就能创建动画。
 >2. 动画运行效果良好，甚至在低性能的系统上。渲染引擎会使用跳帧或者其他技术以保证动画表现尽可能的流畅。而使用JavaScript实现的动画通常表现不佳（除非经过很好的设计）。
 >3. 让浏览器控制动画序列，允许浏览器优化性能和效果，如降低位于隐藏选项卡中的动画更新频率。


事件中的开始和结束指的是整个动画的"开始"和"结束"，只执行一次。而迭代是什么呢？在动画中有一个属性可以设置动画执行的次数，所以会有很多的开始和结束，这中间重复动画的结束并开始下一次将触发整个事件“迭代”事件。

**属性说明**

- 动画第一次开始时触发 animationstart
- 除了首次动画外，其它的每次动画迭代触发 animationiteration
- 动画结束时触发 animationend

比如一个循环3次的动画，在首次触发时，会触发start，在后两次的循环中会触发两次的animationiteration事件，最后触发一次end事件。

CSS:

![image01](https://lilywei739.github.io/img/161124-3.jpg)

JS:

![image01](https://lilywei739.github.io/img/161124-1.jpg)

结果
![image01](https://lilywei739.github.io/img/161124-2.jpg)


## 总结

所以回答上面的这个问题的答案是：


```
var box = document.getElementById('box');

box.className = 'animation1';

box.addEventListener('animationstart', function () {
    console.log('start');
}, false)

box.addEventListener('animationiteration', function () {
    console.log('loding');
}, false)

box.addEventListener('animationend', function () {
    console.log('end');
    box.className = 'animation2';    //在A动画结束后开始第二个动画
}, false)
``` 


