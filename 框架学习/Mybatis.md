# Mybatis

> 视频参考：https://www.bilibili.com/video/BV1NE411Q7Nx

## 1.简介

### 1.1、什么是Mybatis

![image-20200713162023554](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200713162023554.png)

- MyBatis 是一款优秀的**持久层框架**
- 它支持自定义 SQL、存储过程以及高级映射
- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作
- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录
- MyBatis 本是[apache](https://baike.baidu.com/item/apache/6265)的一个开源项目[iBatis](https://baike.baidu.com/item/iBatis), 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。2013年11月迁移到Github。

#### 1.1.1、如何获取Mybatis？

- Maven仓库

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.2</version>
</dependency>
```

- GitHub地址: https://github.com/mybatis/mybatis-3
- 中文文档: https://mybatis.org/mybatis-3/zh/index.html

### 1.2、持久化

#### 1.2.1、数据持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程
- 内存：断电即失
- 数据库（JDBC），io文件持久化
- 生活例子：工作记忆转换为长时记忆

#### 1.2.2、为什么需要持久化

- 有一些对象，不能让他丢失
- 内存的价格太贵

### 1.3、持久层

Dao层、Service层

- 完成持久化工作的代码块
- 层界限十分明显

### 1.4、为什么需要Mybatis

- 方便
- 帮助程序员将数据存入到数据库中
- 传统的JDBC代码太复杂。简化、框架、自动化
- 不用Mybatis也可以，更容易上手，**技术没有高低之分**
- 优点
  - **简单易学：**本身就很小且简单。没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现
  - **灵活**：mybatis不会对应用程序或者数据库的现有设计强加任何影响。 sql写在xml里，便于统一管理和优化。通过sql语句可以满足操作数据库的所有需求
  - **解除sql与程序代码的耦合：**通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，**提高了可维护性**
  - **提供映射标签，支持对象与数据库的orm字段关系映射**
  - **提供对象关系映射标签，支持对象关系组建维护**
  - **提供xml标签，支持编写动态sql**
- **最重要的一点：使用的人多！**

---

## 2、第一个Mybatis程序

> *思路流程：环境搭建-> 导入Mybatis->编写代码->测试

### 2.1、环境演示

- 搭建数据库

```mysql
CREATE TABLE `user` (
  `id` int(11) NOT NULL,
  `name` varchar(30) DEFAULT NULL,
  `pwd` varchar(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `user`(`id`,`name`,`pwd`) VALUES
(1,'kidjaya','123456'),
(2,'ccHealther','123456'),
(3,'yangdada','123456')

SELECT * FROM `user`
```

- 导入maven依赖

```xml
<!-- 父工程 -->
<dependencies>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.4</version>
  </dependency>
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
  </dependency>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
  </dependency>
</dependencies>
```

- 编写mybatis工具类

```java
public class MybatisUtils {
  private static SqlSessionFactory sqlSessionFactory;

  static {
    try {
      //获取sqlSessionFactory对象
      String resource = "org/mybatis/example/mybatis-config.xml";
      InputStream inputStream = Resources.getResourceAsStream(resource);
      sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    } catch (IOException e) {
      e.printStackTrace();
    }
  }

  //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
  //SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。你可以通过 SqlSession 实例来直接执行  已映射的 SQL 语句。
  public static SqlSession getSqlSession(){
    return sqlSessionFactory.openSession();
  }
}
```

- 配置mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
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
        <mapper resource="xyz/kidjaya/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

- 创建实体类

```java
package xyz.kidjaya.pojo;

public class User {
    private int id;
    private String name;
    private String pwd;

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```

- Dao接口

```java
package xyz.kidjaya.dao;

import xyz.kidjaya.pojo.User;

import java.util.List;

public interface UserDao {
    List<User> getUserList();
}
```

- 接口实现类由原来的UserDaoImpl转变为一个Mapper配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--绑定一个对应的Dao/Mapper接口-->
<mapper namespace="xyz.kidjaya.dao.UserDao">
    <select id="getUserList" resultType="xyz.kidjaya.pojo.User">
        select * form mybatis.user
    </select>
</mapper>
```

### 2.2、测试

> 注意点：org.apache.ibatis.binding.**BindingException**: Type interface xyz.kidjaya.dao.UserDao is not known to the MapperRegistry.

- junit测试

```java
package xyz.kidjaya.dao;

import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import xyz.kidjaya.pojo.User;
import xyz.kidjaya.utils.MybatisUtils;

import java.util.List;

public class UserDaoTest {
    @Test
    public void test(){
        //第一步，获得SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        //执行sql
        //方式一
        UserDao mapper = sqlSession.getMapper(UserDao.class);
        List<User> userList = mapper.getUserList();

        //方式二
        List<User> userList2 = sqlSession.selectList("xyz.kidjaya.dao.UserDao.getUserList");
        for (User user : userList ) {
            System.out.println(user);
        }

        sqlSession.close();
    }
}
```

- 可能会遇到的问题

  1. 配置文件没有注册(mybatis-config.xml)

  ```xml
  <mappers>
    <mapper resource="xyz/kidjaya/dao/UserMapper.xml"/>
  </mappers>
  ```

  2. 绑定接口错误
  3. 方法名不对
  4. 返回类型不对

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!--绑定一个对应的Dao/Mapper接口-->
  <mapper namespace="全类名Dao/Mapper接口">
      <select id="方法名" resultType="全类名返回值类型">
          select * form mybatis.user
      </select>
  </mapper>
  ```

5. Maven导出错误问题

```xml
<build>
  <resources>
    <resource>
      <directory>src/main/resources</directory>
      <includes>
        <include>**/*.properties</include>
        <include>**/*.xml</include>
      </includes>
      <filtering>true</filtering>
    </resource>
    <resource>
      <directory>src/main/java</directory>
      <includes>
        <include>**/*.properties</include>
        <include>**/*.xml</include>
      </includes>
      <filtering>true</filtering>
    </resource>
  </resources>
</build>
```

## 3、CRUD

### 3.1、namespace

namespace中的包名要和Dao/mapper接口的包名一致

### 3.2、select

选择、查询语句

- id：就是对应的namespace中的方法名
- resultType：Sql语句执行的返回值
- parameterType：参数类型

1. 编写接口

```java
//根据id查询用户
User getUserById(int id);
```

2. 编写对应的Mapper中的sql语句

```xml
<select id="getUserById" parameterType="int" resultType="xyz.kidjaya.pojo.User">
  select * from  mybatis.user where id = #{id}
</select>
```

3. 测试

```java
@Test
public void getUserById(){
  SqlSession sqlSession = MybatisUtils.getSqlSession();
  UserMapper mapper = sqlSession.getMapper(UserMapper.class);
  User user = mapper.getUserById(1);
  System.out.println(user);
  sqlSession.close();
}
```

### 3.3、Insert

```xml
<insert id="addUser" parameterType="xyz.kidjaya.pojo.User">
  insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd});
</insert>
```

### 3.4、update

```xml
<update id="updateUser" parameterType="xyz.kidjaya.pojo.User">
  update mybatis.user set name = #{name},pwd = #{pwd} where id = #{id};
</update>
```

### 3.5、delete

```xml
<delete id="deleteUser" parameterType="int">
  delete from mybatis.user where id = #{id};
</delete>
```

- 注意

  - 增删改需要提交事务

  ```java
  sqlSession.commit();
  ```

### 3.6、万能Map

> 假设，我们的实体类，或者数据库中的表，字段或者参数过多时，我们应当考虑使用Map

- 不是正规路子，属于野路子

```java
//添加用户(万能map)
int addUser2(Map<String,Object> map);
```

```xml
<!--对象中的属性，可以直接取出来，传递map的key-->
<insert id="addUser2" parameterType="map">
  insert into mybatis.user (id,name,pwd) values (#{userId},#{userName},#{password});
</insert>
```

```java
  @Test
    public void addUser2(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

        Map<String,Object> map = new HashMap<String, Object>();
        map.put("userId",5);
        map.put("userName","kiddddd");
        map.put("password","asjdalkjdlakdla");

        userMapper.addUser2(map);
        sqlSession.commit();
        sqlSession.close();
    }
```

- Map传递参数，直接在sql中取出即可 ->  parameterType="map"
- 对象传递参数，直接在sql中取对象的属性即可 -> parameterType="Object"
- 只有一个基本类型参数的情况下，可以直接在sql中取到
- 多个参数使用map，或者注解

### 3.7、模糊查询

1. Java代码执行的时候，传递通配符%

```java
@Test
public void getUserLike(){
  SqlSession sqlSession = MybatisUtils.getSqlSession();
  UserMapper mapper = sqlSession.getMapper(UserMapper.class);
  List<User> userList = mapper.getUserLike("%k%");

  for (User user : userList ) {
    System.out.println(user);
  }

  sqlSession.close();
}
```

2. 在sql拼接中使用通配符

```java
<select id="getUserLike" resultType="xyz.kidjaya.pojo.User">
  select * from mybatis.user where name like "%"#{value}"%"
</select>
```

- 注意sql注入的问题

## 4、配置解析

### 4.1、核心配置文件

- mybatis-config.xml
- MyBatis的配置文件包含了深深影响Mybatis行为的设置和属性信息

```xml
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
```

### 4.2、环境配置

- MyBatis可以配置成适应多种·环境
- **不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**
- 学会使用配置多套运行环境
- MyBatis默认的事务管理器就是JDBC，连接池：POOLED

### 4.3、属性

- 我们可以通过properties属性来实现引用配置文件
- 这些属性都是可以外部配置且动态替换·1的，既可以在典型的java属性文件中配置，也可以通过properties元素的字元素来传递。-> db.properties

- 编写一个db.properties文件

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username=root
password=aaa791654109
```

- 在核心配置文件中引入

```xml
<properties resource="db.properties">
  <property name="username" value="root"/>
  <property name="pwd" value="aaa791654109"/>
</properties>
```

- 可以直接引入外部文件
- 可以在其中增加一些属性配置
- 如果两个配置位置，有同一个字段，优先使用外部配置文件的！

### 4.4、类型别名（typeAliases）

- 类型别名是为java类型设置一个短的名字
- 存在的意义仅在用来减少类完全限定名的冗余

```xml
<!--可以给实体类起别名-->
<typeAliases>
  <typeAlias type="xyz.kidjaya.pojo.User" alias="User"/>
</typeAliases>
```

- 也可以指定一个包名，Mybatis会在包名下搜索需要的JavaBean。
- 扫描实体类的包，它的默认别名就为这个类的类名，首字母小写（标识为扫描出来的结果）

```xml
<!--可以给实体类起别名-->
<typeAliases>
  <package name="xyz.kidjaya.pojo"/>
</typeAliases>
```

- 在实体类比较少的时候，使用第一种方式
- 如果实体类十分多，建议使用第二种方式
- 第一种可以DIY别名，第二种不行，如果非要改，在实体类上增加注解

```java
@Alias("Kid")
public class User {
  ....
}
```

### 4.5、设置

- 这里是MyBatis中极为重要的调整设置，他们会改变MyBatis的运行时行为

- 一个配置完整的 settings 元素的示例如下：

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```

### 4.6、其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - Mybatis-generator-core
  - mybatis-plus
  - 通用mapper

### 4.7、mappers

- MapperRegistry:注册绑定我们的Mapper文件

**方式一(推荐使用)**

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

**方式二**

```xml
<!--每一个Mapper.xml都需要在mybatis和核心配置文件中注册-->
<mappers>
  <mapper class="xyz.kidjaya.dao.UserMapper"/>
</mappers>
```

注意点：

- 接口和她的Mapper配置必须同名
- 接口和它的Mapper文件必须在同一个包下

**方式三**

- 使用扫描包进行注入绑定

```xml
<mappers>
  <package name="xyz.kidjaya.dao"/>
</mappers>
```

注意点：

- 接口和她的Mapper配置必须同名
- 接口和它的Mapper文件必须在同一个包下

### 4.8、作用域和生命周期

![image-20200804165738652](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200804165738652.png)

- 生命周期和作用域，是至关重要的，因为错误的使用会导致非常严重的**并发问题**

- SqlSessionFactoryBuilder

  - 一旦创建了SqlSessionFactoryBuilder，就不再需要它了
  - 局部变量

- SqlSessionFactory

  - 可以理解为：数据库连接池
  - SqlSessionFactory一旦被创建就应该在应用运行期间一直存在，没有任何理由丢弃它或者重新创建另一个实例
  - 因此SqlSessionFactory的最佳作用域是应用作用域
  - 最简单的就是使用单例模式或者静态单例模式

- SqlSession

  - 连接到连接池的一个请求
  - 用完之后需要赶紧关闭，否则资源会被占用

  ![image-20200804165826151](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200804165826151.png)

  - 这里的每一个Mapper就代表一个具体的业务

## 5、解决属性名和字段名不一致的问题

### 5.1、问题

- 数据库

![image-20200804170816200](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200804170816200.png)

- 实体类

![image-20200804170847947](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200804170847947.png)

- 测试出现问题

![image-20200804171843204](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200804171843204.png)

```xml
// select * from mybatis.user where id = #{id}
//类型处理器
// select id,name,pwd from mybatis.user where id = #{id}
```



- 解决方法

  - 起别名

  ```xml
  <select id="getUserList" resultType="kid" >
    select id,name,pwd as password from mybatis.user
  </select>
  ```

### 5.2、resultMap

结果集映射

```xml
id name pwd
id name password
```

```xml
<resultMap id="UserMap" type="kid">
  <!--column数据库中的字段，property实体类中的属性-->
  <result column="pwd" property="password"/>
</resultMap>

<select id="getUserList" resultMap="UserMap" >
  select * from mybatis.user
</select>
```

- resultMap元素是MyBatis最重要最强大的元素
- ResultMap的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只要描述他们的关系就好了
- ResultMap最优秀的·地方在于，虽然你对他相当了解，但是根本就不需要显式的用到他们
- **如果世界总是这么简单就好了～**

## 6、日志

#### 6.1、日志工厂

> 如果一个数据库操作，出现了异常，我们需要排错，日志就是最好的助手
>
> 曾经：sout、debug
>
> 现在：日志工厂

![image-20200804174908942](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200804174908942.png)

- SLF4J
- Log4j 2
- LOG4J【*】
- JDK logging
- COMMONS_LOGGING 
- STDOUT_LOGGING 【*】
- NO_LOGGING

在Mybatis中具体使用哪一个日志实现，在设置中设定

**STDOUT_LOGGING**标准日志输出

在mybatis核心配置文件中，配置我们的日志

```xml
Opening JDBC Connection
Created connection 1159114532.
Setting autocommit to false on JDBC Connection [com.mysql.jdbc.JDBC4Connection@4516af24]
==>  Preparing: select * from mybatis.user 
==> Parameters: 
<==    Columns: id, name, pwd
<==        Row: 1, kidjaya, 123456
<==        Row: 2, ccHealther, 123456
<==        Row: 3, yangdada, 123456
<==        Row: 4, 杨, abcedggggg
<==        Row: 5, kiddddd, asjdalkjdlakdla
<==      Total: 5
User{id=1, name='kidjaya', pwd='123456'}
User{id=2, name='ccHealther', pwd='123456'}
User{id=3, name='yangdada', pwd='123456'}
User{id=4, name='杨', pwd='abcedggggg'}
User{id=5, name='kiddddd', pwd='asjdalkjdlakdla'}
Resetting autocommit to true on JDBC Connection [com.mysql.jdbc.JDBC4Connection@4516af24]
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@4516af24]
Returned connection 1159114532 to pool.
```

#### 6.2、LOG4J

- 什么是LOG4J？

  - Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件
  - 我们也可以控制每一条日志的输出格式
  - 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程
  - 通过一个[配置文件](https://baike.baidu.com/item/配置文件/286550)来灵活地进行配置，而不需要修改应用的代码

  

1. 先导入LOG4J的包

   ```xml
   <!-- https://mvnrepository.com/artifact/log4j/log4j -->
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

2. Log4j.properties

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/kidjaya.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

3. 配置log4j为日志实现

```xml
<settings>
  <setting name="logImpl" value="LOG4J"/>
</settings>
```

4. Log4j的使用，直接测试运行刚才的查询

![image-20200804181709486](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200804181709486.png)

- 简单使用

  1. 要在使用Log4j的类中，导入包 import org.apache.log4j.Logger;
  2. 日志对象，参数为当前类的class

  ```java
  static Logger logger =  Logger.getLogger(Test.class);
  ```

  3. 日志级别

  ```java
  logger.info("info进入");
  logger.debug("debug ing");
  logger.error("error ing");
  ```

## 7、分页

> 为什么要分页
>
> - 减少数据的处理量

### 7.1、limit实现分页

- 使用Limit实现分页

```MySQL
#语法
select * from table limit startIndex,pageSize;
select * from table limit 5,10;

#为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为-1
select * from table limit 10,-1;

#如果只给定一个参数，它表示返回最大的记录行数目
#换句话说，limit 等价于limit 0,n
select * from table limit 5;
```

- 步骤

  1. 修改Mapper文件

  ```xml
  <select id="getUserList" parameterType="map" resultMap="UserMap" >
    select * from mybatis.user limit #{startIndex},#{pageSize}
  </select>
  ```

  2. Mapper接口，参数为map

  ```java
  //分页获取用户
  List<User> getUserList(Map<String,Integer> map);
  ```

  3. 在测试类中传入参数测试

  - 推断：起始位置 = （当前页面-1）*页面大小

  ```java
  @org.junit.Test
    public void getUserList(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
  
    int currentPage = 2;
    int pageSize = 2;
    Map<String,Integer> map = new HashMap<String, Integer>();
    map.put("startIndex",(currentPage-1)*pageSize);
    map.put("pageSize",pageSize);
  
    List<User> userList = mapper.getUserList(map);
  
    logger.info("entry!!!");
  
    for (User user : userList) {
      System.out.println(user);
    }
  
    sqlSession.close();
  }
  ```

### 7.2、RowBound实现分页

- 不再使用SQL实现分页

1. 接口

```java
List<User> getUserByRowBound();
```

2. Mapper.xml

```xml
<!--分页获取-->
<select id="getUserByRowBound" resultMap="UserMap" >
  select * from mybatis.user
</select>
```

3. 测试

```java
@org.junit.Test
public void getUserByRowBound(){
  SqlSession sqlSession = MybatisUtils.getSqlSession();

  //rowBounds实现
  RowBounds rowBounds = new RowBounds(3, 2);

  //通过java代码层面实现分页
  List<User> userList = sqlSession.selectList("xyz.kidjaya.dao.UserMapper.getUserByRowBound",null,rowBounds);
  for (User user : userList) {
    System.out.println(user);
  }

  sqlSession.close();
}
```

### 7.3、PageHelper

![image-20200804202740208](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200804202740208.png)

- 了解即可
- 官方文档：https://pagehelper.github.io/



## 8、注解开发

### 8.1、面向接口编程

- 真正开发，很多时候使用面向接口编程
- **根本原因：解耦，可拓展，提高复用性，分层开发中，上层不用管具体的实现，大家都遵守共同的标准，使得开发变得容易，规范性更好**
- 在一个面向对象的系统中，系统的各种功能是由许许多多不同的对象协做完成的。在这种情况下，各个对象内部具体是如何实现自己的，对系统设计人员来讲就不那么重要了
- 而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各个模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。

**关于接口的理解**

- 接口从更深层次的理解，应该是定义（规范，约束）与实现（名实分离的原则）的分离
- 接口的本身反映了系统设计人员对系统的抽象理解
- 接口应有两类
  - 第一类是对一个个体的抽象，他可对应为一个抽象体（abstract class）
  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）
- 一个个体可能有多个抽象面。抽象体与抽象面是有区别的

**三个面向的区别**

- 面向对象是指，我们考虑问题的时候，以对象为单位，考虑它的属性和方法
- 面向过程是指，我们考虑问题的时候，以一个具体的流程（事务过程）为单位，考虑它的实现
- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题，更多的体现就是对系统整体的架构

### 8.2、使用注解开发

1. 注解在接口上实现

```java
//获取全部用户
@Select("select * from user")
List<User> getUsers();
```

2. 需要在和核心配置文件中绑定接口

```xml
<!--绑定接口-->
<mappers>
  <mapper class="xyz.kidjaya.dao.UserMapper"/>
</mappers>
```

3. 测试

```java
@org.junit.Test
public void test(){
  SqlSession sqlSession = MybatisUtils.getSqlSession();
  UserMapper mapper = sqlSession.getMapper(UserMapper.class);
  List<User> users = mapper.getUsers();
  for (User user : users) {
    System.out.println(user);
  }
  sqlSession.close();
}
```

本质：反射机制实现

底层：动态代理

- 设计模式笔记：http://www.kidjaya.xyz/index.php/archives/95/



### 8.3、Mybatis执行流程

![image-20200806145824754](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200806145824754.png)

### 8.4、注解实现CRUD

- 我们可以在工具类创建的时候实现自动提交事务

```java
public static SqlSession getSqlSession(){
  return sqlSessionFactory.openSession(true);
}
```

- 编写接口，增加注解

```java
public interface UserMapper {
    //获取全部用户
    @Select("select * from user")
    List<User> getUsers();

    //通过id查询用户,方法存在多个参数，所有的参数前面加@Param注解
    @Select("select * from user where id = #{id}")
    User getUserById(@Param("id") int id);

    //增加用户
    @Insert("insert into user(id,name,pwd) values (#{id},#{name},#{password})")
    int addUser(User user);

    //更新用户
    @Update("update user set name = #{name},pwd=#{password} where id = #{id}")
    int updateUser(User user);

    //删除用户
    @Delete("delete user from user where id = #{uid}")
    int deleteUser(@Param("uid") int id);
}
```

- 测试类

```java
public class Test {
    @org.junit.Test
    public void test(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = mapper.getUsers();
        for (User user : users) {
            System.out.println(user);
        }
        sqlSession.close();
    }

    @org.junit.Test
    public void getUserById(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        User user = mapper.getUserById(1);
        System.out.println(user);
        sqlSession.close();
    }

    @org.junit.Test
    public void addUser(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.addUser(new User(6,"xxxxdddd","asdkljaklsd"));
        sqlSession.close();
    }

    @org.junit.Test
    public void updateUser(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.addUser(new User(6,"xxxxdddd","asdkljaklsd"));
        sqlSession.close();
    }

    @org.junit.Test
    public void deleteUser(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.deleteUser(6);
        sqlSession.close();
    }
}
```

- **注意：我们必须要将接口绑定到核心配置文件中**

### 8.5、关于@Param()注解

- 基本类型的参数或者String类型，需要加上
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但建议加上
- 我们在SQL中引用的就是我们这里的@Param("")中设定的属性名

- **#{}和${}的区别** ---参考:https://www.cnblogs.com/zhaoyuan72/p/11597880.html
  - 两者都可以在mybatis中用在输入映射
  - #{}是预编译处理，
  - ${}是字符串替换。
  - mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；
    mybatis在处理 $ { } 时，就是把 ${ } 替换成变量的值，完成的是简单的字符串拼接。
  - 补充：在mybatis中使用#{}可以防止sql注入，提高系统安全性。
  - sql注入是什么：
    例如：用户输入的账号密码在代码中是以字符串拼接的方式生成查询语句的，这样用户输入的内容很容易改变我们的原查询代码，这就相当一一个数据库注入问题。
    我们验证密码和账号的时候，如果用户在输入密码的时候 输入 or 1=1；那不管他输入的密码是什么都可以通过。

## 9、LomBok

- java库
- 插件
- 构建工具
- 只需要加上一个注解

- 使用步骤

  1. 在IDEA中安装Lombok插件
  2. 在项目中导入lombok中的jar包
  3. 重启IDEA

  ```xml
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
  </dependency>
  ```

- 常用
  - @Date：无参构造，get，set，toString，hashcode，equals
  - @AllArgsConstructor
  - @NoArgsConstructor
  - @EqualAndHashCode
  - @ToString

## 10、多对一处理

**多对一：**

![image-20200806162324543](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200806162324543.png)

- 多个学生，对应一个老师
- 对于学生而言，关联->多个学生，关联一个老师【多对一】
- 对于老师而言，集合->一个老师有很多学生【一对多】

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200806163111628.png" alt="image-20200806163111628" style="zoom:50%;" />

**SQL:**

```mysql
CREATE TABLE `teacher` (
`id` INT(10) NOT NULL,
`name` VARCHAR(30) DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');

CREATE TABLE `student` (
`id` INT(10) NOT NULL,
`name` VARCHAR(30) DEFAULT NULL,
`tid` INT(10) DEFAULT NULL,
PRIMARY KEY (`id`),
KEY `fktid` (`tid`),
CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');
```

- **测试环境搭建**
  1. 导入lombok
  2. 新建实体类Teacher，Student
  3. 建立Mapper接口
  4. 建立Mapper.xml文件
  5. 在核心配置文件中绑定注册我们的Mapper接口或者文件
  6. 测试查询是否能够成功

- **按照查询嵌套处理**

```xml
<mapper namespace="xyz.kidjaya.dao.StudentMapper">
    <!--
     思路：
      1. 查询所有的学生信息
      2. 根据查询出来的学生tid，寻找对应的老师
    -->
    <select id="getStudent" resultMap="StudentTeacher">
        select * from student
    </select>
    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--复杂的属性需要单独处理
            对象：association
            集合：collection
        -->

        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>

    </resultMap>

    <select id="getTeacher" resultType="Teacher">
        select * from teacher where id = #{id}
    </select>
</mapper>
```

- **按照结果嵌套处理**

```xml
<!--按照结果嵌套处理-->
<select id="getStudent2" resultMap="StudentTeacher2">
  select s.id sid,s.name sname,t.name tname
  from student s,teacher t
  where s.tid = t.id
</select>
<resultMap id="StudentTeacher2" type="Student">
  <result property="id" column="sid"/>
  <result property="name" column="sname"/>
  <association property="teacher" javaType="Teacher">
    <result property="name" column="tname"/>
  </association>
</resultMap>
```



## 11、一对多处理

- 环境搭建

```java
@Data
public class Student {
    protected int id;
    private String name;
    private int tid;
}
```

```java
@Data
public class Teacher {
    private int id;
    private String name;

    //一个老师有n个学生
    private List<Student> students;
}
```

1. TeacherMapper接口

```java
//获取指定老师下所有学生和老师的信息
Teacher getTeacher(@Param("tid")int id);
Teacher getTeacher2(int id);
```

2. 编写接口对应Mapper配置文件

```xml
<mapper namespace="xyz.kidjaya.dao.TeacherMapper">
    <!--按结果嵌套查询-->
    <select id="getTeacher" resultMap="TeacherStudent">
        select s.id sid,s.name sname,t.name tname,t.id tid
        from student s,teacher t
        where s.tid = t.id and t.id = #{tid}
    </select>

    <resultMap id="TeacherStudent" type="Teacher">
        <result property="id" column="tid"/>
        <result property="name" column="tname"/>
        <!--复杂的属性，我们需要单独处理 对象：association 集合collection
        javaType=""指定属性的类型
        集合中的泛型信息，我们使用ofType获取 比如 List<Student>
        -->
        <collection property="students" ofType="Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>
    </resultMap>

    <!-- ========================================== -->
  	<!-- 按查询嵌套查询 -->
    <select id="getTeacher2" resultMap="TeacherStudent2">
        select * from teacher where id = #{id}
    </select>
    <resultMap id="TeacherStudent2" type="Teacher">
        <collection property="students" javaType="ArrayList" ofType="Student" select="getStudentByTeacherId" column="id"/>
    </resultMap>

    <select id="getStudentByTeacherId" resultType="Student">
        select * from student where tid = #{id}
    </select>
</mapper>
```

3. 将Mapper文件注册到Mybatis-config文件中

```xml
<!--绑定接口-->
<mappers>
  <mapper resource="xyz/kidjaya/dao/TeacherMapper.xml"/>
  <mapper resource="xyz/kidjaya/dao/StudentMapper.xml"/>
</mappers>
```

4. 测试

```java
public class MyTest {
    @Test
    public void test(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);

        Teacher teacher = mapper.getTeacher(1);
        System.out.println(teacher);
        /*
        Teacher(id=1, name=秦老师, students=[Student(id=1, name=小明, tid=1), Student(id=2, name=小红, tid=1),
        Student(id=3, name=小张, tid=1), Student(id=4, name=小李, tid=1), Student(id=5, name=小王, tid=1)])
         */
        sqlSession.close();
    }

    @Test
    public void test2(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        TeacherMapper mapper = sqlSession.getMapper(TeacherMapper.class);

        Teacher teacher2 = mapper.getTeacher2(1);
        System.out.println(teacher2);

        sqlSession.close();
    }
}
```

- 小结
  1. 关联 - association 【多对一】
  2. 集合 - collection 【一对多】
  3. JavaType & ofType
     - JavaType 用来制定实体类中属性的类型
     - ofType用来指定映射到List或者集合中的pojo类型，泛型中的约束类型
- 注意点
  - 保证SQL的可读性，尽量保证通俗易懂
  - 注意一对多和多对一中，属性名和字段的问题
  - 如果问题不好排查错误，可以使用日志，建议使用Log4j，可以输出日志文件

- **面试高频**
  - Mysql引擎
  - InnoDB底层原理
  - 索引
  - 索引优化



## 12、动态SQL

- **什么是动态sql：动态sql就是根据不同的条件生成不同的sql语句**

### 12.1、简介



### 12.2、搭建环境

- 新建一个数据库表

```mysql
CREATE TABLE `blog`(
`id` VARCHAR(50) NOT NULL COMMENT '博客id',
`title` VARCHAR(100) NOT NULL COMMENT '博客标题',
`author` VARCHAR(30) NOT NULL COMMENT '博客作者',
`create_time` DATETIME NOT NULL COMMENT '创建时间',            
`views` INT(30) NOT NULL COMMENT '浏览量',
 PRIMARY KEY ( `id` )
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

- 创建一个基础工程

  1. 导包
  2. 编写配置文件
  3. 编写实体类

  ```java
  @Data
  public class Blog {
      private int id;
      private String title;
      private String author;
      private Date createTime;
      private int views;
  }
  ```

  4. 编写实体类对应Mapper接口和Mapper.xml文件

### 12.3、if语句

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
  select * from blog where 1=1
  <if test="title != null">
    and title = #{title}
  </if>
  <if test="author != null">
    and author = #{author}
  </if>
</select>
```

### 12.4、where语句

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
  select * from blog
  <where>
    <if test="title != null">
      and title = #{title}
    </if>
    <if test="author != null">
      and author = #{author}
    </if>
  </where>
</select>
```

- 这个where标签会知道如果它包含的标签中有返回值的话，他就插入一个where，此外，如果标签返回的内容是以and 或者 or开头的，则他会剔除掉

### 12.5、Set

```xml
<update id="updateBlogSet" parameterType="map">
  update blog
  <set>
    <if test="title != null">
      title = #{title},
    </if>
    <if test="author != null">
      author = #{author},
    </if>
  </set>
  where id = #{id}
</update>
```

- 会对多余的逗号进行处理

### 12.6、choose

> 有时候，我们不想用到所有的查询，只想选择其中一个，查询条件有一个满足即可

```xml
<select id="queryBlogChoose" parameterType="map" resultType="blog">
  select * from blog
  <where>
    <choose>
      <when test="title != null">
        title = #{title}
      </when>
      <when test="author != null">
        author = #{author}
      </when>
      <otherwise>
        views = #{views}
      </otherwise>
    </choose>
  </where>
</select>
```

### 12.7、SQL片段

- 有时候我们某个sql语句用得特别频繁，为了增加代码的重用性，简化代码，我们可以将这些代码抽离出来，然后使用的时候直接调用

- 提取SQL片段

```xml
<sql id="if-title-author">
  <if test="title != null">
    and title = #{title}
  </if>
  <if test="author != null">
    and author = #{author}
  </if>
</sql>
```

- 引用SQL片段

```xml
<select id="queryBlogIf" parameterType="map" resultType="blog">
  select * from blog
  <where>
    <include refid="if-title-author"></include>
  </where>
</select>
```

- 注意：
  - 最好给予单表来定义sql片段，提高片段的可重用性，就是尽量的简单
  - 在sql片段中不要包括where

### 12.8、foreach

- 将数据库前三个数据的id修改为1，2，3
- 查询blog表中id分别为1，2，3的博客信息

1. 编写接口

```java
//查询范围内的博客
List<Blog> queryBlogForeach(Map map);
```

2. 编写SQL语句

```xml
<select id="queryBlogForeach" parameterType="map" resultType="blog">
  select * from blog
  <where>
    <!--
     collection:指定输入对象中的集合属性
     item:每次遍历生成的对象
     open:开始遍历时的拼接字符串
     close:结束时拼接的字符串
     separator:遍历对象之间需要拼接的字符串
     select * from blog where 1=1 and (id=1 or id=2 or id=3)
    -->
    <foreach collection="ids" item="id" open="and (" close=")" separator="or">
      id=#{id}
    </foreach>
  </where>
</select>
```

3. 测试

```java
@Test
public void queryBlogForeach(){
  SqlSession sqlSession = MybatisUtils.getSqlSession();
  BlogMapper mapper = sqlSession.getMapper(BlogMapper.class);
  HashMap map = new HashMap();
  List<Integer> list = new ArrayList<Integer>();
  list.add(1);
  list.add(3);
  map.put("ids",list);
  List<Blog> blogs = mapper.queryBlogForeach(map);
  for (Blog blog : blogs) {
    System.out.println(blog);
  }
  sqlSession.close();
}
```

- 小结
  - 其实动态sql语句的编写往往就是一个拼接的问题，为了保证拼接准确，我们最好首先要写原生的sql语句出来，然后通过mybatis动态sql对照着改，防止出错。多在实践中使用才是熟练掌握它的技巧

## 13、缓存

### 13.1、简介

> 查询：连接数据库-> 耗费资源
> 一次查询结果，给它暂存再一个可以直接取到的地方！--> 内存:缓存
>
> 我们再次查询相同数据的时候，直接走缓存，就不用走数据库了

1. 什么是缓存【Cache】？
   - 存在内存中的临时数据
   - 将用户经常查询的数据放在缓存（内存）中，用户查询数据就不用从磁盘上（关系型数据库文件）查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题
2. 为什么使用缓存？
   - 减少和数据库的交互次数，减少系统开销，提高系统效率
3. 什么样的数据能使用缓存 
   - 经常查询并且不经常改变的数据。【可以使用缓存】

### 13.2、MyBatis缓存

- MyBatis包含一个非常强大的查询缓存特性，他可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。
- MyBatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
  - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存
  - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来定义二级缓存

### 13.3、一级缓存

- 一级缓存也叫本地缓存
  - 与数据库同一次会话期间查询到的数据会放在本地缓存中
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库

- 测试步骤

  1. 开启日志
  2. 测试在一个Session中查询两次
  3. 查看日志输出

  ![image-20200807232111394](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200807232111394.png)

- 缓存失效情况

  1. 查询不同的东西

  2. 增删改操作，可能会改变原来的数据，所以必定会刷新缓存
  3. 查询不同的Mapper.xml，不同的SqlSession
  4. 手动清理缓存 

  ```java
  public class MyTest {
      @Test
      public void test(){
          SqlSession sqlSession = MybatisUtils.getSqlSession();
          UserMapper mapper = sqlSession.getMapper(UserMapper.class);
  
          User user = mapper.queryUserById(1);
          System.out.println(user);
          System.out.println("============");
  
  //        mapper.updateUser(new User(2,"123","dingnigefei"));
          sqlSession.clearCache();
  
          User user2 = mapper.queryUserById(1);
          System.out.println(user2);
          System.out.println(user == user2);
  
          sqlSession.close();
      }
  }
  ```

- 一级缓存是默认开启的，只在一次SqlSession中有效，也就是拿到连接到关闭连接这个区间

- 一级缓存相当于一个Map

### 13.4、二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存
- 基于namespace级别的缓存，一个命名空间，对应一个二级缓存
- 工作机制：
  - 一个会话查询一条数据，这个数据就会被放在当前回话的一级缓存中
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了，但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中
  - 新的会话查询信息，就可以从二级缓存中获取内容
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中

- 步骤

  1. 开启全局缓存

  ```xml
  <!--显示的开启全局缓存-->
  <setting name="cacheEnabled" value="true"/>
  ```

  2. 在要使用耳机缓存的mapper中开启

  ```xml
  <!--在当前mapper.xml中使用二级缓存-->
  <cache/>
  ```

  也可以自定义一些参数

  ```xml
  <cache
         eviction="FIFO"
         flushInterval="60000"
         size="512"
         readOnly="true"/>
  ```

  3. 测试

     - 注意：我们需要将实体类序列化，否则就会报错

     ```
     Cause: java.io.NotSerializableException: xyz.kidjaya.pojo.User
     ```

- 小结

  - 只要开启了二级缓存，在同一个mapper下就有效
  - 所有的数据都会先放到一级缓存中
  - 只有当会话提交，或者关闭的时候，才会放到二级缓存中

### 13.5、缓存原理

![image-20200808093334232](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200808093334232.png)

### 13.6、自定义缓存ehcache

> Ehcache是一种广泛使用的开源Java分布式缓存。主要面向通用缓存

- 要在程序中使用ehcache，先要导包（别人实现好的Cache接口）

```xml
<dependency>
  <groupId>org.mybatis.caches</groupId>
  <artifactId>mybatis-ehcache</artifactId>
  <version>1.2.1</version>
</dependency>
```

- 在mapper中指定使用我们的ehcache缓存实现

```xml
<!--在当前mapper.xml中使用二级缓存-->
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

- ehcache.xml

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200808100357321.png" alt="image-20200808100357321" style="zoom:50%;" />

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">

    <diskStore path="./tmpdir/Tmp_EhCache"/>

    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>

    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>
</ehcache>
```

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200808100330754.png" alt="image-20200808100330754" style="zoom:50%;" />





