# JUC并发编程

> 视频参考：https://www.bilibili.com/video/BV1B7411L7tE/?p=1

## 1.什么是JUC

**java.util.concurrent**

源码+文档 ==> 面试高频 

Java.util工具包，包 ， 分类

业务：普通的线程代码 Thread

Runnable没有返回值，效率相比Callable较低

## 2.进程和线程

> 线程、进程，如果不能使用一句话说出来的技术，不扎实

**进程**：一个程序，Wechat.exe,QQ.app等的集合

一个进程往往可以包含多个线程，至少包含一个！

Java默认包含几个线程？2个main 和 GC

**线程**：开了一个进程Typora，写字，自动保存（线程负责的） 

对于Java而言 Thread、Runnable、Callable

Java真的可以开启线程吗？

```java
    public synchronized void start() {
        /**
         * This method is not invoked for the main method thread or "system"
         * group threads created/set up by the VM. Any new functionality added
         * to this method in the future may have to also be added to the VM.
         *
         * A zero status value corresponds to state "NEW".
         */
        if (threadStatus != 0)
            throw new IllegalThreadStateException();

        /* Notify the group that this thread is about to be started
         * so that it can be added to the group's list of threads
         * and the group's unstarted count can be decremented. */
        group.add(this);

        boolean started = false;
        try {
            start0();
            started = true;
        } finally {
            try {
                if (!started) {
                    group.threadStartFailed(this);
                }
            } catch (Throwable ignore) {
                /* do nothing. If start0 threw a Throwable then
                  it will be passed up the call stack */
            }
        }
    }
		//本地方法调用底层C++，Java无法直接操作硬件
    private native void start0();
```



> 并发、并行

并发编程：并发、并行

并发（多线程操作同一个资源）

- CPU一核，模拟出多条线程，天下武功，唯快不破，快速交替

并行（多个人一起行走）

- CPU多核，多个线程可以同时执行；

```java
public class Test01 {
    public static void main(String[] args) {
        //获取CPU核数
        //CPU密集型，IO密集型
        System.out.println(Runtime.getRuntime().availableProcessors());
    }
}
```

并发编程的本质：**充分利用CPU的资源**

所有公司都很看重

企业，挣钱=>提高效率，裁员，找一个厉害的顶替三个不怎么厉害的

人员、技术成本！



> 线程有几个状态

```java
				/**
         新生
         */
        NEW,

        /**
 					运行
         */
        RUNNABLE,

        /**
					阻塞
         */
        BLOCKED,

        /**
      		等待，死死的等
         */
        WAITING,

        /**
        	超时等待
         */
        TIMED_WAITING,

        /**
        	终止
         */
        TERMINATED;
```



> wait/sleep 区别

1. 来自不同的类
   - wait=>Object
   - sleep=>Thread
2. 关于锁的释放
   - wait会释放锁，sleep睡觉了，抱着锁睡觉，不会释放！
3. 使用的范围是不同的
   - wait  必须在同步代码块中
   - sleep 可以在任何地方睡
4. 是否需要捕获异常
   - wait 不需要捕获异常
   - sleep 必须要捕获异常



## 3.Lock锁（重点）

> 传统Synchroized

```java
public class SaleTicketDemo02 {

    public static void main(String[] args) {
        //并发：多个线程操作同一个资源类，把资源类丢入线程
        Ticket2 ticket = new Ticket2();

        //FunctionalInterface 函数式接口
        new Thread(()->{for (int i = 0; i < 30; i++)  ticket.sale(); },"A2").start();
        new Thread(()->{for (int i = 0; i < 30; i++)  ticket.sale(); },"B2").start();
        new Thread(()->{for (int i = 0; i < 30; i++)  ticket.sale(); },"C2").start();
    }
}

//资源类 oop
//Lock三部曲
//1.new ReentrantLock();
//2.lock.lock()加锁
//3.finally--> lock.unlock();//解锁
class Ticket2{
    //属性、方法
    private int number = 50;
    private int count = 0;

    //
    Lock lock = new ReentrantLock();

    //卖票的方式
    public void sale(){
        lock.lock();//加锁
        try{
            if (number>0){
                System.out.println(Thread.currentThread().getName()+"卖出了"+(++count)+"张票,剩余"+(--number));
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
```



> Lock接口

