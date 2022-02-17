## JAVA中的集合

集合中可以存放任意类型的数据吗?

今天我来试一下--

```java
class Person{
    String name;
    int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }

}

public class Test {
    public static void main(String args[]) {

        Collection col = new ArrayList();
        col.add(3);
        List list = new ArrayList<Integer>();
        List list1= Arrays.asList(new Integer[]{12,12,122,2133});
        list.add(1236789);//集合中可以存放任意类型的数据.如果不指定类型

        col.add(list);
        col.addAll(list1);
        Person person = new Person("张三",18);
        col.add(person);
        System.out.println(col.toString());

    }
}
```

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
[3, [1236789], 12, 12, 122, 2133, baozhuang.Person@4f023edb]
```

可以看到确实可以存放任意类型数据,前提是没有限定数的存储类型--如果指定了数据类型----如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714132607920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
就会报错;

其他常用方法---

```java
class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

}

public class Test {
    public static void main(String args[]) {

        Collection col = new ArrayList();
        col.add(3);
        List list = new ArrayList<Integer>();
        List list1 = Arrays.asList(new Integer[]{12, 12, 122, 2133});
        list.add(1236789);//集合中可以存放任意类型的数据.如果不指定类型

        col.add(list);
        col.addAll(list1);
        Person person = new Person("张三", 18);
        col.add(person);
        System.out.println(col.toString());
        col.remove(person);//移除person对象
        System.out.println(col.toString());
        col.clear();//清除所有元素
        int size = col.size();//集合大小
        boolean empty = col.isEmpty();//判断按集合是否为空
        System.out.println(empty);
        System.out.println(size);
        System.out.println(col.toString());


        Collection col1= new ArrayList();
        Collection col2= new ArrayList();
        col1.add(new Integer(1));
        col2.add(1);
        boolean aaa=col1.cotains(1);
        boolean equals = col1.equals(col2);
        System.out.println(equals);
        System.out.println(col1==col2);
        System.out.println(aaa);
    }
}
```

```java
class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

}

public class Test {
    public static void main(String args[]) {

       Collection col= new ArrayList();
       col.add(new Person("二狗子",18));
       col.add(1);
       col.add('A');
       col.add("Hello");
//遍历方式1----
        for (Object o:col
             ) {
            System.out.println(o);

        }
        //遍历方式二
        Iterator iterator = col.iterator();
while (iterator.hasNext()){
    System.out.println(iterator.next()+"\t");
}
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714140151745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
 Iterator 迭代的方式----通过移动指针的方式如果有下一位元素,则继续迭代,否则 退出该循环--原理图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714140442901.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

**为啥父类和子类都要实现List<E>接口?**

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
```

```java
public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E> {
    /**
     * Sole constructor.  (For invocation by subclass constructors, typically
     * implicit.)
     */
    protected AbstractList() {
    }

```

这样的好处是什么?
没必要吧!!!!!!历史原因而已.

### ArryList

```java
public class Test {
    public static void main(String args[]) {
      //集合的开辟方式
        Collection aaaa= new ArrayList();//接口=实现类
        List list1= new ArrayList();//接口=实现类
        ArrayList arrayList= new ArrayList();//实现类=实现类
    }
}
```

最好是用接口=实现类来开辟,因为容易维护,修改数据结构也方便

```java
public class Test {
    public static void main(String args[]) {
      //集合的开辟方式
        Collection aaaa= new ArrayList();//接口=实现类
        List list1= new ArrayList();//接口=实现类
        ArrayList arrayList= new ArrayList();//实现类=实现类
         aaaa= new LinkedList();
    }
}
```

下面看一下ArrayList的实现源码
继承和实现类如下:

> public class ArrayList<E> extends AbstractList<E>
>         implements List<E>, RandomAccess, Cloneable, java.io.Serializable

里面有几个重要的参数---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714160201120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714160248333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70) 
底层使用Object 数组来接收数据--

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714160944962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)从JDK1.8开始    默认的创建的底层数组大小为空,当我们添加数据的时候才开始创建一个大小为10 的空间,如果空间不够的话,扩容,然后旧数组的内容拷贝到新数组;

### Arraylist与Vectord的区别

> 实际上随着JDK版本的不断更新,性能上会有所改进,于是乎一些老的接口或者方法会逐渐被抛弃,但并不是不能使用,知识不在提倡使用;
> 今天我们一起来看一下ArrayList与Vector 的区别

按照惯例的话,先来后到,先看Vector(JDK15)

Vector继承了实现了AbstractList,实现了List, RandomAccess, Cloneable, java.io.Serializable

```java
public class Test {
    public static void main(String args[]) {

        Vector v= new Vector();
        v.add(123);
        boolean add = v.add("abc");


    }
}
```

> Vector中的属性 
>  protected Object[] elementData;  //说明底层也是Object数组
>  protected int elementCount;   
> protected int capacityIncrement;



调用构造方法---

```java
 public Vector() {
        this(10);
    }
```

无参构造去调用了有参构造,传入参数为10

```java
 public Vector(int initialCapacity) {
        this(initialCapacity, 0);
    }

```

初始化为容量为10的数组,增长幅度为0
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714170550346.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



接着往里面添加元素

```java
 public synchronized boolean add(E e) {
        modCount++;
        add(e, elementData, elementCount);
        return true;
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714171701411.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)
            elementData = grow();
        elementData[s] = e;
        elementCount = s + 1;
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714171923715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
一开始s(即初始的elementCount)小于数组的长度10的,所以elementData[1]=123;
然后 elementCount = s + 1;
随着元素的添加,当s == elementData.length,时就要开始扩容了;

```java
 private Object[] grow() {
        return grow(elementCount + 1);
    }

```

```java
private Object[] grow(int minCapacity) {
        int oldCapacity = elementData.length;
        int newCapacity = ArraysSupport.newLength(oldCapacity,
                minCapacity - oldCapacity, /* minimum growth */
                capacityIncrement > 0 ? capacityIncrement : oldCapacity
                                           /* preferred growth */);
        return elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714172735520.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)这里也可以看出Vector扩容后的容量是原来的两倍

```java
public static int newLength(int oldLength, int minGrowth, int prefGrowth) {
        // assert oldLength >= 0
        // assert minGrowth > 0

        int newLength = Math.max(minGrowth, prefGrowth) + oldLength;
        if (newLength - MAX_ARRAY_LENGTH <= 0) {
            return newLength;
        }
        return hugeLength(oldLength, minGrowth);
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714173345473.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)一般是走不到return Integer.MAX_VALUE;这一步的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714173445687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)所以Vector的底层依旧是Object数组,但是Vector时线程安全的;

再来看ArrayList-----(JDK15)

```java
public class Test {
    public static void main(String args[]) {

        Vector v= new Vector();
        v.add("abc");
        boolean add = v.add(123);
ArrayList list= new ArrayList();
list.add("abc");

    }
}
```

先看ArrayList中的属性---(JDK15)

> ***private static final int DEFAULT_CAPACITY = 10;*** 

> ***private static final Object[] EMPTY_ELEMENTDATA = {};***  

> ***private static final Object[]DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};***   

> ***transient Object[] elementData;***

> ***private int size;***

调用无参构造---

```java
public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
```

这里默认时开辟空间大小的0 的一个Object数组,

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071420294946.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70#pic_center)
注释表明在第一次添加元素后才确定数组空间大小

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210714203214504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)具体源码--请看JDK1.8之前的版本
这里只说一下   底层也是Object数组,默认开辟大小为16的空间然后如果空间不够,也是要扩容的;

