---
title: request库小记
date: 2020-04-04 17:59:07
tags:
- Crawler
- Python
categories:
- Python爬虫
---
***学习爬虫之前先记录一下request库***
<!--more-->
---
- **HTTP协议**
  > HTTP，Hypertext Transfer Protocol，超文本传输协议。
  > HTTP是一个基于“请求与响应”模式的、无状态的应用层协议。
  > HTTP协议采用URL作为定位网络资源的标识。

- **Request库的7个主要方法**

  |  方法           |  说明  |
  |  ----           |  ----  |
  |requests.request()|构造一个请求，支撑一下个方法的基础方法|
  |requests.get()|获取HTML网页的主要方法，对应于HTTP的GET|
  |requests.head()|获取HTML网页头信息的方法，对应于HTTP的HEAD|
  |requests.post()|向HTML网页提交POST请求的方法，对应于HTTP的POST|
  |requests.put()|向HTML网页提交PUT请求的方法，对应于HTTP的PUT|
  |requests.patch()|向HTML网页提交局部修改请求，对应于HTTP的PATCH|
  |requests.delete()|向HTML网页提交删除请求，对应于HTTP的DELETE|

- **requests.request(method, url, \*\*kwargs)**
  > method：上述说明的7种方法。
  > url：拟获取页面的url链接。
  > **kwargs：13个控制访问的参数，均为可选项。

- **\*\*kwargs详解**
  1. params：字典或字节序列，作为可选参数增加到url中。
        ```python
        >>> import requests
        >>> kv = {'key1':'value1', 'key2':'value2'}
        >>> r = requests.request('GET', 'http://python123.io/ws', params=kv)
        >>> print(r.url)
        https://python123.io/ws?key1=value1&key2=value2
        ```
  2. data：字典、字节序列或文件对象，作为Requests的数据进行存储。
        ```python
        >>> kv = {'key1':'value1', 'key2':'value2'}
        >>> r = requests.request('POST', 'http://python123.io/ws', data=kv)
        >>> body = '主体内容'
        >>> r = requests.request('POST', 'http://python123.io/ws', data=body.encode('utf-8'))
        ```
  3. json：JSON格式的数据，作为Requests的数据。
        ```python
        >>> kv = {'key1':'value1'}
        >>> r = requests.request('POST', 'http://python123.io/ws', json=kv)
        ```
  4. headers：字典，HTTP头定制。
        ```python
        >>> hd = {'user-agent':'Chrome/10'}#模拟Chrome浏览器第10个版本
        >>> r = requests.request('POST', 'http://python123.io/ws', headers=hd)
        ```
  5. cookies：字典或CookieJar，Request中的cookie。
  6. auth：元组，支持HTTP认证功能。
  7. files：字典类型，传输文件。
        ```python
        >>> fs = {'file': open('data.xls', 'rb')}
        >>> r = requests.request('POST', 'http://python123.io/ws', files=fs)
        ```
  8. timeout：设定超时时间，秒为单位。
       ```python
        >>> r = requests.request('POST', 'http://www.baidu.com', timeout=10)#在timeout时间内请求内容未返回则引起timeout异常
        ```
  9.  proxies：字典类型，设定访问代理服务器，可以增加登录认证。
        ```python
        #可以防止爬虫爬去信息
        >>> pxs = {'http': 'http://user:pass@10.10.10.1:1234', 'https': 'https://pass@10.10.10.1:4321'}
        >>> r = requests.request('GET', 'http://www.baidu.com', proxies=pxs)
        ```
  10. allow_redirects：True/False，默认为True，重定向开关。
  11. stream：True/False，默认为True，获取内容立即下载开关。
  12. verify：True/False，默认为True，认证SSL证书开关。
  13. cert：本地SSL证书路径。

- **requests.get(url, params=None, \*\*kwargs)**
  > url：拟获取页面的url链接。
  > params：url中的额外参数，字典或字节流格式，可选。
  > **kwargs：剩余12个可选的控制访问的参数。
  > 返回值是一个Response对象。

- **requests.head(url, \*\*kwargs)**
  > url：拟获取页面的url链接。
  > **kwargs：13个可选的控制访问的参数。

