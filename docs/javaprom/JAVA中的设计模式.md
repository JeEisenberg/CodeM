# JAVA中的设计模式

## 工厂设计

说到工厂，我就联想到了亚洲的大工厂富士康--接过订单然后按照固定的模板生产商品，其实java中工厂类中的工厂方法也是一样，接过参数，根据参数来生产需要的商品；

今天我们一起来看看何为工厂设计模式，少罗嗦，直接看东西--
假如有客户委托工厂生产披萨，那么这个工厂要先获得客户想要的披萨的相关信息，有没有披萨需要特殊定制的；
根据客户需要先整出一个父类---以下代码仅供参考

```java
package pissa.kind;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/4 - 07 - 04 - 8:26
 * @Description: com.piissa.order
 * @version: 1.0
 */
public  class Pissa {
//封装--这是必须的
    private String name;//名称
   private double price;//价格
    private int size;//价格
//通用构造方法，子类也能能用到
    public Pissa(String name, double price, int size) {
        this.name = name;
        this.price = price;
        this.size = size;
    }
//gettersetter方法获得封装之后的信息
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    public int getSize() {
        return size;
    }

    public void setSize(int size) {
        this.size = size;
    }
    //类似于产品展示方法--说明书
    public String showInfo() {
        return  "名称--" + name + "\n" +
                "价格--" + price +"\n" +
                "大小--" + size  ;
    }
}

```

在根据每个商品的独有的方面单独定制；

```java
 public class BaconPissa extends Pissa {
    private int BaconKG;
    public BaconPissa(String name, double price, int size,int baconKG) {
        super(name, price, size);
this.BaconKG=baconKG;
    }

    public int getBaconKG() {
        return BaconKG;
    }

    public void setBaconKG(int baconKG) {
        BaconKG = baconKG;
    }

    @Override
    public String showInfo() {
        return super.showInfo()+"\n培根的克数是："+BaconKG+"克";
    }
}


```

```java
public class FruitPissa extends Pissa {
    String fruitKind;

    public FruitPissa(String name, double price, int size,String fruitKind) {
        super(name, price, size);
        this.fruitKind=fruitKind;
    }

    public String getFruitKind() {
        return fruitKind;
    }

    public void setFruitKind(String fruitKind) {
        this.fruitKind = fruitKind;
    }

    @Override
    public String showInfo() {
        return super.showInfo()+"\n你要加入的水果："+fruitKind;
    }
}
```

定制生产模板完成之后，整个工厂类来接收客户定制的具体信息---

```java
package com.piissa.order;

import pissa.kind.BaconPissa;
import pissa.kind.FruitPissa;
import pissa.kind.Pissa;

import java.util.Scanner;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/4 - 07 - 04 - 8:41
 * @Description: com.piissa.order
 * @version: 1.0
 */
public  class GetPissaFactory {


        public static Pissa getPissa(int choice){
            Scanner sc = new Scanner(System.in);
            Pissa p = null;//先定义一个Pissa接收返回值
            switch (choice){
                case 1:
                {
                    System.out.println("请录入培根的克数：");
                    int BaconKG = sc.nextInt();
                    System.out.println("请录入匹萨的大小：");
                    int size = sc.nextInt();
                    System.out.println("请录入匹萨的价格：");
                    int price = sc.nextInt();
                    //将录入的信息封装为培根匹萨的对象：
                    BaconPissa baconP = new BaconPissa("培根匹萨",size,price,BaconKG);
                    p = baconP;
                }
                break;
                case 2:
                {
                    System.out.println("请输入想要加入的水果：");
                    String fruitKind = sc.next();
                    System.out.println("请输入匹萨的大小：");
                    int size = sc.nextInt();
                    System.out.println("请输入匹萨的零售价：");
                    int price = sc.nextInt();
                    //将录入的信息封装为水果匹萨的对象：
                    FruitPissa fruitP = new FruitPissa("水果匹萨",size,price,fruitKind);
                    p = fruitP;
                }
                break;
            }
            return p;
        }
    }

```

以上就定制完成，接下来在客户端呈现

