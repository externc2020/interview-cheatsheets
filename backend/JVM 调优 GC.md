### 常见名词

- Major/Minor GC
- FullGC/YGC
- CMS/G1/ZGC 垃圾回收器名字
- stw = stop-the-world
- oop = ordinary object pointer

### 常见问题

问：JVM 自动内存管理如何实现<br/>
答：垃圾回收

问：Minor GC 与 Full GC 的触发机制<br/>
答：新生代，老年代

问：JVM 调优基本思路<br/>
答：减少 Full GC，缩短 stw

问：淘宝热门商品信息在 JVM 哪个内存区域<br/>
答：老年代，热 = 存活时间久

问：类的加载过程<br/>
答：

System.gc() 一定立刻回收吗？一定触发 FullGC 吗？<br/>
答：

### JVM 内存布局

- 不要和 Java 内存模型（JMM）混淆
- 五大区域：方法区，堆，线程栈，程序计数器，本地方法区
- 运行时常量池？

![HotSpot JVM: Architecture](../assets/hotspot.png)

### 对象的引用

- 强
- 软
- 弱
- 虚

### 垃圾回收器
- Java 8 默认 Parallel Scavenge + Parallel Old
- Java 9 默认 G1
- CMS：初始标记，并发标记，重新标记，并发清除，2次stw

### 垃圾回收算法
- 引用计数，解决不了循环引用
- 根可达 root searching
  * mark-sweep 标记清除，标记垃圾内存，易产生碎片
  * copying 拷贝，划分区域，从一个区域拷贝有效对象到另一个空间，无碎片，浪费空间
  * mark-compact 标记压缩，标记垃圾内存，同时拷贝其他有用内存数据，做数据整理，消除碎片，效率较copy低（移动数据要线程同步，copy一次性）

### GC Roots
- jvm stack
- native method stack
- runtime constant pool
- static references in method area,
- class

### JVM 堆布局
- 和使用的垃圾回收器有关
- 分代型：Serial，Parallel，CMS
  * 新生代 = Eden + Survivor(S0, S1) Minor GC 发生在 Survivor，来回换，new = eden(8):s0(1):s1(1)
  * 老年代 Full GC
  * 永久代（PermGen）1.8 以前 -> 1.8 元数据区 metaspace，装 class metadata，例如 aop 创建很多 proxy 会爆这个区域
  * 字符串常量 1.8 以前存永久代，1.8 存堆
  * 元数据区是操作系统管理，非jvm管理（待求证）
  * survivor 区: copying 回收算法
  * old 区：mark-compact 回收算法
  * s区装不下直接丢old，或者对象过大也直接丢old，old 满 FullGC
- 网格型：G1，ZGC，Shenandoah

### JVM 调优

- 性能基准，衡量维度
  * 吞吐
- 到底优化什么
  * 调参数 -> 大小，比例，年龄，等
- 各种垃圾回收器使用场景
- GC日志

### JVM 调优策略

- 调整年轻代大小 `-Xmn` 防止年轻代过小，对象直接丢老年代
- 调大 `-XX:PetenureSizeThreshold` 防止年轻代大对象直接丢老年代
- 取消动态堆大小调整 `-Xmx` = `-Xms`
- 调整堆伸缩阀值 `-XX:MinHeapFreeRatio=40`扩展 和 `-XX:MaxHeapFreeRatio=70`压缩
- 使用并行垃圾回收器 `-XX:+UseParallelGC` `-XX:+UseParallelOldGC`
- 使用 CMS 减少老年代 GC 停顿 `-XX:+UseConcMarkSweepGC`
- 使用大的内存分页，增加 CPU 内存寻址能力，提升性能 `-XX:+LargePageSizeInBytes`
- `-XX:+AlwaysPreTouch` 将会在 Java 堆空间中写满 0, 避免在实际使用到堆空间时再触发缺页异常，影响运行时的效率


### JVM 调优工具

- 快照
  * jmap: 生成内存快照
- 观察
  * jps: 进程状态
  * jstat: 内存，垃圾回收，类加载
  * jstack: 栈状态
  * jinfo: ?
  * 运维监控: Zabbix, Prometheus
- 分析
  * jhat: 分析内存快照，会启动 web 服务器，可以通过浏览器分析
  * mat: 功能比较强大的内存分析工具，可以替代jhat
  * jvisualvm
  * jprofile
  * jca
- 调试
  * jdb

### JVM 调优参数
- `-XX:+HeapDumpOnOutOfMemoryError` 表示当JVM发生OOM时，自动生成DUMP文件
- `-Xms` 初始堆大小
- `-Xmx` 最大堆大小
- `-Xns`, `-XX:NewSize` 初始年轻代大小
- `-Xmn`, `-XX:MaxNewSize` 最大年轻代大小
- `-Xss1m` 每个线程的堆栈大小(等价于-XX:ThreadStackSize)
- `-XX:PermSize=256m` 设置持久代(perm gen)初始值(JDK8以后弃用改为-XX:MetaspaceSize)
- `-XX:MaxPermSize=256m` 设置持久代最大值(JDK8以后弃用改为-XX:MaxMetaspaceSize)
- `-XX:SurvivorRatio` Eden区与Survivor区的大小比值（默认为8）
- `-XX:NewRatio`
- `-XX:MaxTenuringThreshold=15` 年轻代垃圾回收最大年龄，默认15，15次后进入老年代
- `-XX:PretenureSizeThreshold` 对象超过多大直接在老年代分配
- `-Xverify:no` 禁止字节码校验，提高编译速度（jdk13之后作废，生产环境不使用）
- `-XX:+DisableExplicitGC` 关闭 System.gc()
- `-Xnoclassgc` 禁用垃圾回收
- `-XX:+CollectGen0First` FullGC时是否先YGC，默认false
- `-XX:MaxDirectMemorySize` 在NIO中可以直接访问直接内存，这个就是设置它的大小，不设置默认就是最大堆空间的值-Xmx
- `-XX:TLABWasteTargetPercent` TLAB占eden区的百分比，默认是1%，TLAB全称是Thread Local Allocation Buffer
- `-XX:ReservedCodeCacheSize`
- `-XX:SoftRefLRUPolicyMSPerMB`
- `-XX:CICompilerCount=2`
- `-Dsun.io.useCanonPrefixCache=false`
- `-Djava.net.preferIPv4Stack=true`
- `-XX:-OmitStackTraceInFastThrow`
- `-XX:+UseCompressedOops`

打印参数

- `-XX:+PrintCommandLineFlags` 使用了哪些参数
- `-XX:+PrintFlagsInitial`
- `-XX:+PrintFlagsFinal`
- `-XX:+PrintGC`
- `-XX:+PrintGCDetails`
- `-XX:+PrintGCApplicationStoppedTime` 打印 stw
