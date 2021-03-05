### Bean scopes
- Singleton
- Prototype
- Request
- Session
- Global session

### IOC 原理
- Ioc容器的加载过程
- BeanFactory

### 如何处理循环依赖

- 构造函数依赖无法解决
- setter 循环依赖通过三级缓存结局

### AOP 原理
- 动态代理的实现原理
- AOP 动态代理，CGLIB，动态代理与cglib实现的区别

### 事务传递
- REQUIRED: 如果当前没有事务，则自己新建一个事务，如果当前存在事务，则加入这个事务
- SUPPORTS: 当前存在事务，则加入当前事务，如果当前没有事务，就以非事务方法执行
- MANDATORY: 当前存在事务，则加入当前事务，如果当前事务不存在，则抛出异常
- REQUIRES_NEW: 创建一个新事务，如果存在当前事务，则挂起该事务
- NOT_SUPPORTED: 始终以非事务方式执行,如果当前存在事务，则挂起当前事务
- NEVER: 不使用事务，如果当前事务存在，则抛出异常
- NESTED: 如果当前事务存在，则在嵌套事务中执行，否则REQUIRED的操作一样（开启一个事务）