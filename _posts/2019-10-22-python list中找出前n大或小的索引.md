---
layout: article
title: python list中找出前n大或小的索引
key: May20191022-1
date: 2019-10-22
tags: [python]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---

```python
import heapq

nums = [1, 8, 2, 23, 7, -4, 18, 23, 24, 37, 2]

# 最大的3个数的索引
max_num_index_list = map(nums.index, heapq.nlargest(3, nums))

# 最小的3个数的索引
min_num_index_list = map(nums.index, heapq.nsmallest(3, nums))

print(list(max_num_index_list))
print(list(min_num_index_list))
————————————————
版权声明：本文为CSDN博主「Immok」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ns2250225/article/details/80118621
```