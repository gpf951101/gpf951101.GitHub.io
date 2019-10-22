---
layout: article
title: python numpy求每一行或列的最大值的索引
key: May20191022-2
date: 2019-10-22
tags: [python, numpy]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---

`axis=0`求的是每一列最大值的索引

`axis=1`求的是每一行最大值的索引

```python
import numpy as np
a = np.random.randint(50, size=(4, 5))
print(a)
[[20 21  7 44 39]
 [ 0 47 17 31 19]
 [35 35  1  2 31]
 [ 2 38 49  7 47]]
print(a.argmax(axis=1))
[3 1 0 2]
```

