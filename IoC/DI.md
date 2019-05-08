### DI:Dependency Injiection 依赖注入
>is-a 继承  
>has-a 成员变量 依赖  
>🌰举个栗子 
```java
  class B{
    private A a;
  }
```

**依赖**：一个对象需要使用另一个对象  
**注入**：通过setter方法进行另一个对象实例设置
>
```java
    classBookService{
    //之前开发 接口=实现类
      private BookDao bookDao=new BookDaoImpl();
    //spring之后
      private BookDao bookDao
      setter方法
    }
   //模拟spring执行过程
   创建service实例：BookService bs=new BookServiceImpl();  -->Ioc  <bean>
   创建dao实例：BookDao bookDao=new BookDaoImpl();         -->Ioc
   将dao设置给service:  bookService.setBookDao(bookDao)    -->DI   <property>
```
#### 目标类
- 创建BookService接口和实现类
- 创建BookDao接口和实现类
- 将dao和service配置到xml文件
- 测试api

>
```java
1. dao
package Lee.DI;

public interface BookDao {
    public void addBook();
}

-----
package Lee.DI;

public class BookDaoImpl implements BookDao{
    @Override
    public void addBook() {
        System.out.println("DI add book");
    }
}

2. service
package Lee.DI;

public interface BookService {
    public void addBook();
}
----
package Lee.DI;

public class BookServiceImpl implements BookService {
    //方式一:之前，接口=实现类
    //方式2:接口+setter
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
    @Override
    public void addBook() {
            this.bookDao.addBook();
    }
}

3. Test
package Lee.DI;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void demo(){
        //从Spring容器获得
        String xmlPath="Lee/DI/bean.xml";
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext(xmlPath);
        BookService bs=(BookService) applicationContext.getBean("bookServiceId");
        bs.addBook();
    }
    public static void main(String[] args) {
        demo();
    }
}
```
#### 配置文件
>
```java
    <!--配置Service
    模拟Spring执行过程
    创建service  BookService bs=new BookServiceImpl(); IoC bean
    创建dao实例: BookDao bd=new BooDaoImpl();  Ioc
    将dao设置给service: bs.setBookDao(bd);  DI property
    <property>  用于属性注入
            name： bean属性名，通过setter方法获得
            ref ： 另一个bean的id值

    -->
    <!--创建service-->
    <bean id="bookServiceId" class="Lee.DI.BookServiceImpl">
        <property name="bookDao" ref="bookDaoId"></property>
    </bean>
    <!--创建dao-->
    <bean id="bookDaoId" class="Lee.DI.BookDaoImpl"></bean>

```

