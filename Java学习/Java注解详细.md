# Java注解详细

> 参考博客：https://www.jianshu.com/p/9471d6bcf4cf

> Java 注解用于为 Java 代码提供元数据。作为元数据，注解不直接影响你的代码执行，但也有一些类型的注解实际上可以用于这一目的。Java 注解是从 **Java5** 开始添加到 Java 的
>
> Annotation可以从源文件、class文件或者在运行时通过反射机制多种方式被读取。

## 一、注解的定义

- 注解和class、interface一样，也是一种类型，他是使用修饰符@interface来表示的

### 注解类的写法

- 新建

```java
public @interface TestAnnotation {
    
}
```

- 使用

```java
@TestAnnotation()
private void testAnnotation(){
  System.out.println("testAnnotation");
}
```

- 在注解中没有任何代码，所以这个注解没有什么意义

---

## 二、元注解

- 元注解，是注解的注解，给注解增加一些意义，用于实现我们想要实现注解的功能
- 包括@Retention @Target @Document @Inherited @Repeatable 五种

### @Retention

- retention有保留、保持的意思，表示注解存在的阶段，使用**枚举RententionPolicy**类来表示注解保留时期
- @Retention(RetentionPolicy.SOURCE),源码期间
- @Retention(RetentionPolicy.CLASS),字节码期间
- @Retention(RetentionPolicy.RUNTIME),字节码期间，运行期间，可以通过反射获取到
- 自定义注解，得通过反射获取才有其作用，在运行期间可以获得，所以使用的一般都是**@Retention(RetentionPolicy.RUNTIME)**

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation {
}
```

### @Target

- target目标的意思，表示注解要作用的scope，也就是作用域，使用**枚举ElementType表达作用域**
- @Target(ElementType.TYPE)  作用**接口、类、枚举、注解**
- @Target(ElementType.FIELD) 作用**属性字段、枚举的常量**
- @Target(ElementType.METHOD) 作用**方法**
- @Target(ElementType.PARAMETER)  作用**方法参数**
- @Target(ElementType.CONSTRUCTOR)  作用**构造函数**
- @Target(ElementType.LOCAL_VARIABLE) 作用**局部变量**
- @Target(ElementType.ANNOTATION_TYPE) 作用**注解**（@Retention注解中就使用该属性）
- @Target(ElementType.PACKAGE) 作用于**包**
- @Target(ElementType.TYPE_PARAMETER)  作用于**类型泛型 (即泛型方法、泛型类、泛型接口)**
- @Target(ElementType.TYPE_USE) 类型使用.可以用于标注**任意类型除了 class**

### @Documented

- document意思是文档，作用：将注解包含到javadoc中

### @Inherited

- inherited意思是继承，和平时继承类extends差不多，子类继承父类的注解，**在使用时用在类上，可以被子类所继承，对属性或方法无效**

```java
/**自定义注解*/
@Documented
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface MyTestAnnotation {
}
/**父类标注自定义注解*/
@MyTestAnnotation
public class Father {
}
/**子类*/
public class Son extends Father {
}
/**测试子类获取父类自定义注解*/
public class test {
   public static void main(String[] args){

      //获取Son的class对象
       Class<Son> sonClass = Son.class;
      // 获取Son类上的注解MyTestAnnotation可以执行成功
      MyTestAnnotation annotation = sonClass.getAnnotation(MyTestAnnotation.class);
   }
}
```

### @Repeatable

- repeatable意思是可重复的，说明这个元注解所在的注解可以同时作用一个对象多次，但是每次作用注解又可以代表不同的意义
- JDK1.8出现的，作用是解决一个类上不能标注重复的注解
- 可以说@Repeatable(Skills.class)中的Skills这个类**相当于它的一个容器**

```java
/**技能容器注解*/
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface Skills {
    Skill[] value();
}
/**技能注解*/
@Repeatable(Skills.class)
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface Skill {
    String value() default "睡觉";
}
/**人类*/
@Skill(value = "java")
@Skill(value = "php")
@Skill(value = "python")
@Skill(value = "javasript")
public class Human {
  	
}
```

```java
public static void main(String[] args) {
  Human man = new Human();
  Class<?> clazz = man.getClass();
  Skill[] skills = clazz.getAnnotationsByType(Skill.class);
  for (Skill skill : skills) {
      System.out.println("他会:"+skill.value());
  }
}

