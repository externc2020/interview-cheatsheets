# 容器（Collections）

## List

### 方法

- add/remove
- addAll/removeAll
- get/set
- indexOf/lastIndex/Of
- contains/containsAll

### 实现

- ArrayList：内部数组，默认大小 10
- LinkedList：内部双链表
- CopyOnWriteArrayList：内部数组，修改操作加锁，锁就是普通 new Object()，使用 builtin monitors 而没有使用 ReentrantLock，其实两者都行

## Queue

### 方法

- add/offer: 入队，限制多/少
- poll/peek: 取队首，删/不删
- remove/element: 取队首，删/不删，报异常 NoSuchElementException

### 实现
- LinkedList
- PriorityQueue：内部堆
- ArrayDeque：内部数组

## BlockingQueue

### 方法
- put/take: InterruptedException

### 实现
- ArrayBlockingQueue
- LinkedBlockingQueue
- PriorityBlockingQueue
- DelayQueue


## SynchronousQueue

- 实现 BlockingQueue
- 容量：0
- 类似 CSP 和 Ada 里面的 Rendezvous Channels
- 用于线程间传递消息

## TransferQueue 

- 继承 BlockingQueue
- 生产者要等到消费者确认收到消息
- Java 7
- 实现：LinkedTransferQueue

## Deque

### 方法

### 实现
- LinkedList
- ArrayDeque
- LinkedBlockingDeque

## Map
- HashMap: 内部哈希表，冲突链表
- LinkedHashMap: 内部双向链表，get O(n)
- TreeMap: 红黑树
- EnumMap
- WeakHashMap: WeakReference
- IdentityHashMap: 指针值相等
- 接口 ConcurrentMap: atomic putIfAbsent, remove, replace
- ConcurrentHashMap

## Set
- HashSet: 无序，内部 `HashMap<E,Object>`;
- LinkedHashSet: 保持插入顺序，内部 `LinkedHashMap<>`;
- TreeSet: 内部 `NavigableMap<K,V>` 继承自 `SortedMap<K,V>` 默认实现 TreeMap (红黑树);
- EnumSet: 内部是一个数组;
- CopyOnWriteArraySet: 内部是 `CopyOnWriteArrayList<E>`;
- EntrySet: 用于 map 内部;

## 静态方法
- Collections.synchronizedCollection/Set/List/Map/SortedSet/SortedMap
- Collections.unmodifiableCollection/Set/List/Map/SortedSet/SortedMap
- Collections.checked: ClassCastException
- Arrays.asList
- Collections.nCopies
- Collections.singleton
- Collections.emptySet/List/Map
