# Java面试题

## 1,JDK,JRE与JVM的区别

JDK--也就是我们常说的java开发环境--开发人员使用
JRE--也就是java运行环境--普通用户使用，开发人员也需要用
JVM--也就是我们常说的java虚拟机
三者关系如下

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062218162674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



![在这里插入图片描述](https://img-blog.csdnimg.cn/20210622191640325.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

[核心文档](https://blog.csdn.net/weixin_54061333/article/details/118112689)

## 2,什么是面向对象?

面向对象一般要跟面向过程做对比;

**面向过程**就是按照步骤一步一步来实现,即每一步要做什么,更加注重的是步骤,在这个步骤中需要有哪些参与者;

> 把大象放进冰箱需要几步?
>
> 1,人把冰箱门打开
>
> 2,人把大象放进去
>
> 3,人关闭冰箱门

**面向对象**就是将某个事件/物体抽象出来然后这个对象有什么功能,这个功能怎么实现的外部人员不关心;

> 把大象放进冰箱需要几步?
>
> 人~对象 把冰箱门打开/关上
>
> 大象~对象 被装进冰箱
>
> 冰箱~对象  盛放大象

从这个例子可以看出,如果我们把大象换成别的,面向过程的方式需要重投开始更改代码,而面向对象则只需要将大象换成对应的对象即可;~可扩展性好,易于维护;



面向对象的三大特性~~封装,继承和多态

**封装的形式:**

private 封装属性,明确标识了哪些数据可以被外部访问,以及这些数据不能被外部随意修改;

orm封装~即将代码封装成一个可以完成某项功能的jar包,我们不关心该jar内部是怎样实现的,只需要引入该jar,然后调用方法完成需要的操作即可;

**继承的形式:**

继承基类的非private/final修饰的方法

> 子类重写继承的方法时,不可以降低方法的访问权限，子类继承父类的访问修饰符要比父类的更大，也就是更加开放，假如父类是protected修饰的，其子类只能是protected或者public，绝对不能是private，当然使用private就不是继承了。还要注意的是，继承当中子类抛出的异常必须是父类抛出的异常的子异常，或者子类抛出的异常要比父类抛出的异常要少。

多态的形式:

继承基类

> 存在单继承的缺陷

实现接口

> 可以多实现

## 3,== 和 equals 区别是什么

`==` 是 Java 中一种操作符，它有两种比较方式

- 对于`基本数据类型`来说， == 判断的是两边的`值`是否相等
- 对于`引用类型`来说， == 判断的是两边的`引用`是否相等，也就是判断两个对象是否指向了同一块内存区域。

> 在很多java内置类中重新写了equals方法

关于== 与equals的面试题常常会围绕着堆栈进行,例如调用intern()方法将字符串放入常量池与不调用是的结果是不一样的;



equals 方法和 hashCode 它们经常被一起重写,用于判断值是否一样,常常用于集合的开发;

- 如果两个对象的 equals 相等，那么 hashCode 必须相同
- 如果两个对象 equals 不相等，那么 hashCode 也有可能相同，所以需要重写 hashCode 方法，因为你不知道 hashCode 的底层构造（反正我是不知道，有大牛可以传授传授），所以你需要重写 hashCode 方法，来为不同的对象生成不同的 hashCode 值，这样能够提高不同对象的访问速度。
- hashCode 通常是将地址转换为整数来实现的。

## 4,简述final修饰符

> 在日常中我们用final的机会很少,但是这并不代表不重要;

- final修饰的类~

> 不能被继承(没有子类);final类中的方法默认是final的。  

- final修饰的方法~

> 该方法不能被重写,但可以被重载

- final修饰的变量~

> 声明时必须初始化;
>
> 类中的基本数据类型其值不能被修改,
>
> 类中的引用类型的数据,只要地址不变即可;例如 final 修饰的集合List  可以改集合里的数据;

局部内部类和匿名内部类中的变量为final~原因就是当外部类被当作来及回收时,内部类的变量被其他类引用,那么这部得分数据就不应该被回收;

## 5,String,StringBuffer与StringBuilder区别

String 是由final修饰的~不可变字符串(这里的不可变并不是值不可变,我们在每一次赋值时,String 都会重新指向一个新的地址)

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence 
```

StringBuilder 位于 `java.util` 包下，StringBuilder 是一非线程安全的容器，StringBuilder 的 append 方法常用于字符串拼接，它的拼接效率要比 String 中 `+` 号的拼接效率高。StringBuilder 一般不用于并发环境;

StringBuffer 位于 `java.util` 包下，StringBuffer 是一个线程安全的容器，**多线程场景下**一般使用 **StringBuffer** 用作字符串的拼接;

StringBuilder 和 StringBuffer 都是继承于AbstractStringBuilder 类，AbstractStringBuilder 类实现了 StringBuffer 和 StringBuilder 的常规操作。

> StringBuffer与StringBuilder都是在原来对象的基础上进行操作,StringBuilder出现的比较晚,性能好,但是线程不安全,StringBuffer出现的早,线程安全,想能比StringBuilder低;



## 6,重载与重写的区别

重载~在同一个类中,方法名一样,参数类型,顺序,个数不一样,方法的返回值和修饰符可以不同;

重写~在父子类中,方法名参数列表和父类相同,返回值范围小于等于父类,修饰符的权限不能小于父类(父类private/final修饰的方法不能被子类重写)



## 7,接口和抽象类区别

接口~定义标准,约束实现类的功能(必须将接口中的方法实现了)

抽象类~代码复用

抽象类中可以存在普通成员函数,(即带有抽象方法的类才能成为抽象类)

```java
public abstract class CodeM {
    public void show(){//方法体
         };
    public abstract void show(String str);
    protected  void show(Integer i){}
}
```

接口中也可以存在default修饰的成员函数(jdk1.8以后),其中的属性为静态共享属性final static 修饰

```java
interface jiekou{
//     可被重写也可以不被重写
     default void fun(String name){
         System.out.println(name);
     }
//     必须被子类重写，如果子类是抽象类则可以不重写，也可以重写
     public abstract void fun(int i);
    static final String name="张三";//
     }

public abstract class CodeM implements jiekou{
    public void show(){//方法体
         };
/*
    @Override
    public void fun(int i) {    
    }*/
@Override
public void fun(String name){  
}
    public abstract void show(String str);
    protected  void show(Integer i){}
}
```

## 8,list与set

- list 

存储的数据有序--按照顺序保存对象;

元素可重复,允许多个null值;

可以提供下标遍历数据也可以提供迭代器迭代;



- set

存储的数据无需--并非按照顺序保存对象;

元素不可重复,最多只有一个null;

只能通过迭代器遍历;



## 9,hashcode与equals

- 如果在 Java 运行期间对同一个对象调用 hashCode 方法后，无论调用多少次，都应该返回相同的 hashCode，但是在不同的 Java 程序中，执行 hashCode 方法返回的值可能不一致。
- 如果两个对象的 **equals 相等**，那么 **hashCode 必须相同**
- 如果两个对象 **equals 不相等**，那么 hashCode 也有可能相同，所以需要重写 hashCode 方法，来为不同的对象生成不同的 hashCode 值，这样能够提高不同对象的访问速度。
- hashCode 通常是将地址转换为整数来实现的。



## 10,ArrayList与LinkedList

- ArrayList

底层是动态数组实现的,是连续内存存储;

可通过下标访问,适合访问频繁,修改频率小的存储容器;



扩容机制:

旧数组的拷贝,如果是插入/删除数据还需要涉及到元素的位置移动;

> 从JDK1.8开始 默认的创建的底层数组大小为空,当我们添加数据的时候才开始创建一个大小为10 的空间,如果空间不够的话,扩容,然后旧数组的内容拷贝到新数组; 

> 设置初始大小+尾插法~提高性能;new ArrayList(100)

[arraylist底层源码实现](https://blog.csdn.net/weixin_54061333/article/details/118729770)

- LinkedList

LinkedList底层是使用Node实现，相比于ArrayList单向链表较耗费空间。
LinkedList插入，删除节点只需要修改要删除前后的地址将他们连起来即可,效率更高。
LinkedList查找元素时需要从头使用遍历，效率一般。 LinkedList同时是双向队列的实现。

> 底层是链表,可以将数据分散的存储到内存中/不适合查询频繁的操作,适合做数据的插入删除频繁的数据存储容器;

[linkedlist底层实现源码](https://blog.csdn.net/weixin_54061333/article/details/118857612)

## 11,聊一下HashMap

- hashmap的底层实现

**底层是数组+链表**

数组中存放的的entry<key,value>

通过key算出一个hash值(indexof),

然后通过hash值与数据长度取余操作算出一个存放数据的位置的

由于算出的hash值可能会重复,所以加上一个链表来存放hash值相同的数据;



hashmap的put方法:

new hashMap()一开始是一个空数组,在调用push的时候会检查数组是否为空,如果为空,就会初始化数组,这个跟list类似;然后检查key是否为null,不为null则根据key算出一个值,这个值跟数组的长度 做位与操作;



```java
import java.util.Map;

import javax.sound.sampled.SourceDataLine;

import java.util.HashMap;

public class one {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("qwer", "1");
        String str = map.put("qwer", "2");//put方法插入的 key重复时,返回的是这个key之前对用的旧的value,同时会将新的value存储到对应位置; ~~返回旧的值1
        System.out.println(str);
        String result = map.get("qwer");//map中存储的是新值  2
        System.out.println(result);
    }
}
/**Associates the specified value with the specified key in this map (optional operation).  If the map previously contained a mapping for the key, the old value is replaced by the specified value.*/
//--------部分源码------//
 if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
```

put 方法的大致逻辑--->

1,判断数组是否为null;

2,判断key是否为null;

3,根据key数组长度算出下标;

4,在该下标处插入数据(首位)~只需要将该节点指向源下标处首元素;

5,其他数据下移~节点移动;

伪代码~

int index=key.hashcode&table.length-1 ;~根据key算出一个hash值,然后跟数组的长度位与操作得到一个index,

 tab[i] = newEntry(hash, key, value, tab[i]); ~刚插入的数据的下一个节点指向该index 所在的数组的头节点,然后所有节点下移,这样插入时的效率是比较高的;

数组的大小~

默认初始容量一定是2的4次幂~16



```java
/**
     * Returns a power of two size for the given target capacity.
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```

为什么数组的大小一定要是2的幂次方?

因为hashmap在存储数据时会根据key的值算出一个hashcode,然后跟数组的长度减一 做位与操作

这时候由于数组的长度已经确定,此时我们要考虑下标不能越界,

经过计算

任何key 在计算hashcode时与2的幂次方做位与运算都能保证数组小标不越界,但是这会产生一个问题~空间利用会比较糟糕,大量数据会存放于hashmap的首尾,其他位置就不存放数据了吗?



例如:

| key.hashcode=随机 | table.length  |       &        |
| :---------------: | :-----------: | :------------: |
|     1001 0001     | 0001 0000(16) | 0001 0000 (16) |
|     1111 0001     | 0001 0000(16) | 0001 0000(16)  |
|     1111 1111     | 0001 0001(17) | 0001  0001(17) |
|     0000 1000     | 0001 0001(17) |  0000 0000(0)  |
|     0101 0011     | 0000 1111(15) | 0000  0011(3)  |
|     0110 1100     | 0000 1111(15) | 0000 1100(12)  |



考虑到空间利用率,在key计算hashcode之后与2的幂次方减一的空间利用率比2的幂次方要好的多;

> 所以我们数组的大小为2的幂次方,位与时用数组长度减一

[hashmap扩容机制~](https://blog.csdn.net/weixin_54061333/article/details/118998336)

**hashmap初始化过程**

1,当新建一个hashmap时,如果没有被分配即指定初始容量,

```xml
if the table array has not been allocated(分配), this
    field holds the initial array capacity, or zero signifying DEFAULT_INITIAL_CAPACITY.)
```

在第一次插入数据时才开始将容量初始化,当没有指定容量和加载因子时~

```xml
<!--属性描述-->
constructs an empty {@code HashMap} with the default initial capacity
* (16) and the default load factor (0.75)
    
```

> 源码~当Node<K,V>[] tab长度为0或者为null(只有第一次为null)时,初始化tab

```java
if ((tab = table) == null || (n = tab.length) == 0)
    n = (tab = resize()).length;
```

接下来是插入数据;



> 当put后的容量达到12时,可能会扩容,但是不一定;

比如下面这种情况:当某个下标处所在数组为null时,虽然已经够了12个 元素,但是也不会扩容;

![在这里插入图片描述](https://img-blog.csdnimg.cn/9422dd7d28e34bcfacea2a3eb0c7f273.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```c
#属性描述
the next size value at which to resize (capacity * load factor).
```

源代码~

```java
 if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
```

> 扩容后是原来长度的两倍;(oldThr << 1)



如果声明hashmap时指定了初始容量

源码~

```java

static final int tableSizeFor(int cap) {
        int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);//-1 无符号右移
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```

> 返回给定目标容量的2次幂大小. 

即分别指定一下容量时

| 0    | 1    |
| ---- | ---- |
| 1    | 1    |
| 2    | 2    |
| 3    | 4    |
| 4    | 4    |
| 10   | 16   |
| 30   | 32   |
| 40   | 64   |
| 100  | 128  |

源码~

```java
static final int MAXIMUM_CAPACITY = 1 << 30;
static final int tableSizeFor(int cap) {
    int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```



扩容:

根据key算的hash值是不变的,这时候将key的hash值取出来跟新的数组的长度-1做位与操作,然后根据算出来的下标将元素放进去;

容量为16的hashmap,在扩容过后~

计算出来的下标与原来的下标的规律~要么与原来的一样,要么是原来的下标+扩容的容量(移动新表中使用2的幂次偏移量 );如果旧的是1,则新下标可能位1或者17

那么什么时候会重新计算一下新的下标呢?

源码~

```java
if ((e = oldTab[j]) != null) {
    oldTab[j] = null;
    if (e.next == null)
        newTab[e.hash & (newCap - 1)] = e;
```

当oldTab[j]指向的下一个为null的时候,即当所有数组都被填满时,会重新计算hash,然后作循环进行元素移动,

```java
do {
    next = e.next;
    if ((e.hash & oldCap) == 0) {
        if (loTail == null)
            loHead = e;
        else
            loTail.next = e;
        loTail = e;
    }
    else {
        if (hiTail == null)
            hiHead = e;
        else
            hiTail.next = e;
        hiTail = e;
    }
} while ((e = next) != null);
```

(e.hash & oldCap) == 0时,将 loHead 和 loTail将 指向这个节点e。如果后面还有节点 hash & oldCap 为0的话，则将节点链入 loHead 指向的链表e中，并将 loTail 指向该节点。

如果值为非0的话，则让 hiHead 和 hiTail 指向该节点。完成遍历后，可能会得到两条链表，此时就完成了链表分组：

最后再将这两条链接存放到相应的位置中，完成扩容。如下图：

 