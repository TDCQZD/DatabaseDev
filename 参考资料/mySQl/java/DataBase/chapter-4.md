**一、显示这个库下的所有表：**

Show tables;

**二、查询表结构：**

1、查看表基本结构

Desc【表名】;

2、查看表详细结构：

Show create table 表名；

**三、删除表：**

Drop table【表名】;

**四、修改表**：

1、修改表名

ALTER TABLE 旧表名 RENMAE 新表名 ；

2、修改字段

ALTER TABLE 表名 CHANGE 旧属性名 新属性名 新数据类型

3、增加字段

ALTER TABLE 表名 ADD 属性名 1 数据类型 \[完整性约束条件\] \[FIRST \|AFTER 属性名 2\]

4、删除字段

ALTER TABLE 表名 DROP 属性名

```
desc t_bookType;
```

```
show create table t_bookType;

alter table t_book rename t_book2;//重命名表


alter table t_book change bookName bookName2 varchar(20);//

alter table t_book add testField int first ;

alter table t_book drop testField;

drop table t_bookType;

drop table t_book;
```

**五、创建数据库表：**

表是数据库存储数据的基本单位。个一个表包含若干字段或记录；

语法：

```
CREATE TABLE 表名( 属性名 数据类型 [完整性约束条件],
                   属性名 数据类型 [完整性约束条件],
                   .
                   .
                  属性名 数据表格 [完整性约束条件]
                );
```

| 约束条件 | 说明 |
| :--- | :--- |
| PRIMARYKEY | 标识该属性为该表的主键，可以唯一的标识对应的记录 |
| FOREIGN KEY | 标识该属性为该表的外键，与某表的主键关联 |
| NOT NULL | 标识该属性不能为空 |
| UNIQUE | 标识该属性的值是唯一的 |
| AUTO\_INCREMENT | 标识该属性的值自动增加 |
| DEFAULT | 为该属性设置默认值 |

创建图书类别表：t\_bookType

```
CREATE TABLE t_bookType(
    id int primary key auto_increment,
    bookTypeName varchar(20),
    bookTypeDesc varchar(200)
);
```

创建图书表：t\_book

    CREATE TABLE t_book(
        id int primary key auto_increment,
        bookName varchar(20),
        author varchar(10),
        price decimal(6,2),
        bookTypeId int,
        constraint `fk` foreign key (`bookTypeId`) references `t_bookType`(`id`)
    );



