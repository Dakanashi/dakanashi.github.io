---
title: mariaDB连接
date: 2020-04-21 16:20:42
tags:
- MySql
categories:
- 数据库
---
***对mariaDB不同连接方式的记录（简单记录，太详细会浪费大量时间）***
<!--more-->
---
- **内连接，INNER JOIN (也称为SIMPLE JOIN)**
  - INNER JOIN是最常见的连接类型，它返回连接条件满足的多个表中的所有行。
  - 位置在FROM之后。
  - ![内连接](/images/connect_1.png)
    ```sql
    INNER JOIN database_name  
    ON expression
    ```

- **左外连接**
  - LEFT OUTER JOIN用于返回ON条件中指定的左侧表中的所有行，并仅返回满足连接条件的其他表中的行。
  - ![左外连接](/images/connect_2.png)
    ```sql
    SELECT columns  
    FROM table1  
    LEFT [OUTER] JOIN table2  
    ON table1.column = table2.column;
    ```

- **右外连接**
  - RIGHT OUTER JOIN用于返回ON条件中指定的右表中的所有行，并且仅返回来自其他表中连接字段满足条件的行。
  - ![右外连接](/images/connect_3.png)
    ```sql
    SELECT columns  
    FROM table1  
    RIGHT [OUTER] JOIN table2  
    ON table1.column = table2.column;
    ```


