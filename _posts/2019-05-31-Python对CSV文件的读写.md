---
layout: article
title: Python对CSV文件的读写
key: May20190531_1
date: 2019-5-31
tags: [python]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---
### 1.Python读取csv文件
如CSV文件名为`stock.csv`：<br>
```javascript
Symbol,Price,Date,Time,Change,Volume
"AA",39.48,"6/11/2007","9:36am",-0.18,181800
"AIG",71.38,"6/11/2007","9:36am",-0.15,195500
"AXP",62.58,"6/11/2007","9:36am",-0.46,935000
"BA",98.31,"6/11/2007","9:36am",+0.12,104800
"C",53.08,"6/11/2007","9:36am",-0.25,360900
"CAT",78.29,"6/11/2007","9:36am",-0.23,225400
```
<br>
#### 1.1 将这段数据读取为一个元组的序列
```python
import csv
with open('stocks.csv') as f:
    f_csv = csv.reader(f)
    headers = next(f_csv)
    for row in f_csv:
        print(row)
        print(type(row))
        print(row[1])
```
<br>
row是一个列表，可以用下标访问。<br>
![图一](/images/20190531165902.png)
<br>

#### 1.2 使用命名元组
```python
import csv
from collections import namedtuple
with open('stocks.csv') as f:
    f_csv = csv.reader(f)
    headings = next(f_csv)
    Row = namedtuple('Row', headings)
    for r in f_csv:
        row = Row(*r)
        print(row)
```
<br>
结果如下：<br>
![图二](/images/20190531170416.png)

#### 1.3 将数据读取到字典序列中
```python
import csv
with open('stocks.csv') as f:
    f_csv = csv.DictReader(f)
    for row in f_csv:
        print(type(row))
        print(row)
        print(row['Symbol'])
```
结果如下：<br>
![图三](/images/20190531170706.png)

### 2 python写入CSV文件
#### 2.1 写入数据是元组
```python
headers = ['Symbol','Price','Date','Time','Change','Volume']
rows = [('AA', 39.48, '6/11/2007', '9:36am', -0.18, 181800),
         ('AIG', 71.38, '6/11/2007', '9:36am', -0.15, 195500),
         ('AXP', 62.58, '6/11/2007', '9:36am', -0.46, 935000),
       ]
with open('stocks.csv','w') as f:
    f_csv = csv.writer(f)
    f_csv.writerow(headers)
    f_csv.writerows(rows)
```
![图四](/images/20190531171252.png)
#### 2.2 写入数据是字典
```python
headers = ['Symbol', 'Price', 'Date', 'Time', 'Change', 'Volume']
rows = [{'Symbol':'AA', 'Price':39.48, 'Date':'6/11/2007',
        'Time':'9:36am', 'Change':-0.18, 'Volume':181800},
        {'Symbol':'AIG', 'Price': 71.38, 'Date':'6/11/2007',
        'Time':'9:36am', 'Change':-0.15, 'Volume': 195500},
        {'Symbol':'AXP', 'Price': 62.58, 'Date':'6/11/2007',
        'Time':'9:36am', 'Change':-0.46, 'Volume': 935000},
        ]
with open('stocks.csv','w') as f:
    f_csv = csv.DictWriter(f, headers)
    f_csv.writeheader()
    f_csv.writerows(rows)
```
![图五](/images/20190531171252.png)
<br>
参考链接：[地址](https://python3-cookbook.readthedocs.io/zh_CN/latest/c06/p01_read_write_csv_data.html#id1)
<br>
可以使用`open(birth_weight_file,'w',newline='')`解决中间有空行的问题。