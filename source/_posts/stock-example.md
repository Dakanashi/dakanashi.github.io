---
title: 股票爬虫实例
date: 2020-04-15 00:30:53
tags:
- Crawler
- Example
- Python
categories:
- Python爬虫
---
***股票爬虫实例***
<!--more-->
---
- **功能描述**
  - 目标：获取上交所和深交所所有股票的名称和交易信息
  - 输出：保存到文件中
  - 技术路线：
    > requests
    > bs4
    > re

- **候选数据网站的选择**
  > 新浪股票：http://finance.sina.com.cn/stock/
  > 百度股票：https://gupiao.baidu.com/stock/
  - 选取原则：股票信息静态存在于HTML页面中，非js代码生成,没有Robots协议限制
  - 选取方法：浏览器 F12，源代码查看等
  - 选取心态：不要纠结于某个网站，多找信息源尝试

- **数据网站的确定**
  - 获取股票列表：
    > 东方财富网：http://quote.eastmoney.com/stocklist.html
  - 获取个股信息：
    > 百度股票：https://gupiao.baidu.com/stock/
    > 单个股票：https://gupiao.baidu.com/stock/sz002439.html

- **程序的结构设计**
  > 步骤1：从东方财富网获取股票列表
  > 步骤2：根据股票列表逐个到百度股票获取个股信息
  > 步骤3：将结果存储到文件

- **程序代码（已经优化）**
```python
import requests
from bs4 import BeautifulSoup
import traceback
import re

# 默认设置为utf-8编码，使用apparent_encoding会花费更多时间
def getHTMLText(url, code="utf-8"):
    try:
        r = requests.get(url)
        r.raise_for_status()
        r.encoding = code
        return r.text
    except:
        return ""

def getStockList(lst, stockURL):
    html = getHTMLText(stockURL, "GB2312")
    soup = BeautifulSoup(html, 'html.parser') 
    a = soup.find_all('a')
    for i in a:
        try:
            href = i.attrs['href']
            lst.append(re.findall(r"[s][hz]\d{6}", href)[0])
        except:
            continue

# 增加动态速度
def getStockInfo(lst, stockURL, fpath):
    count = 0
    for stock in lst:
        url = stockURL + stock + ".html"
        html = getHTMLText(url)
        try:
            if html=="":
                continue
            infoDict = {}
            soup = BeautifulSoup(html, 'html.parser')
            stockInfo = soup.find('div',attrs={'class':'stock-bets'})

            name = stockInfo.find_all(attrs={'class':'bets-name'})[0]
            infoDict.update({'股票名称': name.text.split()[0]})
            
            keyList = stockInfo.find_all('dt')
            valueList = stockInfo.find_all('dd')
            for i in range(len(keyList)):
                key = keyList[i].text
                val = valueList[i].text
                infoDict[key] = val
            
            with open(fpath, 'a', encoding='utf-8') as f:
                f.write( str(infoDict) + '\n' )
                count = count + 1
                print("\r当前进度: {:.2f}%".format(count*100/len(lst)),end="")
        except:
            count = count + 1
            print("\r当前进度: {:.2f}%".format(count*100/len(lst)),end="")
            continue

def main():
    stock_list_url = 'http://quote.eastmoney.com/stocklist.html'
    stock_info_url = 'http://gupiao.baidu.com/stock/'
    output_file = 'D:/BaiduStockInfo.txt'
    slist=[]
    getStockList(slist, stock_list_url)
    getStockInfo(slist, stock_info_url, output_file)

main()
```

