## Bean的实例化方式
### 默认构造
```java
<bean id="" class=""> //必须提供默认构造
````



### 静态工厂
- 常用与spring整合其他框架
- 用于生成实例对象，所有的方法必须是static
```java
 <bean id="" class="工厂权限定类名（包名+类名）"  factory-method="静态方法">
```
##### 代码
- 工厂
>
```java
package Lee.Bean.XML.static_factory;

public class MyBeanFactory {
    /**
     * 创建实例
     *
     * @return
     */
    public static UserService createService(){
            return new UserServiceImpl();
    }
}
```

- Spring配置
>
```java
     <!--将静态工厂的创建实例交予spring
      class确定静态工厂的权限定名
      factory-method确定静态方法名
     -->
        <bean id="userServiceId" class="Lee.Bean.XML.static_factory.MyBeanFactory" factory-method="createService"></bean>
```


### 实例工厂
- 实例工厂：必须现有工厂实例对象，通过实例对象创建对象。提供所有的方法都是非静态的
##### 代码
- 工厂
>
```java
public class MyBeanFactory {
    /**
     * 创建实例工厂
     *
     * @return
     */
    public  UserService createService(){
            return new UserServiceImpl();
    }
}

```

- Spring配置
>
```java
    <!--创建工厂实例-->
    <bean id="myBenaFactoryId" class="Lee.Bean.XML.factory.MyBeanFactory"></bean>
    <!--获得userService
    factory-bean 确定工厂实例
    factory-method 确定方法
    -->
    <bean id="userServiceId" factory-bean="myBenaFactoryId" factory-method="createService"></bean>
```
## Bean种类 👀   
- 普通bean：之前操作的都是普通bean。``` <bean id="" class="A">```,spring直接创建A实例，并返回
- FactoryBean：是一个特殊的bean，具有工厂生成对象的能力，只能生成特定对象
>bean必须使用FactoryBean接口,此接口提供方法getObject()用于获得特定bean 
```<bean id="" class="FB">先创建FB实例，再调用getObject方法，并返回方法的返回值
    FB fb=new FB();
    return fb.getObject 
```
- BeanFactory和FactoryBean对比  
BeanFactory：工厂，用于生成任意bean  
FactoryBean：特殊bean，用于生成另一个特定bean，例如：proxyFactoryBean，用于生产代理```<bean id="" class="...ProxyFactoryBean">获得代理对象实例。AOP使用

## 作用域
- 用于确定spring创建bean实例个数
- 取值
  - singleton 单例，默认值
  - prototype 多例，每执行一次，将获得一个实例。
- 配置信息
```<bean id="" class="" scope="">```

![image](/scope.png)

## 生命周期
### 初始化和销毁
  ##### 目标方法执行前后，将进行初始化或销毁
    <bean id="" class="" init-method="初始化方法名称" destroy-method="销毁的方法名称"
 - 目标类 
   ```java
    public class UserServiceImpl implements UserService {
    @Override
    public void addUser() {
        System.out.println("life add user");
    }
    public void myInit(){
        System.out.println("初始化");
    }
    public void myDestory(){
        System.out.println("销毁");
    }
     ```
 - 配置信息
   ```java
         <!--
        init-method  用于配置初始化方法，准备数据等
        destroy-method 用于配置销毁方法，清理资源等
        -->
        <bean id="userServiceId" class="Lee.Bean.XML.lifestyle.UserServiceImpl"
        init-method="myInit" destroy-method="myDestory"
        >
        </bean>
   ```
### BeanPostProcessor （后处理Bean）
- spring提供一种机制，只要实现此接口，并将实现类提供给spring容器，spring容器将自动执行，在初始化前执行before()，初始化方法后执行after()，配置```<bean class="">```
![image](/beanPostPro.png)
- spring 提供工厂勾子，用于修改实例对象，可以生成代理对象，是AOP底层
>
```java
//模拟
 A a=new A();
 a=(A)B.before(a);  //将a的实例对象传递给后处理bean 可以生成代理对象并返回
 a.init();
 a=(A)B.after(a);
 
 a.addUser();   //生成代理对象，目的在目标方法前后执行(例如：开启事务、提交事务)
 
 a.destroy();
```

