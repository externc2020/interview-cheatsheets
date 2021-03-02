JVM自动内存管理，Minor GC与Full GC的触发机制

了解过JVM调优没，基本思路是什么

淘宝热门商品信息在JVM哪个内存区域

字节码的编译过程

类的加载过程
classloader

对象在内存中的布局
org.openjdk.jol
java object layout

System.gc() 不一定立刻回收
Runs the garbage collector.
When control returns from the method call, the virtual machine has made its best effort to recycle all discarded objects.

垃圾回收算法
引用计数，解决不了循环引用
根可达 root searching
root=jvm stack, native method stack, runtime constant pool, static references in method area, class
常见算法
mark-sweep 标记清除，标记垃圾内存，易产生碎片
copying 拷贝，划分区域，从一个区域拷贝有效对象到另一个空间，无碎片，浪费空间
mark-compact 标记压缩，标记垃圾内存，同时拷贝其他有用内存数据，做数据整理，消除碎片，效率较copy低（移动数据要线程同步，copy一次性）

JVM（内存布局）
新生代 = Eden + Survivor(S0, S1) Minor GC 发生在 Survivor，来回换
老年代 Full GC

传统gc分代算法，new，old
1.7 有 perm gen（永久代）-> 1.8 元数据区 metaspace，装 class metadata，例如 aop 创建很多 proxy 会爆这个区域
字符串常量 1.7 存永久代，1.8 存堆
元数据区是操作系统管理，非jvm管理
g1没有分代

new = eden(8) : s0(1) :s1(1)
survivor copying 回收算法
old mark-compact 回收算法

s区装不下直接丢old，old 满 full gc

gc 调优目标：减少 full gc (stop-the-world stw)

回收器类型
新生代可配置的回收器
- 串行回收 Serial（单线程，停所有其他线程）
- 并行回收 Parallel Scavenge = PS（多线程，无法配合CMS）
- 改进版并行回收 ParNew（PS改进，可配合 CMS）

老年代配置的回收器
- Serial Old
- Parallel Old
- CMS（Concurrent Mark Sweep）比较复杂，减少 stm 时间（200ms）

不分代回收器
G1(stm 10ms)，ZGC(stm 1ms)，Shenandoah

调试用回收器
Epsilon

1.8 默认垃圾回收器 PS + Parallel Old

HotSpot 参数
-Xms
-Xmx
-XX:PermSize 非堆内存
-XX:MaxPermSize
-XX:NewSize (-Xns) 年轻代
-XX:MaxNewSize (-Xmn)
-XX:SurvivorRatio=8
-Xss 堆栈
-XX:NewRatio
-XX:+PrintGC
-XX:+PrintGCDetails

-XX:MaxDirectMemorySize 
在NIO中可以直接访问直接内存，这个就是设置它的大小，不设置默认就是最大堆空间的值-Xmx
-XX:+DisableExplicitGC	
关闭System.gc()
-XX:MaxTenuringThreshold	
垃圾可以进入老年代的年龄
-Xnoclassgc	
禁用垃圾回收
-XX:TLABWasteTargetPercent	
TLAB占eden区的百分比，默认是1%，TLAB全称是Thread Local Allocation Buffer
-XX:+CollectGen0First	
FullGC时是否先YGC，默认false


-XX:+PrintFlagsFinal
-XX:+PrintFlagsInitial
-XX:+PrintCommandLineFlags
使用了哪些参数