---
layout: article
title: WAMP+DVWA学习SQL注入
sharing: true
show_author_profile: true
lang: zh
key: May20190522_1
comment: true
pageview: true
date: 2019-5-22
modify_date: 2019-5-23
tags: [技术,网安,SQL注入]
---

&emsp;&emsp;刚刚开始学习网络安全相关的知识，身为一个网络空间安全的学生，对一些基本的安全名词和安全操作不太了解，有些说不过去。今天主要总结下WAMP+DVWA环境下学习SQL注入。<!--more--><br>

**如果自己只是想简单的操作一下SQL注入，不需要自己搭建环境，注册实验楼账号，然后可以直接使用实验楼提供的虚拟机进行操作，[链接地址](https://www.shiyanlou.com/courses/876)**<br>

&emsp;&emsp;我在学习的过程中发现，好多知识主要针对的语言是PHP，可能是PHP语言和前端结合比较紧密，搭建起来比较简单，所以容易进行教学演示吧。当然，这只是我自己的一些理解。<br>
&emsp;&emsp;因为我之前学习PHP自己windows系统安装了WAMP，所以主要使用WAMP+DVWA环境进行的学习。具体的环境搭建参考别人的一遍博文，[链接地址](https://blog.csdn.net/qq_32261191/article/details/80338091)。因为我之前安装了别的服务器，80端口被占用，所以修改了Apache的`httpd.conf`文件，监听的端口是8080。然后因为本地安装有MySQL数据库，所以WAMP的MySQL数据库会启动失败，然后只需要在服务中将开机自启的本地MySQL数据库暂停即可保证WAMP中的MySQL数据库正常启动。<br>

**紧急更新：不要轻易关闭本地的MySQL服务，我今天重新启动失败了。。。建议尝试其他方法**<br>
&emsp;&emsp;然后将DVWA解压放置在php.in指定的目录中即可，默认是WAMP安装目录下的www目录。DVWA的SQL注入的安全等级默认是`impossible`，需要在`DVWA Security`中根据自己的需求进行更改。然后就可以进行简单的SQL注入了。<br>
&emsp;&emsp;因为我也是刚刚开始学习SQL注入，没有自己的一套思想，所以就不自己写自己怎样进行的操作，在这贴出我当时进行简单操作的[参考博文](https://www.jianshu.com/p/078df7a35671)。希望自己有一天能有自己对SQL注入的理解，然后再来修改这篇博文。<br>