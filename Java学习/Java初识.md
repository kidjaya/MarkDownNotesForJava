# Java初识

## Java特性和优势

1. 简单性 *C++纯净版*
2. 面向对象 *万物皆对象*
3. 可以移植性 *跨平台*
4. 高性能 
5. 分布性 *网络分布式环境，处理TCP/IP，支持远程调用*
6. 动态性 *反射*
7. 多线程 *提高CPU使用效率*
8. 安全性 *防病毒防篡改*
9. 健壮性

## java三大版本

> Write Once,Run Anywhere

1. javaSE：标准版  *桌面程序，控制台开发*
2. javaME:  嵌入式开发 *手机，小家电*
3. javaEE： E企业级开发 *web端，服务器开发*

##  JDK/JRE/JVM

- JDK:  *Java Development Kit*

  - JDK包含JRE && JVM
- JRE:  *Java Runtime Enviroment*

  - JRE包含JVM
- JVM:  *Java Virtual Machine*

  - Java多平台运行的根据，处理java源代码，编译成目标代码


## Java程序运行机制

- 编译型
  - compile将java源代码编译成计算机可执行程序
  - C++/C  编译型
  - 制作操作系统
- 解释型
  - 对速度要求不高
  - Java
  - 制作网页

> 流程：源文文件(.java) = java-c => 字节码(.class) ==> 类转载器 ==> 字节码校验器 ==> 解释器 ==> 操作系统平台