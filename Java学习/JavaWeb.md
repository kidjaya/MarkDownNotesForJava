# JavaWeb

> 视频参考（狂神）<https://www.bilibili.com/video/BV12J411M7Sj?p=1>

---

## 基本概念

### web开发

- web，网页
- 静态web
  - html、css
  - 提供给别人看的数据始终不会发生变化
- 动态web
  - 淘宝网站等
  - 提供给别人看的数据会发生变化，每个人在不同时间，看到的信息不同
  - 技术栈：Sevlet/JSP、ASP、PHP

在Java中，动态web开发资源的技术统称为JavaWeb

### web应用程序

- web浏览器：可以提供浏览器访问的程序
  - a.html...多个web资源，对外部提供服务
  - 能访问到任何页面资源都存在于世界的某一个角落的计算机上
  - URL
  - 统一的web资源会放在同一个文件夹下，就是web应用程序->Tomcat:服务器
  - 一个web应用由多部分组成（静态、动态web）
    - html css js
    - jsp servlet
    - java程序
    - jar包
    - 配置文件（Properties）

web应用程序编写完后，若想提供外界访问：需要提供一个服务器来统一管理。

### 静态web

- *.htm *.html这些都是网页的后缀，如果服务器一直存着这些东西，我们就可以直接进行读取

  <img src="https://s1.ax1x.com/2020/05/17/Y2Vtl8.md.png" alt="Y2Vtl8.md.png" style="zoom:150%;" />
- 静态web存在的缺点
  - Web无法动态更新，所有用户都是同一个页面
  - 轮播图，点击特效：伪动态
  - JavaScript【实际开发用的多】
  - VBScript
- 他无法和数据库交互（数据无法持久化，用户无法交互）

### 动态web

