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
    <td rowspan="2">Literals</td>
    <td>boolean</td>
    <td>n/a</td>
</tr>
<tr>
    <td>null</td>
    <td>n/a</td>
</tr>
<tr>
    <td>Other</td>
    <td>void</td>
    <td>n/a</td>
</tr>
<table>

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

参数，不要用 executors 静态方法，内存泄漏


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








