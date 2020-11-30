# Java入门

## java的数据类型

> java是强类型语言 安全性高 要求变量定义后才能使用

- 基本类型

  1. 数字类型 

     1. 整数类型 byte1 short2 int4 long8
     2. 浮点类型 float4 double8
     3. 字符类型 char2

  2. boolean类型

     数字进制:二进制=>0b 八进制=>0 十六进制=>0x 开头

     拓展：银行业务怎么表示？钱

     使用BigDecimal 数学工具类 最好避免使用浮点数进行比较（有限，离散，舍入误差，大约，接近但不等于）

      

     字符拓展

     所有字符本质还是数字，char类型可以强制转换为int 

     编码Unicode表：97 = a  65 = A 

  ​      

  ​       字符转义

  ​		\t  制表符

  ​        \n  换行

  ​        .......

- 引用数据类型
  1. 类
  2. 接口
  3. 数组

### 类型转换

- 强制转换 （类型）变量名  高->低
- 自动转换   低->高

注意点1：

1. 不能对布尔值进行转换
2. 不能把对象类型转换为不相干类型
3. 在高容量转换到低容量的时候，强制转换
4. 转换的时候可能存在内存溢出，或者精度问题

注意点2:

- 操作数比较大的时候，注意溢出问题
- JDK7新特性，数字之间可以用下划线分割 10_0000_0000 == 10000000000
- 数值运算时转换为long类型
- L 和  l 都可以表示long 但是建议使用L 因为l容易看成数字

---

## 变量

###  作用域

- 类变量 static int var = 0
- 实例变量 String str = “hello world”
- 局部变量 public void method(){ int i = 0 }

### 常量

- 特殊的变量，初始化后不能再改变值
- 需要使用关键字 final，final时修饰符，可以变动位置 static final  type var == final static type var
- 常量名一般使用大写字符

### 变量命名规范

- 所有变量、方法、类名：见名知意
- 类成员变量：首字母小写，后面单词大写 userName
- 局部变量：首字母小写，后面单词大写 userName
- 常量：大写字母和下划线  MAX_SALARY
- 类名：首字母大写和驼峰原则  DataBase
- 方法名：首字母小写和驼峰原则   addCookie()

---

## 运算符

- 算数运算符 + - * / % ++ -- (与long/double/...类型进行的运算，计算结果变为long/double/...类型，否则为int类型)
- 赋值运算符 =
- 关系运算符 > < >= <= == != instanceOf
- 逻辑运算符 && || !
- 位运算符 & | ^ ~ >> << >>>
- 条件运算符 ? :
- 扩展运算符 ++  -=  *=  /=

**```注意要点 ```**

```b = ++a 和 b = a++区别 (同理--)```

``` 前者:执行完代码后，先给b赋值，再自增``` 

```后者：执行完这行代码前，先自增，再给b赋值```

**```位运算符```**

``` &:与 二进制都为1则为1```

```|:或 二进制有一个为1则为1```

```^:异或 二进制不同则为1```

```～:取反 1和0互换```

**```扩展运算符&&三元运算符```**

```a+=b // a = a+b```

```a-=b // a = a-b```

***```面试扩展```***

```字符串连接符```

``` syso(""+a+b) 结果为连接的字符串```

``` syso(a+b+"") 结果为a+b的值 (原因：先进行a+b运算后转为字符串)```

---

## 包机制

> 为了更好的组织类，用于区别类名的命名空间
>
> 一般用公司域名倒置作为包名 com.baidu.www
>
> 为了能够使用某一包的成员，使用import语句可完成此功能
>
> - import package1[.package2...].(classname|*)

```package pkg1[.pkg2[.pkg3]]```

---

## JavaDoc生成文档

1. 命令行生成

   javadoc -encoding utf-8 -charset utf-8 （后面的参数避免中文乱码）

