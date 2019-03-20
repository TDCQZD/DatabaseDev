# 第一节 ：存储过程和函数的引入

存储过程和函数是在数据库中定义一些 SQL 语句的集合，然后直接调用这些存储过程和函数来执行已经定义好的 SQL 语句。存储过程和函数可以避免开发人员重复的编写相同的 SQL 语句。而且，存储过程和函数是在 MySQL服务器中存储和执行的，可以减少客户端和服务器端的数据传输

# 第二节 ：创建存储过程和函数

**2.1 创建存储过程                
**

语法：

```
CREATE PROCEDURE sp_name([proc_parameter[,...]])
[characteristic...] routine_body
```

p\_name 参数是存储过程的名称；

proc\_parameter 表示存储过程的参数列表；

characteristic 参数指定存储过程的特性；

routine\_body 参数是 SQL 代码的内容，可以用 BEGIN...END 来标志 SQL 代码的开始和结束。

proc\_parameter 中的每个参数由 3 部分组成。这 3 部分分别是输入输出类型、参数名称和参数类型。

\[ IN \| OUT \| INOUT \] param\_name type

其中，IN 表示输入参数；OUT 表示输出参数；INOUT 表示既可以是输入，也可以是输出；param\_name 参数是

存储过程的参数名称；type 参数指定存储过程的参数类型，该类型可以是 MySQL 数据库的任意数据类型；

Characteristic 参数有多个取值。其取值说明如下：

LANGUAGE SQL：说明 routine\_body 部分是由 SQL 语言的语句组成，这也是数据库系统默认的语言。

\[ NOT \] DETERMINISTIC ：指明存储过程的执行结果是否是确定的。DETERMINISTIC 表示结果是确定的。每次执行存储过程时，相同的输入会得到相同的输出。NOT DETERMINISTIC 表示结果是非确定的，相同的输入可能得到不同的输出。默认情况下，结果是非确定的。

{ CONTAINS SQL \| NO SQL \| READS SQL DATA\| MODIFIES SQL DATA} ：指明子程序使用 SQL 语句的限制；

CONTAINS SQL 表示子程序包含 SQL 语句，但不包含读或写数据的语句；

NO SQL 表示子程序中不包含 SQL语句；

READS SQL DATA 表示子程序中包含读数据的语句；

MODIFIES SQL DATA 表示子程序中包含写数据的语句。默认情况下，系统会指定为 CONTAINS SQL；

SQL SECURITY { DEFINER \| INVOKER }；指明谁有权限来执行。

DEFINER 表示只有定义者自己才能够执行；

INVOKER 表示调用者可以执行。默认情况下，系统指定的权限是 DEFINER。

COMMENT ‘string’ ：注释信息；

示例：

**2.2 创建存储函数                
**

```
CREATE FUNCTION sp_name ( [func_parameter[,...]] )
RETURNS type
[ characteristic... ] routine_body
```

sp\_name 参数是存储函数的名称；

func\_parameter 表示存储函数的参数列表；

RETURNS type 指定返回值的类型；

characteristic 参数指定存储过程的特性，该参数的取值与存储过程中的取值是一样的；

routine\_body 参数是 SQL 代码的内容，可以用 BEGIN...END 来标志 SQL 代码的开始和结束；

func\_parameter 可以由多个参数组成，其中每个参数由参数名称和参数类型组成，其形式如下：

param\_name type 其中，param\_name 参数是存储函数的参数名称；

type 参数指定存储函数的参数类型，该类型可以是 MySQL 数据库的任意数据类型；

示例：

**2.3 变量的使用            **

**2.3.**1、定义变量

```
DECLARE var_name [,...] type [ DEFAULT value ]
```

**2.3.**2、为变量赋值

```
SET var_name = expr [,var_name=expr] ...

SELECT col_name[,...] INTO var_name[,...]
         FROM table_name WHERE condition
```

示例：

**2.4 游标的使用                
**

