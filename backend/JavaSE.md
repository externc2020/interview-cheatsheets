### 基本类型（Primitive Types）

<table>
<tr>
    <th>Category</th>
    <th>Types</th>
    <th>Size (bytes)</th>
<tr>
<tr>
    <td rowspan="5">Integer</td>
    <td>byte</td>
    <td>1</td>
</tr>
<tr>
    <td>short</td>
    <td>2</td>
</tr>
<tr>
    <td>int</td>
    <td>4</td>
</tr>
<tr>
    <td>long</td>
    <td>8</td>
</tr>
<tr>
    <td>char</td>
    <td>4</td>
</tr>
<tr>
    <td rowspan="2">Floating-point</td>
    <td>float</td>
    <td>4</td>
</tr>
<tr>
    <td>double</td>
    <td>8</td>
</tr>
<tr>
    <td>Literals</td>
    <td>boolean</td>
    <td>n/a</td>
</tr>
<table>

特殊：`null`, `void`

注：array 不是基本类型

### 字符串（Strings）

* String: final class
* String: immutable

### 容器（Collections）

Set implementations
- HashSet: 无序，内部 `HashMap<E,Object>`;
- LinkedHashSet: 保持插入顺序，内部 `LinkedHashMap<>`;
- TreeSet: 内部 `NavigableMap<K,V>` 继承自 `SortedMap<K,V>` 默认实现 TreeMap (红黑树);
- EnumSet: 内部是一个数组;
- CopyOnWriteArraySet: 内部是 `CopyOnWriteArrayList<E>`;
- EntrySet: 用于 map 内部;

List implementations
- ArrayList
- LinkedList
- CopyOnWriteArrayList

Map implementations
- HashMap
- LinkedHashMap: 内部双向链表，get O(n)
- TreeMap: 红黑树
- EnumMap
- WeakHashMap: WeakReference
- IdentityHashMap: 指针值相等
- 接口 ConcurrentMap: atomic putIfAbsent, remove, replace
- ConcurrentHashMap

Queue implementations
- LinkedList
- PriorityQueue
- LinkedBlockingQueue
- ArrayBlockingQueue
- PriorityBlockingQueue
- DelayQueue
- SynchronousQueue: a simple rendezvous mechanism that uses the BlockingQueue interface
- 1.7 TransferQueue: a specialized BlockingQueue in which code that adds an element to the queue has the option of waiting (blocking) for code in another thread to retrieve the element. TransferQueue has a single implementation: LinkedTransferQueue: an unbounded TransferQueue based on linked nodes

Deque implementations
- LinkedList
- ArrayDeque
- LinkedBlockingDeque

Wrapper implementations
- Collections.synchronizedCollection/Set/List/Map/SortedSet/SortedMap
- Collections.unmodifiableCollection/Set/List/Map/SortedSet/SortedMap
- Collections.checked: ClassCastException

Convenience implementations
- Arrays.asList
- Collections.nCopies
- Collections.singleton
- Collections.emptySet/List/Map

### 并发 - 基本

- synchronized
- volatile
- Object.wait/notify/notifyAll()

### 并发 - 线程池（ThreadPool）

- ThreadPoolExecutor 构造函数参数
  * corePoolSize: 闲置依然存活的线程数量，allowCoreThreadTimeOut=true，闲置超时可销毁
  * maximumPoolSize: 线程总数 = 核心线程数 + 非核心线程数
  * keepAliveTime: 该线程池中非核心线程闲置超时时长
  * workQueue: 该线程池中的任务队列, 维护着等待执行的 Runnable 对象。
    - SynchronousQueue: 这个队列接收到任务的时候，会直接提交给线程处理，而不保留它，如果所有线程都在工作怎么办？那就新建一个线程来处理这任务！所以为了保证不出现<线程数达到了maximumPoolSize而不能新建线程>的错误，使用这个类型队列的时候，maximumPoolSize一般指定成Integer.MAX_VALUE，即无限大
    - LinkedBlockingQueue: 这个队列接收到任务的时候，如果当前线程数小于核心线程数，则新建线程(核心线程)处理任务；如果当前线程数等于核心线程数，则进入队列等待。由于这个队列没有最大值限制，即所有超过核心线程数的任务都将被添加到队列中，这也就导致了maximumPoolSize的设定失效，因为总线程数永远不会超过corePoolSize
    - ArrayBlockingQueue: 可以限定队列的长度，接收到任务的时候，如果没有达到corePoolSize的值，则新建线程(核心线程)执行任务，如果达到了，则入队等候，如果队列已满，则新建线程(非核心线程)执行任务，又如果总线程数到了maximumPoolSize，并且队列也满了，则发生错误
    - DelayQueue: 队列内元素必须实现Delayed接口，这就意味着你传进去的任务必须先实现Delayed接口。这个队列接收到任务时，首先先入队，只有达到了指定的延时时间，才会执行任务
  * threadFactory: 如何创建线程
  * RejectedExecutionHandler: 异常处理
- 核心线程数，最大线程数
- 阿里 Java 规范推荐不使用 Executors 静态方法，因为屏蔽细节，容易内存泄漏

执行策略：
1. 线程数量未达到 corePoolSize，则新建一个线程(核心线程)执行任务
2. 线程数量达到了 corePoolSize，则将任务移入队列等待
3. 队列已满，新建线程(非核心线程)执行任务
4. 队列已满，总线程数又达到了maximumPoolSize，就会由 RejectedExecutionHandler 抛出异常

默认线程池

类型 | core | max | keepAlive | workQueue
----|------|-----|-----------|----------
CachedThreadPool | 0 | Integer.MAX_VALUE | 60s | SynchronousQueue
FixedThreadPool | n | n | 0ms | LockedBlockingQueue
ScheduledThreadPool | n | Integer.MAX_VALUE | ? | DelayedWorkQueue
SingleThreadExecutor | 1 | 1 | 0ms | LinkedBlockingQueue

### 并发 - 工具类（java.util.concurrent)

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


### 异常（Exceptions）




### 反射（Reflection）



### 类加载（ClassLoader）








