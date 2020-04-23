---
title: mariaDB字句
date: 2020-04-21 14:25:18
tags:
- MySql
categories:
- 数据库
---
***对mariaDB字句的记录（简单记录，太详细会浪费大量时间）***
<!--more-->
---
- **where字句**
  - WHERE子句是一个可选的子句。它可以和AND，OR，AND & OR，LIKE运算符一起使用，优先级从左至右逐渐降低。
    ```sql
    [COMMAND] field,field2,... FROM table_name,table_name2,... WHERE [CONDITION]
    ```

- **Like字句**
  - 它用于模式匹配并返回true或false，可以和NOT结合使用。用于比较的模式接受以下通配符：
    > "%"通配符：匹配字符数(0或更多)。
    > "_"通配符：匹配单个字符。它匹配其集合中的字符。
    ```sql
    SELECT field, field2,... FROM table_name, table_name2,...  
    WHERE field LIKE condition
    ```

- **By字句**
  - ORDER BY子句用于按升序或降序对结果集中的记录进行排序：
    > ASC表示升序
    > DESC表示降序
    > 可以不使用上面的两个，按照某一个数据进行排序（默认为ASC）
    > 可以使用多列排序
    ```sql
    ELECT expressions  
    FROM tables  
    [WHERE conditions]  
    ORDER BY expression [ ASC | DESC ];
    ```

- **Distinct字句**
  - DISTINCT子句用于在SELECT语句中从结果中删除重复项。当在DISTINCT子句中仅使用表达式时，查询将返回该表达式的唯一值。当您使用多个表达式在DISTINCT子句时，查询将返回多个表达式的唯一组合。DISTINCT子句不会忽略NULL值。因此，在SQL语句中使用DISTINCT子句时，结果集将包含NULL作为不同的值。
    ```sql
    SELECT DISTINCT expressions  
    FROM tables  
    [WHERE conditions];
    ```

- **From子句**
  - FROM子句用于从表中获取数据，它也被用来连接表。
    ```sql
    SELECT columns_names FROM table_name;
    ```