查询语句可能查询出多条记录，在存储过程和函数中使用游标来逐条读取查询结果集中的记录。游标的使用包括声明游标、打开游标、使用游标和关闭游标。游标必须声明在处理程序之前，并且声明在变量和条件之后。

**2.4.**1、声明游标

```
DECLARE cursor_name CURSOR FOR select_statement ;
```

**2.4.**2、打开游标

```
OPEN cursor_name;
```

**2.4.**3、使用游标

```
FETCH cursor_name INTO var_name [,var_name ... ]；
```

**2.4.**4、关闭游标

```
CLOSE cursor_name；
```

示例：

**2.5 流程控制的使用                
**存储过程和函数中可以使用流程控制来控制语句的执行。MySQL 中可以使用 IF 语句、CASE 语句、LOOP  
语句、LEAVE 语句、ITERATE 语句、REPEAT 语句和 WHILE 语句来进行流程控制。

**2.5.**

**2.5.1、**IF 语句

```
IF search_condition THEN statement_list
      [ ELSEIF search_condition THEN statement_list ]...
      [ ELSE statement_list ]
END IF
```

示例：

```
/* IF 语句*/
DELIMITER &&
CREATE PROCEDURE pro_user5(IN bookId INT)
    BEGIN
     SELECT COUNT(*) INTO @num FROM t_user WHERE id=bookId;
     IF @num>0 THEN UPDATE t_user SET userName='java12345' WHERE id=bookId;
     ELSE
       INSERT INTO t_user VALUES(NULL,'2312312','2321312');
     END IF ;
    END 
&&
DELIMITER ;
```

**2.5.**2 CASE 语句

```
CASE case_value
     WHEN when_value THEN statement_list
     [WHEN when_value THEN statement_list]...
      [ELSE statement_list ]
END CASE
```

示例：

```
/*CASE 语句 */
DELIMITER &&
CREATE PROCEDURE pro_user6(IN bookId INT)
    BEGIN
     SELECT COUNT(*) INTO @num FROM t_user WHERE id=bookId;
     CASE @num
      WHEN 1 THEN UPDATE t_user SET userName='java12345' WHERE id=bookId;
      WHEN 2 THEN INSERT INTO t_user VALUES(NULL,'2312312','2321312');
      ELSE INSERT INTO t_user VALUES(NULL,'231231221321312','2321312321312');
     END CASE ;
    END 
&&
DELIMITER ;
```

**2.5.**3、LOOP，LEAVE 语句

LOOP 语句可以使某些特定的语句重复执行，实现一个简单的循环。但是 LOOP 语句本身没有停止循环的语句，必须是遇到 LEAVE 语句等才能停止循环。LOOP 语句的语法的基本形式如下：

```
[begin_label：]LOOP
               Statement_list
END LOOP [ end_label ]
```

LEAVE 语句主要用于跳出循环控制。语法形式如下：

```
LEAVE label
```

示例：

```
/*LOOP，LEAVE 语句*/
DELIMITER &&
CREATE PROCEDURE pro_user7(IN totalNum INT)
    BEGIN
      aaa:LOOP
        SET totalNum=totalNum-1;
        IF totalNum=0 THEN LEAVE aaa ;
        ELSE INSERT INTO t_user VALUES(totalNum,'2312312','2321312');
        END IF ;
      END LOOP aaa ;
    END 
&&
DELIMITER ;
```

**2.5.**4、ITERATE 语句

ITERATE 语句也是用来跳出循环的语句。但是，ITERATE 语句是跳出本次循环，然后直接进入下一次  
循环。基本语法：

```
ITERATE label ;
```

示例：

```
/* ITERATE语句 */
DELIMITER &&
CREATE PROCEDURE pro_user8(IN totalNum INT)
    BEGIN
      aaa:LOOP
        SET totalNum=totalNum-1;
        IF totalNum=0 THEN LEAVE aaa ;
        ELSEIF totalNum=3 THEN ITERATE aaa ;
        END IF ;
        INSERT INTO t_user VALUES(totalNum,'2312312','2321312');
      END LOOP aaa ;
    END 
&&
DELIMITER ;
```