添加元素--
public boolean add(E e) {
        modCount++;
        add(e, elementData, size);
        return true;
    }

方法跟Vector一样,不在赘述;



> ***Arraylist与Vector的区别---*** 

> 1,线程安全---Vector,效率比Arraylist低;
> 2,Vector数据增长为100%,而ArrayList为50%
> 3,如果涉及到查询\修改等的效率,不考虑安全性问题,那么优先考虑ArrayList,如果涉及到堆栈操作,考虑Vector





### Linkedlist

```java
package baozhuang;

import java.util.Iterator;
import java.util.LinkedList;
import java.util.ListIterator;

/**
 * @author : Gavin
 * @date: 2021/7/15 - 07 - 15 - 19:40
 * @Description: baozhuang
 * @version: 1.0
 */
public class Today {
    public static void main(String[] args) {
        LinkedList <String>list = new LinkedList();
        list.add("aaaa");
        list.add("bbbb");
        list.add("cccc");
        list.add("dddd");
        list.add("eeee");
        
        list.add(2,"qwer");
        list.addFirst("1111");
        list.addLast("2222");
        list.push("hahaha");//压栈
        String peek = list.peek();
        System.out.println(peek);//返回第一个元素
        /*public E peek() {
            final LinkedList.Node<E> f = first;
            return (f == null) ? null : f.item;
        }
        */

        System.out.println(list.element());//返回第一个元素,如果没有则报错Exception in thread "main" java.util.NoSuchElementException
       /* public E element() {
            return getFirst();
        }*/
        System.out.println(list.indexOf("aaaa"));//查找元素
        System.out.println(list.offer("tttt"));//在末尾插入元素--返回是否插入成功
        System.out.println(list.pollFirst());//pollFirst()与poll()方法内容一样
        list.poll();
      /*  public E pollFirst() {
            final LinkedList.Node<E> f = first;
            return (f == null) ? null : unlinkFirst(f);
        }*/

      /*  public E poll() {
            final LinkedList.Node<E> f = first;
            return (f == null) ? null : unlinkFirst(f);
        }*/
//        -----unlinkFirst(f)----
//        private E unlinkFirst(Node<E> f) {
//            // assert f == first && f != null;
//            final E element = f.item;
//            final LinkedList.Node<E> next = f.next;
//            f.item = null;
//            f.next = null; // help GC//删除第一个元素
//            first = next;
//            if (next == null)
//                last = null;
//            else
//                next.prev = null;
//            size--;
//            modCount++;
//            return element;//返回第一个元素
//        }
//      
        //ListIterator<String> stringListIterator = list.listIterator(3);//从指定位置开始返回一个

        System.out.println("------------------------------------------");
        for (int i =0;i<list.size();i++){
            System.out.println(list.get(i));
        }
        System.out.println("------------------------------------------");
        for(String s:list){
            System.out.println(s);
        }
        System.out.println("------------------------------------------");
        Iterator<String> iterator = list.iterator();
        while(iterator.hasNext())
            System.out.println(iterator.next());
        System.out.println("------------------------------------------");
//        for(初始化体;条件;)
//        这种方式节省内存
        for(Iterator<String> it = list.iterator();it.hasNext();)
            System.out.println(it.next());

        
        
    }
}

```



