# JAVA中的线程

认识并发与并行

并发是针对一个CPU来讲，“同一时间”执行多个任务--实质上是划分时间片，由于时间很短给人的感觉是是同时执行的；
并行是针对多个CPU来讲的，同时执行多个任务（一个CPU执行一个），这是真正意义上的同时执行；

**入门程序分析（打印---HelloWorld）**

```java
class Car{
    public void drive(){
        System.out.println("我会跑");
    }
}

public class HuiXinThread {
    public static void main(String[] args) {
        Car car=null;
        System.out.println("HelloWorld");//主线程---main()
        car.drive();//异常处理的线程---
        new Car().drive();//运行完毕，new Car()匿名对象会被回收--垃圾收集器
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f04626e9ef4b4676a6b14462936d67f9.png)所以在我们初学java时，并非只有一个线程，其实是多线程的包括   主线程、处理异常的线程，以及垃圾收集器线程等等；

## 线程之抢占CPU资源

先来看一个案例----

![](C:\Users\Gavin\Pictures\Camera Roll\9.png)

当我们打开一个程序前，其实就是存放在电脑中的代码（静态的），打开之后，这个程序才算开启，（为他分配空间以及CPU资源），然后在这个程序中开启不同的功能--即开启了多个线程，然后CPU再次为这些线程服务，如果线程过多，CPU不够用，那就会使得一个CPU要执行多个线程---形成了并发（采用时间片机制），同时可以为线程设置优先级来提高相应线程被先执行的概率（并不是优先级低就一定会被后执行）

## 线程的开辟

线程的创建方式---
第一种---继承Thread类

```java
class Car extends  Thread{
    @Override
    public void run(){
        for (int i=0;i<10;i++){
        System.out.println("我会跑");}
    }
}

public class HuiXinThread {
    public static void main(String[] args) {
        //主线程---main()
        for (int i = 0; i <10 ; i++) {
            System.out.println("HelloWorld"+i);
        }
    new Car().run();

    }
}

```

这一种并不是多线程，因为该方式只是把run（）当成一个普通方法来执行的；，所以运行结果是 先执行main--主线程，然后执行run()，此时run也是属于主线程里面的；

![在这里插入图片描述](https://img-blog.csdnimg.cn/caeb286452dd4685bcc427aa7f7036db.png)

```java
class Car extends  Thread{
    @Override
    public void run(){
        for (int i=0;i<10;i++){
        System.out.println("我会跑");}
    }
}

public class HuiXinThread {
    public static void main(String[] args) {
        //主线程---main()
        for (int i = 0; i <10 ; i++) {
            System.out.println("HelloWorld"+i);
        }
    new Car().start();

    }
}

```

start()是父类的方法，但是与逆行结果依然跟上面一样---

![在这里插入图片描述](https://img-blog.csdnimg.cn/6195a0123dce47bc9caeaa82b0875ff5.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)原因是我们在主线程（for循环）执行时并没有子线程参与进来，要想实现并发效果需要调整一下start(）方法的位置--

```java
class Car extends  Thread{
    @Override
    public void run(){
        for (int i=0;i<10;i++){
        System.out.println("我会跑"+i);}
    }
}