```java
package com.piissa.order;

import pissa.kind.Pissa;

import java.util.Scanner;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/4 - 07 - 04 - 8:49
 * @Description: com.piissa.order
 * @version: 1.0
 */
public class PissaTest {
    public static void main(String[] args) {
        
        Scanner sc = new Scanner(System.in);
        System.out.println("请选择要购买的匹萨（1.培根匹萨 2.水果匹萨）:");
        int choice = sc.nextInt();//选择
      //通过工厂类获取匹萨：
        Pissa pissa = GetPissaFactory.getPissa(choice);
        System.out.println(pissa.showInfo());
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210704102334505.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 简单来说就是 
> 在制造之前要先准备具体的类信息，然后将我想要的产品（对象）传入到工厂类的工厂方法在中，然后工厂根据我提供的参数去生产我想要的产品；

共同学习共同进步；



## 单例设计
不整官方的那套,简单来说就是整个程序有且仅有该类的一个实例。要创建类的实例只能由自己负责创建自己的对象，同时确保只有一个对象被创建。

特点----
1----构造方法私有化
2----持有自己类型的属性
3----对外提供可获取实例的静态方法

**饿汉式**

```java
class Sun{
    String name;
    private Sun(){};
    final static Sun sun = new Sun();
    public static Sun getSun() {
        return sun;
    }
}
public class SingleB {
    public static void main(String[] args) {
        Sun sun = Sun.getSun();
        Sun sun1 = Sun.getSun();
        System.out.println(sun==sun1);
        System.out.println(sun==Sun.sun);
    }
}

```

以上就是人们常说的饿汉式,无论当下需不需要,我先把实例给提供出来,这种方式的好处就是能够保证线程的安全;

**饿汉式的变形**

```java

class Sun {
    private Sun() {//私有构造,只能通过调用GET方法获得实例
    }

    private  static  final Sun sun;
static {
    sun=new Sun();
}

    public static Sun getSun() {

        return sun;

    }
}
public class SingleB {
    public static void main(String[] args) {
        Sun sun = Sun.getSun();
        Sun sun1 = Sun.getSun();
        System.out.println(sun == sun1);
    }
}


```
看似是懒汉式,实则是饿汉式,因为都是在一开始就随类被加载到内存中了,以上这两种方式是一样的,

**懒汉式**

在需要的时候通过get方法来获得实例而不是一上来就加载实例;
```java

class Sun {
    private Sun() {//私有构造,只能通过调用GET方法获得实例
    }

    //既然不是一上来就加载,那么就不能在这里实例化
    //不能被final修饰,因为被final修饰要实例化
    private static Sun sun;


    public static Sun getSun() {
//        在这里实例化
        if (sun == null) {//如果实例为空,则进行初始化
            sun = new Sun();
        }
        return sun;//否则返回 实例sun

    }
}

public class SingleB {
    public static void main(String[] args) {
        Sun sun = Sun.getSun();
        Sun sun1 = Sun.getSun();
        System.out.println(sun == sun1);
    }
}
```
**懒汉式线程安全吗**

>  **当多个线程访问该懒汉式单例时----可能会存在以下问题,**
>  **一个线程A在判断sun==null之后还未生成sun实例被另一个线程B抢走了,这个线程B根据getSun方法获得了实例,之后又到了线程A执行,由于当时线程A判断sun为null则会通过getSun的到一个实例这个时候就会有两个Sun的实例存在;**

模拟----->>>


```java

import java.util.concurrent.TimeUnit;

class Sun {
    private Sun() {//私有构造,只能通过调用GET方法获得实例
    }

    //既然不是一上来就加载,那么就不能在这里实例化
    //不能被final修饰,因为被final修饰要实例化
    private static Sun sun;


