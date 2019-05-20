---
layout: article
title: Ajax请求执行晚于之后的代码
date: 2019-5-20
categories: blog
tags: [技术,前端,Ajax]
lang: zh
key: May04
sharing: true
comment: true
pageview: true  
show_edit_on_github: false
---

&emsp;&emsp;今天写项目的时候，需要从后台获取数据，然后进行处理，但是自己前端输出发现Ajax请求的内容输出在Ajax代码之后。
<!--more-->太长时间不写项目，Ajax请求也都忘记了。开始觉得可能是Ajax请求花费时间较长，想着再Ajax请求之后加一个睡眠函数，尝试之后失败。<br>
&emsp;&emsp;后来查了Ajax的官方文档，看到了熟悉的字段`async`，然后自己一下子就清楚了。Ajax请求默认`async`是`true`，即请求是异步的，所以在进行Ajax请求的时候，下面的代码也在同时执行。所以想要解决我上述的问题，使代码按照顺序执行，只需要将字段`async`设置为`false`，保证代码的同步执行。解决。