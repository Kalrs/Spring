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
 
 ## Spring优点
 - 方便[解耦](README.md#程序的耦合-demo)，简化开发(高内聚低耦合)
 >Spring就是一个大厂，将所有对象创建和依赖关系，交给Spring管理
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
 
 
 
 ### 程序的耦合
 #### 耦合:程序间的依赖关系
 - 类之间的依赖
 - 方法之间的依赖
 #### 解耦:降低程序间的依赖关系
**解耦思路**
   1. 使用反射来创建对象，而避免使用new关键字
   2. 通过读取配置文件来获取要创建的对象权限定类名
 
