### 名词

- 偏向锁（biased locking）object markword 存放当前线程指针，默认延迟 4 秒，偏向锁要撤销，所以在明知某些资源会有多线程竞争，就不用偏向锁，所以会有延迟
-XX:BiasedLockingStartupDelay=0
- 乐观锁（轻量，自旋，无锁 lock-free）CAS，用户态，适合锁定时间短的操作，等待锁消耗 CPU
- 悲观锁（重量）内核态
- 对象锁状态（markword） 8 字节分布及状态

### CAS 原子性问题

CAS 原子性问题，AtomicInteger JUC 大多是时 CAS，CPU 指令也不能保障原子性（LOCK_IF_MP，这种汇编实现是因为多核情况下不行，解决方法汇编加 lock = lock cmpxchg，lock 锁定总线（优先锁定缓存行，然后锁定北桥）

CAS, ABA（引用，泄漏造成大问题，版本号/bool）

### 在同一个缓存行存在竞争

- 缓存行对齐
- 通过缓存行 padding 实现伪共享，确保不在同一个内存行
- 一般内存行是 64 个字节
- disruptor ringbuffer lmax
- long p1, p2, p3, p4, p5, p6, p7 前后填充，解决缓存行伪共享
- 1.8 加入 @sun.misc.Contended 优雅解决缓存行伪共享

### synchronized

- 1.5 后有一个锁升级的过程：偏向 -> 乐观 -> 悲观
- 偏向，一旦有竞争升级乐观，显示调用 Object.wait() 升级悲观
- 字节码 lock cmpxchg

### volatile

- 可见性：Java 内存模型（JMM），强制刷新
- 有序性：禁止指令重排，内存屏障
- 不保证原子性
- 有内存屏障，会导致缓存行失效
- volatile 内存屏障 就是 lock addl 汇编指令下往某个寄存器上加一个 0
- 用 hsdis（hotspot disassembler）观察 synchronized volatile

DCL（double-check lock） 单利为什么要加 volatile

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

### AQS

- 字节码 monitorenter monitorexit 1 个 enter 对应 两个 exit，一个是正常退出，一个是异常退出