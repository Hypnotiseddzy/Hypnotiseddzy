# Spring

* 理念：使现有的技术更加容易使用，整合现有的技术框架
* SSM：SpringMVC+Spring+Mybatis

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.13</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.13</version>
</dependency>

```

* Spring是一个轻量级的、非入侵式的框架！
* 控制反转（IOC），面向切面编程（AOP）
* 支持事务的处理，对框架整合的支持！

==总结一句话：Spring是一个轻量级的控制反转（IOC）和面向切面编程的框架==

## 组成



* SpringBoot
  * 一个快速开发的脚手架
  * 基于SpringBoot可以快速开发单个微服务
  * 约定大于配置
* SpringCloud
  * 基于SpringBoot实现

Spring+SpringMVC--------SpringBoot

# IOC理论推导

1、UserDao接口

2、UserDaoImpl实现类

3、UserService业务接口

4、UserServiceImpl实现类



之前的业务中，用户的需求可能影响我们原来的代码，需要根据用户的需求来修改源代码！如果程序代码量十分大，修改一次的成本代价十分昂贵！



==使用set接口实现==

```java
private UserDao userDao;
public void setUserDao(UserDao userDao){
    this.userDao=userDao;
}
```

变成由用户实现，无需修改代码；系统的耦合性大大降低，可以更加专注在业务的实现

https://mp.weixin.qq.com/s/VM6INdNB_hNfXCMq3UZgTQ

**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

# HelloSpring

创建项目hellospring并运行

- Hello 对象是谁创建的 ?  hello 对象是由Spring创建的
- Hello 对象的属性是怎么设置的 ?  hello 对象的属性是由Spring容器设置的

这个过程就叫控制反转 :

- 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的
- 反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

依赖注入 : 就是利用set方法来进行注入的.

 IOC是一种编程思想，由主动的编程变成被动的接收

我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 ! 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- services -->
    <bean id="BaseImpl" class="com.dzy.Dao.Impl.UserDaoImpl"/>
    <bean id="MysqlImpl" class="com.dzy.Dao.Impl.UserDaoMysqlImpl"/>
    <bean id="OracleImpl" class="com.dzy.Dao.Impl.UserDaoOracleImpl"/>

    <bean id="UserServiceImpl" class="com.dzy.Service.Impl.UserServiceImpl">
<!--        ref:引用Spring中创建好的对象 value：具体的值，基本数据类型-->
        <property name="userDao" ref="MysqlImpl"/>
    </bean>

    <!-- more bean definitions for services go here -->

</beans>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--使用Spring来创建对象，这些在Spring中都叫做Bean

    id为变量名，class为new的对象，property：给对象中的属性设置一个值

-->
    <bean id="hello" class="com.dzy.pojo.Hello">
        <property name="name" value="Spring"></property>
    </bean>
    <!-- more bean definitions go here -->

</beans>
```

# IOC创建对象的方式

1.默认使用无参构造器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.dzy.pojo.User">
        <property name="name" value="dzy"/>
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

2.使用有参构造器创建对象

​	1.下标赋值

```xml
<bean id="user" class="com.dzy.pojo.User">
    <constructor-arg index="0" value="dzy2"/>
</bean>
```

​	2.通过类型创建（不推荐）

```xml
<bean id="user" class="com.dzy.pojo.User">
    <constructor-arg type="java.lang.String" value="dzy3"/>
</bean>
```

​	3.直接通过参数名来赋值

```xml
<bean id="user" class="com.dzy.pojo.User">
    <constructor-arg name="name" value="dzy4"/>
</bean>
```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了！ 

# Spring配置

## 别名

```xml
<alias name="user" alias="user2"/>
```

## Bean的配置

id：Bean的唯一标识符，相当于对象名

class：Bean对象所对应的（全限定名）实体类

name：也是别名，可以同时取多个别名

## import

一般用于团队开发，可以将多个配置文件导入合并为1个

在总的xml文件中import不同人员开发的xml，相当于合并成总的，使用时直接使用总的。（applicationcontext.xml）

# DI（依赖注入）

1.构造器注入

2.set方式注入【重点】

* 依赖注入：Set注入！
  * 依赖：bean对象的创建依赖于容器
  * 注入：bean对象的所有属性，由容器来注入

Java实体类：Student，Address

```java
@Data
@ToString
public class Address {
    private String address;
}

@Data
@ToString
public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> game;
    private String wife;
    private Properties info;
}
```

配置文件beans.xml（部分）