/*
他会:java
他会:php
他会:python
他会:javasript
*/
```

---

## 三、注解的属性

- 注解的属性其实和类中定义的变量有异曲同工之处，只是**注解中的变量都是成员变量**，并且注解中是没有方法的，**变量名就是注解中对应的参数名**，变量返回值注解对应注解的参数类型。
- @Repeatable注解中的变量则类型则是对应**Annotation（接口）的泛型Class**。

```java
/**注解Repeatable源码*/
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Repeatable {
    /**
     * Indicates the <em>containing annotation type</em> for the
     * repeatable annotation type.
     * @return the containing annotation type
     */
    Class<? extends Annotation> value();
}
```

### 注解的本质

- 本质：是一个Annotation接口

![image-20200817171453108](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200817171453108.png)

- 通过源码我们知道，注解本身就是Annotation接口的子接口，就是说注解中其实是可以有属性和方法的，但是**接口中的属性都是static final的**，对于注解没有什么意义，我们**定义接口的方法就相当于注解的属性**，也就是为什么注解只有属性成员变量，其实他就是接口的方法，这就是为什么成员变量会有括号，不同于接口的是，我们可以在注解中给成员变量（接口中的方法）赋值

### 注解的类型

- 注解成员变量**支持的属性类型**有
  1. 基本数据类型
  2. String
  3. 枚举类型（用于多返回值的情况）
  4. Class类型
  5. 以上类型的一维数组

### 注解成员变量赋值

- 如果注解**有多个属性，可以在注解括号中用逗号隔开**，分别给对应的属性赋值
- 如果注解想给注解**设置默认值在注解后加入default然后注入值**，在使用注解时可以不声明，声明就覆盖，不声明就使用默认的

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Inherited
public @interface TestAnnotation {
    String name();
    int count() default 10;
}
```

```java
public class Tset2 {
    @TestAnnotation(name="hello,world")
    public void testAnnotation(String ...args){
        System.out.println("testAnnotation");
    }
}
```

```java
public class Test {

  public static void main(String[] args) throws NoSuchMethodException {
    try{
      Method annotationValue = Tset2.class.getDeclaredMethod("testAnnotation", String[].class);
      boolean hasAnnotation = annotationValue.isAnnotationPresent(TestAnnotation.class);
      if (hasAnnotation){
        TestAnnotation annotation = annotationValue.getAnnotation(TestAnnotation.class);
        int count = annotation.count();
        String name = annotation.name();
        System.out.println("count:"+count);
        System.out.println("name:"+name);
      }
    }catch (NoSuchMethodException error){
      error.printStackTrace();
    }
  }
}
/**
输出结果：
count:10
name:hello,world
*/

```

### 获取注解属性

> 参考博客：https://blog.csdn.net/Goodbye_Youth/article/details/84036809

- 这是使用注解的关键，获取属性的值才是使用注解的目的
- 获取属性注解，只能使用反射，主要有三个基本方法

