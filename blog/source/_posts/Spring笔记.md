---
title: Spring笔记
date: 2023-2-12 09:34:49
tags:
    - Spring

categories:
    - 后端开发    

cover: /img/rice.jpg
---


# Spring复习笔记

>
> Spring是开源的免费框架
>
> **轻量级、非入侵式的**
>
> **IOC控制反转， AOP面向切面**编程 					是重点
>
> 支持事务，对框架·整合的指出							也是重点

总结一句话。Spring就是一个轻量级的控制反转和面向切面编程的框架。



## LC1. 任何项目，首先先导这两个maven

```
1. spring-webmvc
2. spring-jdbc

第一个webmvc包含了很多spring的包，比如spring-core，spring-context。这样就不用多次导入不同的包了。

第二个jdbc是用来整合mybatis连接数据库的
```

![image-20230417092051417](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230417092051417.png)



## LC2. Spring简介

> - Springboot快速开发的脚手架。基于springboot可以快速开发单个微服务.
>
> - Springcloud是基于springboot实现的。

Spring起到了承上启下的作用。

弊端： 发展了太久，违背了原来的理念。



> ## LC3. Spring IOC

以前写业务

* UserDao接口

* UserDaoImpl实现类(会有多个)

* UserService 业务接口

* UserServiceImpl业务实现类(会有多个)
* ![image-20230417104203040](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230417104203040.png)

![image-20230417092414541](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230417092414541.png)

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码。



改进1：![image-20230417092618282](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230417092618282.png)

这样就可以动态的注入UserDao到UserServiceImpl中，避免写死。

也就是主动型变了。原来是程序主动创建对象，现在是程序被动接受一个对象。**系统的耦合性降低**。



## LC4: IOC本质

![image-20230417101934817](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230417101934817.png)



采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者何为一体。Bean的定义信息直接以注解的形式定义再类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注释），并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器。其实现方式是依赖注入（Dependency Injection）。**

> **大白话理解Ioc就是。原本对象的创建都是由程序员来控制的。而现在，通过XML中写这些bean标签，以及使用Autowire等注解进行开发，可以让Spring Ioc容器帮我们来创建对象并完成对象的配置。程序员因此只需要获取这些由spring创建好的对象即可**



## LC5: Bean Definition Inheritance (Bean标签的理解)

> "Bean definition can contain a lot of configuration information, including **constructor arguments**, **property values**, and **container-specific information**, such as the **initialization method**, a static factory method name, and so on. "

需要掌握的代码

```java
ApplicationContext applicationConext = new ClassPathXmlApplicationContext("BeanName.xml");
```



## LC6. Spring对象的创建

1. Spring容器默认使用无参构造创建对象
2. 假如需要使用有参构造创建对象， 三种方式 <constructor-arg>
    1.  下标 index
    2. 参数类型 type   -> 不建议使用，多个同类型的参数，会乱。
    3. 直接通过参数名 name.
        * 也可以用ref 对象的方法。和通过参数名类似。

**总结： 在配置文件加载的时候，容器中管理的对象就已经被初始化了。**

> 面试： BeanFactory相关。
>
> BeanFactory是什么？FactoryBean又是什么？Bean存哪里, 怎么存？ BeanPostProcessor是什么？ 这些高频面试题都不讲，确实拉了



### 1.BeanPostProcessor 是什么？

> "The `BeanPostProcessor` interface defines callback methods that you can implement to provide your own (or override the container’s default) instantiation logic, dependency resolution logic, and so forth. If you want to implement some custom logic after the Spring container finishes instantiating, configuring, and initializing a bean, you can plug in one or more custom `BeanPostProcessor` implementations."

首先， BeanPostProcessor是一个接口，定义了一些方法，来实现你对于对象创建时初始化的逻辑。当Spring Ioc容器实例化bean之后，BeanPostProcessor就开始工作了。







## LC7. Spring配置详解

### 1. 别名

<bean> 标签的name和alias都是别名。getBean的时候，使用这两个标签配置的对象都可以被get到。

使用name时，可以同时取多个别名。别名可以逗号，空格，分号分隔。



### 2. Import

将多个配置文件导入到一个xml, 通常是把所有的xml导入到applicaitonContext.xml下

```xml
<import resource="beans1.xml">
<import resource="beans2.xml">    
<import resource="beans3.xml">    
```



## LC8. 依赖注入

### 1. 构造器注入

​	前面讲过了

```xml
<constructor-arg>
```



### 2. Set方式注入