```xml
    <bean id="address" class="com.dzy.pojo.Address">
        <property name="address" value="宁波"/>
    </bean>

    <bean id="student" class="com.dzy.pojo.Student">
<!--第一种 普通值注入 name value-->
        <property name="name" value="dzy"/>
<!--第二种 Bean注入，name ref-->
        <property name="address" ref="address">

        </property>
<!--数组注入-->
        <property name="books">
            <array>
                <value>书1</value>
                <value>Book2</value>
                <value>Book3</value>
            </array>
        </property>
<!--List注入-->
        <property name="hobbys">
            <list>
                <value>Listening</value>
                <value>Coding</value>
            </list>
        </property>
<!--Map注入-->
        <property name="card">
            <map>
                <entry key="IDcard" value="1111111"/>
            </map>
        </property>
<!--Set注入-->
        <property name="game">
            <set>
                <value>LOL</value>
                <value>COD</value>
            </set>
        </property>
<!--null值注入-->
        <property name="wife">
            <null/>
        </property>
<!--Properties注入-->
        <property name="info">
            <props>
                <prop key="学号">22151069</prop>
                <prop key="性别">男</prop>
                <prop key="姓名">dzy</prop>
            </props>
        </property>
    </bean>
```

最后Student的toString返回结果：

> ​	Student(name=dzy, address=Address(address=宁波), books=[书1, Book2, Book3], hobbys=[Listening, Coding], card={IDcard=1111111}, game=[LOL, COD], wife=null, info={学号=22151069, 性别=男, 姓名=dzy})

3.拓展方式注入

* p命名空间
* c命名空间

```xml
<!--p命名空间注入，可以直接注入属性的值-->
    <bean id="user" class="com.dzy.pojo.User" p:id="1" p:name="dzy"/>
    <bean id="user2" class="com.dzy.pojo.User" c:id="2" c:name="dzy"/>
```

注意点：p命名和c命名空间，不能直接使用，需要导入xml约束。

```xml
xmlns:p="http://www.springframework.org/schema/p" 
xmlns:c="http://www.springframework.org/schema/c"
```

4.bean的作用域

* 单例模式（Spring默认机制）

* ```xml
  <bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
  ```

* 原型模式：每次从容器中get的时候，都会产生一个新对象

* ```xml
  <bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
  ```

* request session application

# Bean的自动装配

* 自动装配是Spring满足Bean依赖的一种方式！
* Spring会在上下文中自动寻找，并自动给bean装配属性！

在Spring中有三种自动装配方式

​	1.在xml中显示的配置

​	2.在java中显示配置

​	3.隐式的自动装配bean

测试：

```java
//实体类Cat
public class Cat {
    public void shout(){
        System.out.println("miao");
    }
}
//实体类Dog
public class Dog {
    public void shout(){
        System.out.println("wang");
    }
}
//实体类People
@Data
@ToString
public class People {
    private Cat cat;
    private Dog dog;
    private String name;
}

```

配置文件beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="cat" class="com.dzy.pojo.Cat"/>
    <bean id="dog" class="com.dzy.pojo.Dog"/>
    <bean id="people" class="com.dzy.pojo.People">
        <property name="name" value="dzy"/>
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
    </bean>
    <!-- more bean definitions go here -->

</beans>
```

测试类

```java
public class mytest {
    public static void main(String[] args) {
        ApplicationContext context=new ClassPathXmlApplicationContext("beans.xml");
        People people=(People) context.getBean("people", People.class);
        System.out.println(people);
        people.getCat().shout();
        people.getDog().shout();
    }
}
```

***ByName ByType自动装配***

```xml
<!--byName:会在容器上下文中自动查找，和自己对象set方法后面对应的bean的id（这里会自动寻找cat，dog）-->
<!--byType:会在容器上下文中自动查找，和自己对象类型相同的bean-->
<bean id="people" class="com.dzy.pojo.People" autowire="****">
    <property name="name" value="dzy"/>
</bean>
```

* 在ByName的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
* 在ByType的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的类型一致

***注解实现自动装配***

使用注解须知：

​	1.导入约束 context约束 

​	2.配置注解的支持 ==<<context:annotation-config/>>==

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

@Nullable

字段上标明这个注解表示该字段可为空

@Component

组件，放在类上，说明这个类被Spring管理了，就是Bean

@Autowired

直接在类的属性上用即可，也可以在set方法上使用

使用后可以不用编写Set方法，前提是这个自动装配的属性在IOC（Spring）容器中存在，且符合名字ByName

```java
@Data
@ToString
public class People {
    //如果定义了Autowired的required属性为false，说明这个对象可以为null，否则不允许为空
    @Autowired(required=false)
    private Cat cat;
    @Autowired
    @Qualifier(value="xxx")
    private Dog dog;
    private String name;
}
//在xml中配置后即可实现自动注入
```

如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以通过使用@Qualifier(value="xxx")去配置@Autowired的使用，指定唯一的一个bean对象来注入。



@Resource

```java
    @Resource(name = "dog222")
    private Dog dog;
