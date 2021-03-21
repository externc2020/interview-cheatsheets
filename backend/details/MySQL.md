# MySQL

- 5.5之前默认MyISAM，不支持事务，行级锁
- 事务的四大特性：原子性（Atomicity）一致性（Consistency）隔离性（Isolation）持久性（Durability）

### InnoDB ACID 实现原理

- 原子性：undo log
- 持久性：redo log
- 隔离性：锁 + MVCC
- 一致性：保证原子性、持久性和隔离性，如果这些特性无法保证，事务的一致性也无法保证；数据库本身提供保障，例如不允许向整形列插入字符串值、字符串长度不能超过列的限制等；应用层面进行保障，例如如果转账操作只扣除转账者的余额，而没有增加接收者的余额，无论数据库实现的多么完美，也无法保证状态的一致。

### InnoDB 锁

- 共享锁,读锁
- 排他锁,写锁
- 行级锁：算法 Record Lock, Gap Lock, Next-Key Lock
- 意向锁

### 索引

- 聚簇索引（Clustered Index)：数据存储在索引中，叶子节点保存记录，InnoDB
- 非聚簇索引（Secondary Index）：叶子节点保存主键值：MyISAM
- 覆盖索引：查询字段就在索引中，不用“回表”
- 最左前缀：判断会不会走索引
- 数据结构：哈希表，B 树，B+ 树
- 组合索引 `(A,B,C)`
- B vs B+ 树，B+ 非叶子节点没有数据，可以放更多指针 1170；B+ 叶子节点双链表，方便范围查询

### 多版本并发控制（MVCC）

- MVCC 可以保证不阻塞地读到一致的数据。但是，MVCC 并没有对实现细节做约束，为此不同的数据库的语义有所不同
- tx-id, roll pointer, 回滚日志
- 一致性视图 read view: `[100, 101, 102], 200`, min-id, max-id
- READ_COMMITED: 每次读都创建 read view，REPEATABLE_READ：一直使用第一次创建的 read view

### Buffer Pool

- 从磁盘加载页进入内存
- 淘汰机制
- 写日志
- 异步，批量刷盘

### 日志

- 重做日志(redo log)
- 回滚日志（undo log）
- 二进制日志（binlog）
- 错误日志（errorlog）
- 慢查询日志（slow query log）
- 一般查询日志（general log）
- 中继日志（relay log）

### 常见 SQL

- 双重否定
- 翻转

### B+ 树

- 2000 万条数据 3 层：InnoDB 页大小 16k（16384），主见 bigint 8 字节，指针 6 字节，共 14 字节，一页可以放 16384/14=1170 节点，假设一条数据 1K，一页可以放 16 条。所以两层树可以放 1170 * 16 = 18720 条，三层树可以放 1170 * 1170 * 16 = 21902400 条
- 按照页做索引
