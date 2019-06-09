---
layout: article
title: Ubuntu开通SSH服务
key: May20190609_1
date: 2019-6-9
tags: [Ubuntu, SSH]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---
&emsp;&emsp;今天重新安装了一个Ubuntu虚拟机，安装的时候，什么服务都没有开启，完全自己手动操作一下。先安装一下SSH服务。<!--more--><br>
#### 1.更新源
`sudo apt-get update`
#### 2.安装openssh-server
`sudo apt-get install openssh-server`
#### 3.查看状态
`ps -ef | grep ssh`<br>
如果包含sshd则默认启动，如果没开启，可以使用`sudo service ssh start`启动。
#### 4.修改SSH服务端口号
SSH默认端口号为22，可以修改。修改文件`/etc/ssh/ssh_config`的Port属性，然后使用`sudo service ssh restart`进行重启。