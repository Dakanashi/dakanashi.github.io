---
title: 几个简单的爬虫实例
date: 2020-04-11 14:43:13
tags:
- Crawler
- Python
categories:
- Python爬虫
---
***爬虫实例记录。***
<!--more-->
---
## **中国大学排名定向爬虫**
- **网站**
  > http://www.zuihaodaxue.cn/zuihaodaxuepaiming2016.html

- **功能描述**
  > 输入：大学排名URL链接。
  > 输出：大学爱明信息的屏幕输出（排名，大学名称，总分）。
  > 技术路线：request-bs4。
  > 定向爬虫：仅对输入URL进行爬取，不扩展爬取。

- **程序的设计步骤**
  > 步骤1：从网络上获取大学排名网页内容，使用getHTMLText()。
  > 步骤2：提取网页内容中信息到合适的数据结构，使用fillUnivList()。
  > 步骤3：利用数据结构展示并输出结果，使用printUnivList()。

- **实例编写**
```python
import requests
from bs4 import BeautifulSoup
import bs4

def getHTMLText(url):
    try:
        r = requests.get(url, timeout=30)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text
    except:
        return ""

def fillUnivList(ulist, html):
    soup = BeautifulSoup(html, "html.parser")
    for tr in soup.find('tbody').children:
        if isinstance(tr, bs4.element.Tag):
            tds = tr('td')
            ulist,append([tds[0].string, tds[1].string, tds[3].string])


def printUnivList(ulist, num):
    print("{:^10}\t{:^6}\t{:^10}".format("排名", "学校名单", "总分"))
    for i in range(num):
        u = ulist[i]
        print("{:^10}\t{:^6}\t{:^10}\t".format(u[0], u[1], u[2]))

def main():
    uinfo = []
    url = 'http://www.zuihaodaxue.cn/zuihaodaxuepaiming2016.html'
    html = getHTMLText(url)
    fillUnivList(uinfo, html)
    printUnivList(uinfo, 20) # 打印20个大学

if __name__ == '__main__':
    main()
```

**优化**
- **中文字符对齐问题的解决**
  > 采用中文字符的空格填充chr(12288)
```python
def printUnivList(ulist, num):
    tplt = {0:^10}\t{1:^6}\t{2:^10}
    print(tplt.format("排名", "学校名单", "总分"), chr(12288))
    for i in range(num):
        u = ulist[i]
        print(tplt.format(u[0], u[1], u[2], chr(12288)))
```