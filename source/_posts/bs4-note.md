---
title: BeautifulSoup 解析
date: 2020-04-08 23:05:01
tags:
- BeautifulSoup
- Python
categories:
- Python爬虫
---
***爬虫学习中对bs4库的记录***
<!--more-->
---
## **BeautifulSoup的安装**
```shell
pip install beautifulsoup4
```

## **基本使用**
```python
>>> from bs4 import BeautifulSoup# 注意大小写
>>> soup = BeautifulSoup("<html>data</html>", "html.parser")
>>> soup2 = BeautifulSoup(open("/home/john/celitea/demo.html"), "html.parser")
```

## **基本元素**
|  基本元素   |  说明  |
|  :----: |  :----:  |
|Tag|标签,最基本的信息组成单元 ，分别用<>和</>标明开头和结尾|
|Name|标签的名字，\<p\>...\</p\>的名字是'p'，格式：\<tag\>.name|
|Attributes|标签的属性，字典形式组织，格式：\<tag\>.attrs|
|NavigableString|标签内非属性字符串，<>...</>中字符串，格式：\<tag\>.string，可跨越嵌套的标签层次|
|Comment|标签内字符串的注释部分，一种特殊的Comment类型|

- 代码演示
```python
>>> import requests
>>> r = requests.get("http://python123.io/ws/demo.html")
>>> demo = r.text
>>> demo
'<html><head><title>This is a python demo page</title></head>\r\n<body>\r\n<p class="title"><b>The demo python introduces several python courses.</b></p>\r\n<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:\r\n<a href="http://www.icourse163.org/course/BIT-268001" class="py1" id="link1">Basic Python</a> and <a href="http://www.icourse163.org/course/BIT-1001870001" class="py2" id="link2">Advanced Python</a>.</p>\r\n</body></html>'


# 任何存在与HTML语法中的标签都可以用soup.<tag>访问获得
#当HTML文档中存在多个想用<tag>对应内容时，soup.<tag>返回第一个
>>> from bs4 import BeautifulSoup
>>> soup = BeautifulSoup(demo, "html.parser")
>>> soup.title
<title>This is a python demo page</title>
>>> tag = soup.a
>>> tag
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>


# 每个<tag>都有自己的名字，通过<tag>.name获取，字符串类型
>>> tag = soup.a
>>> tag
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>
>>> soup.a.name
'a'
>>> soup.a.parent.name
'p'
>>> soup.a.parent.parent.name
'body'


# 一个<tag>可以有0或多个属性，字典类型
>>> tag = soup.a
>>> tag.attrs
{'href': 'http://www.icourse163.org/course/BIT-268001', 'class': ['py1'], 'id': 'link1'}
>>> tag.attrs['class']
['py1']
>>> tag.attrs['href']
'http://www.icourse163.org/course/BIT-268001'
>>> type(tag.attrs)
<class 'dict'>
>>> type(tag)
<class 'bs4.element.Tag'>


# NavigableString可以跨越多个层次
>>> soup.a
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>
>>> soup.a.string
'Basic Python'
>>> soup.p
<p class="title"><b>The demo python introduces several python courses.</b></p>
>>> soup.p.string
'The demo python introduces several python courses.'
>>> type(soup.p.string)
<class 'bs4.element.NavigableString'>


# Comment是一种特殊类型
>>> newsoup = BeautifulSoup("<b><!--This is a comment--></b><p>This is not a comment</p>", "html.parser")
>>> newsoup.b.string
'This is a comment'
>>> type(newsoup.b.string)
<class 'bs4.element.Comment'>
>>> newsoup.p.string
'This is not a comment'
>>> type(newsoup.p.string)
<class 'bs4.element.NavigableString'>
```

