# 1.Spring

> 视频参考：https://www.bilibili.com/video/BV1WE411d7Dv

## 1.1、简介

- Spring：春天----->给软件行业带来了春天
- 2002，首次推出了Spring框架的雏形：interface21框架
- Spring框架以interface21框架为基础，经过重新设计，并不断丰富其内涵，于2004年3月24发布1.0正式版
- Spring Framework创始人，著名作者。 Rod在[悉尼大学](https://baike.baidu.com/item/悉尼大学/1700805)不仅获得了计算机学位，同时还获得了音乐学位。更令人吃惊的是在回到软件开发领域之前，他还获得了音乐学的博士学位。

- Spring理念：解决企业应用开发的复杂性，使现有技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架

- SSH：Struct2+Spring+Hibernate
- SSM：SpringMVC+Spring+Mybatis

- 官网：https://spring.io/projects/spring-framework#overview
- 官方下载地址：https://repo.spring.io/release/org/springframework/spring/
- GitHub：https://github.com/spring-projects/spring-framework

- Maven依赖

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.8.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.8.RELEASE</version>
</dependency>

```

## 1.2、优点

- Spring是一个开源的免费的框架（容器）
- Spring是一个轻量级的、非入侵式的框架
- 控制反转（IOC），面向切面编程（AOP）
- 支持事务的处理，对框架整合的支持

- 总结
  - **Spring就是一个轻量级的控制反转（IOC）和面向切面变成（AOP）的框架**

## 1.3、组成

![image-20200808150158583](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200808150158583.png)

## 1.4、扩展

- Spring的介绍：现代化的java开发，说白了就是基于Spring的开发

![image-20200808150328461](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200808150328461.png)

- Springboot
  - 一个快速开发的脚手架
  - 基于Springboot可以快速的开发单个微服务
  - 约定大于配置
- Spring Cloud
  - Spring Cloud式基于SpringBoot实现的

- 现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要完全掌握Spring以及SpringMVC

- **弊端：发展太久之后，违背了原来的理念，配置十分繁琐，人称“配置地狱”**

# 2、IOC理论推导

- 原来的步骤
  1. UserDao接口
  2. UserDaoImpl实现类
  3. UserService业务接口
  4. UserService业务实现类
- 在我们之前的业务中，用户的需求可能会影响原来的代码，我们需要根据用户的需求去修改源代码，如果程序代码量十分大，修改一次的成本十分昂贵
- 我们使用一个set接口实现

```java
private UserDao userDao;
//利用set进行动态实现值的注入
public void setUserDao(UserDao userDao) {
  this.userDao = userDao;
}
```

- 之前，程序是主动创建对象，控制权在程序猿手上
- 使用了set注入后，程序不再具有主动性，而是变成了被动的接收对象

- 这种思想，从本质上解决了问题，我们程序猿不再去管理对象的创建了，系统的耦合性大大降低，可以更加专注的在业务的实现上，这是IoC的原型

![image-20200808193257515](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200808193257515.png)

## IoC本质

- **控制反转IoC（Inversion of Control），是一种设计思想，DI（依赖注入）是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，控制反转就是：**获得依赖对象的方式反转了**（由程序猿---变为-->由用户）

![image-20200808193750202](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200808193750202.png)

- 采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用了注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的
- **控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方式是依赖注入（Dependency injection，DI）**

# 3、HelloSpring

- 导入maven依赖或者jar包

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>5.2.8.RELEASE</version>
</dependency>
```

- 编写代码

  - 实体类

  ```java
  package xyz.kidjaya.pojo;
  
  public class Hello {
  
    private String string;
  
    public String getString() {
      return string;
    }
  
    public void setString(String string) {
      this.string = string;
    }
  
    @Override
    public String toString() {
      return "Hello{" +
        "string='" + string + '\'' +
        '}';
    }
  }
  ```

  - 编写spring配置文件，命名为beans.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
      <!--使用Spring来创建对象，在Spring这些都成为Bean
          类型 变量名 = new 类型();
          Hello hello = new Hello();
  
          id = 变量名
          class = new的类型()
          property相当于给对象中的属性设置一个值
      -->
      <bean id="hello" class="xyz.kidjaya.pojo.Hello">
          <property name="string" value="SpringFramework"/>
      </bean>
  </beans>
  ```

  - 测试代码

  ```java
  @Test
  public void test(){
    //获取Spring的上下文对象
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    //我们的对象现在都在Spring中管理了，我们要使用，直接去里面取出来就可以
    Hello hello = (Hello) context.getBean("hello");
    System.out.println(hello.toString());
  }
  ```

  - 思考
    - Hello对象是谁创建的？【hello对象是由Spring创建的】
    - Hello对象的属性是怎么设置的？【hello对象的属性是由Spring容器设置的】
    - 控制：有谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用Spring后，对象由Spring来创建的
    - 反转：程序本身不创建对象，而编程被动的接收对象
  - 依赖注入：就是利用set方法来进行注入的
  - IoC是一种编程思想，由主动的编程变成被动的接收
  - 可以通过newClassPathXmlApplicationContext去浏览一下底层源码

- 修改传统编码

  - 新增beans.xml配置文件

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="mysqlImpl" class="xyz.kidjaya.pojo.UserDaoMysqlImpl"/>
      <bean id="oracleImpl" class="xyz.kidjaya.pojo.UserDaoOracleImpl"/>
  
      <bean id="userServiceImpl" class="xyz.kidjaya.service.UserServiceImpl">
          <!--
          ref：引用Spring容器中创建好的对象
          value：具体的值，基本数据类型
          -->
          <property name="userDao" ref="oracleImpl"/>
      </bean>
  </beans>
  ```

  - 测试

  ```java
  @Test
  public void test(){
    //获取ApplicationContext：拿到Spring的容器
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    //容器在手，天下我有，需要什么，就直接get什么
    UserServiceImpl userServiceImpl = (UserServiceImpl) context.getBean("userServiceImpl");
    userServiceImpl.getUser();
  }
  ```

- **我们彻底不用在到程序中去该懂了，要实现不同的操作，只需要在xml配置文件中进行修改，所谓的IoC，就是：对象由Spring来创建，管理，装配**

# 4、IoC创建对象的方式

1. 使用无参构造创建对象，默认

2. 假设我们要使用有参构造创建对象

   1. 下标赋值

   ```xml
   <!--方式一：下标赋值-->
   <bean id="user" class="xyz.kidjaya.pojo.User">
     <constructor-arg index="0" value="kidjaya"/>
   </bean>
   ```

   2. 类型

   ```xml
   <!--方式二：使用类型，不建议使用,因为可能有同种类型参数，产生歧义-->
   <bean id="user" class="xyz.kidjaya.pojo.User">
     <constructor-arg type="java.lang.String" value="kidjaya"/>
   </bean>
   ```

   3. 参数名

   ```xml
   <!--方式三：-->
   <bean id="user" class="xyz.kidjaya.pojo.User">
     <constructor-arg name="name" value="kidjaya"/>
   </bean>
   ```

- 总结：在配置文件加载的时候，容器中管理的对象就已经默认初始化了

# 5、Spring配置

## 5.1、别名

```xml
<!--别名，也就是多个名字可以引用-->
<alias name="user" alias="kid"/>
```

## 5.2、Bean配置

```xml
<!--
        id:bean的唯一标识符，也就是相当于我们学的对象名
        class:bean对象所对应的全限定名：包名+类名
        name:也是别名，而且name更高级，可以同时取多个别名
    -->
<bean id="userT" class="xyz.kidjaya.pojo.UserT" name="user2,u2;u3 u4">
  <property name="name" value="kidjaya"/>
</bean>
```

## 5.3、import

- 这个import，一般用于团队开发使用，他可以将多个配置文件，导入合并为一个

- 假设，现在项目中有多个人开发，这三个人复制不同的类开发，不同的类需要注册不同的bean中，我们可以利用import将所有人的beans.xml合并为一个总的

  - beans.xml
  - beans2.xml
  - beans3.xml
  - applicationContext.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
                             https://www.springframework.org/schema/beans/spring-beans.xsd">
    <import resource="beans.xml"/>
    <import resource="beans2.xml"/>
    <import resource="beans3.xml"/>
  </beans>
  ```

  - 使用的时候直接用总配置就好了
  - 注意
    1. 如果import之后有相同的会覆盖
    2. 如果import之后有歧义，覆盖顺序是按最后一个import的值为准

# 6、依赖注入

## 6.1、构造器注入

- IoC创建对象的方式已详细说明

## 6.2、Set方式注入【重点】

- 依赖注入：Set注入
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入

- 【环境搭建】

  1. 复杂类型

  ```java
  public class Address {
      private String address;
  
      public String getAddress() {
          return address;
      }
  
      public void setAddress(String address) {
          this.address = address;
      }
  }
  ```

  2. 真实测试对象

  ```java
  public class Student {
      private String name;
      private Address address;
      private String[] books;
      private List<String> hobbies;
      private Map<String,String> card;
      private Set<String> games;
      private String wife;
      private Properties info;
  }
  ```

  3. beans.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="student" class="xyz.kidjaya.pojo.Student">
          <!--第一种：普通值注入，value-->
          <property name="name" value="kidjaya"/>
      </bean>
  </beans>
  ```

  4. 测试类

  ```java
  public class MyTest {
    public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
      Student student = context.getBean("student", Student.class);
      System.out.println(student.getWife());
    }
  }
  
  ```

- 完善注入信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="address" class="xyz.kidjaya.pojo.Address">
        <property name="address" value="广东深圳"/>
    </bean>

    <bean id="student" class="xyz.kidjaya.pojo.Student">
        <!--第一种：普通值注入，value-->
        <property name="name" value="kidjaya"/>
        <!--Bean注入，ref-->
        <property name="address" ref="address"/>
        <!--数组注入，ref-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>西游记</value>
                <value>三国演义</value>
                <value>水浒传</value>
            </array>
        </property>
        <!--List-->
        <property name="hobbies">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>看电影</value>
            </list>
        </property>
        <!--Map-->
        <property name="card">
            <map>
                <entry key="身份证" value="111112222333333"/>
                <entry key="银行卡" value="asdasd1231a23ds"/>
            </map>
        </property>
        <!--Set-->
        <property name="games">
            <set>
                <value>LOL</value>
                <value>POP</value>
                <value>BOB</value>
            </set>
        </property>
        <!--Null-->
        <property name="wife">
            <null/>
        </property>
        <!--Properties-->
        <property name="info">
            <props>
                <prop key="学号">201911111111</prop>
                <prop key="姓名">kidjaya</prop>
                <prop key="用户名">root</prop>
            </props>
        </property>
    </bean>
</beans>
```

## 6.3、拓展方式注入

- 我们可以使用p命名空间和c命名空间进行注入
- 官方解释

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200809111435579.png" alt="image-20200809111435579" style="zoom:50%;" />

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200809111504839.png" alt="image-20200809111504839" style="zoom:50%;" />

- 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:p="http://www.springframework.org/schema/p"
               xmlns:c="http://www.springframework.org/schema/c"
               xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--p命名空间注入，可以直接注入属性值：property-->
    <bean id="user" class="xyz.kidjaya.pojo.User" p:age="22" p:name="羊大大"/>
    <!--c命名空间注入，通过构造器注入，construct-args-->
    <bean id="user2" class="xyz.kidjaya.pojo.User" c:age="99" c:name="你好"/>
</beans>
```

- 测试

```java
public class MyTest {
  public static void main(String[] args) {
    ApplicationContext context = new ClassPathXmlApplicationContext("userBeans.xml");
    User user = context.getBean("user", User.class);
    System.out.println(user.toString());
    User user2 = context.getBean("user2", User.class);
    System.out.println(user2);
  }
}
```

- 注意点：p命名空间和c命名空间不能直接使用，需要导入xml约束

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

## 6.4、Bean的作用域

![image-20200809111827321](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200809111827321.png)

1. 单例模式（Spring默认机制）

```xml
<bean id="user" class="xyz.kidjaya.pojo.User" p:age="22" p:name="羊大大" scope="singleton"/>
```

2. 原型模式：每次从容器get的时候，都会产生一个新对象

```xml
<bean id="user" class="xyz.kidjaya.pojo.User" p:age="22" p:name="羊大大" scope="prototype"/>
```

3. 其余的request, session,application，这些只能在web开发中使用到（与JavaWeb作用域一样）

# 7、Bean的自动装配

- 自动装配是Spring慢煮bean依赖的一种方式
- Spring会在上下文中自动寻找，并自动给bean装配属性

- 在Spring中有三种装配的方式
  1. 在xml中显式的配置
  2. 在java中显式的配置
  3. 隐式的自动装配bean【重要】

## 7.1、测试

1. 环境搭建

   - 一个人有两个宠物

   ```java
   public class People {
       private Cat cat;
       private Dog dog;
       private String name;
   }
   ```

   - 猫

   ```java
   public class Cat {
       public void shout(){
           System.out.println("喵～");
       }
   }
   ```

   - 狗

   ```java
   public class Dog {
       public void shout(){
           System.out.println("汪～");
       }
   }
   ```

## 7.2、ByName自动装配

 ```xml
<bean id="cat" class="xyz.kidjaya.pojo.Cat"/>
<bean id="dog" class="xyz.kidjaya.pojo.Dog"/>
<!--
    byName：会自动在容器上下文查找，和自己对象set方法后面值对应的beanId
-->
<bean id="people" class="xyz.kidjaya.pojo.People" autowire="byName">
  <property name="name" value="kidjaya"/>
</bean>
 ```

## 7.3、ByType自动装配

```xml
<bean id="cat" class="xyz.kidjaya.pojo.Cat"/>
<bean class="xyz.kidjaya.pojo.Dog"/>
<!--
    byName：会自动在容器上下文查找，和自己对象set方法后面值对应的beanId
    byType：会自动在容器上下文查找，和自己对象属性类型相同的bean
-->
<bean id="people" class="xyz.kidjaya.pojo.People" autowire="byType">
  <property name="name" value="kidjaya"/>
</bean>
```

- 小结
  - byName的时候，需要所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
  - byType的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性类型一致

## 7.4、使用注解实现自动装配

- jdk1.5支持的注解，Spring2.5支持注解

- The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML.

- 要使用注解须知：

  1. 导入约束：context约束
  2. **配置注解支持：  context:annotation-config**

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

- @Autowired

  - 直接在实体类属性上使用，也可以在set方式上使用
  - 使用Autowired我们可以不用编写set方法了，前提是你这个自动装配的属性在IoC（Spring）容器中存在，并且符合类型 同上面的ByType
  - 机制：先按类型，再按名字

- 科普

  ```java
  @Nullable 字段标记了这个注解，说明这个字段可以为null;
  ```

  ```java
  public @interface Autowired {
      boolean required() default true;
  }
  ```

  - 测试代码

  ```java
  public class People {
      @Autowired(required = false)
      private Cat cat;
      @Autowired
      private Dog dog;
      private String name;
  }
  ```

  - 如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以睡用@Qualifier(value="xxx")去配置@Autowired的使用，指定一个唯一的bean对象注入（就是bean中存在许多类型不唯一的值，需要指定一个唯一的id指定唯一的bean对象）

  ```java
  public class People {
      @Autowired(required = false)
      private Cat cat;
      @Autowired
      @Qualifier(value = "dog2")
      private Dog dog;
      private String name;
  }
  ```

  - @Resource注解

  ```java
  public class People {
      @Resource
      private Cat cat;
      @Resource(name = "dog2")
      private Dog dog;
      private String name;
  }
  ```

- 小结

  - @Resource 和 @Autowried的区别
    - 都是用来自动装配的，都可以放在属性字段上
    - @Autowired先通过ByType，如果唯一则注入，否者通过ByName查找【常用】
    - @Resource先通过ByName查找，无匹配，则通过ByType查找【常用】
    - 如果两个都找不到的情况下就报错

# 8、使用注解开发

> 在Spring4之后，要使用注解开发，必须导入aop依赖

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200809134847207.png" alt="image-20200809134847207" style="zoom:50%;" />

- 使用注解需要导入context约束，增加注解的支持

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启注解的支持-->
    <context:annotation-config/>

</beans>
```



1. bean

2. 属性如何注入

```java
//等价于<bean id="user" class="xyz.kidjaya.pojo.User"/>
//组件
//id默认为类名小写
@Component
public class User {
    //相当于 <bean id="user" class="xyz.kidjaya.pojo.User">
    //        <property name="name" value = "kidjaya"/>
    //      </bean>
    @Value("kidjaya")
    public String name;

    //set放着也是同样的效果
//    @Value("kidjaya")
    public void setName(String name) {
        this.name = name;
    }
}
```

3. 衍生的注解
   - @Component有几个衍生注解，我们在web开发中，会按照mvc三层架构分层
     - dao【@Repository】
     - service【@Service】
     - controller【@Controller】
   - 这四个注解功能都是一样的，都是代表某个类注册到Spring容器中，装配Bean

4. 自动装配配置
   - @Autowired:自动装配 
   - @Resource：自动装配 
   - @Nullable：允许为空
   - @Component:组件，放在类上，说明这个类被Spring管理了，就是bean

5. 作用域

```java
@Component
@Scope("singleton")
public class User {
    //相当于 <bean id="user" class="xyz.kidjaya.pojo.User">
    //        <property name="name" value = "kidjaya"/>
    //      </bean>
    @Value("kidjaya")
    public String name;

    //set放着也是同样的效果
//    @Value("kidjaya")
    public void setName(String name) {
        this.name = name;
    }
}
```

6. 小结

   - XML与注解：

     - xml更加万能，适合于任何场合，维护简单方便
     - 注解：不是自己的类用不了，维护相对复杂

   - 最佳实践

     - xml用来管理bean；
     - 注解只负责完成属性的注入
     - 我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持

     ```xml
     <!--指定要扫描的包，这个包下的注解就会生效-->
     <context:component-scan base-package="xyz.kidjaya"/>
     <!--开启注解的支持-->
     <context:annotation-config/>
     ```

# 9、使用java的方式配置Spring

- 我们现在要完全不使用Spring的xml配置了，全权交给java来做
- JavaConfig是Spring的一个子项目，在Spring4之后，它成为了一个核心功能

![image-20200809143002143](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200809143002143.png)

- 实体类

```java
//这里这个注解的意思，就是说明这个类被Spring接管了，注册到了容器中，可以不加
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }

    @Value("Kidjaya")//属性注入值
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