![tKTkNV.png](https://s1.ax1x.com/2020/05/30/tKTkNV.png)

![tKTec4.png](https://s1.ax1x.com/2020/05/30/tKTec4.png)

![tKTmjJ.png](https://s1.ax1x.com/2020/05/30/tKTmjJ.png)

公平锁：十分公平--->先来后到

**非公平锁：十分不公平--->可以插队（默认）**

```java
public class SaleTicketDemo02 {

    public static void main(String[] args) {
        //并发：多个线程操作同一个资源类，把资源类丢入线程
        Ticket2 ticket = new Ticket2();

        //FunctionalInterface 函数式接口
        new Thread(()->{for (int i = 0; i < 30; i++)  ticket.sale(); },"A2").start();
        new Thread(()->{for (int i = 0; i < 30; i++)  ticket.sale(); },"B2").start();
        new Thread(()->{for (int i = 0; i < 30; i++)  ticket.sale(); },"C2").start();
    }
}

//资源类 oop
//Lock三部曲
//1.new ReentrantLock();
//2.lock.lock()加锁
//3.finally--> lock.unlock();//解锁
class Ticket2{
    //属性、方法
    private int number = 50;
    private int count = 0;

    //
    Lock lock = new ReentrantLock();

    //卖票的方式
    public void sale(){
        lock.lock();//加锁
        try{
            if (number>0){
                System.out.println(Thread.currentThread().getName()+"卖出了"+(++count)+"张票,剩余"+(--number));
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
```



> Synchronized 和 Lock 区别

1. Synchronized 内置的java关键字，Lock是一个java类
2. Synchronized 无法判断获取锁的状态，Lock可以判断是否获取到了锁
3. Synchronized 会自动释放锁，Lock必须要手动释放锁！如果不释放锁，死锁
4. Synchronized 线程1（获得锁，阻塞）、线程2（等待，傻傻的等），Lock就不一定会等待下去
5. Synchronized 可重入锁，不可以中断的，非公平；Lock，可重入锁，可以判断锁，非公平（可以自己设置）
6. Synchronized 适合锁少量的代码同步问题，Lock适合锁大量的代码同步问题



> 锁是什么，如何判断锁的是谁！

 

## 4.生产者和消费者问题

面试：单例模式、排序算法、生产者和消费者、死锁

> 生产者和消费者问题 Synchronized版

```java
/*
    线程之间的通信问题：生产者与消费者问题  等待唤醒，通知唤醒
    线程交替执行 A B 同时操作一个变量 num = 0
    A num+1
    B num-1
 */
public class A {
    public static void main(String[] args) {
        Data data = new Data();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try{
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"A").start();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try{
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();
    }
}

// 判断等待，业务，通知
class Data{//数字 资源类
    private int number = 0;

    //+1
    public synchronized void increment() throws InterruptedException {
        if (number!=0){
            //等待
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        //通知其他线程，我+1完毕了
        this.notifyAll();
    }

    //-1
    public synchronized void decrement() throws InterruptedException {
        if (number == 0){
            //等待
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);
        //通知其他线程，我-1完毕了
        this.notifyAll();
    }
}
```

> 问题存在 A B C D 四个线程 还安全吗～ 虚假唤醒

 ![tKTKBR.png](https://s1.ax1x.com/2020/05/30/tKTKBR.png)



> JUC版的生产者和消费者问题

![tKTYge.png](https://s1.ax1x.com/2020/05/30/tKTYge.png)

![tKTlAx.png](https://s1.ax1x.com/2020/05/30/tKTlAx.png)

代码实现：

 ```java
/*
    线程之间的通信问题：生产者与消费者问题  等待唤醒，通知唤醒
    线程交替执行 A B 同时操作一个变量 num = 0
    A num+1
    B num-1
 */
public class B {
    public static void main(String[] args) {
        Data2 data = new Data2();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try{
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"A").start();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try{
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try{
                    data.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"C").start();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                try{
                    data.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"D").start();
    }
}

// 判断等待，业务，通知
class Data2{//数字 资源类
    private int number = 0;

    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();

    //condition.await();等待
    //condition.signalAll();唤醒全部

    //+1
    public void increment() throws InterruptedException {
        try{
            lock.lock();
            //业务代码
            while (number!=0){
                //等待
                condition.await();
            }
            number++;
            System.out.println(Thread.currentThread().getName()+"=>"+number);
            //通知其他线程，我+1完毕了
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }

    //-1
    public void decrement() throws InterruptedException {
        try{
            lock.lock();
            while (number == 0){
                //等待
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName()+"=>"+number);
            //通知其他线程，我-1完毕了
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
 ```

**任何一种新技术，绝对不仅仅只是覆盖了原来的技术，是优势和补充**

> Condition 精准的通知和唤醒线程

![tKb55t.png](https://s1.ax1x.com/2020/05/30/tKb55t.png)

代码测试：

```java
/*
    线程之间的通信问题：生产者与消费者问题  等待唤醒，通知唤醒
    线程交替执行 A B 同时操作一个变量 num = 0
    A num+1
    B num-1
 */
public class C {
    public static void main(String[] args) {
        Data3 data = new Data3();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                data.printA();
            }
        },"A").start();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                data.printB();
            }
        },"B").start();

        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                data.printC();
            }
        },"C").start();

    }
}

// 判断等待，业务，通知
class Data3{//数字 资源类
    private int number = 0;

    Lock lock = new ReentrantLock();
    Condition condition1 = lock.newCondition();
    Condition condition2 = lock.newCondition();
    Condition condition3 = lock.newCondition();

    public void printA(){
        try{
            lock.lock();
            //业务代码
            while (number!=0){
                //等待
                condition1.await();
            }
            number = 1;
            System.out.println(Thread.currentThread().getName()+"=> AAAAAAAA");
            //通知其他线程，我+1完毕了
            condition2.signal();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }

    public void printB(){
        try{
            lock.lock();
            while (number != 1){
                //等待
                condition2.await();
            }
            number = 2;
            System.out.println(Thread.currentThread().getName()+"=> BBBBBBBBB");
            //通知其他线程，我-1完毕了
            condition3.signal();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }

    public void printC(){
        try{
            lock.lock();
            while (number != 2){
                //等待
                condition3.await();
            }
            number = 0;
            System.out.println(Thread.currentThread().getName()+"=> CCCCCCCCC");
            //通知其他线程，我-1完毕了
            condition1.signal();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
```

## 5.    8锁现象

如何判断锁的是谁！永远知道什么锁，锁到底锁的是谁！

**深刻理解锁**

```java
package xyz.kidjaya.lock8;

import java.util.concurrent.TimeUnit;

/**
 * 8锁，就是关于锁的8个问题
 * 1.标准情况下，两个线程先打印 发短信还是打电话 1/发短信 2/打电话
 * 2.sendSms延迟4秒，两个线程先打印 发短信还是 打电话 1/发短信 2/打电话
 */
public class Test1 {
    public static void main(String[] args) {
        Phone phone = new Phone();

        // phone.sendSms()
        // 锁的存在
        new Thread(()->{
            phone.sendSms();
        },"A").start();

        //捕获
        try{
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone.call();
        },"B").start();
    }
}

class Phone{
    //synchronized 锁的是方法的调用者
    //两个方法用的是同一个锁，谁先拿到谁先执行
    public synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("sendSms");
    }

    public synchronized void call(){
        System.out.println("call");
    }
}
```

```java
package xyz.kidjaya.lock8;

import java.util.concurrent.TimeUnit;

public class Test2 {
    public static void main(String[] args) {
        Phone2 phone = new Phone2();
        Phone2 phone2 = new Phone2();

        // phone.sendSms()
        // 锁的存在
        new Thread(()->{
            phone.sendSms();
        },"A").start();

        //捕获
        try{
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone2.call();
        },"B").start();
    }
}

class Phone2{
    //synchronized 锁的是方法的调用者
    //两个方法用的是同一个锁，谁先拿到谁先执行
    public synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("sendSms");
    }

    public synchronized void call(){
        System.out.println("call");
    }
}
```

```java
package xyz.kidjaya.lock8;

import java.util.concurrent.TimeUnit;

public class Test3 {
    public static void main(String[] args) {
        //两个对象的Class类模版只有一个，static，锁的是class
        Phone3 phone1 = new Phone3();
        Phone3 phone2 = new Phone3();

        // phone.sendSms()
        // 锁的存在
        new Thread(()->{
            phone1.sendSms();
        },"A").start();

        //捕获
        try{
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone2.call();
        },"B").start();
    }
}

//Phone3唯一的一个class对象
class Phone3{
    //synchronized 锁的是方法的调用者
    //static 静态方法
    //类一加载就有了！锁的事Class
    public static synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("sendSms");
    }

    public static synchronized void call(){
        System.out.println("call");
    }
}
```

```java
package xyz.kidjaya.lock8;

import java.util.concurrent.TimeUnit;

public class Test4 {
    public static void main(String[] args) {
        //两个对象的Class类模版只有一个，static，锁的是class
        Phone4 phone1 = new Phone4();
        Phone4 phone2 = new Phone4();

        // phone.sendSms()
        // 锁的存在
        new Thread(()->{
            phone1.sendSms();
        },"A").start();

        //捕获
        try{
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(()->{
            phone2.call();
        },"B").start();
    }
}

//Phone3唯一的一个class对象
class Phone4{
    //synchronized 锁的是方法的调用者
    //static 静态方法
    //类一加载就有了！锁的是Class
    public static synchronized void sendSms(){
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("sendSms");
    }

    //锁的是调用者，和上面不是同一个锁
    public synchronized void call(){
        System.out.println("call");
    }
}
```

> 小结

- new this 具体的一个手机

- static Class 唯一的一个模版

## 6.集合类不安全

> List不安全

```java
//java.util.ConcurrentModificationException
public class TestList {
    public static void main(String[] args) {
        //并发下ArrayList是不安全的
        /**
         * 解决方案：
         * 1.List<String> list = new Vector<>();
         * 2.List<String> list = Collections.synchronizedList(new ArrayList<>());
         * 3.List<String> list = new CopyOnWriteArrayList<>();
         */

        //CopyOnWrite 写入时复制 COW 计算机程序设计领域的一种优化策略；
        //多个线程调用的时候，List，读取的时候，固定的，写入（覆盖）
        //写入的时候避免覆盖，造成数据问题
        //读写分离 MyCat （数据库方面扩展）
        //CopyOnWriteArrayList 比 Vector NB 在哪里
        //Vector使用synchronized CopyOnWriteArrayList使用Lock锁
        
        List<String> list = new CopyOnWriteArrayList<>();
        for (int i = 1; i <= 10; i++) {
            new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(list);
            },String.valueOf(i)).start();
        }
    }
}
```

学习方法推荐：1.先会用  2.货比三家，寻找其他解决方案  3.分析源码

> Set 不安全

```java
/**
 * 解决方案
 * 1.Set<String> set = Collections.synchronizedSet(new HashSet<>());
 * 2.Set<String> set = new CopyOnWriteArraySet<>();
 */
public class TestSet {
    public static void main(String[] args) {
//        Set<String> set = new HashSet<>();
//        Set<String> set = Collections.synchronizedSet(new HashSet<>());
        Set<String> set = new CopyOnWriteArraySet<>();

        for (int i = 0; i < 10; i++) {
            new Thread(()->{
                set.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(set);
            },String.valueOf(i)).start();
        }
    }
}
```

hashSet底层是什么？

```java
public HashSet() {
        map = new HashMap<>();
}

//add set 本质就是 map，key是无法重复的
public boolean add(E e) {
        return map.put(e, PRESENT)==null;
}

private static final Object PRESENT = new Object();//不变的值

```

> Map

```java
//java.util.ConcurrentModificationException
public class TestMap {
    public static void main(String[] args) {
        //map 是这样用吗？不怎么用HashMap<>();
        //默认等价于什么？ 16，0.75  new HashMap<>(16,0.75)
        //加载因子、初始化容量

        /**
         * 解决方案
         * 1.Map<String,String> map = Collections.synchronizedMap(new HashMap<>());
         * 2.Map<String,String> map = new ConcurrentHashMap<>();//juc
         */
//        Map<String,String> map = Collections.synchronizedMap(new HashMap<>());
//        Map<String,String> map = new HashMap<>();
        Map<String,String> map = new ConcurrentHashMap<>();//juc

        for (int i = 1; i <= 30; i++) {
            new Thread(()->{
                map.put(Thread.currentThread().getName(), UUID.randomUUID().toString().substring(0,5));
                System.out.println(map);
            },String.valueOf(i)).start();
        }

    }
}
```

- 扩展，查看文档了解**ConcurrentHashMap**原理



## 7.Callable

![tQrp0e.png](https://s1.ax1x.com/2020/05/30/tQrp0e.png)

1. 可以有返回值
2. 可以抛出异常
3. 方法不同，run()  / call()

> 代码测试

![tQs1Ve.png](https://s1.ax1x.com/2020/05/30/tQs1Ve.png)

![tQ6fN4.png](https://s1.ax1x.com/2020/05/30/tQ6fN4.png)

```java
public class TestCallable {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //new Thread(new Runnable()).start();
        //new Thread(new FutureTask<V>()).start();
        //new Thread(new FutureTask<V>(Callable)).start();
        new Thread().start();//  怎么启动Callable？通过Runnable～
        MyThread thread = new MyThread();
        FutureTask futureTask = new FutureTask(thread);//适配类
        new Thread(futureTask,"A").start();
        new Thread(futureTask,"B").start();//结果会被缓存，提高效率
        //get方法可能会产生阻塞，把这个放到最后一行，或者使用异步通信处理
        String o = (String) futureTask.get();//获取Callable的返回结果
        System.out.println(o);
    }
}

class MyThread implements Callable<String> {

    @Override
    public String call() {
        System.out.println("call()");
        return "123";
    }
}
```

**实战！**

细节：

1. 有缓存
2. 结果可能需要进行一些操作，需要等待，会阻塞

## 8.常用辅助类（必会）

### 8.1、CountDownLatch

![tQgwSs.png](https://s1.ax1x.com/2020/05/30/tQgwSs.png)

```java
public class CountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        //总数是6.必须要执行任务的时候，再使用。
        CountDownLatch countDownLatch = new CountDownLatch(6);
        for (int i = 1; i <= 6; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+" Go out");
                countDownLatch.countDown();
            },i+"").start();
        }

        countDownLatch.await();//等待计数器归零,再向下执行
        System.out.println("Close Door");
    }
}
```

原理：

countDownLatch.countDown() ：数量-1

countDownLatch.await() ：等待计数器归零，然后再向下执行

每次有线程调用countDown()就会数量-1，假设计数器变为0，await()就会唤醒线程，继续执行

**底层实现：AbstractQueuedSynchronizer**（需深入）

### 8.2、CyclicBarrier

![tQLf91.png](https://s1.ax1x.com/2020/05/30/tQLf91.png)

加法计数器

```java
public class CyclicBarrierDemo {
    public static void main(String[] args) {
        /**
         * 集齐七棵龙珠召唤神龙
         */
        //召唤龙珠线程
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7,()->{
            System.out.println("召唤神龙成功");
        });

        for (int i = 1; i <= 7; i++) {
            final int temp = i;
            //lambda能操作到i吗？
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+"收集"+temp+"个龙珠");
                try {
                    cyclicBarrier.await();//等待并计数
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}
```



### 8.3、Semaphore

![tQXvm8.png](https://s1.ax1x.com/2020/05/30/tQXvm8.png)

- 信号量

6车---3个停车位

```java
public class SemaphoreDemo {
    public static void main(String[] args) {
        //线程数量：限流！！
        //允许运行的线程数量：停车位
        Semaphore semaphore = new Semaphore(3);

        for (int i = 1; i <= 6; i++) {
            new Thread(()->{
                //acquire() 得到
                try{
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName()+"抢到车位");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println(Thread.currentThread().getName()+"离开车位");
                }catch (InterruptedException e){
                    e.printStackTrace();
                }finally {
                    semaphore.release();
                }
                //release()释放

            }).start();
        }
    }
}
```

原理：

semaphore.acquire();//获取，假设如果已经满了，等待，等待被释放为止

semaphore.release();//释放，会将当前的信号量释放+1，然后唤醒等待的线程

作用：多个共享资源互斥使用！并发限流，控制最大的线程数。



## 9.读写锁

>  ReadWriteLock

![tlGQPS.png](https://s1.ax1x.com/2020/05/31/tlGQPS.png)

```java
/**
 * 独占锁（写锁）一次只能被一个线程占有
 * 共享锁（读锁）多个线程可以同时占有
 * ReadWriteLock
 * 读-读 可以共存
 * 读-写 不能共存
 * 写-写 不能共存
 */
public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyThreadLock myThread = new MyThreadLock();
        for (int i = 1; i <= 5; i++) {
            final int temp = i;
            new Thread(()->{
                myThread.put(""+temp,temp+"");
            },""+i).start();
        }

        for (int i = 1; i <= 5; i++) {
            final int temp = i;
            new Thread(()->{
                myThread.get(temp+"");
            },""+i).start();
        }
    }
}

/**
 * 自定义缓存
 */
class MyThread{
    private volatile Map<String,Object> map = new HashMap<>();

    //存，写
    public void put(String key,Object value){
        System.out.println(Thread.currentThread().getName()+"写入"+key);
        map.put(key,value);
        System.out.println(Thread.currentThread().getName()+"写入OK");
    }

    //取，读
    public void get(String key){
        System.out.println(Thread.currentThread().getName()+"读取"+key);
        Object o = map.get(key);
        System.out.println(Thread.currentThread().getName()+"读取ok");
    }
}

/**
 * 加锁的
 */
class MyThreadLock{
    private volatile Map<String,Object> map = new HashMap<>();
    //读写锁，更加细粒度的控制
    private ReadWriteLock lock = new ReentrantReadWriteLock();

    //存，写   写入的时候，只希望同时只有一个线程写
    public void put(String key,Object value){
        try {
            lock.writeLock().lock();//写锁
            System.out.println(Thread.currentThread().getName()+"写入"+key);
            map.put(key,value);
            System.out.println(Thread.currentThread().getName()+"写入OK");
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            lock.writeLock().unlock();
        }
    }

    //取，读   读取的时候多个线程可以读
    public void get(String key){
        try {
            lock.readLock().lock();
            System.out.println(Thread.currentThread().getName()+"读取"+key);
            Object o = map.get(key);
            System.out.println(Thread.currentThread().getName()+"读取ok");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.readLock().unlock();
        }
    }
}
```

## 10.阻塞队列

- 如果生产队列满了，必须阻塞等待
- 如果队列是空的，必须阻塞等待生产

阻塞队列：

![tlD8Hg.png](https://s1.ax1x.com/2020/05/31/tlD8Hg.png)

![tlsEYd.png](https://s1.ax1x.com/2020/05/31/tlsEYd.png)

BlockingQueue 不是新东西 ![tls8Yj.png](https://s1.ax1x.com/2020/05/31/tls8Yj.png)

- 什么情况下会使用阻塞队列
  - 多线程并发处理
  - 线程池

- 使用队列
  - 添加、移除
- 四组API
  1. 抛出异常
  2. 不会抛出异常
  3. 阻塞
  4. 超时等待

| 方式       | 抛出异常 | 有返回值 | 阻塞、等待 | 超时等待  |
| ---------- | -------- | -------- | ---------- | --------- |
| 添加       | add      | offer    | put        | offer(,,) |
| 移除       | remove   | poll     | Take       | poll(,)   |
| 判断队列首 | element  | peek     | -          | -         |

```java
    /*
    抛出异常
     */
    public static  void test1(){
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        System.out.println( blockingQueue.add("a") );
        System.out.println( blockingQueue.add("b") );
        System.out.println( blockingQueue.add("c") );

        //IllegalStateException: Queue full 抛出异常
//        System.out.println( blockingQueue.add("d") );
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());

        //NoSuchElementException 抛出异常
        System.out.println(blockingQueue.remove());
    }
```

```java
    /**
     *有返回值没有异常
     */
    public static void test2(){
        //队列大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));
        //false 不抛出异常
        System.out.println(blockingQueue.offer("c"));


        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        //不抛出异常 null
        System.out.println(blockingQueue.poll());
    }
```

```java
    /**
     * 等待，阻塞（一直阻塞）
     */
    public static void test3() throws InterruptedException {
        //队列大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

        //一直阻塞
        blockingQueue.put("a");
        blockingQueue.put("b");
        blockingQueue.put("c");
        //队列没有位置，一直阻塞
//        blockingQueue.put("d");
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        //没有这个元素，一直阻塞
        System.out.println(blockingQueue.take());
    }
```

```java
 /**
     * 等待，阻塞（等待超时）
     */
    public static void test4() throws InterruptedException {
        //队列大小
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));
        System.out.println(blockingQueue.offer("d", 2, TimeUnit.SECONDS));

        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll(2, TimeUnit.SECONDS));
    }
```

> SynchronousQueue 同步队列

没有容量：进去一个元素，必须等待取出来之后，才能再往里放一个元素

```java
/**
 * 同步队列
 * 和其他的BlockingQueue不一样，SynchronousQueue不存储元素
 * put了一个元素，必须先从里面take取出来，否则不能在put进去值～
 */
public class SynchronousQueueDemo {
    public static void main(String[] args) {
        BlockingQueue<String> blockingQueue = new SynchronousQueue<>();//同步队列

        new Thread(()->{
            try {
                System.out.println(Thread.currentThread().getName()+" put 1");
                blockingQueue.put("1");
                System.out.println(Thread.currentThread().getName()+" put 2");
                blockingQueue.put("2");
                System.out.println(Thread.currentThread().getName()+" put 3");
                blockingQueue.put("3");
            } catch (Exception e) {
                e.printStackTrace();
            }
        },"T1").start();

        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+blockingQueue.take());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"T2").start();
    }
}
//学了技术不会用？看得少！
```



## 11.线程池

线程池：3大方法、7大参数、4种拒绝策略

> 池化技术

程序的运行，本质：占用系统的资源！优化CPU资源的使用！=>池化技术

- 线程池、连接池、内存池、对象池....创建和销毁十分浪费资源

池化技术：事先准备好一些资源，有人要用，就来这里拿，用完之后还。



**线程池的好处：**

1. 降低资源的消耗
2. 提高响应的效率
3. 方便管理

- **线程复用、控制最大并发数、管理线程**



> 线程池：三大方法

![tlbwk9.png](https://s1.ax1x.com/2020/05/31/tlbwk9.png)

```java
//Executors 工具类，3大方法
public class Demo01 {
    public static void main(String[] args) {
//        ExecutorService threadPool = Executors.newSingleThreadExecutor();//单个线程
//        ExecutorService threadPool = Executors.newFixedThreadPool(5);//创建一个固定的线程池的大小
        ExecutorService threadPool = Executors.newCachedThreadPool();//可伸缩的，遇强则强，遇弱则弱

        try {
            for (int i = 0; i < 100; i++) {
                //使用了线程池之后，要用线程池来创建变量
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown();
        }
    }
}
```

> 七大参数

源码分析：

```java
public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
}

public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
}

 public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,//21亿 oom溢出
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
}

//本质：ThreadPoolExecutor()

public ThreadPoolExecutor(int corePoolSize,// 核心线程池大小
                              int maximumPoolSize,//最大核心线程池大小
                              long keepAliveTime,//超时了没有人调用就会释放
                              TimeUnit unit,//超时单位
                              BlockingQueue<Runnable> workQueue,//线程工厂，创建线程的，一般不用动
                              RejectedExecutionHandler handler)//拒绝策略 {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), handler);
    }
```

![t1eYjK.png](https://s1.ax1x.com/2020/05/31/t1eYjK.png)

> 手动创建一个线程池
>
> - **有时间查看阿里巴巴开发手册**

```java
//Executors 工具类，3大方法
public class Demo01 {
    public static void main(String[] args) {
//        ExecutorService threadPool = Executors.newSingleThreadExecutor();//单个线程
//        ExecutorService threadPool = Executors.newFixedThreadPool(5);//创建一个固定的线程池的大小
//        ExecutorService threadPool = Executors.newCachedThreadPool();//可伸缩的，遇强则强，遇弱则弱

        ExecutorService threadPool = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.DiscardOldestPolicy());
        /**
         * AbortPolicy 银行满了，还有人进来，不处理这个人的，抛出异常
         * CallerRunsPolicy哪来的回哪里去
         * DiscardPolicy 队列满了，不会抛出异常，丢掉任务
         * DiscardOldestPolicy 队列满了，尝试去和最早的竞争，成功执行，否则丢弃，也不会抛弃异常
         */


        try {
            //最大承载：LinkedBlockingDeque.size()+maxPoolSize
            //超载：RejectedExecutionException
            for (int i = 1; i <= 10; i++) {
                //使用了线程池之后，要用线程池来创建变量
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            threadPool.shutdown();
        }
    }
}
```

> 四种拒绝策略

![t1eo3q.png](https://s1.ax1x.com/2020/05/31/t1eo3q.png)

 ```java
/**
* AbortPolicy 银行满了，还有人进来，不处理这个人的，抛出异常
* CallerRunsPolicy哪来的回哪里去
* DiscardPolicy 队列满了，不会抛出异常，丢掉任务
* DiscardOldestPolicy 队列满了，尝试去和最早的竞争，成功执行，否则丢弃，也不会抛弃异常
*/
 ```

> 小结和拓展

池的大小如何定义

了解：IO密集型，CPU密集型：（调优）

```java
/**
* 最大线程到底该如何定义
* 1.CPU 密集型 12条线程同时执行，几核，就是几，可以保持CPU的效率最高
*      sout(Runtime.getRuntime().availableProcessors())--》获取核数
* 2.IO 密集型 判断程序中国呢十分耗IO的线程，IO线程总数*2
*      程序 15个大型任务 io十分占资源
*/
```



## 12.四大函数式接口(重点)

新时代程序员：lambda表达式、链式编程、函数式接口、Stream流式计算

> 函数式接口：只有一个方法的接口 

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
//超级多FunctionalInterface
//简化编程模型，在新版本的框架底层大量应用
//foreach(消费者类型的函数式接口)
```

![t139Bt.png](https://s1.ax1x.com/2020/05/31/t139Bt.png)

代码测试：

> Function函数式接口

![t1R0dP.png](https://s1.ax1x.com/2020/05/31/t1R0dP.png)

```java
/**
 * Function函数型接口,有一个输入参数，有一个输出参数
 * 只要是函数型接口 可以用 lambda表达式
 */
public class Demo01 {
    public static void main(String[] args) {
        //工具类：输出输入的值
//        Function function = new Function<String,String>() {
//            @Override
//            public String apply(String s) {
//                return s;
//            }
//        };

        Function function = (str)->{return str;};
        System.out.println(function.apply("add"));

    }
}
```

> 断定型接口：有一个输入参数，返回值只能是布尔值

```java
/**
 * 断定型接口：有一个输入参数，返回值只能是布尔值
 */
public class Demo02 {
    public static void main(String[] args) {
//        Predicate<String> predicate = new Predicate<String>() {
//            //判断字符串是否为空
//            @Override
//            public boolean test(String s) {
//                return s.isEmpty();
//            }
//        };
        Predicate<String> predicate =(str)->{
            return str.isEmpty();
        };
        System.out.println(predicate.test(""));
    }
}
```

> Comsumer 消费型接口

```java
/**
 * 消费型接口：只有输入，没有返回值
 */
public class Demo03 {
    public static void main(String[] args) {
//        Consumer<String> consumer = new Consumer<String>(){
//            @Override
//            public void accept(String s) {
//                System.out.println(s);
//            }
//        };
        Consumer<String> consumer = (str)->{
            System.out.println(str);
        };
        
        consumer.accept("adjaklsdj");
    }
}
```

> Supplier 供给型接口

```java
/**
 * 供给型接口：没有参数，只有返回值
 */
public class Demo04 {
    public static void main(String[] args) {
//        Supplier<Integer> supplier = new Supplier<Integer>() {
//            @Override
//            public Integer get() {
//                System.out.println("launch get()");
//                return 1024;
//            }
//        };
        Supplier<Integer> supplier = ()->{return 1024;};
        System.out.println(supplier.get());
    }
}
```

## 13.流式计算Stream

> 什么是Stream流式计算

大数据：存储+计算

集合、MySQL本质就是存储东西的

计算机都应该交给流来操作

```java
/**
 * 一分钟完成此题
 * id为2的倍数
 * 选出大于等于20
 * 倒序排列
 * 只要一个
 */
public class Test {
    public static void main(String[] args) {
        User u1 = new User(1,"a",21);
        User u2 = new User(2,"b",22);
        User u3 = new User(3,"c",23);
        User u4 = new User(4,"d",24);
        User u5 = new User(5,"e",25);
        //集合就是存储
        List<User> users = Arrays.asList(u1, u2, u3, u4, u5);

        //计算交给Stream流
        //lambda表达式，链式编程，函数式接口，Stream流式计算
        users.stream()
                .filter(u->{return u.getId()%2==0;})
                .filter(u->{return u.getAge()>20;})
                .map(u->{return u.getName().toUpperCase();})
                .sorted((a,b)->{return  b.compareTo(a);})
                .limit(1)
                .forEach(System.out::println);
    }
}
```

- **Stream需要扩展写一篇**



## 14.ForkJoin

> 什么是ForkJoin

ForkJoin在JDK1.7，并行执行任务！提高效率，大数据量！

大数据：Map Reduce（**把大任务拆分成小任务**） 

![t1bmbF.png](https://s1.ax1x.com/2020/05/31/t1bmbF.png)

> ForkJoin特点：工作窃取

这个里面维护的都是双端队列

![t1v32d.png](https://s1.ax1x.com/2020/05/31/t1v32d.png)

> ForkJoin

![t1xnQs.png](https://s1.ax1x.com/2020/05/31/t1xnQs.png)

![t1xGYF.png](https://s1.ax1x.com/2020/05/31/t1xGYF.png)

```java
/**
 * 求和计算的任务！
 * 3 6 9等
 * 如何使用forkJoin
 * 1.forkjoinPool 通过他来执行
 * 2.计算任务 forkjoinPool.execute(ForkJoinTask task)
 * 3.计算类要继承ForkJoinTask
 */
public class ForkJoinDemo extends RecursiveTask<Long> {
    private Long start;
    private Long end;
    //临界值
    private Long temp = 10000L;

    public ForkJoinDemo(Long start, Long end) {
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {
        if ((end-start)<temp){//递归
            Long sum = 0L;
            for (Long i = start; i <= end; i++) {
                sum += i;
            }
            return sum;
        }else{
            //分支合并计算
            long middle =  (start+end)/2;
            ForkJoinDemo task1 = new ForkJoinDemo(start, middle);
            task1.fork();//拆分任务，把任务压入线程队列
            ForkJoinDemo task2 = new ForkJoinDemo(middle+1, end);
            task2.fork();//拆分任务，把任务压入线程队列

            return task1.join()+task2.join();
        }
    }
}
```

```java
public class Test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
//        test1();//sum=时间3367
//        test2();//sum=时间4769
//        test3();//时间212

    }

    //普通程序员
    public static void test1(){
        Long sum = 0L;
        long start = System.currentTimeMillis();

        for (long i = 1L; i < 10_0000_0000L; i++) {
            sum+=i;
        }

        long end = System.currentTimeMillis();
        System.out.println("sum="+"时间"+(end-start));
    }

    //使用forkJoin
    public static void test2() throws ExecutionException, InterruptedException {
        long start = System.currentTimeMillis();

        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Long> task = new ForkJoinDemo(0L, 10_0000_0000L);
        ForkJoinTask<Long> submit = forkJoinPool.submit(task);//提交任务
        Long sum = submit.get();

        long end = System.currentTimeMillis();
        System.out.println("sum="+"时间"+(end-start));
    }

    public static void test3(){
        long start = System.currentTimeMillis();

        //Stream并行流()
        LongStream.rangeClosed(0L,10_0000_0000L).parallel().reduce(0,Long::sum);

        long end = System.currentTimeMillis();
        System.out.println("sum="+"时间"+(end-start));
    }
}
```

## 15.异步回调

> Future设计初衷：对将来的某个事件的结果进行建模

![t3pwmn.png](https://s1.ax1x.com/2020/05/31/t3pwmn.png)

```java
/**
 * 异步调用：CompletableFuture
 * 异步执行
 * 成功回调
 * 失败回调
 */
public class Demo01 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //发起一个请求
        //没有返回值的runAsync异步回调
//        CompletableFuture<Void> completableFuture = CompletableFuture.runAsync(()->{
//            try{
//                TimeUnit.SECONDS.sleep(0);
//            } catch (InterruptedException e) {
//                e.printStackTrace();
//            }
//            System.out.println(Thread.currentThread().getName()+"Void has been over");
//        });
//        System.out.println("test1");
//        completableFuture.get();// 获取，会阻塞，返回执行结果
//        System.out.println("test2");

        //有返回值的supplyAsync异步回调
        //ajax，有成功和失败回调
        //失败时返回的是错误信息；
        CompletableFuture<Integer> supplyAsync = CompletableFuture.supplyAsync(() -> {
            System.out.println(Thread.currentThread().getName()+"supplyAsync has been over");
            int i = 10/0;
            return 1024;
        });

        System.out.println(supplyAsync.whenComplete((T, U) -> {
            System.out.println("T = " + T);//正常的返回结果
            System.out.println("U = " + U);//错误信息java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
        }).exceptionally((e) -> {
            System.out.println(e.getMessage());
            return 500;//可以获得到返回结果
        }).get());
    }
}
```

## 16.JMM

> 请你谈谈对Volatile的理解

Volatile是Java虚拟机提供**轻量级的同步机制**

1. 保证可见性
2. 不保证原子性
3. 禁止指令重排

> 什么是JMM

JMM：java内存模型，不存在的东西，就是一个概念

**关于JMM的一些同步约定**：

1. 线程解锁前，必须把共享变量**立刻**刷回主存
2. 线程加锁前，必须读取主存中的最新值到工作内存中
3. 加锁和解锁是同一把锁

线程：**工作内存/主内存**

8种操作：

![t3in4P.png](https://s1.ax1x.com/2020/05/31/t3in4P.png)

![t3ifgO.png](https://s1.ax1x.com/2020/05/31/t3ifgO.png)

　内存交互操作有8种，虚拟机实现必须保证每一个操作都是原子的，不可在分的（对于double和long类型的变量来说，load、store、read和write操作在某些平台上允许例外）

- lock   （锁定）：作用于主内存的变量，把一个变量标识为线程独占状态

- unlock （解锁）：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定
- read  （读取）：作用于主内存变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的load动作使用
- load   （载入）：作用于工作内存的变量，它把read操作从主存中变量放入工作内存中
- use   （使用）：作用于工作内存中的变量，它把工作内存中的变量传输给执行引擎，每当虚拟机遇到一个需要使用到变量的值，就会使用到这个指令
- assign （赋值）：作用于工作内存中的变量，它把一个从执行引擎中接受到的值放入工作内存的变量副本中
- store  （存储）：作用于主内存中的变量，它把一个从工作内存中一个变量的值传送到主内存中，以便后续的write使用
- write 　（写入）：作用于主内存中的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中

　　**JMM对这八种指令的使用，制定了如下规则：**

- 不允许read和load、store和write操作之一单独出现。即使用了read必须load，使用了store必须write

- 不允许线程丢弃他最近的assign操作，即工作变量的数据改变了之后，必须告知主存
- 不允许一个线程将没有assign的数据从工作内存同步回主内存
- 一个新的变量必须在主内存中诞生，不允许工作内存直接使用一个未被初始化的变量。就是怼变量实施use、store操作之前，必须经过assign和load操作
- 一个变量同一时间只有一个线程能对其进行lock。多次lock后，必须执行相同次数的unlock才能解锁
- 如果对一个变量进行lock操作，会清空所有工作内存中此变量的值，在执行引擎使用这个变量前，必须重新load或assign操作初始化变量的值
- 如果一个变量没有被lock，就不能对其进行unlock操作。也不能unlock一个被其他线程锁住的变量
- 对一个变量进行unlock操作之前，必须把此变量同步回主内存

问题：程序不知道主内存的值已经被修改过了

![t3FyQS.png](https://s1.ax1x.com/2020/05/31/t3FyQS.png)

## 17.Volatile

> 1.保证可见性

```java
package xyz.kidjaya.tvolatile;

import java.util.concurrent.TimeUnit;

public class JMMDemo {
    //不加 volatile 程序就会死循环
    //加 volatile 可以保证可见性
    private volatile static int num = 0;
    public static void main(String[] args) {//main线程
        new Thread(()->{//线程1
            while (num == 0){

            }
        }).start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        num = 1;
        System.out.println(num);
    }
}
```

> 2.不保证原子性

原子性：不可分割

线程A在执行任务的时候，不能被打扰的，也不能被分割。要么同时成功，要么同时失败

```java
public class VDemo02 {
    //volatile不保证原子性
    private volatile static int num = 0;

    public static void add(){
        num++;
    }

    public static void main(String[] args) {

        //理论上Number为2w
        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }).start();
        }

        while(Thread.activeCount()>2){//main gc
            Thread.yield();
        }

        System.out.println(Thread.currentThread().getName()+" "+num);
    }
}
```

如果不加lock和synchronized如何保证原子性

![t3e2eU.png](https://s1.ax1x.com/2020/05/31/t3e2eU.png)

使用原子类解决原子性问题

![t3mSSI.png](https://s1.ax1x.com/2020/05/31/t3mSSI.png)

> 原子类为什么这么高级 0v0

```java
package xyz.kidjaya.tvolatile;

import java.util.concurrent.atomic.AtomicInteger;

public class VDemo02 {
    //volatile不保证原子性
    private volatile static AtomicInteger num = new AtomicInteger();

    public static void add(){
//        num++;//不是一个原子性操作
        num.getAndIncrement();//AtomicInteger +1 方法 CAS CPU并发原理 后期深入学习CS扩展
    }

    public static void main(String[] args) {

        //理论上Number为2w
        for (int i = 1; i <= 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }).start();
        }

        while(Thread.activeCount()>2){//main gc
            Thread.yield();
        }

        System.out.println(Thread.currentThread().getName()+" "+num);
    }
}
```

这些类的底层都直接和操作系统挂钩。在内存中修改值！Unsafe类是一个很特殊的存在

> 3.禁止指令重排

什么是指令重排：**你写的程序，计算机并不是按照你写的那样去执行的**

源代码---> 编译器优化重排--->指令并行也可能会重排--->内存系统也会重排--->执行

**处理器在进行相关指令重排的时候，考虑：数据之间的依赖性！**

```java
int x = 1; //语句1
int y = 2; //语句2
x = x + 5; //语句3
y = x * x; //语句4

我们所期望的：1234 但是可能执行的时候变成  2134  1324   
可不可能是 4123 不可能
```

可能造成影响的结果:a,b,x,y 默认为0

| 线程a | 线程B |
| ----- | ----- |
| x=a   | y=b   |
| b=1   | a=2   |

正常结果 x=0,y=0;但是可能由于指令重排改变执行顺序

| 线程a | 线程B |
| ----- | ----- |
| b=1   | a=2   |
| x=a   | y=b   |

指令重排导致的诡异结果：x=2,y=1;

> 非计算机专业

**volatile可以避免指令重排**：

内存屏障：CPU指令

1. 保证特定的操作的执行顺序
2. 可以保证某些变量的内存可见性（利用这些特性volatile实现了可见性）

![t3MM8J.png](https://s1.ax1x.com/2020/05/31/t3MM8J.png)

**volatile是可以保证可见性，不能保证原子性，由于内存屏障可以避免指令重排的现象产生！**

## 18.彻底玩转单例模式

饿汉式，DCL懒汉式，深究

> 饿汉式

```java
//饿汉式单例模式
public class Hungry {

    //可能浪费空间
    private byte[] data1 = new byte[1024*1024];
    private byte[] data2 = new byte[1024*1024];
    private byte[] data3 = new byte[1024*1024];
    private byte[] data4 = new byte[1024*1024];

    private Hungry(){

    }

    private final static Hungry HUNGRY = new Hungry();

    public static Hungry getInstance(){
        return  HUNGRY;
    }
}
```

> 懒汉式

```java
//懒汉式单例模式
//道高一尺，魔高一丈
public class LazyMan {

    private static boolean flag = false;

    private  LazyMan(){
        synchronized (LazyMan.class){
            if (flag == false){
                flag = true;
            }else{
                throw new RuntimeException("不要试图使用反射破坏单例模式");
            }
            if (lazyMan!=null){
                throw new RuntimeException("不要试图使用反射破坏单例模式");
            }
        }
        System.out.println(Thread.currentThread().getName()+"ok");
    }

    private volatile static LazyMan lazyMan;

    //双重检测锁模式 懒汉式单例 DCL懒汉式
    public static LazyMan getInstance(){
        if (lazyMan == null){
            synchronized (xyz.kidjaya.single.LazyMan.class){
                if (lazyMan == null){
                    lazyMan = new LazyMan();//不是原子性操作
                    /**
                     * 1.分配内存空间
                     * 2.执行构造方法，初始化对象
                     * 3.把这个对象指向这个空间
                     *
                     * 执行123
                     * 重排132 A
                     *        B//此时return 的 LazyMan还没有完成构造
                     */
                }
            }
        }
        return lazyMan;
    }

    //单线程下没问题
    //并发下有问题

//    public static void main(String[] args) {
//        for (int i = 0; i < 10; i++) {
//            new Thread(()->{
//                LazyMan.getInstance();
//            }).start();
//        }
//    }

    //反射破解
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException, NoSuchFieldException {
//        LazyMan instance = LazyMan.getInstance();

        Field flag = LazyMan.class.getDeclaredField("flag");

        Constructor<LazyMan> declaredConstructor = LazyMan.class.getDeclaredConstructor(null);
        LazyMan instance = declaredConstructor.newInstance();

        flag.set(instance,false);

        LazyMan instance2 = declaredConstructor.newInstance();
        System.out.println(instance);
        System.out.println(instance2);
    }
}
```

> 静态内部类

```java
//静态内部类
public class Holder {
    private Holder(){

    }

    public static Holder getInstance(){
        return InnerClass.HOLDER;
    }

    public static class InnerClass{
        private static final Holder HOLDER = new Holder();
    }
}
```

> 单例不安全，因为有反射

> 枚举

```java
//enum 是一个什么？本身也是一个Class类
public enum EnumSingle {
    INSTANCE;

    public EnumSingle getInstance(){
        return INSTANCE;
    }
}

class Test{
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        EnumSingle instance1 = EnumSingle.INSTANCE;
        Constructor<EnumSingle> declaredConstructor = EnumSingle.class.getDeclaredConstructor(String.class,int.class);
        declaredConstructor.setAccessible(true);
        EnumSingle instance2 = declaredConstructor.newInstance();

        System.out.println(instance1);
        System.out.println(instance2);
    }
}
```

![t3NjAA.png](https://s1.ax1x.com/2020/05/31/t3NjAA.png)

枚举的最终反编译源码：

![t3UCjS.png](https://s1.ax1x.com/2020/05/31/t3UCjS.png)

## 19.深入理解CAS

> 什么是CAS

大厂：必须要深入研究底层！有所突破！**修内功*：操作系统，计算机网络原理*

```java
public class CASDemo {

    //CAS compareAndSet：比较并交换
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);
        //public final boolean weakCompareAndSet(int expect, int update)
        //期望，更新
        //如果期望的值达到了，就更新，否则就不更新,CAS是计算机的并发原语法
        atomicInteger.weakCompareAndSet(2020,2021);
        System.out.println(atomicInteger.get());
        atomicInteger.getAndIncrement();

        System.out.println(atomicInteger.weakCompareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());
    }
}
```

> Unsafe类

![t3a5QK.png](https://s1.ax1x.com/2020/05/31/t3a5QK.png)

![t3dtOO.png](https://s1.ax1x.com/2020/05/31/t3dtOO.png)

![t3d7cV.png](https://s1.ax1x.com/2020/05/31/t3d7cV.png)

CAS：比较当前工作内存中的值和主内存的值，如果这个值是期望的，那么则执行操作！如果不是就一直循环（自旋锁）

缺点：

1. 循环会耗时
2. 一次性只能保证一个共享变量的原子性
3. 会存在ABA问题



> CAS:  ABA问题（狸猫换太子）

![t30QR1.png](https://s1.ax1x.com/2020/05/31/t30QR1.png)

```java
public class CASDemo {

    //CAS compareAndSet：比较并交换
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);
        //public final boolean weakCompareAndSet(int expect, int update)
        //期望，更新
        //如果期望的值达到了，就更新，否则就不更新,CAS是计算机的并发原语法
        System.out.println(atomicInteger.weakCompareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());

      
        System.out.println(atomicInteger.weakCompareAndSet(2021, 2020));
        System.out.println(atomicInteger.get());

      
        System.out.println(atomicInteger.weakCompareAndSet(2020, 6666));
        System.out.println(atomicInteger.get());
    }
}
```

## 20.原子引用

> 解决ABA问题，引入原子引用，带版本号的操作-->对象的内存地址

带版本号的原子操作

![t3sRBV.png](https://s1.ax1x.com/2020/05/31/t3sRBV.png)

 ```java
public class CASDemo {
    //CAS compareAndSet：比较并交换
    public static void main(String[] args) {
        //AtomicStampedReference 注意，如果范型是一个包装类，注意对象的引用问题
        AtomicStampedReference<Integer> atomicReference = new AtomicStampedReference<>(1,1);
        //正常的业务操作，这里面比较都是一个个对象，比如User
        new Thread(()->{
            int stamp = atomicReference.getStamp();//获得版本号
            System.out.println("a1=>"+stamp);

            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(atomicReference.compareAndSet(1, 2, atomicReference.getStamp(), atomicReference.getStamp() + 1));
            System.out.println("a2=>"+atomicReference.getStamp());

            System.out.println(atomicReference.compareAndSet(2, 1, atomicReference.getStamp(), atomicReference.getStamp() + 1));
            System.out.println("a3=>"+atomicReference.getStamp());

            },"a").start();

        //乐观锁原理获得
        new Thread(()->{
            int stamp = atomicReference.getStamp();//获得版本号
            System.out.println("b1=>"+stamp);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(atomicReference.compareAndSet(1, 6, stamp, stamp + 1));
            System.out.println("b2=>"+atomicReference.getStamp());

        },"b").start();
    }
}
 ```

## 21.各种锁的理解

### 1.公平锁、非公平锁

 公平锁：非常公平，不能够插队，必须先来后到

非公平锁：非常不公平，可以插队，可以允许一些插队（默认非公平）

```java
public ReentrantLock() {
        sync = new NonfairSync();
}

public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
}
```

### 2.可重入锁

可重入锁（递归锁）

![t36MJe.png](https://s1.ax1x.com/2020/05/31/t36MJe.png)

> Sychronized

```java
//Synchronized
public class Demo01 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            phone.sms();
        },"a").start();

        new Thread(()->{
            phone.sms();
        },"b").start();
    }
}

class Phone{
    public synchronized void sms(){
        System.out.println(Thread.currentThread().getName() + "sms");
        call();//这里也有锁
    }

    public synchronized void call(){
        System.out.println(Thread.currentThread().getName() + "call");
    }
}
```

> Lock

```java
//Lock
public class Demo02 {
    public static void main(String[] args) {
        Phone2 phone = new Phone2();
        new Thread(()->{
            phone.sms();
        },"a").start();

        new Thread(()->{
            phone.sms();
        },"b").start();
    }
}

class Phone2{
    Lock lock = new ReentrantLock();
    public void sms(){
        lock.lock();//lock获取了两个锁
        //锁必须配对
        try {
            System.out.println(Thread.currentThread().getName() + "sms");
            call();//这里也有锁
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void call(){
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "call");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

### 3.自旋锁

spinlock（不断尝试，直到成功为止）

![t3gZ4O.png](https://s1.ax1x.com/2020/05/31/t3gZ4O.png)

自定义一个锁测试

```java
/**
 * 自旋锁
 */
public class SpinlockDemo {
    //int 0
    //Thread null
    AtomicReference<Thread> atomicReference = new AtomicReference<>();//原子引用

    //加锁
    public void myLock(){
        Thread thread = Thread.currentThread();
        System.out.println(Thread.currentThread().getName()+"===> myLock");

        //自旋锁
        while(!atomicReference.compareAndSet(null,thread)){

        }
    }

    //解锁
    public void myUnLock(){
        Thread thread = Thread.currentThread();
        System.out.println(Thread.currentThread().getName()+"===> myLock");
        atomicReference.compareAndSet(thread,null);
    }

}
```

> 测试

```java
public class TestSpinLock {
    public static void main(String[] args) {
//        ReentrantLock lock = new ReentrantLock();
//        lock.lock();
//        lock.unlock();

        //底层使用自旋锁CAS
        SpinlockDemo spinlockDemo = new SpinlockDemo();

        new Thread(()->{
            spinlockDemo.myLock();
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                spinlockDemo.myUnLock();
            }
        },"T1").start();

        new Thread(()->{
            spinlockDemo.myLock();
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                spinlockDemo.myUnLock();
            }
        },"T2").start();
    }
}
```

![t8ibYq.png](https://s1.ax1x.com/2020/06/01/t8ibYq.png)

### 4.死锁

![t8FojO.png](https://s1.ax1x.com/2020/06/01/t8FojO.png)

死锁测试，怎么排出死锁

```java
public class DeadLockTest {
    public static void main(String[] args) {
        String lockA = "lockA";
        String lockB = "lockB";

        new Thread(new MyThread(lockA,lockB),"T1").start();
        new Thread(new MyThread(lockB,lockA),"T2").start();
        new Thread().start();
    }
}

class MyThread implements Runnable{

    private String lockA;
    private String lockB;

    public MyThread(String lockA, String lockB) {
        this.lockA = lockA;
        this.lockB = lockB;
    }

    @Override
    public void run() {
        synchronized (lockA){
            System.out.println(Thread.currentThread().getName()+"lock:"+lockA+"=> get"+lockB);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            synchronized (lockB){
                System.out.println(Thread.currentThread().getName()+"lock:"+lockB+"=> get"+lockA);
            }
        }
    }
}
```

> 解决问题

1. 使用jps定位进程号

![t8ECkj.png](https://s1.ax1x.com/2020/06/01/t8ECkj.png)

2. jstack 进程号 看堆栈信息

![t8EMN9.png](https://s1.ax1x.com/2020/06/01/t8EMN9.png)

3. 面试提问如何排查进程问题
   1. 查看日志
   2. 查看堆栈信息（如上）

**产生死锁的四个条件**

1. 互斥条件：一个资源每次只能被一个进程使用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放
3. 不剥夺条件：进程已获得的资源，在未使用完之前，不能强行剥夺
4. 循环等待条件：若干进程之间形成一种头尾相接的循环等待关系