> 各种数据类型的注入
>
> ![image-20230417165807121](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230417165807121.png![image-20230417165836555](C:\Users\18795\AppData\Roaming\Typora\typora-user-images\image-20230417165836555.png)



### 3. 拓展方式注入

命名空间注入：

比如 p命名空间，直接能够注入property

```
<beans id="xxx" class="com.xxx.xxx" p:name="xxx" p:age="11";
```

以及  c命名空间，其实就是constructor-arg



## LC9. Bean的作用域

1. 单例模式（默认）
2. 原型模式



## LC10. Bean的自动装配

> 1. **如果是byName，那么你的bean的id 必须严格遵守你在pojo里面定义的set方法中的后缀名。比如pojo中一个方法叫setXxx()。 注意本例中Xxx是一个引用类型的对象，那么，在你的bean id里面，只能写xxx（全小写，否则报错）。不能写xxx1   xxx2 ，Xxx 会报错**
> 2. **而byType相反。你bean id想怎么写就怎么写。** 管你xxx1 1xxx  xx1x, 只要class 是 com.example.xxx, spring都能通过反射创建你的对象.



Spring 在 2.5版本提供了对注解的支持。如果要使用注解，需要导入context约束。

可以直接去Spring官网复制黏贴代码就可以。节约时间。

链接： https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-annotation-config

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

复制完这个配置类之后，我们的spring就可以通过注解来实现自动装配了。



补充:

```
<context:annotation-config/> 隐式的注册了以下的PostProcessors

	ConfigurationClassPostProcessor

	AutowiredAnnotationBeanPostProcessor

	CommonAnnotationBeanPostProcessor

	PersistenceAnnotationBeanPostProcessor

	EventListenerMethodProcessor


```



### 1. Autowired可以用在哪里

* 构造器上

    * 注意! 如果由多个constructor并且没有无参构造的话，至少得有一个constructor上面需要加autowired. 这样Ioc才知道用什么。

      ```
      As of Spring Framework 4.3, an @Autowired annotation on such a constructor is no longer necessary if the target bean defines only one constructor to begin with. However, if several constructors are available and there is no primary/default constructor, at least one of the constructors must be annotated with @Autowired in order to instruct the container which one to use. See the discussion on constructor resolution for details.
      ```

*  传统的set方法上

```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Autowired
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```



* 带参的方法上

  ```
  public class MovieRecommender {
  
      private MovieCatalog movieCatalog;
  
      private CustomerPreferenceDao customerPreferenceDao;
  
      @Autowired
      public void prepare(MovieCatalog movieCatalog,
              CustomerPreferenceDao customerPreferenceDao) {
          this.movieCatalog = movieCatalog;
          this.customerPreferenceDao = customerPreferenceDao;
      }
  
      // ...
  }
  ```



* 成员变量和构造方法上

  ```java
  public class MovieRecommender {
  
      private final CustomerPreferenceDao customerPreferenceDao;
  
      @Autowired
      private MovieCatalog movieCatalog;
  
      @Autowired
      public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
          this.customerPreferenceDao = customerPreferenceDao;
      }
  
      // ...
  }
  
  ```



* 如果想让Spring为相同类型的多个bean的话，可以使用数组、集合

  ```
  public class MovieRecommender {
  
      @Autowired
      private MovieCatalog[] movieCatalogs;
  
      // ...
  }
  
  public class MovieRecommender {
  
      private Set<MovieCatalog> movieCatalogs;
  
      @Autowired
      public void setMovieCatalogs(Set<MovieCatalog> movieCatalogs) {
          this.movieCatalogs = movieCatalogs;
      }
  
      // ...
  }
  
  
  ```



### 2. Autowire失败条件

> utowiring fails when no matching candidate beans are available for a given injection point.



### 3. @Autowiresd(required= false) 意思

```
In other words, setting the required attribute to false indicates that the corresponding property is optional for autowiring purposes, and the property will be ignored if it cannot be autowired. This allows properties to be assigned default values that can be optionally overridden via dependency injection.
```

其实就是不需要，但是有机会还是想注入。替代方案有Java.util.Optional 和  Nullable。



### 4. 什么是Qualifier

> 多个同类型的bean实现autowire的时候使用
>
> ```xml
> @AutoWired
> @Qualifier(value=xxx1)
> private Dog dog;
> 
> 
> 
> 
> <xml ....>
> 
> <bean id='xxx1' class="dog">
> <bean id="xxx", class = "dog">
> ```



### 5. Resource注解

> Javax.annotation.resource
>
> Ioc查找时。要么xml中只有一个该类型bean。要么bean id和set后缀想通同。不然找不到。也可以用name来找到。



引申问题： Resource和Autowire的区别？





## LC11 注解开发

关键字：@ component

在spring4之后，需要导入aop包，来开启注解开发。

```
spring-aop
```



流程：

1. ```xml
    <context:annotation-config> 
    
    ```

2. ```xml
    <context:component-scan base-package="com.example.xxx">
    <!--这里 base-package是指定要扫描的包。这个包下的所有注解都会生效-->
    ```



@component 挂在类上，代表这个类被spring管理了

@Value挂在变量上面，代表这个变量的值

```java
@Value("anicaa")
private String name;
```

* 也可以挂在set方法上。同理

  ```java
  @Value("anicaa")
  public void setName(String name){
  	this.name = name;
  }
  ```



@Scope，就是决定单例，多例的注解。同样挂在类上。

```java
@Scope("prototype")
@Scope("singleton")
```



### @components注解的衍生

> 在web开发中，我们会按照model-view-controller三层架构来分层。
>
> 1. dao层
> 2. service层
> 3. controller层
>
> **每层的注解都是特种的。dao 是 @Repository, serivce是 @Service, controller 是 @Controller。**



## LC12. 使用纯Java配置Spring

> 旨在离开xml配置，只使用java就能完成配置。

```java
//UserConfig类

package com.example.config;


import com.example.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class UserConfig {

    @Bean
    public User getUser(){
        return new User();
    }

}

```

```java
//User类

package com.example.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    @Value("anicaa")
    private String name;

    public String getName(){
        return this.name;
    }
}

```

```java
测试代码
package com.example.springconfig;

import com.example.config.UserConfig;
import com.example.pojo.User;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class SpringConfigApplication {

    public static void main(String[] args) {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(UserConfig.class);
        User user = (User) applicationContext.getBean("getUser");
        System.out.println(user.getName());
    }
}

```

- 挂在方法上的@Bean, 其方法名就是bean id。返回值就是Bean class.

- @ComponentScan("com.exmaple.xxx"). 挂在config类头上，就是扫描。
- @Import() 导入别的config类.



## LC13. AOP

> Spring中比较重要的模式： 代理模式 和 工厂模式

### 设计模式-代理模式（AOP底层）

#### 1. 静态代理

> 静态代理的**静态**，体现在他的代理是被“写死的“。以UserService以及其接口的实现类为例

1. User Service接口

```java
package com.example.test;

public interface UserService {
    public void add();
    public void delete();
    public void query();
    public void update();
}

```

2.  UserServiceImpl 实现类

```java
package com.example.test;

public class UserServiceImpl implements UserService{


    @Override
    public void add() {
        System.out.println("添加了");
    }

    @Override
    public void delete() {
        System.out.println("删除了");
    }

    @Override
    public void query() {
        System.out.println("查询了");
    }

    @Override
    public void update() {
        System.out.println("更新了");
    }
}

```

3. UserServiceProxy 静态代理类。

```
package com.example.test;

import com.example.pojo.User;

public class UserServiceProxy implements UserService {
    private UserServiceImpl userService;

    public UserServiceProxy(){

    }

    public UserServiceProxy(UserServiceImpl userService){
        this.userService = userService;
    }

    @Override
    public void add() {
        log("add");
        userService.add();
    }

    @Override
    public void delete() {
        log("delete");
        userService.delete();
    }

    @Override
    public void query() {
        log("query");
        userService.query();
    }

    @Override
    public void update() {
        log("update");
        userService.update();
    }

    public void log(String str){
        System.out.println("call " + str);
    }
}

```

4.Client 类. 调用了代理，执行接口的放法。

```
package com.example.test;

public class Client {
    public static void main(String[] args) {
        UserServiceImpl userService = new UserServiceImpl();
        UserServiceProxy userServiceProxy = new UserServiceProxy(userService);
        userServiceProxy.add();
        userServiceProxy.delete();
        userServiceProxy.query();
        userServiceProxy.update();
    }
}

```

在这里, 写死体现在了静态代理类中的log方法。这种需要经常改动的类，可以通过InvocationHandler来实现动态的改动。



#### 2. 动态代理

> 而动态代理的作用就在于，不需要申明一个代理类。只需一个InvocationHandler就可以作为模板，动态的代理多个接口。
>
> 在代理模式中，接口可以被看作是房东。而接口的实现类，以及实现类中的方法，就是房东的房子里的房间。
>
> 原先，我们需要创建一个静态代理类，来管理这些实现类，以及调用实现类中的方法。这就像是房东找了中介来出租房子里的每个房间。
>
> 现在，通过动态代理。我们可以直接把房子交给中介。也就是把实现类交给InvocationHandler
>
> ```java
> setTarget(Object target)
> ```
>
> 就能设定需要代理的接口。InvocationHandler会通过反射，找到实现类**所对应的接口**，并把这个接口返回给我们。
> 之后，Client可以直接通过这个返回来的对象调用其方法，实现代理。



1. InvocationHandler

```
package com.example.test;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class UserServiceInvocationHandler implements InvocationHandler {
    //被代理的对象
    private Object target;

    //获取代理对象
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(),this);
    }

    public void setTarget(Object target){
        this.target = target;
    }
    
    //代理对象以及代理调用方法时候，都会invoke这个
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        return method.invoke(target, args);
    }

    private void log(String msg){
        System.out.println("Calling" + msg);
    }
}

```

2. Client

```
package com.example.test;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class UserServiceInvocationHandler implements InvocationHandler {
    //被代理的对象
    private Object target;

    //获取代理对象
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(),this);
    }

    public void setTarget(Object target){
        this.target = target;
    }
    //代理对象以及代理调用方法时候，都会invoke这个
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        log(method.getName());
        return method.invoke(target, args);
    }

    private void log(String msg){
        System.out.println("Calling" + msg);
    }
}

```



## LC14. Spring AOP
