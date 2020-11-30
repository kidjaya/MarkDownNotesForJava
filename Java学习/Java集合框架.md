# 集合框架

> - 视频学习：<https://www.bilibili.com/video/BV1Wx411f7qN?p=148>
> - 集合：用来储存多个元素的容器
> - 集合和数据区别：
>   - 元素类型：
>     - 集合：引用类型（储存基本类型自动装箱）
>     - 数组：基本类型，引用类型
>   - 元素个数：
>     - 集合；不固定，可以扩容
>     - 数组：固定，不能改变容量
>   - 集合的优点：不受容器大小的限制，随时添加、删除元素，提供了大量操作元素的方法

> - Java的集合体系
>   - 单列集合（Collection）
>     - List->ArrayList
>     - Set->HashSet
>   - 双列集合（Map：key，value）
>     - Map->HashMap



---

## List集合

> - 特点
>   - 可重复、有序(存取顺序相同，可扩展数组）
> - 应用
>   - List list = new ArrayList();

``` java
class TestList{
    public static void main(String[] args) {
        //1.创建集合对象
        List list = new ArrayList();
        //2.创建元素,元素可以重复～
        Teacher yang = new Teacher("yang",4);
        Teacher huang = new Teacher("huang",6);
        Teacher yang2 = new Teacher("yang",4);
        //3.将元素放到集合
        list.add(yang);
        list.add(yang2);
        list.add(huang);
        //4.打印
        System.out.println(list);
        //5.获取索引为2的元素
        System.out.println(list.get(2));
        //6.循环输出
        for (int i = 0; i < list.size() ; i++) {
            System.out.println("list["+i+"]"+list.get(i));
        }
        //7.使用增强for循环输出
        //快捷键 iter 增强for底层原理是Iterator迭代器
        for (Object o : list) {
            System.out.println(o);
        }
    }
}
```

- 迭代器
  - 概述：对于过程的重复，遍历Collection集合的通用方式
  - 方法：E next()   boolean hasNext()
  - 注意：列表迭代器是List体系独有的便利方式，可以在对集合遍历的同时进行添加、删除等操作，必须通过调用列表迭代器的方法来实现
  - 使用步骤：
    1. 根据集合对象获取其对象的迭代器对象
    2. 判断迭代器中是否有元素
    3. 如果有元素就获取元素

```java
class TestList{
    public static void main(String[] args) {
        //创建集合对象
        List list = new ArrayList();
        //创建元素,元素可以重复～
        Teacher yang = new Teacher("yang",4);
        Teacher huang = new Teacher("huang",6);
        Teacher yang2 = new Teacher("yang",4);
        //将元素放到集合
        list.add(yang);
        list.add(yang2);
        list.add(huang);
        //迭代器访问
        //根据集合对象获取其迭代器对象
        Iterator iterator = list.iterator();
        while (iterator.hasNext()){//如果迭代器中有元素，就一直迭代
            Object obj = (Teacher)iterator.next();//获取迭代器元素
            System.out.println("obj = " + obj);
        }
    }
}
```

- 测试列表迭代器

```java
public class ListIteratorTest {
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add(1);
        list.add(2);
        list.add(3);
        //在遍历过程中判断b，在其后面加一个新的数字
        //根据集合对象获取列表迭代器对象
        ListIterator lit = list.listIterator();
        //判断迭代器是否有元素
        while (lit.hasNext()){
            Integer i = (Integer)lit.next();
            if (i.equals(2)){//"a".equals(i)对于String类型可以写成这样，规避空指针异常
                //list.add(666);//java.util.ConcurrentModificationException并发修改异常
                lit.add(666);
            }
            System.out.println(i);
        }
        System.out.println(list);
    }
}
```

- 总结
  1. 普通迭代器（Iterator）在遍历集合的同时不能添加或删除元素，否则会报错---并发修改异常
  2. 列表迭代器（ListIteratorTest）在遍历集合过程的同时可以修改集合中的元素 l

---

## 泛型简介

> 指任意类型，又叫参数化类型，对具体类型的使用起到辅助作用，类似于方法的参数
>
> - 集合类泛型
>   - 表示该集合中存放指定类型的元素
>   - List<String> list = new ArrayList<>();
>   - 好处：类型安全，避免了类型的转换

```java
public class TTest {
    public static void main(String[] args) {
        //不使用泛型的集合
        //创建集合对象
        List list = new ArrayList();
        //创建元素对象
        list.add("a");
        list.add("b");
        list.add("c");
        //list.add(666); 类型不安全，会报java.lang.ClassCastException类型转换异常
        //遍历数据
        for (Object o : list) {
            String str = (String)o;
            System.out.println("str = " + str);
        }
        
        //创建集合对象
        List<String> list2 = new ArrayList<String>();
        //添加数据
        list2.add("aa");
        list2.add("bb");
        list2.add("cc");
        //遍历集合
        for (String s : list2) {
            System.out.println("s = " + s);
        }
    }
}
```

- 总结
  1. 泛型一般只和集合类相结合使用
  2. 泛型是JDK5的新特性，但是JDK7开始，后面的泛型可以不用写具体的类型了（菱型泛型）

---

## Collections工具类

> Collections属于java.util 可以通过类名.方法直接调用！
>
> 针对集合进行操作的工具类
>
> 成员方法：（查询API文档）
>
> - max(List<T>)	返回集合的最大元素
> - sort(List<T>)               根据元素的自然顺序，将指定列表按升序排列
> - reverse(List<T>)          反转List集合元素
> - shuffle(List<T>).          使用默认的随机源置换指定的列表

```java
public class CollectionsTest {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(55);
        list.add(1);
        list.add(7);
        list.add(3);
        list.add(8);
        list.add(9);
        list.add(5);
        list.add(2);
        //打印集合
        System.out.println("list = " + list);
        //获取集合中最大元素
        Integer max = Collections.max(list);
        System.out.println("max = " + max);
        //升序排序
        Collections.sort(list);
        System.out.println("sort_list = " + list);
        //数据反转
        Collections.reverse(list);
        System.out.println("reverse_list = " + list);
        //随机置换
        Collections.shuffle(list);
        System.out.println("shuffle_list = " + list);
    }
}
```

---

## Set集合

> - 特点
>   - 不可重复，无序
> - 应用
>   - Set<T> set = new HashSet<>();
> - 结论
>   - Set集合保证元素唯一性的依赖: Object类中的equals() 和 hashCode() 两个方法，新建的类需要重写

```java
public class SetTest {
    public static void main(String[] args) {
        //创建集合对象
        Set<Student> set = new HashSet<>();
        //创建元素对象
        Student s1 = new Student("yang",99,"s1997");
        Student s3 = new Student("yang1",33,"s1994");
        Student s2 = new Student("yang1",33,"s1994");
        Student s4 = new Student("yang4",100,"s1991");
        Student s5 = new Student("yang4",100,"s1991");
        Student s6 = new Student("yang",99,"s1997");
        //将元素对象添加到集合对象中
        set.add(s1);
        set.add(s2);
        set.add(s3);
        set.add(s4);
        set.add(s5);
        //遍历集合
        System.out.println(set);//打印顺序和存储不一致，无序
        /**
         * 为什么Set没有去重，因为Set集合保证元素的唯一性依赖：equals() hashCode()两个方法
         * 需要在新类中重写这两个方法～否者比较的调用的是Object类中的地址值，所以都会不一样
         */
        //迭代器输出
        Iterator<Student> iterator = set.iterator();
        while (iterator.hasNext()){
            Student s = iterator.next();
            System.out.println("s = " + s);
        }
        //增强for循环输出
        for (Student student : set) {
            System.out.println("s = " + student);
        }
    }
}
```

## Map集合

> - 特点
>   - 双列集合，有key和value构成
>   - key不可以重复，value可以重复
> - 应用
>   - Map<T1,T2>map = new HashMap<>();
> - 成员方
>   - V put(K key,V value);     添加元素(键值对的形式)，元素第一次添加返回null，重复添加，会覆盖旧值，并返回旧值
>   - V get(Object key);         根据键获取其对应的值
>   - Set<K> keySet();           获取所有键的集合

```java
public class MapTest {
    public static void main(String[] args) {
        //创建集合对象
        Map<Integer,Student> map = new HashMap<>();
        //创建元素对象
        Student s1 = new Student("hello",99,"akjdsklaj");
        Student s2 = new Student("world",97,"asdasdas");
        Student s3 = new Student("!",98,"cxvzxczxc");
        //将元素对象添加到集合中
        Student temp = map.put(1,s1);
        Student temp1 = map.put(1,s2);//第一返回null，重复添加会返回旧值
        map.put(2,s1);
        map.put(3,s3);
        //根据键值获取对应值
        Student temp2 = map.get(2);
        System.out.println("temp2 = " + temp2);
        //打印集合
        System.out.println(map);
        //遍历集合
        //1.获取所有键的集合
        Set<Integer> keys = map.keySet();
        //2.遍历所有的键，获取到每一个键
        Iterator<Integer> it = keys.iterator();
        while (it.hasNext()){
            //如果迭代器中有数据，就获取
            Integer key = it.next();
            Student value = map.get(key);
            System.out.println("key = " + key + " value = " + value);
        }
        //增强for遍历获取
        for (Integer key : keys) {
            System.out.println("key = " + key + " value = " + map.get(key));
        }
    }
}
```

## 模拟斗地主发牌

```java
public class SendPokerTest {
    public static void main(String[] args) {
        //买牌
        //定义一个双列集合，键表示牌的编号，值表示具体的牌 规则：编号越小，牌就越小
        Map<Integer,String> pokers = new HashMap<>();
        //定义一个单列集合，用来存储所有的牌号
        List<Integer> list = new ArrayList<>();
        //具体的买牌动作
        //普通牌：52张
        String[] colors = {"♥","♠","♣","♦"};
        String[] numbers = {"3","4","5","6","7","8","9","10","J","Q","K","A","2"};
        //循环嵌套
        int num = 0;
        for (String number : numbers) {
            for (String color : colors) {
                String poker = color+number;
                //将所有牌的编号放到双列集合中
                pokers.put(num,poker);
                //将牌的编号放到单列集合中
                list.add(num);
                num++;
            }
        }
        //添加大小王
        pokers.put(num,"小王");
        list.add(num);
        num++;
        pokers.put(num,"大王");
        list.add(num);
        System.out.println("所有的牌: "+pokers);
        System.out.println("牌的编号: " + list);
        //洗牌
        Collections.shuffle(list);
        System.out.println("洗好的牌: " + list);
        //发牌
        //定义四个集合，分别表示三个玩家和底牌
        List<Integer> yang = new ArrayList<>();
        List<Integer> liu  = new ArrayList<>();
        List<Integer> xie  = new ArrayList<>();
        List<Integer> dipai  = new ArrayList<>();
        //索引为3取模发牌
        for (int i = 0; i <list.size() ; i++) {
            if (i>=list.size()-3){
                dipai.add(list.get(i));
            }else if (i%3 == 0){
                yang.add(list.get(i));
            }else if (i%3 == 1){
                liu.add(list.get(i));
            }else if (i%3 == 2){
                xie.add(list.get(i));
            }
        }
        System.out.println("yang = " + yang);
        System.out.println("liu = " + liu);
        System.out.println("xie = " + xie);
        System.out.println("dipai = " + dipai);
        //查看牌
        System.out.println("yang = " + printPoker(yang,pokers));
        System.out.println("liu = " + printPoker(liu,pokers));
        System.out.println("xie = " + printPoker(xie,pokers));
        System.out.println("dipai = " + printPoker(dipai,pokers));

    }
  
    //查看牌
    public static String printPoker(List<Integer> nums,Map<Integer,String> pokers){
        //排列顺序
        Collections.sort(nums);
        //遍历编号，获取到每一个编号的值
        StringBuilder str = new StringBuilder();
        for (Integer num : nums){
            //查找对应值
            String poker = pokers.get(num);
            str.append(poker+" ");
        }
        String _pokers = str.toString();
        return  _pokers;
    }
}
```

