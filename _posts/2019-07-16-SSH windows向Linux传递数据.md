---
layout: article
title: SSH windows向Linux传递数据 nltk_data离线下载安装
key: May20190716_1
date: 2019-7-16
tags: [SSH, Linux, windows, 文件传输, nltk_data, 离线]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---
## 文件传输

使用scp命令在windows CMD中进行操作:<br>
`scp /path/filename username@servername:/path/`<br>
前者是windows下的路径，后者是Linux服务器的路径，如果下载，则互换位置即可，其他两台服务器之间传输文件的命令也是scp。<br>

## nltk_data离线下载

[nltk_data下载路径](https://github.com/nltk/nltk_data/tree/gh-pages/packages).<br>

使用的时候会提示从哪些位置找nltk_data数据包，然后从上面的网址下载上nltk数据包然后放到指定位置解压即可。我一般是放在当前用户的nltk_data目录下，文档结构要与上面网址的结构相同，文件要放在和网址相同的文件夹下。<br>
