---
title: Python中的Scrapy库
date: 2020-04-15 15:49:52
tags:
- Crawler
- Scrapy
- Python
categories:
- Python爬虫
---
 ***本笔记内容巨多，请耐心学习***
<!--more-->
---
## **Scrapy介绍**
- **简介**
    > Scrapy不是一个函数功能库，而是一个爬虫框架。
    > 爬虫框架是实现爬虫功能的一个软件结构和功能组件集合。
    > 爬虫框架是一个半成品，能够帮助用户实现专业网络爬虫。

- **框架结构**

![scrapy 框架](/images/scrapy.png)
> Spider模块（用户配置）
> Engine模块（已有实现）
> Item Pipline模块（用户配置）
> Downloader模块（已有实现）
> Scheduler模块（已有实现）

- **数据流的三个路径**
    1. **路径1**
        ![scrapy 路径1](/images/scrapy_1.png)
        > 1 Engine从Spider处获得爬取请求(Request)
        > 2 Engine将爬取请求转发给Scheduler，用于调度
    2. **路径2**
        ![scrapy 路径2](/images/scrapy_2.png)
        > 3 Engine从Scheduler处获得下一个要爬取的请求
        > 4 Engine将爬取请求通过中间件发送给Downloader
        > 5 爬取网页后，Downloader形成响应（Response）,通过中间件发给Engine
        > 6 Engine将收到的响应通过中间件发送给Spider处理
    3. **路径3**
        ![scrapy 路径3](/images/scrapy_3.png)
        > 7 Spider处理响应后产生爬取项（scraped Item）和新的爬取请求（Requests）给Engine
        > 8 Engine将爬取项发送给Item Pipeline（框架出口）
        > 9 Engine将爬取请求发送给Scheduler

## **Scrapy框架解析**
- **Engine**
    > (1) 控制所有模块之间的数据流
    > (2) 根据条件触发事件
    > 不需要用户修改

- **Downloader**
    > 根据请求下载网页
    > 不需要用户修改

- **Scheduler**
    > 对所有爬取请求进行调度管理
    > 不需要用户修改

- **Downloader Middleware**
    > 目的：实施Engine、Scheduler和Downloader之间进行用户可配置的控制
    > 功能：修改、丢弃、新增请求或响应
    > 用户可以编写配置代码,如果不对request和response进行修改就不需要编写

- **Spider**
    > (1) 解析Downloader返回的响应（Response）
    > (2) 产生爬取项（scraped item）
    > (3) 产生额外的爬取请求（Request）
    > 需要用户编写配置代码

- **Item Pipelines**
    > (1) 以流水线方式处理Spider产生的爬取项
    > (2) 由一组操作顺序组成，类似流水线，每个操作是一个Item Pipeline类型
    > (3) 可能操作包括：清理、检验和查重爬取项中的HTML数据、将数据存储到数据库
    > 需要用户编写配置代码

- **Spider Middleware**
    > 目的：对请求和爬取项的再处理
    > 功能：修改、丢弃、新增请求或爬取项
    > 用户可以编写配置代码

## **requests库和Scrapy爬虫的比较**
- **相同点**
    > 两者都可以进行页面请求和爬取，Python爬虫的两个重要技术路线
    > 两者可用性都好，文档丰富，入门简单
    > 两者都没有处理js、提交表单、应对验证码等功能（可扩展）

- **不同点**
    |requests|Scrapy|
    |:----:|:----:|
    |页面级爬虫|网站级爬虫|
    |功能库|框架|
    |并发性考虑不足，性能较差|并发性好，性能较高|
    |重点在于页面下载|重点在于爬虫结构|
    |定制灵活|一般定制灵活，深度定制困难|
    |上手十分简单|入门稍难|

- **看情况挑选**
    > 非常小的需求，requests库
    > 不太小的需求，Scrapy框架
    > 定制程度很高的需求（不考虑规模），自搭框架，requests > Scrapy

## **Scrapy爬虫的常用命令**
- **格式**
    > scrapy <command> [options] [args]

- **常用命令**
    |命令|说明|格式|
    |:----:|:----:|:----:|
    |startproject|创建一个新工程|scrapy startproject \<name\> [dir]|
    |genspider|创建一个爬虫|scrapy genspider [options] \<name\> \<domain\>|
    |settings|获得爬虫配置信息|scrapy settings [options]|
    |crawl|运行一个爬虫|scrapy crawl \<spider\>|
    |list|列出工程中所有爬虫|scrapy list|
    |shell|启动URL调试命令行|scrapy shell [url]|

