# 第一节 ：使用 DatabaseMetaData  获取数据库基本信息

# 

DatabaseMetaData 可以得到数据库的一些基本信息，包括数据库的名称、版本，以及得到表的信息。

String getDatabaseProductName\(\) 获取此数据库产品的名称。

int getDriverMajorVersion\(\) 获取此 JDBC 驱动程序的主版本号。

int getDriverMinorVersion\(\) 获取此 JDBC 驱动程序的次版本号。

# 第二节 ：使用 ResultSetMetaData  获取 ResultSet 

ResultSetMetaData 可获取关于 ResultSet 对象中列的基本信息；

int getColumnCount\(\) 返回此 ResultSet 对象中的列数。

String getColumnName\(int column\) 获取指定列的名称。

int getColumnTypeName\(int column\) 获取指定列的 SQL 类型名称。



