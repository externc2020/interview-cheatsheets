# RocketMQ

- 核心概念：主题，生产者，消费者，消息，消息属性，分组，生产者集群，消费者集群
- 死信  dead letter
- JMS

### RocketMQ 集群原理

- nameserver 无状态 zookeeper 有状态
- broker（master，slave）收发消息，持久化（刷盘）
- 写 master，读 slave 读写分离，可以同步写也可以异步写slave，主从复制
- 生产者 producer 发消息到 broker
- 消费者 consumer 从 broker 上拉取消息消费，消费完成返回 ack
- commitlog
- 高性能：顺序写，零拷贝，写 pagecache，基于 netty

### RocketMQ Dledger

- RocketMQ 4.5之后支持了一种叫做Dledger机制，基于Raft协议实现的一个机制。
- 我们可以让一个Master Broker对应多个Slave Broker， 一旦 Master Broker 宕机了，在多个 Slave 中通过 Dledger 技术 将一个 Slave Broker 选为新的 Master Broker 对外提供服务
- 在生产环境中可以是用Dledger机制实现自动故障切换，只要10秒或者几十秒的时间就可以完成
