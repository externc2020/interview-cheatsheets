# Java 并发

### 基本

- Object Layout: mark word
- Monitor
- synchronized/volatile
- Java 1.6 关于 synchronized 的优化：锁升级
- [锁的底层原理](./Java+并发.md)
- JMM（Java 内存模型）
  * 主内存（堆），线程工作内存（线程栈）
  * 线程：工作内存 <--> 主内存
  * 线程工作内存相互隔离
  * lock/unlock
  * read -> load -> use -> 执行引擎 -> assign -> store -> write
  


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
