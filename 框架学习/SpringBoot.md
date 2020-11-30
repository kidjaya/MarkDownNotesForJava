# SpringBoot



## 原理初探

- 自动配置：

- pom.xml
  - Spring-boot-denpendencies:核心依赖在父工程中
  - 我们在写或者引入一些SpringBoot依赖的时候，不需要制定版本，就应为有这些版本库

- 启动器

  - ```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
    </dependency>
    ```

  - 启动器：说白了就是Spring的启动场景

  - 比如spring-boot-starter-web,他就会帮我们自动导入web环境所有的依赖

  - Springboot会将所有的功能场景，都变成一个个的启动器

  - 我们需要什么功能，就只需要找到对应的启动期就可以了`start`

  - 官网链接：https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter

### 主程序

```java
//SpringBootApplication：标注这个类是一个springboot应用
@SpringBootApplication
public class Springboot01HelloworldApplication {
  public static void main(String[] args) {
    //将springboot应用启动
    SpringApplication.run(Springboot01HelloworldApplication.class, args);
  }
}
```

- 注解

## SpringBoot配置

```yaml
# k=v
# 对空格的要求十分搞
# 可以注入到我们的配置文件中

# 普通的key-value
name: kidjaya

#对象
student:
  name: kidjaya
  age: 19
#行内写法
student2: {name: kidjaya,age: 19}
#数组
pets:
  - cat
  - dog
  - pig
#行内写法
pets2: [cat,pig,dog]
```

---



## Spirng Web开发

-  在Springboot中，我们可以使用以下方式处理静态资源
  - webjars `localhost:8080/webjars/`
  - public , static , /** , resources  `localhost:8080`
- 优先级：resource>static(默认)>public