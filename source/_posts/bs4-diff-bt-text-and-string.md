---
title: Python BeautifulSoup 中.text与.string的区别
date: 2020-04-18 16:24:34
tags:
- BeautifulSoup
- Python
categories:
- Python爬虫
---
***在学习编写爬虫的过程中对于bs4库使用中的小记***
<!--more-->
---
- **二者不同之处**
    > .string对于Tag属性返回一个NavigableString变量，不检索子标签。
    > .text获得所有子子标签字符串并返回使用给定的分隔符连接，.text的返回unicode变量。

- **举例**
    ```html
    <td>some text</td> 
    <td></td>
    <td><p>more text</p></td>
    <td>even <p>more text</p></td>
    ```
    ```pyhton
    # .string返回None
    some text
    None
    more text
    None


    # .text返回一个空字符串
    some text

    more text
    even more text 
    ```

- **总结**
    - 第一行，在指定标签td，没有子标签，且有文本时，两者的返回结果一致，都是文本
    - 第二行，在指定标签td，没有子标签，且没有文本时，.string返回None，.text返回为空
    - 第三行，在指定标签td，只有一个子标签时，且文本只出现在子标签之间时，两者返回结果一致，都返回子标签内的文本
    - 第四行，最关键的区别，在指定标签td，有子标签，并且父标签td和子标签p各自包含一段文本时，两者的返回结果，存在很大的差异:
        > .string返回为空，因为文本数>=2，string不知道获取哪一个
        > .text返回的是，两段文本的拼接。