- 配置文件

```java
package xyz.kidjaya.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import xyz.kidjaya.pojo.User;

@Configuration
@ComponentScan("xyz.kidjaya.pojo")
//这个也会被Spring容器托管，注册到容器中，应为它本省就是一个组件@Component
//@Configuration代表这是一个配置类，就和我们之前看到的beans.xml
@Import(KidConfig2.class)//相当于引入合并，对比xml的import
public class KidConfig {

    //注册一个bean，就相当于之前写的一个bean标签
    //这个方法的名字，就相当于bean标签中的id属性
    //这个方法的返回值，就相当于bean标签中的class属性
    @Bean
    public User getUser(){
        return new User();//就是返回要注入到bean的对象
    }
    @Bean
    public User getUser2(){
        return new User();//就是返回要注入到bean的对象
    }
}
```

- 测试类

```java
package xyz.kidjaya.dao;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import xyz.kidjaya.config.KidConfig;
import xyz.kidjaya.pojo.User;

public class MyTest {
    @Test
    public void test(){
        //如果完全使用了配置类方式去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类的class对象加载
        ApplicationContext context = new AnnotationConfigApplicationContext(KidConfig.class);
        User user = context.getBean("getUser", User.class);//user,getUser都可以
        System.out.println(user.getName());
    }
}
```

