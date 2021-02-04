# 隔离级别

#### READ UNCOMMITTED(未提交读)  
当前事务可以读到其他事务未提交的数据（脏读）

#### READ COMMITTED(提交读)
当前事务只能看到其他事务已经提交的修改。也叫做不可重复读（nonrepeatable read)[^1]。

#### REPEATABLE READ(可重复读)
当前事务多次读取同一记录的结果是一样的，解决了脏读和不可重复读的问题。理论上无法解决幻读[^2]的问题。

#### SERIALIZABLE(串行化)
强制事务串行执行，简单来说会在读取的每一行数据上加锁。

#### 不同隔离级别可能会出现的问题
隔离级别|脏读|不可重复读|幻读|加锁读
-|-|-|-
READ UNCOMMITTED|yes|yes|yes|no
READ COMMITTED|no|yes|yes|no
REPEATABLE READ|no|no|yes|no
SERIALIZABLE|no|no|no|yes

[^1]不可重复读: 一个事务在前后读到的同一条数据可能不一致。例如: tx1(select) -> tx2(update) -> tx1(select)（假设均操作同一条数据), tx1第二次select查到的是被tx2修改后的结果，和第一次select的结果不同。

[^2] 幻读: 事务读物某个范围内的记录是，另一个记录在该范围内插入/删除新的记录。
     例如select * from log where guid > 10; 而另一个事务insert了一条guid>10的记录导致第二次查询多出一条记录
     