- 主要是使用Java反射Method类的三个方法

  1. **isAnnotationPresent(Class<? extends Annotation> annotationClass)**

  ```java
  /**是否存在对应 Annotation 对象*/
  public boolean isAnnotationPresent(Class<? extends Annotation> annotationClass) {
    return GenericDeclaration.super.isAnnotationPresent(annotationClass);
  }
  ```

  - 测试代码

    - 如果该方法对象上有指定类型的注解，则返回 true，否则为 false

    ```java
    public class MethodTest {
      @MethodAnnotation(key = "key", value = "value")
      public void test() {}
    
      public static void main(String[] args) throws Exception {
        Method method = MethodTest.class.getDeclaredMethod("test");
        // true
        System.out.println(method.isAnnotationPresent(MethodAnnotation.class));
      }
    }
    ```

  2. **getAnnotation(Class<T> annotationClass)**

  ```java
  /**获取 Annotation 对象*/
  public <A extends Annotation> A getAnnotation(Class<A> annotationClass) {
    Objects.requireNonNull(annotationClass);
    return (A) annotationData().annotations.get(annotationClass);
  }
  ```

  - 测试代码

    - 如果该方法对象存在指定类型的注解，则返回该注解，否则返回 null
    - **只有类级别的注解会被继承得到，对于其他对象而言，getAnnotation() 方法与 getDeclaredAnnotation() 方法作用相同**

    ```java
    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    public @interface MethodAnnotation {
      String key();
      String value();
    }
    
    public class MethodTest {
      @MethodAnnotation(key = "key", value = "value")
      public void test() {}
    
      public static void main(String[] args) throws Exception {
        Method method = MethodTest.class.getDeclaredMethod("test");
        MethodAnnotation annotation = method.getAnnotation(MethodAnnotation.class);
        // @lang.reflect.MethodAnnotation(value=value, key=key)
        System.out.println(annotation);
      }
    }
    ```

  3. **getAnnotationsByType(Class<T> annotationClass)**

  ```java
  /**获取所有 Annotation 对象数组*/   
  public Annotation[] getAnnotations() {
    return AnnotationParser.toArray(annotationData().annotations);
  }  
  ```

  - 测试代码

    - 如果该方法对象存在指定类型的注解，则返回该注解数组，否则返回 null
    - **只有类级别的注解会被继承得到，对于其他对象而言，getAnnotationsByType() 方法与 getDeclaredAnnotationsByType() 方法作用相同**
    - getAnnotationsByType() 方法与 getAnnotation() 方法的区别在于：getAnnotationsByType() 方法会检查修饰该方法对象的注解是否为可重复类型注解，如果是则会返回修饰该方法对象的一个或多个注解
    - **@Repeatable** 用于声明注解为可重复类型注解
    - **当声明为可重复类型注解后，如果方法注解仍为一个，则 getAnnotation() 方法会正常返回，如果方法注解为多个，则 getAnnotation() 方法会返回 null**

    ```java
    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    @Repeatable(RepeatableAnnotation.class)
    public @interface MethodAnnotation {
      String key();
      String value();
    }
    
    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    @interface RepeatableAnnotation {
      MethodAnnotation[] value();
    }
    
    public class MethodTest {
    
      @MethodAnnotation(key = "key1", value = "value1")
      @MethodAnnotation(key = "key2", value = "value2")
      public void test() {}
    
      public static void main(String[] args) throws Exception {
        Method method = MethodTest.class.getDeclaredMethod("test");
        // null
        System.out.println(method.getAnnotation(MethodAnnotation.class));
        MethodAnnotation[] annotationsByType = method.getAnnotationsByType(MethodAnnotation.class);
        // [@lang.reflect.MethodAnnotation(value=value1, key=key1), @lang.reflect.MethodAnnotation(value=value2, key=key2)]
        System.out.println(Arrays.toString(annotationsByType));
      }
    }
    ```

- 获取一下注解属性，在获取自定义注解之前必须使用元注解**@Retention(RetentionPolicy.RUNTIME)**确保在**运行期间可以获取到注解**

- 测试

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface TestAnnotation {
    String name();
    int count() default 10;
}
```

```java
package xyz.kidjaya.test;

public class Tset2 {
    @TestAnnotation(name="hello,world")
    public void testAnnotation(String ...args){
        System.out.println("testAnnotation");
    }
}
```

```java
public class Test {

  public static void main(String[] args) throws NoSuchMethodException {
    try{
      Method annotationValue = Tset2.class.getDeclaredMethod("testAnnotation", String[].class);
      boolean hasAnnotation = annotationValue.isAnnotationPresent(TestAnnotation.class);
      if (hasAnnotation){
        TestAnnotation annotation = annotationValue.getAnnotation(TestAnnotation.class);
        int count = annotation.count();
        String name = annotation.name();
        System.out.println("count:"+count);
        System.out.println("name:"+name);
      }
    }catch (NoSuchMethodException error){
      error.printStackTrace();
    }
  }
}
/**
运行结果：
count10
name:hello,world
*/
```


---

## 四、JDK 提供的注解

> 参考博客：https://www.itzhai.com/java-based-notebook-annotation-annotation-introduction-and-use-custom-annotations.html

### @Override注解

> java.lang
> 注释类型 Override
> @Target(value=METHOD)
> @Retention(value=SOURCE)
> public @interface ***Override***

- 表示一个方法声明打算重写父类中的另一个方法声明。如果方法利用此注释类型进行注解但没有重写超类方法，则编译器会生成一条错误消息
- @Override注解表示子类要重写父类的对应方法
- Override是一个Marker annotation，用于标识的Annotation，Annotation名称本身表示了要给工具程序的信息，**就是标记的作用**

```java
public class DefaultImpl implements TestDefault {
    @Override
    public String toString() {
        return "DefaultImpl{}";
    }
}
```

### @Deprecated注解

> java.lang
> 注释类型 Deprecated
> @Documented
> @Retention(value=RUNTIME)
> public @interface ***Deprecated***

- 用 @Deprecated 注释的程序元素，不鼓励程序员使用这样的元素，通常是因为它很危险或存在更好的选择。在使用不被赞成的程序元素或在不被赞成的代码中执行重写时，编译器会发出警告
- @Deprecated注解表示方法是不被建议使用的
- Deprecated是一个Marker annotation，**标记被废弃了，但可以使用，可能会有程序出错的危险，不兼容等问题**

```java
package xyz.kidjaya.test;

