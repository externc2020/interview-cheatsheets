# Java Standard Edition

## 语言基础

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

* String: final class, immutable

### Integer

- 自动装箱
- 0-127 缓存

### Object

- wait/notify/notifyAll
- monitor: ObjectMonitor.hpp
- hashCode
- 字节码头部(HotSpot) = mark word + klass pointer
- == vs equals
- 为什么建议重写 equals 后还要同时重写 hashCode

### 类加载

- 过程：加载，链接（验证，准备，解析），初始化
- 类加载器（ClassLoader）：启动，扩展，应用，自定义
- 双亲委派模型
- classloader hell，java 9 jigsaw

### 关键字

- final
- synchronized：锁升级
- volatile：可见性，防指令重排，不保证原子性
- strictfp
- transient

## 容器

- HashMap
- ConcurrentHashMap
- List/Map/Set

## 并发

### 线程（Threads）

- 状态转换：RUNNING，READY(yield切换RUNNING)，WAITING（wait/join/notify,LockSupport.park/unpark),
NEW, TIMED_WAITING(sleep/), BLOCKED(synchronized), TERMINATED

### ThreadLocal

- ThreadLocalMap
- WeakReference
- Thread -> ThreadLocalMap -> Entry -> Value 容易内存泄漏
- Java InheritableThreadLocal 解决父子线程值传递（例如：UserID，TransactinID，TraceID）
- 阿里 TransmittableThreadLocal(TTL) 上面的增强版，解决线程池

## 异常（Exceptions）

- checked/unchecked
- Java每实例化一个Exception，都会对当时的栈进行快照
- jvm thread stack exception table
- 在编译生成的字节码中，每个方法都附带一个异常表，异常表中的每一个条目代表一个异常处理器,并且由 form 指针，to 指针，target 指针以及所捕获的异常类型构成。from 指针和 to 指针标示了该异常所监控的范围：try 代码块所覆盖的范围。
target 指针标示了异常处理器的起始位置：catch 代码块的起始位置。
当程序处罚异常时，Java 虚拟机会从上至下遍历异常表中的所有条目。当触发异常的字节码索引值在某个异常表条目的监控范围内，Java 虚拟机再判断所抛出的异常和该条目想要捕获的异常是否匹配。如果匹配，Java 虚拟机会将控制流转移至该条目 target 指针指向的字节码。
如果遍历完异常表的条目未曾匹配到异常处理器，那么它会弹出当前方法对应的 Java 栈帧，并且在调用者中重复上述操作。

## 其他

- 序列化 transient，安全问题
- 反射（Reflection）
- Java 探针（Java Agent），JVMTI（Tool Interface）
- Java 扩展 SPI（Service Provider Interface)