想了想还是取一部分代码给大家看看吧---
关于迭代器的,

```java
  Iterator<String> iterator = list.iterator();
        System.out.println("做一个标记"+iterator.next());

        while(iterator.hasNext())
            System.out.println(iterator.next());
        System.out.println("------------------------------------------");
//        for(初始化体;条件;)
//        这种方式节省内存



        for(Iterator<String> it = list.iterator();it.hasNext();)
            System.out.println(it.next());

```

   上段代码中第二种迭代方式的巧妙之处在于运行完 it对象就被回收了,整个他的生命周期就结束了,节省空间;

强行解释---
for循环是编程语言中一种循环语句，而 循环语句 由 循环体 及循环的判定 条件 两部分组成，其表达式为：for（单次表达式;条件表达式;末尾循环体）

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071520274782.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
如果it.next()能输出,则会继续循环;

next()注解----

```java
   /**
     * Returns the next element in the iteration.
     *
     * @return the next element in the iteration
     * @throws NoSuchElementException if the iteration has no more elements
     */
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071616101640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)接下来如果要像链表中存入数据,那么就要将我们要存放的数据包装然后放入相应的位置;
第一步--设计一个节点类,用于存放数据
节点类中包含的属性---
上一个节点---Node before;
下一个节点--Node after;
计数器 int count;

> 为了安全,封装一下----设计类的时候都要考虑封装

```java
/**
 * @author : Gavin
 * @date: 2021/7/16 - 07 - 16 - 15:34
 * @Description: LinkedTest
 * @version: 1.0
 */
public class Node {

    private  Node before;//前一个节点
    private  Object obj;
    private  Node after;//后一个节点
    public Node(Node before,Object obj, Node after) {
this.before=before;
this.obj=obj;
this.after=after;
    }
    public Node getBefore() {
        return before;
    }

    public void setBefore(Node before) {
        this.before = before;
    }

    public Object getObj() {
        return obj;
    }

    public void setObj(Object obj) {
        this.obj = obj;
    }

    public Node getAfter() {
        return after;
    }

    public void setAfter(Node after) {
        this.after = after;
    }
}
```

节点类设计完毕,下面做一个测试类;
***框架如下---***

```java
class Mylinked {
public void add(Object obj){}
}

public class Test {
    public static void main(String[] args) {
        Mylinked link = new Mylinked();
        link.add("aaa");
        link.add("bbb");
        link.add("ccc");
       
    }


}
```

以上我们要添加数据,由于没有做类型限制,所以用Object接收;
添加数据之前我们要知道前节点和后节点,
所以在Mylinked类中添加首尾节点,同时还要初始化计数器,
***完善代码---***

```java
class Mylinked {
    Node first;
    Node last;
    int count;
public void add(Object obj){
    
}
}

public class Test {
    public static void main(String[] args) {
        Mylinked link = new Mylinked();
        link.add("aaa");
        link.add("bbb");
        link.add("ccc");
     
       
    }


}
```

接下来完善add方法

如果链表中没有元素,那么first指向为null,根据这一点来逐渐完善代码---

```java
class Mylinked {
    Node first;
    Node last;
    int count;

    public void add(Object obj) {
        Node node = new Node();
        if (first == null) {//如果添加的为第一个元素,那么first==null
            node.setBefore(null);//没有前一个节点.指向空
            node.setObj(obj);
            node.setAfter(null);//没有后一个节点.指向空
            //添加完之后就有了首尾节点,给首位节点赋值
            last = node;//尾节点
            first=node;//下一个元素的首节点--做好记录

        } else {//如果添加的不是第一个元素,
            Node node1 = new Node();
            node1.setBefore(last);//设置该元素的首节点为上一个元素的尾节点
            node1.setObj(obj);//将obj存储
            node1.setAfter(null);//尾节点指向null
            //修改首节点为新添加的元素地址;
            last.setAfter(node1);//设置下一个节点为node1
            last=node1;//赋值

        }
        count++;

    }
    public int size(){//返回链表的数据元素个数
        return count;
    }
    public Object getIndex(int index){

        Node node= first;//找到第一个节点,然后顺藤摸瓜
        
        for(int i=0;i<index;i++){
             node=node.getAfter();
        }
        return node.getObj();
    }
}

public class Test {
    public static void main(String[] args) {
        Mylinked link = new Mylinked();
        link.add("aaa");
        link.add("bbb");
        link.add("ccc");

        System.out.println(link.size());
        System.out.println(link.toString());

        for (int i=0;i<link.size();i++)
        System.out.println(link.getIndex(i));

    }


}
```

接下来做断点测试,验证一下add方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716165515971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

下面的工作就是去看看源码怎么写的,毕竟他们才是大神;

下面是源码分析---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210716173859452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70#pic_center)



总结---
0,直接将类私有化,免去了包装属性;
1,源码中添加数据直接调用linklast,即先将加进来的数据作为尾节点,然后再去判断是否是第一个,
2,用final去定义数据,保证了添加数据的安全性---所以线程上是安全;
3.所以双向链表比较绕的部分是首尾节点的指向。
4.源码用泛型的好处是取用的时候方便，不用再次转型了！

> 先做一下自我反思--为什么昨天没太理解(实际上理解有偏差),看了源码之后我好像懂了,一看就会,一做就废,果然是学废了;李姐万岁;

链表的头尾原来是链表的头尾-----听起来有些绕
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717154455617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



```java
package today;


