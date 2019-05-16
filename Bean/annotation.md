## 装配Bean基于注解
- 就是一个类，使用@注解名称 
- 开发中: 使用注解 取代xml配置文件。
>
```
    @Component 取代 <bean class="">
    @Conponent("id") 取代<bean id="" class="">
    
    //注解使用前提，添加命名空间，让spring扫描含有注解的类
    
```
![images](context.png)



 
