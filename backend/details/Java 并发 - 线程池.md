### 并发 - 线程池（ThreadPool）

- ThreadPoolExecutor 构造函数参数
  * corePoolSize: 闲置依然存活的线程数量，allowCoreThreadTimeOut=true，闲置超时可销毁
  * maximumPoolSize: 线程总数 = 核心线程数 + 非核心线程数
  * keepAliveTime: 该线程池中非核心线程闲置超时时长
  * workQueue: 该线程池中的任务队列, 维护着等待执行的 Runnable 对象。
    - SynchronousQueue: 这个队列接收到任务的时候，会直接提交给线程处理，而不保留它，如果所有线程都在工作怎么办？那就新建一个线程来处理这任务！所以为了保证不出现<线程数达到了maximumPoolSize而不能新建线程>的错误，使用这个类型队列的时候，maximumPoolSize一般指定成Integer.MAX_VALUE，即无限大
    - LinkedBlockingQueue: 这个队列接收到任务的时候，如果当前线程数小于核心线程数，则新建线程(核心线程)处理任务；如果当前线程数等于核心线程数，则进入队列等待。由于这个队列没有最大值限制，即所有超过核心线程数的任务都将被添加到队列中，这也就导致了maximumPoolSize的设定失效，因为总线程数永远不会超过corePoolSize
    - ArrayBlockingQueue: 可以限定队列的长度，接收到任务的时候，如果没有达到corePoolSize的值，则新建线程(核心线程)执行任务，如果达到了，则入队等候，如果队列已满，则新建线程(非核心线程)执行任务，又如果总线程数到了maximumPoolSize，并且队列也满了，则发生错误
    - DelayQueue: 队列内元素必须实现Delayed接口，这就意味着你传进去的任务必须先实现Delayed接口。这个队列接收到任务时，首先先入队，只有达到了指定的延时时间，才会执行任务
  * threadFactory: 如何创建线程
  * RejectedExecutionHandler: 异常处理
- 核心线程数，最大线程数
- 阿里 Java 规范推荐不使用 Executors 静态方法，因为屏蔽细节，容易内存泄漏

执行策略：
1. 线程数量未达到 corePoolSize，则新建一个线程(核心线程)执行任务
2. 线程数量达到了 corePoolSize，则将任务移入队列等待
3. 队列已满，新建线程(非核心线程)执行任务
4. 队列已满，总线程数又达到了maximumPoolSize，就会由 RejectedExecutionHandler 抛出异常

默认线程池

类型 | core | max | keepAlive | workQueue
----|------|-----|-----------|----------
CachedThreadPool | 0 | Integer.MAX_VALUE | 60s | SynchronousQueue
FixedThreadPool | n | n | 0ms | LockedBlockingQueue
ScheduledThreadPool | n | Integer.MAX_VALUE | ? | DelayedWorkQueue
SingleThreadExecutor | 1 | 1 | 0ms | LinkedBlockingQueue