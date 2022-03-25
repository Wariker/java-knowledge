# Spring

## IOC的理解

> IOC：控制反转，原来我们使用的时候，对象是由使用者控制，有了spring之后，可以将对象交给IOC容器来帮我们进行管理（思想
>
> DI：依赖注入，把对应的属性注入到具体的对象中，@Autowired，@Resource,populateBean方法来完成属性注入
>
> 容器：存储对象，使用map结构来存储对象，在spring中存储对象一般有三级缓存，singletonObjects存放完整对象，earlySingletonObjects存放半成品对象，singletonFactory用来存放lambda表达式和对象名称的映射，整个bean的生命周期，从创建到使用到销毁，都是IOC容器在帮我们进行控制。

## Bean的生命周期

>spring容器帮助我们管理对象，从对象的产生到销毁的环节都由容器来控制，其中主要实例化和初始化两个环节比较关键。
>
>1.首先是实例化对象，这一步是通过反射的方式，有一个createBeanInstance方法专门用于生成对象。
>
>2.然后当bean对象创建完成后，对象的属性都是默认值，此时需要调用populateBean方法来完成对象属性的填充。在这个环节会出现循环依赖的问题
>
>3.然后需要向bean对象设置容器对象的属性，如果有实现aware接口，此时会调用invokeAwareMethods方法来将容器对象设置到	具体的bean对象中。
>
>4.然后调用BeanPostProcessor中的前置处理方法来进来Bean的扩展
>
>5.然后调用invokeInitMethods来完成初始化方法的调用，在执行该方法的过程中需要判断有没有实现initializingBean接口，如果实	现了则执行afterPropertiesSet方法来最后设置bean对象
>
>6.调用BeanPostProcessor中的后置处理方法，完成对Bean的后置处理工作，并且aop就是在这个位置实现的，需要
>
>​	实现AbstractAutoProxyCreator接口
>
>7.通过getBean方法来获取完整对象
>
>8.当对象使用完毕后，IOC容器在关闭的时候，会销毁Bean对象，首先会判断是否实现了disposable接口，然后去执行destroyMethod方法 

## 循环依赖

>A依赖B，B依赖A，在创建bean对象的时候可能会出现循环依赖问题。
>
>spring中bean对象的创建都要经历实例化和初始化，如果将对象的状态分开，那么有成品对象和半成品对象，这两种对象在存储的时候需要使用不同的缓存进行存储。
>
>- 如果只有一级缓存，会把成品对象和半成品对象存放到一起，但是半成品对象是不能暴露给外部使用的，所以需要将两种状态的对象分开存放，一级缓存存放成品对象，二级缓存存放半成品对象。
>- 如果只有二级缓存，那么系统中只要不涉及aop的话，能够解决循环依赖的问题，但是如果存在aop并且aop产生了循环依赖的问题，那么就需要使用三级缓存来解决问题。
>- 为什么需要三级缓存？三级缓存的key也是String，但是value是ObjectFactory，他是一个函数式接口，里面定义了一个getObject方法，在调用该方法时会调用存储的lambda表达式，存在的意义是保证在整个容器的运行过程中，只有一个同名的bean对象，当一个对象被调用的时候，会判断该对象是否需要被代理，当获取对象之后根据传入的lambda表达式来确认返回的是代理对象还是原始对象。

## spring使用到的设计模式

>单例模式：
>
>工厂模式：
>
>模板方法：
>
>观察者模式：
>
>适配器模式：
>
>装饰者模式：
>
>责任链模式：
>
>代理模式：
>
>委托者模式：
>
>建造者模式：
>
>策略模式：

## BeanFactory和FactoryBean的区别

> BeanFactory和FactoryBean都可以创建对象，但是创建的流程不一样。
>
> BeanFactory是严格按照bean对象的生命周期来创建对象的
>
> FactoryBean可以自定义Bean的创建过程，该接口包含了三个方法。
>
> isSingleton：判断对象是否是单例
>
> getObjectType：获取对象的类型
>
> getObject：获取对象，可以通过new的方式或者代理的方式。

## spring的aop底层实现原理

>  aop是ioc的一个扩展功能，先有的ioc，再有的aop，他是基于动态代理实现的，在bean的创建过程中，在BeanPostProcessor的后置处理方法中实现。
>
> 1.代理对象的创建过程（advice，切面，切点）
>
> 2.通过jdk或者cglib来生成代理对象。
>
> 3.在执行方法调用的时候，会调用生成的字节码文件中，找到DynamicAdvisoredInterceptor类中的intercept方法，从此处开始执行
>
> 4.根据定义好的通知来生成拦截器链
>
> 5.从拦截器链中依次获取每一个通知开始执行，执行过程中，为了找到下一个通知是谁，会有一个CglibMethodInvocation的对象，	从-1的位置开始查找执行

## Spring的事务是如何回滚的

> spring的事务管理实现
>
> spring的事务是通过aop实现的，首先要按照aop的流程来执行具体的操作逻辑，正常情况下要通过通知来完成核心功能，但是事务不是通过通知来实现的，而是通过一个TransactionInterceptor来实现，然后调用invoke来实现具体逻辑。
>
> 1.先解析各个方法上的事务相关属性，根据具体的属性来判断是否开启新的事务
>
> 2.当需要开启事务的时候，获取数据库连接，关闭自动提交功能，开启事务。
>
> 3.执行具体的sql逻辑
>
> 4.在操作过程中，如果执行失败了，会通过completeTransactionAfterThrowing来完成事务的回滚操作，回滚的具体逻辑是通过doRollBack方法来实现的
>
> 5.在操作过程中，如果执行成功了，会通过completeTransactionAfterReturning来完成事务的提交操作，提交的具体逻辑是通过doCommit方法来实现的
>
> 6.当事务执行完毕后，需要清除相关的事务信息，通过调用cleanupTransactionInfo方法

## Spring的事务传播

> 事务的传播特性：7种
>
> Required,Requires_new,nested,support,not_support,never,mandatory
>
> 事务的传播特性指的是不同方法的嵌套调用过程中，事务应该如何进行处理，是用同一个事务还是不同的事务，当出现异常的时候会进行回滚还是提交，两个方法之间的相关影响，一般使用required，requires_new,nested的比较多
>
> 1.事务的传播特性一般可以分为三类，支持当前的事务，不支持当前的事务，嵌套事务
>
> 2.如果外层方法是required，内层方法是：required，requireds_new,nested
>
> 3.如果外层方法是requires_new，内层方法是：required，requires_new，nested
>
> 4.如果外层方法是nested，内层方法是：required，requires_new,nested
