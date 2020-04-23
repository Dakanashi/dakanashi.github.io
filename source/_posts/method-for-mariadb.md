---
title: mariaDB聚合函数
date: 2020-04-21 15:42:13
tags:
- MySql
categories:
- 数据库
---
***对mariaDB聚合函数的记录（简单记录，太详细会浪费大量时间）***
<!--more-->
---
- **Count()函数**
  - COUNT()函数用于返回表达式的计数/行数，只计算NOT NULL值。
  - 在COUNT之后的aggregate_expression可以添加DISTINCT防止统计重复数据。
    ```sql
    SELECT COUNT(aggregate_expression)  [AS "title"]
    FROM tables  
    [WHERE conditions];
    ```

- **SUM()函数**
  - SUM()函数用于返回表达式求和的值。
  - 可以使用SUM函数的DISTINCT子句来避免重复值的总和。
    ```sql
    SELECT SUM(aggregate_expression)  
    FROM tables  
    [WHERE conditions];
    ```

- **MIN()函数**
  - MIN()函数用于检索表达式的最小值。
  - 可以使用Min函数的GROUP BY子句来分组。
    ```sql
    SELECT MIN(aggregate_expression)  
    FROM tables  
    [WHERE conditions];
    ```

- **MAX()函数**
  - MAX()函数用于检索表达式的最大值。
  - 可以使用MAX函数的GROUP BY子句来分组。
    ```sql
    SELECT MAX(aggregate_expression)  
    FROM tables  
    [WHERE conditions];
    ```

- **AVG()函数**
  - Avg()函数用于检索表达式的平均值。
  - 也可以在AVG()函数使用数学公式。
    ```sql
    SELECT AVG(aggregate_expression)  
    FROM tables  
    [WHERE conditions];

    # 扩展使用
    SELECT expression1, expression2, ... expression_n,  
    AVG(aggregate_expression)  
    FROM tables  
    [WHERE conditions]  
    GROUP BY expression1, expression2, ... expression_n;
    ```