2. idea工具自动生成

   Tools->Generate JavaDoc->设置参数 && 生成目录 && 需要生成Doc文档的类和参数->OK

---

## Scanner对象

> java5新特性，可以通过Scanner类来获取用户的输入

```java
//创建一个扫描器对象，用于接收键盘数据
Scanner scanner = new Scanner(System.in);
//判断用户有没有输入字符串
if(scanner.hasNext()){
  //使用next方式接收
  String str = scanner.next();
  System.out.println(str);
}
//凡是属于IO流的类如果不关闭会一直占用资源，要养成好习惯用完就关掉
scanner.close();
//next()不能得到带有空格的字符串
//nextLine()以enter为结束符也就是获取回车前的所有字符
//hasNextInt()下一个值是否为整型
```

----

## Java语句结构

- 顺序结构

- 选择结构 if/switch

  > switch支持的变量类型可以是 char byte short int
  >
  > 从Java SE7 开始  支持String 类型
  >
  > 同时case标签必须为字符串常量或字面量  
  >
  > 
  >
  > 扩展：反编译 -> 将java生成的class文件字节码文件，反编译为看得懂的源代码
  >
  > 使用相应反编译工具，或者把生成的class文件拖入idea的java文件夹下，自动进行反编译

- 循环结构 while/do..while/for 

  - java5加入增强for循环(foreach遍历)      for(Type item : args ) 没有下标

* 关键字continue，break，goto

  

## 方法

> 概念：语句的集合；包含于类或对象中；在程序中被创建，在其他地方被引用
>
> 原则：保持功能模块原子性，一个方法只能完成一个功能，有利于后期扩展

```java
/*
*		修饰符 返回值类型 方法名（参数类型 参数名）{ //新式参数 调用则为实际参数
*		方法体...
*		return 返回值;
*		}
*/ 
```

### 方法调用

- 调用方法：对象名.方法名（参数列表）
  - 静态方法 -> 类名.方法名 调用，在类编译时一起生成
  - 非静态方法 -> 实例化 new 对象，通过对象.方法名 调用，在new对象之后生成
- 返回值:当做是一个值    /   void：方法调用一定是一条语句 如System.out.println("hello world"); 

> 拓展：值传递，引用传递
>
> ​	值传递：传递值，不改变函数外的值
>
> ​	引用传递：传递地址，同时改变

### 方法重载

- 重载就是在一个类中，有相同的函数名称，但形参不同的函数
- 方法重栽的规则：
  - 方法名必须相同
  - 参数列表必须不同（个数不同/类型不同/参数排列顺序不同）
  - 方法返回类型可以相同也可以不相同，返回值类型不同不足以成为方法重栽

- 原理：系统匹配，匹配失败，编译器报错

### 命令行传参

>有时候希望运行一个程序时候再给他传递消息，这要靠传递命令行参数给main()函数实现

- 命令行运行时，当有packeage的时候，需要回退到包的上一级文件夹输入完整包名才能运行java程序

### 可变参数

- JDK1.5开始，支持传递同类型的可变参数

- 在制定参数类型后面加一个省略号 int... var

- 一个方法中只能指定一个可变参数，它必须是方法最后一个参数，任何普通参数必须在它之前声明

  ```java
  public void test(int a,int... number){
  			System.out.print(number.length);
  }
  ```

### 递归

- 减少代码量，解决复杂问题  
- 递归结构：递归头(结束条件)，递归体(需要调用的时机)
  - 边界条件：边界
  - 前阶段：调用自身
  - 返回阶段：n*(n-1)
- 牵一发而动全身

>Java使用栈机制，把方法压入，会造成大量程序调用，当深度比较深的时候，需要大量的时间开销，空间内存

- 适合规模比较小的算法问题，规模比较大使用其他非递归算法比较好

---

## 数组

 ``` 相同类型数据的有序集合，每一个数据称作数组元素，可以通过下标来访问它们```

### 声明创建

