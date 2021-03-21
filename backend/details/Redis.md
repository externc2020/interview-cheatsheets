# Redis

### 数据类型

- 字符串（String）：GET，SET
- 列表（List）：LPUSH，RPUSH，LRANGE，LTRIM，BLPOP
- 集合（Set）：SADD，SPOP，SRANDMEMBER
- 字典（Hash）：HSET，HGETALL
- 有序集合（Sorted set/Zset）：ZADD，ZRANGE，ZRANGEBYSCORE
- Bitmap
- HyperLogLog
- Stream：5.0 新数据类型：XADD

### SDS(Simple Dynamic String)

- 结构体 `len`, `char buf[]`
- 

### 淘汰策略

- noeviction（默认策略）：对于写请求不再提供服务，直接返回错误（DEL等部分特殊请求除外）
- allkeys-lru：所有 key 采用 LRU 算法淘汰
- volatile-lru：设置了过期时间的 key 采用 LRU 算法进行淘汰
- allkeys-random：随机淘汰
- volatile-random：随机淘汰
- volatile-ttl：越早过期的优先淘汰

注：
- 查看淘汰策略 `config get maxmemory-policy`
- Redis 采用近似 LRU 算法，更节约内存，`maxmemory-samples`


### 持久化

- RDB
  * 同步（SAVE）
  * 异步（BGSAVE）：fork 子进程，在 save 过程中 copy-on-write 要修改的内存页（父进程所有页设置为只读，写操作触发页异常中断）
- AOF

### 单线程

- 复用 IO，事件循环，避免多线程上下文切换
- 6.0 所谓多线程是网络部分多线程

### 高可用


### 穿透，雪崩，击穿

- 穿透：恶意请求（例如 id=-1）
  * 合法性校验
  * [布隆过滤器（Bloom Filter）](数据结构%20-%20BloomFilter.md)
  * ban ip
- 雪崩：大量 key 失效
  * 调整失效时间，避免在同一个时刻失效
  * 跑定时任务，续期失效时间
  * 不设置失效时间
  * 热点 key 不要集中放置在一个节点（集群部署）
- 击穿：热点 key 失效
  * 分布式锁
  * 不过期
  * 预加载

### 分布式锁

- 官方推荐红锁（Redlock）算法
- Redission：SETNX，过期时间，续期，Lua 脚本，适用于 master 单机/集群
- 存在时间跳跃问题，对比其他分布式锁实现（例如：ZooKeeper，Etcd，Chubby）

### 其他

- Redis的内存用完了会发生什么？
