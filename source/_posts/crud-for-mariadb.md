---
title: mariaDB的CRUD操作
date: 2020-04-21 13:52:01
tags:
- MySql
categories:
- 数据库
---
***对mariaDB操作的记录（简单记录，太详细会浪费大量时间）***
<!--more-->
---
```sql

```
- **插入数据**
  - 语法1：
    ```sql
    INSERT INTO tablename (field,field2,...) VALUES (value, value2,...);
    ```

  - 语法2：
    ```sql
    INSERT INTO table  
    (column1, column2, ... )  
    VALUES  
    (expression1, expression2, ... ),  
    (expression1, expression2, ... ),  
    ...;
    ```

  - 语法3：
    ```sql
    INSERT INTO table  
    (column1, column2, ... )  
    SELECT expression1, expression2, ...  
    FROM source_table  
    [WHERE conditions];
    ```

- **查询数据**
  - 基础语法
    ```sql
    SELECT expressions  
    FROM tables  
    [WHERE conditions];
    ```
  - 扩展语法
    ```sql
    SELECT [ ALL | DISTINCT ]  
    expressions  
    FROM tables  
    [WHERE conditions]  
    [GROUP BY expressions]  
    [HAVING condition]  
    [ORDER BY expression [ ASC | DESC ]];# ASC为升序排列，DESC为降序排列
    ```

- **限制返回记录**
    - 使用SELECT语句和LIMIT子句从表中检索一个或多个记录
        ```sql
        SELECT expressions  
        FROM tables  
        [WHERE conditions]  
        [ORDER BY expression [ ASC | DESC ]]  
        LIMIT row_count;# start_row,row_count用于分页
        ```

- **更新数据**
  - 改变一个数据中的某一个值
    ```sql
    UPDATE table  
    SET column1 = expression1,  
        column2 = expression2,  
        ...  
    [WHERE conditions]  
    [ORDER BY expression [ ASC | DESC ]]  
    [LIMIT number_rows];
    ```

- **删除数据**
    - DELETE语句用于从MariaDB数据库的表中删除一个或多个记录。
        ```sql
        DELETE FROM table  
        [WHERE conditions AND/OR other_conditions]  
        [ORDER BY expression [ ASC | DESC ]]  
        [LIMIT number_rows];
        ```

- **TRUNCATE截断表**
  - TRUNCATE TABLE语句用于从表中删除所有记录。它与没有WHERE子句的DELETE语句相同，使用后该表将被永久删除，无法回滚。
    ```sql
    TRUNCATE [TABLE] [database_name.]table_name;
    ```

- **CRUD操作的条件列表**
    |编号|条件|描述|
    |:---:|:---:|:---:|
    |1|AND|在满足2个以上的条件时使用。|
    |2|OR|在满足任何一个条件时使用。|
    |3|AND & OR|当AND＆OR条件满足时使用它。|
    |4|LIKE|在where子句中使用简单的模式匹配(通配符)。|
    |5|RLIKE|在where子句中使用正则表达式匹配。|
    |6|IN|它用作多个OR条件的替代。|
    |7|NOT|它用来否定一个条件。|
    |8|IS NULL|它用于测试一个NULL值。|
    |9|IS NOT NULL|它用来测试一个非NULL值。|
    |10|BETWEEN|它用于在一个范围内(包括)进行检索。|
    |11|EXISTS|它用于指定是否符合条件，然后子查询至少返回一行。|









