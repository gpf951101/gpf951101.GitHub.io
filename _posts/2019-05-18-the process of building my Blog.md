---
layout: article
title: 自己博客小站的建立过程
date: 2019-5-18
modify_date: 2019-5-19
sharing: true
categories: blog
tags: [随笔,技术,Blog]
lang: zh
key: May20190518_2
comment: true
pageview: true
show_edit_on_github: false
---

&emsp;&emsp;我的小站的建立主要参考的是这篇[博文](https://www.zhihu.com/question/20463581/answer/25478916)，使用的GitHub Pages，域名是从阿里云购买的<!--more-->，学生党资金有限。。。没钱购买com站点，只能用site站点咯。购买的时候，发现一个有趣的事情，这个域名购买5年的钱比购买3年的钱还便宜。。。购买的时候上面显示有优惠券可以用，自己可以搜索一下方法，关注一个阿里云域名相关的公众号就可以了。但是不知道是不是我使用方法有问题，显示优惠冲突，大致是因为第一次购买用户然后享受了优惠了吧，你们可以试一下能不能使用。<br>
&emsp;&emsp;然后域名配置解析的时候，开始没有按照上面博文的方法，自己ping了一下自己的github.io，将得到的ip填写到了解析规则里面，然后emmmm，访问速度特别慢。最后又加了两个IP，速度立马跟上，我添加的IP是`192.30.252.153`和` 192.30.252.154`，如果使用阿里云购买域名的话，不需要进行DNS修改，也就是原博文中的*Nameservers*，我就是在这栽了一个大跟头，花了好长时间。所以如果是阿里云购买的域名的话，只需要添加域名解析就可以了。<br>
&emsp;&emsp;祝你也早日有一个属于自己的博客小站。