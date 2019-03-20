# 第一节 ：CallableStatement  接口的引入

CallableStatement 主要是调用数据库中的存储过程，CallableStatement 也是 Statement 接口的子接口。在使用CallableStatement 时可以接收存储过程的返回值。

# 第二节 ：使用 CallableStatement  接口调用存储过程

void registerOutParameter\(int parameterIndex, int sqlType\)

按顺序位置 parameterIndex 将 OUT 参数注册为 JDBC 类型 sqlType。

