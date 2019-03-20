# 第一节 ：触发器的引入

触发器（TRIGGER）是由事件来触发某个操作。这些事件包括 INSERT 语句、UPDATE 语句和 DELETE 语句。  
当数据库系统执行这些事件时，就会激活触发器执行相应的操作。

# 第二节 ：创建与使用触发器

**2.1 创建只有一个执行语句的触发器          
**

语法：

```
CREATE TRIGGER 触发器名 BEFORE |AFTER 触发事件
                ON 表名 FOR EACH ROW 执行语句
```

示例：

```
/* 创建只有一个执行语句的触发器 */
CREATE TRIGGER trig_book AFTER INSERT 
     ON t_book FOR EACH ROW
        UPDATE t_bookType SET bookNum=bookNum+1 WHERE new.bookTypeId=t_booktype.id;
```

2.2 创建有多个执行语句的触发器

语法：

```
CREATE TRIGGER 触发器名 BEFORE |AFTER 触发事件
        ON 表名 FOR EACH ROW
         BEGIN
        执行语句列表
         END
```

示例：

```
/*
创建有多个执行语句的触发器
*/

DELIMITER |
CREATE TRIGGER trig_book2 AFTER DELETE 
    ON t_book FOR EACH ROW
    BEGIN
       UPDATE t_bookType SET bookNum=bookNum-1 WHERE old.bookTypeId=t_booktype.id;
       INSERT INTO t_log VALUES(NULL,NOW(),'在book表里删除了一条数据');
       DELETE FROM t_test WHERE old.bookTypeId=t_test.id;
    END |
DELIMITER ;
```

**注：**

**1、DELIMITER 关键词使用时，需要顶格**

**2、数据处理的一种操作只能有一种触发器。如：表数据的插入只能有一个在插入时操作的触发器。**

# 第三节 ：查看触发器

3.1 SHOW TRIGGERS 语句查看触发器信息

示例：

```
/*查看触发器*/
SHOW TRIGGERS;
```

3.2 在 triggers 表中查看触发器信息

# 第四节：删除触发器

语法：

```
DROP TRIGGER 触发器名；
```

示例：

```
/*删除触发器*/
DROP TRIGGER trig_book2 ;
```



