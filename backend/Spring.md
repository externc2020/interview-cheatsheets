### 如何处理循环依赖
- 构造函数依赖无法解决
- setter 循环依赖通过三级缓存结局

### Bean 的生命周期
- 初始化流程：
- 销毁流程：

### Bean 的作用域（Scope）
- Singleton
- Prototype
- Request
- Session
- Global session

### 单例 Bean 是线程安全的吗

### IOC 的原理和实现
- Ioc容器的加载过程
- BeanFactory

### BeanFactory 和 ApplicationContext 有什么区别

### 依赖注入方式

### 自动装配模式
- no
- byName
- byType
- constructor
- autodetect


### AOP 动态代理和 CGLIB 的区别和原理
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

### Spring 事务和数据库事务有什么区别？

### Spring 提供的标准事件（ApplicationEvent）
- ContextRefreshedEvent
- ContextStartedEvent
- ContextStoppedEvent
- ContextClosedEvent
- RequestHandledEvent

### Spring 支持的资源类型

### 同类中 @Transactional 方法调用另外 @Transactional 方法为何失效

### Spring 框架使用了哪些设计模式
- 代理模式：AOP
- 单例模式: Bean
- 模板模式: JdbcTemplate, RestTemplate
- 委派模式: DispatcherServlet
- 工厂模式: BeanFactory

### Spring Boot 1.5 和 2 版本的主要区别

### Spring Cloud 常用组件

### BeanDefinition

### AspectJ AOP

### SpringMVC DispatcherServlet 工作原理

### Spring 常用注解

### Spring 是如何扫描注解的