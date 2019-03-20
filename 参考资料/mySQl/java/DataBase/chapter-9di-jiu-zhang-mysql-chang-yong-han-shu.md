# **第一节 ：日期和时间函数**

**1、CURDATE\(\) 返回当前日期；**

**2、CURTIME\(\) 返回当前时间；**

**3、MONTH\(d\) 返回日期 d 中的月份值，范围是 1~12**

示例代码：

```
SELECT CURDATE(),CURTIME(),MONTH(birthday) AS m FROM t_function;
```

# **第二节 ：字符串函数**

**1、CHAR\_LENGTH\(s\) 计算字符串 s 的字符数；**

**2、UPPER\(s\) 把所有字母变成大写字母；**

**3、LOWER\(s\) 把所有字母变成小写字母**

示例代码：

```
SELECT userName,CHAR_LENGTH(userName),UPPER(userName),LOWER(userName) FROM t_function;
```

# **第三节 ：数学函数**

**1、ABS\(x\) 求绝对值**

**2、SQRT\(x\) 求平方根**

**3、MOD\(x,y\) 求余**

示例代码：

```
SELECT num,ABS(num) FROM t_function;

SELECT SQRT(4),MOD(9,4) FROM t_function;
```

# **第四节 ：加密函数**

**1、PASSWORD\(str\) 一般对用户的密码加密 不可逆**

**2、MD5\(str\) 普通加密 不可逆**

**3、ENCODE\(str，pswd\_str\) 加密函数，结果是一个二进制数，必须使用 BLOB 类型的字段来保存它；**

**4、DECODE\(crypt\_str，pswd\_str\) 解密函数；**

示例代码：

```
INSERT INTO t_function VALUES(NULL,'2013-1-1','a',1,PASSWORD('123456'));

INSERT INTO t_function VALUES(NULL,'2013-1-1','a',1,MD5('123456'));

INSERT INTO t_function VALUES(NULL,'2013-1-1','a',1,MD5('123456'),ENCODE('abcd','aa'));

SELECT DECODE(pp,'aa') FROM t_function WHERE id=5;
```



