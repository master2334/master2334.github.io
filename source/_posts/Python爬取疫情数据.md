---
title: Python爬取疫情实时数据
date: 2021-1-18 14:30:00
categories: 程序经验
tags: 
- Python
- 爬虫
- 实时数据流
---

## 用python爬取疫情数据网站的实时数据流
一些网站的数据并没有写在html源码中，而是通过数据流实时获取的，因此需要在F12中找到数据流的JSON文件，通过解析JSON文件来获取自己想要的信息。
```python
import requests
from bs4 import BeautifulSoup
from fake_useragent import UserAgent
import json, jsonpath
import sqlite3

url = 'https://api.inews.qq.com/newsqa/v1/query/inner/publish/modules/list?modules=chinaDayList,chinaDayAddList,nowConfirmStatis,provinceCompare'

conn = sqlite3.connect('.\YQdata.db')
c = conn.cursor()
c.execute('''CREATE TABLE IF NOT EXISTS datas
             (Province text, nowConfirm text, confirmAdd text, dead text, heal text)''')

#requests.session() 会话保持，可以让我们在跨请求时保存某些操作
#在爬虫中，需要用到session来保持登录状态，一些网站会要求强制登陆，登录信息不完整时，返回的数据信息也不完整
session = requests.session()
result = session.get(url)
#json.loads()  将json格式解析为dict格式  
#json.dumps()  将dict格式解析为json格式
resJson = json.loads(result.text)
data = jsonpath.jsonpath(resJson, '$.data.chinaDayAddList.*')

for d in data:
    #res为字符串格式的拼接   d['date']为字典键值的引用
    res = '日期:' + d['date'] + '--' + '新增确诊:' + str(d['confirm']) + '--' + '死亡人数:' + str(d['dead']) + '--' + '感染人数:' + str(d['infect'])+ '--' + '治愈人数:' + str(d['heal'])
    # 保存数据到指定路径
    file = '.\global-yq.txt'
    with open(file, 'a+',encoding='utf-8') as f:
        f.write(res + '\n')  # 加\n换行显示
data2 = jsonpath.jsonpath(resJson, '$.data.provinceCompare.')

# 字典格式的常用方法
# for k in data2[0].values():
#     print(data2[0][k])

#使用zip方法，将两个list同时迭代
for (key, value) in zip(data2[0].keys(), data2[0].values()):
   # print(value)
    com = "INSERT INTO datas VALUES (?,?,?,?,?)"
    c.execute(com,tuple((str(key),value['nowConfirm'],value['confirmAdd'],value['dead'],value['heal'])))
    res =  str(key) + '--' + '新增确诊:' + str(value['nowConfirm'])  + '--' + '无症状感染者:' + str(value['confirmAdd']) + '--' +  '死亡人数:' + str(value['dead']) + '--' +  '治愈人数:' + str(value['heal'])
    # 保存数据到我的d盘
    file = '.\china-yq.txt'
    with open(file, 'a+',encoding='utf-8') as f:
        f.write(res + '\n')  # 加\n换行显示

conn.commit()
conn.close()



```