- **简单实例**
    - **步骤1**：建立一个Scrapy爬虫工程
        > scrapy startporject python123demo
    - 生成的目录结构
        ```
        python123demo/                  外层目录
            scrapy.cfg                  部署Scrapy爬虫的配置文件
            python123demo/              Scrapy框架的用户自定义Python代码
                __init__.py             初始化脚本
                items.py                Items代码模板（继承类）
                middlewares.py          Middlewares代码模板（继承类）
                pipelines.py            Pipelines代码模板（继承类）
                settings.py             Scrapy爬虫的配置文件
                __pycache__/            缓存目录，无需修改
                spiders/                Spiders代码模板目录（继承类）
                    __init__.py         初始文件，无需修改
                    __pycache__/        缓存目录，无需修改
        ```
    
    - **步骤2**：在工程中产生一个Scrapy爬虫（也可以手动创建）
        > scrapy genspider demo python123.io
        >> 作用：
        >>> (1) 生成一个名称为demo的spider
        >>> (2) 在spiders目录下增加代码文件 demo.py

    - **步骤3**：配置产生的spider爬虫
        > （1）初始URL地址
        > （2）获取页面后的解析方式

    - 配置完成的 demo.py

        ```python
        # 演示HTML页面地址：http://python123.io/ws/demo.html
        # 文件名称：demo.html

        # -*- coding: utf-8 -*-
        import scrapy
        class DemoSpider(scrapy.Spider):
            name = "demo"

            # 对原来的demo.py进行了优化
            def start_requests(self):
                urls = ['http://python123.io/ws/demo.html']
                #allowed_domains = ["python123.io"]
                for url in urls:
                    # 原本程序中不用写Requests方法
                    yield scrapy.Request(url=url, callback=self.parse)

            # parse()用于处理响应，解析内容形成字典，发现新的URL爬取请求
            def parse(self, response):
                fname = response.url.split('/')[-1]
                with open(fname, 'wb') as f:
                    f.write(response.body)
                self.log('Saved file %s.' % name)
        ```

    - **步骤4**：运行爬虫，获取网页,demo爬虫被执行，捕获页面存储在demo.html
        > scrapy crawl demo

## **Scrapy爬虫的基本使用**
- **使用步骤**
    > 步骤1：创建一个工程和Spider模板
    > 步骤2：编写Spider
    > 步骤3：编写Item Pipeline
    > 步骤4：优化配置策略

- **数据类型**
    - **Request类**
        > scrapy.http.Request()
        > Request对象表示一个HTTP请求
        > 由Spider生成，由Downloader执行

        |属性或方法|说明|
        |:---:|:---:|
        |.url |Request对应的请求URL地址|
        |.method|对应的请求方法，'GET' 'POST'等|
        |.headers|字典类型风格的请求头|
        |.body|请求内容主体，字符串类型|
        |.meta|用户添加的扩展信息，在Scrapy内部模块间传递信息使用|
        |.copy()|复制该请求|

    - **Response类**
        > scrapy.http.Response()
        > Response对象表示一个HTTP响应
        > 由Downloader生成，由Spider处理

        |属性或方法|说明|
        |:---:|:---:|
        |.url |Response对应的URL地址|
        |.status |HTTP状态码，默认是200|
        |.headers |Response对应的头部信息|
        |.body |Response对应的内容信息，字符串类型|
        |.flags |一组标记|
        |.request |产生Response类型对应的Request对象|
        |.copy() |复制该响应|

    - **Item类**
        > scrapy.item.Item()
        > Item对象表示一个从HTML页面中提取的信息内容
        > 由Spider生成，由Item Pipeline处理
        > Item类似字典类型，可以按照字典类型操作

    - **Scrapy爬虫信息的提取方法**
        > Scrapy爬虫支持多种HTML信息提取方法：
        >> Beautiful Soup
        >> lxml
        >> re
        >> XPath Selector
        >> CSS Selector

    - **补充：CSS Selector的基本使用**
        > \<HTML\>.css('a::attr(href)').extract()
        > CSS Selector由W3C组织维护并规范

