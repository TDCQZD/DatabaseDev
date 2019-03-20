# 第一节 ：视图的引入

1、视图是一种虚拟的表，是从数据库中一个或者多个表中导出来的表。

2、数据库中只存放了视图的定义，而并没有存放视图中的数据，这些数据存放在原来的表中。

3、使用视图查询数据时，数据库系统会从原来的表中取出对应的数据。

# 第二节 ：视图的作用

1、使操作简便化；

2、增加数据的安全性；

3、提高表的逻辑独立性；

# 第三节 ：创建视图

**3.1语法：**

```
CREATE [ALGORITHM ={ UNDEFIEND | MERGE | TEMPTABLE }]
        VIEW 视图名 [ ( 属性清单) ]
        AS SELECT 语句
        [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]；
```

**参数说明：**

ALGORITHM 是可选参数，表示视图选择的算法；

“视图名”参数表示要创建的视图的名称；

“属性清单”是可选参数，其指定了视图中各种属性的名词，默认情况下与 SELECT 语句中查询的属性相同；

SELECT 语句参数是一个完整的查询语句，标识从某个表查出某些满足条件的记录，将这些记录导入视图中；

WITH CHECK OPTION 是可选参数，表似乎更新视图时要保证在该视图的权限范围之内；

ALGORITHM 包括 3 个选项 UNDEFINED、MERGE 和 TEMPTABLE。其中，UNDEFINED 选项表示 MySQL 将  
自动选择所要使用的算法；

MERGE 选项表示将使用视图的语句与视图定义合并起来，使得视图定义的某一部分  
取代语句的对应部分；

TEMPTABLE 选项表示将视图的结果存入临时表，然后使用临时表执行语句；

CASCADED  
是可选参数，表示更新视图时要满足所有相关视图和表的条件，该参数为默认值；

LOCAL 表示更新视图时，要  
满足该视图本身的定义条件即可；

**3.2  在单表上创建视图  **

```
CREATE VIEW v1 AS SELECT * FROM t_book;

CREATE VIEW v2 AS SELECT bookName,price FROM t_book;
/*在单表上创建视图 
创建视图 并重命名
*/
CREATE VIEW v3(b,p) AS SELECT bookName,price FROM t_book;
```

**3.3 在多表上创建视图  **

```
CREATE VIEW v4 AS SELECT bookName,bookTypeName FROM t_book,t_booktype WHERE t_book.bookTypeId=t_booktype.id;

CREATE VIEW v5 AS SELECT tb.bookName,tby.bookTypeName FROM t_book tb,t_booktype tby WHERE tb.bookTypeId=tby.id;
```

# 第四节 ：查看视图

**4.1 DESCRIBE 语句查看视图基本信息  **

```
DESC v5;
```

**4.2 SHOW TABLE STATUS 语句查看视图基本信息          
**

```
SHOW TABLE STATUS LIKE 'v5';

SHOW TABLE STATUS LIKE 't_book';
```

**4.3 SHOW CREATE VIEW 语句查看视图详细信息  **

```
SHOW CREATE VIEW v5;
```

**4.3 在 views 表中查看视图详细信息          
**

操作MySQL的工具：SQL

# 第五节 ：修改视图

**5.1 CREATE OR REPLACE VIEW 语句修改视图          
**

语法：

```
CREATE OR REPLACE [ALGORITHM ={ UNDEFINED | MERGE | TEMPTABLE }]
                  VIEW 视图名 [( 属性清单 )]
                  AS SELECT 语句
                  [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]；
```

示例：

```
 CREATE OR REPLACE VIEW v1(bookName,price) AS SELECT bookName,price FROM t_book;
```

**5.2 ALTER 语句修改视图**

**          
**语法：

```
ALTER [ALGORITHM ={ UNDEFINED | MERGE | TEMPTABLE }]
     VIEW 视图名 [( 属性清单 )]
     AS SELECT 语句
     [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]；
```

示例：

```
ALTER VIEW v1 AS SELECT * FROM t_book;
```

# 第六节 ：更新视图

更新视图是指通过视图来插入（INSERT）、更新（UPDATE）和删除（DELETE）表中的数据。因为视图是一个虚拟的表，其中没有数据。通过视图更新时，都是转换基本表来更新。更新视图时，只能更新权限范围内的数据。超出了范围，就不能更新。

**6.1 插入\(INSERT\)          
**示例：

```
INSERT INTO v1 VALUES(NULL,'java good',120,'feng',1);
```

**6.2 更新\(UPDATE\)          
**示例：

```
UPDATE v1 SET bookName='java very good',price=200 WHERE id=5;
```

**6.3 删除\(DELETE\)          
**示例：

```
DELETE FROM v1 WHERE id=5;
```

# 第七节 ：删除视图

删除视图是指删除数据库中已存在的视图。删除视图时，只能删除视图的定义，不会删除数据；

语法：

```
DROP VIEW [ IF EXISTS ] 视图名列表 [ RESTRICT | CASCADE ]
```

示例：

```
DROP VIEW IF EXISTS v4;
```