```



* 都是用来自动装配的，都可以放在属性字段上
* @Autowired通过ByType的方式实现
* @Resource默认通过ByName的方式实现，如果找不到名字，则通过ByType实现！若两个都找不到 就报错！
* 执行顺序不同

# 使用注解开发

* 使用注解开发必须保证daoruaop包
* 使用注解需要导入context约束，增加注解支持

1.bean

2.属性如何注入

```java
//等价于<bean id="user" class="com.dzy.pojo.User"/>
//@Component装配组件
@Component
public class User {
    @Value("dzy")
    public String name;
}
```





3.衍生

​	@Component有几个衍生注解，我们在Web开发中，会按照mvc三层架构分层！

* dao【@Repository】

* service【@Service】

* controller【@Controller】

  都是将某个类注册到Spring中，装配Bean

4.作用域

@Scope("***") 单例模式 原型模式……

5.小结

​	xml与注解：

* xml更加万能，适用于任何场合，维护简单方便
* 注解 不是自己的类不能使用，维护相对复杂

xml与注解最佳实践：

* xml用来管理Bean；
* 注解只负责完成属性的注入；
* 在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持

# 使用Java的方式配置Spring

我们现在要完全不使用Spring的xml配置了，全权交给Java来做！

JavaConfig是Spring的一个子项目，在Spring4之后，成为了一个核心功能。

```java
@Data
@ToString
public class User {
    @Value("dzy111")
    private String name;
}


package com.dzy.config;


import com.dzy.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration//也会被Spring托管，注册到容器中，因为它本来就是一个@Component
public class DzyConfig {
    //注册一个Bean，就相当于写一个Bean标签
    //方法的名字相当于标签中的id
    //方法的返回类型相当于标签中的class
    @Bean
    public User getUser(){
        return new User();//就是返回需要注入到Bean的对象
    }
}

import com.dzy.config.DzyConfig;
import com.dzy.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class mytest {
    public static void main(String[] args) {
        ApplicationContext context=new AnnotationConfigApplicationContext(DzyConfig.class);
        User user=(User) context.getBean("getUser");
        System.out.println(user);
    }
}

```

这种纯Java的配置方式，在SpringBoot种随处可见

# 代理模式

代理模式：SpringAOP的底层！【SpringAOP和SpringMVC】

代理模式的分类：

* 静态代理

  角色分析：

  * 抽象角色：一般会使用接口或者抽象类来解决
  * 真实角色：被代理的角色
  * 代理角色：代理真实角色，代理真实角色后，一般会做一些附属操作
  * 客户：访问代理对象的人

  项目：spring-08-proxy

* 动态代理

* 和静态代理角色一样

代理模式的好处：

* 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
* 公共业务就交给了代理角色，实现了业务的分工
* 公共业务发生扩展的时候，方便集中管理

缺点：

* 一个真实角色就会产生一个代理角色，代码量会翻倍



* 动态代理的代理类是动态生成的，不是我们直接写好的
* 分为两大类：基于接口的动态代理，基于类的动态代理
  * 基于接口---JDK动态代理【目前使用】
  * 基于类： cglib
* 需要了解两个类：Proxy：代理，InvocationHandler  调用处理程序
* 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
* 公共业务就交给了代理角色，实现了业务的分工
* 公共业务发生扩展的时候，方便集中管理
* 一个动态代理类代理的是一个接口，一般就是对应的一类业务
* 一个动态代理类可以代理多个类，只要是实现了同一个接口

# Spring AOP

使用Spring实现AOP

* 1.使用Spring的接口【Spring API接口实现】
* 2.自定义实现AOP【切面定义】
* 3.使用注解实现

# MyBatis整合

项目文件在spring-10-mybatis

参考：https://mp.weixin.qq.com/s?__biz=Mzg2NTAzMTExNg==&mid=2247484144&idx=1&sn=768f97da78a9ceae8321d101da3c480e&scene=19

1.编写数据配置源

2.SqlSessionFactory

3.SqlSessionTemplate

4.给接口添加实现类

5.将自己写的实现类注入到Spring中

6.测试

# 声明式事务

* 回顾事务
  * 把一组业务当成一个业务来做，要么都成功，要么都失败
  * 事务在项目开发中，十分重要，涉及到数据的一致性问题，不能马虎
  * 确保完整性和一致性
* 事务ACID原则
  * 原子性
  * 一致性
  * 隔离性
    * 多个业务只能操作同一个资源，防止数据损坏
  * 持久性
    * 事务一旦提交，无论系统发生什么问题，结果都不会受到影响，被持久化地写到存储器当中
* Spring的事务管理
  * 声明式事务：AOP
  * 编程式事务：
* 为什么需要事务？
  * 如果不配置事务，可能存在数据提交不一致的情况；
  * 如果不在Spring中配置声明式事务，我们就需要在代码中手动配置事务！
  * 事务在项目的开发中十分重要，涉及到数据的一致性和完整性问题，不容马虎
