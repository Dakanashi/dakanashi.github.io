---
title: 正则表达式Re库
date: 2020-04-13 17:48:44
tags:
- Crawler
- Re
- Python
categories:
- Python爬虫
- 正则表达式
---
***Re库笔记。***
<!--more-->
---
## **正则表达式（regular expression regex）**
- **概念**
  > 通用的字符串表达框架
  > 简介表达一组字符串的表达式
  > 针对字符串表达“简洁”和“特征”思想的工具
  > 判断某字符串的特征归属

- **作用**
  > 表达文本类型的特征（病毒、入侵等）
  > 同事查找或替换一组字符串
  > 匹配字符串的全部或部分
  
- **使用**
  > 编译：将符合正则表达式语法的字符串转换成正则表达式特征。

## **正则表达式语法**
|操作符|说明|实例|
|:----:|:----:|:----:|
|.|表示任何单个字符，不能匹配换行符||
|[ ]|字符集，对单个字符给出取值范围，不包含空集|[abc]表示a、b、c，[a-z]表示a到z单个字符|
|[^ ]|非字符集，对单个字符给出取值范围|\[^abc]表示非a或b或c的单个字符|
|*|前一个字符0次或无限次扩展，加在后面|abc\*表示ab、abc、abcc、abccc等|
|+|前一个字符1次或无限次扩展，加在后面|abc+表示abc、abcc、abccc等|
|?|前一个字符0次或1次扩展，加在后面|abc?表示ab、abc|
|\||左右表达式任意一个，包含空集|abc\|def表示abc、def|
|{m}|扩展前一个字符m次|as{2}c表示abbc|
|{m,n}|扩展前一个字符m至n次|ab{1,2}c表示abc、abbc|
|^|匹配字符串开头，加在前面|^abc表示abc且在一个字符串的开头|
|$|匹配字符串结尾，加在后面|abc$表示abc且在一个字符串的结尾|
|( )|分组标记，内部只能使用 \| 操作符|(abc)表示abc，(abc\|def)表示abc、def|
|\\d|数字，等价于[0-9]||
|\\w|单词字符，等价于[A-Za-z0-9_]||

- **实例**

  | 正则表达式 | 对应字符串 |  
  |:----:|:----:|
  |P(Y\|YT\|YTH\|YTHO)N|'PN'、'PYN'、'PYTN'、'PYTHN'、'PYTHON'|
  |PYTHON+|'PYTHON'、'PYTHONN'、'PYTHONNN'...|
  |PY[TH]ON|'PYTON'、'PYHON'|
  |PY[^TH]?ON|'PYON'、'PYaON'、'PYbON'、'PYcON'...|
  |PY{:3}N|'PN'、'PYN'、'PYYN'、'PYYYN'|
  |^\[A‐-Za-z]+$|由26个字母组成的字符串|
  |^\[A‐-Za-z0-9]+$|由26个字母和数字组成的字符串|
  |^‐?\d+$|整数形式的字符串|
  |^\[0-9]\*[1-9][0-9]*$|正整数形式的字符串|
  |[1-9]\d{5}|中国境内邮政编码，6位|
  |[\u4e00-\u9fa5]|匹配中文字符|
  |\d{3}-\d{8}\|\d{4}-\d{7}|国内电话号码，010‐68913536|

- **IP地址匹配**
  1. IP地址字符串形式的正则表达式（IP地址分4段，每段0‐255）
  2. 精确写法
        > 0-99： [1-9]?\d
        > 100‐199: 1\d{2}
        > 200‐249: 2[0-4]\d
        > 250‐255: 25[0-5]
  3. 答案
        > (([1-9]?\d|1\d{2}|2[0-4]\d|25[0-5]).){3}([1-9]?\d|1\d{2}|2[0-4]\d|25[0-5])

## **Re库的基本使用**
- **正则表达式表示类型**
    > raw string类型(原生字符串类型 r'text')。
    > string类型，更繁琐。
    > 当正则表达式包含转义字符时，使用raw string。

- **Re库的主要功能函数**

  |函数|说明|
  |:----:|:----:|
  |re.search()|在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象|
  |re.match()|从一个字符串的开始位置起匹配正则表达式，返回match对象|
  |re.findall()|搜索字符串，以列表类型返回全部能匹配的子串|
  |re.split()|将一个字符串按照正则表达式匹配结果进行分割，返回列表类型|
  |re.finditer()|搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象|
  |re.sub()|在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串|

1. **re.search(pattern, string, flags=0)**
   - 在一个字符串中搜索匹配正则表达式的第一个位置返回match对象。

    > pattern : 正则表达式的字符串或原生字符串表示
    > string : 待匹配字符串
    > flags : 正则表达式使用时的控制标记
    ```python
    >>> import re
    >>> match = re.search(r'[]1-9]\d{5}', 'BIT 100081')
    >>> if match:
    ...     print(match.group(0))
    ... 
    100081
    ```

2. **re.match(pattern, string, flags=0)**
    - 从一个字符串的开始位置起匹配正则表达式返回match对象。
    > pattern : 正则表达式的字符串或原生字符串表示
    > string : 待匹配字符串
    > flags : 正则表达式使用时的控制标记
    ```python
    >>> import re
    >>> match = re.match(r'[]1-9]\d{5}', 'BIT 100081')
    >>> if match:
    ...     print(match.group(0))
    ... 
    >>> match.group(0)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'NoneType' object has no attribute 'group'
    
     
    >>> match = re.match(r'[]1-9]\d{5}', '100081 BIT')
    >>> if match:
    ...     print(match.group(0))
    ... 
    100081
    ```