- 这种纯java的配置方式，在SpringBoot中随处可见

# 10、代理模式

> 为什么要学习代理模式？因为这就是SpringAOP的底层【SpringAOP和SpringMVC】

- **相关笔记 http://www.kidjaya.xyz/index.php/archives/95/**

# 11、AOP

## 11.1、什么是AOP

- **AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发效率**

![image-20200810101254763](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200810101254763.png)

## 11.2、AOP在Spring中的作用

- **提供声明式的事务，允许用户自定义切面**

- 横切关注点：跨越应用程序多个模块的方法或功能。即与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志、安全、缓存、事务等
- 切面（Aspect）：横切关注点被模块化的特殊对象。即，他是一个类
- 通知（Advice）：切面必须要完成的工作。即，他是类中的一个方法
- 目标（Target）：被通知的对象
- 代理（Proxy）：向目标对象应用通知后创建的对象
- 切入点（PointCut）：切面通知执行的“地点”的定义
- 连接点（JointPoint）：与切入点匹配的执行点

![image-20200810102013321](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200810102013321.png)

- SpringAOP中，通过Advice定义横切逻辑，Spring中支持5中类型的Advice

![image-20200810102112796](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200810102112796.png)

- AOP在不改变原有代码的情况下，去增加新的功能

## 11.3、使用Spring实现AOP

