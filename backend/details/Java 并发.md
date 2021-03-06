# Java 并发

### 基本

- Object Layout: mark word
- Monitor
- synchronized/volatile
- Java 1.6 关于 synchronized 的优化：锁升级
- [锁的底层原理](./Java+并发.md)


### 工具类（java.util.concurrent)

#### java.util.concurrent.locks

- 接口 Lock, ReadWriteLock, Condition
- AQS.ConditionObject, AQLS.ConditionObject
- AbstractOwnableSynchronizer
- AbstractQueuedSynchronizer(AQS)
- AbstractQueuedLongSynchronizer
- ReentrantLock
- ReentrantReadWriteLock
- StampedLock

#### java.util.concurrent

- CountDownLatch
- CyclicBarrier
- `Exchanger<V>`
- Executors
- Semaphore

#### java.util.concurrent.atomic

- AtomicBoolean/Integer/Long/Reference/MarkableReference
- Long/DoubleAdder
- Long/DoubleAccumulator
