---
layout: post
title:  懒加载和预加载
subtitle: "懒加载和预加载区别"
tags:
    - JS 
    - Js 优化 
    - 懒加载和预加载
    - 前端  
---



# 懒加载

曾经的一个面试题，没答出来，记录一下。

图片懒加载已经是很久前的知识点了，但是当初自己只用过 JQ 的插件，却没有深究过它的原理。



## 什么是懒加载？

懒加载也就是延迟加载。
当访问一个页面的时候，先把img元素或是其他元素的背景图片路径替换成一张大小为1*1px图片的路径（这样就只需请求一次，俗称占位图），只有当图片出现在浏览器的可视区域内时，才设置图片正真的路径，让图片显示出来。这就是图片懒加载。

延迟加载有多种实现，这里只说其中一种，其实原理都差不多的：


## 为什么要使用懒加载？

很多页面，内容很丰富，页面很长，图片较多。比如说各种商城页面。这些页面图片数量多，而且比较大，少说百来K，多则上兆。要是页面载入就一次性加载完毕。 影响页面打开速度，用户体验也不好。


## 懒加载的原理是什么？

> 页面中的img元素，如果没有src属性，浏览器就不会发出请求去下载图片，只有通过javascript设置了图片路径，浏览器才会发送请求。
懒加载的原理就是先在页面中把所有的图片统一使用一张占位图进行占位，把正真的路径存在元素的“data-url”（这个名字起个自己认识好记的就行）属性里，要用的时候就取出来，再设置为图片的真实src；

## 懒加载的实现步骤？

> 1)首先，不要将图片地址放到src属性中，而是放到其它属性(data-original)中。<br />
> 2)页面加载完成后，根据scrollTop判断图片是否在用户的视野内，如果在，则将data-original属性中的值取出存放到src属性中。<br />
> 3)在滚动事件中重复判断图片是否进入视野，如果进入，则将data-original属性中的值取出存放到src属性中。


## 实现方式

* 第一种是纯粹的延迟加载，使用setTimeOut或setInterval进行加载延迟.
* 第二种是条件加载，符合某些条件，或触发了某些事件才开始异步下载。
* 第三种是可视区加载，即仅加载用户可以看到的区域，这个主要由监控滚动条来实现，一般会在距用户看到某图片前一定距离遍开始加载，这样能保证用户拉下时正好能看到图片。

## 实现代码（可视区加载为例）


JS 的实现：

```
var imgs = document.getElementsByTagName('img');
// 获取视口高度与滚动条的偏移量
function lazyload(){
    var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
    var viewportSize = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
    for(var i=0; i<imgs.length; i++) {
	var x =scrollTop+viewportSize-imgs[i].offsetTop;
	if(x>0){
	    imgs[i].src = imgs[i].getAttribute('loadpic');   
	}
    }
}
setInterval(lazyload,1000);
```


JQ 的实现：

```
/**
* 图片的src实现原理
*/
$(document).ready(function(){
    // 获取页面视口高度
    var viewportHeight = $(window).height();
    var lazyload = function() {
       // 获取窗口滚动条距离
       var scrollTop = $(window).scrollTop();
       $('img').each(function(){
           // 判断 视口高度+滚动条距离 与 图片元素距离文档原点的高度		 
           var x = scrollTop + viewportHeight - $(this).position().top;
           // 如果大于0 即该元素能被浏览者看到，则将暂存于自定义属性loadpic的值赋值给真正的src			
           if (x > 0) {
               $(this).attr('src',$(this).attr('loadpic')); 
           }
       })
    }
    // 创建定时器 “实时”计算每个元素的src是否应该被赋值
    setInterval(lazyload,100);
});
```


## 懒加载的优点是什么？

页面加载速度快、可以减轻服务器的压力、节约了流量,用户体验好

## 补充知识点