- 使用AOP织入，需要导入一个依赖包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

- 新建目标对象

```java
public interface UserService {
    public void add();
    public void delete();
    public void update();
    public void select();
}
```

```java
public class UserServiceImpl implements UserService{
    public void add() {
        System.out.println("add");
    }

    public void delete() {
        System.out.println("delete");
    }

    public void update() {
        System.out.println("update");
    }

    public void select() {
        System.out.println("select");
    }
}
```



- **方式一：Spring的API接口【主要SpringAPI接口实现】**

  - 新建类实现JDK接口

  ```java
  public class AfterLog implements AfterReturningAdvice {
    //returnValue：返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
      System.out.println("执行了"+method.getName()+"方法，返回结果为"+returnValue);
    }
  }
  ```

  ```java
  public class Log implements MethodBeforeAdvice {
      //method：要执行的目标对象的方法
      //objects：参数
      //target：目标对象
      public void before(Method method, Object[] objects, Object target) throws Throwable {
          System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
      }
  }
  ```

  - 编写spring配置文件

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
             https://www.springframework.org/schema/beans/spring-beans.xsd
             http://www.springframework.org/schema/aop
             https://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean id="userService" class="xyz.kidjaya.service.UserServiceImpl"/>
    <bean id="log" class="xyz.kidjaya.log.Log"/>
    <bean id="afterLog" class="xyz.kidjaya.log.AfterLog"/>
  <!--方式一：使用原生的SpringAPI接口-->
    <!--配置aop：需要导入aop的xml约束-->
    <aop:config>
      <!--切入点:expression:表达式，execution（要执行的位置 * * * *）-->
      <aop:pointcut id="pointcut" expression="execution(* xyz.kidjaya.service.UserServiceImpl.*(..))"/>
      <!--执行环绕增加-->
      <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
      <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>
  </beans>
  ```

- **方式二：自定义类【主要是切面定义】**

  - 新建自定义类

  ```java
  public class DiyPointCut {
      public void before(){
          System.out.println("方法执行前");
      }
  
      public void after(){
          System.out.println("方法执行后");
      }
  }
  ```

  - 配置xml

  ```xml
  <!--方式二：自定义类-->
  <bean id="diy" class="xyz.kidjaya.div.DiyPointCut"/>
  <aop:config>
    <!--自定义切面，ref要引用的类-->
    <aop:aspect ref="diy">
      <!--切入点-->
      <aop:pointcut id="point" expression="execution(* xyz.kidjaya.service.UserServiceImpl.*(..))"/>
      <!--通知-->
      <aop:before method="before" pointcut-ref="point"/>
      <aop:after method="after" pointcut-ref="point"/>
    </aop:aspect>
  </aop:config>
  ```

  

- **方式三：使用注解实现**

  - 新建注解测试类

  ```java
  @Aspect//标注这个类是一个切面
  public class AnnotationPointCut {
  
      @Before("execution(* xyz.kidjaya.service.UserServiceImpl.*(..))")
      public void before(){
          System.out.println("方法执行前2222233333");
      }
      @After("execution(* xyz.kidjaya.service.UserServiceImpl.*(..))")
      public void after(){
          System.out.println("方法执行后1113333333");
      }
      //在环绕增强中，我们可以给定一个参数，代表我们要获取处理切入的点
      @Around("execution(* xyz.kidjaya.service.UserServiceImpl.*(..))")
      public void around(ProceedingJoinPoint joinPoint) throws Throwable {
          System.out.println("环绕前");
          //执行方法
          Object proceed = joinPoint.proceed();
          System.out.println(joinPoint.getSignature());
          System.out.println(proceed);
  
          System.out.println("环绕后");
      }
  }
  ```

  - 编写配置文件

  ```xml
  <!--方式三：使用注解-->
  <bean id="annotationPointCut" class="xyz.kidjaya.div.AnnotationPointCut"/>
  <!--开启注解支持 JDK(默认 proxy-target-class="false") cglib (proxy-target-class="true") ==>对动态代理的实现方式不同的两个东西-->
  <aop:aspectj-autoproxy proxy-target-class="true"/>
  ```

- 测试类

```java
public class MyTest {
  @Test
  public void test(){
    ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
    //动态代理，代理的是接口
    UserService userService = context.getBean("userService", UserService.class);
    userService.select();
  }
}
/*执行结果：
环绕前
方法执行前2222233333
select
方法执行后1113333333
void xyz.kidjaya.service.UserServiceImpl.select()
null
环绕后
*/
```

# 12、整合MyBatis

- **步骤**

  1. 导入相关jar包

     - junit
     - mybatis
     - mysql数据库
     - spring相关的
     - aop织入
     - mybatis-spring【new】

     ```xml
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.13</version>
       </dependency>
       <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
         <version>1.18.4</version>
       </dependency>
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>5.1.47</version>
       </dependency>
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.5.2</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.2.8.RELEASE</version>
       </dependency>
       <!--Spring操作数据库的话还需要一个Spring-jdbc-->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-jdbc</artifactId>
         <version>5.1.9.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>org.aspectj</groupId>
         <artifactId>aspectjweaver</artifactId>
         <version>1.8.13</version>
       </dependency>
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis-spring</artifactId>
         <version>2.0.2</version>
       </dependency>
     </dependencies>
     ```

  2. 编写配置文件

  3. 测试



## 12.1、回忆MyBatis

- 步骤

  1. **编写实体类**

  ```java
  public class User {
    private int id;
    private String name;
    private String pwd;
  
  }
  ```

  

  2. **编写核心配置文件**

  ```xml
  <configuration>
    <typeAliases>
      <package name="xyz.kidjaya.pojo"/>
    </typeAliases>
    <environments default="development">
      <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
          <property name="driver" value="com.mysql.jdbc.Driver"/>
          <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
          <property name="username" value="xxxx"/>
          <property name="password" value="xxxx"/>
        </dataSource>
      </environment>
    </environments>
    <mappers>
      <mapper class="xyz.kidjaya.mapper.UserMapper"/>
    </mappers>
  </configuration>
  ```

  

  3. 编写接口

  ```java
  public interface UserMapper {
      public List<User> selectUser();
  }
  ```

  

  4. **编写Mapper.xml**

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="xyz.kidjaya.mapper.UserMapper">
      <select id="selectUser" resultType="user">
          select * from user
      </select>
  </mapper>
  ```

  

  5. 测试

