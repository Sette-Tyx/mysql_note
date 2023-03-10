## **表锁**

锁一整张表，开销最小的策略；

## 行锁

最大开销的锁

InnoDB 目前处理死锁的方式就是，将持有最小行级排他锁的事务进行回滚。

## 事务日志

使用事务日志，只需要引擎修改内存上的拷贝数据之后，然后将这个行为记录到持久在硬盘上的事务日志。事务日志持久之后，可以在后台慢慢刷数据。这种行为叫预写式日志，修改数据需要写两次磁盘。

## MVCC

InnoDB的MVCC，是在每行后面保存两个列来实现的，一个是创建时间，一个是过期时间（删除时间）。存放的不是确切的时间值，而是系统版本号。每开始一个事务，系统的版本号就会递增。

## 如何优化

1）选择简单的数据类型

int比string简单，因为string还有字符之间的排序校准

2）使用内建的数据类型

用datatime 比 使用string存储日期更合适

3）尽量避免NULL

“可为NULL”列，使得索引、索引统计和值比较都会比较麻烦。而且“可为NULL”列会使用更多的存储空间，因为在索引时，会有额外一列来记录。

varchar 和 char比较

varchar是不定长的字符串，需要1or2个字节进行存储来记录长度

char是定长的，比如如果只存1个字节的，char会更好；或者字符长度变化不大的时候，char也挺好，因为不会产生碎片。

## 加快ALTER TABLE操作的速度

MysQL执行大部分修改表 结构操作的方法是用新的结构创建 一个空表，从旧表中查出所有数据插入新表，然后删 除旧表。