**2.5.**5、REPEAT 语句

REPEAT 语句是有条件控制的循环语句。当满足特定条件时，就会跳出循环语句。REPEAT 语句的基本  
语法形式如下：

```
[begin_label:] REPEAT
                 Statement_list
                 UNTIL search_condition
END REPEAT [ end_label ]
```

示例：

```
/* REPEAT 语句 */
DELIMITER &&
CREATE PROCEDURE pro_user9(IN totalNum INT)
    BEGIN
      REPEAT
         SET totalNum=totalNum-1;
         INSERT INTO t_user VALUES(totalNum,'2312312','2321312');
         UNTIL totalNum=1 
      END REPEAT;
    END 
&&
DELIMITER ;
```

**2.5.**6、WHILE 语句

```
[begin_label:] WHILE search_condition DO
                  Statement_list
END WHILE [ end_label ]
```

示例：

```
/* WHILE 语句 */
DELIMITER &&
CREATE PROCEDURE pro_user10(IN totalNum INT)
    BEGIN
     WHILE totalNum>0 DO
      INSERT INTO t_user VALUES(totalNum,'2312312','2321312');
      SET totalNum=totalNum-1;
     END WHILE ;
    END 
&&
DELIMITER ;
```

# 第三节 ：调用存储过程和函数

**3.1 调用存储过程              
**

```
CALL sp_name( [parameter[,...]] )
```

**3.2 调用存储函数**

```
fun_name( [parameter[,...]] )
```

示例：

```
CALL pro_user6(6);
```

# 第四节 ：查看存储过程和函数

**4.1 SHOW STATUS 语句查看存储过程和函数的状态              
**

```
SHOW { PROCEDURE | FUNCTION } STATUS [ LIKE ‘pattern’] ;
```

示例：

```
/* SHOW STATUS 语句查看存储过程和函数的状态*/
SHOW PROCEDURE STATUS LIKE 'pro_book';
```

**4.2 SHOW CREATE 语句查看存储过程的函数的定义**

SHOW CREATE { PROCEDURE \| FUNCTION } sp\_name ;

示例：

```
/* SHOW CREATE 语句查看存储过程的函数的定义*/
SHOW CREATE PROCEDURE pro_book;
```

**4.3 从 information\_schema.Routines 表中查看存储过程和函数的信息**

# 第五节 ：修改存储过程和函数

```
ALTER { PROCEDURE | FUNCTION } sp_name [ characteristic ... ]
characteristic :
{ CONTAINS SQL } NO SQL | READS SQL DATA| MODIFIES SQL DATA}
| SQL SECURITY { DEFINER | INVOKER }
| COMMENT ‘string’
```

其中，sp\_name 参数表示存储过程或函数的名称；characteristic 参数指定函数的特性。

CONTAINS SQL 表示子程  
序包含 SQL 语句，但不包含读或写数据的语句；

NO SQL 表示子程序中不包含 SQL 语句；

READS SQL DATA  
表 示 子 程 序 中 包 含 数 据 的 语 句 ；

MODIFIES SQL DATA 表 示 子 程 序 中 包 含 写 数 据 的 语 句 。

SQL  
 SECURITY{ DEFINER \| INVODER } 指明谁有权限来执行。

DEFINER 表示只有定义者自己才能够执行；

INVODER 表示调用者可以执行。COMMENT ‘string’ 是注释信息。

示例：

```
/*修改存储过程和函数*/
ALTER PROCEDURE pro_book  COMMENT '我来测试一个COMMENT';
```

# 第六节 ：删除存储过程和函数

```
DROP {PROCEDURE | FUNCTION } sp_name ;
```

示例：

```
/*删除存储过程和函数*/
DROP PROCEDURE pro_user3;
```





