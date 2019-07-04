---
layout: article
title: Ubuntu环境下php+fastcgi+nginx+MySQL搭建Wordpress环境
key: May20190609_3
date: 2019-6-9
tags: [Ubuntu, wordpress, nginx, php, MySQL]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---
## 1.安装Nginx 
- `sudo apt-get update`
- `sudo apt-get install nginx`
打开本地IP，出现如下画面：<br>
![nginx访问成功](/images/20190609181113.png)<br>
<!--more-->
## 2.安装MySQL 
`sudo apt-get install mysql-server`<br>
然后安装过程中会让设置`root`用户的密码，设置之后确认。<br>

## 3.安装并配置PHP
### 3.1 安装PHP 
`sudo apt-get install php-fpm php-mysql`<br>

### 3.2 配置PHP
打开配置文件：`sudo vim /etc/php/7.0/fpm/php.ini`，使用“/”查找到`cgi.fix_pathinfo`，将注释取消，并设置值为0。<br>
重新启动PHP：`sudo systemctl restart php7.0-fpm`<br>

## 3.下载Wordpress 
我是重新部署，因为有之前的wordpress项目，所以我直接将原有项目copy到指定位置。如果是第一次安装可以直接下载wordpress，然后解压到指定位置，然后根据wordpress的说明，对项目进行配置即可。<br>
- 将就得html重命名：`sudo mv html/ oldhtml/`
- 修改www目录所属用户：`sudo chown user www/`
- 将新的html拷贝进去
- 将原有数据库信息进行复原

## 4.配置Nginx 
我是之前也有nginx配置文件，所以直接进行了copy。如果没有可以参考其他Nginx配置的相关资料。
- 检查nginx语法 `sudo nginx -t`
- 重启nginx `sudo systemctl reload nginx`

## 5.安装PHP其他拓展 
- `sudo apt-get update`
- `sudo apt-get install php-curl php-gd php-mbstring php-mcrypt php-xml php-xmlrpc`

## 6.重启PHP 
`sudo systemctl restart php7.0-fpm`<br>
