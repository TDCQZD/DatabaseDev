# 第一节 ：ResultSet  结果集的引入

当我们查询数据库时，返回的是一个二维的结果集，我们这时候需要使用 ResultSet 来遍历结果集，获取每一行的数据。

# 第二节 ：使用 ResultSet  遍历查询结果

boolean next\(\) 将光标从当前位置向前移一行。

String getString\(int columnIndex\) 以 Java 编程语言中 String 的形式获取此 ResultSet 对象的当前行中指定列的值。

String getString\(String columnLabel\) 以 Java 编程语言中 String 的形式获取此 ResultSet 对象的当前行中指定列的值。