## **HTML内容遍历**
- **标签树的上层遍历**
    |  属性           |  说明  |
    |  :----:         |  :----:  |
    |.parent|节点的父亲标签|
    |.parents|节点先辈标签的迭代类型，用于循环遍历先辈节点|
    ```python
    >>> soup = BeautifulSoup(demo, "html.parser")
    >>> soup.title.parent
    <head><title>This is a python demo page</title></head>
    >>> soup.html.parent #html的父辈是其本身
    <html><head><title>This is a python demo page</title></head>
    <body>
    <p class="title"><b>The demo python introduces several python courses.</b></p>
    <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>
    </body></html>
    >>> soup.parent #soup本身没有父辈
    >>> 




    # 使用for循环进行上行遍历
    >>> for parent in soup.a.parents:
            if parent is None:
                 print(parent)
            else:
                print(parent.name)
    ```
- **标签树的下层遍历**
    |  属性           |  说明  |
    |  :----:         |  :----:  |
    |.contents|子节点的列表，将\<tag\>所有儿子节点存入列表|
    |.children|子节点的迭代类型，与.content类似，用于循环儿子节点|
    |.descendents|子孙节点的迭代类型， 包含所有子孙节点，用于循环遍历|
    ```python
    >>> soup = BeautifulSoup(demo, "html.parser")
    >>> soup.head
    <head><title>This is a python demo page</title></head>
    >>> soup.head.contents
    [<title>This is a python demo page</title>]
    >>> soup.body.contents
    ['\n', <p class="title"><b>The demo python introduces several python courses.</b></p>, '\n', <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>, '\n']
    >>> len(soup.body.contents)
    5
    >>> soup.body.contents[1]
    <p class="title"><b>The demo python introduces several python courses.</b></p>



    # 使用for循环遍历孩子节点
    >>> for child in soup.body.children:
         print(child)


    # 使用for循环遍历后辈节点
    >>> for child in soup.body.descendants:
         print(child)
    ```
- **标签树的平行遍历**
    |  属性           |  说明  |
    |  :----:         |  :----:  |
    |.next_sibling|返回按照HTML文本顺序的下一个平行节点标签|
    |.previous_sibling|返回按照HTML文本顺序的上一个平行节点标签|
    |.next_sibilings|迭代类型，返回按照HTML文本顺序的后续所有平行节点标签|
    |.previous_siblings|迭代类型，返回按照HTML文本顺序的前续所有平行节点标签|
    ```python
    >>> soup = BeautifulSoup(demo, "html.parser")
    >>> soup.a.next_sibling
    ' and '
    >>> soup.a.next_sibling.next_sibling
    <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>
    >>> soup.a.previous_sibling
    'Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:\r\n'
    >>> soup.a.previous_sibling.previous_sibling
    >>> soup.a.parent
    <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
    <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>


    # 平行遍历后续节点
    >>> for sibling in soup.a.next_sibling:
             print(sibling)

    # 平行遍历前序节点
    >>> for sibling in soup.a.previous_sibling:
             print(sibling)
    ```

## **HTML格式输出**

1. **prettify()方法**
  > prettify()为HTML文本<>及其内容增加'\n'
  > prettify()可用于标签，方法：\<tag\>.prettify()
```python
>>> from bs4 import BeautifulSoup
>>> soup = BeautifulSoup(demo, "html.parser")
>>> soup.prettify()
'<html>\n <head>\n  <title>\n   This is a python demo page\n  </title>\n </head>\n <body>\n  <p class="title">\n   <b>\n    The demo python introduces several python courses.\n   </b>\n  </p>\n  <p class="course">\n   Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:\n   <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">\n    Basic Python\n   </a>\n   and\n   <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">\n    Advanced Python\n   </a>\n   .\n  </p>\n </body>\n</html>'


>>> print(soup.prettify())
<html>
 <head>
  <title>
   This is a python demo page
  </title>
 </head>
 <body>
  <p class="title">
   <b>
    The demo python introduces several python courses.
   </b>
  </p>
  <p class="course">
   Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
   <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">
    Basic Python
   </a>
   and
   <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">
    Advanced Python
   </a>
   .
  </p>
 </body>
</html>
```

2. **编码**
  > bs4库将任何HTML输入都变成utf-8编码
  > Python 3.x默认支持编码时utf-8，解析无障碍
```python
>>> soup = BeautifulSoup("<p>中文</p>", "html.parser")
>>> soup.p.string
'中文'
>>> print(soup.p.prettify())
<p>
 中文
</p>
```