public class HuiXinThread {
    public static void main(String[] args) {
        //子线程
        new Car().start();
        //主线程---main()
        for (int i = 0; i <10 ; i++) {
            System.out.println("HelloWorld"+i);
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9145df3672f64465993a8cdd6e5326b5.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 运行后发现---有时候并不一定会出现并发---当运行量少的时候；

**为线程设置名字和获取线程名字**

```java
public class Car extends  Thread{
    @Override
    public void run(){
        for (int i=0;i<10;i++){
            System.out.println(this.getName()+i);}
    }
}

public class HuiXinThread {
    public static void main(String[] args) {
        //子线程
            Car car= new Car();
            car.setName("小汽车");
        car.start();
        //主线程---main()
        for (int i = 0; i <10 ; i++) {
            System.out.println(Thread.currentThread().getName()+i);
        }
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8018216a7101479891c210510f6a0ea3.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
还可以通过构造器来设置线程名字---

```java
public class Car extends  Thread{
    public Car(String name){
        super(name);
    }

    @Override
    public void run(){
        for (int i=0;i<10;i++){
            System.out.println(this.getName()+i);}
    }
}


public class HuiXinThread {
    public static void main(String[] args) {
        //子线程
            Car car= new Car("小火车");
            //car.setName("小汽车");

        car.start();
        //主线程---main()
        for (int i = 0; i <10 ; i++) {
            System.out.println(Thread.currentThread().getName()+i);
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/55f6ba64fdd7420a9f09c06638b05af2.png)
设置线程名的父类构造方法---
![在这里插入图片描述](https://img-blog.csdnimg.cn/1d470a6200734ac0a083cb6a36a8df6e.png)

## 线程开辟的方式

### 继承Thread类--

演示代码如上，不在赘述----注意点为   开启线程是start()方法，而run()方法还是当作一个普通方法来处理的；

start()源码---

> 此方法不为主方法线程或“系统”调用。  
> 虚拟机创建/设置的组线程。 添加的新功能  
> 到这个方法以后可能还需要添加到VM中。

![在这里插入图片描述](https://img-blog.csdnimg.cn/f66733f94539419ea0f14108c96632e7.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

### 实现Runnable接口

```java
class BuyBook implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println("i---"+Thread.currentThread());
        }
    }
}

public class Test {
    public static void main(String[] args)  {
        BuyBook bb= new BuyBook();
        bb.run();
        for (int i = 0; i <10 ; i++) {
            System.out.println("i---"+Thread.currentThread());
        }

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/722edded37734ceda44008f97eb42cce.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> Runnable接口中只有一个run方法,

![在这里插入图片描述](https://img-blog.csdnimg.cn/582525a34919410288ed0d6c325ad153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

并没有发现start方法,需要怎么做呢?

start()方法在Thread类中,所以要先在创建一个Thread的对象

```java
class BuyBook implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 3; i++) {
            System.out.println("i---"+Thread.currentThread());
        }
    }
}

public class Test {
    public static void main(String[] args)  {
        Thread.currentThread().setName("主线程");
        BuyBook bb= new BuyBook();
        Thread thread= new Thread(bb);
        thread.setName("子线程");
        thread.start();
        for (int i = 0; i <3 ; i++) {
            System.out.println("i---"+Thread.currentThread());
        }

    }
}

```

> public Thread(Runnable target) {//构造器--传入的是Runnable对象
>       this(null, target, "Thread-" + nextThreadNum(), 0);
>   }



当处理量少的时候得到的结果像是顺序执行的,多运行几次就可以了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/d81146f50cd84580818ad686c696239d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 对比以上两种方式--- 
> 1,由于单继承的局限性,所以一般是用方式二多实现来开辟线程的'
> 2,实现接口的方式对于数据的共享性做了很好的实现----不需要static去修饰数据变量;

### 实现Callable接口

![在这里插入图片描述](https://img-blog.csdnimg.cn/b2ae70d733154fd5b576a9778be1fb9b.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/a4d445a078d84590b07c278e3086eed8.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
import java.util.Random;
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

class BuyBook implements Callable<Integer>{
    @Override
    public Integer call() throws Exception {
        return new Random().nextInt(100);
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        Thread.currentThread().setName("主线程");
//        开辟线程对象
        BuyBook bb= new BuyBook();
//          包装线程
        FutureTask ft= new FutureTask(bb);
        FutureTask ft1= new FutureTask(bb);
        FutureTask ft2= new FutureTask(bb);
//        开辟线程
       Thread td= new Thread(ft,"线程一");
       Thread td1= new Thread(ft1,"线程二");
       Thread td2= new Thread(ft2,"线程三");
//          启动线程
       td.start();
       td1.start();
       td2.start();
       //获得线程名
        System.out.println(td.getName()+"在运行");
        System.out.println(td1.getName()+"在运行");
        System.out.println(td2.getName()+"在运行");
        System.out.println("-----------线程返回值获取方式1---------------");
        System.out.println(ft.get());
        System.out.println(ft1.get());
        System.out.println(ft2.get());
        System.out.println("-----------线程返回值获取方式2---------------");
        System.out.println(bb.call());
        System.out.println(bb.call());
        System.out.println(bb.call());
        System.out.println("-------------主线程----------------");
        for (int i = 0; i <3 ; i++) {
            System.out.println("i---"+Thread.currentThread());
        }
    }
}

```

> 总结--- 
> 前两种开辟线程的方式没有返回值,不能抛异常,第三种很好的你不了前两种的的缺陷,但是开辟线程的过程相对前两种来讲不太简洁;

> 

@[TOC](JAVA中的线程)

为什么要有多线程与高并发，就让程序这么不受约束的运行不好吗？

**不受约束的运行--的后果**

```java
package HuiXin;

class Buytickets extends Thread {
    int ticket = 12;

    public Buytickets(String name) {
        super(name);
    }

    @Override
    public void run() {
        for (int i = 1; i < 100; i++) {

            if (ticket > 0) {

                System.out.println("我抢到了" + this.getName() + "卖的去往俄罗斯的第" + ticket-- + "张票");

            }
        }
    }
}

public class HuiXinThread {
    public static void main(String[] args) {
        Buytickets buytickets = new Buytickets("窗口1");
        Buytickets buytickets1 = new Buytickets("窗口2");
        Buytickets buytickets2 = new Buytickets("窗口3");
        buytickets.start();
        buytickets1.start();
        buytickets2.start();

    }
}



```

运行结果-----

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
我抢到了窗口3卖的去往俄罗斯的第12张票
我抢到了窗口3卖的去往俄罗斯的第11张票
我抢到了窗口3卖的去往俄罗斯的第10张票
我抢到了窗口3卖的去往俄罗斯的第9张票
我抢到了窗口1卖的去往俄罗斯的第12张票
我抢到了窗口1卖的去往俄罗斯的第11张票
我抢到了窗口1卖的去往俄罗斯的第10张票
我抢到了窗口1卖的去往俄罗斯的第9张票
我抢到了窗口1卖的去往俄罗斯的第8张票
我抢到了窗口1卖的去往俄罗斯的第7张票
我抢到了窗口1卖的去往俄罗斯的第6张票
我抢到了窗口1卖的去往俄罗斯的第5张票
我抢到了窗口1卖的去往俄罗斯的第4张票
我抢到了窗口1卖的去往俄罗斯的第3张票
我抢到了窗口1卖的去往俄罗斯的第2张票
我抢到了窗口1卖的去往俄罗斯的第1张票
我抢到了窗口2卖的去往俄罗斯的第12张票
我抢到了窗口2卖的去往俄罗斯的第11张票
我抢到了窗口2卖的去往俄罗斯的第10张票
我抢到了窗口2卖的去往俄罗斯的第9张票
我抢到了窗口2卖的去往俄罗斯的第8张票
我抢到了窗口2卖的去往俄罗斯的第7张票
我抢到了窗口2卖的去往俄罗斯的第6张票
我抢到了窗口2卖的去往俄罗斯的第5张票
我抢到了窗口2卖的去往俄罗斯的第4张票
我抢到了窗口2卖的去往俄罗斯的第3张票
我抢到了窗口2卖的去往俄罗斯的第2张票
我抢到了窗口2卖的去往俄罗斯的第1张票
我抢到了窗口3卖的去往俄罗斯的第8张票
我抢到了窗口3卖的去往俄罗斯的第7张票
我抢到了窗口3卖的去往俄罗斯的第6张票
我抢到了窗口3卖的去往俄罗斯的第5张票
我抢到了窗口3卖的去往俄罗斯的第4张票
我抢到了窗口3卖的去往俄罗斯的第3张票
我抢到了窗口3卖的去往俄罗斯的第2张票
我抢到了窗口3卖的去往俄罗斯的第1张票

```

我们明明只有12张票，却卖出了36张（因为每个窗口只卖自己的，卖出一张后没有做记录即数据没有共享，这明显不符合我们的真实目的；

![在这里插入图片描述](https://img-blog.csdnimg.cn/6de632ef04f742069d602e65484f9913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

那我们怎么办呢？

**共享数据--static**

解决方法---把票换成共享数据--static

```java
class Buytickets extends Thread {
    static int ticket = 12;

    public Buytickets(String name) {
        super(name);
    }

    @Override
    public void run() {
        for (int i = 1; i < 100; i++) {
            if (ticket > 0) {

                System.out.println("我抢到了" + this.getName() + "卖的去往俄罗斯的第" + ticket-- + "张票");
               
            }
        }
    }
}

public class HuiXinThread {
    public static void main(String[] args) {
        Buytickets buytickets = new Buytickets("窗口1");
        Buytickets buytickets1 = new Buytickets("窗口2");
        Buytickets buytickets2 = new Buytickets("窗口3");
        buytickets.start();
       buytickets1.start();
        buytickets2.start();

    }
}

```

运行结果

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
我抢到了窗口1卖的去往俄罗斯的第12张票
我抢到了窗口1卖的去往俄罗斯的第9张票
我抢到了窗口1卖的去往俄罗斯的第8张票
我抢到了窗口1卖的去往俄罗斯的第7张票
我抢到了窗口3卖的去往俄罗斯的第10张票
我抢到了窗口1卖的去往俄罗斯的第6张票
我抢到了窗口1卖的去往俄罗斯的第4张票
我抢到了窗口1卖的去往俄罗斯的第3张票
我抢到了窗口1卖的去往俄罗斯的第2张票
我抢到了窗口1卖的去往俄罗斯的第1张票
我抢到了窗口2卖的去往俄罗斯的第11张票
我抢到了窗口3卖的去往俄罗斯的第5张票
```

但是 仍然是卖的票多了，（当我们卖最后一张票（第12张的）时候，三个线程同时读取了ticket=11时的状态，然后都开始卖最后一张票--12，也都卖出去了）实际也没有这么多票；问题处在哪里呢？那怎么办呢？

**做一下同步**

解决方法----卖完一张票然后才能卖下一张票------我需要将ticket--做一下同步；

```java
package HuiXin;

class Buytickets extends Thread {
    static int ticket = 12;

    public Buytickets(String name) {
        super(name);
    }

    @Override
    public void run() {
        synchronized (this) {//同步代码块
            for (int i = 1; i < 100; i++) {

                if (ticket > 0) {

                    System.out.println("我抢到了" + this.getName() + "卖的去往俄罗斯的第" + ticket-- + "张票");

                }
            }
        }
    }
}

public class HuiXinThread {
    public static void main(String[] args) {
        Buytickets buytickets = new Buytickets("窗口1");
        Buytickets buytickets1 = new Buytickets("窗口2");
        Buytickets buytickets2 = new Buytickets("窗口3");
        buytickets.start();
        buytickets1.start();
        buytickets2.start();

    }
}

```

结果--

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
我抢到了窗口2卖的去往俄罗斯的第11张票
我抢到了窗口2卖的去往俄罗斯的第9张票
我抢到了窗口2卖的去往俄罗斯的第8张票
我抢到了窗口2卖的去往俄罗斯的第7张票
我抢到了窗口2卖的去往俄罗斯的第6张票
我抢到了窗口2卖的去往俄罗斯的第5张票
我抢到了窗口2卖的去往俄罗斯的第4张票
我抢到了窗口2卖的去往俄罗斯的第3张票
我抢到了窗口2卖的去往俄罗斯的第2张票
我抢到了窗口2卖的去往俄罗斯的第1张票
我抢到了窗口3卖的去往俄罗斯的第10张票
我抢到了窗口1卖的去往俄罗斯的第12张票
```

正好将十二张票卖完；

![在这里插入图片描述](https://img-blog.csdnimg.cn/a0b891a8a6454f78aa8d707d5ceda9d2.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)如果这样的同步代码块太多，程序执行效率会大大降低；

线程的生命周期

![在这里插入图片描述](https://img-blog.csdnimg.cn/4c0aefa0312b4f3aabe6d57c4d9121da.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



### 线程的优先级

> 在java中如果不设置优先级，则默认优先级为5，入过设置某线程的优先级很高，那么一定会先执行该线程吗？



话不多说，看结果---

```java
package HuiXin;

class Book implements Runnable {
    public void run() {

            for (int i = 1; i < 10; i++) {
    System.out.println(Thread.currentThread().getName()+"-----"+i);
            }
    }
}
class BookSchool implements Runnable {
    public void run() {

        for (int i = 11; i < 20; i++) {
            System.out.println(Thread.currentThread().getName()+"-----"+i);
        }
    }
}

public class HuiXinThread {
    public static void main(String[] args) throws Exception {
        Book book = new Book();
        Thread th = new Thread(book,"线程一");
        th.setPriority(1);
        th.start();
      BookSchool bookSchool= new BookSchool();
Thread th2= new Thread(bookSchool,"线程二");
th2.setPriority(10);
th2.start();
//再来主线程
        System.out.println(Thread.currentThread().getName()+"优先级---"+Thread.currentThread().getPriority());
    }
}

```

> Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
> 线程二-----11
> 线程二-----12
> 线程二-----13
> main优先级---5
> 线程二-----14
> 线程二-----15
> 线程二-----16
> 线程二-----17
> 线程二-----18
> 线程二-----19
> 线程一-----1
> 线程一-----2
> 线程一-----3
> 线程一-----4
> 线程一-----5
> 线程一-----6
> 线程一-----7
> 线程一-----8
> 线程一-----9

Process finished with exit code 0

main--主线程优先级为5，但是在优先级10面前也执行了，这说明线程优先级只是提高了该线程优先执行的概率，但并不是百分之百它先被执行；



**join（）方法的用法---**

```java
package HuiXin;

class Book implements Runnable {
    public void run() {

        for (int i = 1; i < 10; i++) {
            System.out.println(Thread.currentThread().getName() + "-----" + i);
            if (i == 6) {
                BookSchool bookSchool = new BookSchool();
                Thread th2 = new Thread(bookSchool, "线程二");
                th2.start();
                try {
                    th2.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

class BookSchool implements Runnable {
    public void run() {
        for (int i = 11; i < 20; i++) {
            System.out.println(Thread.currentThread().getName() + "-----" + i);
        }
    }
}

public class HuiXinThread {
    public static void main(String[] args) throws Exception {
        Book book = new Book();
        Thread th = new Thread(book, "线程一");
        th.setPriority(1);
        th.start();

//再来主线程
        System.out.println(Thread.currentThread().getName() + "优先级---" + Thread.currentThread().getPriority());
    }
}

```

> join方法的用法是先开启线程，然后在某个线程A执行时用join将另一个线程B添加进来，当B执行完毕之后才开始执行A，即原来的A 处于阻塞状态；

> Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
> main优先级---5
> 线程一-----1
> 线程一-----2
> 线程一-----3
> 线程一-----4
> 线程一-----5
> 线程一-----6
> 线程二-----11
> 线程二-----12
> 线程二-----13
> 线程二-----14
> 线程二-----15
> 线程二-----16
> 线程二-----17
> 线程二-----18
> 线程二-----19
> 线程一-----7
> 线程一-----8
> 线程一-----9

休眠方法的作用

```java
package setTest;
import java.text.Format;
import java.text.SimpleDateFormat;
import java.util.Date;
public class Test {
    public static void main(String[] args) throws Exception {
        System.out.println("秒表功能---");
        Format dateFormat = new SimpleDateFormat("HH:mm:ss");
        while (true) {
            Date d = new Date();
            String format = dateFormat.format(d);
            System.out.println(format);

            Thread.sleep(1000);
        }
    }
}

```

定时器,一些家用电器的定时都可以用到





### 线程安全

> 写代码要去解决实际问题,同时要尽可能的去避免漏洞的产生,这才是一段好代码;

那么在写代码的时候我们要明白代码的漏洞在哪里或者代码的不合理部分在哪里,然后针对不合理的部分进行相关操作来确保完成实际问题;

> 注------(^(*￣(oo)￣)^---图片文字由于截图没法修改了,统一把"票了"改为"第"),后续不再提及;

下面以我们入门学习的买票问题进行分析----

数据没有共享的情况下---

```java
class BuyTickets extends Thread {
      int ticketnum = 8;//现在只剩8张票了,
    //假设现在有三个窗口同时买票,每个窗口有200人在排队抢票
    public BuyTickets(String name) {
        super(name);
    }
    @Override
    public void run() {
        for (int i = 1; i < 200; i++) {
            while (ticketnum > 0) {//只要有票,就开始抢
                System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
            }
        }
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets("窗口1");
        BuyTickets bt1 = new BuyTickets("窗口2");
        BuyTickets bt2 = new BuyTickets("窗口3");
        //开启窗口卖票
       bt.start();
       bt1.start();
       bt2.start();
    }
}
```

> 这个时候,相当于于分别给每个窗口下达了一个指令----还剩8张票,赶紧卖,但是每个窗口之间没有相互联系,各自卖自己的票,于是总共卖出24张票,但实际没有这么多票------原因就是没有专门人员负责记录票卖出去的数量;

数据共享的情况下---

```java
class BuyTickets implements Runnable {
 static  int ticketnum = 8;//现在只剩8张票了,
    //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票

    @Override
    public void run() {

        for (int i = 1; i < 20; i++) {
            while (ticketnum > 0) {//只要有票,就开始抢
                System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
            }
        }
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th2 = new Thread(bt, "窗口2");
        Thread th1 = new Thread(bt, "窗口3");
        th.start();
        th1.start();
        th2.start();
    }
}
```

虽然卖出去的票相比没有共享的情况下少了,但是仍然会出现意外情况--
会出现某票被重复卖出的情况------运行多次会出现这种情况
![在这里插入图片描述](https://img-blog.csdnimg.cn/a7eaefccd94f426ab1c0ca9ae2453602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)说明我在窗口3卖第8张票的时候还没卖出去,就被窗口1抢走了,最后导致我们都把第八张票卖出去了;这也不符合实际情况;
那么这个时候怎么办呢?
一张一张的放票,这张没卖出去,下一张不能卖---即给票上一把🔒
在那里上锁呢?----会出现问题的代码处及进行加锁,但是上什么样的锁呢?

并行锁与并发锁?(可能专业不这么叫,但是道理是一样的吧---)
我们来看一下第一种情况

```java
    class BuyTickets extends Thread {
        static int ticketnum = 8;//现在只剩8张票了,
        //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票
        public BuyTickets(String name) {
            super(name);
        }
        @Override
        public void run() {

            for (int i = 1; i < 200; i++) {
                synchronized (this) {
                    if (ticketnum > 0) {//只要有票,就开始抢
                        System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                    }
                }
            }
        }
    }

    public class Test {
        public static void main(String[] args) throws Exception {
            BuyTickets bt = new BuyTickets("窗口1");
            BuyTickets bt1 = new BuyTickets("窗口2");
            BuyTickets bt2 = new BuyTickets("窗口3");
            //开启窗口卖票
            bt2.start();
            bt.start();
            bt1.start();
        }
}
```

多次运行之后,结果仍然会出现这种情况-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/111481657a4b414695b8fdbc1f6fcd25.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
为什么呢?
我们来看锁的类型---

  synchronized (this) {,这里this是指当前调用的对象,但是代码中有三个对象,我这三个对象可能没有对锁的问题达成共识,即窗口1认为应该用自己的锁,窗口2也是这么认为的,窗口3 也是这样认为的;所以this的不同导致锁的问题--如何让解决?
  换一把公认 的锁-----

```java
    class BuyTickets extends Thread {
        static int ticketnum = 8;//现在只剩8张票了,
        //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票
        public BuyTickets(String name) {
            super(name);
        }
        @Override
        public void run() {

            for (int i = 1; i < 200; i++) {
                synchronized (this.getClass()) {//这里只要是引用数据类型就可以
                    if (ticketnum > 0) {//只要有票,就开始抢
                        System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                    }
                }
            }
        }
    }

    public class Test {
        public static void main(String[] args) throws Exception {
            BuyTickets bt = new BuyTickets("窗口1");
            BuyTickets bt1 = new BuyTickets("窗口2");
            BuyTickets bt2 = new BuyTickets("窗口3");
            //开启窗口卖票
            bt2.start();
            bt.start();
            bt1.start();
        }
}
```

当只有一个线程的时候,下面以实现Runnable接口为例

```java
    class BuyTickets implements Runnable {
        static int ticketnum = 8;//现在只剩8张票了,
        //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票

        @Override
        public void run() {

            for (int i = 1; i < 200; i++) {
                synchronized (this) {
                    if (ticketnum > 0) {//只要有票,就开始抢
                        System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                    }
                }
            }
        }
    }

    public class Test {
        public static void main(String[] args) throws Exception {
            BuyTickets bt = new BuyTickets( );
           /* BuyTickets bt1 = new BuyTickets( );
            BuyTickets bt2 = new BuyTickets( );*/
            //开启窗口卖票
            Thread th= new Thread(bt,"窗口1");
            Thread th1= new Thread(bt,"窗口2");
            Thread th2= new Thread(bt,"窗口3");
            th2.start();
            th.start();
            th1.start();

        }
}
```

我们发现并发情况下,this可以将其锁住,
但如果是并行情况,

```java
    class BuyTickets implements Runnable {
        static int ticketnum = 8;//现在只剩8张票了,
        //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票

        @Override
        public void run() {

            for (int i = 1; i < 200; i++) {
                synchronized (this) {
                    if (ticketnum > 0) {//只要有票,就开始抢
                        System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                    }
                }
            }
        }
    }

    public class Test {
        public static void main(String[] args) throws Exception {
            BuyTickets bt = new BuyTickets( );
           BuyTickets bt1 = new BuyTickets( );
            BuyTickets bt2 = new BuyTickets( );
            //开启窗口卖票
            Thread th= new Thread(bt,"窗口1");
            Thread th1= new Thread(bt1,"窗口2");
            Thread th2= new Thread(bt2,"窗口3");
            th2.start();
            th.start();
            th1.start();

        }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/5e720e2258d04ceca6110472dff9adab.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)仍然没有实现一张一张的卖;
所以综上对比发现,在并发或者并行加🔒的时候,最好是加引用类型数据,而不是this;可以用static修饰引用数据类型;一般习惯放类的字节码信息;
![在这里插入图片描述](https://img-blog.csdnimg.cn/6c8e5fb941414427a03eb53d2fb8f596.png)

> 换成其他引用类型也可以,---注意不能是匿名对象;

并发和并行时间上--效率问题-----

并发时间----

```java

class BuyTickets implements Runnable {
   static int ticketnum = 800000;//现在只剩8张票了,
//    static int ticketnum=8;实际实现Runnable接口在只有一个线程的情况下加不加static修饰达到的效果是一样的

    //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票

    @Override
    public void run() {
        long start = System.currentTimeMillis();
        for (int i = 1; i < 2000000; i++) {
            synchronized (this.getClass()) {
                while (ticketnum > 0) {//只要有票,就开始抢
                    //System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                    ticketnum--;
                }
            }
        }
        long end = System.currentTimeMillis();
        System.out.println("售票用时"+(end-start));
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
//        BuyTickets bt1 = new BuyTickets();
//        BuyTickets bt2 = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th2 = new Thread(bt, "窗口2");
        Thread th1 = new Thread(bt, "窗口3");
        th.start();//212
        th1.start();//212
        th2.start();//209
    }
}
```

这是开启了一个线程去处理不同(窗口卖票的)任务------------并发



下面开启三个线程来卖票--即并行卖票时间

```java
class BuyTickets implements Runnable {
   static int ticketnum = 800000;//现在只剩8张票了,
//    static int ticketnum=8;实际实现Runnable接口在只有一个线程的情况下加不加static修饰达到的效果是一样的

    //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票

    @Override
    public void run() {
        long start = System.currentTimeMillis();
        for (int i = 1; i < 2000000; i++) {
            synchronized (this.getClass()) {
                while (ticketnum > 0) {//只要有票,就开始抢
                    //System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                    ticketnum--;
                }
            }
        }
        long end = System.currentTimeMillis();
        System.out.println("售票用时"+(end-start));
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        BuyTickets bt1 = new BuyTickets();
        BuyTickets bt2 = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th2 = new Thread(bt1, "窗口2");
        Thread th1 = new Thread(bt2, "窗口3");
        th.start();//202
        th1.start();//195
        th2.start();//195
    }
}
```

运行结果----差距不大,因为有锁的原因

**实现Callable接口**

单单实现Collable接口,

```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

class BuyTickets implements  Callable<String> {
   static int ticketnum = 800000;//现在只剩8张票了,
//    static int ticketnum=8;实际实现Runnable接口在只有一个线程的情况下加不加static修饰达到的效果是一样的

    //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票


    public void run() {
        long start = System.currentTimeMillis();
        for (int i = 1; i < 2000000; i++) {
            synchronized (this.getClass()) {
                while (ticketnum > 0) {//只要有票,就开始抢
                    System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                    //ticketnum--;
                }
            }
        }
        long end = System.currentTimeMillis();
        System.out.println("售票用时"+(end-start));
    }

    @Override
    public String call() throws Exception {
        new BuyTickets().run();
        return "票卖完了";
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        FutureTask ft= new FutureTask(bt);
        Thread th= new Thread(ft,"窗口1");
        Thread th2= new Thread(ft,"窗口2");
        Thread th3= new Thread(ft,"窗口3");
        th.start();
        th2.start();
        th3.start();


    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8eba091a20974b729abf153a566ab7ff.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

但是这种情况下,会只一个窗口抢票的情况,其他窗口无票可买的情况;
所以我们发现当实现Callable接口时,开启线程实际是调用的call方法,所以在用Callable接口开启线程是要重写call方法;

所以我们发现实现Callable接口时目的主要时要得到运行后的返回值,而且从运行结果来看,只有一个线程(随机)来完成卖票任务;---底层原因是什么?

以下是源代码图解,分析的有瑕疵,
![在这里插入图片描述](https://img-blog.csdnimg.cn/70ef7627734f4e8aa8e0db700b683ef0.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70#pic_center)

大体上是这样的,![在这里插入图片描述](https://img-blog.csdnimg.cn/6e031e45600b4b078713a3aa99c4582c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
主要是这部分代码,

> /**
>
> - Increments the count of unstarted threads in the thread group.
> - Unstarted threads are not added to the thread group so that they
> - can be collected if they are never started, but they must be
> - counted so that daemon thread groups with unstarted threads in
> - them are not destroyed.
>   */
>   未启动的线程不会被加入到该线程组,如果他们从未开始，但他们必须  
>   也要计算，以便在守护线程组中的未启动的线程没有被摧毁。

因为这里有一个锁--this,线程组去锁定未执行线程的数量.所以当我们在用Callable接口启动三个线程时,第一个启动后,会锁定剩余的线程,直到第一个执行完毕;

**要想用callable接口完成多个窗口卖票---貌似做不到**

如果做如下处理---将call()方法中的同步代码修饰符synchronized (BuyTickets.class)去掉,再运行---

```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

class BuyTickets implements  Callable<String> {
   static int ticketnum = 8;//现在只剩8张票了,
//    static int ticketnum=8;实际实现Runnable接口在只有一个线程的情况下加不加static修饰达到的效果是一样的
    //假设现在有三个窗口同时买票,每个窗口有20人在排队抢票
    @Override
    public String call() throws Exception {
        long start = System.currentTimeMillis();
        for (int i = 1; i < 20; i++) {
            //synchronized (BuyTickets.class) {
               while (ticketnum > 0) {//只要有票,就开始抢

                    System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                    //ticketnum--;
               }
            }
       // }
        long end = System.currentTimeMillis();
        System.out.println("售票用时"+(end-start));
        return "票卖完了";
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        BuyTickets bt1 = new BuyTickets();
        BuyTickets bt2 = new BuyTickets();
        FutureTask ft= new FutureTask(bt);
        FutureTask ft1= new FutureTask(bt1);
        FutureTask ft2= new FutureTask(bt2);
        Thread th= new Thread(ft,"窗口1");
        Thread th2= new Thread(ft1,"窗口2");
        Thread th3= new Thread(ft2,"窗口3");
        th.start();
        th2.start();
        th3.start();


    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0bc5ebd2ebe747039eb193732791b122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
所以,Callable接口开启的线程用法---只是为了得到某线程与运行的结果,已被他用,开启多线程还要保证线程安全,老老实实用Runnable接口吧!!!

**为什么会出现线程安全问题?**

在数据不共享的条件下----

同一时间由不同的线程同时操作同一个数据---就有可能产生线程安全问题----

```java
class BuyTickets implements Runnable {
    private  int ticketnum =5;//现在只剩5张票了,
    @Override
    public void run() {
        ticketnum--;//卖票.卖出一张减少一张
        System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum);
        }
    }
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt, "窗口2");
        Thread th2 = new Thread(bt, "窗口3");
        Thread th3 = new Thread(bt, "窗口4");
        Thread th4 = new Thread(bt, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a2d02e9ab0a84477a1953af94759a902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 虽然数据不共享,各自操作自己的数据,但是现实中某一号票只有一张,所以同时将同一张ticketnum票号卖给不同的人的情景是不允许的(当然并不一定每次都这样,但是多次运行后有机会出现这种情况,这是我们不想要的)

> 我们同时也会发现线程的实际运行与start()的顺序无关

**在数据共享的条件下----**

```java
class BuyTickets implements Runnable {
    private static int ticketnum =5;//现在只剩5张票了,
    @Override
    public void run() {
        ticketnum--;//卖票.卖出一张减少一张
        System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum);
        }
    }
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt, "窗口2");
        Thread th2 = new Thread(bt, "窗口3");
        Thread th3 = new Thread(bt, "窗口4");
        Thread th4 = new Thread(bt, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0a030c2c4992429dbc3a36df9479ccb1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

虽然数据的到了共享,但是仍然会出现同一张票卖给了不同的人的情况;

为什么会弧线这种情况呢?
问题就出在"ticketnum--"上,
ticketnum--在程序中(jvm)其实是分三步操作的
1----先读取ticketnum的值
2----执行(ticketnum-1)操作,即(ticketnum=ticketnum-1)
3----给ticketnum赋新值;
当同时访问同一个值的时候并对其进行操作,就会产生线程安全问题;



> 现实中的场景就是  由五个售货员销售五张票,

> 按照第一种代码的情况是----- 
>
> 老板说,有五张票你们去卖一下,然后五个销售员屁颠屁颠的就去了,
> A,B,C,D,E各自卖自己的,虽然只有五张票,但是ABCDE以为老板让他们各自卖五张票(即他们以为老板那的票总数是25箱)
> A卖了标签为1 号的一票,C不知道1号票已经卖了,也把1号票卖了,同理B,C也可能出现将3号票同时卖给了不同的人;
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1a219e20bde647c58406faf0fe565f2f.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 第二种代码的情况是---
> 虽然ABCDE知道只有五张票 但是当A 在卖第3号票时,还没根客户絮叨完就被C抢先卖出去了,当A的客户来拿票时也拿走了3号票(可实际3号票已经被C卖出去了)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f47205e1d6034276a3d72fbfb54913d4.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

**线程安全的解决方式**

同步方法

```java
class BuyTickets implements Runnable {
    private static int ticketnum =5;//现在只剩5张票了,
    @Override
    public synchronized void run() {
        ticketnum--;//卖票.卖出一张减少一张
        System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum);
    }
}
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt, "窗口2");
        Thread th2 = new Thread(bt, "窗口3");
        Thread th3 = new Thread(bt, "窗口4");
        Thread th4 = new Thread(bt, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

在代码中时当一个线程执行某方法时,其他线程不能执行该方法,一般情况下最好不要让run()方法作为同步方法的,(当我们有其他根卖票无关的线程时,由于卖票的线程在使用run(),其他线程无法使用run()导致程序执行的效率会降低)

锁住其他方法---

```java
class BuyTickets implements Runnable {
    private static int ticketnum =5;//现在只剩5张票了,
    @Override
    public  void run() {
        BuyTickets buy= new BuyTickets();
        buy.Buy();
    }
    public synchronized  void Buy(){
        ticketnum--;//卖票.卖出一张减少一张
        System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum);
    }

}
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt, "窗口2");
        Thread th2 = new Thread(bt, "窗口3");
        Thread th3 = new Thread(bt, "窗口4");
        Thread th4 = new Thread(bt, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

由于synchronized 修饰方法时锁定的是调用该方法的对象这里即那个buy对象。它并不能使调用该方法的多个对象在执行顺序上互斥。所以仍可能会出现卖同一张票的情况;

![在这里插入图片描述](https://img-blog.csdnimg.cn/751b0b3e6c1f4ba5b71b241a5abd5500.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)### 同步代码块

锁住有问题的代码块即可---

锁住代码块有两种形式------

第一种---用this关键字作为锁

```java
class BuyTickets implements Runnable {
    private static int ticketnum =5;//现在只剩5张票了,
    @Override
    public  void run() {
        synchronized(this) {
            ticketnum--;//卖票.卖出一张减少一张
            System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum);
        }
    }
    }
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt, "窗口2");
        Thread th2 = new Thread(bt, "窗口3");
        Thread th3 = new Thread(bt, "窗口4");
        Thread th4 = new Thread(bt, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/bd9ed5b1e88c462287ec06bee28ad196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)这时候锁住的时当前执行ticketnum-- 的对象 实质是锁的bt对象(同一个bt对象,用于开启了5次线程对象)

再来看用this能不能锁住不同的bt对象

```java
class BuyTickets implements Runnable {
    private static int ticketnum =5;//现在只剩5张票了,
    @Override
    public  void run() {
        synchronized(this) {
            ticketnum--;//卖票.卖出一张减少一张
            System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum);
        }
    }
    }
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        BuyTickets bt1 = new BuyTickets();
        BuyTickets bt2 = new BuyTickets();
        BuyTickets bt3 = new BuyTickets();
        BuyTickets bt4 = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt1, "窗口2");
        Thread th2 = new Thread(bt2, "窗口3");
        Thread th3 = new Thread(bt3, "窗口4");
        Thread th4 = new Thread(bt4, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/305a4394c7d4433ebe7bbf56fa0db3b4.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

发现锁不住,因为锁的是不同的bt对象,不同的对象需要的是同一把锁而不是多把锁即 此时bt的锁是bt,bt1的锁是bt1,.......发现根本锁不住;解决方法是换一把公认的锁;

大家对公认的锁的要求----
1;-----必须是引用数据类型
2;-----地址必须唯一不能是匿名对象
3;------如果是Object 对象,那么需要为静态的(或者final修饰的)

一般程序员公认的是类的字节码信息--
即如下代码

```java
class BuyTickets implements Runnable {
    private static int ticketnum =5;//现在只剩5张票了,
    @Override
    public  void run() {
        synchronized(this) {
            ticketnum--;//卖票.卖出一张减少一张
            System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum);
        }
    }
    }
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        BuyTickets bt1 = new BuyTickets();
        BuyTickets bt2 = new BuyTickets();
        BuyTickets bt3 = new BuyTickets();
        BuyTickets bt4 = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt1, "窗口2");
        Thread th2 = new Thread(bt2, "窗口3");
        Thread th3 = new Thread(bt3, "窗口4");
        Thread th4 = new Thread(bt4, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b43b8cb4534849acbcc7b4bc7f3e46c3.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
非线程安全是指----多个线程对同一个对象中的同一个实例变量进行操作时会出现制备更改,值不同步的情况,进而影响程序的实际想要的结果.

```java
    class BuyTickets implements Runnable {
        private static int ticketnum =5;//现在只剩5张票了,
        @Override
        public  void run() {
            Buy();
        }
        public synchronized void Buy(){
            ticketnum--;//卖票.卖出一张减少一张
            System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum);
        }
}
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        BuyTickets bt1 = new BuyTickets();
        BuyTickets bt2 = new BuyTickets();
        BuyTickets bt3 = new BuyTickets();
        BuyTickets bt4 = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt1, "窗口2");
        Thread th2 = new Thread(bt2, "窗口3");
        Thread th3 = new Thread(bt3, "窗口4");
        Thread th4 = new Thread(bt4, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

同步方法----
无论时同步代码块还是同步方法,都要明确锁住的对象,对同一个对象,可以用this来锁住,但是对不同的对象要用公认的锁来锁;
![在这里插入图片描述](https://img-blog.csdnimg.cn/303fb1d18e4a493cbb80929f1fd473fd.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)对于同步方法要达到一张一张卖票的结果,做如下修改即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/afa6df3349d4424d85e1643734e97f9e.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
当同步代码遇上非同步代码会碰撞出什么样的火花?
修改上面代码,将ticketnumm-- 放到 println里面-----
修改如下

```java
class BuyTickets implements Runnable {
    private static int ticketnum =5;//现在只剩5张票了,
    @Override
    public  void run() {
       // synchronized(this) {
            //卖票.卖出一张减少一张
            System.out.println("当前" + Thread.currentThread().getName() + "操作的ticketnum---" + ticketnum--);
       // }
    }
}
public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets();
        BuyTickets bt1 = new BuyTickets();
        BuyTickets bt2 = new BuyTickets();
        BuyTickets bt3 = new BuyTickets();
        BuyTickets bt4 = new BuyTickets();
        //开启窗口卖票
        Thread th = new Thread(bt, "窗口1");
        Thread th1 = new Thread(bt1, "窗口2");
        Thread th2 = new Thread(bt2, "窗口3");
        Thread th3 = new Thread(bt3, "窗口4");
        Thread th4 = new Thread(bt4, "窗口5");
        th.start();
        th1.start();
        th2.start();
        th3.start();
        th4.start();
    }
}
```

println方法----源代码如下---

```java
public void println(String x) {
        if (getClass() == PrintStream.class) {
            writeln(String.valueOf(x));
        } else {
            synchronized (this) {
                print(x);
                newLine();
            }
        }
    }
```

这里println方法在内部是同步的,那么是否把ticketnum放到println方法参数里就能做到哦同步呢?

先看实行结果---
![在这里插入图片描述](https://img-blog.csdnimg.cn/cc58bd541bc04e10b420e5115bdc3a0c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)仍然会出现销售同一张票的情况,问题仍然出在ticketnum-- 上,因为它的执行顺序在println之前,所以仍然要进行同步;



> this.getName与Thread.currentThread().getName()的区别----

一个是当前线程对象名,一个是当前调用的线程名
![在这里插入图片描述](https://img-blog.csdnimg.cn/26ef01ff8be749008c42df6532b6100d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



### 线程中断

先看代码--

```java
class BuyTickets extends Thread {

    @Override
    public void run() {
        for (int i = 1; i < 20; i++) {
            System.out.println(i);
            if (i == 5) {
                try {
                    Thread.sleep(20000);//线程休眠20秒
                } catch (InterruptedException e) {
                    System.out.println("线程被中断");
                 // return;
                }
            }

        }
    }
}

public class Test {
    public static void main(String[] args) {
        BuyTickets buyTickets = new BuyTickets();
        Thread th = new Thread(buyTickets, "线程A");
        th.start();
        try {
            Thread.currentThread().sleep(2000);//让程序先执行2秒
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        th.interrupt();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a601b2529b46458fadbab0311313a078.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

没有return的情况下---程序会一直执行,直到结束,虽然我们对线程th做了中断处理,实际上th确实时中断了,但是中断后又回到了就绪状态------接着执行



在有return的情况下,是当程序中断时返回方法调用处---于是

![在这里插入图片描述](https://img-blog.csdnimg.cn/1e092a185a2c4aeca79f93ad3ede14a1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
因此 interrupt()只是标记一下中断状态(catch (InterruptedException e)里面执行了),如果没有return,只能算是中断后又回到可执行状态;

```java
class BuyTickets extends Thread {

    @Override
    public void run() {
        for (int i = 1; i < 20; i++) {
            System.out.println(i);
            if (i == 5) {
                try {
                    Thread.sleep(20000);//线程休眠20秒
                } catch (InterruptedException e) {
                    System.out.println("线程被中断");
                return;
                }
            }

        }
    }
}

public class Test {
    public static void main(String[] args) {
        BuyTickets buyTickets = new BuyTickets();
        Thread th = new Thread(buyTickets, "线程A");
        th.start();
        System.out.println("线程状态--"+th.getState());
        try {
            Thread.currentThread().sleep(2000);//让程序先执行2秒
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        th.interrupt();
        boolean interrupted = th.isInterrupted();
        System.out.println("线程状态--"+th.getState());
        System.out.println("中断状态"+interrupted);
        System.out.println("线程状态--"+th.getState());
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/06a7c412ada14cafab807785c384a483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)关于线程中断的三个方法---

> public boolean isInterrupted()
>
> public void interrupt()
>
> public static boolean interrupted()

![在这里插入图片描述](https://img-blog.csdnimg.cn/b69a2be7fe6145c6bc33187f69676048.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70#pic_center)
总结---

> 中断状态并不能随随便便就结束程序执行,而是需要认为去判断让程序跳过或者返沪方法调用处异或退出;

![在这里插入图片描述](https://img-blog.csdnimg.cn/abcbfd7257b64a04abb44032bb60c265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

线程中断的几种方式
抛异常----
我们知道当我们的程序出现异常时,程序会中断执行,那么抛异常能达到我们想要的结果吗?

```java
class BuyTickets extends Thread {

    @Override
    public void run() {
        for (int i = 1; i < 20; i++) {

            if (i == 5) {
                try {
                    Thread.sleep(20000);//线程休眠20秒
                } catch (InterruptedException e) {
                    System.out.println("线程被中断");

                }

            }
            System.out.println(i);
        }
    }
}

public class Test {
    public static void main(String[] args) {
        BuyTickets buyTickets = new BuyTickets();
        Thread th = new Thread(buyTickets, "线程A");
        th.start();
        System.out.println("线程状态--"+th.getState());
        try {
            Thread.currentThread().sleep(2000);//让程序先执行2秒
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        th.interrupt();
        boolean interrupted = th.isInterrupted();
        if(th.isInterrupted()){
            try {
                throw new InterruptedException();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("线程状态--"+th.getState());
        System.out.println("中断状态"+interrupted);
        System.out.println("线程状态--"+th.getState());
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a0f81e1fdf774af1922bc73981da701d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
抛异常也没有使得当i==5时for循环停止运行;

结合上面的在沉睡中中止线程--炫耀认为干预才能使得for循环达到我们想要的结果,(break或者return)

线程中止的另一个方法------stop()方法-------------------------------已被弃用;

```java
class BuyTickets  {
    private String name="小花";
    private String gender="女";

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }
public void printinfo(String name,String Gender){
    synchronized(BuyTickets.class) {
        try {
            this.name = name;
            Thread.sleep(10000);
            this.gender = gender;
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

}
class CeshiThread extends Thread{
    private BuyTickets buyTickets;

    public CeshiThread(BuyTickets buyTickets) {
        this.buyTickets = buyTickets;
    }

    @Override
    public void run (){
buyTickets.printinfo("小刚","男");
    }
}
public class Test {
    public static void main(String[] args) {

        try {
            BuyTickets buyTickets = new BuyTickets();
            CeshiThread thread= new CeshiThread(buyTickets);
            thread.start();
            Thread.sleep(500);
            thread.stop();
            System.out.println(buyTickets.getName()+buyTickets.getGender());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e9e4a506fa864c3f8e28c5c9f87c9bcf.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 方法stop已经停止使用,强制让线程停止可能会使得处理性的其他工作
> 不能完成,如果该线程正在操作锁定的对象,那么强行停止该线程会使得对锁定的对象进行解锁,导致数据得不到同步处理而产生线程安全问题;

```java
class BuyTickets extends Thread {
    private static int ticketnum = 10;

    @Override
    public synchronized void run() {
        super.run();
        for (int i = 0; i < 11; i++) {
            try {
                while (ticketnum > 0) {
                    System.out.println("我在" + Thread.currentThread().getName() + "买到了第" + ticketnum + "张票");
                    Thread.currentThread().stop();
                    ticketnum--;
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

public class Test {
    public static void main(String[] args) {
        BuyTickets bts = new BuyTickets();
        Thread th = new Thread(bts, "窗口a");
        Thread th1 = new Thread(bts, "窗口b");
        th.start();
        th1.start();
    }
}
```

如果刚号在买到票的时候,票数统计没有减一的时候停止了该线程,就会导致线程安全问题,所以现在stop方法被弃用了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/0e0df1f6ad4f48fb90d1aa0d66b595c9.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

## 高并发三大特性

### visiblity

代码1----------->>>>
```java
public class VisibilityDemo {
   volatile static boolean flag = true;
    public static void main(String[] args) {
       new Thread(VisibilityDemo::methodA,"T").start();
        SleepHelper.sleepHelper(1);
        flag=false;
    }
    static void methodA(){
        while (flag) {
//              System.out.println("helloWorld");
        }
        System.out.println("end--thread");
    }
}

```
代码1是一个死循环,那怎吗才能让死循环结束呢?(假设不在while代码块中作文章)
那就只能在while条件上做文章.
代码2=========>>
```java
public class VisibilityDemo {
   static boolean flag = true;
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            while (flag) {
//              System.out.println("helloWorld");
            }
            System.out.println("end--thread");
        });
        Thread thread1 = new Thread(() -> {
            flag = false;
        });
        thread.start();
        SleepHelper.sleepHelper(1);
           thread1.start();
    }
}

```
代码块2也不能使得死循环结束,这是为什么呢???

![在这里插入图片描述](https://img-blog.csdnimg.cn/0ba25ffb5c4e4d78bac0d8a02b385455.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
除非当flag的值作出修改时对许所有线程是可见的;

代码3=================>>>>使用 volatile修饰变量

```java
package ThreadStateDemo;

public class VisibilityDemo {
   volatile static boolean flag = true;
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            while (flag) {
//              System.out.println("helloWorld");
            }
            System.out.println("end--thread");
        });
        Thread thread1 = new Thread(() -> {
            flag = false;
        });
        thread.start();
        SleepHelper.sleepHelper(1);
           thread1.start();
    }
}

```
 volatile同步是需要耗费一定时间的------观察以下代码
代码4===============>>>>>>>>>>
使用interrupt()配合isInterrupted()来打断
```java

public class VisibilityDemo {
    volatile static boolean flag = true;

    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            int result = 0;
            while (flag) {
                result++;
                if (Thread.currentThread().isInterrupted()) {
                    break;
                }
            }
            System.out.println("end--thread----" + result);
        });
       thread.start();
       SleepHelper.sleepHelper(1);
       thread.interrupt();
    }
}

```

> 第一次结果--------end--thread----1159276619
>  第二次结果--------end--thread----1187722156
> 第三次结果-------- end--thread----1121924318

代码5===============>>>
```java

public class VisibilityDemo {
    volatile static boolean flag = true;
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            int result = 0;
            while (flag) {
                result++;

            }
            System.out.println("end--thread----" + result);
        });
       thread.start();
       SleepHelper.sleepHelper(1);
      flag=false;
    }
}

```

> 第一次结果--------end--thread----1727090587
>  第二次结果end--thread----1644897205
> 第三次结果-------- end--thread----1564364340

虽然有误差,但是也反映出了一定的问题,volatile在可见性上(非线程安全的同步上)是需要耗费一定时间的;
**原因可能是当线程修改完flag后可能并没有立即写入到主存之中**

代码6===============>>>>>>>>

```java

public class VisibilityDemo {
     static boolean flag = true;
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            int result = 0;
            while (flag) {
                result++;
                System.out.println("ending..." + result);
            }
            System.out.println("ended");
        });
       thread.start();
       SleepHelper.sleepHelper(1);
      flag=false;
    }
}

```
这种能不能让线程停下来呢?

答案是-----能 
![在这里插入图片描述](https://img-blog.csdnimg.cn/3fd54521359041fc8d8d6d3daa527543.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


那为什么这种可以呢?
原因就在于 循环体中加入了System.out.println()
来看一下println的源码
![在这里插入图片描述](https://img-blog.csdnimg.cn/2107ddc86a4d4c7380ee391c76b95df7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
图片是个什么意思呢------->>>

> **当while循环体中有输出语句(只要有同步代码块也可以)那么做的同步动作都要写回到主存中,这时候线程thread中的数据与主存中做了同步,(修改flag的线程在对flag写入到主存之中后);**


再来看下一个代码
代码7===============>>>>>>

```java
public class VisibilityDemo {
    private static class Tests{
        boolean flag = true;
        public  void methodA(){
            while (flag) {

            }
            System.out.println("ended");
        }
    }
    private volatile static  Tests tests= new Tests();
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
         tests.methodA();
        });
       thread.start();
       SleepHelper.sleepHelper(1);
     tests.flag=false;
    }
}

```
这样能使得程序停下来吗???
答案是不能.
![在这里插入图片描述](https://img-blog.csdnimg.cn/365b9a3b15964516a0a09bbb86b63b67.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

当volatile修饰引用数据类型的时候,不能保证内部字段的可见性,只能保证引用本身(引用地址)的可见性;
代码9==========>>>小测试

```java
import java.util.Arrays;
public class VisibilityDemo {
   volatile static  int []array=new int[2];
    private static class Tests{
        public  void methodA(){
           array[0]=100;
           array[1]=200;
            System.out.println(Arrays.toString(array));
        }
    }
    public static void main(String[] args) {
        Tests tests= new Tests();
        Thread thread = new Thread(() -> {
         tests.methodA();
        });
       SleepHelper.sleepHelper(1);
       array[0]=888;
       array[1]=999;
        thread.start();
    }
}
```
结果是什么呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/96352a3aba44450c9bdc1c889bea3120.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
原因是  volatile修饰的是数组Array而不是数组中的内容,所以thread1对数组中的内容作出修改时并没有及时对线程Thread可见'
### 有序性
有序性==========>>>>
在我们的理解中程序是按照顺序执行的,但是真的是这样吗?
怎么验证呢?
代码10-------------->>>>>>>>>>>>>

```java

import java.util.concurrent.CountDownLatch;

public class OrderDemo {

    private static int a=0,b=0;
    private static int x=0,y=0;
    public static void main(String[] args) throws InterruptedException {
        for (int i = 0; i < Integer.MAX_VALUE; i++) {
            x=0;
            y=0;
            a=0;
            b=0;
            CountDownLatch cdl= new CountDownLatch(2);
            Thread one=new Thread(new Runnable() {
                @Override
                public void run() {
                    a=1;
                    x=b;
                    cdl.countDown();
                }
            });


            Thread other= new Thread(new Runnable() {
                @Override
                public void run() {
                    b=1;
                    y=a;
                    cdl.countDown();
                }
            });
            one.start();
            other.start();
            cdl.await();
            String result="第"+i+"次("+x+","+y+")";
            if(x==0&&y==0){
                System.out.println(result);
                break;
            }
        }
    }
}

```
结果----->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b36e6855f2c47578bde387b8c563612.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
原因分析=============>>>>>>>>>>>>>
首先为什么会出现x=0.y=0的情况?一定是x=a,以及y=b先执行了,才会出现这样的结果;

![在这里插入图片描述](https://img-blog.csdnimg.cn/8646355f7eff409c82dc916c474f5aca.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


为什么会存在乱序的现象呢?
现在CPU的对指令进行专门的设计,
在单线程中   代码11===============>>>>>>>>>>

```java

public class SingleThreadDemo {
  static  int a=0;
    static int b=0;
    static int x;
    public static void main(String[] args) {
        m();
    }
    public static void m(){

        x=b;
        a=1;
       /* a=1;
        x=b;*/
       //交换顺序对最后的结果并不能产生影响
        System.out.println("a="+a+",x="+x);
    }
}

```
x=b;
        a=1;谁先执行对最后结果并没有影响(因为他俩之间并没有什么关联)
        再看代码12=============================>>>>
```java

public class SingleThreadDemo {
  static  int a=0;
    static int b=0;
    static int x;
    public static void main(String[] args) {
        m();
    }
    public static void m(){
        a=b++;
        x=b;
       /* a=b++;
        x=b;*/
       //交换顺序对最后的结果产生影响,那么CPU就不会使其交换
        System.out.println("a="+a+",x="+x);
    }
}
```
在这种情况下,最后的结果产生影响,

#### 乱序存在的原则

所以在单线程中乱序存在的原则------------------->>>>
不影响单线程结果的最终结果;

那乱序方法多线程中,
![在这里插入图片描述](https://img-blog.csdnimg.cn/9688f477a84249598ed0a51ab27a865f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
单就a=1;x=b;放在Thread one单线程中对单线程的执行结果没有什么影响,
单就b=1;y=a;放在Thread other单线程中对单线程执行结果没有什么影响,
但是这两个线程之间有关联,那么乱序就会对结果产生影响;

怎么避免呢?
使用voliate关键字修饰相关变量,以消除乱序的对多线程执行结果的影响影响(voliate---保证禁止指令重排序)

在高并发的一本书中有这么一段代码================>>
代码==============13
```java

public class OrderDemo2 {
    private  static boolean ready=false;
    private static int num;
    static class Ordered extends Thread{
        @Override
        public void run() {
          while(!ready){
              Thread.yield();
          }
            System.out.println("num"+num);
        }
    }
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Ordered();
        t.start();
        num=55;
        ready=true;
        t.join();
    }
}
```
这段代码运行起来看似没有什么问题,但是就真的没有问题吗?
根据前面对单线程中有序性的原则(前后指令有依赖关系,对单线程结果产生影响的要保证按序执行);

num=55 与ready=true这两个在main线程中并没有什么关联,完全有可能会进行乱序执行即ready=true;先执行,num=55后执行
那么控制台打印的结果就是num=0;

### 原子性
就是常说的同步问题;
多个线程访问同一个数据时会产生与预期结果不一致的结果

例如下列代码-----....

```java

import java.util.concurrent.CountDownLatch;
public class ThreadYZDemo {
    private static Long count=0L;
    public static void main(String[] args) throws InterruptedException {
        Thread[] threads = new Thread[100];
        CountDownLatch latch = new CountDownLatch(threads.length);
        for (int i = 0; i < threads.length; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 10000; j++) {
                    //synchronized (ThreadYZDemo.class){
                        count++;
                   // }
                }
                latch.countDown();
            });

        }
        for (Thread t :
                threads) {
            t.start();
        }
        latch.await();
        System.out.println(count);
    }
}
```
本意时每个线程都执行++操作,最后预期得到的值为100万.但是执行后发现并没有达到预期,因为多个线程在同时修改一个数据得时候就会出现数据得不一致问题;

怎么解决?
加锁

```java

import java.util.concurrent.CountDownLatch;
public class ThreadYZDemo {
    private static Long count=0L;
    public static void main(String[] args) throws InterruptedException {
        Thread[] threads = new Thread[100];
        CountDownLatch latch = new CountDownLatch(threads.length);
        for (int i = 0; i < threads.length; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 10000; j++) {
                    synchronized (ThreadYZDemo.class){
                        count++;
                    }
                }
                latch.countDown();
            });

        }
        for (Thread t :
                threads) {
            t.start();
        }
        latch.await();
        System.out.println(count);
    }
}
```
另一种方式----采用原子类操作------------->>>>>>>>>

```java

import java.util.concurrent.CountDownLatch;
import java.util.concurrent.atomic.AtomicLong;

public class ThreadYZDemo {
    //private static Long count=0L;
   private static  AtomicLong count=new AtomicLong(0);
    public static void main(String[] args) throws InterruptedException {
        Thread[] threads = new Thread[100];
        CountDownLatch latch = new CountDownLatch(threads.length);

        for (int i = 0; i < threads.length; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 10000; j++) {
                       count.getAndIncrement();//相当于i++操作
                }
                latch.countDown();
            });
        }
        for (Thread t :
                threads) {
            t.start();
        }
        latch.await();
        System.out.println(count);
    }
}
```

> **高并发上锁的本质并发编程序列化**

### 对象的半初始化状态

你知道你刚刚new出来的对象有哪几种状态吗?


先看一小段代码---------->>>>

```java
public class HalfComplete {
    public static void main(String[] args) {
        Object object=new Object();
    }
}

```
很简单对吧,看一下class文件
<init>初始化----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/a0559bd4e2b84f9b9e93fd61d65ae0da.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
aload_0表示从局部变量中加载 reference即从main方法中的String[]args中加载
然后执行特殊方法----Object构造方法;
然后返回;//结束
main方法中的------------------>>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/b7c01e82d76a45628ad5ec36092494e3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
new为Object对象开辟空间,
dup表示复制一份object对象用于下一次的使用
执行特使方法初始化Object对象;(由于构造方法中没有属性参数,看不出什么问题)
存储局部变量object对象

再来看另一个小代码-----------带属性的

```java
class TestOne {
    int m = 1000;
    public TestOne() {
       int n=888;
    }
}
public class HalfComplete {
    public static void main(String[] args) {
        TestOne testOne = new TestOne();
      
    }
}
```
<init>初始化
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a872c5635cc431793916201003fa926.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
main方法中的
![在这里插入图片描述](https://img-blog.csdnimg.cn/8d488f22234849e8ade3a2fd0a29546c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以发现即使带了属性/参数,在调用构造方法是并没有连同构造方法中的属性/参数一起初始化了,所以这时候构造方法中的n默认为0,那么什么时候n=100了呢?


我们再看以下代码


```java
class TestOne {
    int m = 1000;
    public TestOne() {
       int n=888;
    }
}
public class HalfComplete {
    public static void main(String[] args) {
        TestOne testOne = new TestOne();
        System.out.println(testOne.m);
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/956e74411a554b9dad12f7fbe9b22e41.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



属性n什么时候夹加载呢?------------->>>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/32fb90d41bb149d7b5abb31625a55a29.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


> **总结------------->>>
> 所以类在初始化的时候
> 初始化会先加载父类Object然后加载子类---即new对象的构造,在加载构造时并没有将类中的变量以及构造中的变量完成初始化,可以认为这时候是一个半初始化状态,   此时 int m   以及int n 都为默认值 0;**

> **无参构造则会先将参数先入栈,剩下的跟无参构造一样;**


那我们来做一个小测验看下面的代码,
也是一本书中的代码------------->>>>>

```java
import java.io.IOException;

class One {
    int m = 9999;

    public One() {
        new Thread(() -> {
                System.out.println(this.m);
        }).start();
    }
}
public class HalfCompleteTest {
    public static void main(String[] args) throws IOException {

           One one= new One();

        System.in.read();
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed0e49fe422149e7a8e0379788739973.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **这里涉及到指令重排序的问题,当 invokespecial
> 与astore_1顺序颠倒时,即 new对象还没有完全初始化,这时候来了一个线程,拿到了这个对象,但是该对象还没有初始化完成,即m这时候还是0,那么 System.out.println(this.m);就会输出     0这种结果;**

这种现象叫this的溢出;


```java

import java.io.IOException;

class One {
    int m = 9999;

    public One() {
        new Thread(() -> {
            if (this.m == 0) {
                System.out.println("被我抓住了");
            }
        }).start();
    }
}
public class HalfCompleteTest {
    public static void main(String[] args) throws IOException {
        for (int i = 0; i < Integer.MAX_VALUE; i++) {
            new One();
            }
        System.in.read();
    }
}

```


关于this溢出的解决--------->>>
可以在构造方法里new线程,但是不要再构造方法里启动的线程,

```java
import java.io.IOException;

class One {
    int m = 9999;
Thread t ;
    public One() {
     t=   new Thread(() -> {
     System.out.println(this.m);           
        });
    }
    public void starts(){
    t.start();
}
public class HalfCompleteTest {
    public static void main(String[] args) throws IOException {
        for (int i = 0; i < Integer.MAX_VALUE; i++) {
            new One();
            }
        System.in.read();
    }
}

```

java中new一个对象是怎样的存储结构呢?

最原始的形式new一个对象-------->>>

```java
package com.thread.gavin.ThreadDemo;

import org.openjdk.jol.info.ClassLayout;

public class NewHeaderDemo {
    public static void main(String[] args) {
    
       Object object= new Object();
       System.out.println(ClassLayout.parseInstance(object).toPrintable());
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a4e192b94b214742aa7f574f1eca5ae6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



先看代码---->>>>

```java

import org.openjdk.jol.info.ClassLayout;
class Te{
    int result=100;
    public Te(int a){

    }
}
public class NewHeaderDemo {
    public static void main(String[] args) {
       Te t= new Te(12);
       System.out.println(ClassLayout.parseInstance(t).toPrintable());
    }
}

```
使用jol可以查看new一个对象的内存状态
那么t对象在内存中是什么样的呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/bb9b5c2eb7f54ab3ae75cebee93f9879.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/7a5bda597ef648e783b5c0ba1d119993.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

这个有什么用呢?

来看一张图hotpot底层sycn的实现----------------->>>
主要是看markword
![在这里插入图片描述](https://img-blog.csdnimg.cn/5aa719f9a3814c658aaa084965c0929d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## JAVA中的锁

在多线程与高并发这一学习阶段最重要的就是对锁的认识.

### 并行锁

```java
class BuyTickets implements Runnable {
    static int ticketnum = 8;//现在只剩8张票了,
    //假设现在有三个窗口同时买票,每个窗口有200人在排队抢票

    @Override
    public void run() {
        for (int i = 1; i < 200; i++) {
            synchronized (BuyTickets.class) {//并行锁---公认的锁
                while (ticketnum > 0) {//只要有票,就开始抢
                    System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                }
            }
        }
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets( );
        BuyTickets bt1 = new BuyTickets( );
       BuyTickets bt2 = new BuyTickets( );
        //开启窗口卖票
        Thread th= new Thread(bt,"窗口1");
        Thread th1= new Thread(bt1,"窗口2");
        Thread th2= new Thread(bt2,"窗口3");
        th2.start();
        th.start();
        th1.start();
    }
}

```

锁住的是当前执行的线程-----一直由一个线程执行,其他线程得不到执行的机会了
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ac68336bfc7457f9cb295aaf630df11.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

### 并发锁

```java
class BuyTickets implements Runnable {
    static int ticketnum = 8;//现在只剩8张票了,
    //假设现在有三个窗口同时买票,每个窗口有200人在排队抢票

    @Override
    public void run() {
        for (int i = 1; i < 200; i++) {
            synchronized (BuyTickets.class) {//并发锁---公认的锁
                while (ticketnum > 0) {//只要有票,就开始抢
                    System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                }
            }
        }
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets( );
        BuyTickets bt1 = new BuyTickets( );
        BuyTickets bt2 = new BuyTickets( );
        //开启窗口卖票
        Thread th= new Thread(bt,"窗口1");
        Thread th1= new Thread(bt,"窗口2");
        Thread th2= new Thread(bt,"窗口3");
        th2.start();
        th.start();
        th1.start();
    }
}
```

```java
class BuyTickets implements Runnable {
    static int ticketnum = 8;//现在只剩8张票了,
    //假设现在有三个窗口同时买票,每个窗口有200人在排队抢票

    @Override
    public void run() {
        for (int i = 1; i < 200; i++) {
            synchronized (this) {//并发锁---当前对象
                while (ticketnum > 0) {//只要有票,就开始抢
                    System.out.println("我在" + Thread.currentThread().getName() + "抢到第" + ticketnum-- + "张票");
                }
            }
        }
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets( );
        BuyTickets bt1 = new BuyTickets( );
        BuyTickets bt2 = new BuyTickets( );
        //开启窗口卖票
        Thread th= new Thread(bt,"窗口1");
        Thread th1= new Thread(bt,"窗口2");
        Thread th2= new Thread(bt,"窗口3");
        th2.start();
        th.start();
        th1.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/09b5cc64056c4006a685f57548ec4367.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
并发的时候锁住当前对象后者换一把公认的锁都是可以的;

### 死锁问题

为什么会产生死锁?就像递归调用的时候为什么会出现死循环?
因为递归调用-----嵌套调用;

**死锁案例1**

```java
class Bangjia {
    public synchronized void say(Shouhai sh) {
        System.out.println("把钱给我，我就放了你弟弟");
        sh.give();		}// 受害者等着放了他弟弟
    public synchronized void give() {
        System.out.println("等着收钱，然后放了他弟弟，");	}}
class Shouhai {
    public synchronized void say(Bangjia bj) {
        System.out.println("放了我弟弟，我就给你钱");
        bj.give();	}//绑架者等着给钱
    public synchronized void give() {
        System.out.println("等着弟弟被放,后给钱");	}}
public class ThreaDemo implements Runnable {
    private Bangjia bj = new Bangjia();
    private Shouhai sh = new Shouhai();
    public ThreaDemo() {
        new Thread(this).start();
        sh.say(bj);	}
    @Override
    public void run() {
        bj.say(sh);	}
    public static void main(String[] args) {
        new ThreaDemo();
    }
}

```

以上程序多次执行就可能会产生死锁问题---
![在这里插入图片描述](https://img-blog.csdnimg.cn/445e74f568f04db0a9e238bd1744d7c9.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
以上只是强行写了一个代码,在日常写代码中尽量避免嵌套的同步;

**死锁案例2**

```java

 class A extends Thread{
    private static String str1= new String("锁1");
  private static  String str2= new String("锁2");
  int flag;
    public  void methodA(){
        //if(flag==1) {
            synchronized (str1) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName()+"A");
                synchronized (str2) {
                    System.out.println(Thread.currentThread().getName()+"B");
                }
            }
       // }
       // if(flag==2){
            synchronized (str2) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName()+"A");
                synchronized (str1) {
                    System.out.println(Thread.currentThread().getName()+"B");
                }
            }
        }
    //}
   @Override
    public void run() {
        super.run();
        methodA();
    }
}
public class Car {
    public static void main(String[] args) {
    A a= new A();
    A b= new A();
    a.flag=2;
    b.flag=1;
    Thread t= new Thread(a,"线程1");
    Thread tt= new Thread(b,"线程2");
    t.start();
    tt.start();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/fbcace48d4df406598caf098d5918eb6.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

锁的其他实现方式---

还有一种锁的方式时实现Lock接口来完成,

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class BuyTickets implements Runnable {
   
//现有一把锁
    Lock lock = new ReentrantLock();
    @Override
    public void run() {
            lock.lock();
            try {
                for (int i = 1; i < 4; i++) {
                    System.out.println("我在" + Thread.currentThread()+ i);
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }


public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets( );
        BuyTickets bt1 = new BuyTickets( );
        BuyTickets bt2 = new BuyTickets( );
        //开启窗口卖票
        Thread th= new Thread(bt,"窗口1");
        Thread th1= new Thread(bt,"窗口2");
        Thread th2= new Thread(bt,"窗口3");
        th2.start();
        th.start();
        th1.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b58281fd10844eca80f82e53a155b80d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
可以看到是一个打印完成后另一个再打印的;-----同一个bt对象

如果是不同的对象------

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class BuyTickets implements Runnable {
  
//现有一把锁
   static Lock lock = new ReentrantLock();
    @Override
    public void run() {
            lock.lock();
            try {
                for (int i = 1; i < 4; i++) {
                    System.out.println("我在" + Thread.currentThread()+ i);
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }


public class Test {
    public static void main(String[] args) throws Exception {
        BuyTickets bt = new BuyTickets( );
        BuyTickets bt1 = new BuyTickets( );
        BuyTickets bt2 = new BuyTickets( );
        //开启窗口卖票
        Thread th= new Thread(bt,"窗口1");
        Thread th1= new Thread(bt1,"窗口2");
        Thread th2= new Thread(bt2,"窗口3");
        th2.start();
        th.start();
        th1.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f9971706b74e419abe7f212f9bcfae70.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> lock 锁   --------------------- 
> 1,线程无论是否出现异常都不会主动释放锁,释放锁需要手动关闭;



所这个问题分析起来比较复杂,而且锁的定义根实际问题相关联,这次先分析一下--------

### 可重入锁

> 可重入就是说某个线程已经获得某个锁，可以再次获取该锁而不会出现死锁。

代码演示-----

```java
class Personsaid {
    public  synchronized void lisiSaid() {
        System.out.println("我是李四");
    }
    public  synchronized void zhangsanSaid() {
        System.out.println("我是张三");
        lisiSaid();
    }
    public  synchronized void wangwuSaid() {
        System.out.println("我是王五");
        zhangsanSaid();
    }
}
class PersonTest implements Runnable {
    @Override
    public void run() {
        Personsaid ps = new Personsaid();
        ps.wangwuSaid();
    }
}

public class Test {
    public static void main(String[] args) {
        PersonTest ps = new PersonTest();
        Thread th = new Thread(ps);
        th.start();
        }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/ddf95a1973a04f55b3192307d29edd63.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
这说明在线程获得一个锁的时候,是可以获得该类中的其他的锁的(必须是同一把锁),因为上图是只有一个线程对象,如果是多个线程对象,那么结果如下

```java
class Personsaid {
    public static synchronized void lisiSaid() {
        System.out.println("我是李四");
    }
    public static synchronized void zhangsanSaid() {
        System.out.println("我是张三");
        lisiSaid();
    }
    public static synchronized void wangwuSaid() {
        System.out.println("我是王五");
        zhangsanSaid();
    }
}

class PersonTest implements Runnable {
    @Override
    public void run() {
        Personsaid ps = new Personsaid();
        ps.wangwuSaid();
    }
}

public class Test {
    public static void main(String[] args) {
        PersonTest ps = new PersonTest();
        PersonTest ps1 = new PersonTest();
        PersonTest ps2 = new PersonTest();
        Thread th = new Thread(ps);
        Thread th1 = new Thread(ps1);
        Thread th2 = new Thread(ps2);
        th.start();
        th1.start();
        th2.start();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/be309484d54f47afb408fc7e4373c007.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

### 继承关系中的锁

对于继承关系时,子类也可以通过继承关系来调用父类的同步方法---

```java
class PersonT{
    public synchronized static  void PersonSay(){
        System.out.println("我是人会说话");
    }
}

class Personsaid extends  PersonT{
    public static synchronized void lisiSaid() {
        System.out.println("我是李四");
        PersonSay();
    }
    public static synchronized void zhangsanSaid() {
        System.out.println("我是张三");
        lisiSaid();
    }
    public static synchronized void wangwuSaid() {
        System.out.println("我是王五");
        zhangsanSaid();
    }
}
class PersonTest implements Runnable {
    @Override
    public void run() {
        Personsaid ps = new Personsaid();
        ps.wangwuSaid();
    }
}
public class Test {
    public static void main(String[] args) {
        PersonTest ps = new PersonTest();
         Thread th = new Thread(ps);
         th.start();
    }
}
```

同步的方法不具有继承性,即如果父类中有一个同步方法,子类中有一个同名方法,那么这个同名方法也必须声明为同步方法才能实现子类中的该方法的同步;
![在这里插入图片描述](https://img-blog.csdnimg.cn/c6cf25daa51143af98ac34515c3219b1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

### 释放锁的几种情形

> synchronized---------------- 
> 1,线程执行完会释放锁. 
> 2,线程遇到异常时会自动释放锁

#### 锁的释放

***异常锁-----***
加入线程在执行的时候出现异常,如果没有做正确处理,那么原来同步的方法可能就会产生线程安全问题---

```java
class A {
	static int count=10;
	public static synchronized void fun() {
		for(int i =0;i<10;i++) {
			if(count>0) {
			System.out.println(count--);
			if(i==5) {
				int a=i/0;
				System.out.println("出现异常");
				
				
			}
		}
		}
	}
	public static synchronized void fun1() {
		System.out.println("对count进行其他的操作"+(count+1));
	}
}
class AA extends Thread{

	@Override
	public void run() {
		// TODO Auto-generated method stub
		super.run();
		System.out.println(this.getName()+"AA执行");
		new A().fun();
	}
	
}
class AAA extends Thread{

	@Override
	public void run() {
		// TODO Auto-generated method stub
		super.run();
		System.out.println(this.getName()+"AAA执行");
		new A().fun1();
	}
	
}
public class Test {
    public static void main(String[] args) {
    AA a = new AA();
    	Thread th= new Thread(a);
    	th.start();
        AAA aa = new AAA();
    	Thread th1= new Thread(aa);
    	th1.start(); 	
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/dc6ed3dc207349c9b9baf38812a77a0f.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



锁谁有一定的讲究----------------------
![在这里插入图片描述](https://img-blog.csdnimg.cn/1703d01a0dee45fbb2fdddfa4cd2388f.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

总结

synchronized的用法

修饰实例方法----即锁住当前对象

```java
public class A {
        public synchronized void methodA() {
    System.out.println("A")
    }
}
   
```

修饰静态方法----即锁住类

```java
public class B {
    public static synchronized void methodB() {
 System.out.println("B")
    }
}
```

修饰代码块----即可锁住类也可锁住当前对象

```java
public class C {
    public void methodC() {
    	// 对当前对象加锁
        synchronized (this) {
         System.out.println("C")
        }
    }

    public void methodD() {
    	// 对类加锁即整个类中的所有加锁
        synchronized (C.class) {
        
        }
    }
}
```

修饰代码块----即锁住其他---公用监视器

```java
class E extends Thread {
 String o = new String("Lock");
public void methodE() {
	synchronized (o) {
		}
		}}
```

注意多态下的锁

```java
public class Test01 {
    static Object o = new Object();
    static int ticket = 10;
    public void fun() {
        for (int i = 0; i < 100; i++) {
            synchronized (o) {
                if (ticket > 0) {
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName()+"---"+ticket--);
                }

            }

        }
    }
    public static void main(String[] args) {
Test01 test01= new Test01();
new Thread(test01::fun,"线程1").start();//启动一个线程1
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        o=new Object();
        new Thread(test01::fun,"线程2").start();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b1c2fe8d376d4d9c990b37d3ade060be.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)所以可以为o加一个final修饰符以保证锁的唯一性;

### 生产者与消费者问题

> 不啰嗦,学过的都知道问题的描述,已经是大神级别的也不会来看这篇文章吧........

```java
public class Product {//生产的商品信息
    private String Brand;
    private String name;

    public String getBrand() {
        return Brand;
    }

    public void setBrand(String brand) {
        Brand = brand;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
```

```java
public class ProducerP implements Runnable{//生产者生产商品
   private Product p;

    public ProducerP(Product p) {//生产的商品要确定
        this.p = p;
    }

    @Override
    public void run() {//开始生产商品
        for (int i = 1; i <21 ; i++) {
            if(i%2==0){
                p.setName("苹果");//生产
                try {
                    Thread.sleep(100);//模拟工作时间间隔
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                p.setBrand("栖霞");//贴牌

            }else{
                p.setName("啤酒");  //生产
                try {
                    Thread.sleep(100);//模拟工作时间间隔
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                p.setBrand("青岛");//贴牌
            }
            System.out.println("我是生产者.我生产了"+p.getBrand()+"------"+p.getName());
        }
    }
}

```

```java
public class ConsumerP implements Runnable{//消费者消费
    private Product p ;// 消费的商品

    public ConsumerP(Product p) {
        this.p = p;
    }

    @Override
    public void run() {
        for (int i = 1; i <21 ; i++) {
            System.out.println("我是消费者.我消费了"+p.getBrand()+"------"+p.getName());
        }
    }
}

```

```java
public class ProdTest {
    public static void main(String[] args) {
        Product p= new Product();
        ProducerP pp= new ProducerP(p);
        ConsumerP cp= new ConsumerP(p);
        Thread th= new Thread(pp);
        Thread th1= new Thread(cp);
        th.start();
        th1.start();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/4aaf5f21a7dc4859af2b074366304009.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

出现以上结果的原因时生产者刚刚生产完商品还没来的及贴牌,就被消费者买走了,所以会出现null;

怎么解决???加同步呗,加什么样的锁?生产者消费者是两个不同的对象,因此加一个公认的锁---可以是Procuct 类的对象也可以是其他,很明显用Product 类对象p是比较好的选择----
![在这里插入图片描述](https://img-blog.csdnimg.cn/a5727808216f4bd08c98855e4f8ddbd8.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)如果是锁住方法的话,问题的本源在于商品身上,所以只要锁住商品就可以做到,问题是在生产者和消费者那里锁住了商品p那么要想在方法中锁住商品,那么就需要在Product中写一个方法来完成----
所以代码改动如下----



![](C:\Users\Gavin\Pictures\Camera Roll\10.png)

虽然解决了生产消费的问题,但是并没有达到实际效果,问题应该改按照生产者生产一个消费者拿走一个的方向走,那怎么办呢?

```java
    public synchronized void setProduct(String brand, String name) {
        if (flag == true) {//如果有商品,生产者就等待消费者取走商品后在生产
            try {
                wait();//等待,下面的代码不执行
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //否则
        this.setBrand(brand);
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        this.setName(name);
        System.out.println("生产者生产了：" + this.getBrand() + "---" + this.getName());
        flag = true;//生产完后变为有商品了
        notify();//通知消费者消费
    }


    public synchronized void getProduct() {
        if(flag==false) {//如果没有商品,通知生产者生产,消费者等待
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
            System.out.println("消费者消费了：" + this.getBrand() + "---" + this.getName());
            flag=false;
            notify();
        }
```

消费者生产者有多个,仍然会出现问题----
分析图如下---
![](C:\Users\Gavin\Pictures\Camera Roll\11.png)



解决方式时开辟两个等待池;

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
class C{

    private String name;
    private int age ;//
    //声明一个Lock锁：
    Lock lock = new ReentrantLock();
    //搞一个生产者的等待队列：
    Condition produceCondition = lock.newCondition();
    //搞一个消费者的等待队列：
    Condition consumeCondition = lock.newCondition();
boolean flag=false;
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
    public  void setC(String name, int age){
        lock.lock();
        try{
        if(flag==true){//如果 有,那么停止生产
            try {
              produceCondition.await();//生产者等待队列
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
        this.setName(name);
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        this.setAge(age);
        flag=true;
        consumeCondition.signal();
        }
        catch(Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }


    }
    public   void getC(){
        lock.lock();
        try {
            if (flag == false) {
                try {
                    consumeCondition.await();//消费者等待队列
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }

            System.out.println(this.getName() + "----" + this.getAge());
            flag = false;
            produceCondition.signal();
        }catch(Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }

    }
}

class A extends Thread {
    private C c;

    public A(C c) {
        this.c = c;
    }

    @Override
    public void run() {
        super.run();

        for (int i = 0; i < 10; i++) {
            if (i % 2 == 0) {
               c.setC("Aaaa",10);
            }else{

                c.setC("Bbbbb",20);
            }


        }
    }
}

class B extends Thread{
    private C c;

    public B(C c) {
        this.c = c;
    }

    @Override
    public void run() {
        super.run();
        for (int i = 0; i <10 ; i++) {
            c.getC();
        }
    }
}
public class Car {
    public static void main(String[] args) {
        C c = new C ();
        A a = new A(c);
        Thread t = new Thread(a, "线程1");

        t.start();
       B b = new B(c);
        Thread tt = new Thread(b, "线程2");
tt.start();
    }
}

```

图解分析以上两种模式的区别

![](C:\Users\Gavin\Pictures\Camera Roll\12.png)



## 线程之间共享数据

### static与volatile关键字

还是以卖票为例---

```java
class Person implements Runnable{
  static int ticketnum=10;
    @Override
    public void run() {
        for (int i = 0; i <20 ; i++) {
            while(ticketnum>0)            System.out.println(Thread.currentThread().getName()+"---"+ticketnum--);
        }
    }
    
}
public class Test01 {
      public static void main(String[] args) {
     
    Person per = new Person();
    Person per1 = new Person();
    Thread a= new Thread(per,"小明");
    Thread aa= new Thread(per1,"小花");
    a.start();
    aa.start();
    }
}

```

```java
class Person implements Runnable{
   volatile int ticketnum=10;
    @Override
    public void run() {
        for (int i = 0; i <20 ; i++) {
            while(ticketnum>0)            System.out.println(Thread.currentThread().getName()+"---"+ticketnum--);
        }
    }
    
}
public class Test01 {
      public static void main(String[] args) {
     
    Person per = new Person();
    Person per1 = new Person();
    Thread a= new Thread(per,"小明");
    Thread aa= new Thread(per1,"小花");
    a.start();
    aa.start();
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c9b11b03f94243ca8526039e124b3082.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

由上图可以看到volatile有同步的影子(票是一张一张卖的),但是并不能保证线程安全性;

> 原因是虽然volatile保证了可见性-----但是并不能保证原子性
>
> 被volatile关键字修饰的变量，编译器与运行时都会注意到这个变量是共享的，因此不会将该变量上的操作与其他内存操作一起重排序。volatile变量不会被缓存在寄存器或者对其他处理器不可见的地方，因此在读取volatile类型的变量时总会返回最新写入的值。

> 即----当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去内存中读取新值。

于是就有了每次执行时,直接去内存中读取count的值,并且每次修改完该值后对所有线程都是可见的-----


再来举个例子----

代码一     	------------
```java

public class Test01 {
    static boolean flag=true;
    // boolean flag=true;//跟static修饰的运行结果一样
    void methodA(){
        System.out.println(
                "methodA---start"
        );
        while(flag){

        }
        System.out.println(
                "methodA---end"
        );
    }
    public static void main(String[] args) {
        Test01 t= new Test01();
        new Thread(t::methodA,"t1").start();
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        t.flag=false;//在main线程中将flag的值进行修改,对线程t1是不可见的,t1于是在while处进入死循环

    }
}
```
代码二		--------------
```java

public class Test01 {
    volatile boolean flag=true;
    void methodA(){
        System.out.println(
                "methodA---start"
        );
        while(flag){

        }
        System.out.println(
                "methodA---end"
        );
    }
    public static void main(String[] args) {
        Test01 t= new Test01();
        new Thread(t::methodA,"t1").start();
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        t.flag=false;//在main线程中将flag的值进行修改,对线程t1是可见的,t1于是在while处跳出循环

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/5458be124cba498aa732ae9ba6a93892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
最后在分析以下volatile是如何保证可见性的---

先来看没有volatile修饰的原理图----

![在这里插入图片描述](https://img-blog.csdnimg.cn/8c3ab7f8cedf4229b584c07b6e0ad101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
java里面是有堆内存的,堆内存对所有线程是共享的,除了共享的内存之外每个线程都会有自己的专属区域----即自己的工作内存;

> 当堆内存中有一个变量而线程去访问该变量的时候会将这个值拷贝到自己的工作空间中,然后是对这个值的任何改变当前都是体现在这个工作空间中的,需改结束后在写会到内存中,但是什么时候写回去就不得而知了(可能马上,也可能在其他时间)

所以在代码一里面,这个main线程里面修改完flag的值并没有及时反映到另一个线程里面,所以while(flag)会进入死循环,

而通过volatile修饰就可以修改完flag后会及时反映到内存中.其他线程也会在内存中及时得到最新值,所以代码二中的循环可以及时停止;

volatile 还会禁止指令重排序;

## 锁案例

### lock锁案例

```java

public class ThreadDemo01 {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();//可重入锁
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    lock.lock();
                    System.out.println("线程1开始---");
                    Thread.sleep(Integer.MAX_VALUE);
                    System.out.println("线程1结束---");
                } catch (InterruptedException e) {
                    System.out.println("线程1Interrupted");
                } finally {
                    lock.unlock();
                }

            }
        });
        thread.start();
        Thread thread1 = new Thread(() -> {
            try {
                 lock.lock();
              //lock.lockInterruptibly();//如果当前线程没有被中断,则获取锁,如果已经中断,则抛出异常
                System.out.println("线程2开始---");
                Thread.sleep(1000);
                System.out.println("线程2结束---");
            } catch (InterruptedException e) {
                System.out.println("线程2结束等待");
            } finally {

            lock.unlock();

            }
        });
        thread1.start();
        try {
            Thread.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        thread1.interrupt();//结束线程2的等待状态
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/84542f3c1b04459a9bb903ec0be449cf.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
由于线程1休眠时间足够长,那么线程2是拿不到这把锁的,这样线程2一直处于等待状态,怎么让线程2结束等待-------------中断,但是并没有产生实际效果


>lock.lock()是无法打断线程2获取锁的状态的,而如果是用 lock.lockInterruptibly(),就可以因为异常的抛出而结束线程等待状态;
线程在lock.lock()或者 synchronized 获取锁的时候中断并不会抛出异常,

但是用Lock锁会更灵活,lock.lockInterruptibly()方法获取锁的时候中断会抛出异常而结束线程等待状态;;

#### lockInterruptibly()

> 所以当线程2突然因为别的原因中断的时候,会有一个返回值.根据返回值而选择是否获取锁,这个返回值跟interrupt()中断线程相联动,如果线程中断,则会抛出异常而结束等待状态, 不然的话可能我们一直以为线程2处于等待状态-----看如下代码

```java

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class JoinDemo {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();//可重入锁
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    lock.lock();
                    System.out.println("线程1开始---");
                    Thread.sleep(Integer.MAX_VALUE);
                    System.out.println("线程1结束---");
                } catch (InterruptedException e) {
                    System.out.println("线程1Interrupted");
                } finally {
                    lock.unlock();
                }

            }
        });
        thread.start();
        Thread thread1 = new Thread(() -> {
            try {
                // lock.lock();
                lock.lockInterruptibly();//如果当前线程没有被中断,则获取锁,如果已经中断,则抛出异常
                System.out.println("线程2开始---");
                Thread.sleep(1000);
                System.out.println("线程2结束---");
            } catch (InterruptedException e) {
                System.out.println("线程2结束等待");
            } finally {
                if ("RUNNABLE".equals(Thread.currentThread().getState())) {
                    lock.unlock();
                } else {
                    //什么也不做
                }
            }
        });
        thread1.start();
        try {
            Thread.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        thread1.interrupt();//结束线程2的等待状态
    }
}
```

即 lock.lockInterruptibly()会对中断做出反应

![在这里插入图片描述](https://img-blog.csdnimg.cn/a966e769a7964c55b2c40fc8bafb44de.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

#### lock.trylock()

> 可以根据需要对代码进行加锁-----trylock()

```java
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @author : Gavin
 * @date: 2021/8/16 - 08 - 16 - 20:08
 * @Description: setTest
 * @version: 1.0
 */import java.util.concurrent.locks.Lock;


public class ThreadDemo01 {
    static  Lock lock = new ReentrantLock();//可重入锁
    static boolean flag =false;
    public static void main(String[] args) {

        String name="张三";

        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                     flag="张三".equals(name);
                    if(flag){//如果结果是想要的结果加锁
                        
                      /* tryLock(long timeout, TimeUnit unit)
                      如果在给定的等待时间内没有被另一个线程占用 ，
                      并且当前线程尚未被保留，则获取该锁*/
                   lock.tryLock(100,TimeUnit.SECONDS);
                        System.out.println("线程1开始---");
                        Thread.sleep(1000);
                        System.out.println("线程1结束---");
                        }

                } catch (InterruptedException e) {
                    System.out.println("线程1Interrupted");
                } finally {
                    if (flag){
                        lock.unlock();
                    }else{
                        System.out.println("不符合加锁条件");
                    }

                }

            }
        });
        thread.start();
        Thread thread1 = new Thread(() -> {
            try {
                // lock.lock();
              lock.lockInterruptibly();//如果当前线程没有被中断,则获取锁,如果已经中断,则抛出异常
                System.out.println("线程2开始---");
                Thread.sleep(1000);
                System.out.println("线程2结束---");
            } catch (InterruptedException e) {
                System.out.println("线程2结束等待");
            } finally {

            lock.unlock();

            }
        });
        thread1.start();
        try {
            Thread.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
       // thread1.interrupt();//结束线程2的等待状态
    }
}

```

可以根据是否满足条件来调整什么时间加锁,因此在lock锁比 synchronized更灵活一些;



### synchronized 锁案例

用  synchronized 来锁定------

```java
public class ThreadDemo01 {
    public static void main(String[] args) {
        //Lock lock = new ReentrantLock();//可重入锁
final  Object obj= new Object();
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (obj) {
                    try {
                        //lock.lock();
                        
                        System.out.println("线程1开始---");
                        Thread.sleep(Integer.MAX_VALUE);
                        System.out.println("线程1结束---");
                    } catch (InterruptedException e) {
                        System.out.println("线程1Interrupted");
                    } finally {
                        //lock.unlock();
                    }

                }
            }
        });
        thread.start();
        Thread thread1 = new Thread(() -> {
            synchronized (obj) {
                try {
                    // lock.lock();
                    // lock.lockInterruptibly();//锁打断

                    System.out.println("线程2开始---");
                    Thread.sleep(1000);
                    System.out.println("线程2结束---");
                } catch (InterruptedException e) {
                    System.out.println("线程2Interrupted");
                } finally {

                    // lock.unlock();

                }
            }
        });
        thread1.start();
        try {
            Thread.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        thread1.interrupt();//线程中断会释放锁
    }
}

```


尝试中断也没有效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/745b67dc8cb1495f985a64fe3180449b.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

#### synchronized锁升级

![](C:\Users\Gavin\Pictures\Camera Roll\45.jpg)



##### 锁升级

为了提升性能，JDK 1.6 引入了偏向锁（就是这个已经被 JDK 15 废弃了）、轻量级锁、重量级锁概念，来减少锁竞争带来的上下文切换，而正是新增的 Java 对象头实现了锁升级功能。


锁升级的过程

**new---->>>偏向锁----->>>轻量级锁---->>>重量级锁**

锁升级的过程是依靠新增的对象头实现的;

对象头都包含哪些内容??

mark word
class pointer
arraylength
instancedata
padding

其中markword记录了对象和锁的有关信息
在64位jvm中
![在这里插入图片描述](https://img-blog.csdnimg.cn/a79403836c514f9fafb00ef5a954d347.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)因此在java1.6之后对jvm进行了优化-------->>>即我们现在所说的锁升级过程;

new---->>>偏向锁----->>>轻量级锁---->>>重量级锁

#####  偏向锁

为什么要有偏向锁呢？偏向锁主要用来优化同一线程多次申请同一个锁的竞争。可能大部分时间一个锁都是被一个线程持有和竞争。假如一个锁被线程 A 持有，后释放；接下来又被线程 A 持有、释放……如果使用 monitor，则每次都会发生用户态和内核态的切换，性能低下。

作用：当一个线程再次访问这个同步代码或方法时，该线程只需去对象头的 Mark Word 判断是否有偏向锁指向它的 ID，无需再进入 Monitor 去竞争对象了。当对象被当做同步锁并有一个线程抢到了锁时，锁标志位还是 01，“是否偏向锁”标志位设置为 1，并且记录抢到锁的线程 ID，表示进入偏向锁状态。

一旦出现其它线程竞争锁资源，偏向锁就会被撤销。撤销时机是在全局安全点，暂停持有该锁的线程，同时坚持该线程是否还在执行该方法。是则升级锁；不是则被其它线程抢占。

> **在高并发场景下，大量线程同时竞争同一个锁资源，偏向锁会被撤销，发生 stop the world后，开启偏向锁会带来更大的性能开销（这就是 Java 15 取消和禁用偏向锁的原因），可以通过添加 JVM 参数关闭偏向锁：**

可重入锁的拿锁解锁过程
![在这里插入图片描述](https://img-blog.csdnimg.cn/04d6ad05238c43ba8aa5f6d9cd5ce6ce.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

##### 管程模型

![在这里插入图片描述](https://img-blog.csdnimg.cn/5ac32b55d5204bcaa78a5f4130b91b24.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


> 每个对象都与一个 monitor 相关联。当且仅当 monitor 对象有一个所有者时才会被锁定。执行 monitorenter 的线程试图获得与 objectref 关联的 monitor 的所有权，如下所示:
>   若与 objectref 相关联的 monitor 计数为 0，线程进入 monitor 并设置 monitor 计数为 1，这个线程成为这个 monitor 的拥有者。
>   如果该线程已经拥有与 objectref 关联的 monitor，则该线程重新进入 monitor，并增加 monitor 的计数。
>   如果另一个线程已经拥有与 objectref 关联的 monitor，则该线程将阻塞，直到 monitor 的计数为零，该线程才会再次尝试获得 monitor 的所有权。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5822f660e40942e9af85fa0b9a329524.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 主要的意思是说：
>   执行 monitorexit 的线程必须是与 objectref 引用的实例相关联的 monitor 的所有者。
>   线程将与 objectref 关联的 monitor 计数减一。如果计数为 0，则线程退出并释放这个 monitor。其他因为该 monitor 阻塞的线程可以尝试获取该 monitor。


当多个线程访问同步代码块时,多个线程会鲜卑放在entrylist集合中,抢到锁的则获得该对象的monitor,   而monitor是依靠底层OS的mutexlock实现的,线程申请mutex成功,则持有mutex;
如果当前线程wait()则会释放mutex,且该线程会被加入到 waitset集合中等待被唤醒;
![在这里插入图片描述](https://img-blog.csdnimg.cn/dcd5188cd5504600ae96d76a8b8e19fc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

由于monitor底层依赖于OS,存在用户态和内核态的切换 ,会增加系统开销;

##### 轻量级锁
拿到自旋锁的线程后如果有另一个线程来竞争,由于这个锁是偏向锁,则判断对象头的 Markword的线程ID不是自己的ID,就会进行CAS操作来视图获取锁----即视图将对象头中的id改为自己的id,成功则该锁依旧会保持为偏向锁,失败则标识锁有竞争,偏向锁会升级为轻量级锁;

当一个先后曾持有锁的时间很短,其他线程自旋很快就会拿到锁;
但是当轻量级锁的cas操作后一直拿不到锁,(一般是自旋10次或者等待的线程超过计算机核心数的一半)线程会挂起阻塞.此时锁升级为重量级锁;未抢到锁资源的线程都会进入monitor
若正在持有的所得线程释放了锁,那么刚刚进入阻塞队列的线程又要重新申请锁资源;


##### 锁优化

Synchronized 只在 JDK 1.6 以前性能才很差，因为这之前的 JVM 实现都是重量级锁，直接调用 ObjectMonitor 的 enter 和 exit。从 JDK 1.6 开始，HotSpot 虚拟机就增加了上述所说的几种优化：

    偏向锁
    轻量级锁
    自旋锁

其余还有：

    适应性自旋
    锁消除
    锁粗化

##### 锁消除

这属于编译器对锁的优化，JIT 编译器在动态编译同步块时，会使用逃逸分析技术，判断同步块的锁对象是否只能被一个对象访问，没有发布到其它线程。

如果确认没有“逃逸”，JIT 编译器就不会生成 Synchronized 对应的锁申请和释放的机器码，就消除了锁的使用。

##### 锁粗化

JIT 编译器动态编译时，如果发现几个相邻的同步块使用的是同一个锁实例，那么 JIT 编译器将会把这几个同步块合并为一个大的同步块，从而避免一个线程“反复申请、释放同一个锁“所带来的性能开销。

##### 减小锁粒度

我们在代码实现时，尽量减少锁粒度，也能够优化锁竞争。
总结

    其实现在 Synchronized 的性能并不差，偏向锁、轻量级锁并不会从用户态到内核态的切换；只有在竞争十分激烈的时候，才会升级到重量级锁。
    Synchronized 的锁是由 JVM 实现的。
    偏向锁已经被废弃了。


> **学无止境,当你知道的越多,会发现越无知!!!!!**

话不多说,开始扯......
在jdk1.6之后 jvm对synchronied进行了优化以使得申请锁的时候第一时间并不是重量级锁,而是偏向锁,轻量级锁最后才是重量级锁;

## synchronied锁的冰山一角

![在这里插入图片描述](https://img-blog.csdnimg.cn/5fd945f5df8d4b599f3408c82d1ae904.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## synchronized是用来干啥的,怎么用?
要学习synchronized就先要知道synchronized是用来干啥的怎么用.

### synchronized是用来干啥的??
**简言之~~**
synchronized被用来实现一种锁机制,用于多线程访问同已段代码块时保证操作能符合我们的要求,实现原子操作;
### synchronized怎么用??
一张图告诉你
![在这里插入图片描述](https://img-blog.csdnimg.cn/4064b0dd1bcf4b45ba453913f3fd9ca0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

##  分析synchronized上锁的过程
以修饰的对象入手----->>>>
先看一段代码,
代码1------<<<<
```java
public class NewHeaderDemo {
    public static void main(String[] args) throws InterruptedException {
        Object object= new Object();    
        System.out.println(ClassLayout.parseInstance(object).toPrintable());   
    }
}
```
代码很简单就是new一个对象,然后看一下这个对象的都存储结构~~说白了就是这个对象中都包含什么;
这里就要补充一些知识了;
**在JDK1.6之后,对每一个对象都新增了对象头这么一个属性,对象头是什么呢?新增对象头的目的又是什么呢???**

## java中的对象头
我们不妨看一下new的出一个对象是长什么样子的!!!
代码2-----<<<
```java
public class NewHeaderDemo {
    public static void main(String[] args) throws InterruptedException {
        Object object= new Object();
   System.out.println(ClassLayout.parseInstance(object).toPrintable());
    }
}

```
这就是一个对象长的样子;
![在这里插入图片描述](https://img-blog.csdnimg.cn/85835eea5f954333b25496892f566850.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

画图说明一下------>>>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/479027ba54304c4589b2c01aab429caa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/bd3b7ec9c5fa4f6f8ec0e1f0e9c1a37f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
我们让main线程睡5秒之后在次运行该程序----->>>>
代码3-----<<<<
```java

public class NewHeaderDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread.sleep(5000);
        Object object= new Object();
        System.out.println(ClassLayout.parseInstance(object).toPrintable());
    }
}

```
可以看到,这首new出来的对象加了偏向锁,**偏向锁里的指针指向空**
![在这里插入图片描述](https://img-blog.csdnimg.cn/4f5938626490482f9f84bda0f1db877b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
啥,我是怎么知道的?
看图每个对象头里面记录了很多信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/69f7e883c5274ee1ac3833cc4507f9eb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
为什么睡五秒就会开启偏向锁呢?
因为JVM在加载的时候会有好多线程来竞争呀,偏向锁设计的目的就是当一个线程.....(先不说那么多)
jvm在加载的时候会默认偏向锁开启延迟 一般是4秒,所以主线程睡五秒后会开启偏向锁的;

代码4-----------<<<<

```java
public class NewHeaderDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread.sleep(5000);
        Object object= new Object();
        synchronized (object){
            System.out.println(ClassLayout.parseInstance(object).toPrintable());
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/54f95aa68d9e4e949f4c287c14f4fa83.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

代码5----<<<<<
```java
public class NewHeaderDemo {
    public static void main(String[] args) throws InterruptedException {
        Object object= new Object();
        synchronized (object){
            System.out.println(ClassLayout.parseInstance(object).toPrintable());
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/71a2b34605e54aafa06b16e94e66b85f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
由代码4和5可以看到当偏向锁开启时sycn会优先加偏向锁,当偏向锁没有开启时会加轻量级锁;

> 但是这些在JDK15之后发生了改变,因为JDK15废弃了偏向锁,而且在以后的版本中也会遭到移除,为什么会这样子呢?

这要了解偏向锁设计的初衷------------------------>>>>>>
**偏向锁设计的目的就是用来优化同一线程多次申请同一个锁的竞争。可能大部分时间一个锁都是被一个线程持有和竞争。假如一个锁被线程 A 持有，后释放；接下来又是被线程 A 持有、释放……(即只有一个线程频繁的申请锁和释放锁),这时如果使用 monitor(管程)，则每次都会发生用户态和内核态的切换，这是非常消耗系统资源的,因此直接使用monitor性能低下。**


那为什么JDK15之后又要舍弃偏向锁呢?
这也要了解时代在发展,在JDK1.6的时候高并发场景还不那么突出,但是现在高并发场景下,取消偏向锁是考虑到高并发情况下 偏向锁的撤销会非常消耗系统资源的;
**原因如下,假如有10000个线程来访问同步资源,那么会进行锁升级,偏向锁的撤销肯定会发生,一旦偏向锁撤销发生,那么就会使得持有偏向锁得线程执行暂停-----即stop the world ,同时又要记录其他线程的锁状态这是非常消耗资源的(你可以参考一下你在win系统上打开任务管理器,你会发现CPU利用率抖升 达到100%因为统计所有线程的内存信息以及其他信息十分消耗CPU的计算能力);**


**当有了偏向锁和轻量级锁,就不用每次去操作系统底层申请重量级锁了;**

synchronized 锁的状态反映在字节码信息里为monitorenter与monitorexit
![在这里插入图片描述](https://img-blog.csdnimg.cn/bf7f75b26d2048aa8ff55966e5a71987.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**为什么会有两个monitorexit呢,**

> 因为除了正常执行完毕锁的释放还有一种异常/休眠也会使得锁的释放,所以会有两个;

synchronized是可重入锁,
代码6----<<<
体现在一是父类同步的方法可以被子类覆写,
```java

import org.openjdk.jol.info.ClassLayout;
class Two{
    public synchronized  void add(){
        System.out.println("父类");
    }
}
class Three extends Two{
    public synchronized  void add(){
        System.out.println("子类");
    }
}
public class NewHeaderDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread.sleep(5000);
        Two two= new Three();
        two.add();
        synchronized (two){
            System.out.println(ClassLayout.parseInstance(two).toPrintable());
        }
    }
}
```
另一个是同一个对象拿到锁之后对于同一把锁的代码依旧是可以访问的;
![在这里插入图片描述](https://img-blog.csdnimg.cn/c8ddaf61aef54aa7b5254e47e60cdda0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


于是这就会涉及到一种优化方式------>>>
**锁粗化**
锁粗化是指JVM在编译时会检查代码块上的锁是否是同一把锁,如果是,会将代码块放到一块,然后加一把范围大的锁,

代码7--------------<<<<<<<<
```java

class Two{
    public synchronized  void add(){
        System.out.println("父类");
    }
}
class Three extends Two{
    public synchronized  void add(){
        System.out.println("子类");
    }
}
public class NewHeaderDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread.sleep(5000);
        Two two= new Three();
        Three three= new Three();
        synchronized (two){
           int a=100;
            System.out.println(a);
        }

        synchronized (two){
            int c=600;
            System.out.println(c);
        }

    }
}
```

## 可重入锁是如何记录重入次数?

偏向锁和轻量级锁将重入次数记录在线程栈里,----线程栈里记录的是LockRecord,每重入一次记录一次,第二次只记录次数不记录指针,每释放一次锁,则出栈一次,直至为0
如图所示----->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/7da948e2257a438dab8c2b721be5958f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



这是JVM底层的动态编译机制实现的,在我们日常写代码时也要合理的运用锁粗/细化;


锁粗化的对立面------>>>>**锁细化**
就是有些代码并没有涉及到原子性的操作问题,那么就可以经该代码拿到锁的外部以减少上锁时间;


还有一种优化机制------>>>>>>>**锁消除,**
在预编译时会检查锁是否只有一个线程去使用,如果是那么就会消除锁来提高运行效率;


## 锁的升级过程------>>>>>

### 初始阶段

在JDK15之前,当我们new一个对象时,如果该对象在偏向锁开启之前 new,则不会加锁 ,为无锁状态对象头信息----001
当偏向锁开启之后在new一个对象,则这个对象会被添加一个偏向锁(匿名偏向锁) 对象头信息---101

当我们给对象加synchronized锁时,若偏向锁未开启则默认首先添加轻量级锁,对象头信息--00如果偏向锁开启了,则默认首先添加偏向锁;对象头信息--101

### 锁升级阶段

#### 偏向锁升级为轻量级锁

**偏向锁不升级的情况------>>>>**

> **当持有偏向锁的线程执行完任务,这时候才来了另一个线程,那么该线程会首先CAS操作查看对象头里的偏向锁指针是否是自己的ID,如果不是则尝试CAS操做修改对象头的信息,修改成功,则该锁依旧是偏向锁;**


**偏向锁升级为轻量级锁------>>>>>**

> **当一个线程持有偏向锁,这时候另一个线程来抢这把锁,那么此时偏向锁会被撤销发生stop the world (即抢这把锁的线程(包括向前持有偏向锁的线程)都要暂停执行,同时记录一下来抢这把锁的线程的锁状态以及其他信息),偏向锁撤销之后所有想要该锁的线程来抢这把锁,一个线程抢到了,则修改对象头信息 为00,其他为抢到锁的线程采取自旋的方式来继续尝试拿到这把锁;**
>
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0b6ed79a266f4eb5bfd732a2b1885ad6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 轻量级锁升级为重量级锁

> **当自旋次数超过十次(JDK对此也做了优化,现在叫自适应自旋,自旋次数也是不确定的)或者自选的线程数超过CPU核心数的半,则会升级为重量级锁;**

### 有了自旋锁为什么还需要重量级锁?

> **这是因为自旋锁要消耗cpu的,当线程竞争很激烈的时候自旋锁使得cpu吃不消,所以要升级重量级锁,重量级锁对比自旋锁有什么不同呢?**


### 重量级锁的实现---->>>>

说到重量级锁就要讲一下Monitor这个知识,
从哪开始讲呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/7f6011caee2a4730b198bc0b61cd1966.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
就从monitor开始吧,每一个对象都与一个monitor相关联,
JVM中的同步就是基于objectmonitor的进入和退出实现的;而objectmonitor是由c++代码实现的(这是操做系统内部的)
当且仅当monitor对象有一个所有者的时候才会被锁定,执行monitorenter的线程试图获得objectref关联的所有权;
若与objectref关联的monitor计数为0,则线程进入这个monitor 并增加monitor的计数;
如果monitor中的计数不为0,则该线程进入阻塞状态并加入相应队列,直到monitor的计数为0,则会从队列中拿出一个线程尝试获得该monitor的所有权


```cpp
ObjectMonitor() {
   _header = NULL;
   _count = 0; //记录个数
   _waiters = 0,
   _recursions = 0;
   _object = NULL;
   _owner = NULL;
   _WaitSet = NULL; //处于wait状态的线程，会被加入到_WaitSet
   _WaitSetLock = 0 ;
   _Responsible = NULL ;
   _succ = NULL ;
   _cxq = NULL ;
   FreeNext = NULL ;
   _EntryList = NULL ; //处于等待锁block状态的线程，会被加入到该列表
   _SpinFreq = 0 ;
   _SpinClock = 0 ;
   OwnerIsThread = 0 ;
}

```
### 读写锁

```java

import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

/**
 * @author Gavin
 */
public class ReadWriteLockDemo {
    /**
     * @description 准备读写锁
     */
    static Lock lockS = new ReentrantLock(true);
    ReadWriteLock readWriteLock = new ReentrantReadWriteLock(true);
    /**
     * @author Gavin
     * @decription 读锁
     */
    Lock readLock = readWriteLock.readLock();
    Lock writeLock = readWriteLock.writeLock();
    //这是读写的内容
    static String str = "HelloWorld";
static void TIMESleep(int time){
    try {
        TimeUnit.SECONDS.sleep(time);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
    public static void readM( Lock lock) {
       lock.lock();
            System.out.println("读取的值为---"+str);
             TIMESleep(1);
            lock.unlock();
        }
public static void writeM(Lock lock ,String change){
    lock.lock();

    System.out.println("正在修改...");
    TIMESleep(1);
    str=change;
    System.out.println("修改后的值----"+str);
    lock.unlock();
}

    public static void main(String[] args) {


        for (int i = 0; i < 16; i++) {
            new Thread(()->{
                ReadWriteLockDemo.readM(lockS);
            }).start();
        }
        for (int i = 0; i < 4; i++) {
            new Thread(()->{
                ReadWriteLockDemo.writeM(lockS,"世界你好");
            }).start();
        }
    }
}

```

```java

/**
 * @author Gavin
 * @description 读锁与写锁的运用
 */


import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.*;

/**
 * @author Gavin
 */
public class RWLockDemo {
    /**
     * @description 准备读写锁
     */
    static Lock lockS = new ReentrantLock(true);
    static ReadWriteLock readWriteLock = new ReentrantReadWriteLock(true);
    static Lock readLock = readWriteLock.readLock();
    static Lock writeLock = readWriteLock.writeLock();
    //这是读写的内容
    static String str = "HelloWorld";

    static void TIMESleep(int time) {
        try {
            TimeUnit.SECONDS.sleep(time);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void readM(Lock lock) {
        lock.lock();
        System.out.println("读取的值为---" + str);
        TIMESleep(1);
        lock.unlock();
    }

    public static void writeM(Lock lock, String change) {
        lock.lock();

        System.out.println("正在修改...");
        TIMESleep(1);
        str = change;
        System.out.println("修改后的值----" + str);
        lock.unlock();
    }

    public static void main(String[] args) {


        for (int i = 0; i < 16; i++) {
            new Thread(() -> {
                ReadWriteLockDemo.readM(readLock);
            }).start();
        }
        for (int i = 0; i < 4; i++) {
            new Thread(() -> {
                ReadWriteLockDemo.writeM(writeLock, "改变世界");
            }).start();
        }

    }
}

```

刚刚又犯了一个错误------>>>

```java
package com.thread.gavin.ThreadDemo.company;
import java.util.concurrent.Phaser;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;
class ReadRule extends Phaser {
    static long start = 0;
    static long end = 0;

    @Override
    protected boolean onAdvance(int phase, int registeredParties) {
        switch (phase) {
            case 0:
                System.out.println("准备完毕---所有人都到齐了" + registeredParties);
                return false;
            case 1:
                System.out.println("读取中---" );
                return false;
            case 2:
                System.out.println("读取用时" +  (end - start));
                return true;
            default:
                return true;

        }

    }
}

class Person implements Runnable {
    String name;
   static final Lock lock = new ReentrantLock(true);//
ReadWriteLock RWLock=new ReentrantReadWriteLock(true);
 Lock readLock= RWLock.readLock();
static ReadRule readRule=new ReadRule();

    public Person(String name) {
        this.name = name;
    }

    public void SleepT(int time) {
        try {
            TimeUnit.SECONDS.sleep(time);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void StartRead() {
        ReadRule.start = System.currentTimeMillis();
readRule.arriveAndAwaitAdvance();
    }

    public void Reading(Lock lock) {
        lock.lock();
        SleepT(1);

        lock.unlock();
readRule.arriveAndAwaitAdvance();
    }

    public void EndRead() {
       ReadRule.end=System.currentTimeMillis();
readRule.arriveAndAwaitAdvance();
    }

    @Override
    public void run() {
        StartRead();
        Reading(lock);
        EndRead();
    }
}


public class ReadWriteTime {
    public static void main(String[] args) {
Person.readRule.bulkRegister(10);
        for (int i = 0; i < 10; i++) {
            new Thread(new Person("学生")).start();
        }
    }
}
```
这里如果不是final修饰则每个线程会自己建自己的,这样锁就失效了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a10a1c31ae14402194316a8401b6fbe5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## LockSupport
**类继承实现关系**
***public class LockSupport extends Object***
用于创建锁和其他同步类的基本线程阻塞原始体。

> static Object	getBlocker(Thread t)	 返回提供给最近调用的尚未解锁的公园方法的拦截对象，或未阻止的空。

>
> static void	park()	 
> 除非有许可证，否则将当前线程禁用以用于线程调度目的


> static void	park​(Object blocker)	
>  除非有许可证，否则将当前线程禁用以用于线程调度目的。
>

> static void	parkNanos​(long nanos)	
>
> 禁用当前线程以用于线程调度目的，最长时间等待，除非有许可证。
>
> 

> static void	parkNanos​(Object blocker, long nanos)	
>
> 禁用当前线程以用于线程调度目的，最长时间等待，除非有许可证。
>
> 

>
> static void	parkUntil​(long deadline)	
> 禁用当前线程用于线程调度目的，直到指定的截止日期，除非有许可证可用。
>

> static void	parkUntil​(Object blocker, long deadline)	
> 禁用当前线程用于线程调度目的，直到指定的截止日期，除非有许可证可用。
>

> static void	unpark​(Thread thread)	 如果尚未提供，请为给定线程提供许可证。


根据类中的方法-----全是static修饰,可以用类名直接调用方法

**案例演示----**

```java

import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.LockSupport;

public class LockSupportDemo {
    public static void main(String[] args) {
       Thread t= new Thread(() -> {
            for (int i = 0; i < 20; i++) {
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if (i == 10) {
                    //使得当前线程休眠
                    LockSupport.park();
                }
                System.out.println(i);
            }
        });
        t.start();
      

```
运行结果----->>>到9的时候线程阻塞了,代码中也没有出现同步方法/锁
![在这里插入图片描述](https://img-blog.csdnimg.cn/40b9a06cf5404894bb63bd44cca7b643.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)修改代码如下

```java

public class LockSupportDemo {
    public static void main(String[] args) {
       Thread t= new Thread(() -> {
            for (int i = 0; i < 20; i++) {
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if (i == 10) {
                    //使得当前线程休眠
                    LockSupport.park();
                }
                System.out.println(i);
            }
        });
        t.start();
       
        LockSupport.unpark(t);
    }
}
```
**由于线程开启之后紧接着调用唤醒方法,就相当于 t线程没有被阻塞过一样;**
**如果一个线程被阻塞过两次---------------->>>>**

```java
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.LockSupport;
public class LockSupportDemo {
    public static void main(String[] args) {
       Thread t= new Thread(() -> {
            for (int i = 0; i < 20; i++) {
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if (i == 8) {
                    //使得当前线程休眠
                    LockSupport.park();
                }
                if(i==16)
                    //使得当前线程休眠
                    LockSupport.park();
                System.out.println(i);
            }
        });
        t.start();

        LockSupport.unpark(t);
        LockSupport.unpark(t);
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d3aef87fed804878abab6ba40bf58ccb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)什么原理呢?

先来一点点分析-----

```java
public class LockSupportDemo {
    public static void main(String[] args) {
        LockSupport.park();
        System.out.println("block.");
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/31908a5077fb4e22abc4377bb8b32b8e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)没什么问题吧!
再来

```java

public class LockSupportDemo {
    public static void main(String[] args) {
        Thread thread=Thread.currentThread();
        LockSupport.unpark(thread);//释放许可 
        LockSupport.park();//获取许可
        System.out.println("block");

    }
}
```
# Java中Sycn

先写一段代码然后再来分析------>>>
代码很简单就是有一个公共的数,,开五个线程,每个线程加2,最后得到最后的数的值;
```java
package com.thread.gavin.ThreadDemo.company;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class RetrenLockDemo {
    static  int count=0;
    public static void main(String[] args) throws InterruptedException {
         Lock lock = new ReentrantLock();
         //准备1000个线程
        Thread [] threads= new Thread[1000];
        for (int i = 0; i < 1000; i++) {
            //每个线程加10

          threads[i]= new Thread(()->{
              synchronized (RetrenLockDemo.class){
                for (int j = 0; j< 10; j++) {
                       count++;
                   }
                  //  lock.unlock();
                }
            });
        }
        for (Thread th :
                threads) {
            th.start();
            th.join();
        }
        System.out.println("count--"+count);
    }
}

```
源码分析------>>>>

再分析源码之前有一个知识还得补充----
ThreadLocal

先来看一下吧---->>>>
ThreadLocal类信息
**public class ThreadLocal<T> extends Object**

> **此类提供线程局部变量。 这些变量不同于 它们的正常对应物在于每个线程访问一个（通过其 get或者 set方法）有自己的，独立初始化 变量的副本。 ThreadLocal实例通常是私有的 希望将状态与线程相关联的类中的静态字段（例如， 用户 ID 或交易 ID）。**

 构造方法

![在这里插入图片描述](https://img-blog.csdnimg.cn/7606f05c3302460f9b679c9bec8e50c3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

常用方法![在这里插入图片描述](https://img-blog.csdnimg.cn/1f9a52f405f44fcd86a667d201253387.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)代码很简单---->>>

```java

public class ThreadLocalDemo {
    public static void main(String[] args) {
        new Thread(()->{
            ThreadLocal<Integer>threadLocal= new ThreadLocal<>();
            threadLocal.set(100);
            Integer integer = threadLocal.get();
            System.out.println(integer);
        });
    }
}
```
我对这个Integer做出的修改对别的线程是不可见的;所以 ThreadLocal的作用方向-----实现事务的隔离

![](C:\Users\Gavin\Pictures\Camera Roll\47.png)



> 起晚了,都说三更灯火五更鸡,奈何熬夜伤身体;偏偏秉烛夜游.....................

# JAVA中的ThreadLocal

> **public class ThreadLocal<T> extends Object**
> 此类提供线程局部变量。 这些变量不同于 它们的正常对应物在于每个线程访问一个（通过其 get或者 set方法）有自己的，独立初始化的 变量的副本。 ThreadLocal实例通常是私有的 希望将状态与线程相关联的类中的静态字段（例如， 用户 ID 或交易 ID）。 

**只要线程是活的并且实例是可访问的，每个线程都暗中引用其线程本地变量的副本**：线程消失后，其线程本地实例的**所有副本都受到垃圾收集的约束**（除非存在对这些副本的其他引用）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/768220d05dd64e2592d731bb82b82933.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**代码案例**

```java

import java.util.concurrent.TimeUnit;

class Person{
   volatile String name="张三";
    public Person(){

    }
}
public class ThreadLocalDemo {
    static  ThreadLocal<Person>tl= new ThreadLocal<>();
    public static void main(String[] args) {
        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(tl.get());
        }).start();

        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            tl.set(new Person());
            System.out.println(tl.get().name);
        }).start();
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7bda99d3f3254a0690cab18df7ff2eef.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)一个线程修改name,但是对于另一个线程依然是不可见的,即每个线程操做的都是一个副本;

代码对比----->>

```java

import java.util.concurrent.TimeUnit;

class Person{
   volatile String name="张三";
    public Person(){

    }
}
public class ThreadLocalDemo {
   /* static  ThreadLocal<Person>tl= new ThreadLocal<>();*/
    static Person tl= new Person();
    public static void main(String[] args) {

      new Thread(()->{
          try {
              TimeUnit.SECONDS.sleep(2);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          System.out.println(tl.name);
      }).start();

      new Thread(()->{
          try {
              TimeUnit.SECONDS.sleep(1);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
        tl.name="李四";
      }).start();
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7b51f9b331fc473baebfd7ac4fbfe122.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
图解ThreadLocal原理------->>>>>

对比结果可以发现ThreadLocal类的作用-----实现本地变量的副本!

![](C:\Users\Gavin\Pictures\Camera Roll\48.png)

小结---->
ThreadLocal用在访问一个基础变量,每个线程再对这个变量做出的修改仅对当前线程有效,对其他线程无影响;



# JAVA中的VarHandle类
继承实现关系----->>

>**public abstract class VarHandle extends Object**

描述:
VarHandle 是对**变量或对 参数化定义的变量引用**，包括静态字段， 非静态字段、数组元素或堆外数据的组件 结构体。 在各种支持下**访问这些变量 访问模式** ，包括普通读/写访问、易失性 读/写访问和比较和设置。VarHandles 是不可变的，没有可见的状态。

![](C:\Users\Gavin\Pictures\Camera Roll\49.png)


**一个 VarHandle 有：**

引用的每个变量的类型  通过这个 VarHandle 来指向 这个variable type T； 和coordinate types CT1, CT2, ..., CTn的访问类型的 坐标的清单 表达 该 共同定位此 VarHandle 引用的变量。 
 什么意思呢?
 就是说VarHandle通过  coordinate 和 variable type 来共同定位这个引用;看一下代码就理解了!
代码1---------->>>
基本类型
```java
class Test{
    String name="扎根三";
    int count=99;
}
public class VarHandleDemo {
    int count=100;
    static VarHandle varHandle;
    static VarHandle varHandle1;
    public static void main(String[] args) {
        VarHandleDemo var= new VarHandleDemo();
        Test test= new Test();
        try {
            varHandle= MethodHandles.lookup().findVarHandle(VarHandleDemo.class,"count",int.class);
            varHandle1= MethodHandles.lookup().findVarHandle(Test.class,"count",int.class);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
//        CAS操做
       varHandle.compareAndSet(var,100,500);
       varHandle1.compareAndSet(test,99,250);

        System.out.println(var.count);
        System.out.println(test.count);

        Class<?> aClass = varHandle.varType();//返回VarHandle引用的变量类型
        Class<?> aClass1 = varHandle1.varType();//返回VarHandle引用的变量类型
        System.out.println(aClass);
        System.out.println(aClass1);
        System.out.println("------------------");
        List<Class<?>> classes = varHandle.coordinateTypes();
        for ( Class c:classes
             ) {
            System.out.println(c);
        }
        System.out.println("------------------");
        List<Class<?>> classes1 = varHandle1.coordinateTypes();
        for ( Class c:classes1
             ) {
            System.out.println(c);
        }
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f9f4172d26b14e00a979ca6bc60089a1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**变量和坐标类型可以是原始类型(即基本类型)或引用类型，**并由Class对象表示。 坐标类型列表可以为空。
生产或 lookupVarHandle 实例记录了支持的变量类型和列表 坐标类型。 
代码2------>>
引用类型
```java
class Test{
    String name;
    int count=99;
    public Test() {
    }

    public Test(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Test{" +
                "name='" + name + '\'' +
                '}';
    }
}
public class VarHandleDemo {
    static VarHandle varHandle;
    Test test = new Test("张三");

    public static void main(String[] args) throws Exception {
        VarHandleDemo var= new VarHandleDemo();
        varHandle= MethodHandles.lookup().findVarHandle(VarHandleDemo.class,"test",Test.class);//引用类型
          Class<?> aClass = varHandle.varType();//返回VarHandle引用的变量类型class com.gain.VarHandle.Test
//      反射练习
        Class<Test> t = (Class<Test>) varHandle.varType();
        Constructor<Test> constructor = t.getConstructor(String.class);
        Test test = constructor.newInstance("李四");
        System.out.println(test);
        TypeVariable<Constructor<Test>>[] typeParameters = constructor.getTypeParameters();
        System.out.println(typeParameters);
        System.out.println("------------------");
        //练习结束
        List<Class<?>> classes = varHandle.coordinateTypes();
        for ( Class c:classes
             ) {
            System.out.println(c);
        }

    }
}
```
API中的案例

```java
 String[] sa = ...
 VarHandle avh = MethodHandles.arrayElementVarHandle(String[].class);
 boolean r = avh.compareAndSet(sa, 10, "expected", "new");
```


## VarHandle的原子操做
VarHandle通过控制变量的访问模式来实现原子性和一致性的;

> **访问模式控制**原子性和一致性属性。
> 普通读（ get） 和写 （ set) 保证访问仅对引用是按位原子的 并且对于最多 32 位的原始值，并且不施加可观察的 与线程相关的排序约束，而不是 执行线程。
> 不透明 操作是按位原子的，并且 **对同一变量的访问顺序一致**。 除了遵守 Opaque 属性外， Acquire 模式 读取及其后续访问在匹配后排序 释放 模式写入及其之前的访问。 在 除了遵守 Acquire 和 Release 属性，所有 易失性 操作彼此是完全有序的 。


总体的访问模式---->>>

> **读取指定的变量值的访问模式** 记忆排序效应。 属于该组的相应访问模式方法集 由方法组成 get, getVolatile, getAcquire, getOpaque.
>

> **将变量的值设置为指定值的写访问模式** 记忆排序效应。 属于该组的相应访问模式方法集 由方法组成 set, setVolatile, setRelease, setOpaque.
>
### set与get
> public final Object get ( Object ... args)

返回一个变量的值，读取的内存语义为 如果变量被声明为**非 volatile. 通**常称为 作为普通读访问。 

> public final void set ( Object ... args)

将变量的值设置为 newValue，有记忆 设置的语义就像变量被声明为**非 volatile 和非 final.** 通常称为普通写访问。 

### setVolatile与getVolatile

> public final Object getVolatile ( Object ... args)

返回一个变量的值，具有读取的内存语义 if 变量被声明 volatile. 

> public final void setVolatile ( Object ... args)

将变量的值设置为 newValue，有记忆 设置的语义就像变量被声明一样 volatile. 

**将变量设置为Volatile类型和获得该变量**  即禁止指令重排序
代码3----->>>
```java
import java.lang.invoke.MethodHandles;
import java.lang.invoke.VarHandle;
class VarTest {
    private  int flag = 0;
    static VarHandle varHandle;
    static {
        try {
            varHandle = MethodHandles.lookup().findVarHandle(VarTest.class, "flag", int.class);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }
    void setFlag() throws Exception {
        varHandle.setVolatile(this, 1);
    }
    int getFlag() {
        int aVolatile = (int) varHandle.getVolatile(this);
        return aVolatile;
    }
}

public class AtomicVarHandleDemo {
    public static void main(String[] args) throws Exception {
        VarTest VT= new VarTest();
        Thread tt = new Thread(() -> {
            while (VT.getFlag() == 0) {
                System.out.println(VT.getFlag());
            }
        });

        Thread t = new Thread(() -> {
            try {
                VT.setFlag();
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
        tt.start();
        Thread.sleep(100);
        t.start();
    }
}
```

### setRelease与getAcquire

> public final Object getAcquire ( Object ... args)

返回变量的值，并确保后续加载和 在此访问之前，不会重新排序。

>  public final void setRelease ( Object ... args)

将变量的值设置为 newValue，并确保 在此访问之后，先前的加载和存储不会重新排序。


返回变量的值，并确保后续加载和 在此访问之前，商店不会重新排序。 
将变量的值设置为 newValue，并确保 在此访问之后，**先前的加载和存储不会重新排序。**  即禁止指令重排序

```java
import java.lang.invoke.MethodHandles;
import java.lang.invoke.VarHandle;
class VarTest {
    private  int flag = 0;
    static VarHandle varHandle;
    static {
        try {
            varHandle = MethodHandles.lookup().findVarHandle(VarTest.class, "flag", int.class);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }
    void setFlag2() throws Exception {
        varHandle.setRelease(this, 1);
    }
    int getFlag2() {
        int aVolatile = (int) varHandle.getAcquire(this);
        return aVolatile;
    }
}

public class AtomicVarHandleDemo {
    public static void main(String[] args) throws Exception {
        VarTest VT= new VarTest();
        Thread tt = new Thread(() -> {
            while (VT.getFlag2() == 0) {
                System.out.println(VT.getFlag());
            }
        });

        Thread t = new Thread(() -> {
            try {
                VT.setFlag2();
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
        tt.start();
        Thread.sleep(100);
        t.start();
    }
}

```

### setOpaque与getOpaque

> public final Object getOpaque ( Object ... args)

返回一个变量的值，按程序顺序访问，但没有 保证相对于其他线程的内存排序效果。 

> public final void setOpaque ( Object ... args)

将变量的值设置为 newValue，按程序顺序， 但不能保证相对于其他的记忆排序效果 线程。


按程序顺序将变量的值设置为newValue，但不保证相对于其他线程的内存顺序影响。---即不保证指令不重排序
```java
import java.lang.invoke.MethodHandles;
import java.lang.invoke.VarHandle;
class VarTest {
    private  int flag = 0;
    static VarHandle varHandle;
    static {
        try {
            varHandle = MethodHandles.lookup().findVarHandle(VarTest.class, "flag", int.class);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }
  
    void setFlag3() throws Exception {
        varHandle.setOpaque(this, 1);
    }
    int getFlag3() {

        int aVolatile = (int) varHandle.getOpaque(this);
        return aVolatile;
    }
}

public class AtomicVarHandleDemo {
    public static void main(String[] args) throws Exception {
        VarTest VT= new VarTest();
        Thread tt = new Thread(() -> {
            while (VT.getFlag2() == 0) {
                System.out.println(VT.getFlag());
            }
        });

        Thread t = new Thread(() -> {
            try {
                VT.setFlag2();
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
        tt.start();
        Thread.sleep(100);
        t.start();
    }
}

```
针对一个变量,再高并发情况下,再JDK9之后java带来了新特性varhandle来替代一些JDK9之前的原子操做的方式(比如原子类AtomicInteger,AtomicLong等);

**回想一下之前学过的高并发下的三大特性----原子性,可见性,有序性
实现原子性------加锁,CAS操做,原子类**

> 加锁的话需要对线程进行同步，在高并发下,线程要上下文切换,这时给系统带来的开销是很大的，
>

> 使用Atomic包下的原子类进行间接管理，但增加了开销，也可能导致额外的问题如ABA问题；
>

> 使用原子性的FieldUpdaters，利用反射机制，开销也会增大；
>
> 直接操做底层,但是考虑到安全性和可移植性上
> 使用sun.misc.Unsafe提供的JVM内置函数，但直接操作JVM可能会损害安全性和可移植性；

针对以上的问题，VarHandle就是用来替代上述方式的一种方案，它提供了一系列标准的**内存屏障操作**，用于**更细粒度的控制指令排序**，在安全性、可用性、性能等方面都要优于现有的Api，且基本上能够和任何类型的变量相关联。

> **原子更新访问模式**，例如，原子地比较和设置 指定内存排序效应下的变量值。 属于该组的相应访问模式方法集 由方法组成
> compareAndSet, weakCompareAndSetPlain, weakCompareAndSet,
> weakCompareAndSetAcquire, weakCompareAndSetRelease,
> compareAndExchangeAcquire, compareAndExchange,
> compareAndExchangeRelease, getAndSet, getAndSetAcquire,
> getAndSetRelease.
>
> 







>  数字原子更新访问模式**，例如，原子地获取和 设置指定内存顺序下的变量值 效果。 属于该组的相应访问模式方法集 由方法组成 getAndAdd, getAndAddAcquire, getAndAddRelease,
>

>    位原子更新访问模式，例如，原子地获取和 按位或指定内存顺序下的变量值 效果。 属于该组的相应访问模式方法集 由方法组成 getAndBitwiseOr, getAndBitwiseOrAcquire, getAndBitwiseOrRelease,
>    getAndBitwiseAnd, getAndBitwiseAndAcquire, getAndBitwiseAndRelease,
>    getAndBitwiseXor, getAndBitwiseXorAcquire, getAndBitwiseXorRelease.

# java中的引用类型
定义: 由类型的实际值引用（类似于C中的指针）表示的数据类型。如果为某个变量分配一个引用类型，则该变量将引用（或“指向”）原始值。不创建任何副本。引用类型包括类、接口、委托和装箱值类型。

> 关于引用类型最大的父类(除了Object) 

public abstract class Reference<T> extends  Object

## 引用类型的分类
**在java中有四种引用类型,强,软,弱,虚**

在JDK 1.2以前的版本中，若一个对象不被任何变量引用，那么程序就无法再使用这个对象。也就是说，只有对象处于(reachable)可达状态，程序才能使用它。

从JDK 1.2版本开始，对象的引用被划分为4种级别，从而使程序能更加灵活地控制对象的生命周期。这4种级别由高到低依次为：强引用、软引用、弱引用和虚引用。

Reference<T>  的子类---->>
PhantomReference(虚引用),
SoftReference(软引用),
WeakReference(弱引用),
这些都跟GC回收有关,因为父类Reference<T> 是跟GC有关的;

### 强引用

public abstract class Reference<T> extends Object

**引用对象的抽象基类**。 这个类定义了 所有引用对象通用的操作。 因为引用对象是 与垃圾收集器密切合作实现，这个类可以 不能直接子类化。 

#### 强引用API

强引用是使用最普遍的引用。如果一个对象具有强引用，那垃圾回收器绝不会回收它

![在这里插入图片描述](https://img-blog.csdnimg.cn/c8fa796dac03491597d3e405358218a6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)代码3----->
```java
import java.io.IOException;

class N{
    @Override//该方法已弃用,但是为了演示一下,还是用一下吧!
    protected void finalize(){
        System.out.println("已回收");
    }
}
 public class M {
     public static void main(String[] args) throws IOException {
         N n = new N();
          n=null;//当n没有指向任何对象时,就会被垃圾回收器回收;至于jvm什么时间回收是不确定的;
         System.gc();//手动调一下回收;
         System.in.read();
     }

}
```
由于强引用是普遍的,所以reference是一个抽象类;

get()-----拿到引用
clear()方法----清除引用
enqueue()---清除引用，如果有队列的话则将其加入到队列中,返回值为boolean
isEnqueued()----检查是否被清除过,如果在引用创建前没有在队列中注册----即没有调用enqueue方法(或者引用创建时的构造方法参数无RQ),那么永远返回false;
代码1----->
```java
   ReferenceQueue RQ= new ReferenceQueue();
     SoftReference<byte[]>softReference= new SoftReference<>(new byte[5*1024*1024],RQ);
        SoftReference<byte[]>softReference2= new SoftReference<>(new byte[5*1024*1024],RQ);
        SoftReference<byte[]>softReference3= new SoftReference<>(new byte[5*1024*1024],RQ);
        System.out.println(softReference.get());//拿到引用
      /*  Thread.sleep(200);*/
       // softReference.enqueue();//清除引用并将其放入队列中
         softReference.clear();//清除引用
        System.out.println(softReference.isEnqueued());//检查是否被enqueue过   enqueue()方法 得到 true,clear()得到false
        System.out.println(softReference.get());

```

 reachabilityFence​(Object ref)

> **确保给定引用所引用的对象保持不变 强可及 ， 无论程序之前的任何可能导致 变得无法访问的对象**； 因此，引用的对象不是 至少在调用之后可以通过垃圾收集回收 这种方法。 调用此方法本身不会启动垃圾 收集或完成。
> 代码2----->
```java
public class SoftReferenceDemo {
    public static void main(String[] args) throws InterruptedException {
        //设置堆内存大小为20M
ReferenceQueue RQ= new ReferenceQueue();
SoftReference<byte[]> softReference= new SoftReference<>(new byte[10*1024*1024]);
        Reference.reachabilityFence(softReference);
        System.gc();
        Thread.sleep(500);
        byte []bytes=new byte[15*1024*1024];
        System.out.println(softReference.get());
    }
}

```
报虚拟机OOM错误
![在这里插入图片描述](https://img-blog.csdnimg.cn/60ba81b17f4242948a4c3a2b77092df2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果注释掉Reference.reachabilityFence(softReference);
则会正常输出---
![在这里插入图片描述](https://img-blog.csdnimg.cn/56e52840586e4adbafc8be7483001007.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)所以,该方法可以保持指定引用暂时不被虚拟机gc清除;

public static void reachabilityFence​(Object ref)方法中没有定义任何方法体,而是通过@ForceInline注解的方式来完成的----即强制内联;






简单的的说就是new的一个对象大部分都是强引用;

强引用的特点:
1,使用最普遍的引用;
2,当内存空间不足时，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足的问题。
如果强引用对象不使用时，需要弱化从而使GC能够回收，如下：
**strongReference=null;** 显示的设置为NULL
或让其超出对象的生命周期范围，则gc认为该对象不存在引用，这时就可以回收这个对象。具体什么时候收集这要取决于GC算法。

特点2演示:
代码4---->
```java
public class SoftReferenceDemo {
    public static void main(String[] args)  {
byte[]bytes= new byte[1024*1024*10];
        System.out.println(bytes);
byte[]byte2= new byte[1024*1024*15];
        System.out.println(byte2);
    }
}

```
宁愿抛错误也不愿意回收掉还有引用指向的强引用;
(如果你的JVM没有抛出错误,那说明你的JVM分配的对空间足够大;)

![在这里插入图片描述](https://img-blog.csdnimg.cn/4f3075b12e6647b2a28de645bf847fcf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如何设置堆空间最大最小值---(临时的,并非全局)
**在 VM optionss中设置   -Xms20M -Xmx20M** 



> 引用计数：在Java堆中,每一个对象都会有一个引用计数属性，用来记录引用次数,每引用一次,计数加1，每释放1次引用,则计数减1。

在一个方法的内部有一个强引用，即-局部变量为强引用类型,这个引用(地址)保存在Java栈中，而真正的引用内容(Object)则保存在Java堆中。
当这个方法运行完成后，该强引用就会退出方法栈，引用计数就会回归到0,则引用当引用计数回归到0时,这个对象会被垃圾回收器回收。

但是如果这个强引用是全局变量时，就需要在不用这个对象时赋值为null，因为强引用不会被垃圾回收器回收,见代码3


### 软引用----->>>

当堆内存空间足够时,即使主动调用GC也不会立即回收它,而是通知垃圾回收器,将来给我回收了,具体什么时间回收------>>>当堆内存空间不够的时候才会回收该对象;因此软引用常用来做缓存用;
代码5---->
```java
import java.lang.ref.SoftReference;
import java.util.concurrent.TimeUnit;

public class SoftReferenceDemo {
    public static void main(String[] args) throws InterruptedException {
        //引用了一个10M的字节数组
        SoftReference<byte[]>softReference= new SoftReference<>(new byte[1024*1024*10]);
        //回收前打印一下这个引用
        System.out.println(softReference.get());
        //回收
        System.gc();
        //Thread.sleep(100);
        TimeUnit.SECONDS.sleep(1);
        //回收之后再打印一下这个引用
        System.out.println(softReference.get());
        //之后又来了一个引用,大小为15M
        SoftReference<byte[]>softReference1= new SoftReference<>(new byte[1024*1024*15]);
       //强引用也可以,只要使得堆空间不够用就可以;
        //byte b[]=new byte[1024*1024*15];
        //再打印一下
        System.out.println(softReference.get());//null

    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f5a398cf19f4bdeba0b0dc3b8d05044.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 软引用的API------>>>

类的继承实现关系

public class SoftReference<T> extends Reference<T>

**类描述:**
软引用对象，由垃圾自行清除 收集器响应内存需求。 软引用是最常用的 实现**内存敏感的缓存**。

> 假设垃圾收集器在某个时间点确定对象是软可达的。 此时，它可以选择以原子方式清除对该对象的所有软引用，以及对通过强引用链可访问该对象的任何其他软可访问对象的所有软引用。在同一时间或稍后的时间，它将把那些新清除的、已注册到引用队列中的软引用放入队列。   

> **对软可达对象的所有软引用保证在虚拟机抛出OutOfMemoryError  之前清除。**
> 否则，对软引用被清除的时间或对不同对象的一组这样的引用被清除的顺序没有任何限制。 但是，鼓励虚拟机实现偏向于清除最近创建或最近使用的软引用。

 简单的理解就是-----软引用在虚拟机抛出OOM的之前的时候一定会被清理;如果没有达到虚拟机内存域值,则对域软引用的清除不做任何操做;

**构造方法--->**

![在这里插入图片描述](https://img-blog.csdnimg.cn/933f6538699141b090c1da1b93f42d16.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)两个构造方法的区别在于一个是注册在队列中,一个没有注册在队列中;

代码6------->>>

```java
  String str="HelloWorld";
        ReferenceQueue rq= new ReferenceQueue();
     SoftReference<String>reference=new SoftReference<>(str,rq);
        String s = reference.get();//拿到这个字符串
        System.out.println(s);
        str=null;
        System.gc();//通知回收一下,看看能不能被回收
        System.out.println(reference.get());


        System.out.println("------------------芬贝格县---------------");

        SoftReference<String>reference2=new SoftReference<>(str);
        String str1 = reference.get();//拿到这个字符串
        System.out.println(str1);
        str=null;
        System.gc();//通知回收一下,看看能不能被回收
        System.out.println(reference.get());

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/1fc5bd7f0f174047897c671f480dea4d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以看到当  JVM内存够用时不会主动清除


get方法的源码----->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/d29d953cdf944b90a30e9baaab61d1bd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
软引用在JVM内存不够用时的回收----->>>
代码7---->
```java

public class SoftReferenceDemo {
    public static void main(String[] args) throws InterruptedException {
     ReferenceQueue RQ= new ReferenceQueue();
     SoftReference<byte[]>softReference= new SoftReference<>(new byte[8*1024*1024]);
        System.out.println(softReference.get());
        SoftReference<byte[]>softReference2= new SoftReference<>(new byte[8*1024*1024]);
        System.out.println(softReference2.get());
        System.gc();
        Thread.sleep(10000);
       byte []bytes2= new byte[5*1024*1024];
        System.out.println(softReference.get());
        System.out.println(softReference2.get());
    }
}

```
运行结果---->
有时候是这个结果,
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc3823a32b6f4022bd70e925cfbad5f2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
有时候是另一个结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/3c285ac977d743cf897d8a384a4cd233.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)虽然虚拟机鼓励回收实现偏向于清除最近创建或最近使用的软引用---试验了一下这句话的意思是说相对于最新一个创建的软引用而言最近的一个,但是并不保证其他的一定不会被回收!-------可以这么理解--保留最近的一个;
当把软引用数量提升到三个的时候
![在这里插入图片描述](https://img-blog.csdnimg.cn/31b84d1fcd554b76a921ecceb11102e9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d231477aadb54d26811ebde855882061.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以发现依然会出现都被清除的情况,说明即使是鼓励,但也不见得一定不会被回收;

再来看这个引用队列------>>>
代码8----->>>>>
```java

public class SoftReferenceDemo {
    public static void main(String[] args) throws InterruptedException {
     ReferenceQueue RQ= new ReferenceQueue();
     SoftReference<byte[]>softReference= new SoftReference<>(new byte[5*1024*1024],RQ);
        SoftReference<byte[]>softReference2= new SoftReference<>(new byte[5*1024*1024],RQ);
        SoftReference<byte[]>softReference3= new SoftReference<>(new byte[5*1024*1024],RQ);
        System.out.println(softReference.get());
        System.out.println(softReference2.get());
        System.out.println(softReference3.get());
        /*由于此时还没有gc(),所以为null*/
        Reference poll1 = RQ.poll();
        System.out.println(poll1);//null
        System.gc();
        Thread.sleep(5000);
       byte []bytes2= new byte[8*1024*1024];
        System.out.println(softReference.get());
        System.out.println(softReference2.get());
        System.out.println(softReference3.get());

        Reference poll = RQ.poll();//gc之后软引用会被添加到队列中,如果队列中由可用软引用,则立即返回该引用,并将其从队列中删除
        System.out.println(poll);
    }
}

```

在ReferenceQueue中的三个方法---->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/b9e20ca6b3af4f258f9ca6511d631038.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)public Reference<? extends T> poll()

Polls this queue to see if a reference object is available. If one is available without further delay then it is removed from the queue and returned. Otherwise this method immediately returns .null


在SoftReference​(T referent, ReferenceQueue<? super T> q)构造时,个在gc之后,软引用的对象会被添加到该队列q中;

**如果在gc()调用之前,poll一定返回null;**

![在这里插入图片描述](https://img-blog.csdnimg.cn/e2b6ff6f7ed845d6b62c03001c44d86a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
代码9------>
```java
public class SoftReferenceDemo {
    public static void main(String[] args) throws InterruptedException {
     ReferenceQueue RQ= new ReferenceQueue();
     SoftReference<byte[]>softReference= new SoftReference<>(new byte[5*1024*1024],RQ);
        SoftReference<byte[]>softReference2= new SoftReference<>(new byte[5*1024*1024],RQ);
        SoftReference<byte[]>softReference3= new SoftReference<>(new byte[5*1024*1024],RQ);
        System.out.println(softReference.get());
        System.out.println(softReference2.get());
        System.out.println(softReference3.get());
      /*  *//*由于此时还没有gc(),所以为null*//*
        Reference poll1 = RQ.poll();
        System.out.println(poll1);//null*/
       /* Reference remove = RQ.remove();//阻塞方法,直至超时
        System.out.println(remove);*/
        System.gc();
        Thread.sleep(5000);
       byte []bytes2= new byte[8*1024*1024];
        System.out.println("--------------");
        System.out.println(softReference.get());
        System.out.println(softReference2.get());
        System.out.println(softReference3.get());

      /*  Reference poll = RQ.poll();//gc之后软引用会被添加到队列中,如果队列中由可用软引用,则立即返回该引用,并将其从队列中删除
        System.out.println(poll);*/
        Reference remove1 = RQ.remove();
        System.out.println(remove1);
    }
}

```
remove方法源码---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/7ae6d3d5e77941edad00e634761eba3f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
### 弱引用----->>>
代码10---->

```java
import java.lang.ref.WeakReference;
class A {
    @Override
    protected void finalize() {
        System.out.println("finalize--invoke");
    }
}
public class WeakReferenceDemo {
    public static void main(String[] args) throws InterruptedException {

        WeakReference<A> weak = new WeakReference<>(new A());
        System.out.println(weak.get());
        Thread.sleep(200);
        System.gc();
        System.out.println(weak.get());
    }
}
```

结果------->
![在这里插入图片描述](https://img-blog.csdnimg.cn/71b4f83012384d33bfa37aa173ec5a50.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)只要gc看到了弱引用,直接就会回收弱引用;

继承实现关系----
public class WeakReference<T> extends Reference<T>

构造方法跟软引用类似;

根据弱引用的特点-----只要gc看到了弱引用,直接就会回收弱引用;

#### 弱引用的API

WeakReference​(T referent) 	
创建一个引用给定对象的新弱引用。
WeakReference​(T referent, ReferenceQueue<? super T> q) 	
创建一个引用给定对象的新弱引用，并且是 在给定的队列中注册。 

#### 弱引用在在ThreadLocal类中的应用

```java
public class WeakReferenceDemo {
    public static void main(String[] args) throws InterruptedException, IOException {
        ThreadLocal<A>tl= new ThreadLocal<>();
        tl.set(new A());
        tl.remove(); 
    }
}

```

set方法出打断点----->>
![](C:\Users\Gavin\Pictures\Camera Roll\51.png)

内存泄露-------意思是指内存中有一个永远不会被回收的对象,但是这个对象没有被其他地方使用,垃圾回收器看它是强引用类型也
不会主动去回收它,于是就会产生内存泄漏;

通过ThreadLocal类中的set(),我们来分析一下内存泄露的可能性!!

1,假使ThreadLocal中的静态内部类entry不是一个弱引用,而是一个强引用类型;
当ThreadLocal对象  指向null的时候,按理说整个tl对象应该被GC回收,但是由于ThreadLocal中的Entry[]指向一个强引用对象,那么这个ThreadLocal永远也不会被回收,虽然用不着,但是也不能被回收,所以就会产生内存泄漏;
2,同理如果是软引用类型,也可能产生内存泄露(在内存足够的情况下)
3,只有弱引用才是合适的;


内存溢出---是指内存不够用了,才会造成内存溢出即 OOM

注意：如果一个对象是偶尔(很少)的使用，并且希望在使用时随时就能获取到，但又不想影响此对象的垃圾收集，那么你应该用Weak Reference来记住此对象。


#### 弱引用的生命周期---->>

WeakReference对象的生命周期基本由垃圾回收器决定，一旦垃圾回收线程发现了弱引用对象，在下一次GC过程中就会对其进行回收。

### 虚引用

```java

public class PhrReferenceDemo {
    public static void main(String[] args) {
        ReferenceQueue rq = new ReferenceQueue();
        PhantomReference<byte[]> phantomReference = new PhantomReference<>(new byte[10 * 1024 * 1024], rq);
        System.out.println(phantomReference.get());//得到值为空--虚引用直接get是get不到的.
        //Reference.reachabilityFence(rq);
        phantomReference.enqueue();//移除引用并加入到队列中

        if (rq.poll() != null) {
            //说明有虚引用进入了队列,
            System.out.println("可以被回收");
            System.gc();
        }
        byte[] bytes = new byte[8 * 1024 * 1024];

        // phantomReference.enqueue();//移除并加入到队列中,并立即返回该移除对象
        //  System.out.println(phantomReference.isEnqueued());

        System.out.println(phantomReference.get());//得到值为空--虚引用直接get是get不到的.

    }
}
```

#### 虚引用API
继承实现关系---->

public class PhantomReference<T> extends Reference<T>
Phantom 引用对象，**在收集器之后排队 确定他们的参照物可能会被回收**。 幻影 引用最常用于安排事后清理操作。 


构造方法----->>可以看到只有一种构造,必须要加一个引用队列
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6c139fed808416eb389f2f9fe07e7b2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
虚引用-----当你get值得时候get不到,那虚引用还有必要存在吗?

虚引用主要用来跟踪对象被垃圾回收器回收的活动,一般是编写JVM虚拟机得时候用的,虚引用可以指向任何对象,

在清理堆外内存的时候可能会用到,那如何清理堆外内存呢?
可以通过一些方法使得虚引用进入队列,然后通过判断队列是否有虚引用来对一些情况进行处理;

小结------>>>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/094d55a384a64bb594f2ab33ce9def78.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)





# JAVA中原子类

## 原子基本类

简单来说就是跟原子操作一样,能够保证线程的安全

今天借机会整理一下java中的原子类,看看他们是怎么用的;

先找一下java中的原子类总体图------
![在这里插入图片描述](https://img-blog.csdnimg.cn/f43a3f893c9045178aa1419301c92eb1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

###  

```java

public class Yuanzi{
    public static void main(String[] args) {
        AtomicInteger ai= new AtomicInteger();//无参-----默认为0
        AtomicInteger ati= new AtomicInteger(100);
        AtomicBoolean ab= new AtomicBoolean();//无参----默认为false
        AtomicBoolean abi= new AtomicBoolean(true);
        AtomicLong al= new AtomicLong();//无参-----默认为0
        AtomicLong ali= new AtomicLong(100);
           }
}
```

###  继承和实现关系
```go
public class AtomicInteger extends Number implements java.io.Serializable 
public class AtomicBoolean implements java.io.Serializable 
public class AtomicLong extends Number implements java.io.Serializable 
```
####  构造方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f56319f601e4f98abfc7b08eb934b85.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

#### 常用方法

```java
package victory;

import java.util.concurrent.atomic.AtomicBoolean;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicLong;
import java.util.function.IntBinaryOperator;

public class Yuanzi{
    public static void main(String[] args) {
        IntBinaryOperator ibo = new IntBinaryOperator() {
            @Override
            public int applyAsInt(int left, int right) {
                int x=left-right;
                return x;
            }
        };
        AtomicInteger ai= new AtomicInteger();//无参-----默认为0
        AtomicBoolean ab= new AtomicBoolean();//无参----默认为false
        AtomicLong ali= new AtomicLong(100);

        // AtomicInteger 常用方法---都是原子方法---线程安全
        //减法操作--/* 先取值(为0)减1之后为-1,然后减1在取值(为-2)*/
        System.out.println("先取值在减一"+ai.getAndDecrement());//先取值在减一  结果为0   即  ai-- 操作
        System.out.println("获得当前值"+ai.get());//获得当前值 -1
        System.out.println("先减一在取值"+ai.decrementAndGet());//先减一在取值  结果为-2 即  --ai 操作
       //加法操作
        System.out.println("先取值在加一"+ ai.getAndIncrement());//先取值在加一  结果为 -2 即  ai++ 操作
        System.out.println("获得当前值"+ai.get());//获得当前值-1
        System.out.println("先加一在取值"+ai.incrementAndGet());//先加一在取值 结果为 0 即 ++ai 操作

        System.out.println("将指定参数原子的加到ai的操作"+ai.addAndGet(-10));//将指定参数加到ai的操作   结果为-10
/*使用将给定函数应用于当前值和给定值的结果原子更新当前值，返回更新后的值。*/
        System.out.println(ai.accumulateAndGet(10, ibo));
        System.out.println("获得当前值"+ai.getAcquire());
        System.out.println(ai.getAndAccumulate(10, ibo));
        System.out.println("获得当前值"+ai.get());
        /*期望值---当前值(也叫见证值/目击值),返回见证值,
        /如果相等的话表示成功具体有什么用处以后再聊--当前只需要记住返回见证值即可*/
        System.out.println("---------------分割线---------------------");
        System.out.println(ai.compareAndSet(-10, ai.get()));
        System.out.println(ai.compareAndExchange(-10, ai.get()));
        System.out.println(ai.compareAndExchangeAcquire(-10, ai.get()));
        System.out.println(ai.compareAndExchangeRelease(-10, ai.get()));
        //以上三种方法底层都是compareAndExchangeInt(this, VALUE, expectedValue, newValue);来实现的,返回值为int类型
        //取得当前值
        System.out.println(ai.getAndSet(10));////取得当前值并设置新值
        System.out.println(ai.get());//10
         System.out.println(ai.getPlain());//返回当前值10
        System.out.println(ai.getAcquire());//10
        //类型转换
        System.out.println(ai.doubleValue());
        System.out.println(ai.floatValue());
//比较期望值与目击值(真实值)是否相等
        System.out.println(ai.weakCompareAndSet(20, ai.get()));//此方法已被弃用,被weakCompareAndSetIntPlain所替代
        System.out.println(ai.weakCompareAndSetAcquire(20, ai.get()));
        System.out.println(ai.weakCompareAndSetPlain(20, ai.get()));
        System.out.println(ai.weakCompareAndSetRelease(20, ai.get()));
        System.out.println(ai.weakCompareAndSetVolatile(20, ai.get()));
        //以上底层都用了compareAndSetInt,返回值为布尔类型


    }
}

```

```java

public class Yuanzi{
    public static void main(String[] args) {
        IntUnaryOperator updateFunction = new IntUnaryOperator() {
            @Override
            public int applyAsInt(int operand) {
                return 999;
            }
        };
        AtomicInteger ai= new AtomicInteger();
        System.out.println(ai.getAndSet(100));//设置新值,返回当前值0
        System.out.println(ai.getAndUpdate(updateFunction));//返回当前值的前一个ai的值100,设置新值(可自己设定这里设定为999)
        System.out.println(ai.get());//返回当前值999
        System.out.println(ai.getPlain());//返回当前值999
        ai.lazySet(100);//设置新值100,无返回值
        System.out.println(ai.getPlain());
        ai.set(111);//设置新值111,无返回值
        ai.setOpaque(112);
        System.out.println(ai.getOpaque());

    }
}
```

>  AtomicLong和AtomicBoolean的方法跟 AtomicInteger类似,不在赘述--- 
>  AtomicBoolean方法比其他两个方法数量上要少----因为没有加减操作;
>  ###  原子基本类线程安全

```java

public class Test01 {
 AtomicInteger at= new AtomicInteger(10);
    public void fun() {
        for (int i = 1; i < 100; i++)
            if (at.get()>0) {
                System.out.println(Thread.currentThread().getName() + "---" + at.getAndDecrement());
            }
                }
    public static void main(String[] args) {
Test01 test01= new Test01();
new Thread(test01::fun,"线程1").start();//启动一个线程1
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        new Thread(test01::fun,"线程2").start();
    }
}

```

> 并不需要声明同步就实现了线程安全,使用AtomicInteger是非常的安全的.而且因为AtomicInteger由硬件提供原子操作指令实现的。在非激烈竞争的情况下，开销更小，速度更快。
> 可以避免多线程的非线程安全和死锁情况的发生，提高在高并发处理下的性能。

说一下compareAndSet(-10, ai.get())方法,前一个时我期望的值,后一个是要修改成的值,只有当期望值和要修改的值相等时我才做出修改,否则进行下一次尝试

```java

public class Yuanzi{
    public static void main(String[] args) {
     AtomicInteger ai= new AtomicInteger(10);
        //当期望值跟当前值一样时我才修改为新值,否则就是失败或者可重新尝试修改
     System.out.println(ai.compareAndSet(10, 12));
        //期望值与当前值一样,所以被修改为新值12
        System.out.println(ai.getPlain());
        //期望值与当前值不一样,所以没有修改为新值999
     System.out.println(ai.compareAndSet(18, 999));
      System.out.println(ai.getPlain());

    }
}

```

###  为什么要有原子类
   当多线程访问同一共享变量时，我们需要加锁来保证线程的安全性，但是而加锁会损耗一部分运行性能和时间，加锁的部分也就常用的那么几个类,所以从jdk1.5之后，API中新增的原子操作类，这些类位于java.util.包下的concurrent.atomic包下，随着jdk版本的推演,到jdk1.8，扩展到现在的16个原子操作类，大体分为原子基本类、原子数组类、原子属性类、原子引用类。






### 锁与原子类

> 主要使用锁与原子类进行操作,
> Class Long
> Class LongAdder
> Class AtomicLong


```java
package victory;

import java.util.concurrent.atomic.AtomicLong;
import java.util.concurrent.atomic.LongAdder;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @author : Gavin
 * @date: 2021/8/15 - 08 - 15 - 10:57
 * @Description: victory
 * @version: 1.0
 */

//高并发情况下不同的线程安全问题的时间花费对比
public class LockTest {

    private static long m1 = 0l;
    static AtomicLong m2 = new AtomicLong(0);
    static LongAdder m3 = new LongAdder();//
    static Lock lock = new ReentrantLock();

    public static void main(String[] args) {
        Thread[] threads = new Thread[10000];//先准备10000个线程,
        for (int i = 0; i < threads.length; i++) {//然后开启
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 10000; j++) {
                    try {
                        lock.lock();
                        m1++;
                    } catch (Exception e) {
                        e.printStackTrace();
                    } finally {
                        lock.unlock();
                    }

                }
            }
            );

        }
        long start = System.currentTimeMillis();
        for (Thread th : threads
        ) {
            th.start();
        }

        for (Thread th : threads
        ) {
            try {
                th.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        long end = System.currentTimeMillis();
        System.out.println(m1 + "时间" + (end - start));
        System.out.println("------------------分割线------------------------");
        for (int i = 0; i < threads.length; i++) {//然后开启
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 10000; j++) {
                    m2.incrementAndGet();//++操作
                }
            }
            );

        }
        start = System.currentTimeMillis();
        for (Thread th : threads
        ) {
            th.start();
        }

        for (Thread th : threads
        ) {
            try {
                th.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        end = System.currentTimeMillis();
        System.out.println(m2 + "时间" + (end - start));
        System.out.println("------------------分割线------------------------");
        for (int i = 0; i < threads.length; i++) {
            threads[i] = new Thread(new Runnable() {
                @Override
                public void run() {
                    for (int j = 0; j < 10000; j++) {
                        m3.increment();
                    }
                }
            });


        }

        start = System.currentTimeMillis();
        for (Thread th : threads
        ) {
            th.start();
        }

        for (Thread th : threads
        ) {
            try {
                th.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        end = System.currentTimeMillis();
        System.out.println(m3 + "时间" + (end - start));
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/76ae88173fe64656ac611f8854b14172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

为什么当高并发时时间差距从哪里来呢?
1,差距最大的是带锁的情况因为有锁,当其他线程多的时候,相关线程处于锁等待状态的时间就会多;

剩下两个时间差距不大,论具体差距是差在哪呢?
![](C:\Users\Gavin\Pictures\Camera Roll\14.png)

![](C:\Users\Gavin\Pictures\Camera Roll\15.png)



**降低锁的竞争**

 1,减少锁的持有时间-----即锁细化
 2,减少锁的请求频率-----即锁粗化
 3,使用带有协调机制的独占锁,----比如分段锁



# 线程的执行时间

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.concurrent.atomic.AtomicLong;
import java.util.concurrent.atomic.LongAdder;

public class T {
    static long count=0L;
    final static Object lock = new Object();
   static  AtomicLong aLong= new AtomicLong(0);
   static LongAdder longAdder= new LongAdder();
    public static void main(String[] args) {
    List<Thread>list= new ArrayList<>();
        for (int i = 0; i < 1000; i++) {//1000个线程
            list.add(new Thread(()->{
                for (int j = 0; j < 100000; j++) {
                    synchronized (lock) {
                        count++;
                    }
                }
            }));
        }
        long LongStart = System.currentTimeMillis();
        list.forEach((o)->{o.start();});
        list.forEach((o)-> {
            try {
                o.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        long LongEND = System.currentTimeMillis();
        Thread []threads= new Thread[1000];
        for (int i = 0; i < 1000; i++) {
            threads[i]=new Thread(new Runnable() {
                @Override
                public void run() {
                    for (int j = 0; j < 100000; j++) {
                        aLong.incrementAndGet();//i++
                    }
                }
            });
        }
        long AutoStart = System.currentTimeMillis();
        for (Thread t :
                threads) {
            t.start();
        }
        for (Thread t :
                threads) {
            try {
                t.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        long AutoEnd = System.currentTimeMillis();
        long addStart = System.currentTimeMillis();

        LinkedList<Thread>threadLinkedList= new LinkedList<>();
        for (int i = 0; i < 1000; i++) {
            threadLinkedList.add(new Thread(()->{
                for (int j = 0; j < 100000; j++) {
                    longAdder.increment();
                }

                }));
        }
        threadLinkedList.forEach((o)->{o.start();});
        threadLinkedList.forEach((o)->{
            try {
                o.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        long addEnd = System.currentTimeMillis();


        System.out.println("最后结果--"+count+"longCount用时"+(LongEND-LongStart));
        System.out.println("最后结果--"+aLong.get()+"longAuto用时"+(AutoEnd-AutoStart));
        System.out.println("最后结果--"+longAdder.longValue()+"longAdder用时"+(addEnd-addStart));


}

}

```
加入主线程中然后.....,又学一招


![在这里插入图片描述](https://img-blog.csdnimg.cn/3e3e9581780c451fad3a72d1d60f2c18.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,sizsye_20,color_FFFFFF,t_70,g_se,x_16)首先分析一下第一种，syc，应用了锁，所以当线程越累越多的时候，更多的线程在等待，又偏向锁到自旋锁再到重量级锁，会有一个锁升级的状态，用时会比较多

结局方法是加volatile,
结果看图----
![在这里插入图片描述](https://img-blog.csdnimg.cn/08cbf910d13d47ff93d15c6cda9c7030.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 第二种CAS操作，相当于无锁，但为什么运行时间却比第一种多呢？ 问题在于那个static long count 上，因为虽然是静态的，但是当一个线程修改完数据之后对其他线程是不可见的，等该线程将count写入到内存之后下一个线程才开始，这会耗费一定时间（当线程过多的时候，耗费的时间累计页不容小觑）
>

> 第三种为什么这么快？因为
> LongAdder的内部做了一个分段锁，在它内部做运算的时候会将值放到一个数组里，比如说数组长度为4，最开始是0，1000个线程，250个线程被锁在第一个数组元素里，每一个都往上递增，算出来的结果加到一起
> 类似于这么一个效果
>
> 

>
> 250个线程同时加1，在将结果加到一起，累加了250而不是一个线程自己累加250次；当然按照现在的处理器4核8线程，一次最多加8，但是也比只有一个线程拿到锁，一次加1累加8次快；
>
> 



> 当线程数量级小或者循环的次数减少，三者的差距不会太大，如果都很多的话，LongAdder的效率是三者中高的；

## 线程数越多越好吗?
假如有一个int 数组,里面有100000000个元素,要求数组中元素的和
有两种方式----
第一种是单线程去处理
第二种是多线程去处理


```java
package com.thread.gavin.ThreadDemo;

import java.util.Random;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.atomic.AtomicInteger;

/**
 *
 */
public class addTest {
    static long start = 0;
    static long end = 0;
    static Random random = new Random();
    static int[] array = new int[100000000];

    static {
        //首先将这数组填满
        for (int i = 0; i < array.length; i++) {
            array[i] = random.nextInt(15);//取21以内的数以免超int范围
        }
    }

    //采用单线程去处理
    static void methodA() {
        int result = 0;
        start = System.currentTimeMillis();
        for (int i = 0; i < array.length; i++) {
            result += array[i];
        }
        end = System.currentTimeMillis();
        System.out.println("耗时" + (end - start) + "毫秒," + "结果--" + result);

    }

    static int result1 = 0;
    static int result2 = 0;

    //多线程
    static void methodB() throws InterruptedException {
        Thread t = new Thread(() -> {
            start = System.currentTimeMillis();
            for (int i = 0; i < array.length / 2; i++) {
                result1 += array[i];
            }
        });

        Thread tt = new Thread(() -> {

            start = System.currentTimeMillis();
            for (int i = array.length / 2; i < array.length; i++) {
                result2 += array[i];
            }
        });
        start = System.currentTimeMillis();
        t.start();
        t.join();
        tt.start();
        tt.join();
        end = System.currentTimeMillis();
        System.out.println("耗时" + (end - start) + "毫秒," + "结果--" + (result1 + result2));
    }
    static int resultsum=0;
    //超多线程去处理岂不更快?
    static void methodC() {
        //假设有一万个线程去处理,这样得把原数组分成10000份
        //定义线程数
        int ThreadCounts = 8000;
        Thread[] threads = new Thread[ThreadCounts];
        int[] result = new int[ThreadCounts];//用于存放和的
        int foot = array.length / ThreadCounts;
        CountDownLatch cdl = new CountDownLatch(threads.length);
        //开10000个线程
        for (int i = 0; i < threads.length; i++) {
            int m = i;
            threads[i]= new Thread(() -> {
                for (int j = m * foot; j < (m + 1) * foot&&j<array.length; j++) {
                    result[m] += array[j];
                }
                cdl.countDown();
            });
            //

        }


        for (Thread t :
                threads) {
            t.start();
        }

        start = System.currentTimeMillis();
        try {
            cdl.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        for (int j = 0; j < threads.length; j++) {
            resultsum+=result[j];
        }
        end = System.currentTimeMillis();
        System.out.println("耗时" + (end - start) + "毫秒," + "结果--" + resultsum);
    }

    public static void main(String[] args) throws InterruptedException {
        methodA();
        methodB();
        methodC();
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/05c219c741394fe7a1ead58bb28b8f36.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



# 开辟线程的方式(面试题)

```java
package com.thread.gavin.ThreadDemo;
import java.util.concurrent.*;

class T extends Thread {
    @Override
    public void run() {
        System.out.println("T----start");
    }
}
class P implements Runnable {
    @Override
    public void run() {
        System.out.println("P----start");
    }
}
class F implements  Callable<String>{
    @Override
    public String call() throws Exception {
        String success="Callable----start";
        System.out.println(success);
        return success;
    }
}

public class ThreadStart {
    public static void main(String[] args) {
        //第一种-----继承Thread
        new T().start();
//        第二种实现Runnable接口,本质上是借助Thread的start方法
        new Thread(new P()).start();
//        第三种 lambda
        new Thread(() -> {
            System.out.println("匿名----start");
        }).start();
//第四种 实现Callable接口, 实现call方法,可以指定返回值的   线程开启本质上是借助Thread的start方法
new Thread(new FutureTask<String>(new F())).start();
//第五种 线程池
        ExecutorService service= Executors.newCachedThreadPool();
       Thread thread= new Thread(()->{
               System.out.println("线程池中的线程开启了");
       });
   service.execute(thread);//假如线程池
        service.submit(thread);
   Future<String> f=service.submit(new F());
   service.shutdown();
    }
}

```

## 线程池

**类的继承实现关系--->**

ExecutorService---->>>>>这是一个接口,  
用于执行线程池中的线程
**public interface ExecutorService extends Executor**

接口描述-->>

> 提供管理终止的方法和方法的执行者，未来可生成跟踪一个或多个异步任务进度的。

**接口中的方法-->**

> <T> List<Future<T>>	invokeAll​(Collection<? extends Callable<T>>
> tasks)	
> 执行给定任务，在全部完成后返回保留其状态和结果的期货列表。


**简而言之就是实现Callable接口的类作为一个集合, 执行集合里所有的线程,由于实现Callable接口要实现Call()方法,并且带有返回值,所以会有一个List集合来接收所有返回值;**

> <T> List<Future<T>>	invokeAll​(Collection<? extends Callable<T>>
> tasks, long timeout, TimeUnit unit)	
>
> 执行给定任务，在全部完成或超时到期时返回保留其状态和结果的期货列表，以先发生者为准。

**简而言之就是任务完成/超时时做出的一些处理
第一个参数是实现Callable接口的类作为一个集合,第二个是设施超时的等待时间,第三个是设置超时的计时单位;返回值也是一个List集合'**


话不多说,既然有了方法,那就去实现一下子呗;
 分析一下,Callable是一个接口,而invokeAll需要传入的是一个Collection集合,集合里面的元素是继承Callable接口的的,说明是Callable的子类或者孙子类

**@FunctionalInterface
public interface Callable<V>**

> 但是阅读API发现,callable接口是一个功能性接口,并没有直接实现的子类 那怎么办?只能手动写类取实现Callable接口

然后开始写一些关于invokeAll的
<T> List<Future<T>>	invokeAll​(Collection<? extends Callable<T>>
 tasks)	

```java
package TreeB;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.concurrent.*;

class Person implements Callable<String> {
    @Override
    public String call() throws Exception {
        Random random = new Random();
        int i = random.nextInt(10);
        TimeUnit.SECONDS.sleep(i);
        return "我在学习----" + i;
    }
}
public class ThreadPoolDemo {
    public static void main(String[] args)  {
        //开辟一个线程池
        ExecutorService service = Executors.newCachedThreadPool();
//        准备线程----带返回值的
        List<Person> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            list.add(new Person());
        }
        List<Future<String>> futures = null;
        try {
            futures = service.invokeAll(list);
            //futures = service.invokeAll(list, 8, TimeUnit.SECONDS);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("参与工作的CPU核心数--"+Runtime.getRuntime().availableProcessors());
        //遍历
        for (Future<String> fu :
                futures) {
            String str = null;
            try {
                str = fu.get();
                System.out.println(str);
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (ExecutionException e) {
                e.printStackTrace();
            }
        }
      service.shutdown();
    }
}
```

>  <T> List<Future<T>>	invokeAll​(Collection<? extends Callable<T>>
>  tasks, long timeout, TimeUnit unit)	

超时时间是针对的所有线程而言，而不是单个线程的超时时间。如果超时，会取消没有执行完的所有任务，并抛出超时异常。
如果任务处理超时了,会发生什么呢?
简单修改一下------

```java
package TreeB;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.concurrent.*;

class Person implements Callable<String> {
    @Override
    public String call() throws Exception {
        Random random = new Random();
        int i = random.nextInt(10);
        TimeUnit.SECONDS.sleep(i);
        return "我在学习----" + i;
    }
}

public class ThreadPoolDemo {
    public static void main(String[] args) throws InterruptedException, ExecutionException {
        //开辟一个线程池
        ExecutorService service = Executors.newCachedThreadPool();
//        准备一个线程就够了吧----带返回值的
        List<Person> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            list.add(new Person());
        }
        List<Future<String>> futures = service.invokeAll(list, 3, TimeUnit.SECONDS);
        //遍历---为了看得到结果
        for (Future<String> fu :
                futures) {
            String str = fu.get();
            System.out.println(str);
        }
        service.shutdown();
    }
}
```
为了在控制台上看到结果,使用了get来获取程序执行的返回值,(是在get这里报的异常)可以看到本次只有三个线程执行了,其他线程被取消了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/4e7d884622134aa6839edb69c46656cd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> <T> T invokeAny​(Collection<? extends Callable<T>> tasks) throws
> InterruptedException, ExecutionException
>
> 执行给定任务，返回已成功完成的任务的结果（即，无一例外），如果有的话。正常或特殊返回时，未完成的任务将被取消。如果在此操作进行期间修改了给定的集合，则此方法的结果未定义

简言之就是返回已经执行成功的线程的返回值的一个;返回第一个执行完的值
![在这里插入图片描述](https://img-blog.csdnimg.cn/acb19479325b46779f2367bf9118a05c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> <T> T invokeAny​(Collection<? extends Callable<T>> tasks, long
> timeout, TimeUnit unit) throws InterruptedException,
> ExecutionException, TimeoutException
> 执行给定任务，返回已成功完成的任务的结果（即无一例外），如果在给定超时之前执行任何任务。正常或特殊返回时，未完成的任务将被取消。如果在此操作进行期间修改了给定的集合，则此方法的结果未定义。


 invokeAny超时是怎么处理的呢?
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/5f7cb9f391024d5d9de3b827802fbddb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果没有任务完成则会抛出-----

>  java.util.concurrent.ExecutionException:******************

**应用场景------需要最短的时间内返回(线程执行最快的)结果,**


> boolean	isShutdown()	
>  如果此执行器已关闭，则返回。true 
>                                                                                                                 ---------------------------------------------------------------------
> boolean	isTerminated()	
> 如果所有任务在关闭后完成，则返回。true

isShutdown()	----用于检测线程执行器是否关闭

![在这里插入图片描述](https://img-blog.csdnimg.cn/cb68193049274168adba27c3d85303ac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
isTerminated()	用于检测线程是否都已经执行完毕
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c1960a0505740aa924a0dd00ecb64d1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
关闭启动器,无返回值

> void	shutdown()	
> 启动有序关闭，执行以前提交的任务，但不会接受任何新任务。

 **1、停止接收新的submit的任务；**
 **2、已经提交的任务（包括正在跑的和队列中等待的）,会继续执行完成；** 
 **3、等到所有任务执行完成后，才真正停止；**

强制关闭启动器,有返回值
>
> List<Runnable>	shutdownNow()	 试图阻止所有积极执行的任务，停止处理等待任务，并返回等待执行的任务列表。

**1、跟 shutdown() 一样，先停止接收新submit的任务；**

**2、忽略队列里等待的任务；**

**3、尝试将正在执行的任务interrupt中断；**

**4、返回未执行的任务列表；**

修改后代码如下---
```java

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.Random;
import java.util.concurrent.*;

class Person implements Callable<String> {
    @Override
    public String call() throws Exception {
        Random random = new Random();
        int i = random.nextInt(10);
    TimeUnit.SECONDS.sleep(1);
        //throw new RuntimeException();
        return "我在学习----" + i;
    }
}

public class ThreadPoolDemo {
    public static void main(String[] args) {
        //开辟一个线程池
        ExecutorService service = Executors.newCachedThreadPool();
//        准备一个线程就够了吧----带返回值的
        List<Person> list = new ArrayList<>();
        for (int i = 0; i < 100000; i++) {
            list.add(new Person());
        }
        try {
            List<Future<String>> futures1 = service.invokeAll(list);
            //执行完在返回结果
            for (Future<String> ff :
                    futures1) {
                String s = ff.get();
//                System.out.println(s);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println("---------------------");
        List<Runnable> runnables =null;
        if(!service.isTerminated()){
            runnables= service.shutdownNow();
        }
        Iterator<Runnable> iterator = runnables.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next().toString());
        }
    }
}

```
结果是所有线程都执行完毕,但是shutdownNow的返回值列表并不为null,这是什么原因呢?
猜想------->可能是线程最后都执行完毕了,因为代码中线程并没有对Interrupt()做出响应

任务的提交----->>

> Future<?>	submit​(Runnable task)	
>  提交可执行的任务，未来并返回代表该任务的返回值。
>

> <T> Future<T>	submit​(Runnable task, T result)	
> 提交可执行的任务，未来并返回代表该任务的返回值。
>

> <T> Future<T>	submit​(Callable<T> task)	
> 提交价值回报任务以执行，未来并返回代表任务待决结果的返回值。

**参数可以传Runnable和Callable**


shutdown()与shutdownNow()的异同点---------->>>

  1. shutdown与shutdownNow 都是立即返回并且拒绝接受新任务;
  2. shutdown等待任务全部结束;shutdownNow是试图结束所有任务---(包括正在运行的与没有运行的)但不保证能结束;

## FutureTask
一个实现Runnable且带有返回值的类---FutureTask

```java

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;
class TTT implements Callable<String> {
    @Override
    public String call() throws Exception {
       /* for (int i = 0; i < 10; i++) {
            System.out.println(i);
        }*/
        return "success";
    }
}
public class Tead {
    public static void main(String[] args) {
       FutureTask<String>futureTask= new FutureTask<String>(new TTT());
       new Thread(futureTask).start();
        try {
            String s = futureTask.get();
            System.out.println(s);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```

> 在这个案例中,用Thread 实例中传入了FutureTask,,一次来得到返回值,这种方式其实也是绕了一圈,传入的FutureTask中其实是有实现Callable接口的的,但不管怎样,我们来看类的信息;



> public class FutureTask<V> extends Object implements RunnableFuture<V>
>

> **FutureTask实现了Future接口，FutureTask提供了启动和取消异步任务，查询异步任务是否计算结束以及获取最终的异步任务的结果的一些常用的方法。通过get()方法来获取异步任务的结果，但是会阻塞当前线程直至异步任务执行结束。一旦任务执行结束，任务不能重新启动或取消，除非调用runAndReset()方法**





> public interface RunnableFuture<V> extends Runnable, Future<V>
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/7ad1dfff42db4b1e8d202479864e5f2b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**构造 函数**

FutureTask​(Runnable runnable, V result)	
创建一个在运行时执行给定结果，并在成功完成时返回给定结果的安排。

V  result表示运行结束后的返回值
```java

public class Tead {
    public static void main(String[] args) {
      FutureTask f= new FutureTask(new Runnable() {
          @Override
          public void run() {
              for (int i = 0; i < 5; i++) {
                  System.out.println(i);
              }
          }
      },"完成了");//运行结束后会返回"完成了"
      new Thread(f).start();
        try {
            Object o = f.get();
            System.out.println(o);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}

```

> FutureTask​(Callable<V> callable)	 
> 创建一个FutureTaskCallable,返回值将在运行时给定。

```java
FutureTask ff= new FutureTask(new Callable() {
            @Override
            public Object call() throws Exception {
                return "success";
            }
        });
        new Thread(ff).start();
        try {
            Object of = ff.get();
            System.out.println(of);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

```

**FutureTask类中的方法**

```java
V	get()	
//如有必要，等待计算完成，然后检索其结果。
V	get​(long timeout, TimeUnit unit)	
///如果需要，最多等待给定时间才能完成计算，然后在可用时检索其结果。
```

> protected boolean	runAndReset()	
> 执行计算而不设置其结果，然后将此Future重置为初始状态，如果计算遇到异常或被取消，则未执行该状态
> 这是个什么鬼?

![](C:\Users\Gavin\Pictures\Camera Roll\36.png)

> protected void	set​(V v)	
> 将此Future的结果设置为给定值，除非此Future已经设置或已取消。

![](C:\Users\Gavin\Pictures\Camera Roll\37.png)

> protected void	done()	
> 当此任务过渡到状态（正常或通过取消）时调用受保护的方法。isDone


> protected void	setException​(Throwable t)	
> 导致此未来报告执行例外与给定可投掷的原因，除非此未来已经设置或已取消。
>

> String	toString()	 返回此未来任务的字符串表示。
>
> 

**小结----线程的启动方式**

覆写/实现run()方法
1,继承Thread类-----
new MyThread().start();
2,new Thread(r).start; 其中r可以是lambda,可以实现runnable接口也可以是继承Thread的类
3,ThreadPool 线程池的方式
4,Future Callable以及FutureTask

但是归根结底就是  
描述一------->>>>>继承Thread/实现Runnable/Callable
描述二----------->>>>>覆写run()/call()方法

> **青春过了一大半,原来只是陪时间玩耍!!!**

# 细说线程池

## Executors
继承实现关系------->>>>>>
**public class Executors extends Object**

描述----<<<<<
>  java.util.concurrent.包中定义的**执行程序**、**执行者服务**、**计划执行者服务**、**线程工厂**和**可调用类的工厂和实用方法**。此类支持以下方法：
>  创建和返回使用通常有用的配置设置的执行者服务的方法。
>  使用通常有用的配置设置创建和返回计划执行服务的方法。
>  创建并返回"包装"执行者服务的方法，通过使无法访问特定于实施的方法来禁用重新配置。
>  创建并返回将新创建的线程设置为已知状态的线程工厂的方法。
>  从其他类似关闭的表单创建和返回可调用的方法，因此可用于需要执行方法。

**Executors类中的方法**

查看API发现没有显式的定义构造方法,其中该类中的方法都为静态方法,这一点跟LockSupport类似;

### 静态方法------>返回值为Callable的

#### static Callable<Object> callable(Runnable task)

> **static Callable<Object>	callable​(Runnable task)**	
> 返回可调用对象，当调用时，该对象运行给定任务并返回 null
> **返回值为Callable<Object>,但是返回值为null;**

```java
import java.util.concurrent.*;

public class FutureTaskDemo {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        Callable<Object> th = Executors.callable(new Runnable() {
            @Override
            public void run() {
                System.out.println("我不带结果");
            }
        });
        FutureTask ft = new FutureTask(th);
        Thread thread = new Thread(ft);
        thread.start();

        if (null == ft.get()) {
            System.out.println("真的不带结果");

        }
    }
}
```

分析------->>>

![](C:\Users\Gavin\Pictures\Camera Roll\38.png)

#### static <T> Callable<T> callable(Runnable task, T result)

> static <T> Callable<T>	callable​(Runnable task, T result)	
> 返回可调用对象（当调用时）运行给定任务并返回给定结果。当应用需要对其他无结果的行动进行方法时，这很有用。
> 跟上面类似,只不过带了返回值

```java
略
```

> static Callable<Object>	callable​(PrivilegedAction<?> action)	
> 返回可调用对象（当调用时）**运行给定特权操作**并返回其结果。


```java
   //callable​(PrivilegedAction<?> action)
        PrivilegedAction p = new PrivilegedAction() {
            @Override
            public Object run() {
                //定义权限条件
                return "需要满足权限条件来触发一些特权";
            }
        };
        Callable<Object> ca = Executors.callable(p);
        FutureTask futureTask = new FutureTask(ca);
        Thread thread1 = new Thread(futureTask);
        thread1.start();
        Object o = futureTask.get();
        System.out.println(o);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b1ade75f8de43809e5177d9e092b9b0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

#### static Callable<Object> callable​(PrivilegedExceptionAction<?> action)

> static Callable<Object>	callable​(PrivilegedExceptionAction<?> action)	
> 返回可调用对象（当调用时**）运行给定特权异常操作**并返回其结果。


```java
  PrivilegedExceptionAction pp = new PrivilegedExceptionAction() {
            @Override
            public Object run() throws Exception {

                return "特权异常怎么处理";
            }
        };//可以用Lambada
        Callable<Object> cal = Executors.callable(pp);
        FutureTask<Object> objectFutureTask = new FutureTask<>(cal);
        new Thread(objectFutureTask).start();
        System.out.println(objectFutureTask.get());

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c42dcd88aca742cc84b4614b9563d2ba.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 静态方法------------>>>>返回值为ThreadFactory

#### static ThreadFactory defaultThreadFactory()

static ThreadFactory	defaultThreadFactory()	
返回用于创建新线程的默认线程工厂。

```java
    //线程工厂用于创建新线程,该线程非守护线程优先级也是线程组中允许的较小和最大优先级.默认为Default
        //新线程的名称可通过线程.getName（）的池-N线程-M（N）访问，其中N是本工厂的序列号，M是该工厂创建的线程的序列号。
        ThreadFactory threadFactory = Executors.defaultThreadFactory();
        Thread thread2 = threadFactory.newThread(() -> {
            System.out.println("开第一个新线程");
        });
        Thread thread3 = threadFactory.newThread(() -> {
            System.out.println("开第二个新线程");
        });
        thread2.start();
        thread3.start();
        System.out.println(thread2.getName());//pool-1-thread-1
        System.out.println(thread3.getName());//pool-1-thread-2
```
分析------------>>>>
![](C:\Users\Gavin\Pictures\Camera Roll\39.png)
### 静态方法------------>>>>返回值为ExecutorService	

#### static ExecutorService newCachedThreadPool()

> static ExecutorService	newCachedThreadPool()	
> 创建一个线程池，根据需要创建新线程，但在可用时将重复使用以前构建的线程。

创建一个线程池，根据需要**创建新线程**，但在**可用时将重复使用以前构建的线程**。这些池通常会**提高**执行许多**短寿命异步任务的程序的性能**。如果可用，呼叫将重复使用以前构建的线程。如果没有可用的线程，将**创建一个新的线程并添加到池中。未使用 60 秒的线程被终止并从缓存中删除。**因此，**闲置足够长的时间池不会消耗任何资源**。请注意，具有类似属性但具有不同详细信息（例如超时参数）的池可能使用线程池执行器构造器创建。execute

```java
  // 创建一个线程池，根据需要创建新线程，但在可用时将重复使用以前构建的线程。
        ExecutorService service = Executors.newCachedThreadPool();
        Thread thread4 = new Thread(() -> {
            System.out.println("我是一个线程");
        });
        FutureTask F= new FutureTask(new Callable() {
            @Override
            public Object call() throws Exception {
                System.out.println("我也是一个线程");
                return null;
            }
        });
        service.submit(F);
        service.submit(thread4);
        System.out.println(thread4.getName());
        service.shutdown();
```
之前在这里遇到过问题,所以也需要分析一下

分析------------>>>
![](C:\Users\Gavin\Pictures\Camera Roll\40.png)

#### static ExecutorService newCachedThreadPool​(**ThreadFactory threadFactory**)
> static ExecutorService	newCachedThreadPool​(**ThreadFactory threadFactory**)	
> 创建一个线程池，根据需要创建新线程，但在可用时将重复使用以前构建的线程，并在需要时使用提供的线程工厂创建新线程。

简言之就是传入一个线程工厂并缓存起来

```java
     ExecutorService service1 = Executors.newCachedThreadPool(threadFactory);
        Thread thread5 = threadFactory.newThread(() -> {
            System.out.println("由线程工厂创建的线程缓冲池中的线程");
        });
        Thread thread6 = threadFactory.newThread(() -> {
            System.out.println("由线程工厂创建的线程缓冲池中的线程");
        });
        service1.submit(thread5);
        service1.submit(thread6);
        System.out.println(thread5.getName());//pool-1-thread-3
        System.out.println(thread6.getName());// pool-1-thread-4
        service1.shutdown();
        
```

#### static ExecutorService	newFixedThreadPool​(int nThreads
> **static ExecutorService	newFixedThreadPool​(int nThreads)**	
> 创建一个nThreads大小的线程池，该线程池**可重复使用从共享的无限制队列**中运行的固定数量的线程。在任何时候，大多数线程都将是**主动处理任务**。如果在**所有线程处于活动状态时提交其他任务，则它们将在队列中等待，直到有线程可用**。如果任何线程在关闭前执行**过程中因故障而终止，则如果执行后续任务需要，则将取代新线程。**池中的线程将存在，直到它被明确关闭。

**简言之就是我这有一个池子,能容纳 nThreads个线程,当池子满了的时候先添加的线程进入等待,直到池子有空位置的时候,如果池子中的线程再关闭时因为故障而终止,新线程就会取而代之,除非仍然需要这个线程干活,这个线程才会留在池子里,直到它明确关闭**

```java
/*   创建一个nThreads大小的线程池，该线程池可重复使用从共享的无限制队列中运行的固定数量的线程。
        在任何时候，大多数线程都将是主动处理任务。如果在所有线程处于活动状态时提交其他任务，
        则它们将在队列中等待，直到有线程可用。如果任何线程在关闭前执行过程中因故障而终止，
        则如果执行后续任务需要，则将取代新线程。池中的线程将存在，直到它被明确关闭。*/
        ExecutorService service2 = Executors.newFixedThreadPool(1);
        Thread  thread7;
        Thread  thread8;
        thread7 = new Thread(() -> {
            for (int i = 10; i < 20; i++) {
            }
            System.out.println("我还留在线程池里吗?");
        });
        thread8 = new Thread(() -> {
            for (int i = 0; i < 100; i++) {
            }
            System.out.println("我被执行了吗?");
        });
        service2.submit(thread7);
        service2.submit(thread8);
        service2.shutdown();
```

#### static ExecutorService newFixedThreadPool​(int nThreads, ThreadFactory threadFactory)

> static ExecutorService	newFixedThreadPool​(int nThreads, ThreadFactory threadFactory)	
> 创建一个线程池，该线程池可重复使用从共享的无限制队列中运行的固定数量的线程，使用提供的**线程工厂在需要时创建新线程**。

![](C:\Users\Gavin\Pictures\Camera Roll\41.png)

#### static ExecutorService newFixedThreadPool​(int nThreads, ThreadFactory threadFactory)

> static ExecutorService	newFixedThreadPool​(int nThreads, ThreadFactory threadFactory)	
> 创建一个线程池，该线程池可重复使用从共享的无限制队列中运行的固定数量的线程，使用提供的线程工厂在需要时创建新线程。

跟上面类似,这次是用自己指定的线程工厂创建线程
![在这里插入图片描述](https://img-blog.csdnimg.cn/64627c8fd05c495ca2d3820cf7ee0fe4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
### 静态方法------------>>>>返回值为ExecutorService	

#### static ScheduledExecutorService newScheduledThreadPool​(int corePoolSize)

> static ScheduledExecutorService	newScheduledThreadPool​(int corePoolSize)	
> 创建一个线程池，可以安排命令在给定延迟后运行，或定期执行。
> **corePoolSize- 要保留在池中的线程数，即使它们处于空闲状态**
>

```java
 /* 可安排命令在给定延迟后运行或定期执行的执行者服务。
 这些方法会产生各种延迟的任务，并返回可用于取消或检查执行的任务对象。
 并且方法创建和执行定期运行直到取消的任务。schedulescheduleAtFixedRatescheduleWithFixedDelay
使用执行器.执行（可运行）和执行器服务方法提交的命令的延迟为零。
方法中还**允许零延迟和负延迟（但不允许延迟）**，并视为立即执行请求。*/
      ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(1);
        //可以传Runnable
        ScheduledFuture<?> ai = scheduledExecutorService.schedule(() -> System.out.println("I love you"), 5, TimeUnit.SECONDS);//延迟为0或者负值则视为不延迟

        //还可以传Callable
        ScheduledFuture<String> schedule = scheduledExecutorService.schedule(() -> "我爱你中国", 2, TimeUnit.SECONDS);
        System.out.println("定时任务的返回值"+schedule.get());

        //除了执行一次性任务外,还可以执行多次任务
        Runnable runnable=()->{
            System.out.println("I LOVE YOU BABY");
        };
        //参数 ,要执行的线程,第一次执行延期的时间,连续执行之间的时间
        ScheduledFuture<?> scheduledFuture = scheduledExecutorService.scheduleAtFixedRate(runnable, 10, 1, TimeUnit.SECONDS);//初始执行延迟10秒,之后每隔一秒打印一句I LOVE YOU BABY

        //scheduledExecutorService.shutdown();

        System.out.println("------------------------Executors.newScheduledThreadPool(2, threadFactory1);----------------------------");
        ScheduledExecutorService scheduledExecutorService1 = Executors.newScheduledThreadPool(2, threadFactory1);
        Thread thread11 = threadFactory1.newThread(() -> {
            System.out.println("五秒后提醒我玩游戏");
        });
        scheduledExecutorService1.schedule(thread11, 6, TimeUnit.SECONDS);
        scheduledExecutorService1.shutdown();
```

### 简单研究一下scheduleAtFixedRate与scheduleWithFixedDelay

#### scheduleAtFixedRate

分为两种情况------------>>>>>>>>>>>>
**第一种是如果任务执行的时间小于等于周期即设置的参数----period 那么就会按照周期设定值进行循环任务执行**


```java
//导包.....略
public class TestSchedule {
    public static void main(String[] args) {
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(2);
        Runnable runnable=()->{
            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("I LOVE YOU BABY");
        };
        scheduledExecutorService.scheduleAtFixedRate(runnable,1,1, TimeUnit.SECONDS);
    }
}

```

>    //假设初始时间为0    
>       //任务第一次执行     延迟1秒   
>       //从开始到第一次执行时间 要延迟1秒,在到第一次执行完,总共花了initialDelay(初始延迟1秒)+ 执行时间(忽略不计,反正不超过1秒),执行时间不超过Period,下一个任务在上一个执行完毕之后1秒后开始执行,即各任务之间并不是并发任务
>       //第二次执行的时间为initialDelay(1秒)+ 第一次执行时间(忽略不计)=第2秒 开始, 第二次执行完后1秒又开始执行第三次


**第二种是如果任务时间大于周期即设置的参数----period ,那么就会在等待上一个任务完成后立即执行下一个任务**


```java

public class TestSchedule {
    public static void main(String[] args) {
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(2);
        Runnable runnable=()->{
            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("I LOVE YOU BABY");
        };
        scheduledExecutorService.scheduleAtFixedRate(runnable,1,1, TimeUnit.SECONDS);
    }
}
```

> //依旧假设初始时间为0
>       //任务第一次执行延迟1秒,每隔1秒执行一次,但是一次任务需要5秒,\
>       //开始到第一次执行时间延迟1秒,到第一次执行完    总共花了initialDelay(1秒)+ 执行时间(5秒),然 超过Period了,下一个线程等待等上一个线程结束后才开始立即执行,    即并不是并发任务
>       //第二次执行的时间为initialDelay(1秒)+ 第一次执行时间(5秒)=第六秒 开始,然后执行一次又花了5秒, 第二次执行完的时间  为第11秒

#### scheduleWithFixedDelay

也是分为两种情况讨论--------->>>>>>>
**第一种任务执行时间小于等于延迟时间**

```java
public class TestSchedule {
    public static void main(String[] args) {
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(2);
             Runnable runnable2=()->{
            System.out.println("我爱你宝贝");
        };
scheduledExecutorService.scheduleWithFixedDelay(runnable2,1,3,TimeUnit.SECONDS);
    }
}
```

> //依旧假设初始时间为0;
> 从开始到第一个任务执行    延迟1秒                                 第一秒
> 第一个任务开始到第一个任务结束     花费时间忽略不计
> 从第一个任务结束到第二个任务开始    花费 3秒  即第二个任务在第四秒时开始;


**第二种情况是当任务执行时间大于延迟时间**

```java

public class TestSchedule {
    public static void main(String[] args) {
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(2);
        Runnable runnable2=()->{
            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("我爱你宝贝");
        };
scheduledExecutorService.scheduleWithFixedDelay(runnable2,1,3,TimeUnit.SECONDS);
    }
}

```

> 依旧假设初始时间为0 第一次执行时 延迟 1秒,      第1秒 
> 由于任务执行时间为5秒, 所以第一次执行时间结束为   第6秒,  
> 等着第一次任务执行结束后 在延迟   3秒执行下一次任务,    第9秒
> 即每次任务结束到下一次任务开始之间间隔为 3秒  所以第二次执行开始的时间为  第  9秒,结束时间 第14秒,在等待三秒下一次任务才开始执行;

看一下源码吧!!!!
![](C:\Users\Gavin\Pictures\Camera Roll\42.png)

![](C:\Users\Gavin\Pictures\Camera Roll\43.png)
未完待续.......





从生命的角度讲,人的生命周期----由受精卵开始到婴儿儿童........这不是重点,你要知道的;


# 线程的生命周期

你觉得线程的生命周期是从哪里开始的?

![](C:\Users\Gavin\Pictures\Camera Roll\44.png)

当然是new一个线程对象了呗!!!!

## NEW RUNNABLE 与TERMINATED

```java
package ThreadStateDemo;

import java.util.concurrent.TimeUnit;
class SleepHelper{
    public static  void sleepHelper( int time){
        try {
            TimeUnit.SECONDS.sleep(time);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
public class ThreadDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(() -> {
            System.out.println("2--"+Thread.currentThread().getState());
            for (int i = 0; i < 3; i++) {
               SleepHelper.sleepHelper(1);
                System.out.print(i+" ");
            }
            System.out.println();
        });
        System.out.println("1--"+thread.getState());//线程启动之前
        thread.start();线程启动
        thread.join();
        System.out.println("3--"+thread.getState());//不加join()那么输出的是main的状态----runnable.不信你就试试看
    }
}

```
运行一下你会得到什么结果?
![在这里插入图片描述](https://img-blog.csdnimg.cn/e45dc00eabb04fbc88d9a8a0a311cfb3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
## WAITING与TIMED_WAITING

```java
 Thread thread= new Thread(()->{
           LockSupport.park();//执行时先阻塞
           System.out.println("Thread---go-go-go");
           SleepHelper.sleepHelper(4);//再睡四秒
       });
       thread.start();

       SleepHelper.sleepHelper(1);//让主线程睡一秒,已确保thread先启动,不能用join(),因为 LockSupport.park();没有可以唤醒它的第三者线程了,就会一直waiting
        System.out.println("4--"+thread.getState());
         //再唤醒线程thread,所以一定要让thread先执行起来;
        LockSupport.unpark(thread);
        SleepHelper.sleepHelper(1);//让主线程睡一秒,已确保thread先启动
        System.out.println("5--"+thread.getState());
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a83eb1b1256a42a1bed7235bc3bcd01f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### LockSupport.park()后的状态

```java
 Thread thread = new Thread(() -> {
            LockSupport.park();

        });
        thread.start();
        SleepHelper.sleepHelper(1);
        System.out.println("8--" + thread.getState());

```

## synchronized与Lock锁等待的状态

```java
        final Object o = new Object();
        Thread thread = new Thread(() -> {
            synchronized (o) {
                System.out.println("我想要拿到那把锁");
            }
        });
        Thread thread1 = new Thread(() -> {
            synchronized (o) {
                System.out.println("我先拿到那把锁了");
                //拿到锁睡5秒
                SleepHelper.sleepHelper(5);
            }
        });
        thread1.start();
        //主线程睡一秒
        SleepHelper.sleepHelper(1);
        //然后启动thread2
        thread.start();
        SleepHelper.sleepHelper(1);
        System.out.println("6--" + thread.getState());
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/323edf7e8373491aa8efac7732028bd3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **synchronized同步方法拿不到锁的时候是blocked状态,其他锁的是waiting状态------自旋等待状态**

将上面的锁换成Lock

```java
 Lock lock = new ReentrantLock();
        Thread thread = new Thread(() -> {
            lock.lock();//try catch finally 省略了吧
            System.out.println("我想要拿到那把锁");
            lock.unlock();
        });
        Thread thread1 = new Thread(() -> {
            lock.lock();
            System.out.println("我先拿到那把锁了");
            //拿到锁睡5秒
            SleepHelper.sleepHelper(5);
            lock.unlock();
        });
        thread1.start();
        //主线程睡一秒
        SleepHelper.sleepHelper(1);
        //然后启动thread2
        thread.start();
        SleepHelper.sleepHelper(1);
        System.out.println("7--" + thread.getState());
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/63f7f650ef5e4a7bb5dd032f55933689.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **原因是[Lock锁是JUC中的锁](https://blog.csdn.net/yjp198713/article/details/78941709)底层是CAS,会自旋,拿不到锁就在那里转圈圈等待而非阻塞状态;**





# 线程也可以很优雅的退场
![在这里插入图片描述](https://img-blog.csdnimg.cn/eaf190ba44114abba4d4afdbc279134e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
对程序员来说优雅就是能够掌控代码的一切!!!
在java中有三个很容易让初学者混乱的三个方法
interrupt()、isInterrupted()和interrupted(),他们到底有什么区别呢?接下来一来看看吧!

## interrupt()

**public void interrupt()**
方法描述---->>>>>
中断此线程。

如果此线程在调用 Object 类的**wait(), wait(long)， 或者 wait(long, int)的方法 ，或 join(), join(long), join(long, int), sleep(long)， 或者 sleep(long, int), ，**那么它的中断状态将被清除并且它 将抛出 InterruptedException.

如果此线程**在 I/O 操作中**被阻塞 InterruptibleChannel那么通道将被关闭，线程的中断 状态将被设置，并且线程将收到一个 **ClosedByInterruptException.**

> 如果前面的条件都不成立，则该线程的中断 状态将被设置。

代码1演示------>>>

```java
public class InterRuptedDemo {
    public static void main(String[] args) {
       Thread t= new Thread(()->{
            for (; ; ) {
                System.out.println("没有被真正打断");
            }
        });
       t.start();
       SleepHelper.sleepHelper(2);
      t.interrupt();
    }

}
```
输出结果是一直是在运行状态,并没有被打断,
什么原因呢???
原因就是当线程调用此方法是只是给该线程设置一个标记位,以便在后续能够使得程序能够按照我们设定的方式结束;


## isInterrupted()
**public boolean isInterrupted()** 

测试此线程是否已被中断。 该 中断 状态 线程的 不受此方法的影响。

线程中断被忽略，因为线程不活动 在中断的时候会被这个方法反映 返回假。 


看一下代码2演示------>>

```java

public class InterRuptedDemo {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            for (; ; ) {
                System.out.println("没有被真正打断");
                if (Thread.currentThread().isInterrupted()) {
                    System.out.println("Thread is interrupted");
                    System.out.println(Thread.currentThread().isInterrupted());
                }
            }
        });

        t.start();
//让主线程睡2秒使得能够运行起来,然后调用打断方法
        SleepHelper.sleepHelper(2);
        t.interrupt();
    }

}

```

以上代码得运行结果也是程序没有被打断,而是继续执行(死循环)
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2631ae880724b1b864e4453bcf776b2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
那代码2运行结果为什么会这样呢?
原因是  isinterupted()方法用来检测某个线程是否有中断标志位,从运行结果也可以看到   返回值为true,说明线程的确被设置标志位了;


当我们检测到标记位时,我们可以调用  break;/return/Sysytem.exit(1)等方法来结束线程/程序的运行





## interrupted()
**public static boolean interrupted()**
方法描述---->>
测试当前线程是否被中断。 这 中断状态 通过该方法清除线程的 。 在 换句话说，如果这个方法被连续调用两次， 第二次调用将返回 false（除非当前线程是 再次中断，在第一个呼叫清除其中断后 状态并且在第二个电话检查它之前）。

线程中断被忽略，因为线程不活动 在中断的时候会被这个方法反映 返回假。 

简言之就是查询当前线程是否被中断过,并重置打断标记;
代码3----------->>>

```java
 if (Thread.interrupted()) {//这里检查时候有打断标记,如果有,则输出下面内容
                    System.out.println("Thread is interrupted");
                    System.out.println(Thread.currentThread().isInterrupted());//检测是否有打断标记位
                  // break;//跳出死循环
                    //return;返回方法调用处
                    System.exit(1);//结束
                }
```
结果如下图------------->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/3a03495d705546f189396c6b7a73978b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

当调用两次Thread.interrupted(),但是interrupt()方法只调用一次

代码4----------->>
```java

public class InterRuptedDemo {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            for (; ; ) {
                System.out.println("没有被真正打断");
                //这里检查的时候结果为true,但是线程打断标记被重置为false,为了能够输出if块内容,取反
                if (!Thread.interrupted()) {
                    System.out.println("Thread is interrupted");
                    System.out.println(Thread.interrupted());//检测是否有打断标记位
                  // break;//跳出死循环
                    //return;返回方法调用处
                    System.exit(1);//结束
                }
            }
        });

        t.start();
//让主线程睡2秒使得能够运行起来,然后调用打断方法
        SleepHelper.sleepHelper(2);
        t.interrupt();
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6e8e597b5fda4f169ded9f56ca3e80cb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**如何优雅的结束线程~~interrupt()和isInterrupted()**

下面就是正确的打断线程/程序执行的方式-------->>
代码5------------------->>>>>   

```java

public class InterRuptedDemo {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            for (; ; ) {
                System.out.println("没有被真正打断");
                if (Thread.currentThread().isInterrupted()) {
                    System.out.println("Thread is interrupted");
                    System.out.println(Thread.currentThread().isInterrupted());//检测是否有打断标记位
                  // break;//跳出死循环
                    //return;返回方法调用处
                    System.exit(1);//结束
                }
            }
        });

        t.start();
//让主线程睡2秒使得能够运行起来,然后调用打断方法
        SleepHelper.sleepHelper(2);
        t.interrupt();
    }

}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7c658a58b0074bd0b36b71e078672065.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


当调用一下方法时 的代码演示------------>>>>>>>>>

wait(), wait(long)， 或者 wait(long, int)的方法 ，或 join(), join(long), join(long, int), sleep(long)， 或者 sleep(long, int), 

**有关sleep(long)/sleep(long,int)方法是的interrupt()方法**
当调用有关sleep方法时会抛出java.lang.InterruptedException:异常
代码6----------------->>>
```java

public class InterRuptedDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(() -> {
            for (int i=0;i<100 ;i++ ) {
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(i);
            }
        });

        t.start();
//让主线程睡2秒使得能够运行起来,然后调用打断方法
      TimeUnit.SECONDS.sleep(2);
        t.interrupt();
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d3fdae60ee84478e9c5032d71492abb8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
但是尽管抛异常但是程序还是继续执行,因为并没有真正打断该线程的执行而是设置了一个打断标志位,这之后怎么处理这个中断异常是结束运行还是不管他,让程序继续执行要看怎么处理了;

其他方法类似不在演示.

下面来演示一下在锁竞争状态设置中断标志位会不会抛出InterruptedException呢?

代码7---------------->>>>

```java
import java.util.concurrent.TimeUnit;
public class JoinDemo {
    public static void main(String[] args) throws InterruptedException {
        final Object o = new Object();
        Thread t = new Thread(() -> {
            synchronized (o) {
                System.out.println("我先拿到这把锁了");
                try {//之后睡十秒
                    TimeUnit.SECONDS.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        Thread TNT = new Thread(() -> {
            synchronized (o) {
                System.out.println("我也想拿到这把锁");
            }
        });
        t.start();
        //100毫秒后TNT启动
        Thread.sleep(100);
        TNT.start();
        Thread.sleep(100);
        //100毫秒后TNT设置中断标志
        TNT.interrupt();
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e67a88625fe4917b6d1077f90401833.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
换成Lock锁--------------JUC锁试试看!
![在这里插入图片描述](https://img-blog.csdnimg.cn/26e5f5b2d4a847179cebede2842e959b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
依然是不抛InterruptedException的

但是真就没办法打断lock锁了吗?
实际上是有一种方式打断的该方法叫lockInterruptibly()

##  JUC中的lockInterruptibly()方法

public void lockInterruptibly() throws InterruptedException
方法描述-------------->>>>>
获取锁，除非当前线程是 打断了 。

如果没有被另一个线程持有，则获取锁并返回 立即，将锁定保持计数设置为 1。

如果当前线程已经持有这个锁，那么持有计数 递增 1，该方法立即返回。

如果锁被另一个线程持有，那么 当前线程被禁用以进行线程调度 目的并处于休眠状态，直到发生以下两种情况之一：

    锁被当前线程获取； 或者
    其他某个线程 中断 了 当前线程。 

如果当前线程获取了锁，则锁保持 计数设置为一。

如果当前线程：

    在进入此方法时设置其中断状态； 或者
    被 中断 获取时 锁， 

然后 InterruptedException被抛出并且当前线程的 中断状态被清除。

在这个实现中，因为这个方法是一个显式的 中断点，优先响应 中断正常或可重入获取锁。 

简而言之就是 线程尝试获取这把锁如果在获得这把锁期间被设置了中断标志,则会抛出中断异常;
这个方法会结束该线程的等待状态;

代码8--------------->>>>>>>>>>>>

```java
package ThreadStateDemo;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class JoinDemo {
    public static void main(String[] args) throws InterruptedException {
        Lock lock = new ReentrantLock();
        Thread t=null;
        Thread TNT = null;
        t = new Thread(() -> {
            lock.lock();//try catch finally 省略
            try {
                System.out.println("我先拿到这把锁了");
                //之后睡十秒
                TimeUnit.SECONDS.sleep(10);
            } catch (InterruptedException e) {

            } finally {
                lock.unlock();
            }
        });
       TNT = new Thread(() -> {
            try {
                //尝试获得这把锁
                lock.lockInterruptibly();
                System.out.println("等我拿到这把锁");
            } catch (InterruptedException e) {
                System.out.println("这里抛出中断异常了");
            }finally {
                //这里判断TNT是否拿到了锁,如果没拿到那就不用解锁了
              if("RUNNABLE".equals(Thread.currentThread().getState())){
                   lock.unlock();
              }else{
                  //什么也不做
              }
            }
        });
        t.start();
        //100毫秒后TNT启动
        Thread.sleep(100);
        TNT.start();
        Thread.sleep(100);
        //100毫秒后TNT设置中断标志
        TNT.interrupt();
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e07b96931eb24545a3ce0a6b66b8d226.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

那这个方法有什么作用呢?

加入有一个任务拿到锁了,迟迟任务没有结束,另一个线程不能就这么一直等下去,结束等待状态;
代码9---------------->>>>>>>>

```java

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class JoinDemo {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();//可重入锁
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    lock.lock();
                    System.out.println("线程1开始---");
                    Thread.sleep(Integer.MAX_VALUE);
                    System.out.println("线程1结束---");
                } catch (InterruptedException e) {
                    System.out.println("线程1Interrupted");
                } finally {
                    lock.unlock();
                }

            }
        });
        thread.start();
        Thread thread1 = new Thread(() -> {
            try {
                // lock.lock();
                lock.lockInterruptibly();//如果当前线程没有被中断,则获取锁,如果已经中断,则抛出异常
                System.out.println("线程2开始---");
                Thread.sleep(1000);
                System.out.println("线程2结束---");
            } catch (InterruptedException e) {
                System.out.println("线程2结束等待");
            } finally {
                if ("RUNNABLE".equals(Thread.currentThread().getState())) {
                    lock.unlock();
                } else {
                    //什么也不做
                }
            }
        });
        thread1.start();
        try {
            Thread.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        thread1.interrupt();//结束线程2的等待状态
    }
}
```



有这么一个场景,用户在设置用户名和密码的时候突然遇到了一些异常什么的,需要终止程序的运行,那么怎么结束合适呢?
看一下代码1------->>>
```java

public class StopDemo {
    static String username;
    static String password;
    public static void main(String[] args) {
       Thread t= new Thread(()->{
           SleepHelper.sleepHelper(5);
           username="Gavin";
           System.out.println("设置的用户名为--"+username);
       });
        Thread tt= new Thread(()->{
            SleepHelper.sleepHelper(5);
           password="123456";
            System.out.println("设置的用户密码--"+password);
        });
        t.start();
        SleepHelper.sleepHelper(1);
        tt.start();
        SleepHelper.sleepHelper(1);
        tt.stop();
    }
}

```
stop()直接暴力的结束显然是不好的,因为用户名和密码是关联在一起的,只设置好用户名显然是不符合业务逻辑的(当然例子可能不是很符合日常思维,这不是重点,重点是两者业务之间的关系;)

那么在数据库中该怎么结束比较合适?
要么都不设置(回滚),要么等都设置完毕再进行结束操作;


stop()方法已经被废弃

## 为什么不建议使用stop()方法
在高并发的情况下,如果一个线程持有了同步锁,用stop方法强行将该线程杀死,那么如果这个线程还没有处理完任务,比如有人向你的账户里转100000000元钱,好没有处理完任务啪就给干结束了,这肯定不是你想要的,对于程序也是;





同样被废弃的还有一对方法----->>>

suspend()和resume()

## 为什么不建议使用suspend()和resume()

同样是高并发情况下,suspend()是让线程暂停,那么暂停的时候恰好是一个线程持有锁的时间,如果一直不被恢复执行,那么就会产生其他线程一致拿不到该锁,同时也很容易产生死锁问题;

## 优雅的结束线程~~volatile

```java

public class VolatileDemo {
    volatile static boolean flag = true;
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            long result = 0L;
            while (flag) {
                result++;
            }
            //第一次结果--2349296237
            //第二次结果--2556612429
            //第二次结果--2518435975
            System.out.println("end--" + result);
        });
        thread.start();
        SleepHelper.sleepHelper(1);
        flag = false;
    }
}

```

> **此种方法有局限性,当执行过程时间不影响执行的结果时可以用这种方式;
> 还有当在设置flag之前某些线程出现了阻塞或者其他什么的,同样线程也是无法结束的;

>
> **用interrupt方式来中断也有自己的局限性所在;**

```java
package ThreadStateDemo;

public class VolatileDemo {
     static boolean flag = true;
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            long result = 0L;
            while (flag) {
                result++;
                if(Thread.interrupted()){
                    break;
                }
            }
            //第一次结果--1841670558
            //第二次结果--1899516796
            //第二次结果--1830305875
            System.out.println("end--" + result);
        });
        thread.start();
        SleepHelper.sleepHelper(1);
       thread.interrupt();
    }
}

```

因此要想精准的中断线程,需要各个线程之间来配合;

> 总结----- 用volatile和interrupt() 打断线程可以看到interrupt()
> 打断的时间上要稍微快一些,同时也反映出volatile在弱同步上(可见性)的处理上是需要耗费一定时间的;

