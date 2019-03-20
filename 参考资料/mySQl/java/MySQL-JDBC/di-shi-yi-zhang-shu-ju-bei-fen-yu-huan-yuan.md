# 第一节 ：数据备份

备份数据可以保证数据库中数据的安全，数据库管理员需要定期的进行数据库备份；

**1.1 使用 mysqldump 命令备份**

```
mysqldump -u username -p dbname table1 table2 ... > BackupName.sql
```

dbname 参数表示数据库的名称；table1 和 table2 参数表示表的名称，没有该参数时将备份整个数据库；

BackupName.sql 参数表示备份文件的名称，文件名前面可以加上一个绝对路径。通常以 sql 作为后缀。

**1.2 使用 sqlyog 图形工具备份**

# 第二节 ：数据还原

**2.2 使用 mysql 命令还原**

```
Mysql -u root -p [dbname] < backup.sql
```

dbname 参数表示数据库名称。该参数是可选参数，可以指定数据库名，也可以不指定。指定数据库名时，表

示还原该数据库下的表。不指定数据库名时，表示还原特定的一个数据库。而备份文件中有创建数据库的语句。

**2.3 使用 sqlyog 图形工具还原**



