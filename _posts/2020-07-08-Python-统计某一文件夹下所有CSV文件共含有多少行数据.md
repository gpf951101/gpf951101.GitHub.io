---
layout: article
title: Python-统计某一文件夹下所有CSV文件共含有多少行数据
key: Cyber20200708_1
date: 2020-7-8
tags: [python]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---



```python
def getCountOfDir(filepath):
    fileList = os.listdir(filepath)
    sum = 0
    for file in fileList:
        with open(filepath+"/"+file, encoding="utf8") as rf:
            sum += len(rf.readlines())
    print(sum)
```

<br>

