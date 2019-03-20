# 第一节 ：索引的引入

索引定义：索引是由数据库表中一列或者多列组合而成，其作用是提高对表中数据的查询速度；类似于图书的目录，方便快速定位，寻找指定的内容；

# 第二节 ：索引的优缺点

优点：提高查询数据的速度；

缺点：创建和维护索引的时间增加了；

# 第三节 ：索引实例

# 第四节 ：索引分类

**1、普通索引**

这类索引可以创建在任何数据类型中；

**2、唯一性索引**

使用 UNIQUE 参数可以设置，在创建唯一性索引时，限制该索引的值必须是唯一的；

**3、全文索引**

使用 FULLTEXT 参数可以设置，全文索引只能创建在 CHAR，VARCHAR，TEXT 类型的字段上。主要作用就是提高查询较大字符串类型的速度；只有 MyISAM 引擎支持该索引，Mysql 默认引擎不支持；

主要用于文本。

**4、单列索引**

在表中可以给单个字段创建索引，单列索引可以是普通索引，也可以是唯一性索引，还可以是全文索引；

**5、多列索引**

多列索引是在表的多个字段上创建一个索引；

**6、空间索引**

使用 SPATIAL 参数可以设置空间索引。空间索引只能建立在空间数据类型上，这样可以提高系统获取空间数据的效率；只有 MyISAM 引擎支持该索引，Mysql 默认引擎不支持；

# 第五节 ：创建索引

**5.1 创建表的时候创建索引    **

**5.1 1、创建普通索引**

示例：

```
CREATE TABLE t_user1(id INT ,
                     userName VARCHAR(20),
                     PASSWORD VARCHAR(20),
                     INDEX (userName)
                 );
```

**5.1 2、创建唯一性索引**

关键字：UNIQUE

示例：

```
/*
创建表的时候创建唯一性索引 UNIQUE
*/             
CREATE TABLE t_user2(id INT ,
                     userName VARCHAR(20),
                     PASSWORD VARCHAR(20),
                     UNIQUE INDEX index_userName(userName)
                 );
```

**5.1 3、创建全文索引**

MySQL默认引擎不支持

**5.1 4、创建单列索引**

示例：

```
CREATE TABLE t_user2(id INT ,
                     userName VARCHAR(20),
                     PASSWORD VARCHAR(20),
                     INDEX index_userName(userName)
                 );
```

**5.1 5、创建多列索引**

示例：

```
/*
创建表的时候创建多列索引 userName PASSWORD
*/          
CREATE TABLE t_user3(id INT ,
                     userName VARCHAR(20),
                     PASSWORD VARCHAR(20),
                     INDEX index_userName_password(userName,PASSWORD)
                 );
```

**5.1 6、创建空间索引**

MySQL默认引擎不支持

**5.2 在已经存在的表上创建索引              
**

语法：

```
CREATE [ UNIQUE | FULLTEXT | SPATIAL ] INDEX 索引名
ON 表名 (属性名 [(长度)] [ASC | DESC])；
```

示例：

    /*
    在已经存在的表上创建索引   
    */  
    DROP TABLE IF EXISTS `t_user4`;

    CREATE TABLE t_user4(id INT NOT NULL,
                         userName VARCHAR(20),
                         PASSWORD VARCHAR(20)                  
                     );

    CREATE     INDEX index_userName ON t_user4(userName);

    DROP INDEX index_userName ON t_user4;

    /*
    在已经存在的表上创建唯一性索引 UNIQUE
    */
    CREATE     UNIQUE INDEX index_userName ON t_user4(userName);
    /*
    在已经存在的表上创建多列索引
    */
    CREATE  INDEX index_userName_password ON t_user4(userName,PASSWORD);

**5.3 用 ALTER TABLE 语句来创建索引              
**

语法：

```
ALTER TABLE 表名 ADD [ UNIQUE | FULLTEXT | SPATIAL ] INDEX
索引名 (属性名 [(长度)] [ ASC | DESC])
```

示例：

```
/*
用 ALTER TABLE 语句来创建索引  
*/
CREATE TABLE t_user5(id INT ,
                     userName VARCHAR(20),
                     PASSWORD VARCHAR(20)                
                 );

ALTER TABLE t_user5 ADD INDEX index_userName(userName);

ALTER TABLE t_user5 ADD UNIQUE INDEX index_userName(userName);

ALTER TABLE t_user5 ADD INDEX index_userName_password(userName,PASSWORD);
```

# 第六节 ：删除索引

语法：

```
DROP INDEX 索引名 ON 表名 ；
```

示例：

```
/*
删除索引
*/
DROP INDEX index_userName ON t_user5;

DROP INDEX index_userName_password ON t_user5;
```

# 