public class Node<E>{
    Node before;
     E e;
    Node after;

    public  Node(){};//无参构造
    public  Node(E e){
        this();
        this.e=e;
    };
    public Node(Node before ,E e,Node after) {
        this(e);
        this.before=before;
        this.after=after;
    }
    //Setting,getter方法
    public Node getBefore() {
        return before;
    }

    public void setBefore(Node before) {
        this.before = before;
    }

    public E getE() {
        return e;
    }

    public void setE(E e) {
        this.e = e;
    }

    public Node getAfter() {
        return after;
    }

    public void setAfter(Node after) {
        this.after = after;
    }


    public String toString() {
        return "Node{" +
                "before=" + before +
                ", e=" + e +
                ", after=" + after +
                '}';
    }

}
```

***链表的属性中最重要的是Node,然后将要添加的数据封装;***



```java
package today;

/**
 * @author : Gavin
 * @date: 2021/7/17 - 07 - 17 - 8:16
 * @Description: today
 * @version: 1.0
 */

class myLink<E>{
    Node first;
    Node last;
    int count;

    public boolean addFirst(E e){//添加元素使其成为第一个,
        Node<E> node= first;//先找到第一个元素
        Node<E> newNode=new Node<E>(null,e,first);//添加的元素指向原来的第一个节点
        if(node==null){//如果首节点为null,那说明原链表为空
            first=newNode; //新添加的为第一个节点
        }else{//否则说明链表不为空,那么原来首节点的前一个就应该是新添加的
            first.before=newNode;
        }

        first=newNode;//新添加的节点指向添加之前的(即现在的第二个)
        count++;//计数器自增
        return true;

    }
    public Boolean addLast(E e){//在末尾添加元素
        Node<E> node = last;//找到最后一个元素
        Node<E> newNode =new Node<E>(last,e,null);//添加的元素指向原来最后的节点
        last=newNode;//原来尾节点指向新添加的节点
        if (node == null)
           first = newNode;
        else
            node.after = newNode;
        count++;
        return true;
    }



    public Boolean add(E e){
        addLast(e);
        return true;
    }
    public int size(){
        return count;
    }
    public Object getIndex(int index){
        Node node=first;//遍历要从第一个开始,顺藤摸瓜
        for(int i=0;i<index;i++){
            node=node.after;
        }

        return node.e;
    }


}
```

> //方法中最重要的是首尾节点的指向问题,这里处理好了就可以了,添加完元素之后就要修改头尾节点的指向

```java
public class Test {
    public static void main(String[] args) {
        myLink<Integer> node = new myLink<>();
        node.add(111);
        node.add(222);
        node.add(333);
        System.out.println(node.size());
        for(int i=0;i<node.size();i++){
            System.out.println(node.getIndex(i));
        }


    }
}
```

> LinkedList底层是使用Node实现，相比于ArrayList单向链表较耗费空间。
> LinkedList插入，删除节点只需要修改要删除前后的地址将他们连起来即可,效率更高。
> LinkedList查找元素时需要从头使用遍历，效率一般。 LinkedList同时是双向队列的实现。



LinkedList,Iterator与Iterable 的联系与区别;
![](C:\Users\Gavin\Pictures\Camera Roll\1.png)
代码分析----

![](C:\Users\Gavin\Pictures\Camera Roll\2.png)

### Iterator与ListIterator迭代的区别

迭代的时候可以修改数据吗?
答,Iterator迭代的时候可以移除数据,但是不能添加;而ListIterator迭代时可以添加数据,移除数据,倒序遍历;

```java
public class Bianli {
    public static void main(String[] args) {
        ArrayList<String> list= new ArrayList<>();
        list.add("aaa");
        list.add("sss");
        list.add("ddd");
        list.add("fff");
        list.add("ggg");
        Iterator<String> iterator = list.iterator();

        while(iterator.hasNext()){
            if(iterator.next().equals("ddd")){
                list.add("eee");//不可以添加,
                iterator.remove();//但是可以移除

            }

        }
        for (String s:list
             ) {
            System.out.println(s);
        }
        }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717214839929.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)这个迭代器只有remove方法,无add方法,要想添加数据得靠list对象,但是这时候如果通过list添加数据,就相当于有两个人同时操作一个数据,会产生
Exception in thread "main" java.util.ConcurrentModificationException

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717215049385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)而ListIterator迭代器提供了add方法,可以通过迭代器进行添加数据,而不用通过集合对象添加;

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717215421136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
package today;

import java.util.*;

/**
 * @author : Gavin
 * @date: 2021/7/17 - 07 - 17 - 21:23
 * @Description: today
 * @version: 1.0
 */