## 12.2、MyBatis-spring

> 文档：http://mybatis.org/spring/zh/index.html

- 基础知识

  - 在开始使用 MyBatis-Spring 之前，你需要先熟悉 Spring 和 MyBatis 这两个框架和有关它们的术语。

  ![image-20200810195611249](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200810195611249.png)

- Maven依赖

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>2.0.2</version>
</dependency>
```

- 步骤

  1. **编写数据源配置**
     - 替换了MyBatis里的配置

  ```xml
  <!--DataSource：使用Spring的数据源替换MyBatis的配置 c3p0 dbcp druid-->
  <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8"/>
    <property name="username" value="root"/>
    <property name="password" value="aaa791654109"/>
  </bean>
  ```

  2. **sqlSessionFactory**

  ```xml
  <!--sqlSessionFactory-->
  <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!--绑定MyBatis配置文件-->
    <property name="configLocation" value="classpath:mybatis-config.xml"/>
    <property name="mapperLocations" value="classpath:xyz/kidjaya/mapper/*.xml"/>
  </bean>
  ```

  3. **sqlSessionTemplate**

  ```xml
  <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <!--只能使用构造器注入sqlSessionFactory，因为它没有se方法-->
    <constructor-arg index="0" ref="sqlSessionFactory"/>
  </bean>
  ```

  4. **需要给接口加实现类,私有化sqlSessionTemplate【new】**

  ```java
  package xyz.kidjaya.mapper;
  
  import org.mybatis.spring.SqlSessionTemplate;
  import xyz.kidjaya.pojo.User;
  
  import java.util.List;
  
  public class UserMapperImpl implements UserMapper{
      //我们的所有操作，都使用sqlSession来执行，在原来，现在都使用SqlSessionTemplate
      private SqlSessionTemplate sqlSession;
  
      public void setSqlSession(SqlSessionTemplate sqlSession) {
          this.sqlSession = sqlSession;
      }
  
      public List<User> selectUser() {
          return sqlSession.getMapper(UserMapper.class).selectUser();
      }
  }
  ```

  5. **将自己写的实现类注入到spring容器**

  ```xml
  <bean id="userMapper" class="xyz.kidjaya.mapper.UserMapperImpl">
    <property name="sqlSession" ref="sqlSession"/>
  </bean>
  ```

  6. **注册使用**

  ```java
  public class MyTest {
    @Test
    public void test() throws IOException {
      ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
      UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
      List<User> users = userMapper.selectUser();
      for (User user : users) {
        System.out.println(user);
      }
    }
  }
  ```

  - **现在的MyBatis配置**

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
    <typeAliases>
      <package name="xyz.kidjaya.pojo"/>
    </typeAliases>
  </configuration>
  ```

