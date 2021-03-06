# Mysql-锁

## 不同存储引擎的锁粒度
不同的存储引擎支持的锁粒度是不一样的：

- InnoDB行锁和表锁都支持！

- MyISAM只支持表锁！

InnoDB只有通过索引条件检索数据才使用行级锁，否则，InnoDB将使用表锁
- 也就是说，InnoDB的行锁是基于索引的！

## 表锁
表锁下又分为两种模式：

- 表读锁（Table Read Lock）

- 表写锁（Table Write Lock）

在表读锁和表写锁的环境下：读读不阻塞，读写阻塞，写写阻塞！

- 读读不阻塞：当前用户在读数据，其他的用户也在读数据，不会加锁

- 读写阻塞：当前用户在读数据，其他的用户不能修改当前用户读的数据，会加锁！

- 写写阻塞：当前用户在修改数据，其他的用户不能修改当前用户正在修改的数据，会加锁！


在表读锁和表写锁的环境下：读读不阻塞，读写阻塞，写写阻塞！

- 读读不阻塞：当前用户在读数据，其他的用户也在读数据，不会加锁

- 读写阻塞：当前用户在读数据，其他的用户不能修改当前用户读的数据，会加锁！

- 写写阻塞：当前用户在修改数据，其他的用户不能修改当前用户正在修改的数据，会加锁！


从上面已经看到了：读锁和写锁是互斥的，读写操作是串行。

- 如果某个进程想要获取读锁，同时另外一个进程想要获取写锁。在mysql里边，写锁是优先于读锁的！

- 写锁和读锁优先级的问题是可以通过参数调节的：`max_write_lock_count`和`low-priority-updates`

注意：
> MyISAM可以支持查询和插入操作的并发进行。可以通过系统变量concurrent_insert来指定哪种模式，在MyISAM中它默认是：如果MyISAM表中没有空洞（即表的中间没有被删除的行），MyISAM允许在一个进程读表的同时，另一个进程从表尾插入记录。

> 但是InnoDB存储引擎是不支持的！

## 行锁
Mysql一般是使用InnoDB存储引擎的。InnoDB和MyISAM有两个本质的区别：

- InnoDB支持行锁

- InnoDB支持事务

InnoDB实现了以下两种类型的行锁：
- 共享锁（S锁）：允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁。

    * 也叫做读锁：读锁是共享的，多个客户可以同时读取同一个资源，但不允许其他客户修改。

- 排他锁（X锁)：允许获得排他锁的事务更新数据，阻止其他事务取得相同数据集的共享读锁和排他写锁。

    * 也叫做写锁：写锁是排他的，写锁会阻塞其他的写锁和读锁。

||X	|S|
|------|------|------|
|X|	冲突|	冲突|
|S|	冲突|	兼容|

> 对于UPDATE、DELETE和INSERT语句，InnoDB会自动给涉及数据集加排他锁（X）

> 对于普通SELECT语句，InnoDB不会加任何锁

事务可以通过以下语句显式地给记录集加共享锁或排他锁：

* 共享锁: `SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE`，等同读锁
* 排他锁: `SELECT * FROM table_name WHERE ... FOR UPDATE`，等同写锁

用`SELECT ... IN SHARE MODE`获得共享锁，主要用在需要数据依存关系时来确认某行记录是否存在，并确保没有人对这个记录进行UPDATE或者DELETE操作。但是如果当前事务也需要对该记录进行更新操作，则很有可能造成死锁，对于锁定行记录后需要进行更新操作的应用，应该使用`SELECT... FOR UPDATE`方式获得排他锁。

另外，为了允许行锁和表锁共存，实现多粒度锁机制，InnoDB还有两种内部使用的意向锁（Intention Locks），这两种意向锁都是表锁：

* 意向共享锁（IS）：事务打算给数据行加行共享锁，事务在给一个数据行加共享锁前必须先取得该表的IS锁。

