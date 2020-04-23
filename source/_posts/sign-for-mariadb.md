---
title: mariaDB符号
date: 2020-04-21 16:40:24
tags:
- MySql
categories: 
- 数据库
---
***对mariaDB操作符的记录（简单记录，太详细会浪费大量时间）***
<!--more-->
---
- **比较运算符**
    |编号|比较运算符|描述|示例|
    |:---:|:---:|:---:|:---:|
    |1|=|比较等于|select * from students where id=100|
    |2|<=>|比较等于(安全比较NULL值)|select * from students where student_name<=>'Maxsu'|
    |3|<>|比较不等于|select * from students where student_name<>'Maxsu'|
    |4|!=|比较不等于|select * from students where student_name!='Maxsu'|
    |5|>|比较大于|select * from students where student_id>5|
    |6|>=|比较大于或等于|select * from students where student_id>=5|
    |7|<|比较小于|select * from students where student_id<5|
    |8|<=|比较小于或等于|select * from students where student_id<=5|
    |9|in ( )|匹配列表中的值|select * from students where student_id IN(1,3,6)|
    |10|not|否定一个条件|select * from students where student_id  NOT IN(1,3,6)|
    |11|between|匹配在一个范围内(含)|select * from students where student_id between 1 AND 3)|
    |12|is null|判断是否为NULL值|select * from students where student_address IS NULL|
    |13|is not null|判断是否为非NULL值|select * from students where student_address IS NOT NULL|
    |14|like|与%和_模式匹配|select * from students where student_name LIKE 'Ma%'|
    |15|exists|如果子查询返回至少一行，则满足条件。|—|

- **Union运算符**
  - UNION运算符用于组合两个或更多SELECT语句的结果集。它删除各种SELECT语句之间的重复行。
  - UNION运算符中的每个SELECT语句在具有相似数据类型的结果集中必须具有相同数量的字段。
  - 使用ORDER BY子句的UNION运算符从两个表中检索多个列。
    ```sql
    SELECT expression1, expression2, ... expression_n  
    FROM tables  
    [WHERE conditions]  
    UNION [DISTINCT]  
    SELECT expression1, expression2, ... expression_n  
    FROM tables  
    [WHERE conditions];
    ```

- **Union All运算符**
  - UNION ALL操作符与UNION操作符相同，但不会删除重复的记录。它返回查询中的所有行，并且不删除各种SELECT语句之间的重复行。
  - UNION All运算符中的每个SELECT语句在具有相似数据类型的结果集中必须具有相同数量的字段。
  - 使用ORDER BY子句的UNION运算符从两个表中检索多个列。
    ```sql
    SELECT expression1, expression2, ... expression_n  
    FROM tables  
    [WHERE conditions]  
    UNION ALL  
    SELECT expression1, expression2, ... expression_n  
    FROM tables  
    [WHERE conditions];
    ```

- **Intersect运算符**
  - INTERSECT运算符用于返回2个或更多表的交集。 如果两个表中都存在记录，它将被包含在INTERSECT结果中。 否则，它将从INTERSECT结果中被省略。
  - MariaDB不支持INTERSECT运算符，但是通过使用IN运算符来模拟INTERSECT查询，可以看到相同的结果，如下示例中所示。
    ```sql
    SELECT expression1, expression2, ... expression_n  
    FROM tables  
    [WHERE conditions]  
    INTERSECT  
    SELECT expression1, expression2, ... expression_n  
    FROM tables  
    [WHERE conditions];
    ```

