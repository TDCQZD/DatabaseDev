# 第一节 ：JDBC  连接数据库步骤

**第一步：加载驱动；**



**第二步：连接数据库；**

**第三步：使用语句操作数据库；**

**第四步：关闭数据库连接，释放资源；**

# 第二节 ：在项目里配置数据库驱动

右击项目 -&gt; Build Path -&gt; Configure Build Path -&gt; Add Exteranl JARs...

# 第三节 ：加载数据驱动

Mysql 驱动名：com.mysql.jdbc.Driver

加载方式： Class.forName\(驱动名\)； Ctrl+1

# 第四节：连接及关闭数据库

1、DriverManager 驱动管理类，主要负责获取一个数据库的连接；

static Connection getConnection\(String url, String user, String password\) 试图建立到给定数据库 URL 的连

接。

2、MySQL 数据库的连接地址格式

jdbc:mysql://IP 地址:端口号/数据库名称

jdbc 协议：JDBC URL 中的协议总是 jdbc ；

子协议：驱动程序名或数据库连接机制（这种机制可由一个或多个驱动程序支持）的名称，如 mysql；

子名称：一种标识数据库的方法。必须遵循“//主机名：端口/子协议”的标准 URL 命名约定，如

//localhost:3306/db\_book

3、Connection 接口 与特定数据库的连接（会话）。

void close\(\)

立即释放此 Connection 对象的数据库和 JDBC 资源，而不是等待它们被自动释放。

