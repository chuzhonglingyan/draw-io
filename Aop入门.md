## AOP 入门 ##


### 一、简介 

面向切面编程（AOP）通过提供另一种思考程序结构的方式来补充面向对象编程（OOP）,准确来说是一种编程思想。 

OOP中模块化的关键单元是类，而在AOP中，模块化单元是方面。方面实现了诸如跨越多种类型和对象的事务管理之类的关注点的模块化。


#### 示意图

![](https://s2.ax1x.com/2019/09/30/utE1u8.jpg)

### 二、基本概念

#### AOP术语

- Aspect（切面）：跨越多个类的关注点的模块化。事务管理是企业Java应用程序中横切关注点的一个很好的例子。在Spring AOP中，方面是使用常规类（ schema-based approach ）或使用 @Aspect 注释（ @AspectJ style ）注释的常规类实现的。

- Joinpoint（连接点）：在**程序执行过程中的某个阶段点**，它实际上是对象的一个操作，例如方法的调用或异常的抛出。在 Spring AOP中，连接点就是指方法的调用。

- Pointcut（切入点）：是指切面与程序流程的交叉点，即那些需要处理的连接点，如下图程序流程所示。通常在程序中，**切入点指的是类或方法名**，如某个通知要应用到所有以add开头的方法中，那么所有满足这一规则的方法都是切入点。

- Advice（通知/增强处理）：AOP 框架在特定的切入点执行的增强处理，即在定义好的切入点处所要执行的程序代码。可以将其理解为**切面类中的方法，它是切面的具体实现**。

- Target Object（目标对象）：是指所有被通知的对象，也称为被**增强对象**。如果AOP框架采用的是动态的AOP实现，那么该对象就是一个被代理对象。

- Proxy（代理）：将通知应用到目标对象之后，被动态创建的对象，是**代理对象**。在Spring Framework中，AOP代理将是JDK动态代理或CGLIB代理。

- Weaving（织入）：**将切面代码插入到目标对象上，从而生成代理对象的过程**。这可以在编译时（例如，使用AspectJ编译器），加载时间或在运行时完成。与其他纯Java AOP框架一样，Spring AOP在运行时执行编织。


#### 示意图


![](https://s2.ax1x.com/2019/09/30/utEMgP.jpg)

### 三、使用场景

AOP适用多模块系统横切联系，统一处理,具体可以在下面的场景中使用:

- Authentication 权限

-  Caching 缓存

- Context passing 内容传递

- Error handling 错误处理

- Lazy loading　懒加载

- Debugging　　调试

- logging, tracing, profiling and monitoring　记录跟踪　优化　校准

- Performance optimization　性能优化

- Persistence　　持久化

- Resource pooling　资源池

- Synchronization　同步

- Transactions 事务
