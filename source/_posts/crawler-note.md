---
title: Python爬虫笔记
date: 2020-04-04 18:28:46
tags:
- Crawler
- Python
categories:
- Python爬虫
---
***对于爬虫的学习记录***
<!--more-->
---
# ***相关知识说明***
- **爬虫规模说明**
  |  方法           |  说明  |用途|
  |  :----:           |  :----:  |:----:|
  |小规模|数据量小，爬取数据不敏感，Requests库|爬取网页，玩转网页|
  |中规模|数据规模较大，爬取速度敏感，Scrapy库|爬取网站，爬取系列网站|
  |大规模|搜索引擎，爬取速度关键|爬取全网|

- **网络爬虫的限制**
  - 来源审查：判断User-Agent进行限制
    > 检查来访HTTP协议头的User-Agent域，只响应浏览器或友好爬虫的访问。
  - 发布公告：Robots协议
    > 告知所有爬虫网站的爬取策略，要求爬虫遵守。

- **Robots协议**
  - 作用：网站告知网络爬虫哪些页面可以抓取，那些不行。
  - 形式：在网站根目录下的robots.txt文件夹。

# ***Requests库网络爬虫实例***
1. **京东商品页面的爬取**
    ```python
    import requests
    url = "https://item.jd.com/2967929.html"
    try:
        r = requests.get(url)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        print(r.text[:1000])
    except:
        print("爬取失败")
    ```

2. 亚马逊商品页面的爬取
    ```python
    import requests
    url = "https://www.amazon.cn/gp/product/B01M8L5Z3Y"
    try:
        kv = {'usr-agent':'Mozilla/5.0'}# 不更改headers会遭到拒绝访问
        r = requests.get(url, headers=kv)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        print(r.text[1000:2000])
    except:
        print("爬取失败")
    ```

3. 百度/360搜索相关字提交
    ```Python
    import requests
    keyword = "Python"
    try:
        kv= {'wd': keyword}# 设置关键词
        # 将关键词传入，360搜索将url换为http://www.so.com/s
        r = requests.get("http://www.baidu.com/s", params=kv)
        print(r.request.url)
        r.raise_for_status()
        print(lem(r.text))# 打印搜索到的条目数量
    except:
        print("爬取失败")
    ```

4. 网络图片的爬取和存储
    ```Python
    import requests
    import os
    url = "http://image.ngchina.com.cn/2019/0523/20190523103156143.jpg"
    root = "/home/john/celitea"
    path = root + url.split('/')[-1]
    try:
        if not os.path.exists(root):
            os.mkdir(root)
        if not os.path.exists(path):
            r = requests.get(url)
            with open(path, 'wb') as f:
                f.write(r.content)
                f.close()
                print("文件保存成功")
        else:
            print("文件已存在")
    except：
        print("爬取失败")
    ``` 

5. IP地址归属地的自动查询
    ```Python
    # 此方法已不适用，仅供参考
    import requests
    url = "http://m.ip138.com/ip.asp?ip="
    try:
        r = requests.get(url+'104.36.66.224')
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        print(r.text[-500:])
    except：
        print("爬取失败")
    ```
