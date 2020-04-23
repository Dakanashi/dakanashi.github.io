---
title: 信息标记笔记
date: 2020-04-10 20:54:42
tags:
- Crawler
- Python
categories:
- Python爬虫
---
***学习爬虫途中所学信息标记相关知识的笔记。***
<!--more-->
---
## **HTML的信息标记**
  > HTML时WWW（World Wide Web）的信息组织方式，由声音、图像、视频组成超文本。
  > HTML通过预定义的<>...</>标签形式组织不同类型的信息。
  > 有三种主要形式——XML、JSON和YAML。
- **XML（extensible markup language）**
    ```xml
    <!--类似html-->
    <img src="china.jpg" size="10">...</img>
    ```

- **JSON（JavaScript Obejt Notation）**
    ```json
    # Json本身没有注释
    # 键值对key:value
    "name" : "John"

    # 多值用[,]组织
    "name" : ["John", "Dakanashi"]

    # 键值对嵌套用{,}
    "name" : {
            "newName" : "John",
            "oldname" : "Dakanashi"
    }
    ```
- **YAML（YAML Ain't Markup Language）**
    ```yaml
    # 无类型键值对key:value
    name : John

    # 通过缩进形式表达所属关系
    name : 
        newName : John
        oldName : Dakanashi
         
    # 使用‘-’表达并列关系
    name : 
    - John
    - Dakanashi

    # 使用‘|’表达整块数据，使用‘#’表示注释
    text: |
    ajsldjaskldjsakldjaskldjaskldjakldjsakldjakldjlk
    ```

## **三种信息标记形式的比较**
|信息标记|  自身特点 |  应用特点  |
| :----: |  :----:   |  :----:  |
|XML|最早的通用信息标记语言，可扩展性好，但繁琐|Internet上的信息交互与传递|
|JSON|信息有类型，适合程序处理（js），较XML简洁|移动应用云端和节点的信息用心，无注释|
|YAML|信息无类型，文本信息比例最高，可读性好|各类系统的配置文件，有注释易读|

## **信息提取的一般方法**
1. **形式解析：完整解析信息的标记个事，再提取相关信息（需要标记解析器，例如bs4库的标签树遍历）。**
    > 优点：信息解析准确
    > 缺点：提取过程繁琐，速度慢

2. **搜索：无视标记形式，直接搜索关键信息，对信息的文本查找函数即可。**
    > 优点：提取过程简洁，速度较快。
    > 缺点：提取结果准确性与信息内容相关。

3. **融合方法：结合形式解析与搜索方法， 提取关键信息，需要标记解析器及文本查找函数。**
```python
# 提取HTML中所有URL链接
# 思路：  1）搜索到所有<a>标签
#         2）解析<a>标签格式，提取href后的链接内容
>>> from bs4 import BeautifulSoup
>>> import requests
>>> r = requests.get("http://python123.io/ws/demo.html")
>>> demo = r.text
>>> soup = BeautifulSoup(demo, "html.parser")
>>> for link in soup.find_all('a'):
...     print(link.get('href'))
... 
http://www.icourse163.org/course/BIT-268001
http://www.icourse163.org/course/BIT-1001870001
```

## **基于bs4库的HTML内容查找方法**
- **<>.find_all(name, attrs=None, recursive=True, string=None, \*\*kwargs)**
  > name：对标签名称的检索字符串。
  > attrs：对标签属性值的检索字符串，可标注属性检索。
  > recursive：是否对子孙全部进行检索，默认为True。
  > string：<>...</>中字符串区域的检索。
    ```python
    # 查找含a标签的部分
    >>> soup.find_all('a')
    [<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>, <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]

    # 查找a，b两个标签的部分
    >>> soup.find_all(['a', 'b'])
    [<b>The demo python introduces several python courses.</b>, <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>, <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]

    # 输出所有标签
    >>> for tag in soup.find_all(True):
    ...     print(tag.name)
    ... 
    html
    head
    title
    body
    p
    b
    p
    a
    a

    # 导入正则表达式库，查找名字中带b的标签
    >>> import re
    >>> for tag in soup.find_all(re.compile('b')):
    ...     print(tag.name)
    ... 
    body
    b

    # 查询含有course属性的p标签
    >>> soup.find_all('p', 'course')
    [<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>]

    # 寻找所有标签中含有id为link1的标签
    >>> soup.find_all(id='link1')
    [<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>]

    # 查询的link必须是完全匹配的才可以
    >>> soup.find_all(id='link')
    []

    # 使用正则表达式查询包含link的属性
    >>> soup.find_all(id=re.compile('link'))
    [<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>, <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]

    # 查找所有标签a
    >>> soup.find_all('a')
    [<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>, <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>]
    # recursive被设置为False，没有子孙节点被搜索，结果为空
    >>> soup.find_all('a', recursive=False)
    []

    >>> soup
    <html><head><title>This is a python demo page</title></head>
    <body>
    <p class="title"><b>The demo python introduces several python courses.</b></p>
    <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>
    </body></html>

    #查找Basic Python字符串
    >>> soup.find_all(string='Basic Python')
    ['Basic Python']

    # 使用正则表达式查找包含python的字符串
    >>> soup.find_all(string=re.compile('python'))
    ['This is a python demo page', 'The demo python introduces several python courses.']
    ```

- **备注**
  > \<tag\>(..)等价于\<tag\>.find_all(..)
  > soup.(..)等价于soup.find_all(..)
  > <>.find()方法使用与<>.find_all()方法略有不同，搜索只返回一个为字符串类型的结果，参数和find_all()相同。