![网页请求过程](https://lilywei739.github.io/img/20170206/20170206-1.jpg)

1、 屏幕可视窗口大小：对应于图中1、2位置处

* 原生方法：window.innerHeight 标准浏览器及IE9+ || document.documentElement.clientHeight 标准浏览器及低版本IE标准模式 ||
document.body.clientHeight 低版本混杂模式
* jQ方法：$(window).height()

2、浏览器窗口顶部与文档顶部之间的距离，也就是滚动条滚动的距离：也就是图中3、4处对应的位置；

* 原生方法：window.pagYoffset——IE9+及标准浏览器 || document.documentElement.scrollTop 兼容ie低版本的标准模式 ||
document.body.scrollTop 兼容混杂模式；
* jQ方法：$(document).scrollTop();


3、获取元素的尺寸：对应于图中5、6位置处；

左边jquery方法，右边原生方法

```
$(e).width() = e.style.width;
$(e).innerWidth() = e.style.width + e.style.padding;
$(e).outerWidth() = e.offsetWidth = e.style.width + e.style.padding + e.style.border;
$(e).outerWidth(true) = e.style.width + e.style.padding + e.style.border + e.style.margin;
```

> 注意：要使用原生的style.xxx方法获取属性，这个元素必须已经有内嵌的样式，如<div style="...."></div>； <br />
> 如果原先是通过外部或内部样式表定义css样式，必须使用
e.currentStyle[xxx] || document.defaultView.getComputedStyle(e)[xxx]来获取样式值

4、获取元素的位置信息：对应与图中7、8位置处

* 原生：getoffsetTop()
* jQuery：
    * $(e).offset().top元素距离文档顶的距离
    * $(e).offset().left元素距离文档左边缘的距离


5、顺便提一个知识点，返回元素相对于第一个以定位的父元素的偏移距离，注意与上面偏移距的区别；

* jQuery：position()返回一个对象
   * $(e).position().left = style.left，
   * $(e).position().top = style.top；


# 预加载

提前加载图片，当用户需要查看时可直接从本地缓存中渲染

## 为什么要使用预加载？

图片预先加载到浏览器中，访问者便可顺利地在你的网站上冲浪，并享受到极快的加载速度。这对图片画廊及图片占据很大比例的网站来说十分有利，它保证了图片快速、无缝地发布，也可帮助用户在浏览你网站内容时获得更好的用户体验。


## 实现原理

让img 标签先显示其他的图片，当其指向的真实图片缓存完成后，再显示为实际的图片。

## 实现预加载的方法有哪些？

实现图片预加载方法有很多，可以用CSS(background)、JS(Image)、HTML(<img />)，常用的是使用new Image()，设置其src属性实现图片预加载，然后使用image对象的onload事件把预加载完成的图片赋值给要显示该图片的<img>标签。



## 实现代码


```
function preLoadImg(url, callback) {
    var img = new Image();
    img.src = url;
    //兼容ie、opera刷新页面时，不触发onload事件
    if (img.complete) { // 如果图片已经存在于浏览器缓存，直接调用回调函数
        callback(img);
        return; // 直接返回，不用再处理onload事件
    }
    img.onload = function() { //图片下载完毕时异步调用callback函数。
        callback(img);
    };
}
window.onload = function() {
    var arr = ["img/11.jpg", "img/12.jpg", "img/13.jpg"],
        imgs = document.getElementsByTagName("img"),
        len = imgs.length;
    preLoadImg(arr[0], function(data) {
        imgs[0].src = data.src;
    });
    preLoadImg(arr[1], function(data) {
        imgs[1].src = data.src;
    });
};
```


首先实例化一个Image对象赋值给img，然后设置img.src为参数url指定的图片地址,接着判断img的complete属性，如果本地有这张图片的缓存，则该值为true，此时我们可以直接操作这张图片，如果本地没有缓存，则该值为false，此时我们需要监听img的onload事件。



# 懒加载和预加载的对比

## 概念：

* 懒加载也叫延迟加载：JS图片延迟加载,延迟加载图片或符合某些条件时才加载某些图片。
* 预加载：提前加载图片，当用户需要查看时可直接从本地缓存中渲染。


## 区别：

两种技术的本质：两者的行为是相反的，一个是提前加载，一个是迟缓甚至不加载。懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。



参考博文：[预加载实现讲解](http://www.cnblogs.com/v10258/p/3376455.html)


