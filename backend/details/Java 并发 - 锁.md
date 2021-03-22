### 名词

- 偏向锁（Biased Locking）object markword 存放当前线程指针，默认延迟 4 秒，偏向锁要撤销，所以在明知某些资源会有多线程竞争，就不用偏向锁，所以会有延迟
-XX:BiasedLockingStartupDelay=0
- 乐观锁（轻量，自旋，无锁 lock-free）CAS，用户态，适合锁定时间短的操作，等待锁消耗 CPU
- 悲观锁（重量）内核态
- 对象锁状态（markword） 8 字节分布及状态
- 公平，非公平
- 共享（读），排他（写）
- CLH 锁（SMP），MCS 锁（NUMA）

### CAS 原子性问题

- AtomicInteger JUC 大多是时 CAS，
- CPU 指令也不能保障原子性（LOCK_IF_MP，这种汇编实现是因为多核情况下不行，解决方法汇编加 lock = lock cmpxchg，lock 锁定总线（优先锁定缓存行，然后锁定北桥）
- CAS, ABA（引用，泄漏造成大问题，版本号/bool）
- CAS 不是锁，是实现乐观锁，轻量级锁的一个核心机制

### 在同一个缓存行存在竞争

- 缓存行对齐
- 通过缓存行 padding 实现伪共享，确保不在同一个内存行
- 一般内存行是 64 个字节
- disruptor ringbuffer lmax
- long p1, p2, p3, p4, p5, p6, p7 前后填充，解决缓存行伪共享
- 1.8 加入 @sun.misc.Contended 优雅解决缓存行伪共享

### synchronized

- 1.6 锁升级的过程：偏向 -> 乐观 -> 悲观
- 偏向，一旦有竞争升级乐观，显示调用 Object.wait() 升级悲观
- 字节码 cmpxchg (多核CPU lock cmpxchg)

### volatile

- 可见性：Java 内存模型（JMM），强制刷新
- 有序性：禁止指令重排，内存屏障，
- 不保证原子性
- 有内存屏障，会导致缓存行失效
- volatile 内存屏障 就是 lock addl 汇编指令下往某个寄存器上加一个 0
- 用 hsdis（hotspot disassembler）观察 synchronized volatile

### DCL（double-check lock）单例为什么要加 volatile

```
public class Singleton {
    private volatile static Singleton instance;

    private Singleton(){}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

Java可以利用JVM内部静态类装载的特点实现“延迟初始化占位类模式”来达到同样的效果

### JVM 存在锁降级吗？

- stw 阶段 VMThread 独占的对象上的重量级锁可以降级（deflate）
- 逃逸分析后发生的锁消除

### 锁优化

- 锁粗化（Lock Coarsening）将多个连续的锁扩展成一个范围更大的锁，用以减少频繁互斥同步导致的性能损耗。
- 锁消除（Lock Elimination）JVM及时编译器在运行时，通过逃逸分析，如果判断一段代码中，堆上的所有数据不会逃逸出去从来被其他线程访问到，就可以去除这些锁。
- 偏向锁（Biased Locking）
- 轻量级锁（Lightweight Locking）
- 适应性自旋（Adaptive Spinning）


### Java

- Object Layout: mark word
- synchronized/volatile

### JMM（Java 内存模型）
  * 主内存（堆），线程工作内存（线程栈）
  * 线程：工作内存 <--> 主内存
  * 线程工作内存相互隔离
  * lock/unlock
  * read -> load -> use -> 执行引擎 -> assign -> store -> write