public class Bianli {
    public static void main(String[] args) {
        ArrayList<String> list= new ArrayList<>();
        list.add("aaa");
        list.add("sss");
        list.add("ddd");
        list.add("fff");
        list.add("ggg");
        Iterator<String> iterator = list.iterator();

        while(iterator.hasNext()){
            if(iterator.next().equals("ddd")){
                //list.add("eee");//不可以添加,
                iterator.remove();//但是可以移除

            }

        }
        for (String s:list
             ) {
            System.out.println(s);
        }
        List <String>list1 = new ArrayList<>();
        list1.add("111");
        list1.add("222");
        list1.add("444");
        list1.add("666");
        list1.add("888");
        ListIterator<String> iterator1 = list1.listIterator();
        while(iterator1.hasNext()){
            if("444".equals(iterator1.next())){
                iterator1.remove();
                iterator1.add("333");

            }
        }
        for (String str : list1) {
            System.out.println(str);
        }
    }
}

```

![](C:\Users\Gavin\Pictures\Camera Roll\3.png)

我们还可以倒叙迭代,通过listiterator的hasPrevious()方法--因为当我们正序迭代完之后,光标指针已经到了尾部了;

```java
System.out.println(iterator1.hasNext());
       System.out.println(iterator1.hasPrevious());
        while(iterator1.hasPrevious()){
            System.out.println(iterator1.previous());
        }
    }
```

### HashSet

```java
import java.util.HashSet;
import java.util.Set;
/**
 * @author : Gavin
 * @date: 2021/7/18 - 07 - 18 - 9:13
 * @Description: setTest
 * @version: 1.0
 */
class Person{
    String name;
    int age;
    String gender;

    public Person(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Person)) return false;

        Person person = (Person) o;

        if (age != person.age) return false;
        if (!name.equals(person.name)) return false;
        return gender.equals(person.gender);
    }

    @Override
    public int hashCode() {
        int result = name.hashCode();
        result = 31 * result + age;
        result = 31 * result + gender.hashCode();
        return result;
    }
}