- **requests.post(url, data=None, json=None, \*\*kwargs)**
  > url：拟获取页面的url链接。
  > data：字典、字节序列或文件，Request的内容。
  > json：JSON格式的数据，Request的内容。
  > **kwargs：剩余11个可选的控制访问的参数。

- **requests.put(url, data=None, \*\*kwargs)**
  > url：拟获取页面的url链接。
  > data：字典、字节序列或文件，Request的内容。
  > **kwargs：剩余12个可选的控制访问的参数。

- **requests.patch(url, data=None, \*\*kwargs)**
  > url：拟获取页面的url链接。
  > data：字典、字节序列或文件，Request的内容。
  > **kwargs：剩余12个可选的控制访问的参数。

- **requests.delete(url, \*\*kwargs)**
  > url：拟获取页面的url链接。
  > **kwargs：13个可选的控制访问的参数。

- **Response对象五个必要属性**

  |  异常           |  说明  |
  |  ----           |  ----  |
  |  r.status_code  |  HTTP请求的返回状态，200表示连接成功，404表示失败  |
  |  r.text         |  HTTP相应诶荣的字符串形式，即，url对应的页面内容  |
  |  r.encoding     |  从HTTP header中猜测的响应内容编码方式，如果header中不存在charset，则认为编码为ISO-8859-1  |
  |  r.apparent_encoding  |  从内容中分析出的响应内容编码方式（备选编码方式） |
  |  r.content      |  HTTP相应内容的二进制形式  |

- **r.encoding和r.apparent_encoding辨析**
  ```python
  >>> import requests
  >>> r = requests.get("http://www.baidu.com")
  >>> r.status_code
  200
  >>> r.text
  '<!DOCTYPE html>\r\n<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>ç\x99¾åº¦ä¸\x80ä¸\x8bï¼\x8cä½\xa0å°±ç\x9f¥é\x81\x93</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=ç\x99¾åº¦ä¸\x80ä¸\x8b class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>æ\x96°é\x97»</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>å\x9c°å\x9b¾</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>è§\x86é¢\x91</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>è´´å\x90§</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>ç\x99»å½\x95</a> </noscript> <script>document.write(\'<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u=\'+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ \'" name="tj_login" class="lb">ç\x99»å½\x95</a>\');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">æ\x9b´å¤\x9aäº§å\x93\x81</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>å\x85³äº\x8eç\x99¾åº¦</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>ä½¿ç\x94¨ç\x99¾åº¦å\x89\x8då¿\x85è¯»</a>&nbsp; <a href=http://jianyi.baidu.com/ class=cp-feedback>æ\x84\x8fè§\x81å\x8f\x8dé¦\x88</a>&nbsp;äº¬ICPè¯\x81030173å\x8f·&nbsp; <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>\r\n'
  >>> r.encoding
  'ISO-8859-1'
  >>> r.apparent_encoding
  'utf-8'
  >>> r.encoding = 'utf-8'
  >>> r.text
  '<!DOCTYPE html>\r\n<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>百度一下，你就知道</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=百度一下 class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>新闻</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>地图</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>视频</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>贴吧</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>登录</a> </noscript> <script>document.write(\'<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u=\'+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ \'" name="tj_login" class="lb">登录</a>\');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">更多产品</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>关于百度</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>使用百度前必读</a>&nbsp; <a href=http://jianyi.baidu.com/ class=cp-feedback>意见反馈</a>&nbsp;京ICP证030173号&nbsp; <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>\r\n'
  # 设置encoding属性收可以看到中文
  ```

- **Request异常**

  |  异常           |  说明  |
  |  ----           |  ----  |
  |requests.ConnectionError|网络连接错误异常，如DNS查询失败、拒绝链接等|
  |requests.HTTPError|HTTP错误异常|
  |requests.URLRequired|URL缺失异常|
  |requests.TooManyRedirects|超过最大重定向次数，产生重定向异常|
  |requests.ConnectTimeout|连接远程服务器超时异常|
  |requests.Timeout|请求URL超时，产生超时异常| 
  |r.raise_for_status()|如果不是200，产生异常requests.HTTPError，常放置于try块中|