- **扩展**

  - dao继承Support类，直接利用getSqlSession()获得，然后直接注入SqlSessionFactory，比起之前的，不需要管理SqlSessionTemplate

- **步骤**

  1. 修改实现类

  ```java
  public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper{
      public List<User> selectUser() {
          return getSqlSession().getMapper(UserMapper.class).selectUser();
      }
  }
  ```

  2. 修改bean

  ```xml
  <!--继承SqlSessionDaoSupport就不用注入直接调用即可-->
  <bean id="userMapper2" class="xyz.kidjaya.mapper.UserMapperImpl2">
    <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
  </bean>
  ```

  3. 测试

  ```java
  public class MyTest {
    @Test
    public void test() throws IOException {
      ApplicationContext context = new ClassPathXmlApplicationContext("spring-dao.xml");
      UserMapper userMapper = context.getBean("userMapper2", UserMapper.class);
      List<User> users = userMapper.selectUser();
      for (User user : users) {
        System.out.println(user);
      }
    }
  }
  ```

- **总结**：**整合到Spring中以后可以完全不要MyBatis的配置文件，除了这些方法可以实现整合之外，我们还可以使用注解来实现。**

# 13、声明式事务

## 1、回顾事务

- 把一组业务当成一个业务来做；要么都成功、要么都失败
- 事务在项目开发中，十分重要，涉及到数据的一致性问题，不能马虎
- 确保完整性和一致性

