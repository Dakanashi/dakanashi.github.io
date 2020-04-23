---
title: Arch Linux安装deb包
date: 2020-04-18 16:10:14
tags:
- Linux
categories:
- Linux系统疑难杂症
---
***最近想要使用linux端的百度云，但官网只给出了rpm和deb的安装包，就从网上查询了几种方法***
<!--more-->
---
- **使用dpkg安装（不推荐）**
    > 不推荐原因：arch有自己的pacman，再用其他的包管理会把系统里的依赖关系搞乱
    ```shell
    # 使用yay或yaourt安装dpkg
    yay -S dpkg
    # 使用dpkg安装deb
    dpkg -i xxx.deb
    ```

- **使用debtap将deb转化为pacman安装包（推荐）**
    ```shell
    # 安装bash，binutils，pkgfile，fakeroot和debtap包
    pacman -S bash binutils pkgfile fakeroot
    yay -S debtap
    # 运行以下命令来创建/更新 pkgfile 和 debtap 数据库
    sudo debtap -u
    # 使用debtap转换deb包
    debtap xxx.deb
    # 安装
    pacman -U xxx.pkg
    ```