public class DefaultImpl implements TestDefault {

    @Deprecated
    public void oldFunction(){
        System.out.println(new DefaultImpl());
    }

    @Override
    public String toString() {
        return "DefaultImpl{}";
    }

    public static void main(String[] args) {
        new DefaultImpl().oldFunction();
    }
}
```

![image-20200817181421755](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200817181421755.png)

### @SuppressWarnings注解

> java.lang
> 注释类型 SuppressWarnings
> @Target(value={TYPE,FIELD,METHOD,PARAMETER,CONSTRUCTOR,LOCAL_VARIABLE})
> @Retention(value=SOURCE)
> public @interface ***SuppressWarnings***

- 指示应该在注释元素（以及包含在该注释元素中的所有程序元素）中取消显示指定的编译器警告。注意，在给定元素中取消显示的警告集是所有包含元素中取消显示的警告的超集。例如，如果注释一个类来取消显示某个警告，同时注释一个方法来取消显示另一个警告，那么将在此方法中同时取消显示这两个警告。 根据风格不同，**程序员应该始终在最里层的嵌套元素上使用此注释，在那里使用才有效。如果要在特定的方法中取消显示某个警告，则应该注释该方法而不是注释它的类**
- @SuppressWarnings注解表示抑制警告

```java
@SuppressWarnings("unchecked")
public static void main(String[] args) {
  List list = new ArrayList();
  list.add("abc");
  System.out.println(list);
}
```

- **扩展：List集合使用泛型与不使用泛型有什么区别?**（可以略过）

> 参考：https://blog.csdn.net/u010772673/article/details/58049180?utm_source=blogxgwz7

- 例如：List lists=new ArrayList();和List<String> lists=new ArrayList<String>();他们两者有什么样区别？
- list中取出的值不一样，第一种的话取出来是object类型的，需要强制类型转换，而第二个不用，取出来之后直接就是string类型的
- **注意:** ***List<String> lists=new ArrayList<String>(); 中ArrayList<~~String~~>中的String可以省略,含义相同\***（JDK7增加）
- Java 语言中引入泛型是一个较大的功能增强。不仅语言、类型系统和编译器有了较大的变化，以支持泛型，而且类库也进行了大翻修，所以许多重要的类，比如集合框架，都已经成为泛型化的了。
- 好处：
  1. **类型安全。** 泛型的主要目标是提高 Java 程序的类型安全。通过知道使用泛型定义的变量的类型限制，编译器可以在一个高得多的程度上验证类型假设。没有泛型，这些假设就只存在于程序员的头脑中（或者如果幸运的话，还存在于代码注释中）。
  2. **消除强制类型转换。** 泛型的一个附带好处是，消除源代码中的许多强制类型转换。这使得代码更加可读，并且减少了出错机会。
  3. **潜在的性能收益。** 泛型为较大的优化带来可能。在泛型的初始实现中，编译器将强制类型转换（没有泛型的话，程序员会指定这些强制类型转换）插入生成的字节码中。但是更多类型信息可用于编译器这一事实，为未来版本的 JVM 的优化带来可能。由于泛型的实现方式，支持泛型（几乎）不需要 JVM 或类文件更改。所有工作都在编译器中完成，编译器生成类似于没有泛型（和强制类型转换）时所写的代码，只是更能确保类型安全而已。

---

## 五、注解作用与应用

- 重新看回开头那句话

> Java注释用于Java代码提供元数据，作为元数据，注解不直接影响你的代码执行，但也有一些类型的注解实际上可以用于这一目的

- 经过学习，我们可以知道，注解是一个很方便的东西，它的存活时间，作用域都可以通过元注解来实现，只是看你用注解来实现什么样的功能

- 使用注解可以对参数进行配置，代码可以看到上面的代码 **三、注解的属性->注解成员变量赋值**

---

## 六、注解小结

- 提供信息给编译器：编译器可以利用注解来检测出错误或者警告信息，打印出日志
- 编译阶段时的处理：软件工具可以利用注解信息来自动生成代码、文档或者做其他相应的自动处理**idea**
- 运行时处理：某些注解可以在程序运行的时候接收代码的提取，自动做相应的操作
- 注解能够提供metaData也就是元数据，**处理提取和处理 Annotation 的代码统称为 APT（Annotation Processing Tool)**，处理获取注解值的过程是我们开发者直接写的注解提取逻辑，可以理解为APT工具类
- **作用：为学习SSM以及SpringBoot，SpringCloud打下基础**

