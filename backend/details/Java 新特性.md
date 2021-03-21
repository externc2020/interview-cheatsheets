### Project Valhalla: value types

### Project Loom: virtual threads

- Blocking is cheap 
- 量级：millions vs thousands theads
- No more async
- Structured concurrency - no more goto with threads
- Asynchronous programming: callback hell -> futures or async/await (会把你的代码分成 sync 和 async 两部分)
- Virtual threads mapped into "carrier" threads(OS)
- 纤程, fiber, 用户态线程, 协程
- 和原来的 green thread 是有区别的: 
  * 如何映射回系统线程: Loom 多对多，green 多对一
  * When a green thread blocked, it blocked the carrier thread
- "virtual": supposed to evoke virtual memory that is mapped to actual RAM (类比虚拟内存)


### Project Panama: improved interoperability with native code
