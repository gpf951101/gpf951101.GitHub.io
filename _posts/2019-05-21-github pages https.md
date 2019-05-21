---
layout: article
title: github pages 搭建博客并且进行https认证
date: 2019-5-21
modify_date: 2019-5-22
categories: blog
tags: [技术,搭站,Blog]
lang: zh
key: May20190521_1
sharing: true
comment: true
pageview: true  
show_edit_on_github: false
---

&emsp;&emsp;每次登陆自己的网站都看到有**不安全**的提示，有点强迫症，于是看了下SSL认证，感觉还挺简单的。<!--more--><br>
&emsp;&emsp;首先我尝试从我的域名购买商那进行申请，去到了阿里云，然后也购买了免费的SSL证书，但是，因为博客是部署在github pages上面的，没有服务器，不会对证书进行安装，然后我就不知道下一步该怎么做了。我又换了一种思路，直接Google搜索`github pages https`，然后果然不负期望，搜索出来好多结果，然后按照其中一个进行了更改。我大体再对步骤进行一下总结。<br>
### 1.修改DNS解析
&emsp;&emsp;在初始搭建Blog的时候，设置了DNS解析，添加的记录是`192.30.252.153`和` 192.30.252.154`，[搭建过程](https://cyberbug.site/blog/2019/05/18/the-process-of-building-my-Blog.html)。然后现在需要做的就是把这两条记录替换掉，具体的IP地址是

 - 185.199.108.153
 - 185.199.109.153
 - 185.199.110.153
 - 185.199.111.153

当然，如果变更了，可以从[官方](https://help.github.com/en/articles/setting-up-an-apex-domain#configuring-a-records-with-your-dns-provider)进行查找。我再进行替换的时候没有删除原先的两条记录，直接进行了暂停，添加新的记录之后，效果如下：
![修改记录之后的解析DNS](/images/20190521151238.png)

### 2.修改hithub.io属性
&emsp;&emsp;去github找到你的github.io文件，然后进入设置选项卡，在`Options`->`GitHub Pages`下有个`Enforce HTTPS`，然后勾选就可以(也可以进入设置页面直接搜索`Enforce HTTPS`)。
![Enforce HTTPS](/images/20190521151843.png)

### 3.静静等待设置生效就可以啦
&emsp;&emsp;最后配置生效之后，在域名DNS解析处会增加一条解析，<br>
![DNS增加解析](/images/20190521152151.png)<br>
然后发现github.io处的设置也会发生变化，会有绿色的对勾提示。<br>
![变化](/images/20190521152337.png)。<br>
查看自己的域名访问：<br>
![域名访问](/images/20190521152444.png)<br>
结束。

### 附加更新
&emsp;&emsp;提交部署之后，发现阅读量看不到了，开始没在意，毕竟之前也遇到过。后来发现评论也看不到，且提示`Code 403: 访问被api域名白名单拒绝，请检查你的安全域名设置.`。想起来在使用LeanCloud模块的时候，设置的`Web安全域名`中的协议是http协议，然后更改成https协议之后，导致无法访问，所以只要把此处的安全域名修改一下就可以重新看到评论和浏览量。<br>
![Web安全域名](/images/20190521162104.png)