* 意向排他锁（IX）：事务打算给数据行加行排他锁，事务在给一个数据行加排他锁前必须先取得该表的IX锁。

* 意向锁也是数据库隐式帮我们做了，不需要程序员操心！


## 间隙锁GAP
当我们用范围条件检索数据而不是相等条件检索数据，并请求共享或排他锁时，InnoDB会给符合范围条件的已有数据记录的索引项加锁；对于键值在条件范围内但并不存在的记录，叫做“间隙（GAP)”。InnoDB也会对这个“间隙”加锁，这种锁机制就是所谓的间隙锁。

> 值得注意的是：间隙锁只会在Repeatable read隔离级别下使用

InnoDB使用间隙锁的目的有两个：

* 为了防止幻读(上面也说了，Repeatable read隔离级别下再通过GAP锁即可避免了幻读)

* 满足恢复和复制的需要

    * MySQL的恢复机制要求：在一个事务未提交前，其他并发事务不能插入满足其锁定条件的任何记录，也就是不允许出现幻读

## 基础知识
###  乐观锁与悲观锁
乐观锁(Optimistic Lock)，是指操作数据库时(更新操作)，总是认为这次的操作不会导致冲突，不到万不得已不去拿锁，在更新时采取判断是否冲突，适用于读操作远多于更新操作的情况。

乐观锁并没有被数据库实现，需要自行实现，通常的实现方式为在表中增加版本version字段，更新时判断库中version与取出时的version值是否相等，若相等则执行更新并将version加1，若不相等则说明数据被其他线程(进程)修改，放弃修改。
```
select (age, version) from user where id = #{id};
# 其他操作
update user set age = 18, version = version + 1 where id = #{id} and version = #{version} 
```
悲观锁(Pessimistic Lock），是指操作数据库时(更新操作)，总是认为这次的操作会导致冲突，每次都要通过获取锁才能进行数据操作，因此要先确保获取锁成功再进行业务操作。

悲观锁需要数据库自身提供支持，MySql提供了共享锁和排他锁来实现对数据行的锁定

### 乐观锁

- 每次去取数据，都很乐观，觉得不会出现并发问题。

- 因此，访问、处理数据每次都不上锁。

- 但是在更新的时候，再根据版本号或时间戳判断是否有冲突，有则处理，无则提交事务。

### 悲观锁

- 每次去取数据，很悲观，都觉得会被别人修改，会有并发问题。

- 因此，访问、处理数据前就加排他锁。

- 在整个数据处理过程中锁定数据，事务提交或回滚后才释放锁.

### 锁粒度
- 表锁： 开销小，加锁快；锁定力度大，发生锁冲突概率高，并发度最低;不会出现死锁。

- 行锁： 开销大，加锁慢；会出现死锁；锁定粒度小，发生锁冲突的概率低，并发度高。

- 页锁： 开销和加锁速度介于表锁和行锁之间；会出现死锁；锁定粒度介于表锁和行锁之间，并发度一般

### 兼容性
* 共享锁：

    - 又称读锁（S锁）。

    - 一个事务获取了共享锁，其他事务可以获取共享锁，不能获取排他锁，其他事务可以进行读操作，不能进行写操作。

    - `SELECT ... LOCK IN SHARE MODE `显示加共享锁。

* 排他锁：

    - 又称写锁（X锁）。

    - 如果事务T对数据A加上排他锁后，则其他事务不能再对A加任任何类型的封锁。获准排他锁的事务既能读数据，又能修改数据。

    - `SELECT ... FOR UPDATE` 显示添加排他锁。

### 锁模式
- 记录锁： 在行相应的索引记录上的锁，锁定一个行记录

- gap锁： 是在索引记录间歇上的锁,锁定一个区间

- next-key锁： 是记录锁和在此索引记录之前的gap上的锁的结合，锁定行记录+区间。

- 意向锁 是为了支持多种粒度锁同时存在；