    public static Sun getSun() {
//        在这里实例化
        if (sun == null) {//如果实例为空,则进行初始化
            try {
                TimeUnit.SECONDS.sleep(1);//模拟高并发时CPU时间片的划分,一个线程在判断sun==nullde 时候
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            sun = new Sun();
        }
         return sun;//否则返回 实例sun

    }
}

public class SingleB {
    public static void main(String[] args) {
       //准备一些线程
        for (int i = 0; i < 5; i++) {
            new Thread(()->{
                System.out.println(Sun.getSun().hashCode());
            }).start();
        }
        for (int i = 0; i < 5; i++) {
            new Thread(()->{
                System.out.println(Sun.getSun().hashCode());
            }).start();
        }
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/449502c00f124e789846e48feb5a87e9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

看结果,由于中间加了休眠,使得线程判断为null的时候没来的接new就被别的线程抢走了,导致产生了很多个实例;

再来一个实例----->>>

```java

import java.util.concurrent.TimeUnit;

class Sun {
    private Sun() {//私有构造,只能通过调用GET方法获得实例
    }

    //既然不是一上来就加载,那么就不能在这里实例化
    //不能被final修饰,因为被final修饰要实例化
    private static volatile   Sun sun;
    public static  Sun getSun() {
//        在这里实例化
        if (sun == null) {//如果实例为空,则进行初始化
            try {
                TimeUnit.SECONDS.sleep(2);//模拟高并发时CPU时间片的划分,一个线程在判断sun==nullde 时候
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            sun = new Sun();
        }

         return sun;//否则返回 实例sun

    }
}

public class SingleB {
   static  int j=0;
   static int k=5;
    public static void main(String[] args) {
       //准备一些线程
        Sun [] s=new Sun[10];

        for (int i = 0; i < 5; i++) {
            new Thread(()->{
                s[j]=Sun.getSun();
                System.out.println(s[j].hashCode());
               j++;
            }).start();
        }

        for (int i = 0; i < 5; i++) {
            new Thread(()->{
                s[k]=Sun.getSun();
                System.out.println(s[k].hashCode());
                  k++;
            }).start();
        }
    }
}

```



**懒汉式线程安全解决方法**

**synchronized同步**

同步的方式有两种---同步方法与同步代码块
```java

class Sun{
    String name;
    private static Sun  sun ;//单例设计模式
   private Sun() {
          }
    public static synchronized Sun getInstance(){
//一些其他代码可读取数据的代码
       if(sun==null){//如果没有实例存在,那么我在进行实例化
           try {
               Thread.sleep(10);
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
           sun= new Sun();
       }//如果有,我直接返回该实例就可以了
        return sun;
    }
    public void MethodA(){
       sun.name="懒汉式";
        System.out.println("假设我在MethodA方法中要用到该实例"+sun.name);
    }
}
public class SingleTest {
    public static void main(String[] args) {
        for (int i = 0; i <5 ; i++) {
            new Thread(()-> System.out.println(Sun.getInstance().toString())).start();
        }

    }
}

```

> **这种方式的缺点------假如有10000个线程需要获得该对象,但是最终只有一个线程会真正new出来一个,其他的全在那里等待,时间效率低;另一个如果方法中除了new对象操作还有还有其他代码,但是这些代码也被上了锁,没拿到锁的线程也只能等待拿到锁的线程释放锁才能进行读取**

**Lock锁同步**

但是随着学习的深入,有些事情还是有必要钻牛角尖的,比如上述线程安全到底出现在哪?如果一开始就有实例,就不可能出现线程安全问题------即饿汉式的写法,所以懒汉式线程安全问题处在一开始实例化的时候,所以只要在初始化的时候加一把锁就可以了-----即锁的细化(锁住的代码越少越好,节约时间使得同步的操作数尽可能少,如果存在锁竞争也会缩短锁等待的时间)

![在这里插入图片描述](https://img-blog.csdnimg.cn/523d855386894f4a810d36ba8fed78d1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
但是这种方法也有缺陷,就是虽然细化了,但是依然存在时间效率低的问题;



**双重检查--------DCL**

```java

class Sun{
    private static ReentrantLock lock= new ReentrantLock();
    String name;
    private  static Sun  sun ;//单例设计模式

   private Sun() {
          }
    public static  Sun getInstance(){
    if{sun==null){//第一次检查如果为空
        lock.lock();
        try {
            if (sun == null) {//第二次检查,防止第一次检查时线程被切换了,如果还没有实例存在,那么我在进行实例化
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                sun = new Sun();
           } }//如果有,我直接返回该实例就可以了
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
        return sun;
    }
    public void MethodA(){
       sun.name="懒汉式";
        System.out.println("假设我在MethodA方法中要用到该实例"+sun.name);
    }
}
public class SingleTest {
    public static void main(String[] args) {
        for (int i = 0; i <5 ; i++) {
            new Thread(()-> System.out.println(Sun.getInstance().toString())).start();
        }

    }
}
```
这种方式也会有一个缺陷---就是我得到的实例可能不是一个完全初始化实例,而是一个半初始化实例;
为什么呢?且看这篇文章[什么是对象半初始化状态](https://blog.csdn.net/weixin_54061333/article/details/120660659)

**基于同步的改进----volatile关键字**

双重检查+volatile

```java

class Sun{
    private static ReentrantLock lock= new ReentrantLock();
    String name;
    private volatile static Sun  sun ;//单例设计模式

   private Sun() {
          }
    public static  Sun getInstance(){
        lock.lock();
        try {
            if (sun == null) {//如果没有实例存在,那么我在进行实例化
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                sun = new Sun();

            }//如果有,我直接返回该实例就可以了
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }

        return sun;

    }
    public void MethodA(){
       sun.name="懒汉式";
        System.out.println("假设我在MethodA方法中要用到该实例"+sun.name);
    }
}
public class SingleTest {
    public static void main(String[] args) {
        for (int i = 0; i <5 ; i++) {
            new Thread(()-> System.out.println(Sun.getInstance().toString())).start();
        }

    }
}
```



这种方式的好处是,只有第一次创建实例的时候我才判断实例是否为null一旦创建了.就不再去判断了,因为volatile关键字修饰的变量一旦更改会立即同步到内存中,且其他线程都是可以看见的,其他线程就不会在去new一个对象了,即下一个线程会直接跳过同步代码块,而直接去取Sun的实例;

**volatile关键字的用法**

先看被volatile关键字修饰的两个性质---

1,可见性,

点击查看之前的文章,不在赘述;
[共享数据----static与volatile有啥区别?????](https://blog.csdn.net/weixin_54061333/article/details/119670548)

2,volatile关键字能禁止指令重排序

关于指令重排大致意思就是jvm在每次代码执行前都会做一个优化,调整一下执行顺序--比如之前提到过的   字符串拼接,包装类的以下 案例,反编译之后会发现[jvm对此做了一些优化](https://blog.csdn.net/weixin_54061333/article/details/118677201);


```java

public class Test01 {
  static volatile boolean flag=true;      
    public static void main(String[] args) {
      
        int j = 1;//a
        int i = 8;//b
        j = i;//c
        Test01.flag = false;//d
        i = 4;//e
        j = 10;//f
    }
}

```
当flag不是 volatile  修饰的时候,jvm会调整以下 以上数据的顺序或者简化以便提高程序执行效率;
但是当flag是 volatile  修饰的时候,能保证在flag执行之前,代码a,b,c先执行, 但并不保证flag之前的代码执行顺序一定是按a,b,c顺序执行的,e,f则是在flag执行完之后执行.

查了下比较正式的说法--------

> volatile关键字禁止指令重排序有两层意思：
>
> 　　1）当程序执行到volatile变量的读操作或者写操作时，在其前面的操作的更改肯定全部已经进行，且结果已经对后面的操作可见；在其后面的操作肯定还没有进行；
>
> 　　2）在进行指令优化时，不能将在对volatile变量访问的语句放在其后面执行，也不能把volatile变量后面的语句放到其前面执行。

**volatile关键字的实现原理和实现机制**

JVM虚拟机
> 1）它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；
>
> 　　2）它会强制将对缓存的修改操作立即写入主存；
>
> 　　3）如果是写操作，它会导致其他CPU中对应的缓存行无效。

## 观察者设计

![](D:\WebRepository\pic\图片25.png)

如果想要实现这样的观察者设计.需要是一个Observer接口和Observeable 类

```java
import java.util.Observable;
import java.util.Observer;
@SuppressWarnings("deprecation")
public  class Per implementsObserver {
	@Override
	public  void update(Observablearg0. Object arg1) {
		// TODO Auto-generated method stub
		//arg1表示改变之后的价格;
		//arg0表示被观察的对象;
		System.out.println("被观察的操作有所修改"+arg0+arg1);
	}
}


```

```java
import java.util.Observable;
@SuppressWarnings("deprecation")
public  class House extendsObservable {
private float  price;
public   House (float  price) {
	this.price=price;}
public   String toString() {
	return  "房子";  }
public   float   getPrice() {
	return  price;}
public  voidsetPrice(float  price) {
	super.setChanged();//通知内容跟已经可以修改
	this.price = price;
	//一旦修改.则表示价格改变.应该立即通知观察者
		super.notifyObservers(price);//通知所有观察这已经改变}}

```

```java
public  class TestObserver {
	@SuppressWarnings("deprecation")
	public  static  void main(String[] args) {
		House h=new House(12000.2f);
Per P1=new Per();
Per P2=new Per();
Per P3=new Per();
h.addObserver(P1);//增加一个观察者
h.addObserver(P2);//增加一个观察者
h.addObserver(P3);//增加一个观察者
h.setPrice(15000.8f);
	}
}
```

每一个人表示一个观察者.每个人观察Observerable的子类；当房子的价格改变的时候,首先要设置 改变的发生点之后通过notifyObserver()方法唤醒全部的观察者；



## 适配器设计

用一个类先将此接口实现.但是所有的方法都属于空实现.之后在继承此类；

应该使用抽象类

```java
interface Window {
	public void Open();//打开窗口
	public void Close();//关闭窗口
	public void Min_icon();//最小化
	public void Max_icon();//最大化
}
abstract class Windows implements Window{
	public void Open() {//打开窗口
	}
	public void Close() {//关闭窗口
	}
	public void Min_icon() {//最小化
	}
	public void Max_icon() {//最大化
	}}

class My_window extends Windows{
public void Open() {//打开窗口
		System.out.println("打开窗口");	}
public void Min_icon() {//最小化
	System.out.println("窗口最小化");
}}
public class Lianxi {
	public static void main (String args[]) {
		Window win10=new My_window();
		win10.Open();
		win10.Close();//空方法
		win10.Min_icon();
	}
}


```

## 集成设计模式

将多个类集合到一个大类，形成一个整体；举个例子，一台电脑--台式机，台式机有什么组成？-

```java
class Cpu {}
class Disk {}
class Dianyuan {}
class Xianshiqi {}
class Jianpan {}
class Zhuban {
	private Cpu cpu;
	private Disk disk;}
class Zhujixiang {
	private Zhuban zhuban;
	private Dianyuan dianyuan;
}
class Computer {
	private Zhujixiang zhujixiang;
	private Xianshiqi xianshiqi;
	private Jianpan jianpan;
}
public class Myfun {
	public static void main(String args[]) {
	}   }
```

