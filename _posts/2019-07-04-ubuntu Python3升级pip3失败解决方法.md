---
layout: article
title: Python3升级pip3失败解决方法
key: May20190704_1
date: 2019-7-4
tags: [Ubuntu, python, 错误解决, pip3]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---
在python3虚拟环境中，升级pip3失败。<br>
尝试多种方法均不可以，每次都是下载一些内容之后就失败。<br>
<!--more-->
# 解决方法

- 根据提示连接，下载whl文件。
![下载连接](/images/20190704173311.png)<br>
- 使用命令`pip3 install xxx.whl`安装或者`pip3 install -U xxx.whl`更新
