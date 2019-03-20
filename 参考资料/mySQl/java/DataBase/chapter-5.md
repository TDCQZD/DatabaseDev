# **一、查询数据**

### 第一类：单表查询

数据库表创建和添加数据：

    create table `t_student` (
        `id` double ,
        `stuName` varchar (60),
        `age` double ,
        `sex` varchar (30),
        `gradeName` varchar (60)
    ); 
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('1','张三','23','男','一年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('2','张三丰','25','男','二年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('3','李四','23','男','一年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('4','王五','22','男','三年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('5','珍妮','21','女','一年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('6','李娜','26','女','二年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('7','王峰','20','男','三年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('8','梦娜','21','女','二年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('9','小黑','22','男','一年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('10','追风','25','男','二年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('11','小小张三','21',NULL,'二年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('12','小张三','23','男','二年级');
    insert into `t_student` (`id`, `stuName`, `age`, `sex`, `gradeName`) values('13','张三锋小','24',NULL,'二年级');

**1.1 功能：查询表所有数据**

语法：

```
1，SELECT 字段 1，字段 2，字段 3...FROM 表名；安照指定数据字段查询表
2，SELECT * FROM 表名
```

示例代码：

```
/* 按照指定数据字段顺序查询表全部数据*/
SELECT id,stuName,age,sex,gradeName FROM t_student ;

SELECT stuName,id,age,sex,gradeName FROM t_student ;

/* 按照默认建表数据字段查询全部数据*/
SELECT * FROM t_student;
```

**1.2 功能：查询指定字段**

语法：

```
SELECT 字段 1，字段 2，字段 3...FROM 表名；
```

示例代码：

```
/* 按照指定数据字段顺序查询表指定数据*/
SELECT stuName,gradeName FROM t_student;
```

**1.3 功能：Where 条件查询**

语法：

```
SELECT 字段 1，字段 2，字段 3...FROM 表名 WHERE 条件表达式；
```

示例代码：

```
SELECT * FROM t_student WHERE id=1;

SELECT * FROM t_student WHERE age>22;
```

**1.4 功能：带 IN 关键字查询**

语法：

```
SELECT 字段 1，字段 2，字段 3...FROM 表名 WHERE 字段 [NOT] IN (元素 1，元素 2，元素 3)；
```

示例代码：

```
SELECT * FROM t_student WHERE age IN (21,23);
SELECT * FROM t_student WHERE age NOT IN (21,23);
```

**1.5 功能：带 BETWEENAND 的范围查询**

语法：

```
SELECT 字段 1，字段 2，字段 3...FROM 表名 WHERE 字段 [NOT] BETWEEN 取值 1 AND 取值 2；
```

示例代码：

```
SELECT * FROM t_student WHERE age BETWEEN 21 AND 24;
SELECT * FROM t_student WHERE age NOT BETWEEN 21 AND 24;
```

**1.6 功能：带 LIKE 的模糊查询**

语法：

```
SELECT 字段 1，字段 2，字段 3...FROM 表名 WHERE 字段 [NOT] LIKE ‘字符串’；

“%”代表任意字符；
“_” 代表单个字符；
```

示例代码：

```
SELECT * FROM t_student WHERE stuName LIKE '张三';
SELECT * FROM t_student WHERE stuName LIKE '张三%';
SELECT * FROM t_student WHERE stuName LIKE '张三__';
SELECT * FROM t_student WHERE stuName LIKE '%张三%';
```

**1.7 功能：空值查询**

语法：

```
SELECT 字段 1，字段 2，字段 3...FROM 表名 WHERE 字段 IS [NOT] NULL；
```

示例代码：

```
SELECT * FROM t_student WHERE sex IS NULL;
SELECT * FROM t_student WHERE sex IS NOT NULL;
```

**1.8 功能：带 AND 的多条件查询**

语法：

```
SELECT 字段 1，字段 2...FROM 表名 WHERE 条件表达式 1 AND 条件表达式 2 [...AND 条件表达式 n]
```

示例代码：

```
SELECT * FROM t_student WHERE gradeName='一年级' AND age=23
```

**1.9 功能：带 OR 的多条件查询**

语法：

```
SELECT 字段 1，字段 2...FROM 表名 WHERE 条件表达式 1 OR 条件表达式 2 [...OR 条件表达式 n]
```

示例代码：

```
SELECT * FROM t_student WHERE gradeName='一年级' OR age=23
```

**1.10 功能：DISTINCT 去重复查询**

语法：

```
SELECT DISTINCT 字段名 FROM 表名；
```

示例代码：

```
SELECT DISTINCT gradeName FROM t_student;
```

**1.11 功能： 对查询结果排序**

语法：

```
SELECT 字段 1，字段 2...FROM 表名 ORDER BY 属性名 [ASC|DESC]

ASC:顺序，从小到大
DESC:逆序，从大到小
```

示例代码：

```
SELECT * FROM t_student ORDER BY age ASC;
SELECT * FROM t_student ORDER BY age DESC;
```

**1.12 功能：GROUP BY 分组查询**

语法：

```
GROUP BY 属性名 [HAVING 条件表达式][WITH ROLLUP]

1，单独使用(毫无意义)；
2，与 GROUP_CONCAT()函数一起使用；
3，与聚合函数一起使用；
4，与 HAVING 一起使用(限制输出的结果)；
5，与 WITH ROLLUP 一起使用(最后加入一个总和行)；
```

示例代码：

```
SELECT * FROM t_student GROUP BY gradeName;

SELECT gradeName,GROUP_CONCAT(stuName) FROM t_student GROUP BY gradeName;

SELECT gradeName,COUNT(stuName) FROM t_student GROUP BY gradeName;

SELECT gradeName,COUNT(stuName) FROM t_student GROUP BY gradeName HAVING COUNT(stuName)>3;

SELECT gradeName,COUNT(stuName) FROM t_student GROUP BY gradeName WITH ROLLUP;
SELECT gradeName,GROUP_CONCAT(stuName) FROM t_student GROUP BY gradeName WITH ROLLUP;
```

**1.13功能： LIMIT 分页查询**

语法：

```
SELECT 字段 1，字段 2...FROM 表名 LIMIT 初始位置，记录数；
```

示例代码：

```
SELECT * FROM t_student LIMIT 0,5;
SELECT * FROM t_student LIMIT 5,5;
SELECT * FROM t_student LIMIT 10,5;
```

### 第二类 ：使用聚合函数查询

数据库表创建和添加数据：

    create table `t_grade` (
        `id` int ,
        `stuName` varchar (60),
        `course` varchar (60),
        `score` int 
    ); 
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('1','张三','语文','91');
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('2','张三','数学','90');
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('3','张三','英语','87');
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('4','李四','语文','79');
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('5','李四','数学','95');
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('6','李四','英语','80');
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('7','王五','语文','77');
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('8','王五','数学','81');
    insert into `t_grade` (`id`, `stuName`, `course`, `score`) values('9','王五','英语','89');

**2.1  COUNT\(\)函数**

**函数功能**：

1、COUNT\(\)函数用来统计记录的条数；

2、与 GOUPE BY 关键字一起使用；

示例代码：

```
SELECT COUNT(*) FROM t_grade;

SELECT COUNT(*) AS total FROM t_grade;

SELECT stuName,COUNT(*) FROM t_grade GROUP BY stuName;
```

注：AS total 对当前的查询结果起别名。

**2.2  SUN\(\)函数**

**函数功能**：

1、SUM\(\)函数是求和函数；

2、与 GOUPE BY 关键字一起使用；

示例代码：

```
SELECT stuName,SUM(score) FROM t_grade WHERE stuName="张三";

SELECT stuName,SUM(score) FROM t_grade GROUP BY stuName;
```

**2.3  AVG\(\)函数**

**函数功能**：

1、AVG\(\)函数是求平均值的函数；

2、与 GOUPE BY 关键字一起使用；

示例代码：

```
SELECT stuName,AVG(score) FROM t_grade WHERE stuName="张三";

SELECT stuName,AVG(score) FROM t_grade GROUP BY stuName;
```

**2.4  MAX\(\)函数**

**函数功能**：

1、MAX\(\)函数是求最大值的函数；

2、与 GOUPE BY 关键字一起使用；

示例代码：

```
SELECT stuName,course,MAX(score) FROM t_grade WHERE stuName="张三";

SELECT stuName,MAX(score) FROM t_grade GROUP BY stuName;
```

**2.5  MIN\(\)函数**

**函数功能**：

1、MIN\(\)函数是求最小值的函数；

2、与 GOUPE BY 关键字一起使用；

示例代码：

```
SELECT stuName,course,MIN(score) FROM t_grade WHERE stuName="张三";

SELECT stuName,MIN(score) FROM t_grade GROUP BY stuName;
```

### 第三类：连接查询

连接查询是将两个或两个以上的表按照某个条件连接起来，从中选取需要的数据；

数据库表创建和添加数据：

    CREATE TABLE `t_book` (
      `id` int(11) NOT NULL AUTO_INCREMENT,
      `bookName` varchar(20) DEFAULT NULL,
      `price` decimal(6,2) DEFAULT NULL,
      `author` varchar(20) DEFAULT NULL,
      `bookTypeId` int(11) DEFAULT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;



    insert  into `t_book`(`id`,`bookName`,`price`,`author`,`bookTypeId`) values (1,'Java编程思想','100.00','埃史尔',1),(2,'Java从入门到精通','80.00','李钟尉',1),(3,'三剑客','70.00','大仲马',2),(4,'生理学(第二版)','24.00','刘先国',4);


    DROP TABLE IF EXISTS `t_booktype`;

    CREATE TABLE `t_booktype` (
      `id` int(11) NOT NULL AUTO_INCREMENT,
      `bookTypeName` varchar(20) DEFAULT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;


    insert  into `t_booktype`(`id`,`bookTypeName`) values (1,'计算机类'),(2,'文学类'),(3,'教育类');

示例代码：

```
SELECT * FROM t_book,t_bookType;
```

**3.1内连接查询                          
**

内连接查询是一种最常用的连接查询。内连接查询可以查询两个或者两个以上的表。

注：学习笛卡尔乘积。

示例代码：

```
SELECT * FROM t_book,t_bookType WHERE t_book.bookTypeId=t_bookType.id;
```

**3.2外连接查询                          
**

外连接可以查出某一张表的所有信息。

```
SELECT 属性名列表 FROM 表名 1 LEFT|RIGHT JOIN 表名 2 ON 表名 1.属性名 1=表名 2.属性名 2；
```

**3.2.1 左连接查询                          
**

可以查询出“表名 1”的所有记录，而“表名 2”中，只能查询出匹配的记录。

示例代码：

```
/* 外连接查询 左 只查询表一*/
SELECT * FROM t_book LEFT JOIN t_bookType ON t_book.bookTypeId=t_bookType.id;

SELECT tb.bookName,tb.author,tby.bookTypeName FROM t_book tb LEFT JOIN t_bookType tby ON tb.bookTypeId=tby.id;
```

**3.2.2 右连接查询                          
**

可以查询出“表名 1”的所有记录，而“表名 2”中，只能查询出匹配的记录。

示例代码：

```
/*外连接查询 右 只查询表二*/
SELECT * FROM t_book RIGHT JOIN t_bookType ON t_book.bookTypeId=t_bookType.id;

SELECT tb.bookName,tb.author,tby.bookTypeName FROM t_book tb RIGHT JOIN t_bookType tby ON tb.bookTypeId=tby.id;
```

**3.3多条件连接查询**

可以查询出“表名 2”的所有记录，而“表名 1”中，只能查询出匹配的记录。

示例代码：

```
SELECT tb.bookName,tb.author,tby.bookTypeName FROM t_book tb RIGHT JOIN t_bookType tby ON tb.bookTypeId=tby.id;


SELECT tb.bookName,tb.author,tby.bookTypeName FROM t_book tb,t_bookType tby WHERE tb.bookTypeId=tby.id AND tb.price>70;
```

### 第四类 ：子查询

**4.1  带 In  关键字的子查询                        
**

一个查询语句的条件可能落在另一个 SELECT 语句的查询结果中。

示例代码：

```
SELECT * FROM t_book WHERE booktypeId IN (SELECT id FROM t_booktype);

SELECT * FROM t_book WHERE booktypeId NOT IN (SELECT id FROM t_booktype);
```

**4.2  带比较运算符的子查询**

子查询可以使用比较运算符。

示例代码：

```
SELECT * FROM t_book WHERE price>=(SELECT price FROM t_pricelevel WHERE priceLevel=1);
```

**4.3 带  Exists  关键字的子查询                        
**

假如子查询查询到记录，则进行外层查询，否则，不执行外层查询；

示例代码：

```
SELECT * FROM t_book WHERE EXISTS (SELECT * FROM t_booktype);

SELECT * FROM t_book WHERE NOT EXISTS (SELECT * FROM t_booktype);
```

**4.4 带  Any  关键字的子查询                        
**

ANY 关键字表示满足其中任一条件；

示例代码：

```
SELECT * FROM t_book WHERE price>= ANY (SELECT price FROM t_pricelevel);
```

**4.5 带  All  关键字的子查询                        
**

ALL 关键字表示满足所有条件；

示例代码：

```
SELECT * FROM t_book WHERE price>= ALL (SELECT price FROM t_pricelevel);
```

### 第五类 ：合并查询结果

```
SELECT id FROM t_book;

SELECT id FROM t_booktype;
```

**5.1 UNION                      
**

使用 UNION 关键字是，数据库系统会将所有的查询结果合并到一起，然后去除掉相同的记录；

示例代码：

```
SELECT id FROM t_book UNION SELECT id FROM t_booktype;
```

**5.2 UNION ALL**

使用 UNION ALL，不会去除掉系统的记录；

示例代码：

```
SELECT id FROM t_book UNION ALL SELECT id FROM t_booktype;
```

### 第六类：为表和字段取别名

**6.1 为表取别名                      
**

语法：

```
格式： 表名 表的别名
```

示例代码：

```
SELECT * FROM t_book WHERE id=1;

SELECT * FROM t_book t WHERE t.id=1;

SELECT t.bookName FROM t_book t WHERE t.id=1
```

**6.2 为字段取别名**

语法：

```
格式： 属性名 [AS] 别名
```

示例代码：

```
SELECT t.bookName bName FROM t_book t WHERE t.id=1;

SELECT t.bookName AS bName FROM t_book t WHERE t.id=1;
```

# **二、添加数据**

**1、给表的所有字段插入数据              
**

语法：

```
INSERT INTO 表名 VALUES(值 1，值 2，值 3，...，值 n)；
```

示例：

```
INSERT INTO t_book VALUES(NULL,'我爱我家',20,'张三',1);
```

**2、给表的指定字段插入数据              
**

语法：

```
INSERT INTO 表名(属性 1，属性 2，...，属性 n) VALUES(值 1，值 2，值 3，...，值 n)；
```

示例：

```
INSERT INTO t_book(id,bookName,price,author,bookTypeId) VALUES(NULL,'我爱我家',20,'张三',1);

INSERT INTO t_book(bookName,author) VALUES('我爱我家','张三');
```

**3、同时插入多条记录**

语法：

```
INSERT INTO 表名 [(属性列表)]
            VALUES(取值列表 1)，(取值列表 2)
              ...，
            (取值列表 n)；
```

示例：

```
INSERT INTO t_book(id,bookName,price,author,bookTypeId) VALUES (NULL,'我爱我家2',20,'张三',1),(NULL,'我爱我家3',20,'张三',1);
```

# **三、删除数据**

语法：

```
UPDATE 表名
        SET 属性名 1=取值 1，属性名 2=取值 2，
         ...，
        属性名 n=取值 n
        WHERE 条件表达式；
```

示例：

```
DELETE FROM t_book WHERE id=5;

DELETE FROM t_book WHERE bookName='我';
```

# **四、修改数据**

语法：

```
DELETE FROM 表名 [WHERE 条件表达式]
```

示例：

```
UPDATE t_book SET bookName='Java编程思想',price=120 WHERE id=1;

UPDATE t_book SET bookName='我' WHERE bookName LIKE '%我爱我家%';
```



