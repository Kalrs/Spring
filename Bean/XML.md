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


