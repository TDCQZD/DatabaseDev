# RocksDB
RocksDB是使用C++编写的嵌入式kv存储引擎，其键值均允许使用二进制流。由Facebook基于levelDB开发， 提供向后兼容的levelDB API。

RocksDB针对Flash存储进行优化，延迟极小。RocksDB使用LSM存储引擎，纯C++编写。Java版本RocksJava正在开发中

RocksDB依靠大量灵活的配置，使之能针对不同的生产环境进行调优，包括直接使用内存，使用Flash，使用硬盘或者HDFS。支持使用不同的压缩算法，并且有一套完整的工具供生产和调试使用。
## 特性
- 高性能
    
    RocksDB使用一套日志结构的数据库引擎，为了更好的性能，这套引擎是用C++编写的。 Key和value是任意大小的字节流。

- 为快速存储而优化

    RocksDB为快速而又低延迟的存储设备（例如闪存或者高速硬盘）而特殊优化处理。 RocksDB将最大限度的发挥闪存和RAM的高度率读写性能。


- 可适配性

    RocksDB适合于多种不同工作量类型。 从像MyRocks这样的数据存储引擎， 到应用数据缓存, 甚至是一些嵌入式工作量，RocksDB都可以从容面对这些不同的数据工作量需求。

-  基础和高级的数据库操作

    RocksDB提供了一些基础的操作，例如打开和关闭数据库。 对于合并和压缩过滤等高级操作，也提供了读写支持。
## 功能
- 为需要存储TB级别数据到本地FLASH或者RAM的应用服务器设计
- 针对存储在高速设备的中小键值进行优化——你可以存储在flash或者直接存储在内存
- 性能碎CPU数量线性提升，对多核系统友好

## 参考资料
* https://github.com/facebook/rocksdb
* https://rocksdb.org.cn/