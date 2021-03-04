# ThreadLocal

- ThreadLocal 内部并非 HashMap 而是 ThreadLocalMap
- Entry 是 WeakReference
- 同时 entry value 也没有链表结构
- 内存泄漏，WeakReference 导致 value 没有回收，记得 finaly localName.remove();
- 那为什么ThreadLocalMap的key要设计成弱引用？
- key不设置成弱引用的话就会造成和entry中value一样内存泄漏的场景。
- ThreadLocal的不足，我觉得可以通过看看netty的fastThreadLocal来弥补