---
title: mariaDB介绍
date: 2020-04-20 16:59:23
tags:
- MySql
categories:
- 数据库
---
---
***除了web前端的那些琐碎知识点，mysql语法也属于许久不用就会遗忘的东西，距离上次学习mariaDB语法已有一年之久，相关知识真的是忘得一干二净，所以直接记录一下等以后忘了再看一下就好了***
<!--more-->
---
- **参考**
  - 部分资料参考自[易百教程](https://www.yiibai.com/mariadb)。
- **安装**
  - 使用的系统为arch Linux，直接在wiki上查询教程。
  - 当自己创建一个用户时，记得赋予其权利，否则无法正常使用。
    ```sql
    GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost' WITH GRANT OPTION;
    ```

- **RDBMS术语**
    - Database - 数据库是由保存相关数据的表组成的数据源。
    - Table -表，这意味着电子表格，是包含数据的矩阵。
    - Column - 表示数据元素的列是保存一种类型的数据的结构;例如，送货日期。
    - Row - 行是对相关数据进行分组的结构;例如，用于客户的数据。它也被称为元组，条目或记录。
    - Redundancy - 此术语指的是存储数据两次，以加速系统。
    - Primary Key - 这指的是唯一的标识值。此值不能在表中出现两次，并且只有一个行与其关联。
    - Foreign Key - 外键用作两个表之间的链接。
    - Compound Key -复合键，或复合键，是指多个列的关键。它指的是多列由于缺乏独特的质量一列。
    - Index - 索引实际上与书的索引相同。
    - Referential Integrity - 此术语指确保所有外键值指向现有行。

- **MARIA数据库**
    - MariaDB是由MySQL的原始开发人员创建的MySQL的流行分支。 它源于与MySQL收购Oracle有关的问题。 它支持小数据处理任务和企业需求。 它旨在成为MySQL的替代，只需要简单的卸载MySQL和安装MariaDB。 MariaDB提供与MySQL等相同的功能。

    - MariaDB的主要特性
      - 所有MariaDB都在GPL，LGPL或BSD下。
      - MariaDB包括各种存储引擎，包括高性能存储引擎，用于与其他RDBMS数据源一起工作。
      - MariaDB使用标准和流行的查询语言。
      - MariaDB在多个操作系统上运行，并支持各种各样的编程语言。
      - MariaDB提供对PHP的支持，PHP是最流行的Web开发语言之一。
      - MariaDB提供Galera集群技术。
      - MariaDB还提供了许多在MySQL中不可用的操作和命令，并消除/取代影响性能的功能。

- **管理命令**
  - USE [database name] - 设置当前默认数据库。
  - SHOW DATABASES - 列出服务器上当前的数据库。
  - SHOW TABLES - 列出所有非临时表。
  - SHOW COLUMNS FROM [table name] - 提供与指定表有关的列信息。
  - SHOW INDEX FROM TABLENAME [table name] - 提供与指定表相关的表索引信息。
  - SHOW TABLE STATUS LIKE [table name] \ G - - 提供有关非临时表的信息的表，以及LIKE子句用于获取表名后显示的模式。