- **事务ACID原则**
  - 原子性。
  - 一致性
  - 隔离性
    - 多个业务可能操作同一个资源，防止数据损坏
  - 持久性
    - 事务一旦提交，无论系统发生声明问题，结果都不会被影响，被持久化的写到存储器中

- 测试

```java
public interface UserMapper {
  public List<User> selectUser();
  public int addUser(User user);
  public int deleteUser(int id);
}
```

```xml
<mapper namespace="xyz.kidjaya.mapper.UserMapper">
  <select id="selectUser" resultType="user">
    select * from user
  </select>

  <insert id="addUser" parameterType="user">
    insert into user (id,name,pwd) values (#{id},#{name},#{pwd})
  </insert>

  <delete id="deleteUser" parameterType="int">
    deletes from user where id = #{id}
  </delete>
</mapper>
```

```java
public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {

  public List<User> selectUser() {
    User user = new User(6, "test", "aaa123456");
    UserMapper mapper = getSqlSession().getMapper(UserMapper.class);
    mapper.addUser(user);
    mapper.deleteUser(5);

    return mapper.selectUser();
  }

  public int addUser(User user) {
    return getSqlSession().getMapper(UserMapper.class).addUser(user);
  }

  public int deleteUser(int id) {
    return getSqlSession().getMapper(UserMapper.class).deleteUser(id);
  }
}
```