3. **re.findall(pattern, string, flags=0)**
    - 搜索字符串，以列表类型返回全部能匹配的子串。
    > pattern : 正则表达式的字符串或原生字符串表示
    > string : 待匹配字符串
    > flags : 正则表达式使用时的控制标记
    ```python
    >>> import re
    >>> ls = re.findall(r'[1-9]\d{5}', 'BIT100081 TSU100084')
    >>> ls
    ['100081', '100084']
    ```

4. **re.split(pattern, string, maxsplit=0, flags=0)**
    - 将一个字符串按照正则表达式匹配结果进行分割返回列表类型，去除匹配的部分。
    > pattern : 正则表达式的字符串或原生字符串表示
    > string : 待匹配字符串
    > maxsplit: 最大分割数，剩余部分作为最后一个元素输出，不进行匹配
    > flags : 正则表达式使用时的控制标记
    ```python
    >>> import re
    >>> re.split(r'[1-9]\d{5}', 'BIT100081 TSU100084')
    ['BIT', ' TSU', '']
    >>> re.split(r'[1-9]\d{5}', 'BIT100081 TSU100084', maxsplit=1)
    ['BIT', ' TSU100084']
    ```

5. **re.finditer(pattern, string, flags=0)**
    - 搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象。
    > pattern : 正则表达式的字符串或原生字符串表示
    > string : 待匹配字符串
    > flags : 正则表达式使用时的控制标记
    ```python
    >>> import re
    >>> for m in re.finditer(r'[1-9]\d{5}', 'BIT100081 TSU100084'):
    ...     if m:
    ...             print(m.group(0))
    ... 
    100081
    100084
    ```

6. **re.sub(pattern, repl, string, count=0, flags=0)**
    - 在一个字符串中替换所有匹配正则表达式的子串返回替换后的字符串。
    > pattern : 正则表达式的字符串或原生字符串表示
    > repl : 替换匹配字符串的字符串
    > string : 待匹配字符串
    > count : 匹配的最大替换次数
    > flags : 正则表达式使用时的控制标记
    ```python
    >>> import re
    >>> re.sub(r'[1-9]\d{5}', ':zipcode', 'BIT100081 TSU100084')
    'BIT:zipcode TSU:zipcode'
    ```

- **flags常用控制标记**

  |常用标记|说明|
  |:----|:----|
  |re.I　re.IGNORECASE|忽略正则表达式中的大小写，[A-Z]能够匹配小写字符|
  |re.M　re.MULTILINE|正则表达式中的^操作符能够将给定字符串的每行当作匹配开始|
  |re.S　re.DOTALL|正则表达式中的.操作符能够匹配所有字符，'.'默认匹配除换行外的所有字符|

- **Re库的另一种等价用法**
  - regex = re.compile(pattern, flags=0)
  > pattern : 正则表达式的字符串或原生字符串表示
  > flags : 正则表达式使用时的控制标记
  ```python
  # 函数式用法：一次性操作
  >>> rst = re.search(r'[1‐9]\d{5}', 'BIT 100081')

  # 面向对象用法：编译后的多次操作
  >>> pat = re.compile(r'[1‐9]\d{5}')
  >>> rst = pat.search('BIT 100081')
  ```

  |函数|说明|
  |:----:|:----:|
  |regex.search()|在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象|
  |regex.match()|从一个字符串的开始位置起匹配正则表达式，返回match对象|
  |regex.findall()|搜索字符串，以列表类型返回全部能匹配的子串|
  |regex.split()|将一个字符串按照正则表达式匹配结果进行分割，返回列表类型|
  |regex.finditer()|搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象|
  |regex.sub()|在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串|

## **Re库的match对象**
- **match对象的属性**

  |属性|说明|
  |:----:|:----:|
  |.string|待匹配的文本|
  |.re|匹配时使用的patter对象（正则表达式）|
  |.pos|正则表达式搜索文本的开始位置|
  |.endpos|正则表达式搜索文本的 结束位置|

- **match对象的方法**
  
  |方法|说明|
  |:----:|:----:|
  |.group(0)|获得匹配后的字符串|
  |.start()|匹配字符串在原始字符串的开始位置|
  |.end()|匹配字符串在原始字符串的结束位置|
  |.span()|返回(.start(), .end())|

- **实例**
  ```python
  >>> import re
  >>> m = re.search(r'[]1-9]\d{5}', 'BIT 100081 TSU 100084')
  >>> m.string
  'BIT 100081 TSU 100084'

  >>> m.re
  re.compile('[]1-9]\\d{5}')

  >>> m.pos
  0

  >>> m.endpos
  21

  >>> m.group(0)
  '100081'

  # 只保留一次结果
  >>> m.group(1)
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  IndexError: no such group

  # 第一次陪陪成功开始位置
  >>> m.start()
  4

  # 第一次陪陪成功结束的后一个位置
  >>> m.end()
  10

  >>> m.span()
  (4, 10)

  ```

## **Re库的贪婪匹配和最小匹配**
- **贪婪匹配**
  - Re库默认采用贪婪匹配，即输出匹配最长的子串。
    ```python
    >>> match = re.search(r'PY.*N', 'PYANBNCNDN')
    >>> match.group(0)
    'PYANBNCNDN
    ```
  - 最小匹配
    ```python
    # 多加了一个'?'
    >>> match = re.search(r'PY.*?N', 'PYANBNCNDN')
    >>> match.group(0)
    'PYAN'
    ```
  - 最小匹配操作符
    |操作符|说明|
    |:----:|:----:|
    |*?|前一个字符0次或无限次扩展，最小匹配|
    |+?|前一个字符1次或无限次扩展，最小匹配|
    |??|前一个字符0次或1次扩展，最小匹配|
    |{m,n}?|扩展前一个字符m至n次（含n），最小匹配|
