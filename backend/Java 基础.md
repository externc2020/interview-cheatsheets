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

java string
final class
immutable

### 容器（Collections）


Set implementations
- HashSet
- LinkedHashSet
- TreeSet
- EnumSet
- CopyOnWriteSet

List implementations
- ArrayList
- LinkedList
- CopyOnWriteArrayList

Map implementations
- HashMap
- LinkedHashMap
- TreeMap
- EnumMap
- WeakHashMap
- IdentityHashMap
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

### 线程池（ThreadPool）

参数，不要用 executors 静态方法，内存泄漏


### 并发（Concurrent 包）

synchronized
volatile
Lock, Condition, 


### 异常（Exceptions）




### 反射（Reflection）



### 类加载（ClassLoader）