## **股票数据Scrapy爬虫实例**
- **功能描述**
    > 目标：获取上交所和深交所所有股票的名称和交易信息
    > 输出：保存到文件中
    > 技术路线：scrapy

- **相关数据网站**
    > 东方财富网：http://quote.eastmoney.com/stocklist.html
    > 百度股票：https://gupiao.baidu.com/stock/
    > 单个股票：https://gupiao.baidu.com/stock/sz002439.html

- **程序编写步骤**
    - **步骤1**：建立工程和Spider模板
        > \>scrapy startproject BaiduStocks
        > \>cd BaiduStocks
        > \>scrapy genspider stocks baidu.com
        > 进一步修改spiders/stocks.py文件

    - **步骤2**：编写Spider
        > 配置stocks.py文件
        > 修改对返回页面的处理
        > 修改对新增URL爬取请求的处理
        ```python
        # 更改前的初始代码
        -*- coding: utf-8 -*-
        import scrapy

        class DtocksSpider(scrapy.Spider):
            name = 'stocks'
            allowed_domains = ['baidu.com']
            start_urls = ['http://baidu.com/']

            def parse(self, response):
                pass


        # 更改后的代码
        # -*- coding: utf-8 -*-
        import scrapy
        import re


        class StocksSpider(scrapy.Spider):
            name = "stocks"
            start_urls = ['http://quote.eastmoney.com/stocklist.html']

            def parse(self, response):
                for href in response.css('a::attr(href)').extract():
                    try:
                        stock = re.findall(r"[s][hz]\d{6}", href)[0]
                        url = 'http://gupiao.baidu.com/stock/' + stock + '.html'
                        yield scrapy.Request(url, callback=self.parse_stock)
                    except:
                        continue

            def parse_stock(self, response):
                infoDict = {}
                stockInfo = response.css('.stock-bets')
                name = stockInfo.css('.bets-name').extract()[0]
                keyList = stockInfo.css('dt').extract()
                valueList = stockInfo.css('dd').extract()
                for i in range(len(keyList)):
                    key = re.findall(r'>.*</dt>', keyList[i])[0][1:-5]
                    try:
                        val = re.findall(r'\d+\.?.*</dd>', valueList[i])[0][0:-5]
                    except:
                        val = '--'
                    infoDict[key]=val

                infoDict.update(
                    {'股票名称': re.findall('\s.*\(',name)[0].split()[0] + \
                    re.findall('\>.*\<', name)[0][1:-1]})

                # 发送给piplines
                yield infoDict

        ```

    - **步骤3**：编写ITEM Pipelines
        > 配置pipelines.py文件
        > 定义对爬取项（Scraped Item）的处理类
        > 配置ITEM_PIPELINES选项(setting.py)
        ```python
        GNU nano 4.9.2                      pipelines.py                              
        # -*- coding: utf-8 -*-

        # Define your item pipelines here
        #
        # Don't forget to add your pipeline to the ITEM_PIPELINES setting
        # See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html


        class BaidustocksPipeline(object):
            def process_item(self, item, spider):
                return item

        # 新增的pipline
        class BaidustocksInfoPipeline(object):
            def open_spider(self, spider):
                self.f = open('BaidustocksInfo.txt', 'w')

            def close_spider(self, spider):
                self.f.close()
            
            def process_item(self, item, spider):
                try:
                    line = str(dict(item)) + "\n"
                    self.f.write(line)
                except:
                    pass
                return item
        ```

        ```python
        # setting.py
        # Configure item pipelines
        # See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
        ITEM_PIPELINES = {
            'BaiduStocks.pipelines.BaidustocksInfoPipeline': 300,
        }

        ```

    - **步骤4**：程序的执行
        > \>scrapy crawl stocks

- **优化：在setting.py文件中配置并发链接选项**
    |选项|说明|
    |:---:|:---:|
    |CONCURRENT_REQUESTS |Downloader最大并发请求下载数量，默认32|
    |CONCURRENT_ITEMS|Item Pipeline最大并发ITEM处理数量，默认100|
    |CONCURRENT_REQUESTS_PER_DOMAIN|每个目标域名最大的并发请求数量，默认8|
    |CONCURRENT_REQUESTS_PER_IP|每个目标IP最大的并发请求数量，默认0，非0有效|
    