- 页面会动态展示：因人而异
- ![Y2V6pV.md.png](https://s1.ax1x.com/2020/05/17/Y2V6pV.md.png)

- 缺点

  - 加入服务器的动态web资源出现了错误，需要重新编写**后台程序**，意味着要重新发布
    - 停机维护

- 优点

  - Web页面可以动态更新，所有用户看到都不是同一个页面

  - 它可以与数据库交互

    ![Y2VX0H.md.png](https://s1.ax1x.com/2020/05/17/Y2VX0H.md.png)



---

## web服务器

### 技术讲解

- ASP
  - 微软、国内最早流行的就是ASP

  - 在HTML中嵌入VB的脚本，ASP+COM

  - 在ASP开发中，基本的一个页面都有几千行代码，页面及其混乱

  - 维护成本高

    ```html
    <html>
      <html>
    		<html>
      		<%
             System.out.println("hello")
           %>
    		</html>  
    	</html>
    </html>
    ```

- PHP

  - PHP开发速度快，功能强大，跨平台，代码简单（70%，WordPress）
  - 缺点：无法承载大访问量的情况（局限性）

- JSP/Servlet

  - B/S：浏览器/服务器
  - C/S：客户端/服务器
  - sun公司主推的B/S架构
  - 基于java语言的（所有大公司，或者一些开源组件，都是用java写的）
  - 可以承载三高问题带来的影响：高并发高可用高性能
  - 语法像ASP，方便ASP转JSP，加强竞争度

### web服务器

> 服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应

- IIS
  
  - 微软的：ASP..，Windows中自带的
  
- Tomcat
  - Apache软件基金会的，免费的，适合初学者，最新版本为9.0
  
    ![tomcat](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=1397363100,4264619668&fm=26&gp=0.jpg)
  - **工作3-5年后可以尝试手写Tomcat服务器**
  - 下载Tomcat
    1. 安装
    2. 了解配置文件和目录结构
    3. 了解这个东西的作用 
  
- Apache

- ...



---

## Tomcat

### tomcat启动和配置

- 文件夹信息

![Y2dNeH.md.png](https://s1.ax1x.com/2020/05/17/Y2dNeH.md.png)

- 启动startup.sh 关闭shutdown.bat
- 可能遇到的问题：
  1. Java环境变量没有配
  2. 闪退问题：需要配置兼容性
  3. 乱码问题：配置文件中设置

### 配置

![Y20Xz6.md.png](https://s1.ax1x.com/2020/05/17/Y20Xz6.md.png)

- 可以配置启动的端口号

  ![Y2BESP.png](https://s1.ax1x.com/2020/05/17/Y2BESP.png)

  - tomcat默认端口：8080
  - mysql：3306
  - http：80
  - https：443

- 可以配置主机的名称

  - 默认的主机名：localhost—>127.0.0.1
  - 默认网站应用存放的地址为：webapps

    ![Y2B1Wq.png](https://s1.ax1x.com/2020/05/17/Y2B1Wq.png)

- **面试题**

  - 请谈谈网站是如何进行访问的
    1. 输入一个域名；回车
    2. 检查本机的hosts配置文件下有没有这个域名的映射
       1. 有：直接返回对应的ip地址，这个地址中，有我们要访问的web程序，可以直接访问
       2. 没有：去DNS服务器上找（全世界的域名管理）
    
  
  ![Y2rC8I.md.png](https://s1.ax1x.com/2020/05/17/Y2rC8I.md.png)
  
  - 配置tomcat环境变量

### 发布一个web网站

​	 1. 将自己写的网站放到服务器指定的web应用下的文件夹（webapps）下，就可以访问了

- 网站应该又的结构

  ```java
  --webapps : Tomcat服务器的web目录
    -ROOT
    -Blog ：网站的目录名
    	- WEB-INF
    		- classes: java程序
        - lib: web应用所依赖的jar包
        - web.xml: 网站的配置文件
      - index.html 默认的首页
      - static 静态资源文件
          - css
          - js
          - img 
  ```



---

## Http

### 什么是http

- 什么是http
  -  超文本传输协议，是一个简单的请求响应的协议，他通常运行在TCP之上
    - 文本：html，字符串
    - 超文本：图片、音乐、视频、定位、地图
    - 80
- https：安全的，第三方验证
  - 443

### 两个时代

- http1.0
  - Http/1.0：客户端可以和web服务器连接，只能获得一个web资源，断开连接
- http2.0
  - Http/1.1：可以获得多个web资源。

### HTTP请求

- 客户端--发请求Request--服务器

  ```java
  Request URL: https://www.baidu.com/ 请求地址
  Request Method: GET	请求方法
  Status Code: 200 OK 状态码
  Remote Address: 14.215.177.38:443  远程地址：端口
  ```

  ```java
  Accept: text/html
  Accept-Encoding: gzip, deflate, br
  Accept-Language: zh-CN,zh;q=0.9,en;q=0.8  语言
  Connection: keep-alive
  ```

- 请求行

  - 请求行中的请求方式：GET
  - 请求方式：GET/POST/DELETE...
    - Get:请求能携带参数有限，会在url地址栏显示数据内容，不安全，但高效
    - Post:请求能携带参数没有限制，不在url显示，安全，但不高效

- 请求头

  ```java
  Accept: 告诉浏览器，他所支持的数据类型
  Accept-Encoding: 支持那种编码格式 GBK UTF-8 GB232 ISO8859-1
  Accept-Language: 告诉浏览器，它的语言环境
  cache-control:缓存控制
  Connection: 告诉浏览器，请求完成是断开还是保持连接
  Host：主机
  ```

  

### HTTP响应

- 服务器--响应Response--客户端

  ```java
  Cache-Control: private  缓冲控制
  Connection: keep-alive	连接
  Content-Encoding: gzip	编码
  Content-Type: text/html;charset=utf-8 返回类型
  ```

- 响应体

  ```java
  Accept: 告诉浏览器，他所支持的数据类型
  Accept-Encoding: 支持那种编码格式 GBK UTF-8 GB232 ISO8859-1
  Accept-Language: 告诉浏览器，它的语言环境
  cache-control:缓存控制
  Connection: 告诉浏览器，请求完成是断开还是保持连接
  Host：主机
  Refrush: 告诉客户端，多久刷新一次
  Location: 让网页重新定位
  ```

- 响应状态码
  - 200:请求响应成功
  - 3xx:请求重定向 200
    - 重定向：你重新到我给你的新位置
  - 4xx:找不到资源 404
    - 资源不存在
  - 5xx:服务器代码错误 500
    - 502:网关错误
- **面试题**
  - 当你的浏览器地址栏输入地址栏并回车的一瞬间到页面能显示回来，经历了什么
  - 三次挥手，四次握手



---

## Maven

> 为什么要学习这个技术？
>
> 1. 在javaWeb开发中，需要使用大量的jar包，我们手动去导入
> 2. 如何让一个东西自动帮我们导入和配置这个jar包
>
> 所以maven诞生了～

### Maven项目架构管理工具

- 用来导jar包的
- Maven的核心思想：**约定大于配置**
  - 有约束，不要去违反
  - Maven会规定好你该如何编写Java代码，必须按照规范来

### 下载安装Maven

![Y22Arq.md.png](https://s1.ax1x.com/2020/05/17/Y22Arq.md.png)

- 建议：电脑环境放置在一个文件夹下，方便管理

### 配置环境变量

- 在系统环境变量中配置如下配置
  - M2_HOME
  - MAVEN_HOME
  - PATH
- 测试Maven是否安装成功： mvn -v(version)

  ![Y2fJZq.png](https://s1.ax1x.com/2020/05/17/Y2fJZq.png)

### 配置阿里云镜像

- 镜像：mirrors

  - 作用：加速我们下载

- 国内建议使用阿里云镜像

   ```xml
   <mirror>
           <!--This sends everything else to /public -->
           <id>nexus-aliyun</id>
           <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf> 
   				<name>Nexus aliyun</name>
           <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      </mirror>
   ```


### 本地仓库

- 在本地的仓库，远程仓库

- 建立一个本地仓库：localRepository

    ```xml
    <localRepository>/Users/kidjaya/Library/Enviroment/apache-maven-3.6.3/maven-repo</localRepository>
    ```


### 在idea中使用Maven

1. 启动idea

2. 创建一个Maven项目

   ![YRiwsH.md.png](https://s1.ax1x.com/2020/05/17/YRiwsH.md.png)

   ![YRiIwn.md.png](https://s1.ax1x.com/2020/05/17/YRiIwn.md.png)

   ![YRFA6e.md.png](https://s1.ax1x.com/2020/05/17/YRFA6e.md.png)

6. **MAC很多奇怪的配置错误**

   1. 是否下载了zch
   2. maven版本报错问题
   3. open -e 重新打开idea 默认配置项刷新
   4. 删除项目后----> **File --- restart and invalided** 刷新缓存，避免奇怪错误

4. 注意事项：idea项目创建后要多一个心眼看看基础配置

   [![YROLPH.md.png](https://s1.ax1x.com/2020/05/17/YROLPH.md.png)](https://imgchr.com/i/YROLPH)

   ![YRDIFH.md.png](https://s1.ax1x.com/2020/05/17/YRDIFH.md.png)

9. 到这里，Maven在IDE的配置和使用就Ok了～

### 在idea中标记文件夹功能

![YRrb4J.png](https://s1.ax1x.com/2020/05/17/YRrb4J.png)

![YRsJK0.md.png](https://s1.ax1x.com/2020/05/17/YRsJK0.md.png)

![YRsBG9.md.png](https://s1.ax1x.com/2020/05/17/YRsBG9.md.png)

### 在idea中配置tomcat

![YRyFdU.md.png](https://s1.ax1x.com/2020/05/17/YRyFdU.md.png)

- MAC 在 配置apcheConfigure的时候出现

![YR6Dnx.md.png](https://s1.ax1x.com/2020/05/17/YR6Dnx.md.png)

- 配置tomcat解决

  ![YRRcqI.md.png](https://s1.ax1x.com/2020/05/17/YRRcqI.md.png)
- 配置idea项目

  [![YRWko6.md.png](https://s1.ax1x.com/2020/05/17/YRWko6.md.png)](https://imgchr.com/i/YRWko6)

- 解决警告问题
- 为什么会出现这个问题：**我们访问一个网站，必须需要指定一个文件夹名字**

  ![YRWBT0.md.png](https://s1.ax1x.com/2020/05/17/YRWBT0.md.png)

  ![YRhlPf.png](https://s1.ax1x.com/2020/05/17/YRhlPf.png)

  ![YR4Ikq.md.png](https://s1.ax1x.com/2020/05/17/YR4Ikq.md.png)

  ![YRhLdI.md.png](https://s1.ax1x.com/2020/05/17/YRhLdI.md.png)

### pom文件

- pom.xml是Maven的核心配置文件

  ![YR5N40.md.png](https://s1.ax1x.com/2020/05/17/YR5N40.md.png)


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--Maven版本和头文件 -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!--这里就是我们配置的GAV-->
  <groupId>xyz.kidjaya</groupId>
  <artifactId>javaweb-01-maven01</artifactId>
  <version>1.0-SNAPSHOT</version>

  <!--Package：项目的打包方式
  jar：java应用
  war：javaWeb应用
  -->
  <packaging>war</packaging>

  <!--配置-->
  <properties>
    <!--项目的默认构造编码-->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--编译版本-->
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <!--项目依赖-->
  <dependencies>
    <!--Maven的高级之处在于，他会帮你导入Jar包所依赖的其他jar包-->
    <!--具体依赖的jar包配置文件-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <!--项目构建用的东西-->
  <build>
    <finalName>javaweb-01-maven01</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

- Maven由于它的约定大于配置，我们之后可能遇到我们写的配置文件，无法被导出或者生效的问题，解决方案

  ```xml
  <!--在build中配置resources，来防止我们资源导出失败的问题-->
  <build>
      .......
        <resources>
          <resource>
              <directory>src/main/resources</directory>
              <excludes>
                  <exclude>**/*.properties</exclude>
                  <exclude>**/*.xml</exclude>
               </excludes>
              <filtering>false</filtering>
          </resource>
          <resource>
              <directory>src/main/java</directory>
              <includes>
                  <include>**/*.properties</include>
                  <include>**/*.xml</include>
              </includes>
              <filtering>false</filtering>
          </resource>
      </resources>
      ......
  </build>
  ```

### Idea操作

![YRoJf0.md.png](https://s1.ax1x.com/2020/05/17/YRoJf0.md.png)

- [![YRou6S.png](https://s1.ax1x.com/2020/05/17/YRou6S.png)](https://imgchr.com/i/YRou6S)

### Maven仓库的使用

地址：<https://mvnrepository.com/>

- 首先

![YWQEs1.png](https://s1.ax1x.com/2020/05/18/YWQEs1.png)

- 遇到报红问题

  ![resovle](https://img-blog.csdn.net/20180517090406916?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RvdWRvdTA4MDk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- 放置到pom的依赖

  ![YWawM8.png](https://s1.ax1x.com/2020/05/18/YWawM8.png)
  
  

---

## Servlet

### Servlet简介

- Servlet是sun公司开发动态web的一门技术

- Sun在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要完成两个小步骤

  1. 编写一个类，实现Servlet接口

  2. 把开发好的Java类部署到web服务器中

把实现了Servlet接口的Java程序叫做，Servlet

### HelloServlet

> Servlet接口在sun公司有两个默认的实现类：HttpServlet，GenericServlet

1. 构建一个普通的Maven项目，删除里面的src目录，以后学习就在这个项目里面建立Moudel；这个空的工程就是Maven主项目

2. 关于Maven父子工程的理解：

- 父项目会有

```xml
<modules>
    <module>servlet-01</module>
</modules>
```

- 子项目会有

  - 可能会有parent标签，没有的话需要自行添加command + n ==》 Parent

  ```xml
  <parent>
      <artifactId>javaweb-02-servlet</artifactId>
      <groupId>xyz.kidjaya</groupId>
      <version>1.0-SNAPSHOT</version>
    </parent>
  ```

- 父项目中的java，子项目可以直接使用

  > Son extends Father

3. Maven环境优化

   1. 修改web.xml为最新的
   2. 将maven的结构搭建完整

4. 编写一个Servlet程序

   1. 编写一个普通类
   2. 实现Servlet接口，直接继承HttpServlet

   ```java
   public class HelloServlet extends HttpServlet {
       //由于get和post知识请求实现的不同的方式，可以互相调用，业务逻辑都一样
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           PrintWriter writer = resp.getWriter();//响应流
           writer.print("Hello Servlet");
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doGet(req, resp);
       }
   }
   ```

5. 编写Servlet的映射

   - 为什么需要映射？
   - 我们写的是Java程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务器中注册我们写的Servlet，还需要给它们一个浏览器能够访问的路径

   ```xml
   <!--注册Servlet-->
       <servlet>
           <servlet-name>hello</servlet-name>
           <servlet-class>xyz.kidjaya.servlet.HelloServlet</servlet-class>
       </servlet>
       <!--Servlet映射路径-->
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   ```

6. 配置tomcat

   - 注意：配置项目发布的路径！

7. 启动测试测试

### Servlet原理

> Servlet是由Web服务器调用，web服务器在收到浏览器请求后会：

![YW7MN9.png](https://s1.ax1x.com/2020/05/18/YW7MN9.png)

### Mapping问题

1. 一个Servlet可以指定一个映射路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
   </servlet-mapping>
   ```

2. 一个Servlet可以指定多个映射路径

   ```xml
   		<servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello2</url-pattern>
       </servlet-mapping>
       <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello3</url-pattern>
       </servlet-mapping>
   ```

3. 一个Servlet可以指定通用映射路径

   ```xml
   		<servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/hello/*</url-pattern>
       </servlet-mapping>
   ```

4. 默认请求路径 优先级大于index 覆盖默认访问路径

   ```xml
   <servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>/*</url-pattern>
       </servlet-mapping>
   ```

5. 可以自定义后缀实现请求映射

   - 注意点：*前面不能加项目映射的路径，无论前面路径怎么加，后缀是这个就访问

   ```xml
   		<servlet-mapping>
           <servlet-name>hello</servlet-name>
           <url-pattern>*.yanghangjie</url-pattern>
       </servlet-mapping>
   ```

6. 优先级问题

   - 指定了固有的映射路径优先级高，如果找不到就会走默认的处理请求，一般默认请求放在最下面

   ```xml
   <!--404-->
       <servlet>
           <servlet-name>error</servlet-name>
           <servlet-class>xyz.kidjaya.servlet.ErrorServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>error</servlet-name>
           <url-pattern>/*</url-pattern>
       </servlet-mapping>
   <!--自定义404-->
   ```

### ServletContext

- web容器在启动的时候，它会为每一个web程序都创建一个对应的ServletContext对象，它代表当前的web应用

  - 共享数据

    - 我在这个Servlet中保存的数据，可以在另一个Servlet中拿到

    ```java
    public class HelloServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            //this.getInitParameter(); 初始化参数
            //this.getServletConfig(); Servlet配置
            //this.getServletContext(); Servlet上下文
            ServletContext servletContext = this.getServletContext();
            String username = "kidjaya";//数据
            servletContext.setAttribute("username",username);
        }
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
        }
    }
    ```

    ```java
    public class GetServlet extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            ServletContext servletContext = this.getServletContext();
            String username = (String) servletContext.getAttribute("username");
            resp.setCharacterEncoding("utf-8");
            resp.setContentType("text/html");
            resp.getWriter().print("用户名："+username);
        }
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            doGet(req, resp);
        }
    }
    ```

    ```xml
    		<servlet>
            <servlet-name>hello</servlet-name>
            <servlet-class>xyz.kidjaya.servlet.HelloServlet</servlet-class>
        </servlet>
        <servlet-mapping>
            <servlet-name>hello</servlet-name>
            <url-pattern>/hello</url-pattern>
        </servlet-mapping>
        
        <servlet>
            <servlet-name>getC</servlet-name>
            <servlet-class>xyz.kidjaya.servlet.GetServlet</servlet-class>
        </servlet>
        <servlet-mapping>
            <servlet-name>getC</servlet-name>
            <url-pattern>/getContext</url-pattern>
        </servlet-mapping>
    ```

    - 测试访问结果

 ### 获取初始化参数

```xml
    <!--配置一些web应用初始化参数-->
    <context-param>
        <param-name>jdbcUrl</param-name>
        <param-value>jdbc:mysql://localhost:3306</param-value>
    </context-param>
```

```java
 @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        //获取xml配置参数
        String url = servletContext.getInitParameter("jdbcUrl");
        resp.getWriter().print(url);
    }
```

### 请求转发

```java
public class ServletDemo04 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        System.out.println("进入了04Demo");
        //请求转发 url路径不变，内容变化
        //RequestDispatcher dispatcher =servletContext.getRequestDispatcher("/gp");//转发的请求路径
        //dispatcher.forward(req,resp);//调用forward实现请求转发
        servletContext.getRequestDispatcher("/gp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

![Yfv4sA.png](https://s1.ax1x.com/2020/05/18/Yfv4sA.png)

### 读取资源文件

- Properties
  - 在java目录下新建properties
  - 在resources目录下新建properties
- 发现：都被打包到了同一个路径：classes，我们俗称这个路径为classpath

- 思路：需要一个文件流

 ```java
public class Servlet05 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        InputStream resourceAsStream = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");

        Properties prop = new Properties();
        prop.load(resourceAsStream);
        String user = prop.getProperty("name");
        String id   = prop.getProperty("id");
				resp.getWriter().print(user+":"+id);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }
}
 ```

- 访问测试OK即可

### HttpServletRequest

> 代表客户端的请求，用户通过http协议访问服务器，Http请求中的所有信息都会被封装到HttpServletRequest，通过这个HttpServletRequest的方法，获得客户端的所有信息

 ![YOi87n.png](https://s1.ax1x.com/2020/05/22/YOi87n.png)

- 获取客户端传来参数

![image-20200522121654915](/Users/kidjaya/Library/Application Support/typora-user-images/image-20200522121654915.png)

- 请求转发

```java
 protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("进入这个请求了");
        //处理请求
        String username = req.getParameter("username");
        String password = req.getParameter("password");

        System.out.println("name:"+username+",password:"+password);
				//转发请求
        req.getRequestDispatcher("test/hello.jsp").forward(req,resp);
    }
```

- 处理完了，分发到下一个[JSP](https://www.baidu.com/s?wd=JSP&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)页面或者下一个Action继续处理。
  会有forward()和redirect()两种情况，forward()是request中的参数继续传递，redirect()则是重新生成request了。

### HttpServletResponse

- web服务器接收到客户端http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，一个代表响应的一个HttpServletResponse
  - 如果要获取客户端请求过来的参数：找HttpServletRequest
  - 如果要给客户端响应一些信息：找HttpServletResponse

1. 简单分类

   - 负责向浏览器发送数据的方法

   ```java
   public ServletOutputStream getOutputStream() throws IOException;
   public PrintWriter getWriter() throws IOException;
   ```

   - 负责向浏览器发送请求头的方法

   ```java
   //ServletResponse
   public void setCharacterEncoding(String charset);
   public void setContentLength(int len);
   public void setContentLengthLong(long len);
   public void setContentType(String type);
   //HttpServletResponse
   public void addDateHeader(String name, long date);
   public void setHeader(String name, String value);
   public void addHeader(String name, String value);
   public void setIntHeader(String name, int value);
   public void addIntHeader(String name, int value);
   public void setStatus(int sc);
   ```

2. 常见应用

   1. 向浏览器输出消息

   2. 下载文件

      1. 获取下载文件的路径
      2. 下载的文件名
      3. 设置想办法让浏览器下载我们需要的东西
      4. 获取下载文件的输入流
      5. 创建缓冲区
      6. 获取OutputStream对象
      7. 将FileOutputStream写入到buffered缓冲区
      8. 使用OutputStream将缓冲区的数据输出到客户端

      ```java
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      // 1. 获取下载文件的路径
                  String realPath = this.getServletContext().getRealPath("/WEB-INF/classes/目标作息.png");
                  System.out.println("下载文件的路径："+realPath);
      // 2. 下载的文件名
                  String fileName = realPath.substring(realPath.lastIndexOf("/") + 1);
                  System.out.println("fileName = " + fileName);
      // 3. 设置想办法让浏览器下载我们需要的东西 如何解决中文乱码？URLEncoder
                  resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(fileName,"utf-8"));
      // 4. 获取下载文件的输入流
                  FileInputStream in = new FileInputStream(realPath);
      // 5. 创建缓冲区
                  int len = 0;//有效字符数
                  byte[] buffer = new byte[1024];//缓冲区
      // 6. 获取OutputStream对象
                  ServletOutputStream out = resp.getOutputStream();
      // 7. 将FileOutputStream写入到buffered缓冲区,使用OutputStream将缓冲区的数据输出到客户端
                  while ((len=in.read(buffer))!=-1){
                      out.write(buffer,0,len);//写入有效字节
                  }
                  in.close();
                  out.close();
              }
      ```

      - **注意pom.xml配置参数！！！否者很多未知问题**

   3. 验证码功能

      - 验证怎么来的？

        - 前端实现
        - 后端实现，需要用到java的图片类，产生一个图片

        ```java
        public class ImageServlet extends HttpServlet {
            @Override
            protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                //如何让浏览器五秒刷新一次
                resp.setHeader("refresh","3");
        
                //在内存中创建一个图片
                BufferedImage image = new BufferedImage(80, 20, BufferedImage.TYPE_3BYTE_BGR);
                //得到图片
                Graphics2D g = (Graphics2D) image.getGraphics();//笔
                //设置图片背景颜色
                g.setColor(Color.white);
                g.fillRect(0,0,80,20);
                //给图片写数据
                g.setColor(Color.BLUE);
                g.setFont(new Font(null,Font.BOLD,20));
                g.drawString(makeNum(),0,20);
                //告诉浏览器，这个请求用图片的方式打开
                resp.setContentType("image/jpg");
                //网站存在缓存，不让浏览器缓存
                resp.setDateHeader("expires", -1);
                resp.setHeader("Cache-Control","no-cache");
                resp.setHeader("Pragma","no-cache");
                //把图片写给浏览器
                ImageIO.write(image,"jpg",resp.getOutputStream());
            }
        
            //生成随机数
            private String makeNum(){
                Random random = new Random();
                String num = random.nextInt(9999999)+"";
                StringBuffer sb = new StringBuffer();
                for (int i = 0; i < 7 - num.length(); i++) {
                    sb.append("0");
                }
                num = sb.toString() + num;
                return num;
            }
        
            @Override
            protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            }
        }
        ```

   4. 实现重定向

      ![YhzJzD.png](https://s1.ax1x.com/2020/05/18/YhzJzD.png)

      - B一个web资源收到客户端A请求后，B他会通知A客户端去访问另一个web资源C，这个过程叫做重定向
      - 常见场景：
        - 用户登陆

      ```java
      public void sendRedirect(String location) throws IOException;
      ```

      ```java
      public class RedirectServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              /*
              * 重定向相当于做了这两步
              * resp.setHeader("Location","/servlet_03_war/img");
              * resp.setStatus(HttpServletResponse.SC_MOVED_TEMPORARILY);
               */   
      
              resp.sendRedirect("/servlet_03_war/img");//重定向
          }
      
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          }
      }
      ```

      **面试题：重定向和转发的区别**

      - 相同点
        - 页面都会实现跳转
      - 不同点
        - 请求转发，url不会产生变化
        - 重定向，url地址栏会发生变化
        - request资源共享区别

      

      - 路径问题(404错误)和获取url参数～

      ```java
      public class RequestTest extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              System.out.println("进入这个请求了");
              //处理请求,设置参数
              String username = req.getParameter("username");
              String password = req.getParameter("password");
      
              System.out.println("name:"+username+",password:"+password);
      
              //重定向页面注意路径问题，否则报404错误
              resp.sendRedirect("test/hello.jsp");
              /**
               * / 代表当前网址 如www.baidu.com/
               * ./ 代表当前页面，所处在的文件夹下或者虚拟路径下的地址 如www.kidjaya.xyz/test/  默认不填有同样的效果
               * ../ 代表上一级页面～
               */
          }
      
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          }
      }
      ```

---

## Cookie、Session

### 会话

- 用户打开一个浏览器，点击了很多超链接，访问多个web，这个过程可以称之为会话

### 有状态会话

- 客户端访问服务端，服务端会知道这个客户端，曾经来过，称之为有状态会话

### 有状态会话

- 客户端----------服务端

  1. 服务端给客户端一个信件，客户端下次访问服务端带上信件就可以了；cookie

  2. 服务器登记你来过，下次你来的时候我来匹配你；session

### 保存会话的两种技术

- cookie
  - 客户端技术（响应、请求）
- session
  - 服务器技术、利用这个技术，可以保存用户的会话信息。我们可以把信息或数据保存在session中

- 常见场景
  - 网站登陆之后，下次不用再次登陆，第二次访问就直接访问上

### Cookie

1. 从请求中拿到cookie信息
2. 服务器响应给客户端

![Cookie](https://s1.ax1x.com/2020/05/22/YX3k1e.png)

```java
Cookie[] cookies = req.getCookies();//Cookie可能有多个
cookie.getName();//获得cookie的key
cookie.getValue()//获得cookie中的value
new Cookie("lastLoginTime", System.currentTimeMillis()+"");//新建一个cookie
cookie.setMaxAge(24*60*60);//设置cookie的有效值
resp.addCookie(cookie);//响应给客户端一个cookie
```

Cookie 一般会保存在本地的 用户目录下 appdata



一个网站cookie是否存在上限

- 一个Cookie只能保存一个信息
- 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie
- Cookie大小限制为4kb
- 300个cookie（浏览器上限）



删除cookie：

- 不设置有效期，关闭浏览器，自动消失
- 设置有效时间为0



### Session

- 什么是session？
  - 服务器会给每一个用户（浏览器）创建一个Session对象
  - 一个Session独占一个浏览器，只要浏览器没有关闭，这个session就存在
  - 用户登录后，整个网站他都可以登陆（单点登陆）

- Session和Cookie的区别：
  - Cookie是吧用户的数据写给用户的浏览器，浏览器保存（可以保存多个）
  - Session把用户的数据写到用户独占Session中，服务器端保存（保存重要的信息，减少服务器资源的浪费）
  - Session对象由服务器创建

- 使用场景
  - 保存一个登陆用户的信息；
  - 购物车信息
  - 在整个网站经常会使用的数据，我们将它保存在Session中

![YX3UA0.png](https://s1.ax1x.com/2020/05/22/YX3UA0.png)

使用session

 ```java
public class SessionDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码问题
        resp.setCharacterEncoding("UTF-8");
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");
        //得到session
        HttpSession session = req.getSession();
        //给session存东西
        session.setAttribute("person",new Person("羊",22));
        //获取session的id
        String id = session.getId();
        //判断session是不是新创建的
        if (session.isNew()){
            resp.getWriter().write("session创建成功"+id);
        }else{
            resp.getWriter().write("session已经存在"+id);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

 ```

得到session

```java
public class SessionDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码问题
        resp.setCharacterEncoding("UTF-8");
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");
        //得到session
        HttpSession session = req.getSession();
        Person person = (Person) session.getAttribute("person");
        System.out.println(person.toString());
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

手动清除session

```java
public class SessionDemo03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码问题
        resp.setCharacterEncoding("UTF-8");
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");
        //获得session
        HttpSession session = req.getSession();
        session.removeAttribute("person");
        //手动注销session
        session.invalidate();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

设置session自动过期

```xml
		<!--  设置session默认的失效时间  -->
    <session-config>
    <!--  1分钟后session自动失效 以分钟为单位 session多了用户多了容易崩溃 -->
        <session-timeout>1</session-timeout>
    </session-config>
```

![YX8wVI.png](https://s1.ax1x.com/2020/05/22/YX8wVI.png)



---

## JSP

> JSP是servlet的扩展！

### 什么是jsp

- Java Server Pages：Java服务器端页面，也和Servlet一样，用于动态web实现

##### 最大特点

- 写jsp就像在写html～

##### 区别

- HTML静态

- JSP 页面可以嵌入Java代码 为用户提供动态数据

  

### JSP原理

> jsp是怎么执行的

- 代码层面没有问题
- 服务器内部工作
  - tomcat中有一个work目录
  - IDEA使用tomcat会在idea的tomcat中产生一个work目录

**浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet**

- jsp最终也会被转换为servlet



![YXsKUS.png](https://s1.ax1x.com/2020/05/22/YXsKUS.png)

```java
 	public void _jspInit() {
    //初始化
  }

  public void _jspDestroy() {
    //销毁
  }

  public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response) throws java.io.IOException, javax.servlet.ServletException {
  //JSPService  
}
```

1. 判断请求

2. 内置一些对象

   ```java
       final javax.servlet.jsp.PageContext pageContext;//页面上下文
       final javax.servlet.ServletContext application;//applicationContext
       final javax.servlet.ServletConfig config;//config
       javax.servlet.jsp.JspWriter out = null;//out
       final java.lang.Object page = this;//page：代表当前页
       javax.servlet.jsp.JspWriter _jspx_out = null;
       javax.servlet.jsp.PageContext _jspx_page_context = null;
   ```

3. 输出前增加的代码

   ```java
    try {
         response.setContentType("text/html; charset=UTF-8");设置响应页面类型
         pageContext = _jspxFactory.getPageContext(this, request, response,
         			null, false, 8192, true);
         _jspx_page_context = pageContext;
         application = pageContext.getServletContext();
         config = pageContext.getServletConfig();
         out = pageContext.getOut();
         _jspx_out = out;
    }
   ```

4. 以上的这些对象可以在jsp中直接使用，实质上是简化了Servlet的相关操作

![YX63mq.png](https://s1.ax1x.com/2020/05/22/YX63mq.png)

- 在jsp页面中，只要是Java代码就会原封不动的输出，如果是HTML代码，就会被转换为如下

```java
 out.write("<html lang=\"en\">\n");
```

- 这样的格式输出到前端

 

### JSP基础语法

- 任何语言都有自己的语法，Java中有，JSP作为java的一种应用，它拥有一些自己扩充的语法，Java所有语法都支持

#### JSP表达式

 ```jsp
<body>
<%--  jsp 表达式
      作用：用来作为程序的输出，输出到客户端
      <%= 变量或表达式 %>
--%>
    <%= new java.util.Date()%>
</body>
 ```

#### JSP脚本片段

```jsp
<%--  jsp 脚本片段  --%>
  <%
    int sum = 0;
    for (int i = 0; i <=10; i++) {
      sum += i;
    }
    out.println("<h1>sum = "+sum+"</h1>");
  %>
```

```jsp
<%--  在代码嵌入HTML元素 --%>
  <%
    for (int i = 0; i < 5; i++) {
  %>
  <h1>Hello,Wolrd<%=i%>></h1>
  <%
    }
  %>
```

### JSP声明

```jsp
 <%!
    static {
      System.out.println("loading Servlet");
    }

    private int globalVar = 0;

    public void test(){
      System.out.println("进入了test方法");
    }
  %>
```

JSP声明：会被编译到JSP生成的JAVA类中，其他的会被生成到_jspService方法中

```jsp
<% %>

<%! %>

<%= %>

<%--注释--%>
jsp的注释不会在客户端显示，html的注释就会显示
```

### JSP指令

```jsp
<%@page args.... %>   很多参数可以设置
```

```java
<%-- @include 会将两个页面和二为一   --%>
    <%@include file="common/header.jsp"%>
    <h1>网页主体</h1>
    <%@include file="common/footer.jsp"%>

<hr>

<%--  jsp标签  --%>
<%-- jsp:include 拼接页面，本质还是三个 --%>
    <jsp:include page="common/header.jsp"/>
    <h1>网页主体</h1>
    <jsp:include page="common/footer.jsp"/>
```

- 两者区别：include容易出现变量重名报500，jsp:include 可以相互拼接，可以引用

### 九大内置对象

- PageContext 存东西
- Request 存东西
- Response
- Session 存东西
- Application --> ServletContext  存东西
- config --> ServletConfig
- out
- page 几乎不用
- Exception

#### 作用域

```jsp
<%
    pageContext.setAttribute("name1","羊1");//保存的数据只在一个页面中有效
    request.setAttribute("name2","羊2");//保存的数据只在一次请求中有效，请求转发会携带这个数据
    session.setAttribute("name3","羊3");//保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
    application.setAttribute("name4","羊4");//保存的数据在服务器中有效，从打开服务器到关闭服务器
%>
```

#### 使用场景

- request:客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用的！
- session:客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车；Hystrix
- application：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用，比如：公众聊天数据

###  JSP标签、JSTL标签、EL表达式

#### **EL表达式 ${ } 是尖括号！**

- 获取数据
- 执行运算
- 获取web开发的常用对象

#### **JSP标签**

```jsp
<jsp:forward page="jspTag2.jsp">
    <jsp:param name="value1" value="1"/>
    <jsp:param name="value2" value="2"/>
</jsp:forward>
```

#### **JSTL标签**

- JSTL标签库的使用就是为了祢补HTML标签的不足；他自定义许多标签，可以供我们使用，标签的功能和java代码一样



##### 格式化标签

##### SQL标签

##### XML标签

##### 核心标签

![Yjc7nA.png](https://s1.ax1x.com/2020/05/23/Yjc7nA.png)

- JSTL使用步骤
  1. 导入核心标签库
  2. 使用其中的方法
  3. **在Tomcat中也需要导入jstl报，否者会报错，jstl解析错误，500错误**

- c:if

```jsp
<%--
    EL表达式获取表单中的数据
    ${parm.参数名}
--%>
<form action="coreif.jsp">
    <input type="text" name="username" value="${param.username}">
    <input type="submit" value="login">
</form>
<%--
    判断如果提交的用户名是管理员，则登陆成功
--%>
<c:if test="${param.username == 'admin'}" var="isAdmin">
    <c:out value="欢迎您，管理员"/>
</c:if>
<%--自闭合标签--%>
<c:out value="${param.username}"/>
```

- c:when

```jsp
<%--定义一个变量为score，值为85--%>
<c:set var="score" value="85"/>
<c:choose>
    <c:when test="${score>=90}">
        成绩优秀
    </c:when>
    <c:when test="${score>=85}">
        成绩良好
    </c:when>
    <c:when test="${score>=60}">
        成绩一般
    </c:when>
    <c:when test="${score<=60}">
        不及格
    </c:when>
</c:choose>
```

- c:foreach

```jsp
<%
    ArrayList<String> people = new ArrayList<>();
    people.add(0,"张3");
    people.add(1,"张4");
    people.add(2,"张5");
    people.add(3,"张6");
    people.add(4,"张7");
    people.add(5,"张8");
    request.setAttribute("list",people);
%>
<%--
var,每一次遍历出来的变量
items，要遍历的对象
begin, 哪里开始
end, 到哪里
step, 每次步数
--%>
<c:forEach var="people" items="${list}">
    <c:out value="${people}"/> <br>
</c:forEach>

<hr>

<c:forEach var="people" items="${list}" begin="1" end="3" step="2">
    <c:out value="${people}"/> <br>
</c:forEach>
```



---

## JavaBean

实体类

JavaBean有特定的写法：

- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法

一般用来和数据库字段做映射 ORM；

ORM：对象关系映射

- 表-->类
- 字段-->属性
- 行记录-->对象

**People表**

| id   | name | age  | address     |
| ---- | ---- | ---- | ----------- |
| 1    | 111  | 44   | 广东，china |
| 2    | 222  | 23   | 深圳，广东  |
| 3    | 333  | 12   | 广州        |

- 过滤器
- 文件上传
- 邮件发送
- JDBC：如何使用、CRUD、事务



---

## MVC三层架构

> MVC:Model、View、Controller 模型、视图、控制器

### 老架构

![Yv8daR.png](https://s1.ax1x.com/2020/05/23/Yv8daR.png)

- 用户直接访问控制层，控制层就可以直接操作数据库

```java
servlet--CRUD-->数据库
弊端：程序十分臃肿，不利于维护
servlet的代码中：处理请求、响应、视图跳转、处理JDBC、处理业务代码、处理逻辑代码
  
架构：没有什么事加一层解决不了的的
 	程序员调用---jdbc---mysql、oracle、sqlServer...公共使用，这就是加了一层的效果
```



### MVC三层架构

![YvJruD.png](https://s1.ax1x.com/2020/05/23/YvJruD.png)

- Model

  - 业务处理：业务逻辑（Service）
  - 数据持久层：CRUD（Dao）

- View

  - 展示数据
  - 提供链接发起Servlet请求

- Controller（Servlet）

  - 接收用户的请求：（req：请求参数、Session信息等）

  - 交给业务层处理对应的代码

  - 控制视图跳转

    ```java
    登陆---接收用户的登陆请求---处理用户的请求（获取用户登陆参数，usernmae，passowrd）---- 交给业务层处理登陆业务（判断用户名密码是否正确：事务回滚）--- Dao层查询用户密码是否正确 ---- 数据库
    ```



---

## 过滤器 Filter*

> Filter：过滤器，用来过滤网站的数据
>
> **Shiro**
>
> - 处理中文乱码
> - 登陆验证

![Yvt1w4.png](https://s1.ax1x.com/2020/05/23/Yvt1w4.png)

- Filter开发步骤
  1. 导入依赖
  2. 重写接口不要错  javax.servlet.Filter

- 实现Filter接口，重写对应方法

```java
public class EncodingFilter implements Filter {
    //初始化：web服务器启动，就已经初始化了，随时等待过滤器对象出现
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("过滤器初始化开始了");
    }

    /*
    1. 过滤中的所有代码，在过滤特定请求的时候都会执行
    2. 必须要让过滤器继续同行
     */
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=utf-8");

        chain.doFilter(request,response);//作用：让我们的请罪继续走，如果不写，程序到这里就会被拦截停止
    }

    //服务器关闭，自动销毁过滤器
    public void destroy() {
        System.out.println("过滤器自己开始销毁了～");
    }
}
```

- 在web.xml中配置Filter

```xml
 		<filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>xyz.kidjaya.filter.EncodingFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/test</url-pattern>
    </filter-mapping>
```



---

## 监听器 Listener

- 实现一个监听器的接口；（有N种）

```java
//统计网站在线人数：统计session
public class OnlineCountListener implements HttpSessionListener {
    
    //创建session监听  看你的一举一动
    //一旦创建Session就会触发一次这个事件
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext servletContext = se.getSession().getServletContext();
        Integer onlineCount = (Integer) servletContext.getAttribute("onlineCount");
        System.out.println(se.getSession().getId());
        if (onlineCount == null){
            onlineCount = 1;
        }else{
            int count = onlineCount;
            onlineCount = count + 1;
        }

        servletContext.setAttribute("onlineCount",onlineCount);
    }

    //销毁session监听
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext servletContext = se.getSession().getServletContext();
        Integer onlineCount = (Integer) servletContext.getAttribute("onlineCount");

        if (onlineCount == null){
            onlineCount = 0;
        }else{
            int count = onlineCount;
            onlineCount = count - 1;
        }

        servletContext.setAttribute("onlineCount",onlineCount);
    }
}
```

- 在web.xml中组册监听器

```java
 <listener>
        <listener-class>xyz.kidjaya.listener.OnlineCountListener</listener-class>
 </listener>
```

- 看情况是否使用～

---

## 常见应用

**监听器：GUI编程中使用较多**

```java
public class TestPANEL {
    public static void main(String[] args) {
        Frame frame = new Frame("情人节快乐");//新建一个窗体
        Panel panel = new Panel(null);//面板
        frame.setLayout(null);//设置窗体布局

        frame.setBounds(10,10,50,50);
        frame.setBackground(new Color(0,255,255));

        panel.setBounds(5,5,30,30);
        panel.setBackground(new Color(0,123,123));

        frame.add(panel);
        frame.setVisible(true);

        //监听关闭事件
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                super.windowClosing(e);
                //0 正常退出 1 非法退出
                System.exit(0);
            }
        });
    }
}
```



**实现：用户登录后才能进入主页，用户注销之后就不能进入主页了～**

1.  用户登陆后，向session中放入用户的数据
2. 进入主页的时候要判断用户是否已经登陆；在过滤器中实现

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        //ServletRequest HttpServletRequest
        HttpServletRequest req = (HttpServletRequest)request;
        HttpServletResponse resp = (HttpServletResponse)response;

        if (req.getSession().getAttribute("USER_SESSION")==null){
            resp.sendRedirect("/login.jsp");
        }
        chain.doFilter(req,resp);
    }
```



---

## JDBC

> 抽象一层，解决多种数据库连接操作统一问题

![YxywUP.png](https://s1.ax1x.com/2020/05/24/YxywUP.png)

- 需要jar包的支持：
  - java.sql
  - javax.sql
  - Mysql-connecter-java-...（连接驱动）必须导入

- 实验环境搭建

  1. 数据库建表，插入数据
  2. 导入数据库依赖

  ```xml
  				<dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>5.1.47</version>
          </dependency>
  ```

  3. IDEA中连接数据库

- JDBC固定步骤

  1. 加载驱动  
  2. 连接数据库，代表数据库
  3. 向数据库发送SQL对象Statement，：CRUD
  4. 编写SQL（根据业务，不同的SQL）
  5. 执行SQL
  6. 关闭连接

  ```java
  public static void main(String[] args) throws SQLException, ClassNotFoundException {
          String url = "jdbc:mysql://localhost:3306/test?useSSL=false&characterEncoding=UTF-8";
          String username = "root";
          String password = "aaa791654109";
          //1 加载驱动
          Class.forName("com.mysql.jdbc.Driver");
          /*
          DriverManager.registerDriver(new com.mysql.jdbc.Driver());
          System.setProperty("jdbc.drivers", "com.mysql.jdbc.Driver");
          */
          //2 连接数据库
          Connection connection = DriverManager.getConnection(url,username,password);
          //3 向数据库发送SQL的对象Statement，PrepareStatement ：CRUD
          Statement statement = connection.createStatement();
          //4 编写SQL
          String sql = "select * from people";
          //5 执行查询SQL，返回一个ResultSet ：结果集
          ResultSet rs = statement.executeQuery(sql);
  
          while (rs.next()){
              System.out.println("id="+rs.getObject("id"));
              System.out.println("age="+rs.getObject("age"));
              System.out.println("name="+rs.getObject("name"));
              System.out.println("address="+rs.getObject("address"));
  
          }
          //6 关闭连接，释放资源，先开后关
          rs.close();
          statement.close();
          connection.close();
      }
  ```

  - 预编译sql，PrepareStatment
  
  ```java
   public static void main(String[] args) throws ClassNotFoundException, SQLException {
          String url = "jdbc:mysql://localhost:3306/test?useSSL=false&characterEncoding=UTF-8";
          String username = "root";
          String password = "aaa791654109";
          //1 加载驱动
          Class.forName("com.mysql.jdbc.Driver");
          /*
          DriverManager.registerDriver(new com.mysql.jdbc.Driver());
          System.setProperty("jdbc.drivers", "com.mysql.jdbc.Driver");
          */
          //2 连接数据库
          Connection connection = DriverManager.getConnection(url,username,password);
          //3 编写SQL
          String sql = "INSERT into people values (?,?,?,?)";
  
          //4 向数据库发送SQL的对象Statement，PrepareStatement ：CRUD
          PreparedStatement statement = connection.prepareStatement(sql);
          statement.setInt(1,3);
          statement.setInt(2,30);
          statement.setString(3,"xxxx");
          statement.setString(4,"asdasd");
  
          //5 执行查询SQL，返回一个ResultSet ：结果集
          /*
              增删改都是用executeUpdate，返回受影响的行数
           */
   
          int result = statement.executeUpdate();
          if (result>0){
              System.out.printf("插入成功～");
          }
          
          //6 关闭连接，释放资源，先开后关
          statement.close();
          connection.close();
      }
  ```

- 事务(要么都成功，要么都失败)

```java
public static void main(String[] args) throws ClassNotFoundException, SQLException {
        Connection connection = null;
        Statement statement = null;
        try {
            String url = "jdbc:mysql://localhost:3306/test?useSSL=false&characterEncoding=UTF-8";
            String username = "root";
            String password = "aaa791654109";
            //1 加载驱动
            Class.forName("com.mysql.jdbc.Driver");
        /*
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
        System.setProperty("jdbc.drivers", "com.mysql.jdbc.Driver");
        */
            //2 连接数据库
            connection = DriverManager.getConnection(url,username,password);
            connection.setAutoCommit(false);
            //3 编写SQL
            String sql = "UPDATE people set age = age+100 where id = 1";

            //4 向数据库发送SQL的对象Statement，PrepareStatement ：CRUD
            statement = connection.createStatement();
            //5 执行查询SQL，返回一个ResultSet ：结果集
        /*
            增删改都是用executeUpdate，返回受影响的行数
         */
            statement.executeUpdate(sql);
            int i = 1/0;
            connection.commit();
        }catch (Exception e){
            connection.rollback();
            System.out.println("Exception~");
        }

        //6 关闭连接，释放资源，先开后关
        statement.close();
        connection.close();
    }
```



- ACID原则：保证数据安全

```java
//1.开启事务
//2.事务提交 commit()
//3.事务回滚 rollback()
//4.关闭事务

转帐
A:1000
B:1000
A给B转钱服务器崩了怎么办～ 使用事务回滚
```

- junit单元测试

  - 依赖

  ```java
  				<dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.13</version>
          </dependency> 
  ```

  - 简单使用

    - @Test注解只在方法上有效，只要加了这个注解方法，就可以直接运行

    ```java
        @Test
        public void test(){// junit单元测试
            System.out.println("hello");
        }
    ```

    ![YzUNZT.png](https://s1.ax1x.com/2020/05/24/YzUNZT.png)

    - 失败的时候是红色的

    ![YzUBW9.png](https://s1.ax1x.com/2020/05/24/YzUBW9.png)
