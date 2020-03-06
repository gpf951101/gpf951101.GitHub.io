---
layout: article
title: Wireshark找不到VM虚拟机的网卡
key: May20190530_1
date: 2019-5-30
tags: [抓包, Wireshark]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---
&emsp;&emsp;前几天VM需要设置桥接方式，然后对虚拟机的网卡进行了重置。再次打开Wireshark之后，发现找不到虚拟网卡了。<br><!--more-->
&emsp;&emsp;网上中文查找了一些方法，都是什么windows设备管理器什么的，可是我这就没有“非即插即用驱动程序”。然后google英文搜索了一下，立马找到答案，我的问题解决了。可以试试。<br>
#### 1.使用管理员命令打开CMD。
#### 2.执行 `sc query npf`查询npf状态。
![npf状态](/images/20190530154957.png)
#### 3.执行 `sc stop npf`暂停npf。
![暂停npf](/images/20190530155149.png)
#### 4.执行 `sc start npf`开启npf。
![开启npf](/images/20190530155211.png)
#### 5.重新打开Wireshark或者刷新网卡`F5`。
