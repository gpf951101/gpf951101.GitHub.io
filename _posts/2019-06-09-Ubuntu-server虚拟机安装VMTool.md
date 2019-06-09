---
layout: article
title: Ubuntu开通SSH服务
key: May20190609_2
date: 2019-6-9
tags: [Ubuntu, SSH]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---
&emsp;&emsp;今天重新安装了一个Ubuntu虚拟机，因为复制等操作不方便，所以可以安装VMTool。<!--more--><br>
#### 1.点击VM菜单栏中的虚拟机，然后选择安装VMTool工具。
#### 2.在虚拟机中挂载 

- `sudo mkdir /mnt/vmtool` #创建文件夹
- `mount /dev/cdrom /mnt/vmtool`
- `cd /mnt/vmtool`
- `cp VMwareTools-10.2.5-8068393.tar.gz /var` #从挂载目录复制出去
- `cd /var`
- `sudo tar -zxvf VMwareTools-10.2.5-8068393.tar.gz` 
- `cd vmware-tools-distrib`
- `sudo ././vmware-install.pl`

#### 3.默认不断回车安装即可