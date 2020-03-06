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
<!--more-->
```python
import heapq

nums = [1, 8, 2, 23, 7, -4, 18, 23, 24, 37, 2]

# 最大的3个数的索引
max_num_index_list = map(nums.index, heapq.nlargest(3, nums))

# 最小的3个数的索引
min_num_index_list = map(nums.index, heapq.nsmallest(3, nums))

print(list(max_num_index_list))
print(list(min_num_index_list))
```
