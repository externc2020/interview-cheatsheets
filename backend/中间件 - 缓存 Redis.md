缓存的实现原理，设计缓存要注意什么
redis aof rdb

Redis的内存用完了会发生什么？lru
lfu

集群 Gossip 协议

分布式锁

雪崩
缓存穿透
bloomfilter bitmap

只要返回数据不存在，则肯定不存在。
返回数据存在，但只能是大概率存在。
同时不能清除其中的数据。


zset 跳表

cache lfu lru

key 是 sds(simple dynamic string)
len
char buf[]

hash
hash冲突，链表

Boltdb
LevelDB
RocksDB
Redis

LevelDB and its derivatives (RocksDB, HyperLevelDB) are similar to Bolt in that they are libraries bundled into the application, however, their underlying structure is a log-structured merge-tree (LSM tree). An LSM tree optimizes random writes by using a write ahead log and multi-tiered, sorted files called SSTables. Bolt uses a B+tree internally and only a single file. Both approaches have trade-offs.

Bolt supports fully serializable ACID transactions.

Bolt was originally a port of LMDB so it is architecturally similar. Both use a B+tree, have ACID semantics with fully serializable transactions, and support lock-free MVCC using a single writer and multiple readers.





