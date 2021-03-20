# AbstractQueuedSynchronizer

- `java.util.concurrent` 包中的核心类，核心数据结构：state + CLH 无锁队列（变种，线程等待队列）
- 怎么实现可重入？state + 1
- 公平 vs 非公平

### 内部原理

- state=0, 没有任何线程占有共享资源的锁
- state>=1, 则说明有线程目前正在使用共享变量，其他线程必须加入同步队列进行等待，AQS内部通过内部类Node构成FIFO的同步队列来完成线程获取锁的排队工作，同时利用内部类ConditionObject构建等待队列，当Condition调用wait()方法后，线程将会加入等待队列中，而当Condition调用signal()方法后，线程将从等待队列转移动同步队列中进行锁竞争。注意这里涉及到两种队列，一种的同步队列，当线程请求锁而等待的后将加入同步队列等待，而另一种则是等待队列(可有多个)，通过Condition调用await()方法释放锁后，将加入等待队列。
- 因为是可重入的，每重入一次，锁会+1

### CLH 无锁队列

- 变种，变了什么，为什么变
- 为什么 node.prev 遍历线程安全，node.next 非线程安全
- 为什么要用双链表
- 新的线程进来怎么加入队列

### ReetrantLock

- 独占模式

### Semaphore

- 共享模式
