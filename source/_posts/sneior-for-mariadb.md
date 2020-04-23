---
title: mariaDB高级技巧
date: 2020-04-21 20:15:05
tags:
- MySql
categories:
- 数据库
---
***对mariaDB高级部分的记录（简单记录，太详细会浪费大量时间）***
<!--more-->
---
- **MariaDB创建函数**
  - 语法
    ```sql
    CREATE   
    [ DEFINER = { CURRENT_USER | user_name } ]   
    FUNCTION function_name [ (parameter datatype [, parameter datatype]) ]  
    RETURNS return_datatype [ LANGUAGE SQL  
    | DETERMINISTIC  
    | NOT DETERMINISTIC  
    | { CONTAINS SQL   
    | NO SQL  
    | READS SQL DATA  
    | MODIFIES SQL DATA }  
    | SQL SECURITY { DEFINER | INVOKER }  
    | COMMENT 'comment_value'  
    BEGIN  
        declaration_section  
        executable_section  
    END;
    ```
  - 示例
    ```sql
    DELIMITER //
    CREATE FUNCTION CalcValue ( starting_value INT )
    RETURNS INT DETERMINISTIC
    BEGIN
        DECLARE total_value INT;# 声明一个变量
        SET total_value = 0;# 为变量赋值
        label1: WHILE total_value <= 3000 DO# 循环
        SET total_value = total_value + starting_value;
        END WHILE label1;# 结束循环
        RETURN total_value;# 返回值
    END; //# 结束循环
    DELIMITER ;
    ```

  - 参数说明
    - DEFINER子句：它是一个可选的子句。如果没有指定，定义者是创建函数的用户。 如果您希望指定不同的定义者，则必须包含DEFINER子句，其中user_name是该函数的定义者。
    - function_name：指定要在MariaDB中分配给此函数的名称。
    - return_datatype：它指定函数返回值的数据类型。
    - LANGUAGE SQL：语法为可移植语法，但不会影响函数。
    - DETERMINISTIC：表示该函数将总是返回给定一组输入参数的一个结果。
    - NOT DETERMINISTIC：表示给定一组输入参数，该函数可能会返回不同的结果。 结果可能受到表数据，随机数字或服务器变量的影响。
    - CONTAINS SQL：这是默认的。这是一个告知MariaDB该函数包含SQL的信息性子句，但数据库不验证为真。
    - NO SQL：没有使用的信息性子句将不会影响函数。
    - READS SQL DATA：一个告知MariaDB该函数将使用SELECT语句读取数据但不修改任何数据的信息性子句。
    - MODIFIES SQL DATA：一个告知MariaDB该函数将使用INSERT，UPDATE，DELETE或其他DDL语句修改SQL数据的信息性子句。
    - declaration_section：声明局部变量的函数的地方。
    - executable_section：在函数中输入函数代码的地方。

- **MariaDB过程**
  - 语法
      ```sql
      CREATE   
      [ DEFINER = { CURRENT_USER | user_name } ]   
      PROCEDURE procedure_name [ (parameter datatype [, parameter datatype]) ]  
      [ LANGUAGE SQL  
      | DETERMINISTIC  
      | NOT DETERMINISTIC  
      | { CONTAINS SQL   
      | NO SQL  
      | READS SQL DATA  
      | MODIFIES SQL DATA }  
      | SQL SECURITY { DEFINER | INVOKER }  
      | COMMENT 'comment_value'  
      BEGIN  
      declaration_section  
      executable_section  
      END;
      ```

  - 参数说明
    - DEFINER：可选。procedure_name：在MariaDB中分配给此过程的名称。
    - Parameter：传入过程的一个或多个参数。创建过程时，可以声明三种类型的参数：
        - IN：参数可以被程序引用。 该参数的值不能被程序覆盖。
        - OUT：参数不能被程序引用，但参数的值可以被程序覆盖。
        - IN OUT：参数可以被程序引用，参数的值可以被程序覆盖。
    - LANGUAGE SQL：语法为可移植语法，但不会影响函数。
    - DETERMINISTIC：表示该函数将始终返回给定一组输入参数的一个结果。
    - NOT DETERMINISTIC：表示给定一组输入参数，该函数可能会返回不同的结果。 结果可能受到表格数据，随机数字或服务器变量的影响。
    - CONTAINS SQL：这是默认的。这是一个告知MariaDB该函数包含SQL的信息性子句，但数据库不验证它是真的。
    - NO SQL：这是一个信息性子句，不使用也不会影响功能。
    - READS SQL DATA：这是一个告知MariaDB的函数，它将使用SELECT语句读取数据，但不会修改任何数据。
    - MODIFIES SQL DATA：这是一个告知MariaDB的信息子句，该函数将使用INSERT，UPDATE，DELETE或其他DDL语句修改SQL数据。
    - declaration_section：声明局部变量的过程中的位置。
    - executable_section：输入过程代码的过程中的位置。
  
  - 示例
    ```sql
    DELIMITER //  
    CREATE procedure CalcValue ( OUT ending_value INT )  
    DETERMINISTIC  
    BEGIN  
    DECLARE total_value INT;  
    SET total_value = 50;  
    label1: WHILE total_value <= 3000 DO  
        SET total_value = total_value * 2;  
    END WHILE label1;  
    SET ending_value = total_value;  
    END; //  
    DELIMITER ;
    ```

  - 删除过程
      ```sql
      DROP procedure [ IF EXISTS ] procedure_name;
      ```

- **MariaDB正则表达式**
    - 详见之前学习爬虫时关于[re库的笔记](https://www.johnaki.xyz/2020/04/13/re-note/)。





