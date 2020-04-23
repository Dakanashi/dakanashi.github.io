---
title: gnome3.36版本解决dash to dock无法使用问题
date: 2020-03-12 22:37:05
tags:
- Linux
categories:
- Linux系统疑难杂症 
---
## *Dash to dock can't work well.*
作为一个arch用户，当滚动更新gnome之后虽然觉得登录界面毛玻璃背景和动态效果很好看，但是很多插件无法使用让人着实难受，最要命的是dock插件无法使用带来的极大困扰。。。。。
<!--more-->
***
1. git clone -b tmp/gnome-3.36 https://github.com/micheleg/dash-to-dock.git （国内用户可以使用proxychins代理）  
2. cd dash-to-dock
3. make && make install  
当插件正常更新后删除掉文件夹即可
