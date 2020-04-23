---
title: mariaDB语法笔记
date: 2020-04-20 17:25:26
tags:
- MySql
categories:
- 数据库
---
***对mariaDB操作的记录***
<!--more-->
---
- **创建数据库**
    - 语法：CREATE DATABASE [数据库名称] 
    - 注意：必须在要有相应权限
    - 也可以在创建数据库的同时初始化一些数据
        ```sql
        MariaDB [(none)]> CREATE DATABASE SCHOOL;
        Query OK, 1 row affected (0.000 sec)
        ```
    
- **选择数据库**
  - 语法：USE [数据库名称]
  - 代码：
    ```sql
    MariaDB [(none)]> USE SCHOOL
    Database changed
    MariaDB [SCHOOL]> 
    ```

- **删除数据库**
  - 语法：DROP DATABASE [数据库名称]
  - 代码：
    ```sql
    MariaDB [(none)]> show databases
        -> ;
    +--------------------+
    | Database           |
    +--------------------+
    | SCHOOL             |
    | information_schema |
    | mysql              |
    | performance_schema |
    | test               |
    +--------------------+
    5 rows in set (0.001 sec)

    MariaDB [SCHOOL]> DROP DATABASE SCHOOL;
    Query OK, 0 rows affected (0.002 sec)

    MariaDB [(none)]> SHOW DATABASES;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | test               |
    +--------------------+
    ```

- **创建表**
  - 语法：
    > CREATE [OR REPLACE] [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    (create_definition,...) [table_options    ]... [partition_options]

    > CREATE [OR REPLACE] [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    [(create_definition,...)] [table_options   ]... [partition_options]
    select_statement

    > CREATE [OR REPLACE] [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    { LIKE old_table_name | (LIKE old_table_name) }

    > select_statement:
    [IGNORE | REPLACE] [AS] SELECT ...   (Some legal select statement)
  - 代码：
    ```sql
    MariaDB [SCHOOL]> CREATE TABLE students(
        -> student_id INT NOT NULL AUTO_INCREMENT,
        -> student_name VARCHAR(100) NOT NULL,
        -> student_address VARCHAR(40) NOT NULL,
        -> admission_date DATE,
        -> PRIMARY KEY (student_id)
        -> );
    Query OK, 0 rows affected (0.006 sec)


    MariaDB [SCHOOL]> SHOW TABLES;
    +------------------+
    | Tables_in_SCHOOL |
    +------------------+
    | students         |
    +------------------+
    1 row in set (0.001 sec)


    MariaDB [SCHOOL]> SHOW CREATE TABLE `SCHOOL`.`students`;
    +----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | Table    | Create Table                                                                                                                                                                                                                                                                                                                                            |
    +----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    | students | CREATE TABLE `students` (
    `student_id` int(11) NOT NULL AUTO_INCREMENT,
    `student_name` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
    `student_address` varchar(40) COLLATE utf8mb4_unicode_ci NOT NULL,
    `admission_date` date DEFAULT NULL,
    PRIMARY KEY (`student_id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci |
    +----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
    1 row in set (0.001 sec)
    ```


- **删除表**
  - 语法：DROP TABLE [表名称];
  - 代码：
    ```sql
    MariaDB [SCHOOL]> DROP TABLE students;
    Query OK, 0 rows affected (0.009 sec)

    MariaDB [SCHOOL]> SHOW TABLES;
    Empty set (0.000 sec)
    ```

- **修改表**
  - 语法：
    > 添加一列：
    >> ALTER TABLE xxxx ADD COLUMN [IF NOT EXISTS]  (col_name column_definition,...)

    > 删除一列
    >> ALTER TABLE xxxx DROP COLUMN [IF EXISTS] col_name [CASCADE|RESTRICT]

    > MODIFY修改列里面值的类型
    >> ALTER TABLE t1 MODIFY x bigint unsigned;

    > CHANGE修改包括列名在内的一切数值
    >> ALTER TABLE t1 CHANGE a b bigint unsigned auto_increment;

    > ADD CONSTRAINT添加约束，不满足约束的数据不会被更新：
    >> ALTER TABLE table_name 
    >> ADD CONSTRAINT [constraint_name] CHECK(expression);

    > DROP CONSTRAINT删除约束：
    >> ALTER TABLE table_name
    >> DROP CONSTRAINT [constraint_name];

    > ENGINE更改存储引擎（会重建表）：
    >> ALTER TABLE xxxx ENGINE = InnoDB;

    > 重建表
    >> ALTER TABLE t1 FORCE;

    > 组合使用：
    >> 前面几种方法可以进行组合使用，用','隔开各个操作

  - 代码：
    ```sql
    MariaDB [SCHOOL]> CREATE TABLE students(
        -> student_id INT NOT NULL KEY AUTO_INCREMENT,
        -> student_name VARCHAR(100),
        -> student_address VARCHAR(100) NOT NULL
        -> );
    #添加一行
    MariaDB [SCHOOL]> ALTER TABLE students ADD COLUMN (student_age INT NOT NULL);
    # 删除一行
    MariaDB [SCHOOL]> ALTER TABLE students DROP COLUMN student_age;
    # 改变一列除了列名以外的所有数据
    ALTER TABLE students MODIFY student_age BIGINT UNSIGNED AUTO_INCREMENT;
    # 改变一列的所有数据
    ALTER TABLE students CHANGE student_age student_Age INT UNSIGNED AUTO_INCREMENT;
    # 添加约束
    ALTER TABLE students 
    ADD CONSTRAINT is_balanced 
        CHECK((student_id + student_Age) > 0);
    # 删除约束
    ALTER TABLE students 
    DROP CONSTRAINT is_balanced;
    # 组合用法
    ALTER TABLE students DROP x, ADD x2 INT,  CHANGE y y2 INT;
    # 更改存储引擎
    ALTER TABLE students ENGINE = InnoDB;
    # 重建表
    ALTER TABLE students FORCE;
    ```