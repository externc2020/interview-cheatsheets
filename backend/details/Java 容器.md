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