public class setHash {
    public static void main(String[] args) {
        Set<Person> hashSet= new HashSet<>();
        Person per= new Person("李一狗",18,"男");
        Person per1= new Person("李二狗",19,"男");
        Person per2= new Person("李三狗",20,"男");
        Person per3= new Person("李四狗",21,"男");
        Person per4= new Person("李一狗",18,"男");
        Person per5= new Person("李六狗",23,"男");
        hashSet.add(per);
        hashSet.add(per1);
        hashSet.add(per2);
        hashSet.add(per3);
        hashSet.add(per4);
        hashSet.add(per5);
       
//        遍历
        for (Person s:hashSet
             ) {
            System.out.println(s);
        }

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718115529107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

HasSet无序,且不允许添加重复元素

![1642863278398](C:\Users\Gavin\AppData\Local\Temp\1642863278398.png)
接下来分析一下HashSet的底层是怎么实现的,使用的最新版本jdk16(我有个习惯喜欢追求最新的)

创建对象---

```java
Set<Person> hashSet= new HashSet<>();
```

构造方法---

```java
public HashSet() {   map = new HashMap<>();   }
```

发现HashSet构造中创建了一个HashMap对象

```java
 public HashMap() {//无参数的构造函数，加载因子为默认加载因子0.75
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }
```

> 在HashMap中有几个重要参数-- static final float DEFAULT_LOAD_FACTOR = 0.75f;
>
> 学习HashSet就不得不提HashMap
>
> HashMap几个关键的属性-- 
>
> 存储数据的数组   
>
> transient Node<K,V>[] table;
>
> 默认容量 ---- 
>
> static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; 
>
> 最大容量 static final int MAXIMUM_CAPACITY = 1 << 30;
>
> 加载因子是一个比例，当HashMap的数据大小>=容量*加载因子时，HashMap会将容量扩容 
>
> 默认加载因子----
>
> static final float DEFAULT_LOAD_FACTOR = 0.75f;
>
> 当实际数据大小超过threshold--阈值时(如当实际数据大小超过16*0.75=12时会扩容,重新计算存储位置)，HashMap会将容量扩容，
>
> threshold＝容量*加载因子 int threshold; 
>
> 加载因子 ---
>
> final float loadFactor;
>
> 

再来看add()方法---add(per)

```java
public boolean add(E e) {//添加Person对象per
        return map.put(e, PRESENT)==null;
    }

```

这里继续看map,present与put分别代表什么----

```java
//present为Object对象
private static final Object PRESENT = new Object();
```

```java
private transient HashMap<E,Object> map;
```

这里是创建的HashMap对象map,调用put方法

```java
public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
```

> return map.put(per, PRESENT)==null; 这里put(per, PRESENT)返回的一个present对应类型的数据--这里是Object类型的,如果返回值为null,则add返回true说明添加成功,否则返回false.添加失败;



再来看---put方法中调用的putVal方法(看起来比较复杂)
传入的参数---
int类型的hash(per)--哈希值,
Person类型的per,
Object类型的 Present
boolean类型的 false
boolean类型的true

> HashMap中的重要属性 ----
> transient Node<K,V>[] table; 
> final int hash;
>         final K key;
>         V value;
>         
> 再看putVal方法之前先看一下resize()方法

```java
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;//将oldTab指向table
        //如果oldTab == null,那么为第一次添加oldCap=0,(如果不是第一次,那么oldCap=oldTab的长度)
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
       //----- int oldCap=0
       // field holds the initial array capacity, or zero signifying
        int oldThr = threshold;//threshold初始值为0--初始化集合
        int newCap, newThr = 0;
        if (oldCap > 0) {//-----若 int oldCap>0,说明不是第一次添加,oldCap=oldTab的长度
            if (oldCap >= MAXIMUM_CAPACITY) {
            // static final int MAXIMUM_CAPACITY = 1 << 30;--一个很大的数了2^31--(计算的时候位移的效率更高)
                threshold = Integer.MAX_VALUE;//也是一个很大的数了
                return oldTab;//返回oldTab
            }
            /**0<oldCap < MAXIMUM_CAPACITY左移一位相当于newCap的平方
            newCap^<2^31并且oldCap>=DEFAULT_INITIAL_CAPACITY(初始值为2^4=16)--*/
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                     //将旧的长度的平方赋值给新长度,
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;//默认16
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);//0.75*16
        }
        if (newThr == 0) {//如果长度为0,开辟新长度
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
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
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
```

resize主要是用来扩充大小用的,返回 一个新的newTab

```java
 final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab;//创建新对象tab 
        Node<K,V> p; //创建新对象p
        int n, i;//int 类型的 n,i
        if ((tab = table) == null || (n = tab.length) == 0)//如果tab为空或者tab的长度为0,那么重新为n赋值,
            n = (tab = resize()).length;//
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {//这里决定了不能添加重复元素
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718204611143.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70#pic_center)
做好笔记啊......
为什么是
“e.hash& (newCap-1)”这样做是为了提高HashMap的效率。

首先我们要确定一下，HashMap的默认长度为16,扩容的时候是乘以2^n,因此数组长度一定是偶数，在实际容量达到    容量*0.75即12时开始扩容，

```java
else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
```

小总结----

所以length-1位奇数，奇数\偶数有什么不同呢?

提高存储位置的均匀性,避免某位置存储过多,而其它位置过少的问题;

hashMap在每次插入数据前，会检查table数组的实际容量，如果实际容量>=初始容量，则把table的初始容量扩为原来的2倍，这时，就需要一个一个复制原来的数据项了，这是比较费时的！所以，初始容量很重要。
由于扩容后要重新计算存储位置,数据量小还可以,如果时千万级的数据,内存的开销---cpu的开销非常大,所以一开始最好是预估一下存储的数量;

在高并发情况下最好不使用HashMap,HashMap是线程不安全的，如果被多个线程共享的操作--后果可想而知------



搞完HashSet,今天来补充一下LinkedHashSet----0720

```java
public class Test {
    public static void main(String args[]) {
        Set<Integer> set=  new LinkedHashSet();
set.add(111);
set.add(333);
set.add(222);
set.add(999);
set.add(777);
        for (Integer integer : set) {
            System.out.println(integer);
        }

        Object[] objects = set.toArray();
        for (Object obj:objects
             ) {
            System.out.println(obj);
        }
        for (int i = 0; i <objects.length ; i++) {
            System.out.println(objects[i]);
        }
    }
}
```

今天突然发现其实集合其实都可以用fori循环进行遍历,只要   用toArray()方法然后用Object数组接收...不过如果要用数据的话得转型,麻烦;

你可能会想,为什么不换成对应的我需要的类?
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/333a406e5b14a704caa39de3635db5f3.png)

toArray()方法如下,

```java
 public Object[] toArray() {
        Object[] result = new Object[size];
        int i = 0;
        for (Node<E> x = first; x != null; x = x.next)
            result[i++] = x.item;
        return result;
    }
```

其实可以通过覆写的方式来实现我们想要达到的目的,但为题又来了,如果类很多...

所以不如用迭代的方式方便;

```java
import java.util.*;

/**
 * @author : Gavin
 * @date: 2021/7/19 - 07 - 19 - 18:45
 * @Description: setTest
 * @version: 1.0
 */

public class Test {
    public static void main(String args[]) {
        Set<Integer> set = new LinkedHashSet();
        set.add(111);
        set.add(222);
        set.add(113);
        set.add(333);
        set.add(114);
        System.out.print("LinkedHashSet---");
        for (Integer integer : set) {
            System.out.print(integer+"\t");
        }
/*
        Object[] objects = set.toArray();
        for (Object obj : objects
        ) {
            System.out.println(obj);
        }
        for (int i = 0; i < objects.length; i++) {
            System.out.println(objects[i]);
        }*/


        System.out.println();
        System.out.print("HashSet---");
        Set <Integer>set1 = new HashSet();
        set1.add(111);
        set1.add(222);

        set1.add(333);

        set1.add(113);
        set1.add(111);
        set1.add(114);

        for (Integer i:set1
             ) {
            System.out.print(i+"\t");

        }
        System.out.println();
        System.out.print("LinkedList---");
LinkedList<Integer> linkedList= new LinkedList<Integer>();

        linkedList.add(111);
        linkedList.add(222);
        linkedList.add(113);
        linkedList.add(333);
        linkedList.add(114);
        linkedList.add(111);
  Object[] objects3 = linkedList.toArray();
        for (Object obj:objects3
             ) {
            System.out.print(obj+"\t");
        }
        List<Integer> list = new ArrayList();//本身提供了(序号)fori的方式遍历
        list.add(111);
        list.add(222);
        list.add(113);
        list.add(333);
        list.add(114);
        list.add(111);
        System.out.println();
        System.out.print("ArrayList---");
        Object[] objects1 = list.toArray();
        for (int i = 0; i < list.size(); i++) {
            System.out.print(list.get(i)+"\t");
        }

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/e09eb3014db3c6d0bef61c99af0e9ffc.png)
**

> 总结--- 
> Arrayist---单向链表--底层实现是数组,连续的空间,查找效率高(索引)
> LinkedList--双向链表,底层实现是链表,不连续的空间,支持高效的插入和删除操作,查找效率低, 都允许插入重复数据,有序;
> HashSet--单向链表+数组实现,底层是map,通过Hashcode和equals来实现非重复和无序的特点,如果要求对象不重复，并且对存取的速度要求高的话，就可以使用HashSet。
> LinkedHashSet--双向链表+数组,因此也是 有序的,如果要保持添加的顺序，可以使用HashSet一个子类，LinkedHashSet。 
> Set还有一个重要的
> 实现类TreeSet，它可以排序,(compareto 和实现compareble两种方式);

### TreeSet

java中集合部分是比较常用的，也会企业要求必须精通的部分；
所以喽，作为小白的我只能看源码度日了；
话不多说，看东西；

```java
public class TodayTest {
    public static void main(String[] args) {
        TreeSet<Integer> set= new TreeSet<Integer>();
        set.add(111);
        set.add(100);
        set.add(101);
        for (Integer integer : set) {
            System.out.println(integer);
        }
    }

}
```

为什么TreeSet添加的数据会进行排序？

回顾---
***先看List接口----***
ArrayList 添加的数据是按照添加顺序排列的，可以添加重复数据；
底层是数组来实现的；
LinkedList添加数据也是按照添加顺序进行排列的，也可以添加重复数据；
底层是数组+链表即哈希表实现的；

ListIterator在迭代时候可以添加数据、移除数据
Iterator在迭代式只可以移除数据

***set接口----***
HasSet--添加的数据是无序的，且不能重复
底层是哈希表，且要实现hashcode和equals方法
LinkedHashSet --添加数据是按照添加顺序添加的，因为底层是数组+链表的形式（双向链表）

调用构造器--
 public TreeMap() {
        comparator = null;
    }
可以发现该构造器中--comparator = null;

> private final Comparator<? super K> comparator;

TreeSet中add方法的实现--

![](C:\Users\Gavin\Pictures\Camera Roll\4.png)



 红黑树----
设置树的颜色的;

```java
private void fixAfterInsertion(Entry<K,V> x) {
        x.color = RED;

        while (x != null && x != root && x.parent.color == RED) {
            if (parentOf(x) == leftOf(parentOf(parentOf(x)))) {
                Entry<K,V> y = rightOf(parentOf(parentOf(x)));
                if (colorOf(y) == RED) {
                    setColor(parentOf(x), BLACK);
                    setColor(y, BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    x = parentOf(parentOf(x));
                } else {
                    if (x == rightOf(parentOf(x))) {
                        x = parentOf(x);
                        rotateLeft(x);
                    }
                    setColor(parentOf(x), BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    rotateRight(parentOf(parentOf(x)));
                }
            } else {
                Entry<K,V> y = leftOf(parentOf(parentOf(x)));
                if (colorOf(y) == RED) {
                    setColor(parentOf(x), BLACK);
                    setColor(y, BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    x = parentOf(parentOf(x));
                } else {
                    if (x == leftOf(parentOf(x))) {
                        x = parentOf(x);
                        rotateRight(x);
                    }
                    setColor(parentOf(x), BLACK);
                    setColor(parentOf(parentOf(x)), RED);
                    rotateLeft(parentOf(parentOf(x)));
                }
            }
        }
        root.color = BLACK;
    }
```



刚开始学西二叉树,不知道分析的对不对,请大佬们指正...万分感激

> 今天（0722）再回来看一下put方法，添加的元素相同，则返回V----value的值，因为TreeSet只添加了一个元素（虽然是add方法，但是底层是用put方法实现的），是用TreeMap来保存--即V--value始终为空
>
> 而	TreeMap是以键值对存储的，所以添加的元素相同时则返回V----value的值不是null

### HashMap与HashTable

岁岁年年人不同，年年岁岁花相似；

> 数据结构---大数据方向必须要掌握的，之前一直想的是从事大数据方向，现在感觉那么遥远。。。。打工人。。。加油

```java
package com.Date.Test;

import java.util.*;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/21 - 07 - 21 - 15:50
 * @Description: com.Date.Test
 * @version: 1.0
 */
public class TodayTest {
    public static void main(String[] args) {

        Map<String,Integer>map = new HashMap<>();
        //添加数据----返回值为V类型
       /* public V put(K key, V value) {
            return putVal(hash(key), key, value, false, true);


        }*/

        map.put("张三",18);
        map.put("李四",18);
        map.put("王五",18);
        map.put("张三",19);
        map.put("赵六",18);
        map.put("郑八",18);
        Integer pu = map.put("郑八", 18);

        map.put("王十", 20);
        Integer pu1 = map.put("测试",10);
        System.out.println("添加重复元素返回---该元素的V--value值"+pu);
        System.out.println("添加非重复元素返回--"+pu1);
        System.out.println("集合大小"+map.size());
        System.out.println(map);
        //删除数据--返回值为Integer类型的信息
      /*  public V remove(Object key) {
            TreeMap.Entry<K,V> p = getEntry(key);
            if (p == null)
                return null;

            V oldValue = p.value;
            deleteEntry(p);
            return oldValue;
        }*/
        Integer  zs= map.remove("张三");//会移除哪一个张三的信息呢？
        System.out.println("移除的是"+zs);
        boolean te = map.remove("测试", 10);

        //        map.clear();//移除所有数据
        //查找数据
        Integer g = map.get("郑八");
        System.out.println(g);
//将键值作为一个集合
        Set<Map.Entry<String, Integer>> entries = map.entrySet();
        for (Map.Entry<String, Integer> entry : entries) {

            System.out.println(entry.getKey()+"----------"+entry.getValue());

        }
        System.out.println("--------------------");
        //得到value的集合
        Collection<Integer> values = map.values();
        for (Integer value : values) {

            System.out.println(value);
        }
    }

}


```

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
添加重复元素返回---该元素的V--value值18
添加非重复元素返回--null
集合大小7
{李四=18, 张三=19, 王五=18, 测试=10, 王十=20, 赵六=18, 郑八=18}
移除的是19
18
李四----------18
王五----------18
王十----------20
赵六----------18
郑八----------18
--------------------
18
18
20
18
18
```

------



 ArrayList       底层是Object[]数组，构造方法有三个,效率高，但是线程不安全
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/d267496a28c3866379d22a9eb52be576.png)

只看---  ArrayList ()  ArrayList( int 初始容量)
  每次插入都要检查容量是否足够,
无参构造---在第一次插入数据后才开始将数组扩容为10

> return elementData = new Object[Math.max(DEFAULT_CAPACITY,
> minCapacity)];
> 有参构造是直接返回我们指定的大小；
> 扩容机制----第一次添加数据是扩容为10，第二次扩容的时候为原来的1.5倍即15（newCapacity = oldCapacity + (oldCapacity >> 1);）第三次是22，第四次是33以此类推。。。。。

Vector底层也是Object[]数组，构造方法四种，可以分别指定标准容量和指定增量，
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/f46ab3842365911f5740b71845cf036e.png)扩容是每次扩容的时候是原来的二倍----无参构造
有参构造如Vector(10,3)则每次扩容增量为3，即 10,13,16.。。。
有参构造且没指定增量其实也是调用了参数为两个的构造，只不过增长量为0，那么每次扩容也是原来的2倍；

> int newLength = Math.max(minGrowth, prefGrowth) + oldLength;

LikedList---扩容----对不起，没有----因为是双向链表结构，可以随时添加，没有容量限制
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/9f0dde0b618b81b374a4c4bd34c817c4.png)

再来看Set接口中的
突然想到Set的底层其实是map来实现的，所以先看Map，那么对应的Set的扩容机制也就自然带出来了；
首先看Map接口中的HashMap；
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/1221931bc8e02c461d1bfd6e5f6b36e8.png)HashMap构造有四个，只看前三个

HashMap()---空构造器---先加载的是导入因子，扩容也是在添加第一个元素之后再开始扩容，第一次扩容为16

> public HashMap() {
>        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
>    }
>
> else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
>                     oldCap >= DEFAULT_INITIAL_CAPACITY)

当添加的数据达到初始容量*加载因子的时候开始扩容，

> newCap = DEFAULT_INITIAL_CAPACITY;
>             newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);

扩容机制---每次扩容是原来的二倍；

> ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
>                     oldCap >= DEFAULT_INITIAL_CAPACITY)
>                newThr = oldThr << 1;

HashMap(int 初始容量)---加载因子也是默认0.75，容量达到初始容量*加载因子的时候开始扩容，底层也是 resize ()扩容方法，扩容后大小是原来的二倍

 HashMap(int 初始容量，加载因子)容量达到初始容量*加载因子的时候开始扩容，扩容后大小是原来的二倍

------

> HashSet的扩容跟HashMap 一样；LinkedHashMap（HashMap的子类）也是；

------

TreeMap的扩容机制---对不起，也没有

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/662da8337d982fe80a343f079c47472f.png)Stack

  继承于Vector，所以他的扩容机制等同于Vector的默认情况，也就是2倍速度扩容



***2021.07.22日更新***

### Collection接口和Collections的区别

```java
package HuiXin;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        ArrayList<Integer>list= new ArrayList<>();
        list.add(111);
        list.add(99);
        list.add(222);
        list.add(88);
        boolean b = Collections.addAll(list,1,12,15);
        Collections.addAll(list,new Integer[]{12,13,1,45,78,5,45,4556});

        Collections.sort(list);
        System.out.println(list);
        System.out.println(Collections.binarySearch(list,4556));
        List<Integer>list1= new ArrayList<>();
        list1.add(12);
        list1.add(67);
        Collections.copy(list,list1);
        System.out.println(list1);
        System.out.println(list);
    }
}

```

注意的点是----
Collection接口和Collections区别
接口---
public interface Collection<E> extends Iterable<E> {
Collection接口的实现类---ArrayList，LinkedList，HashSet，TreeSet  
类--
public class Collections {
    // Suppresses default constructor, ensuring non-instantiability.
    private Collections() {/私有构造方法
    }
    既然是私有构造方法，说明不能被实例化，然后会发现该类中的方法都为静态方法，可以直接通过类名调用，这样做的好处是什么呢？
该类为public，但是类中所以的属性和方法都为静态的（除了构造方法）

![在这里插入图片描述](https://img-blog.csdnimg.cn/5e5f35fe48134795babb04289ef25699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
注意BinarySearch的方法查询之前要先进行排序