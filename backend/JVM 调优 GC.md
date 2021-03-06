### 常见名词

- Major/Minor GC
- FullGC/YGC
- CMS/G1/ZGC 垃圾回收器名字
- stw = stop-the-world

### 常见问题

- JVM自动内存管理，Minor GC与Full GC的触发机制
- JVM调优基本思路: 减少 FullGC，缩短 stw
- 淘宝热门商品信息在 JVM 哪个内存区域
- 字节码的编译过程
- 类的加载过程
- 对象在内存中的布局 org.openjdk.jol (jol = java object layout)
- System.gc() 不一定立刻回收？
> Runs the garbage collector. When control returns from the method call, the virtual machine has made its best effort to recycle all discarded objects.

### JVM 内存布局

- 不要和 Java 内存模型（JMM）混淆
- 五大区域：方法区，堆，线程栈，程序计数器，本地方法区

![HotSpot JVM: Architecture](../assets/hotspot.png)

### 垃圾回收器
- Java 8 默认 Parallel Scavenge + Parallel Old
- Java 9 默认 G1

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

### JVM 调优常用工具

- jvisualvm
- jstat: 查看 JVM 内存使用状态
- jmap: 生成内存快照
- jhat: 分析内存快照，会启动 web 服务器，可以通过浏览器分析
- jprofile
- jca
- jinfo
- jdb
- MAT工具: 功能比较强大的内存分析工具，可以替代jhat
- 监控系统: Zabbix, Prometheus

### HotSpot 常用调节参数

- `-XX:+HeapDumpOnOutOfMemoryError` 表示当JVM发生OOM时，自动生成DUMP文件
- `-Xms2048m` 初始堆大小
- `-Xmx2048m` 最大堆大小
- `-Xmn1024m` 年轻代大小
- `-Xss1m` 每个线程的堆栈大小(等价于-XX:ThreadStackSize)
- `-XX:PermSize=256m` 设置持久代(perm gen)初始值(JDK8以后弃用改为-XX:MetaspaceSize)
- `-XX:MaxPermSize=256m` 设置持久代最大值(JDK8以后弃用改为-XX:MaxMetaspaceSize)
- `-XX:SurvivorRatio` Eden区与Survivor区的大小比值（默认为8）
- `-XX:MaxTenuringThreshold=15` 年轻代垃圾回收最大年龄，默认15，15次后进入老年代
- `-XX:PretenureSizeThreshold` 对象超过多大直接在老年代分配
- `-Xverify:no` 禁止字节码校验，提高编译速度（jdk13之后作废，生产环境不使用）


```
-XX:NewSize (-Xns) 年轻代
-XX:MaxNewSize (-Xmn)
-XX:SurvivorRatio=8
-XX:NewRatio

-XX:MaxDirectMemorySize 
在NIO中可以直接访问直接内存，这个就是设置它的大小，不设置默认就是最大堆空间的值-Xmx

-XX:+DisableExplicitGC	
关闭System.gc()

-Xnoclassgc	
禁用垃圾回收

-XX:TLABWasteTargetPercent	
TLAB占eden区的百分比，默认是1%，TLAB全称是Thread Local Allocation Buffer

-XX:+CollectGen0First	
FullGC时是否先YGC，默认false

-XX:+PrintGC
-XX:+PrintGCDetails
-XX:+PrintFlagsFinal
-XX:+PrintFlagsInitial
-XX:+PrintCommandLineFlags
使用了哪些参数
```