```java
public class demo{
  public static void main(String[] args){
    int[] nums;//定义，推荐，声明一个数组
    // int nums[];//适配C/C++
    nums = new int[10];//分配空间，创建一个数组
    //给元素赋值
    nums[0] = 1;
    nums[1] = 2;
    ...
    nums[9] = 10;
    //nums.length 获取数组长度
  }
}
```

### 内存分析

- 堆：存放new的对象和数组；可以被所有线程共享，不会存放别的对象引用

- 栈：存放基本变量类型（包含具体数值）；引用对象的变量（存放引用在堆里的具体地址）

- 方法区：可以被所有线程共享；包含了所有的class和static变量

- [![YJS82t.png](https://s1.ax1x.com/2020/05/11/YJS82t.png)](https://imgchr.com/i/YJS82t)

  

### 三种初始化

1. 静态初始化：创建加赋值

   ``` int[] a = {1,2,3}```

2. 动态初始化：包含默认初始化

   ``` int[] a = new int[10]; ```

   ```a[0] = 10;``` //syso输出10

   ```syso(a[1]); //syso输出默认值0```

   

### 数组特点

- 长度确定，不能改变
- 同类型，不允许混合类型
- 可以是任何数据类型
- 数组变量属于引用类型，可以看成对象，每个元素相当于成员变量
- 数组对象在堆中

### 数组边界

- index从0开始 也就是 数组.length - 1

### 多维数组

- 数组的数组

```java
public static void demo(String[] args){
 	int[][] test = {{2,2,2,2},{1,1,1,1},{2,2,2,2},{3,3,3,3}};
  //获取数组长度
  test.length;
  //获取里层数组长度
  test[0].length;
}
```

### Arrays类

- 常用
  - Arrays.toString( array ) 作为字符串输出
  - Arrays.sort( array ) 排序
  - Arrays.fill( array , value ) 填充数字 fill( array,start,end ,calue ) 在范围内填充数字

### 稀疏数组

> 当一个数组大部分元素为0，或者为同一值的时候，可以使用稀疏数组来保存该数组
>
> 稀疏数组的处理方式是：
>
> - 记录数组一共有几行几列，有多少不同的值
> - 把具体不同值的元素和行列记录在一个小规模的数组中，从而缩小程序的规模

---

## 面向对象

> 面向对象和面向过程不可分割，面向对象是架构，而面向过程是具体执行流程
>
> 面向对象编程：OOP，Object-Oriented-Programming
>
> 本质：以类的方式组织代码，以对象来组织封装数据
>
> 抽象：把概念分类出来
>
> 三大特性：
>
> - 封装
> - 继承
> - 多态
>
> 对象：具体的事物，类，是抽象的 (类是一个模版，对象是一个具体的实例)

### 构造器

#### 形式

- 一个类如果什么都不写，他也会存在一个方法，默认的隐式构造器
- 必须和类的名字相同的public方法 
- 没有返回方法

### 作用

- 使用new关键字，本质就是在调用构造器
- 用来初始化值

#### 注意点

- 一旦定义有参构造，无参构造就必须显示定义  
- IntelliJ 使用Command + N 快捷生成构造器 for MAC；Alt+Insert for Windows

### 封装

> 该露的露，追求高内聚，低耦合。
>
> - 高内聚：内的内部数据操作细节自己完成
> - 低耦合：仅暴露少量的方法给外部使用
> - 通常应禁止直接访问一个对象中数据的实际表示，而应通过操作接口来访问，这称为消息隐藏
>   - 属性私有 ，get/set

1. 提高程序的安全性，保护数据
   - 用户输入非法数据 可以在FIeld里面对传入的数据进行判断

2. 隐藏代码的实现细节
3. 统一接口
4. 系统的可维护性增加了

### 继承

> 本质：对某一批类的抽象
>
> extends ：子类是父类的扩展
>
> Java只有单继承，直接或间接继承Object类
>
> 继承是类和类之间的一种关系。除此之外，还好依赖，组合，聚合等
>
> 子类和父类之间，从意义上讲具有 “is a”的关系

- **super**

  - 注意点

    1. super调用父类的构造方法，必须在构造方法的第一个

    2. super必须只能出现在子类的方法或构造方法中

    3. super和this不能同时调用构造方法

       - 代表的对象不同
         -  this：本身调用者这个对象
         - super：代表父类对象的应用 

       - 前提
         - this：没有继承可以使用
         - super：只能在继承条件才可以使用 

       - 构造方法
         - this( ) ：本类的构造
         - super( ) ：父类的构造

- **方法重写**

  > 重写，子类的方法和父类必须一直：不同的是方法体
  >
  > 为什么需要重写：
  >
  > - 父类的功能，子类不一定需要，或者不一定满足

  1. 方法名必须相同
  2. 参数列表必须相同
  3. 修饰符：范围可以扩大  public>protected>default>private
  4. 抛出的异常：可以被缩小，但不能扩大。ClassNotFoundException(小) --> Exception(大)

### 多态

> 同一方法根据发送对象的不同而采用多种不同的行为方式
>
> 一个对象的实际类型是确定的，但可以指向的引用类型有很多

- 注意事项
  1. 多态是方法的多态，属性没有多态
  2. 父类和子类，有联系    否者类型转换异常！ClassCastException
  3. 存在条件：继承关系，方法需要重写，父类引用指向子类对象 Father f = new Son() 【子类重写了父类的方法，执行子类的方法】上转型
  4. 不能重写的方法
     1. static 方法，属于类，他不属于实例
     2. final 是常量
     3. private修饰的方法

#### instanceof和类型转换

```java
/**
*instanceof
*/
public static void main(String[] args){
  //Object->String
  //Object->Person->Teacher
  //Object->Person->Student
  Object object = new Student();
  
  //System.out,println(x instanceof Y);//能不能使编译通过
  
  System.out.println(object instanceof Student);//true
  System.out.println(object instanceof Person);//true
  System.out.println(object instanceof Object);//true
  System.out.println(object instanceof Teacher);//false
  System.out.println(object instanceof String);//false
	
  Person person = new Student();
  System.out.println(person instanceof Student);//true
  System.out.println(person instanceof Person);//true
  System.out.println(person instanceof Object);//true
  System.out.println(person instanceof Teacher);//false
  System.out.println(person instanceof String);//编译报错
  
  Student student = new Student();
  System.out.println(person instanceof Student);//true
  System.out.println(person instanceof Person);//true
  System.out.println(person instanceof Object);//true
  System.out.println(person instanceof Teacher);//编译报错
  System.out.println(person instanceof String);//编译报错
}

```

```java
/**
*类型转换
**/
public static void main(String[] args){
  //类型之间的转换。父子
  //高                    低
  Person obj = new Student();
  
  //student将这个对象转为Student类型，就可以使用Student类的方法
  Student student = (Student)obj;//高转低强制转换
  student.go();
  ---------------------------------
  //子类转换为父类，可能会丢失自己的一些方法
  Student student = new Student();
  student.go();
  Person person = student;//低转高自动转化
}
/*
1.父类的引用指向子类对象
2.把子类转换成父类，向上转型
3.把父类转换为子类，向下转型，强制转换-->方便方法的调用,不用重新new一个，减少重复的代码
*/
```

#### static关键字

```java
//静态倒入包
import static java.lang.Math.random;
import static java.lang.Math.PI;

//final定义类就不能被继承 相当于断子绝孙
public class Student {
	//顺序：2  赋初始值
  {
    //代码块（匿名代码块）
  }
  
  //顺序：1 只执行一次
  static{
    //静态代码块
  }
  
  //顺序：3
  public Student(){
    
  }
  
  private static int age; //静态的变量 多线程
  private double score; //非静态的变量
  public static void go(){
    //与类一起加载
  }
  
  public static void main(String[] args){
		//静态倒入可以直接使用
    System.out.println(random());
    System.out,println(PI);
  }
}
```

#### 抽象类

```java
//abstract 抽象类  类extends 单继承  接口：实现多继承
public abstract class Action{
  //约束～有人帮我们实现
  //abstract,抽象方法，只有方法的名字，没有方法的实现
  public abstract void doSomething();
  /**
  * 1.不能new这个抽象类，只能靠子类去实现它：约束
  * 2.抽象类中可以写普通方法，但抽象方法必须在抽象类中
  *抽象的抽象：约束～
  *目的：提高开发效率
  **/
}
```

#### 接口

> 接口：只有规范，自己无法写方法。约束和实现分离：面向接口编程
>
> 接口就是规范，定义的是一组规则，体现了现实世界中，“如果你是...，你必须能...”的思想。

 ```java
//interface 定义的关键字，接口都需要有实现类
public interface UserService{
  //接口中的所有定义的方法其实都是抽象的 默认 public abstract
  //接口中只能定义常量，默认前缀 public static final
  int age = 999;
  void add(String name);
  void delete(String name);
  void update(String name);
  void query(String name);
}
 ```

```java
//实现类
//类可以实现接口 implements 关键字定义相关的接口
//实现了接口的类，就必须重写接口中的方法，可以实现多个接口，侧面相当于多继承
public class UserServiceImpl implements UserService{
  @Override
  public void add(String name){
    
  }
  @Override
  public void delete(String name){
    
  }
  @Override
  public void udpate(String name){
    
  }
  @Override
  public void query(String name){
    
  }
}
```

- 作用
  1. 约束
  2. 定义的一些方法，让不同的人实现。   多对一
  3. 方法都是 public abstract
  4. 变量都是常量，public static final
  5. 接口不能被实例化 就是new，接口中没有构造方法
  6. implements可以实现多个接口，侧面实现C++中的多继承
  7. 必须要重写接口中的方法

#### 内部类（补充）

> 内部类就是在一个类的内部定义一个类，比如，在A类中定义一个B类，那么B类相对于A类来说就称为内部类，而A类相对于B类就是外部类
>
> 1. 成员内部类
> 2. 静态内部类
> 3. 局部内部类
> 4. 匿名内部类
> 5. lambda表达式
>
> 

```java
public class Outer{
  private int id = 10;
  public void out(){
    System.out.println("Outer");
  }
  
  public class Inner{//如果static访问不到非static变量，编译顺序问题
    public void in(){
      System.out,println("Inside");
      //局部内部类
      class PartInner{
        public partMethod)(){
          //todo
        }
      }
    }
    //获得外部类的私有属性
    //可以实现高效解耦
    public void getId(){
      System.out.println(id);
    }
  }
}

//一个java类中可以有多个class类，但只能有一个public class类
class Other{
  public static void main(String[] args){
    //可以编写测试类
  }
}
```

```java
public class Applicaction{
  public static void main(String[] args){
    Outer outer = new Outer();
    //通过这个外部类来实例化内部类
    Outer.Inner inner = outer.new Inner();
    inner.getId();
   	//没有名字初始化类，不用将实例保存到变量中
    new Apple().eat();
    //定义使用场景不重复的接口类实现
    UserService userService = new UserService{
      @Override
      public void hello(){
        
      }
    }
  }
}

class Apple{
  public void eat(){
    //todo
  }
}

interface UserService{
  void hello();
}
```

---

## 异常机制

### 简单分类

- 检查性异常：用户输入错误
- 运行时异常：编译看不到，运行时出错
- 错误ERROR：逻辑问题等，栈溢出，编译时检查不到

### 异常体系结构

- Java通常把异常当做对象来处理，并定义一个类java.lang.Throwable作为所有异常超类
- Java API中定义了许多异常类，这些异常类分为两大类，错误Error和Exception
  - Error：由Java虚拟机生成抛出，大多数于代码无关，比如栈溢出错误(OutOfMemoryError)，这些异常发生时，程序会停止，尽量避免。
    - 类定义错误(NoClassDefFoundError)，链接错误(LinkageError)
    - 这些错误是不可查的而且是运行时不允许出现的状况，尽量把系统调到最优（否则被炒鱿鱼）
  - Exception：异常
    - RuntimeException是一个重要的子类(运行时异常)
      - ArrayIndexOutOfBoundsException(数组下标越界)
      - NullPointerException(空指针异常)
      - ArithmeticException(算术异常)
      - MissingResourceException(丢失异常)
      - ClassNotFound(找不到类)
      - 这些异常是不检查异常，程序中可以选这捕获处理，也可以不处理
      - 这些异常通常都是由程序逻辑错误引起的，程序应该从逻辑角度避免这种错误的发生

- 区别：Error通常是灾难性的致命错误，是程序无法控制和处理的，当出现这种异常，JVM会选择终止程序。

  ​			Exception通常情况下是可以被程序处理的，并且在程序中应该尽可能去处理这些异常

### 异常处理机制

- 关键字：try、catch、finally、throw、throws

```java
int a = 1;
int b = 0;
try{
  //主动判断抛出异常使用 throw
  if(b == 0){
    throw new ArithmeticException();//自动抛出
  }
  System.out.println(a/b);
}catch(ArithmeticException e){//catch 捕获异常
  System.out.println("出现异常")
}catch(Exception e){
  //捕获多个异常，范围要从小到大
  e.printStackTrace();//打印错误的栈信息
}finally{//处理善后工作
  //可以不写，用来处理IO流，和资源等，用完默认关闭
}

//Windos快捷：ctrl+alt+t   Mac快捷：option+command+T -》自动生成代码块
```

```java
//假设方法中处理不了这个异常，就在方法上抛出异常，使用throws而不是throw
public void test(int a, int b) throws ArithmeticException{
  if(b == 0){
    throw new ArithmeticException();//主动抛出异常，一般在方法中使用
  }
}
```

### 自定义异常

> 用户自定义异常类，只需要继承Exception即可
>
> 1. 创建自定义异常类
> 2. 在方法中通过throw关键字抛出异常对象
> 3. 如果在抛出异常的方法中处理异常，可以使用try catch语句捕获并处理；否则在方法的声明处通过throws关键字抛出给方法调用者的异常（异常自己处理不了，抛出给调用者），然后进行下一步操作
> 4. 在出现异常方法的调用者中捕获并处理异常

```java
//自定义异常类
public class MyException extends Exception {
    //传递数字
    private int detail;
    public MyException(int a){
        this.detail = a;
    }
    //toString:异常大打印信息
    @Override
    public String toString() {
        return "MyException{"+detail+"}";
    }
}

```

```java
public class Test {
    public static void test(int a) throws MyException{
        System.out.println("传递的参数为"+a);
        if (a > 0){
            throw new MyException(a);//抛出
        }
        System.out.println("Ok");
    }

    public static void main(String[] args) {
        try {
            test(1);
        } catch (MyException e) {
            System.out.println("Exception:"+e);
        }
    }
}
```

- 实际经验
  1. 处理运行时异常时，采用逻辑去合理规避同时辅助try-catch处理
  2. 在多重catch块后面，可以加一个catch(Exception e) 来处理可能被遗漏掉的异常
  3. 对于不稳定定的代码，也可以加上try-catch，处理潜在的异常
  4. 尽量去处理异常，切忌知识简单地调用printStackTrace()去打印输出
  5. 具体如何去处理异常，要根据不同的业务需求和异常类型去决定
  6. 尽量添加finnlly语句快去释放占用的资源，比如IO资源等