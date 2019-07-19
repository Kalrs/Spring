# Spring
## 什么是Spring
  Spring是分层的Java SE/EE应用full-stack(一站式)轻量级开源框架，以IoC(Inverse Of Control:反转控制)和AOP(Aspect Oriented Programming:面向切面编程)为内核，提供了展现层Spring MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多第三方框架和类库，成为使用最多的Java EE企业级应用开源框架
## Spring发展历程
  1997年IBM提出EJB的思想；1998年，Sun制定开发标准规范EJB1.0；1999年，EJB1.1发布；2001年，EJB2.0发布；2003年，EJB2.1发布；2006年，EJB3.0发布；  
 **Rod Johnson(Spring之父)**  
     Expert One-to-One J2EE Design and Development(2002)  
     阐述了J2EE使用EJB开发设计的有点及解决方案  
     Expert One-to-One J2EE Development without EJB(2004)  
     阐述了J2EE开发不使用EJB的解决方式(Spring雏形)  
**2017年9月发布了spring的最新版本spring5.0通用版(GA)**
## Spring体系架构
 ![images](spring-overview.png)
 
 **Core Container** 核心容器 
 - 管理bean
 - 核心
 - 上下文(体现在配置文件)
 - SpEL表达式  
 
 **AOP A** 切面编程
 - 切面编程
 - AOP框架  
 
 **Data Access**  数据库操作
 - JDBC Template数据库开发
 - 整合hibernate
 - 事务管理  
 
 **Web** 表现层
 - web开发
 - 整合Struts  
 
 **Test**  测试
 - 整合Junit  
 ## Spring优点
 - 方便解耦，简化开发(高内聚低耦合)
 >Spring就是一个大工厂(用于生产bean)，将所有对象创建和依赖关系，交给Spring管理
 - AOP编程的支持
 >Spring提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能
 - 声明式事务的支持
 >只需要通过配置就可以完成对事务的管理，而无需手动编程
 - 方便程序的测试
 >Spring对Junit4支持，可以通过注解方便的测试Spring程序
 - 方便集成各种优秀的框架
 >Spring不排斥各种优秀的开源框架，其内部提供了对各种优秀框架(如:Struts、Hibermate、MyBatis、Quartz等)的直接支持
 - 降低JavaEE API的使用难度
 >Spring对JavaEE开发中非常难用的一些API（JDBC，JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低
# Spring配置
[IoC实例](/IoC/Ioc.md)  
[DI实例](/IoC/DI.md)
# 核心API
1. BeanFactory：这是一个工厂，用于生成任意bean
>采取延迟加载，第一次getBean才会初始化Bean

2. ApplicationContext：是BeanFactory的子接口，功能更强大
>可以实现国际化处理，事件传递，Bean自动装配，各种不同应用层的Context实现；当配置文件被加载，就进行对象实例化

3. ClassPathXmlApplicationContext：用于加载classpath(类路径、src)下的指定xml
>加载xml运行时的位置-->/WEB-INF/classes/...xml

4. FileSystemXmlApplicationContext：用于加载指定盘符下的xml
>加载xml运行时的位置-->/WEB-INF/...xml 通过javaweb ServletContextgetRealPath()获得具体盘符

# 装配Bean
[装配Bean基于XML](/Bean/XML.md)  
[装配Bean基于注解](/Bean/Annotation.md)
# AOP
### 什么是AOP
> 在软件业，AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率  

- AOP采取**横向抽取**机制，取代了传统**纵向继承**体系重复性代码  
- 经典应用: 事务管理、性能监视、安全检查、缓存、日志等 
- SpringAOP使用纯java实现，不需要专门的编译过程和类加载器，在运行期通过代理方式向目标类织入增强代码
- AspectJ是一个基于Java语言的AOP框架，Spring2.0开始，SpringAOP引入对Aspect的支持，AspectJ扩展了Java语言，提供了一个专门的编译器，在编译时提供横向代码的织入
### AOP实现原理
- AOP底层将采用代理机制进行实现。
- 接口+实现类：spring采用jdk的动态代理Proxy。
- 实现类：spring采用cglib字节码增强
### AOP术语
- target：目标类，需要被代理的类
- Joinpoint(连接点)：指可能被拦截到的方法。(目标类的所有方法)
- PointCut(切入点)：已经被增强的连接点。(被使用的方法)
- advice (通知/增强)：增强的代码
- Weaving(织入)：是指把增强应用到目标对象来创建新的代理对象的过程
- proxy(代理类)
- Aspect(切面)：是切入点pointcut和通知advice的结合
> 一个切入点和一个通知组成一个特殊的面
### AOP联盟为通知Advice定义了org.aopalliance.aop.Advice
- Spring按照通知Advice在目标类方法的连接点位置，可以分为5类
   - 前置通知 org.springframework.aop.MethodBeforeAdvice
在目标方法执行前实施增强
   - 后置通知 org.springframework.aop.AfterReturningAdvice
在目标方法执行后实施增强
   - **环绕通知** org.aopalliance.intercept.MethodInterceptor
在目标方法执行前后实施增强
   - 异常抛出通知 org.springframework.aop.ThrowsAdvice
在方法抛出异常后实施增强
   - 引介通知 org.springframework.aop.IntroductionInterceptor
在目标类中添加一些新的方法和属性

### 环绕通知
- 必须手动执行目标方法.
>
```java
try{
  //前置通知
  //执行目标方法
  //后置通知
}catch(){
  //抛出异常通知
}
```

### Spring代理模式
[JDK动态代理](/AOP/1.JDK动态代理.md)  
[cglib字节码增强](/AOP/2.cglib字节码增强.md)  
[工厂bean代理-半自动](/AOP/4.半自动代理.md)  
[全自动](/AOP/5.全自动代理.md)  