## 属性注入
### setter方法
- 配置信息
> 
```java
 <!--setter方法注入
        * 普通数据
            <property name="" value="值"></property>
            等效
            <property name="">
            <value>123<value>
            </property>
        * 引用数据
             <property name="" ref="另一个bean的名字"></property>
             等效
            <property name="">
                <ref bean=""></ref>
            </property>

    -->
        <bean id="personId" class="Lee.Bean.XML.setter.Person">
            <property name="name" value="李大叔啊"></property>
            <property name="age">
                <value>1234</value>
            </property>
            <property name="homeaddr" ref="AddressId"></property>
            <property name="comaddr">
                <ref bean="comAddressId"></ref>
            </property>
        </bean>
        <bean id="AddressId" class="Lee.Bean.XML.setter.Address">
            <property name="addr" value="四川"></property>
            <property name="tel" value="911"></property>
        </bean>
         <bean id="comAddressId" class="Lee.Bean.XML.setter.Address">
              <property name="addr" value="广州"></property>
              <property name="tel" value="119"></property>
    </bean>
 ```
 ### p命名空间[了解]
 - 对"setter方法注入"进行简化，替换<property name="属性名">,而是在<bean p:属性名="普通值" p:属性名-ref="引用值">
 - p命名空间使用前提，必须添加命名空间
 - 配置信息  
 
![images](pnamespase.png)
 >
 ```java
 
     <bean id="personId" class="Lee.Bean.XML.p.Person"
          p:age="123"
          p:name="李大叔"
          p:homeaddr-ref="Address"
          p:comaddr-ref="comAddress"
     >
     </bean>
     <bean id="Address" class="Lee.Bean.XML.p.Address"
        p:addr="DG" p:tel="112"
     >
     </bean>
     <bean id="comAddress" class="Lee.Bean.XML.p.Address"
        p:addr="DG" p:tel="123"
     >
    </bean>
 ```

### SpEL[了解]
- 对<property>进行统一编程，所有的内容都使用value <property name="" value="#{表达式}">
>#{123} #{'jack'}: 数字 字符串
 #{beanId} :另一个bean引用
 #{beanId.propName} :操作数据
 #{beanId.toString()} :执行方法
 #{T(类).字段、方法}  :静态方法或字段
 
- 配置信息
>
```java
  <!--
        <property name="uname" value="#{'jack'}"></property>
        <property name="uname" value="#{UserId.uname.toUpperCase()}"></property>
        通过另一个bean，获得属性，调用方法
        <property name="uname" value="#{UserId.uname?.toUpperCase()}"></property>
          ?  如果对象不为null，则调用方法
     -->
        <bean id="UserId" class="Lee.Bean.XML.SpEl.User">
        <property name="uname" value="#{UserId.uname?.toUpperCase()}"></property>
        <property name="pi" value="#{T(java.lang.Math).PI}"></property>
        </bean>
```
### 集合注入
- 配置信息
>
```java
       <!--
         集合的注入都是给<property>添加子标签
         数组:<array>
         List:<list>
         Set:<set>
         Map:<map>  map存放键值对，使用<entry>描述 
         Properties:<props> <prop key="">值</prop>

         普通数据：value
         引用数据：ref
        -->
        <bean id="CollDataId" class="Lee.Bean.XML.coll.CollData">
                <property name="arrayData">
                        <array>
                                <value>DS</value>
                                <value>DZD</value>
                                <value>大叔</value>
                                <value>定制的</value>
                        </array>
                </property>
                <property name="listData">
                        <list>
                                <value>李大叔</value>
                                <value>LDS</value>
                        </list>
                </property>
                <property name="setData">
                        <set>
                                <value>哦豁</value>
                                <value>根本</value>
                                <value>学不会</value>
                        </set>
                </property>
                <property name="mapData">
                        <map>
                                <entry key="大哥" value="李大叔"></entry>
                                <entry key="大嫂" value="萌萌"></entry>

                        </map>
                </property>
               <property name="propsData">
                       <props>
                        <prop key="蓝天">白云</prop>
                        <prop key="林深">见鹿</prop>
                       </props>
               </property>
        </bean>
```
