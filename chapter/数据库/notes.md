**MYSQL的A C I D怎样实现的？**

原子性：事务是一个不可分割的工作单位，要么全部执行完毕，要么不执行。

一致性：事务执行前后，数据处于一种符合逻辑的合法状态。

隔离性：多个事务并发执行的时候，事务内部的操作与其他事务是隔离的，并发执行的各个事务之间不能互相干扰。

持久性：事务一旦提交，对数据库的改变是永久的。其他操作和故障对其不应有任何影响。


原子性：

利用innodb的undo log，回滚日志。当事务回滚时能撤销所有已经执行成功的sql语句。

undo log记录了这些回滚需要的信息，当事务执行失败或调用了rollback，导致事务需要回滚，便可以利用undo log中的信息将数据回滚到修改之前的样子。


一致性：数据库必须要实现AID三大特性，才有可能实现一致性。例如，原子性无法保证，显然一致性也无法保证。



隔离性：采用锁和mvcc机制。


持久性：

利用innodb的redo log，Mysql对数据进行修改时，是先将数据加载到内存中，然后在内存中对数据进行修改。再刷入磁盘，如果此时突然宕机，内存中的数据就会丢失。 -----事务提交前把数据写入磁盘。

只修改一个页面里的一个字节，就要将整个页面刷入磁盘，太浪费资源了。

一个事务里的SQL可能牵涉到多个数据页的修改，而这些数据页可能不是相邻的，也就是属于随机IO。显然操作随机IO，速度会比较慢。

所以采用redo log的机制，redo log进行刷盘比对数据页刷盘效率高

redo log体积小，毕竟只记录了哪一页修改了啥，因此体积小，刷盘快。

redo log是一直往末尾进行追加，属于顺序IO。效率显然比随机IO来的快。


MVCC:

每个事务开启的时候，创建一个一致性视图。

每个事务开启的时候，都会向innodb事务系统申请一个事务id，这个id是唯一、严格递增的。

数据库当中的一条记录，可能会有多个版本。每个版本都有一个rowid。


	