- sql异常
- 插入成功
- 所以我们需要事务
- 手动配置事务十分麻烦，但是Spring给我们提供了事务管理，我们只需要配置即可

## 2、spring中的事务管理

- **编程式事务：**
  - 将事务管理代码嵌套到业务方法中来控制事务的提交和回滚
  - 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码
- **声明式事务：AOP**
  - 一般情况下比编程式好用
  - 将事务管理代码从业务方法中分离出来，以声明的方式来实现业务管理
  - 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring AOP框架支持声明事务管理



- **步骤**

  1. 加入头文件xml约束，tx

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          https://www.springframework.org/schema/aop/spring-aop.xsd
          http://www.springframework.org/schema/tx
          https://www.springframework.org/schema/tx/spring-tx.xsd">
  </beans>
  ```

  2. 事务管理器

  - 无论使用Spring的哪种事务管理策略（编程式、声明式）事务管理器都是必须的
  - 就是Spring的核心事务管理抽象，管理封装了一组独立于技术的方法
  - JDBC事务

  ```xml
  <!--配置声明式事务-->
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
  </bean>
  ```

  3. 配置事务通知和aop织入

  ```xml
  <!--结合AOP实现事务的织入-->
  <!--配置事务的类-->
  <tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!--给哪些方法配置事务-->
    <!--配置事务的传播特性【new】-->
    <tx:attributes>
      <tx:method name="add*" propagation="REQUIRED"/>
      <tx:method name="delete*" propagation="REQUIRED"/>
      <tx:method name="update*" propagation="REQUIRED"/>
      <tx:method name="query*" propagation="REQUIRED"/>
      <tx:method name="*" propagation="REQUIRED"/>
    </tx:attributes>
  </tx:advice>
  
  <aop:config>
    <aop:pointcut id="txPointCut" expression="execution(* xyz.kidjaya.mapper.*.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
  </aop:config>
  ```

- **spring事务传播的特性**

  - 事务传播行为就是多个事务方法互相调用时，事务如何在这些方法间传播。spring支持7种事务传播行为：

  ![image-20200810203035087](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200810203035087.png)

  - Spring默认的事务传播行为是REQUIRED，适合大部分情况

- **思考：为什么需要事务**
  - 如果不配置事务，可能存在数据提交不一致的情况
  - 如果我们不在spring中去配置声明式事务，我们就需要在代码中手动配置事务
  - 事务在项目开发中十分重要，涉及到数据的一致性和完整性问题，不容马虎











