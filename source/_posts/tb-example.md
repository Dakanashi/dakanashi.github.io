---
title: 淘宝爬虫实例
date: 2020-04-14 15:32:57
tags:
- Crawler
- Example
- Python
categories:
- Python爬虫
---
***淘宝爬虫实例***
<!--more-->
---
- **功能描述**
  - 目标：获取淘宝搜索页面的信息，提取其中的商品名称和价格
  - 理解：
    > 淘宝的搜索接口
    > 翻页的处理
  - 技术路线：
    > requests
    > bs4
    > re

- **搜索接口和翻页的URL对应属性**
    - 书包链接（每页44个商品）
        > 起始页：https://s.taobao.com/search?q=书包&js=1&stats_click=search_radio_all%3A1&initiative_id=staobaoz_20170105&ie=utf8
        > 第二页：https://s.taobao.com/search?q=书包&js=1&stats_click=search_radio_all%3A1&initiative_id=staobaoz_20170105&ie=utf8&bcoffset=0&ntoffset=0&p4ppushleft=1%2C48&s=44
        > 第三页：https://s.taobao.com/search?q=书包&js=1&stats_click=search_radio_all%3A1&initiative_id=staobaoz_20170105&ie=utf8&bcoffset=-3&ntoffset=-3&p4ppushleft=1%2C48&s=88

- **注意**
    - 打开淘宝网的robots.txt
        ```txt
        User-agent: *
        Disallow: /
        ```
    - 仅探讨技术实现，请不要不加限制的爬取该网站

- **程序的结构设计**
    > 步骤1：提交商品搜索请求，循环获取页面
    > 步骤2：对于每个页面，提取商品名称和价格信息
    > 步骤3：将信息输出到屏幕上

- **程序代码**
```python
import requests
import re
def getHTMLText(url):
    try:
        r = requests.get(url, timeout=30)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        print("Error")

def parsePage(ilt, html):
    try:
        plt = re.findall(r'\"view_price\"\:\"[\d\.]*\"',html)
        tlt = re.findall(r'\"raw_title\"\:\".*?\"',html)
        for i in range(len(plt))):
            # eval()用于去除引号
            price = eval(plt[i].split(':')[1])
            title = eval(tlt[i].split(':')[1])
            ilt.append([price, title])
    except:
        print("Error2")

def printGoodsList(ilt):
    tplt = "{:4}\t{:8}\t{:16}"
    print(tplt.format("序号", "价格", "商品名称"))
    count = 0
    for g in ilt:
        count = count + 1
        print(tplt.format(count, g[0], g[1]))

def main():
    goods = '书包'
    depth = 2
    start_url = 'https://s.taobao.com/search?q='+goods
    infoList = []
    for i in range(depth):
        try:
            url = start_url + '&s=' + str(44*i)
            html = getHTMLText(url)
            parsePage(infoList, html)
        except:
            continue
    printGoodsList(infoList)

if __name__ == '__main__'
    main()
```