![](..\pic\div\deer.gif)

# JAVA知识点精选

## JAVA中的构造器

```java
package com.gavin.exercise;

import java.util.Scanner;

/**
 * @author : Gavin
 * @date: 2021/6/28 - 06 - 28 - 10:03
 * @Description: com.gavin.exercise
 * @version: 1.0
 */
class person{
    String name;
    int age;
    double height;

    public person(){
        this("小明",18);
        System.out.println("无参构造");
    }
    public person(String name){
        //this();总有一个要没有this来作为调用的出口，不然会形成递归调用，死循环出不来了
        this.name=name;
    }
    public person(String name,int age){
        this("小花");
        this.age=age;
        System.out.println("有两个参数的构造");

    }
    public person(String a ,int b,double height){
        this("小红",19);
        this.height=height;

    }
}
public class Contral {
    public static void main(String[] args) {
person per= new person();

        System.out.println(per.name);
    }
}

```

首先来分析一下代码中的构造，本段代码中有四个构造，

> 在无参构造中调用了有两个参数的构造，在一个参数的构造中没有调用任何构造（可以看到this（）被注释了，接下来会提到），在两个参数的构造中调用了一个参数的构造，在三个参数的构造中调用了两个参数的构造；

那么为什么一个参数的构造不能在调用其他构造呢？

> 因为这回造成递归调用，没有出口造成死循环；

程序执行的结果---

> 有两个参数的构造
> 无参构造
> 小花

接下来分析一下程序构造器是怎么调用的；
1，在内存中执行main方法，遇到person对象，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628192749579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
2，在方法区中（也叫静态区）中加载person类，同时在堆中开辟空间此空间的地址指向方法区中的person.class，然后在堆中开辟空间存放person类（在这个空间中存放着person.class的地址）--这一步在内存中隐式操作，我们看不到。

3，执行到person类的无参构造方法，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628192700359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 此时name=null，age=0，height=0.0，均为默认值

4，在这个无参构造方法里又去调用了person(String name,int age)，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628192718511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 此时name="小明" age=18, height=0.0(因为height没赋值);

5，在两参构造中调用有一个参数的构造，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628193330385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
6，找到一个参数的构造--（）

> --------假如在这里又去调用无参构造，在这里就会因为递归调用--形成一个死循环。
> 这里name=“小花”  age=18

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628193427681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
7，赋值完毕后-继续执行this.age=age; 输出"有两个参数的构造"，然后输出无参构造里面的内容--无参构造

> 小彩蛋---
> java的子类中必须调用父类的构造方法
> 待会会做演示；
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628194740962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
> 至此person类的构造加载完毕，再去执行输出部分
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628195120372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```bash
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
有两个参数的构造
无参构造
小花
18
0.0
```

看图更直接---划内存分析一下

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628200005215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
注意一点就是有些数据在编译时期就确定的就会放入常量池，不确定的变量不会；

下面演示一下    ----子类构造执行的时候也会调用父类相应构造；

```java
package com.gavin.exercise;

/**
 * @author : Gavin
 * @date: 2021/6/28 - 06 - 28 - 10:03
 * @Description: com.gavin.exercise
 * @version: 1.0
 */
class person extends human {
    String name;
    int age;
    double height;

    public person() {

        System.out.println("无参构造");
    }

    public person(String a, int b, double h) {
        //  super();即使没有这行代码也会去调用父类构造；
        name = a;
        age = b;
        height = h;

    }
}

class human {
    String name;
    int age;
    double height;

    public human() {
        System.out.println("human的无参构造");
    }

}

public class Contral {
    public static void main(String[] args) {
        person per = new person("小明", 19, 180.0);

        System.out.println(per.name);
        System.out.println(per.age);
        System.out.println(per.height);//ctrl+d复制一行代码
    }
}

```

感兴趣可以自己去分析一下内存；

> 在java中子类继承了父类的一些属性和方法，父类的初始化要靠子类去完成
> 构造器的作用--- 创建对象和初始化对象的属性--
> 因此构造一个对象，要调用其构造方法，用来初始化其成员函数和成员变量。
> 子类继承了父类的成员变量和成员方法，如果子类中没有调用父类的构造方法，那么子类从父类继承而来的成员变量和成员方法也得不到正确的初始化--因此子类必须去调用父类的构造方法；也即子类构造中会默认必须调用父类相应构造
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210628210841155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

下图为调用有参构造的时候--

> --其实主要是调用无参构造的时候由于super（）--被省略了，所以不容易被发现。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062821171568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 父类不能反过来调用子类，因为父类根本不知道子类有的变量（涉及到多态）而且这样一来子类也得不到初始化的父类变量，导致程序运行出错！



以上就是构造的一些知识----共同学习共同进步；



## JAVA中的String类

直接上代码---

**字符串的拼接**

```java
public class Test {
    public static void main(String args[]) {
//字符串的拼接
        String str0 = "abc";
        String str1 = "abc";
        String str2="a"+"b";
      String str3=new String ("abc");
      String b=str2+"c";
        System.out.println(str0==str1);
        System.out.println(str1==str3);
        System.out.println(str0==b);
        System.out.println(b);

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210712165215180.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

我们编译一下生成class'文件,然后反编译看结果---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210712162801749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

这几年java优化的还不错吧,避免产生不必要的垃圾.

再来,我们通过JVM拉看一下代码是怎么运行的,在控制台输入  javac  将java文件编译,然后输入javap -c   Test.class

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210712171953249.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



**new 关键字产生的String 对象**

new 关键字产生的String 对象    实际上是开了两块空间---一个在堆中,一个在堆中的方法区,见上图.

**有变量参与的字符串拼接**

有变量参与的字符串拼接,在编译器不会进行优化,见上图反编译的文件截图;

## StringBuilder与StringBuffer

> 不积跬步无以至千里,凡是从小开始,把每一次当作第一次来做,抱着一颗学习,敬畏的心;

不罗嗦,直接干-----这里是以最新的JDK版本为例---虽然现在企业大部分用的是jdk1.8但是不耽误我分析呀.....

```java
public class Test {
    public static void main(String args[]) {
        StringBuilder stringBuilder= new StringBuilder();
        stringBuilder.append("abcdef");
        System.out.println(stringBuilder);

    }
}
```

代码倒是很简单,我们来看底层StringBuilder都干了些啥;

看构造方法---

以无参构造为例--

```java
 public StringBuilder() {
        super(16);
    }

```

调用了父类的构造方法,然后传入了一个int类型的参数----

```java
  AbstractStringBuilder(int capacity) {
        if (COMPACT_STRINGS) {
            value = new byte[capacity];
            coder = LATIN1;
        } else {
            value = StringUTF16.newBytesFor(capacity);
            coder = UTF16;
        }
    }

```

底层是确定编码格式LATIN1还是UTF16,不管什么字节码编码格式他们都做了一个动作,开辟了一个字节数组长度为16
所以第一行代码所开展的操作如下图所示--
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071220564445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



接下来StringBuilder对象调用append方法

```java
 public StringBuilder append(String str) {
        super.append(str);
        return this;
    }
```

这里又调用了父类的append方法,并且将字符串传进去了,

```java
public AbstractStringBuilder append(String str) {
        if (str == null) {//如果传的是空,直接返回
            return appendNull();
        }
        int len = str.length();
        ensureCapacityInternal(count + len);//
        putStringAt(count, str);//count=0,str="abcedf"
        count += len;//此时count=6
        return this;
    }
```

有一些属性属于AbstractStringBuilder
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210712210713417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

假设我们传了个null进去---我们走一下if里面的条件,返回如下方法

```java
private AbstractStringBuilder appendNull() {
        ensureCapacityInternal(count + 4);
        int count = this.count;
        byte[] val = this.value;
        if (isLatin1()) {
            val[count++] = 'n';
            val[count++] = 'u';
            val[count++] = 'l';
            val[count++] = 'l';
        } else {
            count = StringUTF16.putCharsAt(val, count, 'n', 'u', 'l', 'l');
        }
        this.count = count;
        return this;
    }
```

,然后又去调用ensureCapacityInternal(),

```java
private void ensureCapacityInternal(int minimumCapacity) {//count + 4传进去了,count为静态到的初始化为0
        // overflow-conscious code
        int oldCapacity = value.length >> coder; //16
        if (minimumCapacity - oldCapacity > 0) {
        
            value = Arrays.copyOf(value,
                    newCapacity(minimumCapacity) << coder);
        }
    }
```

继续往下执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210712211837775.png)
即如果str是null ，则附加四个字符"null" 。 



因为本例传入参数不为空,,所以会执行下面的代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210712212510201.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70#pic_center)
putStringAt()方法,count=0,str就是一开始传入的那个参数

```java
 private final void putStringAt(int index, String str) {
        if (getCoder() != str.coder()) {//统一编码格式
            inflate();
        }  
        str.getBytes(value, index, coder);//这里是str调用了getBytes方法, index=0,value="abcedf",coder=0
    }
```

```java
void getBytes(byte dst[], int dstBegin, byte coder) {
        if (coder() == coder) {//编码格式
       // 数组的拷贝  value 为源数组"abcdef",从位置 0 开始拷贝到目标数组dst 的0到"abcdef"长度的位置
            System.arraycopy(value, 0, dst, dstBegin << coder, value.length);
            
        } else {    // this.coder == LATIN && coder == UTF16
            StringLatin1.inflate(value, 0, dst, dstBegin, value.length);
        }
    }
```

至此,StringBuilder的append方法得到了结果,再将结果一级一级返回去;

> 总结---
>  1,随着JDK版本的更新,和为了加快运算效率,String类底层由Char[]改为Byte[]数组;
> 2,StringBuilder底层是由字节数组的复制来完成的;

再来看StringBuffer



```java
public class Test {
    public static void main(String args[]) {
        StringBuffer sBuffer= new StringBuffer();
        sBuffer.append("abcdef");
        System.out.println(sBuffer);

    }
}
```

按照惯例看无参构造----

```java
 public StringBuffer() {
        super(16);//调用父类的构造
        /**AbstractStringBuilder(int capacity) {
        if (COMPACT_STRINGS) {
            value = new byte[capacity];
            coder = LATIN1;
        } else {
            value = StringUTF16.newBytesFor(capacity);
            coder = UTF16;
        }*/
    }
    }
```

这里也是整了一个长度为16的byte数组;不在啰嗦;

来看append方法

```java
 public synchronized StringBuffer append(String str) {
        toStringCache = null;
        super.append(str);
        return this;
    }

```

这里用同步来修饰说明线程是安全的;
同时这里有一个私有的临时字符串缓存变量--toStringCache,一开始为引用为空

> private transient String toStringCache;

接着又是调用父类的append方法,跟StringBuilder一样,
不同的是有一个 toStringCache;
来看官方给的描述--

> A cache of the last value returned by toString. Cleared
> whenever the StringBuffer is modified.
> 每当toString返回一个字符串时,字符缓冲池就被清空







> 总结---
> 1,基本上StringBuilder与StringBuffer的底层操作差不多,StringBuffer和StringBuilder里的方法也基本都一样,都是继承自父类-AbstractStringBuilder的;
> 2 当字符串操作量少的话,两个效率差不多,但是当操作数量上去了,就要权衡效率与线程安全的问题了;
> 3,当我们有涉及到字符串修改的多线程的时候,尽量用StringBuffer以确保能够的到正确的结果;



如有不当之处,还请各位大神不吝赐教;

## JAVA中的包装类



> 深入学习次啊能刻到骨子里---但是如果年代久远的话还是会忘，只不过忘得会慢一些；
>
> 弱弱的问一句，程序员会的老年痴呆吗？

为什么会有包装类？？？有了基本数据类型不就行了？？

我们都说java是一门面向对象的语言，万物皆对象，将八种基本数据类型
抽象为类，于是乎就有了基本数据类型的对应的包装类；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210707174335667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)其中数值型的父类为Number，Number父类为Object
字符型和布尔型的父类为 Object

每一中包装类都为final修饰--以Integer为例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210707174715942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)说明包装类没有子类，不可被继承

话不都说，先看代码---Integer类

```java
public class test {
    public static void main(String[] args) {
//       Integer i =new Integer(2);//此种写法已经舍弃了，直接写为下面的形式
//       Integer I2=new Integer(2);
//        System.out.println(i.equals(I2));
//        System.out.println(i==I2);
        Integer num1=12;
            Integer num2=12;
        System.out.println(num1==num2);//true
        System.out.println(num1.equals(num2));//true
        System.out.println(num1.compareTo(num2));//0

        int a=12;
        int b=12;
        System.out.println(a==num1);//true
    }
}
```

细心的可能发现基本数据类型和对象a==num1结果 为true
我们反编译一下会发现程序内部自动拆箱操作，

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210707175201768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
我们来看一下valueOf 方法的源码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210707175338891.png)
这里面提到了IntegerCache.cache[i + (-IntegerCache.low)]再次查看

```java
private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer[] cache;
        static Integer[] archivedCache;

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    h = Math.max(parseInt(integerCacheHighPropValue), 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(h, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            // Load IntegerCache.archivedCache from archive, if possible
            VM.initializeFromArchive(IntegerCache.class);
            int size = (high - low) + 1;

            // Use the archived cache if it exists and is large enough
            if (archivedCache == null || size > archivedCache.length) {
                Integer[] c = new Integer[size];
                int j = low;
                for(int i = 0; i < c.length; i++) {
                    c[i] = new Integer(j++);
                }
                archivedCache = c;
            }
            cache = archivedCache;
            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }
```

以上代码啥意思呢？---
其实就是定义了一个数组，长度为256，数组中可以存放[-128,127] 之间的整数，这样做的好处是什么呢？存放常用的数据利于提高程序运行速度；、

接下来就是重点了--

```java
public class test {
    public static void main(String[] args) {
        Integer num1=180;
            Integer num2=180;

        System.out.println(num1==num2);
        System.out.println(num1.equals(num2));
        System.out.println(num1.compareTo(num2));
        int a=180;
         System.out.println(a==num1);

         Integer num3 =5;
            Integer num4=5;
        System.out.println(num3==num4);
        System.out.println(num3.equals(num4));
        System.out.println(num3.compareTo(num4));
        int b=5;
        System.out.println(b==num3);
    }
}
```

> 你会发现当包装类的数值在[-128,127]之间的数值，比较地址他们是相等的，说明----
> -128到127被加载到了常量池(跟String类似),如果超出了这个范围,才会新产生一个对象,所以上述代码中会出现一个false,一个true的结果;

integer的构造方法--
Integer(int value) 
Integer(String s) 
Integer 没有无参构造, 

integer中提供的常用方法-

```java
package baozhuang;

/**
 * @author : Gavin
 * @date: 2021/7/7 - 07 - 07 - 21:36
 * @Description: baozhuang
 * @version: 1.0
 */
public class Test {
    public static void main(String[] args) {
        Integer num= 12;
        double num2=num.doubleValue();
        double num3=12;
        System.out.println(num2==num3);
        Integer num4= 129;
        double num5=num.doubleValue();
        double num6=129;
        System.out.println(num5==num6);

        Integer a= 11;
      float b=  a.floatValue();
        System.out.println(b==a);
        byte c= a.byteValue();
        short d=a.shortValue();
        //字符换转数字
        int F= Integer.parseInt("123456");
        //字符串转相应进制数,radix最小为2,最大为32
        int G=Integer.parseInt("11111",8);//转换为十进制数
        int H=Integer.parseInt("11111",16);//转换为十六进制数

        System.out.println(F);
        System.out.println("1111的八进制数是--:"+G);
        System.out.println("1111的十六进制数是--:"+H);
        }
}

```

Integer类中提供了数据类型转换的方法,同时也提供了进制转换的方法;

这里说一个比较重要的方法---compareTo();---两个数进行比较大小
源码--

```java
public int compareTo(Integer anotherInteger) {
        return compare(this.value, anotherInteger.value);
    }


```

这里调用了compare方法,

```java
public static int compare(int x, int y) {
        return (x < y) ? -1 : ((x == y) ? 0 : 1);
    }
```

三目运算符的嵌套--- 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210707222113202.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

------

在来学习一下double的包装类--Double

------

```java
public class Test {
    public static void main(String[] args) {
       
        Double a= 12.0;
Double b=12.0;
        System.out.println(a==b);

        Byte c=12;
        Byte d =12;
        System.out.println(c==d);


        Short c1=12;
        Short d1 =12;

        System.out.println(c1==d1);
        Short c2=129;
        Short d2 =129;
        System.out.println(c2==d2);
        }
}
```

总结--

> 只要是整数的包装类,都会有一个缓冲池[-127,128],将他们放入常量池以便快速取用--Byte类型的没有,因为他的范围就是[-127,128];
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210707223509699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

包装类Boolean更直接---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210707223849838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
构造方法--
Boolean(boolean value) 
Boolean(String s) 
第一种构造方法很直观,来看第二种---

```java
public class Test {
    public static void main(String[] args) {
Boolean a= true;
Boolean b= true;
        System.out.println(a==b);
        Boolean c= new Boolean("true");//此种声明方式已弃用
        Boolean d= new Boolean("True");

        System.out.println(c==d);

        Boolean c1= new Boolean("false");
        Boolean d1= new Boolean("False");
        System.out.println(c1==d1);
      boolean flag=  Boolean.parseBoolean("true");
        System.out.println(flag);
        }
}

```

> 注意--- 包装类中一般不使用new来产生一个对象,而是像基本数据类型那样直接声明,因为底层已经帮我们自动装箱拆箱操作了;

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210707225118827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

------

```java
import java.sql.Date;

/**
 * @author : Gavin
 * @date: 2021/7/8 - 07 - 08 - 19:37
 * @Description: baozhuang
 * @version: 1.0
 */
public class Testdate {
    public static void main(String[] args) {
       Date date= new Date(121,1,1);//弃用
       System.out.println(date);
        java.sql.Date extends java.util.Date
        Date d=new Date(1234567890L);
        System.out.println(d);

        java.util.Date date1=new java.util.Date();
       // java.util.Date date1= new Date(121,1,1);
        Date dd=(Date)date1;
        Date date2= new Date(date1.getTime());
        java.util.Date date3=dd;
        System.out.println(date3);
    }
}

```

## JAVA中的Date类(日期)

> 查看API会发现util.Date中的大部分方法都过时了,历史遗留原因,逐渐被Calendar类所取代,但是,他还有一个亲戚--sql.Date;
> 查看源码发现他是util.Date的子类;
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708200433181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708200447370.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708200527464.png)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708200618262.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

感兴趣的可以自己查看,

这里我比较感兴趣的是---after和before方法

```java
Date a= new Date(121,5,7);
        java.util.Date b= new java.util.Date();
        System.out.println(a.after(b));
        System.out.println(a.before(b));
```

这里要注意一点----关于多态

```java
 java.util.Date date1=new java.util.Date();
              Date dd=(Date)date1;
        Date date2= new Date(date1.getTime());
        java.util.Date date3=dd;
        System.out.println(date3);
```

```java
 java.util.Date date1= new Date(121,1,1);
        Date dd=(Date)date1;
        Date date2= new Date(date1.getTime());
        java.util.Date date3=dd;
        System.out.println(date3);
```

以上两个代码,哪一个能转型成功呢?

答案:---第二个;
为什么呢?虽然第一处代码检查时没有报错,但是运行时会出现类型转换异常

这是因为一个父类想要转换成子类,那么父类的对象必须是子类的性质,否则也会产生类型转换异常--举个例子---

```java
class Animal{}
class Dog extends Animal{}
class Cat   extends Animal{}
public class Testdate {
    public static void main(String[] args) {
        Animal an = new Animal();
     //  Dog dog=(Dog) an;//类型转换异常

Animal ba=new Dog();
Dog dog=(Dog) ba;
    }
}
```

所以--父类对象指向子类对象才能转型成功;

```java
 public class Testdate {
    public static void main(String[] args) {
        java.util.Date date1= new Date(121,1,1);//父类---->子类
        //java.util.Date date1=new java.util.Date();//父类实例
        java.sql.Date dd=(java.sql.Date)date1;//sql.Date dd=(sql.Date )util.Date ---这里不知道Date具体是谁,也有可能是其他子类;
        //要想成功转型

        java.sql.Date date2= new java.sql.Date(date1.getTime());
        System.out.println(date1.getTime());


    }
}
```

> 将字符串转换为日期总共分几步? 
> 第一步,将字符串转换为sql.Date, 
> 第二步,然后在转换为util.Date
> 第三步,将日期打印输出(其实是转换为字符串输出)---这不有毛病吗!

有毛病的代码---

```java
public class Testdate {
    public static void main(String[] args) {

java.util.Date date1=java.sql.Date.valueOf("2021-07-08");
        System.out.println(date1);
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708205855337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

着中国方式有一个致命缺点---就是字符串的格式 年-月-日,如果不符合要求,那么就会转换失败;

```java
public class Testdate {
    public static void main(String[] args) {

java.util.Date date1=java.sql.Date.valueOf("2021-07-08");
        System.out.println(date1);
//        java.util.Date date2=java.sql.Date.valueOf("2021/07/08");//ava.lang.IllegalArgumentException
//        System.out.println(date2);
        java.util.Date date3=java.sql.Date.valueOf("2021-7-8");
        System.out.println(date3);
        java.util.Date date4=java.sql.Date.valueOf("2021-00007-00008");//java.lang.IllegalArgumentException
        System.out.println(date4);
    }
}
```

所以实际开发中经常用DateFormat 来进行操作;

DateFormat 是一个抽向类--继承自Format
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708210613224.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
Format也是一个抽象类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708210650593.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708210823875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
往下倒腾,倒腾找到具体的子类----

> public class SimpleDateFormat extends DateFormat {}
> SimpleDateFormat是一个具体的类，用于以区域设置敏感的方式格式化和解析日期。 它允许格式化（日期文本），解析（文本日期）和归一化。 

拿过一个类,先看有没有父类实现了哪些接口,然后再看构造方法,再看具体的方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708211231584.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

第二个构造方法允许我们自定义日期格式

> 日期和时间模式  日期和时间格式由日期和时间模式字符串指定。
> 在日期和时间模式字符串中，从'A'到'Z'和从'a'到'z'的非引号的字母被解释为表示日期或时间字符串的组件的模式字母。 可以使用单引号（
> ' ）引用文本，以避免解释。 "''"代表单引号。 所有其他字符不被解释;
> 在格式化过程中，它们只是复制到输出字符串中，或者在解析过程中与输入字符串匹配。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708211443935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210708211736304.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
public class Testdate {
    public static void main(String[] args) throws ParseException {
        DateFormat dateFormat= new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//这里的格式是限制parse方法参数的格式的,
        Date date=dateFormat.parse("2020-12-12 12:12:12");//将字符串站换为日期
        System.out.println(date.toString());
        Date d=dateFormat.parse("012010-12-23 14:23:45",new ParsePosition(2));//parse(String source, ParsePosition pos) 
根据给定的解析位置解析日期/时间字符串。
        System.out.println(d);
    }
}
```

打印的时候还是默认打印 date.toString();
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070821245112.png)
除非我们覆写该方法--
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070821273199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

既然能将字符串转换为日期,那么反过来也是可以的---

```java
public class Testdate {
    public static void main(String[] args) throws ParseException {
        DateFormat dateFormat= new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//这里的格式是限制parse方法参数的格式的,
        String str = dateFormat.format(new Date());//将日期转换为字符串
        System.out.println(str);
    }
}

```

## JAVA中的Calendar 类

> 一个类的设计在之前可能考虑不周,后期进行不断修改完善---我猜的

话不多说--先看代码

关于Date的代码在上一篇已经提到了,感兴趣可以移步,下面是链接

> https://blog.csdn.net/weixin_54061333/article/details/118581212

Calendar 类是一个抽象类,实例化的话需要子类---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709122005576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
public class Testdate {
    public static void main(String[] args) throws ParseException {
        Calendar calendar= new GregorianCalendar();
 System.out.println(calendar);

        
```

看看输出结果--

```bash
java.util.GregorianCalendar[time=1625804460911,areFieldsSet=true,
areAllFieldsSet=true,lenient=true,zone=sun.util.calendar.
ZoneInfo[id="Asia/Shanghai",offset=28800000,dstSavings=0,
useDaylight=false,transitions=31,lastRule=null],firstDayOfWeek=1,
minimalDaysInFirstWeek=1,ERA=1,YEAR=2021,MONTH=6,WEEK_OF_YEAR=28,
WEEK_OF_MONTH=2,DAY_OF_MONTH=9,DAY_OF_YEAR=190,DAY_OF_WEEK=6,
DAY_OF_WEEK_IN_MONTH=2,AM_PM=1,HOUR=0,HOUR_OF_DAY=12,MINUTE=21,
SECOND=0,MILLISECOND=911,ZONE_OFFSET=28800000,DST_OFFSET=0]

Process finished with exit code 0

```

Calender类包含的东西很多,所以可以直接拿过来用,但是直接给用户看--用户保证不打死你;

别着急,

```java
public class Testdate {
    public static void main(String[] args) throws ParseException {
        Calendar calendar= new GregorianCalendar();//父类通过子类实例化
        //System.out.println(calendar);
        StringBuffer stringBuffer= new StringBuffer();
        stringBuffer.append(calendar.get(Calendar.YEAR));
        stringBuffer.append(calendar.get(Calendar.MONTH)+1);
        stringBuffer.append(calendar.get(Calendar.DAY_OF_MONTH));
        System.out.println(stringBuffer);
Calendar calendar1= Calendar.getInstance();//实例化的另一种方式
    }
}
```

看一下getInstance()源码--

> ```java
> public static Calendar getInstance()
>   {
>       Locale aLocale = Locale.getDefault(Locale.Category.FORMAT);
>       return createCalendar(defaultTimeZone(aLocale), aLocale);
>   }
> ```
>
> 

该方法返回一个Calendar的实例(return createCalendar)
再来看一下createCalendar()源码

> ```java
> private static Calendar createCalendar(TimeZone zone,  Locale aLocale)
>   {
>       CalendarProvider provider =     LocaleProviderAdapter.getAdapter(CalendarProvider.class, aLocale)                         .getCalendarProvider();
> ```
>
> 

此方法根据时区不同返回不同的结果---国际化时间

Calendar类中还有很多其他方法

```java
public class Testdate {
    public static void main(String[] args) throws ParseException {

Calendar calendar1= Calendar.getInstance();//实例化的另一种方式--
        System.out.println(calendar1.getFirstDayOfWeek());//获取一周的第一天是什么
        System.out.println(calendar1.getActualMaximum(Calendar.DATE));
        System.out.println(calendar1.getActualMinimum(Calendar.DATE));
    }
}

```

```java
1
31
1
```

既然可以获得数据,同样可以设置数据--------

```java
public class Testdate {
    public static void main(String[] args) throws ParseException {

Calendar calendar1= Calendar.getInstance();//实例化的另一种方式--
        calendar1.set(Calendar.YEAR,1992);
        calendar1.set(Calendar.MONTH,10);
        calendar1.set(1993,10,19);
        calendar1.set(1993,10,19,12,42,34);
        System.out.println(calendar1.get(Calendar.YEAR));
        System.out.println(calendar1.get(Calendar.MONTH));
        System.out.println(calendar1.get(Calendar.DAY_OF_MONTH));
//字符串转Calendar
        java.sql.Date date=java.sql.Date.valueOf("2022-12-11");
        calendar1.setTime(date);
        System.out.println("---------------------华丽的分割线---------------------------");
        System.out.println(calendar1.get(Calendar.YEAR));
        System.out.println(calendar1.get(Calendar.MONTH)+1);
        System.out.println(calendar1.get(Calendar.DAY_OF_MONTH));
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709125055652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
每天能学习一小步,日后精进一大步



**Calendar实现打印日历**

直接上代码---

```java
import java.util.Calendar;
import java.util.Scanner;

/**
 * @author : Gavin
 * @date: 2021/7/9 - 07 - 09 - 21:50
 * @Description: baozhuang
 * @version: 1.0
 */
public class Rili {
    public static void main(String[] args) {
        System.out.println("请输入您要查询的日期--格式(2021-07-09)");
        Scanner sc = new Scanner(System.in);
        String strTime=sc.next();//接收用户输入
        //将字符转转换为Date
        java.util.Date date= java.sql.Date.valueOf(strTime);
        System.out.print("日\t一\t二\t三\t四\t五\t六\n");

        //打印天数---获得给的日期的天数--转换成Calendar类
        Calendar cal=Calendar.getInstance();
        cal.setTime(date);//从0开始为1月,
int nowDay=cal.get(Calendar.DAY_OF_MONTH);
        int maxday=cal.getActualMaximum(Calendar.DAY_OF_MONTH);//获得月份的天数
        //System.out.println(maxday);
        cal.set(Calendar.DAY_OF_MONTH,1);//设置当月天数为1
       // System.out.println(cal);
        int nullPoint=cal.get(Calendar.DAY_OF_WEEK);//获得一个周的第2天--周一
//打印的时候空(nullPoint-1 ) 个空格
        //System.out.println(nullPoint);
        int count =0;
        for (int i = 0; i <nullPoint-1 ; i++) {
            System.out.print("\t");
        }
       count=count+nullPoint-1;
        for (int i = 1; i <=maxday ; i++) {
           if(i==nowDay){
                System.out.print(i+"*"+"\t");
            }else{
               System.out.print(i+"\t");
           }

            count++;
            if(count%7==0){
                System.out.println();
            }
        }
        sc.close();
    }
}

```

主要是注意日期月份有些是从0开始的,西方跟东方的差别吧!!
思路已经写在了代码里;虽然有时候不能一下在写出来,要不断调试;

滚回去改Bug说的就是这个嘛???

## util.Date、sql.Date与Calendar

前几天刚刚写了关于java中的日期的类---util.Date、sql.Date，Calendar
这三个类之间有啥区别与联系？
首先Date是第一批关于时间的类，后来出现了Calendar；

其次 util.Date是sql.Date的父类，

> public class Date extends java.util.Date{}

然后是Calendar，他是一个抽象类，也有自己的子类-- GregorianCalendar

> public class GregorianCalendar extends Calendar {}

再说一下具体的细节，
util.Date 有无参构造，而sql.Date没有，sql.Date的构造可以接收字符串
然后经过转型可以转换为util.Date类型

Calendar 由于是抽象类，实例化要靠子类，但是还有一个方法可以得到Calendar的实例  getInstance();

一般情况下日期是不允许被修改的，于是乎后来又有了 LocalDate、LocalDateime，等关于日期的类；
今天借此机会捣鼓一下子；

> 获取当前的日期，时间，日期+时间

```java
package com.Date.Test;
import java.time.LocalDate;
import java.time.LocalDateTime;
/**
 * @Auther: GavinLim
 * @Date: 2021/7/10 - 07 - 10 - 10:29
 * @Description: com.Date.Test
 * @version: 1.0
 */
public class Test {
    public static void main(String[] args) {
   // now()--获取当前的日期，时间，日期+时间
        LocalDate localDate=LocalDate.now();
        System.out.println(localDate);//2021-07-10
        LocalTime localTime= LocalTime.now();
        System.out.println(localTime);//10:50:02.481155100
        LocalDateTime localDateTime= LocalDateTime.now();
        System.out.println(localDateTime);//2021-07-10T10:50:02.482155200

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071011095667.png)

now()方法源代码---

```java
 public static LocalTime now() {
        return now(Clock.systemDefaultZone());
    }//返回系统默认的时区
```

systemDefaultZone()
该方法返回一个时钟，该时钟使用最佳可用系统时钟返回时钟的当前时刻，其中返回时钟的Zone是默认时区

```java
public static Clock systemDefaultZone() {
        return new SystemClock(ZoneId.systemDefault());
    }//返回一个系统时钟的对象，

 public static ZoneId systemDefault() {
        return TimeZone.getDefault().toZoneId();
    }

```



设置指定的日期，时间，日期+时间

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210710112524367.png)

```java
javapublic class Test {
    public static void main(String[] args) {

        Month month = Month.APRIL;
        LocalDate.of(2021, 7, 10);
        LocalDate localDate = LocalDate.of(2021, month, 10);

        System.out.println(localDate);
        LocalTime.of(12, 12, 12, 456);
        LocalTime localTime = LocalTime.of(11, 14, 56);
        System.out.println(localTime);
        LocalDateTime localDateTime = LocalDateTime.of(1999, 12, 4, 12, 45, 56, 456);

        System.out.println(localDateTime);
    }
}
```

结果---

```java
2021-04-10
11:14:56
1999-12-04T12:45:56.000000456

```

of()方法的源码--

```java
public static LocalDate of(int year, int month, int dayOfMonth) {
        YEAR.checkValidValue(year);
        MONTH_OF_YEAR.checkValidValue(month);
        DAY_OF_MONTH.checkValidValue(dayOfMonth);
        return create(year, month, dayOfMonth);
    }

//返回值为LocalDate，来看一下create方法
 private static LocalTime create(int hour, int minute, int second, int nanoOfSecond) {
        if ((minute | second | nanoOfSecond) == 0) {
            return HOURS[hour];
        }
        return new LocalTime(hour, minute, second, nanoOfSecond);
    }

```

获取我们想要的数据---下面只列了几种，

```java
public class Test {
    public static void main(String[] args) {

        Month month = Month.APRIL;
        LocalDate.of(2021, 7, 10);
        LocalDate localDate = LocalDate.of(2021, month, 10);

        System.out.println(localDate);
        LocalTime.of(12, 12, 12, 456);
        LocalTime localTime = LocalTime.of(11, 14, 56);
        System.out.println(localTime);
        LocalDateTime localDateTime = LocalDateTime.of(1999, 12, 4, 12, 45, 56, 456);

        System.out.println(localDateTime);
        System.out.println(localDate.getYear());
        System.out.println(localDateTime.getHour());
    }
}
```

## JAVA中的日期转换(一)

> 继续日期类的学习DateTimeFormatter----
> 异常---java.time.format.DateTimeParseException:

***字符产转换为时间类的第二种方式***
第一种见链接----

> https://blog.csdn.net/weixin_54061333/article/details/118581212

```java
package baozhuang;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.temporal.TemporalAccessor;
import java.util.Date;

/**
 * @author : Gavin
 * @date: 2021/7/8 - 07 - 08 - 19:19
 * @Description: Date
 * @version: 1.0
 */
public class CESHI {
    public static void main(String[] args) {
       //日期格式化类---DateTimeFormatter
//        预定义的标准格式
        DateTimeFormatter dateTimeFormatter= DateTimeFormatter.ISO_LOCAL_DATE_TIME;
        //System.out.println(dateTimeFormatter);
    LocalDateTime localDate= LocalDateTime.now();
    String str=localDate.format(dateTimeFormatter);
        System.out.println(str);

////        String---->LocalDateTime字符串转换为时间---要了解定义时间的标准格式
       TemporalAccessor par=dateTimeFormatter.parse("2021-07-10T12:46:52.5711012");//这里格式一定,不符合格式不行
       //TemporalAccessor par1=dateTimeFormatter.parse("2021-07-10  2:46:52.5711012");//报错-- java.time.format.DateTimeParseException:
        System.out.println(par);
        //System.out.println(par1);
    }
}

```

这里要注意的是要知道标准格式是什么样的形式,如果字符串格式不会,那么会出现java.time.format.DateTimeParseException:,,

 先看一下别的
日期转换为字符串的其他方式--比较常用;

```java
package baozhuang;

        import java.time.LocalDate;
        import java.time.LocalDateTime;
        import java.time.ZoneId;
        import java.time.format.DateTimeFormatter;
        import java.time.format.FormatStyle;
        import java.time.temporal.TemporalAccessor;
        import java.util.Date;

/**
 * @author : Gavin
 * @date: 2021/7/8 - 07 - 08 - 19:19
 * @Description: Date
 * @version: 1.0
 */
public class CESHI {
    public static void main(String[] args) {
        //日期格式化类---DateTimeFormatter
//        预定义的标准格式
        DateTimeFormatter dateTimeFormatter= DateTimeFormatter.ISO_LOCAL_DATE_TIME;
        //System.out.println(dateTimeFormatter);
        LocalDateTime localDate= LocalDateTime.now();
        String str=localDate.format(dateTimeFormatter);
        System.out.println(str);

////        String---->LocalDateTime字符串转换为时间---要了解定义时间的标准格式
        TemporalAccessor par=dateTimeFormatter.parse("2021-07-10T12:46:52.5711012");//这里格式一定,不符合格式不行
        //TemporalAccessor par1=dateTimeFormatter.parse("2021-07-10  2:46:52.5711012");//报错-- java.time.format.DateTimeParseException:
        System.out.println(par);
        //System.out.println(par1);

        //本地化时间格式

        DateTimeFormatter dateTimeFormatter2=DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL).withZone(ZoneId.systemDefault());
        LocalDateTime localDateTime=LocalDateTime.now();
        String format = dateTimeFormatter2.format(localDateTime);
        System.out.println(dateTimeFormatter2);
        System.out.println(format);
//
       DateTimeFormatter dateTimeFormatter3=DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG).withZone(ZoneId.systemDefault());
        LocalDateTime localDateTime1=LocalDateTime.now();
        String format1 = dateTimeFormatter3.format(localDateTime1);
        System.out.println(dateTimeFormatter3);
        System.out.println(format1);
//
        DateTimeFormatter dateTimeFormatter5=DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM).withZone(ZoneId.systemDefault());
        LocalDateTime localDateTime5=LocalDateTime.now();
        String format3 = dateTimeFormatter5.format(localDateTime5);
        System.out.println(dateTimeFormatter5);
        System.out.println(format3);
      DateTimeFormatter dateTimeFormatter4=DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT).withZone(ZoneId.systemDefault());
        LocalDateTime localDateTime2=LocalDateTime.now();
        String format2 = dateTimeFormatter4.format(localDateTime2);
        System.out.println(dateTimeFormatter4);
        System.out.println(format2);

    }
}

```

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
2021-07-10T13:43:57.680317
{},ISO resolved to 2021-07-10T12:46:52.571101200
Localized(FULL,FULL)
2021年7月10日星期六 格林尼治标准时间 下午1:43:57
Localized(LONG,LONG)
2021年7月10日 CST 下午1:43:57
Localized(MEDIUM,MEDIUM)
2021年7月10日 下午1:43:57
Localized(SHORT,SHORT)
2021/7/10 下午1:43
```

withZone(ZoneId.of("UTC"))指定时区,在JDK1.8之后必须要指定时区,否则会运行时报错--Exception in thread “main“ java.time.DateTimeException: Unable to extract ZoneId from temporal
每一种字符串格式要对应起来
例如--

```java
 DateTimeFormatter dateTimeFormatter4=DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT).withZone(ZoneId.systemDefault());
        LocalDateTime localDateTime2=LocalDateTime.now();
        String format2 = dateTimeFormatter4.format(localDateTime2);
        System.out.println(dateTimeFormatter4);
        System.out.println(format2);
TemporalAccessor temporalAccessor=dateTimeFormatter4.parse("2021年7月10日星期六 格林尼治标准时间 下午1:43:57");
        System.out.println(temporalAccessor);
```

```java
Exception in thread "main" java.time.format.DateTimeParseException: Text '2021年7月10日星期六 格林尼治标准时间 下午1:43:57' could not be parsed at index 4
	at java.base/java.time.format.DateTimeFormatter.parseResolved0(DateTimeFormatter.java:2051)
	at java.base/java.time.format.DateTimeFormatter.parse(DateTimeFormatter.java:1879)
```

那么有没有自定义格式的方法呢?肯定有呀.下一篇继续----我写下一篇的目的主要是为了复习之前学的.......



日拱一兵,只进不退



## JAVA中的日期转换(二)

接上一篇内容

这篇只放代码--不解释;

```java
package baozhuang;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.time.temporal.TemporalAccessor;
import java.util.Date;

/**
 * @author : Gavin
 * @date: 2021/7/8 - 07 - 08 - 19:19
 * @Description: Date
 * @version: 1.0
 */
public class CESHI {
    public static void main(String[] args) {
//        字符与时间相互转化
        //时间转字符串
        DateTimeFormatter da = DateTimeFormatter.ISO_LOCAL_DATE_TIME;//定义时间格式
        LocalDateTime loc = LocalDateTime.now();
        String format = da.format(loc);
        System.out.println(format);

        //字符转时间
        DateTimeFormatter date = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
        TemporalAccessor parse = date.parse("2021-07-10T22:04:45.5283886");//接口接收时间
        System.out.println(parse);

//        方式二

//        时间转字符串
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT).withZone(ZoneId.systemDefault());//指定时间格式,指定时区

        LocalDateTime local = LocalDateTime.now();//获取时间
        String format1 = dateTimeFormatter.format(local);
        System.out.println(format1);
//        FormatStyle.FULL----->2021年7月10日星期六 中国标准时间 下午10:29:22
//        FormatStyle.LONG-------> 2021年7月10日 CST 下午10:31:14
//        FormatStyle.MEDIUM-------->2021年7月10日 下午10:31:59
//        FormatStyle.SHORT---------> 2021/7/10 下午10:32
//        字符串转时间
        TemporalAccessor parse1 = dateTimeFormatter.parse("2021/7/10 下午10:32");
        System.out.println(parse1);
//        时间转字符串
        LocalDateTime loca = LocalDateTime.now();
        String str = dateTimeFormatter.format(loca);
        System.out.println(str);

        //自定义  时间格式-----------
        DateTimeFormatter dt=DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
    //时间转字符串
        LocalDateTime localD=LocalDateTime.now();
        String format2 = dt.format(localD);
        System.out.println(format2);
//        字符串转时间-----
        TemporalAccessor parse2 = dt.parse("2012-12-02 12:12:12");
        System.out.println(parse2);
    }

}

```

## JAVA中的接口interface

```java
package com.array.sort.test;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/3 - 07 - 03 - 12:48
 * @Description: com.array.sort.test
 * @version: 1.0
 */
public interface MyInterface {
    /*基本数据类型*/
    public final static int Num = 100;//接口中可以放基本数据类型--不能修改
    public final int Num1 = 200;
    public static int Num2 = 300;
    public int Num3 = 300;
    int Num4 = 200;//可以简写成这种形式

    /* 方法*/
    /*需要被实现的方法--一定是抽象方法*/
    public abstract void a();

    public void aa();//简写的形式

    void aaa();//简写的形式

    /*静态方法--不被实现的，接口自己特有的，子类不能访问*/
    public static void c() {
        System.out.println("接口中的静态方法有自己的方法体");
    }

    static void d() {
        System.out.println("接口中的静态方法不被实现");
    }

    ;//简写的形式

    //默认方法--可以被实现也可以不被实现
    public default void e() {
        System.out.println("接口中的default 方法也可以有自己的方法体，可以不被实现");
    }
}

class Person implements MyInterface {
  /*  Num=200;
    Num1=200;
    Num2=300;
    Num3=300;
    Num4=500;*///修改都会报错

    @Override
    public void a() {

        e();//default 方法,实现类可以直接调用；
        //  super.e();
        MyInterface.super.e();
    }

    /*需要被实现的方法*/
    @Override
    public void aa() {
        System.out.println("aa-----实现了");
    }

    @Override
    public void aaa() {//这里权限修饰符号不能省
        System.out.println("aaa-----实现了");
    }

    @Override
    public void e() {//实现类中要想实现接口中的非抽象方法，

        System.out.println("子类实现了接口中的default方法");
    }

    void d() {
        System.out.println("这是个什么--子类特有的方法");
    }


}
```

```bash
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
接口中的静态方法有自己的方法体
接口中的静态方法不被实现
aa-----实现了
aa-----实现了
这是个什么--子类特有的方法
子类实现了接口中的default方法
子类实现了接口中的default方法

Process finished with exit code 0
```

总结：
接口中有--、
1，基本数据类型
2，引用数据类型
3，方法
静态方法，
default方法
抽象方法

> 在jdk1.8之后才增加了静态方法和default方法，目的是为了---
> 方便维护代码；
> 因为如果只能增加抽象方法，那么接口的所有实现类就必须把他实现了，这样就要在每个实现类中增加该方法；

共同学习共同进步-日拱一兵，只进不退



## JAVA中的内部类

代码较长，需要仔细查看哦！

> 各段代码都会有小结，如看不懂请查阅资料，看懂个大概就可以了，毕竟内部类就像套娃，打开大娃才能看到二娃，

```java
package finallyvar;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/3 - 07 - 03 - 16:44
 * @Description: finallyvar
 * @version: 1.0
 */
class Animal {
    String name = "xiaodongwu";//名字
    static double weight = 100.00;//种类
    private String kind = "动物";

    public void info() {
        /*方法局部内部类--位于方法之中，也可以位于构造方法之中
         方法内部类只能在定义该内部类的方法内实例化，
        不可以在此方法外对其实例化。*/
        abstract class Cat {

         //final   String   name = "mao";
            String   name = "mao";//简写的形式，实际上是省略了final修饰
            //name="xiaohou";//final修饰的没法更改
          final  public   void info() {//用于修饰方法内部类的只有final和abstract
                System.out.println(name);// --mao 内部类可以直接访问外部类的属性
                System.out.println(kind);//动物   内部类可以直接访问外部类的私有属性
               // info();//内部类可以直接访问外部类的方法，
                infoed();//这是外部类的infoed方法   内部类可以直接访问外部类的私有方法

                /**访问重名的属性的方式*/
                /**普通的类可以用this引用当前的对象，内部类也是如此。
                 *  但是假若内部类想引用外部类当前的对象呢？
                 *  用“外部类名”.this；的形式
                 */

                String name = "小猫";
                System.out.println(name);//--"小猫"
                System.out.println(this.name);//--"mao"
                System.out.println(Animal.this.name);//xiaodongwu

                System.out.println(Animal.this.kind);//----动物    内部类可以直接访问外部类的私有属性
            }

          abstract void add();
        }
        /*方法内部类只能在定义该内部类的方法内实例化，不可以在此方法外对其实例化。
        方法内部类对象不能使用该内部类所在方法的非final局部变量。
        因为方法的局部变量位于栈上，只存在于该方法的生命期内。
        当一个方法结束，其栈结构被删除，局部变量成为历史。
        但是该方法结束之后，在方法内创建的内部类对象可能仍然存在于堆中！
        例如，如果对它的引用被传递到其他某些代码，并存储在一个成员变量内。
        正因为不能保证局部变量的存活期和方法内部类对象的一样长，
        所以内部类对象不能使用它们。*/
        Cat cat = new Cat() {//匿名内部类，重写父类的方法，而不是创建新的方法。因为用父类的引用不可能调用父类本身没有的方法！
            @Override
            void add() {
                System.out.println(kind);//自由访问外部类属性和方法--没啥问题
            }
        };
        cat.info();
       /* 方法内部类的修饰符。
        与成员内部类不同，方法内部类更像一个局部变量。
        可以用于修饰方法内部类的只有final和abstract。*/


    }

    private void infoed() {
        System.out.println("这是外部类的infoed方法");
    }

    //非静态成员内部类
    class Dog {
        String name = "daoge";

        //static int age=110; //非静态成员内部类内不允许有任何静态声明！
        public void info() {


        }

        //对总的来说是局部内部类--   但对于Dog来说输一局 的成员内部类
        class BlackDog {


        }


    }

    //静态内部类 --可以看作外部类
    static class Cat {
        static int age = 110;

        static class BlackCat {//也相当于外部类

            public void CatInfo() {
                System.out.println();
            }


        }


    }

    {//局部内部类--构造块内
        class Sheep {

        }

    }

    public Animal() {
        class Bird {//局部内部类---构造方法中


        }
    }
public  static void Turtle(){

        //静态方法中的局部内部类
        class Hen{

            /*静态方法内的方法内部类。
            静态方法是没有this引用的，
            因此在静态方法内的内部类遭受同样的待遇，
            即：只能访问外部类的静态成员。*/
            public void print(){

                System.out.println(weight);
                //System.out.println(name);//只能访问外部类的静态成员
            }

        }

}

}


public class OuterClass {
    public static void main(String[] args) {
        Animal animal = new Animal();
        animal.info();

    }
}


```

> 几种内部类的共性：
>  A、内部类依旧是一个独立的类，编译时也会被编译成独立的.class文件，内部类的命名格式---外部类的类名$内部类名。

> B、内部类不能够用普通方式访问。可以把内部类看多是外部类的一个成员，或者方法中的一个局部变量，因此内部类可以自由地访问外部类的成员变量和方法，无论是否是private的---这里是指非静态内部类，静态内部类只能访问外部类的静态属性和方法；

> C、访问成员内部类的唯一途径就是通过外部类的对象！

> D、方法内部类的实例化的唯一途径就是在该方法中将它实例化，在别处不行，而且方法中的内部类的方法是被final修饰的，不能被修改，如果该方法内部类中的方法为抽象方法，那么该内部类也必须声明为抽象内部类；自己看代码---理解；

> E、内部类访问外部类的成员变量如果重名了那么----
> 普通的类可以用this引用当前的对象，内部类也是如此。但是假若内部类想引用外部类当前的对象呢？用“外部类名”.this；的形式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210703180328904.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> F，内部类的修饰符号-- 可以将内部类看作是外部类的属性，属性可以用什么修饰，内部类也可以，不顾内部类还可以有abstract修饰；注意--方法内部类的修饰符。
> 与成员内部类不同，方法内部类更像一个局部变量。
> 可以用于修饰方法内部类的只有final和abstract。

> G，用static修饰的内部类可以被看作是外部类，---因为静态属性、方法、类是先被加载到内存的；可以被其他类访问----但是静态方法是没有this引用的，因此在静态方法内的内部类遭受同样的待遇，即：静态内部类只能访问外部类的静态成员。

> H、方法内部类对象不能使用该内部类所在方法的非final局部变量。
> **> 因为方法的局部变量位于栈上，只存在于该方法的生命期内。当一个方法结束，其栈结构被删除，局部变量成为历史。但是该方法结束之后，在方法内创建的内部类对象可能仍然存在于堆中！例如，如果对它的引用被传递到其他某些代码，并存储在一个成员变量内。正因为不能保证局部变量的存活期和方法内部类对象的一样长，所以内部类对象不能使用它们。**

内容有些多，下次再整理接口的内部类--实现类可能会涉及到jdk1.8新引进的lambda表达式----



加油--努力！！！

------

## JAVA中的多态(一)

------

首先了解一下什么是多态---

> 它是指在父类中定义的属性和方法被子类继承之后，子类可以根据继承自父类的数据类型或方法表现出不同的行为（比如属性的扩展，方法的重写Override--方法的扩展），这使得同一个属性或方法在不同的类中具有不同的含义。
> 注意--多态只跟方法有关跟属性无关；

```java
package com.array.sort;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/2 - 07 - 02 - 9:58
 * @Description: com.array.sort
 * @version: 1.0
 */
public class Animal {//父类
    public String name;

    public void shout(){
        name="动物";
        /*因为父类种种没有定义weight属性，而是在子类中
        * 父类无法访问子类特有的属性
        * */
        //weight=100.5;
        System.out.println("我是"+name+"我会叫");
    }
    //new Animal().eat(); 父类无法访问子类方法
}

```

```java
package com.array.sort;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/2 - 07 - 02 - 9:59
 * @Description: com.array.sort
 * @version: 1.0
 */
public class Pig extends Animal {
    double weight;
    @Override
    public void shout(){
        name="猪";//虽然在Pig中没有显式定义name属性，但是他是继承自父类的属性
        System.out.println("我是"+name+"，我嗯嗯嗯的叫");
    }
    public void eat(){
        System.out.println("我喜欢吃了睡睡了吃");
    }
}

```

```java
package com.array.sort;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/2 - 07 - 02 - 10:01
 * @Description: com.array.sort
 * @version: 1.0
 */
public class Test {
    public static void main(String[] args) {
        Pig p = (Pig) new Animal();
        Animal an = new Pig();
        an.name = "佩奇";
            an.shout();
        //an.eat();eat()--子类特有的方法
       //an.weight=100.6;weight--子类特有的属性
        /*因为在编译时，不确定an到底是哪一个对象，
        //所以就不能访问子类Pig中的属性和对象，*/
        p.eat();
        p.weight=250.5;
    }
}

```

多态分为
编译时多态----方法的重载  （由参数来确定调用哪一种方法）
 运行时多态--只有在编译过之后运行才能知道具体的对象（即具体该访问哪个对象的属性和方法）
Java 实现多态有 3 个必要条件：继承（或者实现）、重写和向上转型。

```
继承：在多态中必须有继承关系--多态一定是存在于多个类中（接口的实现关系也算）才能谈多态，否则就是耍流氓。

重写：子类可以对父类中某些方法进行@Override（可以但非必须），在调用这些方法时如果父类中有该方法，子类中也有就会去调用子类的方法，如果父类中都没有那么就需要转型了；

向上转型：在多态中需要将子类的引用赋给父类对象，只有这样该引用才既能可以调用父类的方法，又能调用子类的方法。
向下转型：自动的；--可以参考数据类型的转换


```

多态的好处是什么呢？***

> 引用变量究竟指向哪一个实例对象，在编译期间是不确定的，只有运行期才能确定，提高了程序的复用性，和扩展性，符合面向对象的设计原则：开闭原则。
> 编译看左边（基类），运行看左边（基类）；无论如何都是访问基类的成员变量。



![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702105405526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



## JAVA中的多态(二)

java中关于多态可以说很重要了，重要的是思想，
多态可以延伸到 继承、抽象类、接口上，这三部分各有各的特点；
多态的应用场合:
1，父类作为方法的形参，传入一个子类对象，根据子类对象的不同返回不同的结果；

2，父类作为方法的返回值，可以返回具体的子类对象；

3，接口作为方法的参数，传入一个实现类对象，根据实现类对象不同来执行不同的代码块；

4，接口作为方法的返回值，返回具体的实现类对象；

思考：--继承与接口都可以实现的情况下该选择哪一种呢？

> 优先选择接口，因为继承是单继承的，接口可以多实现；

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702220831114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
对于类的继承--
如果白马都继承了两个父类，那么假设父类中有两个重名方法，那么白马该选择哪一个呢？为了避免这样的问题，只能继承一个；

对于接口，因为接口中的方法为没有方法体的，而且子类必须全部实现接口中的方法，如果多个接口中有重名且参数一致的方法，（重名的方法的返回值必须一致，否则报错），那么子类只需要将该方法实现一次就可以了。

（不像抽象类，如果抽象类的子类没有全部重写其父类的抽象方法，那么子类也必须变成抽象类；---即抽象类的子类可以不必全部重写抽象类中的方法）

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070222412548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



```java
package com.gavin.exercise;

/**
 * @author : Gavin
 * @date: 2021/7/2 - 07 - 02 - 21:34
 * @Description: com.gavin.exercise
 * @version: 1.0
 */
abstract class Person{
    abstract void add();
}
class Man extends  Person{
    Person per = new Man();
    @Override
    void add() {

    }
}
interface OldNokiaTelPhone{
    public  void DrawPicture(Camera camera);//抽象方法照相--接口作为方法的形参，同另一个接口的返回值必须一致，类似于方法的重载，只跟参数有关，跟返回值无关
}
interface Camera{//定义一个相机接口
    public  void DrawPicture(Camera camera);//抽象方法照相--接口作为方法的形参
    public Camera DrawVideo(Camera camera);//抽象方法录制视频--接口作为返回值
}
class Phone implements Camera,OldNokiaTelPhone{
      @Override
    public void DrawPicture(Camera camera) {
        System.out.println("手机能照像");
    }

    @Override
    public Camera DrawVideo(Camera camera) {
          Camera com= new Computer();
        return com;
    }


}
class Computer implements Camera{
    @Override
    public void DrawPicture(Camera camera) {
        System.out.println("电脑能照像");
    }

    @Override
    public Camera DrawVideo(Camera camera) {
        Camera p =  new Phone();
        return p;
    }
}

public class Ceshi {
    public static void main(String[] args) {
        Camera c=new Phone();
        Camera d=new Computer();
        //形参为接口，传入具体的实现对象，根据参数的不同返回不同的执行结果---
        c.DrawPicture(c);
        d.DrawPicture(d);

    }
}

```

## JAVA中的泛型

看代码----

```java
package baozhuang;
class Person <E>{//泛型类
    String name;
    int age;
    E grade;

    public Person(String name, int age,E grade) {
        this.name = name;
        this.age = age;
        this.grade=grade;
    }
public Person(){}

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", grade=" + grade +
                '}';
    }
}

public class Test {
    public static void main(String args[]) {
Person per = new Person("张二",12,98.8);
        System.out.println(per);
Person per0 = new Person("lisi",18,"南");
        System.out.println(per0);
    }
}
```

***泛型在实例化时才确定该数据类型的,所以泛型类中的静态方法不能含有泛型元素(静态方法先加载,如果静态方法中含有泛型元素,那么不确定是啥类型,多以就会报错,同理泛型属性为静态的也不行)***
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071513575165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 实际上泛型类不止可以指定一个泛型元素,还可以指定了多个

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715140306860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 构造器不能指定为泛型,参数里面可以有泛型

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715140256445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 我还想到了,开辟泛型数组,

```java
class Person <E,V,T,N>{//泛型类
    String name;
    int age;
    E grade;
    V v[]= new V[10];//①
    V v1[]= (V[])new Object[10];//②

}
```

> 上面代码哪一种会出错? 第一种出错,泛型数组不能直接实例化,要通过Object来转换

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715141254777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 泛型有什么用呢? 
> 泛型的使用有三种场景：
> 在类中的使用、
> 在接口中的使用、
> 在方法中的使用。 
>
> 

> > 1,提高代码的复用率。
> >  2,泛型中的类型在使用时自动，不需要进行强转，使用更广泛。

比如![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715141548474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715141617940.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> Person中的E grade 参数,不需要转型可以直接用,如果指定为Object类型----

```java
class Person <Object>{
    String name;
    int age;
    Object grade;
    public Person(String name, int age, Object grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
      public Object getGrade() {
        return grade;
    }
  }

public class Test {
    public static void main(String args[]) {
Person per = new Person("张二",12,98.8);
        Object grade = per.getGrade();

    }
}
```

```java
class Person <E>{
    String name;
    int age;
    E grade;
    public Person(String name, int age, E grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
      public E getGrade() {
        return grade;
    }
  }

public class Test {
    public static void main(String args[]) {
Person per = new Person("张二",12,98.8);
        Object grade = per.getGrade();


    }
}
```

> 当我们不指定泛型类型时,实际上grade是Object来接收的,在普通类中指定泛型类型也可以,但是没有必要;
>
> 这是普通类中的泛型,但是当我们在集合中指定泛型时----

```java
import java.util.ArrayList;

public class Test {
    public static void main(String args[]) {
        ArrayList<Integer>arrayList= new ArrayList<Integer>(10);
arrayList.add(10);
//arrayList.add("abc");

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715143759150.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 指定泛型类型的好处是---可以限定添加元素额类型,而且取用的时候也很方便,不需要转型;

```java
import java.util.ArrayList;

public class Test {
    public static void main(String args[]) {           
        ArrayList<Integer> arrayList = new ArrayList<Integer>(10);
        arrayList.add(10);
        Integer integer = arrayList.get(0);
        ArrayList List = new ArrayList<>(10);
        List.add(10);
        Object o = List.get(0);
    }
}
```

> ***泛型类的继承---***

```java
class Animal<E>{
    E e;
    
}
class Cat<String > extends Animal {//子类指定泛型类型,
    String name;

    public Cat(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

class Dog<E> extends Animal<E>{//子类没有指定泛型类型
    E weight;

    public Dog(E weight) {
        this.weight = weight;
    }

    public E getWeight() {
        return weight;
    }
}
class Sheep extends  Animal{//子类继承父类,但是没有泛型,就是一个普通的继承,类中不能使用父类的泛型
    String name;

    public Sheep(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}


public class Test {
    public static void main(String args[]) {
        Cat cat = new Cat("小花");
        Object name = cat.getName();//没有指定泛型类型,只能用Object接收
        Cat<String> cat1= new Cat("xiaohua");
        String name1 = cat1.getName();//指定泛型类型--String 接收
        Dog dog= new Dog(18.5);
        Object weight = dog.getWeight();//没有指定泛型类型,weight只能用Object接收
        Dog<Double> dog1 = new Dog(18.5);//子类没有指定泛型类型, 可在实例化的时候指定,这里指定为double
        Double weight1 = dog1.getWeight();
        Sheep sheep= new Sheep("小样");
        String name2 = sheep.getName();
    }
}
```

*****泛型接口其实和泛型类一样,知识extends换成了implements,*****

```java
public  interface MyName<T>{
  T getData();
  }
  //实现接口时，可以选择指定泛型类型，也可以选择不指定， 如下：
 // 指定类型：
  public class Interface1 implements MyName<String> {
  private String text;
     @Override
     public String getData() {
       return text;
     }
   }
  //不指定类型：
  publicclass Interface1<T> implements MyName<T> {
  private T data;
     @Override
     public T getData() {
       return data;
     }
   }
```

> 是不是跟继承一样,不再赘述;
>
> ***下面来看泛型方法--***

```java
class Animal<E,V> {
    E e;
    
}

class Dog<E,V> extends Animal<E,V>{//子类没有指定泛型类型
    E weight;

    public Dog(E weight) {
        this.weight = weight;
    }

    public E getWeight() {
        return weight;
    }
    public E getVoice(E e){//这不是泛型方法
        
        return  e ;
    }
    public void getVoice(E e,V v){//这不是泛型方法
        
    }
    public <T>fanxingMethod(T t){//这才是泛型方法
    return t;
}
   public static <T>fanxingMethod1(T t){//这才是泛型方法
    return t;
}
}
```

泛型方法--带泛型的方法且和当前的类的泛型无关(即使类没有泛型也可以)
使用泛型方法的好处是可以根据接收参数类型的不同做一些其他事情;

> 静态泛型方法---在调用的时候才确定泛型的类型,跟类的泛型不一样哦! 类的泛型在实例化的时候才确定'

​                                                                                                                                                                                                                              



除了以上泛型的知识,还有一个限定泛型类型范围的方式--

在泛型范围的限制上有两种场合：

①在类上限制类型：

```java
/**
 * @author : Gavin
 * @date: 2021/7/15 - 07 - 15 - 15:21
 * @Description: baozhuang
 * @version: 1.0
 */
interface Animal1{}
class Dog1 implements Animal1{}
class Cat1 implements Animal1{}
class Thing<T extends Animal>{//泛型T限定为Animal的实现类---Dog和Cat
    T t;
}
   
public class qitian {
    public static void main(String[] args) {
        Thing<Dog> p = new Thing<>();
        //Thing<String>pp= new Thing();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210715152457323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
在泛型的限定中，无论是接口的impliments还是extends,这样限定的泛型T，只能是Animal的实现类(子类)。

②在使用中限制类型：

这种限制就是使用关键字"?"做为泛型中的通配符；

```java
package baozhuang;

/**
 * @author : Gavin
 * @date: 2021/7/15 - 07 - 15 - 15:21
 * @Description: baozhuang
 * @version: 1.0
 */
interface Animal1{}
class Dog1 implements Animal1{}
class Cat1 implements Animal1{}
class Thing<T extends Animal1 >{//泛型T限定为Animal1的子类---Dog和Cat
    T t;
}
class eatThing{}
class Fruit extends eatThing{}
class Apple extends Fruit{}
class Pear extends Fruit{}
class AAA<E extends eatThing>{

}
public class qitian {
    public static void main(String[] args) {
        Thing<? extends Animal1> p = new Thing<>();//指定了泛型只能为Animal1中的子类
        Thing<Dog1> pp = new Thing<>();
//在使用中限制类型：
      AAA<?super Pear>aaa=new AAA<Fruit>();//泛型只能为Pear的父类，或父类的父类；
        AAA<?super Pear>BBB=new AAA<eatThing>();//泛型只能为Pear的父类，或父类的父类；

    }
}

```

这样的好处是什么呢?------

反过来想一下如果没有泛型,那么我就需要写好几个重复性的代码,如下所示,
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021071516025477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

分开写的话要写好几个对象(Dog,Cat,如果有好几千个甚至上万个...),而其实在用的时候通过对象p的转型就可以完成,
提高代码复用性;

## JAVA中的异常Exception

真是应了老罗那句话--少罗嗦，直接看东西

```javaimport java.util.scanner;
/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 14:54
 * @Description: PACKAGE_NAME
 * @version: 1.0
 */
public class test {
    public static void main(String[] args) {
        System.out.println("请输入一个数：");
        Scanner scanner = new Scanner(System.in);
       
            int x = scanner.nextInt();
            System.out.println("请输入第二个数");
            int y = scanner.nextInt();
            System.out.println(x/y);
        
        System.out.println("qwewqeqwqwewqwqe");
    }
}

```

先来看上面的代码，如果我在控制台输入的不是程序要求的，则程序就会报错而终止---如以下操作--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705151058220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)第一个数输入的就不符合要求，后面就没有机会输入第二个数--即后续代码根本不执行；
如果没有学习异常的知识，我们通常会这样处理

if else 来进行处理，但是由于异常可能有很多种，

```java
import java.util.Scanner;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 14:54
 * @Description: PACKAGE_NAME
 * @version: 1.0
 */
public class test {
    public static void main(String[] args) {
        int x = 0;
        int y = 0;

        System.out.println("请输入一个数：");
        Scanner scanner = new Scanner(System.in);
        if (scanner.hasNextInt()) {
            x = scanner.nextInt();
        }
        System.out.println("请输入第二个数");
        if (scanner.hasNextInt()) {
            y= scanner.nextInt();
            if(y!=0){
                System.out.println(x/y);
            }else{
                System.out.println("异常");
            }
        }
        System.out.println("qwewqeqwqwewqwqe");
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705151719810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)即使用上述方法处理了异常，也未达到想要的效果
所以java'中提供了处理异常的类--
exception

用try catch捕获异常

```java
import java.util.Scanner;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 14:54
 * @Description: PACKAGE_NAME
 * @version: 1.0
 */
public class test {
    public static void main(String[] args) {
        int x = 0;
        int y = 0;
        try {
            System.out.println("请输入一个数：");
            Scanner scanner = new Scanner(System.in);
            if (scanner.hasNextInt()) {
                x = scanner.nextInt();
            }
            System.out.println("请输入第二个数");
            if (scanner.hasNextInt()) {
                y = scanner.nextInt();
                if (y != 0) {
                    System.out.println(x / y);
                } else {
                    System.out.println("异常");
                }
            }
        } catch (Exception e) {

        }
        System.out.println("qwewqeqwqwewqwqe");
    }
}

```

> 处理异常的几种方式：
>  1，干脆不处理 
>  2，捕获异常，但是不处理 
>  3，抛出异常，交由虚拟机处理 
>  4，捕获后将异常抛出

下面分别来看代码---

> 1，不处理异常--程序中断，后面代码不会执行--（这里指的是qwe....的输出）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705152951803.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 2，捕获异常，但是不处理--处理的话会有相应的提示等；
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705153451189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
public class test {
    public static void main(String[] args)throws  Exception {
        try {
            System.out.println("请输入一个数：");
            Scanner scanner = new Scanner(System.in);
            int x = scanner.nextInt();
            System.out.println("请输入第二个数");
            int y = scanner.nextInt();
            System.out.println(x / y);
        } catch (Exception e) {
            System.out.println("输入错误");
        }
        System.out.println("qwewqeqwqwewqwqe");
    }
}

```

这里做一个简单的处理---给予提示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705154008999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



我们可以看到qwe...执行了

> 
>
>  3，抛出异常，交由虚拟机处理

```java
public class test {
    public static void main(String[] args)throws  Exception {

            System.out.println("请输入一个数：");
            Scanner scanner = new Scanner(System.in);
            int x = scanner.nextInt();
            System.out.println("请输入第二个数");
            int y = scanner.nextInt();
            System.out.println(x / y);

        System.out.println("qwewqeqwqwewqwqe");
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705154228494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



实际上跟第一种很类似，也会中断，只不过在有些代码中会要求你必须处理异常才能运行，所以还是有区别的

> 4，捕获后将异常抛出

```java
import java.util.Scanner;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 14:54
 * @Description: PACKAGE_NAME
 * @version: 1.0
 */
public class test {
    public static void main(String[] args) throws Exception {
        try {
            System.out.println("请输入一个数：");
            Scanner scanner = new Scanner(System.in);
            int x = scanner.nextInt();
            System.out.println("请输入第二个数");
            int y = scanner.nextInt();
            System.out.println(x / y);
        } catch (Exception e) {
            throw e;
        }


        System.out.println("qwewqeqwqwewqwqe");
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705154628336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)效果也是程序中断

> 总结--要想执行后续代码而程序不中断，要捕获异常，最好做一下处理而不是接着把异常抛给上级；
> 最后说一下finally， 结合try catch'进行使用

finally代码块除了 System.exit(0);能使其不被执行，其他任何情况下都要被执行---

finally代码块主要用于 关闭数据库连接、io 、 socekt资源等的关闭操作；

**自定义异常**

自定义异常一般很少用,程序提供的一场类基本已经够用了,但还是要学习自定义异常;

在自定义异常之前我们来看一下其他异常类是怎么写的---照葫芦画瓢还不会吗!!!!

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705204044789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

以上异常继承自运行时异常,
类里面有---
1,序列化号-----这个先不去管他,后面学到序列化自然会明白,先模仿着写;
2,构造方法(无参的)
3,有参构造.--参数可用于提示信息--(by zero 是不是很熟悉)
4,父类中有很多构造方法,这里只用几个常用的

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705204311629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

模仿开始---

```java
package com.test.except;

/**
 * @author : Gavin
 * @date: 2021/7/5 - 07 - 05 - 20:25
 * @Description: com.test.except
 * @version: 1.0
 */
public class GenderException extends RuntimeException {
    static final long serialVersionUID = -7034939L;
    public GenderException() {
    }

    public GenderException(String message) {
        super(message);
    }
}
```

总结--自定义异常特点

> 1,继承异常类 
> 2,写构造方法 
> 3,序列化号(可以不写,如果用不到的话;)

是不是很简单,下面我们用一下这个异常---

```java
package Person;

import com.test.except.GenderException;

/**
 * @author : Gavin
 * @date: 2021/7/5 - 07 - 05 - 20:23
 * @Description: Person
 * @version: 1.0
 */
public class Student {
    String name;
    int age;
    String gender;



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

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        if (gender=="男"|gender=="女")
        this.gender = gender;
        else{
       throw new GenderException("性别录入错误");//在这里抛异常
        }
    }

    public Student() {
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                '}';
    }
}

```

测试类---

```java
package Person;

import com.test.except.GenderException;

/**
 * @author : Gavin
 * @date: 2021/7/5 - 07 - 05 - 20:26
 * @Description: Person
 * @version: 1.0
 */
public class Test {
    public static void main(String[] args) {
        Student per = new Student();
        try {
            per.setGender("未知");
            System.out.println(per.gender);
        } catch (GenderException e) {

            e.printStackTrace();
        }
    }
}

```

打印异常信息----

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
com.test.except.GenderException: 性别录入错误
	at Person.Student.setGender(Student.java:42)
	at Person.Test.main(Test.java:16)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:64)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:564)
	at com.intellij.rt.execution.application.AppMainV2.main(AppMainV2.java:131)

Process finished with exit code 0
```

我们可以继续测试检查异常类
自定义检查异常类---感兴趣的可以自己试一下;





------

## JAVA中throw与throws



> 似乎少罗嗦成了手机行业的一句标准---大体意思是---直接看配置--这不正是手机圈和电脑圈的人一直吵着要拿出来给消费者看得东西吗，程序员界是不是也流行少罗嗦，直接看代码？
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705173816892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
> 今天看到这张图---头大了，似乎发亮的多少决定了程序员的的等级--我变凸了，也变强了----

还是说正题吧---

```java
/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 14:54
 * @Description: PACKAGE_NAME
 * @version: 1.0
 */
class a {
    public void add() {
        try {
            System.out.println(10 / 0);//运行时异常
        } catch (Exception e) {
            throw e;//抛出异常
        }
    }

}

class b {
    public void byZero() throws Exception{

            new a().add();
    }
}

public class test {
    public static void main(String[] args) {
        try {
            new a().add();
        } catch (ArithmeticException e) {
            System.out.println("被除数为0 的错误");
        }
    }
}
```

除了自己处理外，原来异常还可以一级一级向上抛，遵循谁调用谁处理的原则，如果调用处也不处理，那么最终会一直向上倒腾到main()方法，入股连main方法也不愿处理---那只有JVM来接手这件事了；

关于异常中throw和throws关键字的区别--
1，throw 位于catch之中，而且后面必须接异常的对象--抛出异常
2，throws 位于方法的声明处--方法中可能会出现异常，调用者可以对此异常进行处理，也可以再次抛给上一级，直至虚拟机；



## 枚举类

枚举定义:

在数学和计算机科学理论中，一个集的枚举是列出某些有穷序列集的所有成员的程序，或者是一种特定类型对象的计数。这两种类型经常（但不总是）重叠。 是一个被命名的整型常数的集合，枚举在日常生活中很常见，例如表示星期的SUNDAY、MONDAY、TUESDAY、WEDNESDAY、THURSDAY、FRIDAY、SATURDAY就是一个枚举。

**用java语言实现枚举**

我们学过java的都知道,java是面向对象的一门语言,巧了世界上还就有一些食物能归纳成的对象能用脚趾头数过来,比如季节,星期,月份等,

为了简单起见,这里以周为例-------作为枚举性质的类的演示;

```java
import java.util.Scanner;

final class Week {
    private final String WEEKNAME;//季节名
    private final String WEEKDESC;//季节描述

    //定义构造方法---外界不能随便修改
    private Week(String WEEKNAME, String WEEKDESC) {
        this.WEEKNAME = WEEKNAME;
        this.WEEKDESC = WEEKDESC;
    }

  

    public String getWEEKDESC() {
        return WEEKDESC;
    }

    public static Week getMON() {
        return MON;
    }

    public static Week getTUE() {
        return TUE;
    }

    public static Week getWED() {
        return WED;
    }

    public static Week getTHIR() {
        return THIR;
    }

    public static Week getFRI() {
        return FRI;
    }

    public static Week getSAT() {
        return SAT;
    }

    public static Week getSUN() {
        return SUN;
    }

    //定义实例----周一到周日
    private final static Week MON = new Week("周一", "工作日");
    private final static Week TUE = new Week("周二", "情人节");
    private final static Week WED = new Week("周三", "工作日");
    private final static Week THIR = new Week("周四", "工作日");
    private final static Week FRI = new Week("周五", "工作日");
    private final static Week SAT = new Week("周六", "休息日");
    private final static Week SUN = new Week("周日", "休息日");
}

public class meiju {
    public static void main(String[] args) {
        System.out.println("请输入要查询的信息---");
        Scanner sc = new Scanner(System.in);
        int choice = sc.nextInt();
        switch (choice) {
            case 1:
                System.out.println(Week.getMON().getWEEKDESC());
                break;
            case 2:
                System.out.println(Week.getTUE().getWEEKDESC());
                break;
            case 3:
                System.out.println(Week.getWED().getWEEKDESC());
                break;
            case 4:
                System.out.println(Week.getTHIR().getWEEKDESC());
                break;
            case 5:
                System.out.println(Week.getFRI().getWEEKDESC());
                break;
            case 6:
                System.out.println(Week.getSAT().getWEEKDESC());
                break;
            case 7:
                System.out.println(Week.getSUN().getWEEKDESC());
            default:
                System.out.println("九天揽月");

                sc.close();
        }
    }

}

```

看起来很繁琐是不是,于是在jdk1.5之后就出现了枚举类

把上述代码换成枚举类看看有什么区别吧----

**java枚举类**

```java
import java.util.Scanner;
enum Week {
//定义实例----周一到周日
MON ("周一", "工作日"),
TUE("周二", "情人节"),
 WED  ("周三", "工作日"),
THIR("周四", "工作日"),
 FRI ("周五", "工作日"),
SAT ("周六", "休息日"),
 SUN ("周日", "休息日");

private final String WEEKNAME;//季节名
    private final String WEEKDESC;//季节描述

    //定义构造方法---外界不能随便修改
    private Week(String WEEKNAME, String WEEKDESC) {
        this.WEEKNAME = WEEKNAME;
        this.WEEKDESC = WEEKDESC;
    }

    public String getWEEKDESC() {
        return WEEKDESC;
    }

    public static Week getMON() {
        return MON;
    }

    public static Week getTUE() {
        return TUE;
    }

    public static Week getWED() {
        return WED;
    }

    public static Week getTHIR() {
        return THIR;
    }

    public static Week getFRI() {
        return FRI;
    }

    public static Week getSAT() {
        return SAT;
    }

    public static Week getSUN() {
        return SUN;
    }



}

public class meiju {
    public static void main(String[] args) {
        System.out.println("请输入要查询的信息---");
        Scanner sc = new Scanner(System.in);
        int choice = sc.nextInt();
        switch (choice) {
            case 1:
                System.out.println(Week.getMON().getWEEKDESC());
                break;
            case 2:
                System.out.println(Week.getTUE().getWEEKDESC());
                break;
            case 3:
                System.out.println(Week.getWED().getWEEKDESC());
                break;
            case 4:
                System.out.println(Week.getTHIR().getWEEKDESC());
                break;
            case 5:
                System.out.println(Week.getFRI().getWEEKDESC());
                break;
            case 6:
                System.out.println(Week.getSAT().getWEEKDESC());
                break;
            case 7:
                System.out.println(Week.getSUN().getWEEKDESC());
            default:
                System.out.println("九天揽月");

                sc.close();
        }
    }

}

```

可以发现将
1,实例化的对象放在了枚举类中的开头;
2,枚举对象不用new的形式而是直接写该对象的构造方法即可;
3,枚举对象之间用逗号隔开,最后一个对象用的是分号;

> enum类可以定义有参构造,也可以定义无参构造---这点跟普通类是一样的,只不过是实例化时候会有些区别;

例如--

enum类继承的是Java.lang.enum类

**枚举类中的一些其他方法**

```java
 Week mon = Week.valueOf("MON");//返回当前枚举类的
        System.out.println(mon);
        Week[] values = Week.values();//获得所有枚举对象,以数组的形式
        for (Week w:values
             ) {
            System.out.println(w);
        }

```



**枚举关键字**

```java
package org_chwenyu_learn;
public class Meiju {
enum Week {
Sun. Mon.Tue.Wed.Thu.Fri.Sat }
// 枚举类型语法格式--enum 枚举名{元素 1.元素 2};
// 枚举时一定要将所有元素全部列举。
public static void main(String[] args) {
////////////////////////
// 枚举名 对象名= 枚举名.枚举元素
Week c1 = Week.Sat;
switch (c1) {
case Sat: System.out.println("今天周六"); break;
default: { System.out.println("今天上学"); } }
Week[]allWeek=Week.values();//遍历枚举对象
for (Week aWeek:allWeek) { System.out.print(aWeek+"."); }
System.out.println("****************************");
System.out.println(c1.getDeclaringClass());//返回 c1 所在的枚举名
System.out.println(c1.hashcode());//返回枚举常量的哈希码
System.out.println(c1.name());//返回枚举常量的名称（在定义时声明的）
System.out.println(c1.ordinal());//返回枚举常量在枚举声明中的位置.序数从 0 开始
System.out.println(c1.toString());//返回枚举常量的名称。}
```

**Enum 类和 enum 关键字的区别**

使用关键字定义枚举，实际上是继承了 Enum 类；
Class Enum<E extends Enum<E>> 
java.lang.Enum<E>

在含有main方法中不能点工艺枚举类为public

枚举与接口

既然枚举可以是一个类,那么就可以像普通类那样去实现接口;

```java
package enumTest;

interface DescribeSeason {
    void desc();
}
enum WEEKEND implements DescribeSeason {
    //定义实例----周一到周日
    MON {
        @Override
        public void desc() {
            System.out.println("周一");
        }
    },

    TUE() {
        @Override
        public void desc() {

            System.out.println("周二");
        }
    },

    WED {
        @Override
        public void desc() {
            System.out.println("周三");
        }
    },

    THIR {
        @Override
        public void desc() {
            System.out.println("周四");
        }
    },
    FRI {
        @Override
        public void desc() {
            System.out.println("周五");
        }
    },
    SAT {
        @Override
        public void desc() {
            System.out.println("周六");
        }
    },
    SUN {
        @Override
        public void desc() {
            System.out.println("周日");
        }
    };
}

```

**注意:**

- 枚举类的构造方法是不能用 public 声明的， 因为外部不能够调用枚举的构造方法
- 一旦定义了枚举构造，那么就要使用枚举的构造方法



**枚举与接口**

EnumMap

EnumMap的基本信息---

public class EnumMap<K extends Enum<K>,V> extends AbstractMap<K,V>
implements Serializable, Cloneable一个专门Map实现 与枚举类型键一起使用。 枚举映射中的所有密钥必须来自创建映射时明确或隐式指定的单个枚举类型。 枚举地图在内部表示为数组。

> 1,在实例化EnumMap的时候要指定枚举类型---即导入枚举的类信息

代码演示---

```java
public class MeijuLeiji {
    public static void main(String[] args) {
        EnumMap<WEEKEND,Integer> we=new EnumMap<WEEKEND, Integer>(WEEKEND.class);//指定枚举类型
        we.put(WEEKEND.MON,1);
        we.put(WEEKEND.TUE,2);
        we.put(WEEKEND.WED,3);
        we.put(WEEKEND.THIR,4);
        we.put(WEEKEND.FRI,5);
        we.put(WEEKEND.SAT,6);
        we.put(WEEKEND.SUN,7);
        Set<Map.Entry<WEEKEND, Integer>> entries = we.entrySet();
        for (Map.Entry e:entries
             ) {
            System.out.println(e.getValue());
        }
 EnumMap<WEEKEND, Integer> clone = we.clone();//返回该EnumMap的浅拷贝
        System.out.println(clone.size());//返回EnumMap类集大小----7
        clone.putAll(we);//将另一个类集添加到该类集之中;
        System.out.println(clone.size());//返回EnumMap类集大小---因为有重复值,所以大小仍为7
    }
}

```

小结--- 

> 1,EnumMap像其他大部分类集一样,不同步,线程非安全.多线程访问的时候要注意在外部进行同步;  
>
> 2, 如果没有此类对象存在，则应使用**Collections.synchronizedMap(java.util.Map<K,
> V>)**方法“包装”地图。 这**最好在创建时完成**，以防止意外的不同步访问

```java
 Map<EnumKey, V> mapEnum
         = Collections.synchronizedMap(new EnumMap<EnumKey, V>(...)); 
```

Enumset

先看图----
![在这里插入图片描述](https://img-blog.csdnimg.cn/978c7e036e784a46952841058d8158c4.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

Enumset类的基本信息---

public abstract class EnumSet<E extends Enum<E>>
extends AbstractSet<E> implements Cloneable, Serializable

一个专门Set实现与枚举类型一起使用。 **枚举集中的所有元素都必须来自创建集合时明确或隐式指定的单个枚举类型**。

 枚举集在内部表示为位向量。 这种表示非常紧凑和高效。 这个类的空间和时间表现应该足够好，可以将其作为基于传统int的“位标志”的高品质，类型安全的替代品使用 。 即使批量操作（如containsAll和retainAll ）也应该很快运行

Enumset是一个抽象类,只能通过子类或者借助方法来实例化;所以会出现上图的错误提示;

```java
public class MeijuLeiji {
    public static void main(String[] args) {
    //创建一个包含指定元素类型中所有元素的枚举集。 
              EnumSet<WEEKEND> weekends= EnumSet.allOf(WEEKEND.class);
        boolean add = weekends.add(WEEKEND.TUE);
        weekends.add(WEEKEND.FRI);
        weekends.add(WEEKEND.SUN);//添加元素
        System.out.println(weekends.size());//类集大小
        EnumSet<WEEKEND> clone = weekends.clone();//克隆类集
        clone.remove(WEEKEND.FRI);//移除
        System.out.println(clone.spliterator());
        
        //创建一个最初包含指定元素的枚举集。
EnumSet<WEEKEND>weekendEnumSet=EnumSet.of(WEEKEND.MON,WEEKEND.TUE,WEEKEND.THIR);
        Iterator<WEEKEND> iterator = weekendEnumSet.iterator();
        while(iterator.hasNext()) {
            System.out.println(iterator.next());
        }

    System.out.println("---------------------------------------------");
        //创建与指定枚举集具有相同元素类型的枚举集，最初包含此类型的所有元素，该元素 不包含在指定的集合中。
        //通俗的讲就是该方法是剔除weekendEnumSet之后剩下WEEKEND枚举之中所有的
        EnumSet<WEEKEND>weekends1=EnumSet.complementOf(weekendEnumSet);
        Iterator<WEEKEND> iterator1 = weekends1.iterator();
        while(iterator1.hasNext()) {
            System.out.println(iterator1.next());
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/5b9fc3b871fc496a85d4ee112f708f9b.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



小结--- 

> 1,EnumSet像其他大部分类集一样,不同步,线程非安全.多线程访问的时候要注意在外部进行同步;  
>
> 2, 如果没有此类对象存在，则应使用**Collections.synchronizedSet(java.util.Set<T>)**方法“包装”地图。 这**最好在创建时完成**，以防止意外的不同步访问

```java
  Set<MyEnum> s = Collections.synchronizedSet(EnumSet.noneOf(MyEnum.class)); 
```

**枚举类主要的应用**

1,如果一种数据只有有限的个数,可以用枚举类来指定数据的输入范围-----
例如------性别,登录状态等一些实例对象可数的一些数据;

```java
public enum Gender {
    男,女;
}
class Person{
    String name;
    Gender sex;

    public Person(String name, Gender sex) {
        this.name = name;
        this.sex = sex;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setSex(Gender sex) {
        this.sex = sex;
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d6e775ddd4cb495586c35e5993835f57.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)这样在设置性别得时候只能选择两种数据----男,女

2,switch对枚举的支持;

```java
import java.util.Scanner;
import static enumTest.Gender.男;

/**
 * @author : Gavin
 * @date: 2021/8/24 - 08 - 24 - 11:27
 * @Description: enumTest
 * @version: 1.0
 */
public class Test {
    public static void main(String[] args) {
        Person per = new Person("张三", 男);//静态导入
        Person per1 = new Person("李二狗", Gender.女);
        String sexs = null;
        Scanner sc = new Scanner(System.in);
        sexs = sc.next();
        Gender sex = Gender.valueOf(sexs);
        switch (sex) {//switch 对int,short,byte,enum,String类型的支持
            case 男:
                System.out.println("张三");
                break;
            case 女:
                System.out.println("李二狗");
                break;
        }
        sc.close();
    }
}

```

> 注解的作用是用来简化代码或者帮助程序完成某些功能的一种指令

## 注解类

**doc注解---文档注解**

javadoc注解---

```java
/**
 * @author : Gavin
 * @date: 2021/8/24 - 08 - 24 - 14:44
 * @Description: Doczhujie
 * @version: 1.0  指定类的版本
 */
public class Personper {
    private  String name;
    private int age;
    /**
     * @deprecated 构造方法
     * @param name 姓名
     * @param age  年龄
     *
     */
    public Personper(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

```

```java
/**
 * @author : Gavin
 * @date: 2021/8/24 - 08 - 24 - 14:33
 * @Description: Doczhujie
 * @version: 1.0
 */
public class Student extends Personper {
    private  String name;
    private int age;
/**
 * @deprecated 这是get方法,用于获取学生姓名
 * @return String
 ** */
    public String getName() {
        return name;
    }
    /**
     * @deprecated  用于设置学生姓名
     * @param name 学生姓名
     *
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * @inheritDoc

     */
    public Student(String name, int age) {
        super(name, age);

    }

    /**
    @deprecated 这是一个学习的方法---已过时
    @param  subject 学生学习的科目
    @return String   返回学生学习的科目

    * */
   public String Study(String subject){
        return subject;
    }

    /**
     * @exception  RuntimeException
     * @exception IndexOutOfBoundsException
     * @return int
     */
    public int getAge(){
       if(age>120){
           throw  new RuntimeException();
       }
       if (age<0){
           throw new IndexOutOfBoundsException();
       }
       return age;
}


}

```

这是文档注释,我们在看源代码的时候经常也会看到文档注释,比如---
![在这里插入图片描述](https://img-blog.csdnimg.cn/f7f947e6ae4549c7a3a04392dc10b5ad.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
注解的格式@+javadoc相关关键字------以下是doc注解关键字

![在这里插入图片描述](https://img-blog.csdnimg.cn/d7ff440546b942c38498ce9c62e1ed7c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
doc注解的作用是能够帮助程序员生成doc文档,另一个是能够帮助另一个程序员更高的理解自己写的代码;
![在这里插入图片描述](https://img-blog.csdnimg.cn/61efbcb137414c92896467e48dfc0d85.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)上图为生成注解后的文档形式---
为了避免生成后的文档为乱码需要指定生成为utf-8格式;
![在这里插入图片描述](https://img-blog.csdnimg.cn/d6b6e9f7d355486f80dd7cda3319f23c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

**自定义注解类**

首先观看一下源码中的注解格式---
![在这里插入图片描述](https://img-blog.csdnimg.cn/5650fafc0c79487b8cfbfe8d8123dda2.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)格式----@interface  

![在这里插入图片描述](https://img-blog.csdnimg.cn/e26a4a4d311b4e38a6c14d184619889e.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
格式------注解类中的内容---
可以有元素,也可以没有

自定义注解类----

```java
public @interface Gavin {
    String[]name();//参数1
    int[]age();//参数2
}

```

```java
public class GavinTest {
    @Gavin(name={"李二狗","张三","王五"},age={16,28,19})
    public void show(){
        System.out.println("请开始你的表演");
    }

}
```

由上述代码可以发现注解的参数的应用----
1,可以理解为把参数作为某个类型的数组(可变参数)
格式是   参数名={},如果是多个参数,之间用逗号隔开;
![在这里插入图片描述](https://img-blog.csdnimg.cn/7e1e0578a698421e9ff8870559cd0b0d.png)除此之外还可以指定参数的默认值

```java
public @interface Gavin {
    String[]name() default "李二狗";//参数1
    int[]age() default 18;//参数2
}
```

可以通过反射来获得该类的方法,参数和注解等信息;

**常用的注解**

最常见的是 覆写时候用的覆写的注解---Override

```java
public class Gavinper extends GavinTest {
    @Override
    public void show(){
       
    }
 }
```

另一个是压制警告注解
 @SuppressWarnings("unused")
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c9466c3204849a2b11d73df3ab6e005.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
还有一个常用的注解----过时的,或者废弃的

我们在使用某些方法是经常会看到某方法加了条横杠---

![在这里插入图片描述](https://img-blog.csdnimg.cn/f465206aac23483e98ee00129ac34ad7.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
来看下加横杠方法色注解
![在这里插入图片描述](https://img-blog.csdnimg.cn/dfbf39d91b1240bcbfbcc0001531f3cd.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
在深入学习之后---spring boot,     ibatis  等基本就是用的注解方式来完成的;

**元注解**

元注解又成为注解的注解-----即修饰注解的一种注解,其本质还是一种注解;

元注解的分类--

元注解分为  以下几种
@Retention
@Target
@Documented
@Inherited
一个一个拆开来看---

**@Retention-------控制注解的生命周期**

先从源代码中找到他的痕迹

![在这里插入图片描述](https://img-blog.csdnimg.cn/dcde283ea672466abc8aa863bf36df49.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
可以看到该注解要传入一个参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/a5a19eb6cb4541d284c241c430ba1687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
**继续探究-----RetentionPolicy类**

![在这里插入图片描述](https://img-blog.csdnimg.cn/bcbba33208f148c1a85614277adec439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)该类是一个枚举类,里面只有三个参数

> SOURCE------ 
> 标记为@Retention(RetentionPolicy.SOURCE) 表示被修饰的注释将被编译器丢弃。

>  CLASS----- 
>  标记为@Retention(RetentionPolicy.CLASS)  表示被修饰的注释将由编译器记录在类文件中 ，但在运行时不需要被VM保留,这是默认值 行为。

>  RUNTIME------- 
>  标记为@Retention(RetentionPolicy.RUNTIME)  注释将由编译器和记录在类文件中在运行时由VM保留，因此它们可能被反射读取。 另参考-- java.lang.reflect.AnnotatedElement

这三种注释的特点-----
简单的说就是负责被修饰的注解的生命周期

**RESOURCE---**
负责在编译阶段,在源文件中有效(即源文件保留),编译器直接丢弃这种策略的注释，在.class文件中不会保留注解信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/57f5aa7cf14d47ac9a73318e473a52bc.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/013d72d42ad44f79a4745726a70c020f.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



**CLASS---**
在class文件中有效(即class保留)，保留在.class文件中，但是当运行Java程序时，他就不会继续加载了，不会保留在内存中，JVM不会保留注解。如果注解没有加Retention元注解，那么相当于默认的注解就是这种状态。
![在这里插入图片描述](https://img-blog.csdnimg.cn/4377a1ece03843279f33d65343636fc0.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
最后一种---
**RUNTIME-----**

在运行时有效(即运行时保留),当运行 Java程序时，JVM会保留注释，加载在内存中了，那么程序可以通过反射获取该注释。
![在这里插入图片描述](https://img-blog.csdnimg.cn/782d32a230dc45a5934a4670dcd227c4.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
**简单的总结一下,**

RESOURCE 是不在类的字节码文件中,自然也不会在内存中;
CLASS 是在类的字节码文件中,但也不会加载到内存中;
RUNTIME是加载在字节码文件中,也会加载到内存中,因此通过反射要想获得类的信息,需要标记为RUNTIME
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d61dc0cd0da4d57a03cb5719be7f7cb.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

**@TARGET-------控制注解的修饰范围**

来看一以下自定义的注解可以修饰的范围-------

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
@Retention(RetentionPolicy.RUNTIME)
public @interface Gavin {
    String[]name() default "李二狗";//参数1
    int[]age() default 18;//参数2
}

```

```java
@Gavin()//修饰类
public class Gavinper extends GavinTest {
	@Gavin()//修饰属性
    int num=0;
  @Gavin()//修饰构造方法
  public Gavinper() {
	}
    @Gavin()//修饰方法
    public void show(){
      
    }
 }

```

如果想要定义一个注解,只能修饰类或者属性或者方法等,那么就需要Target
来指定该注解修饰的范围

![在这里插入图片描述](https://img-blog.csdnimg.cn/b3db12d1d7824d2eb0b2e95b978e84da.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
参考源代码中的信息可以发现,Target需要传入一个数组参数,用于指定修饰的范围
![在这里插入图片描述](https://img-blog.csdnimg.cn/0042a46ed4644c838b2c29c9164ffc75.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
ElementType为枚举类,

有如下信息------------------------------------------

> TYPE--------修饰类 
> METHOD---------修饰方法 
> PARAMETER-------修饰参数
> CONSTRUCTOR-------修饰构造方法 
> LOCAL_VARIABLE------修饰局部变量
> ANNOTATION_TYPE,------修饰注解 
> PACKAGE,----修饰包  
> TYPE_PARAMETER,-----修饰类参数 
> TYPE_USE----修饰引用类型

举个例子---
![在这里插入图片描述](https://img-blog.csdnimg.cn/c1d000405db641128570b99a46c67572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/91777711e4894ddeb6ab950f4ff74840.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
当然也可以指定多个修饰范围,取合集;

```java
@Target({ElementType.LOCAL_VARIABLE,ElementType.CONSTRUCTOR,ElementType.METHOD,ElementType.ANNOTATION_TYPE,ElementType.FIELD,ElementType.TYPE})
public @interface Gavin {
    String[]name() default "李二狗";//参数1
    int[]age() default 18;//参数2
}

```

```java
@Gavin(name = {"李二狗", "张三", "王五"}, age = {16, 28, 19})
public class GavinTest {
    @Gavin(name = {"李二狗", "张三", "王五"}, age = {16, 28, 19})
    public void show() {
        System.out.println("请开始你的表演");
    }

    public static void main(String[] args) {
        new GavinTest().show();
        @Gavin(name = {"李二狗", "张三", "王五"}, age = {16, 28, 19})
        int num = 0;
    }
}

```

**@Documented-----该注解能导入到API文档中**

用于指定被该元注解修饰的注解类将被javadoc工具提取成文档。默认情况下，javadoc是 不包括注解的，但是加上了这个注解生成的文档中就会带着注解了

```java
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import javax.lang.model.element.Element;
@Documented
@Target(value={ElementType.FIELD,ElementType.PARAMETER,ElementType.TYPE_PARAMETER,ElementType.TYPE,ElementType.METHOD,ElementType.ANNOTATION_TYPE,ElementType.CONSTRUCTOR,ElementType.LOCAL_VARIABLE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Gavin {
    String[]name() default "李二狗";//参数1
    int[]age() default 18;//参数2
}

```

在导出的API中可以发现-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/b9d5288647784fe387ad3534d2f57985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

**@Inherited------子类能够继承父类的注解**

被它修饰的Annotation将具有继承性。如果某个类使用了被

@Inherited修饰的Annotation,则其子类将自动具有该注解。

![在这里插入图片描述](https://img-blog.csdnimg.cn/7726aae561ee4d4d8b710300ef756980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5f60ae19a4b64b9d95bdb10a39814ada.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

## JAVA中的流操作

**Stream**

继承实现关系---->>

> **public interface Stream<T> extends BaseStream<T,​Stream<T>>**

类描述---->>>

支持顺序和并行聚合的元素序列 操作,(支持对元素流进行函数式操作的类--即lambda)

 Stream，这是一个对象引用流， 有原始专业 IntStream, LongStream, 和 DoubleStream，所有这些都被称为“流”和 符合此处描述的特性和限制。

要执行计算，流 操作 被组合成一个 流管道 。 一个流管道由一个源（它 可能是一个数组、一个集合、一个生成器函数、一个 I/O 通道， 等）

**类中的方法---->>>静态**

类中的方法---->>>
由于是接口,所以着重看一下接口中的静态方法,
![在这里插入图片描述](https://img-blog.csdnimg.cn/4d3087260d6041acb4a1702a223e9b52.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**Builder()---流构建器**

```java
   @Test
    public void StreamBuilder(){
        //创建一个流构造器,创建完之后可以指定流中元素的类型
        Stream.Builder<Integer> builder = Stream.builder();
//        往流中添加元素
        builder.add(1);
        builder.add(2);
        builder.add(3);
        builder.add(null);
        //add方法底层也是用的accept
        builder.accept(4);
       // 构建流，将此构建器过渡到已建状态。如果在构建器进入构建状态后，有进一步操作尝试，则会抛出一个IllegalStateException
        Stream<Integer> build = builder.build();
        build.forEach(System.out::println);
    }

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/dabe6f9003d34f309051941c90cc9794.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**合并流Concat()**

```java
 @Test
    public void StreamConcat() {
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 12, 13, 22);
        Stream<Integer> stream = list.stream();
        List<Integer> list2 = new LinkedList<>();
        Collections.addAll(list2, 1,2,3);
        Stream<Integer> stream1 = list2.stream();
//        创建两个集合,并获取流,在合并到一块
        Stream<Integer> concat = Stream.concat(stream, stream1);
//        遍历
concat.forEach(System.out::println);
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a830df2d0a6b4dcb9b62db3e6f90b4fd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**空流Empty()**

```java
 @Test
    public void StreamEmpty() {
//返回一个空的Stream,并非为null
        Stream<Object> empty = Stream.empty();
        System.out.println(empty.toArray().length);//0
    }
```

**无限流Generate()**

```java
@Test
    public void StreamGenerate(){
        //
        //返回一个无限的顺序无序流，其中每个元素是由Supplier提供的。
        Stream<Double> limit = Stream.generate(Math::random).limit(5);
        //limit方法返回由此流元素组成的流，截断长度不超过maxSize长度。
        limit.forEach(System.out::println);
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/51ca05d004634957a11ea1c0298309d4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d35ae07aaafd4378b88c6a861c93a891.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**iterate()---迭代流**

```java
 @Test
    public void StreamIterate(){
//        java8
//传入一个种子,然后对这个种子按照某个规则(UnaryOperator<T> f)迭代-
        Stream<Integer> iterate = Stream.iterate(0, e -> e + 2).limit(3);
        iterate.forEach(System.out::println);
  System.out.println("____________________");
//        java9
//        传入一个种子,迭代时对种子做一些约束,迭代规则
        Stream<Integer> iterate1 = Stream.iterate(0, e -> e<10,f->f+2);
        iterate1.forEach(System.out::println);

    }
```

**静态工厂方法of**

![在这里插入图片描述](https://img-blog.csdnimg.cn/c70d4ce540c3456fb4132473240ee162.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

静态工厂方法,返回一个流,区别就在于流中的元素个数和是否可以为null;

```java
    @Test
    public void StreamOf() {
        Stream<Integer> integerStream = Stream.of(1, 2, 3, null);
        Stream<Integer> integerStream1 = Stream.of(1, 2, 3, null, null);
        Stream<String> n = Stream.of("你好");
//        Stream<Object> objectStream = Stream.of(null);//如果只有一个元素,那么不能为null
        Stream<Object> objectStream1 = Stream.ofNullable(null);
        Stream<Object> objectStream2 = Stream.ofNullable("世界");
//        开始迭代
        integerStream.forEach(System.out::println);
        System.out.println("----------------------");
        integerStream1.forEach(System.out::println);
        System.out.println("----------------------");
        n.forEach(System.out::println);
        // objectStream.forEach(System.out::println);
        System.out.println("----------------------");
        objectStream1.forEach(System.out::println);
        System.out.println("----------------------");
        objectStream2.forEach(System.out::println);
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/5f949fcbaa2b467e8b0d061fb54559a8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)看一下结果,是不时有什么疑问???

这是因为ofnullabe方法中传入null时,返回的是一个空的流(即流中没有元素,)

可以用代码验证一下;

![在这里插入图片描述](https://img-blog.csdnimg.cn/e7729116809747caa5f93f7400d02e4c.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/4fff46cdbcd243beb33a3c3f82b84913.png)



那么流有什么用呢??

**流的应用**

API中给了几个例子---->>

```java
  int sum = widgets.stream()
                      .filter(w -> w.getColor() == RED)
                      .mapToInt(w -> w.getWeight())
                      .sum();
```

在这个例子中， widgets是一个 Collection<Widget>. 我们创造 一股流 Widget对象通过 Collection.stream(), **过滤它**以产生只包含红色小部件的流，然后 将其转换为流 int代表权重的值 每个红色小部件。 然后将该流相加以产生总重量。 



流与集合有一些相似之处,但是他们 有不同的用处和特点。 **集合主要关注高效 管理和访问它们的元素**。 相比之下，流不提供一种直接访问或操作其元素的方法，而是关注声明性地描述它们的来源和 将在该源上汇总执行的计算操作。 即在流中的操做不会对数据源产生实际性影响;

> 流在使用一次后就自动关闭了,除非是IO流;
> 流有一个基础流.关闭（）方法，并实现自动可关闭。大多数流实例在使用后实际上不需要关闭，因为它们由集合、阵列或生成功能支持，不需要特殊的资源管理。通常，只有源为 IO 通道的流（如文件.行（路径）返回的流才需要关闭。如果流确实需要关闭，则必须在尝试资源声明或类似的控制结构中作为资源打开，以确保在操作完成后立即关闭流。

流的简单应用----->>

假如有一个集合----找到集合中的极值-----

```java
@Test  
    public void StreamMax(){
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 12, 13, 22,99,32,566,322,555,8888);
        Optional<Integer> optional = list.stream().max((a, b) -> a - b);
        //拿到极值
        System.out.println(optional.get());
    }
```

这样我们就不用去写代码遍历集合拿到极值,代码大大简化了;

Stream+lambda大大简化了代码的数量,在实际应用中也非常简单方便;

**Stream中的非静态方法**

![在这里插入图片描述](https://img-blog.csdnimg.cn/63e2688dad0b458da8718a3d3f684471.png)

**Match()**

```java
    @Test
    public void StreamMatch(){
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 12, 13, 22,99,32,566,322,555,8888);
//        集合中是否都是偶数
        boolean b = list.stream().allMatch(e -> e % 2 == 0);
        System.out.println("集合中是否都是偶数--"+b);
        boolean b1 = list.stream().anyMatch(e -> e % 2 == 1);
        System.out.println("集合中是有奇数--"+b1);
    }
```

**同理noneMatch**

**Collect()**

![在这里插入图片描述](https://img-blog.csdnimg.cn/3edd3ce21ef8438fa78728c213605d00.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)其中有一种方法要求参数中传入Collector或者其实现类----但是Collector接口的实现还是比较麻烦的,所以像集合(Collections)或者数组(Arrays)一样,java提供了一个工具类Collectors来帮助我们快速实现一个Collector的对象



```java
   @Test
    public void StreamCollect() {
//将数组转换为字符串
        //准备一个数组
        String[] str = {"Hello", "world", "世界", "你好"};
//        利用工具类获得一个流
        String collect = Arrays.stream(str).collect(Collectors.joining("-"));
        System.out.println(collect);
//数组转换为集合
        List<String> collect1 = Arrays.stream(str).collect(Collectors.toList());
        System.out.println(collect1);
        Person per = new Person("张三", 18);
        Person per1 = new Person("张三", 16);
        Person per2 = new Person("张三", 18);
        Person per3 = new Person("张三", 16);
        Person per4 = new Person("张三", 18);
        Person per5 = new Person("张三", 16);
        Person per6 = new Person("张三", 17);
        List<Person> list = new ArrayList<>();
Collections.addAll(list,per,per1,per2,per3,per4,per5,per6);
//        将年龄作为key人数作为value创建一个map集合
        Map<Integer, Long> collect2 = list.stream().collect(Collectors.groupingBy(Person::getAge, Collectors.counting()));
        Set<Map.Entry<Integer, Long>> entries = collect2.entrySet();
        for (Map.Entry<Integer, Long> e : entries) {
            System.out.println(e.getKey()+"--"+e.getValue());
        }

    }
```

Collectors工具类中还提供了一些其他方法;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a5b8acde2e034e7bb0d52ade31ec5217.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/5c0a66dec281415aa3854562f6fd9253.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**另一个collect方法**

![在这里插入图片描述](https://img-blog.csdnimg.cn/1dc953865b4e44e88f57caf984be4dc4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**去除重复元素Distinct()**

```java
   @Test
    public void StreamDistinct() {
        ArrayList<Person> list = new ArrayList<>();
        Person per = new Person("张三", 18);
        Person per1 = new Person("李四", 16);
        Person per2 = new Person("张三", 18);
        Person per3 = new Person("李四", 16);
        Person per4 = new Person("张三", 18);
        Person per5 = new Person("王五", 16);
        Person per6 = new Person("张三", 37);
        Collections.addAll(list, per, per1, per2, per3, per4, per5, per6);
//        System.out.println(list.stream().count());//返回流中元素数量
//distinct去除重复元素
        Stream<Person> distinct = list.stream().distinct();//需要重写equals和hashCode
        distinct.forEach(System.out::println);
    }
```

> default Stream<T> dropWhile​(Predicate<? super T> predicate)

**DropWhile()**

```java
/**
Returns, if this stream is ordered, a stream consisting of the remaining elements of this stream after dropping the longest prefix of elements that match the given predicate.
*/
   @Test
    public void StreamDropWhile(){
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 111, 13, 22, 99, 32, 566, 322, 555, 8888);
//        删除奇数,直到第一个不满足的为止,保留剩下的,jdk9时新增的
        Stream<Integer> integerStream = list.stream().dropWhile(e -> e % 2 == 1);
        integerStream.forEach(System.out::println);
    }
```

**FindAny()**

```java
 @Test
    public  void StreamFindAny(){
        Person per = new Person("张三", 18);
        Person per1 = new Person("李四", 16);
        Person per2 = new Person("张三", 18);
        Person per3 = new Person("李四", 16);
        Person per4 = new Person("张三", 18);
        Person per5 = new Person("王五", 16);
        Person per6 = new Person("张三", 17);
        Person per7 = new Person("王五", 36);
        List<Person> list = new ArrayList<>();
       Collections.addAll(list, per, per1, per2, per3, per4, per5, per6,per7);
/*//        找到第一个
        Optional<Person> first = list.stream().findFirst();
        System.out.println(first.get());*/
//找到任意一个,
        Optional<Person> any = list.stream().findAny();
        //如果流为空
        Person person = any.get();
        System.out.println(person);

    }
```

**FlatMap**

```java
 @Test
    public void StreamFlatMap1() {
        Person per = new Person("张三", 18);
        Person per1 = new Person("李四", 16);
        Person per2 = new Person("张三", 18);
        Person per3 = new Person("李四", 16);
        Person per4 = new Person("张三", 18);
        Person per5 = new Person("王五", 16);
        Person per6 = new Person("张三", 17);
        Person per7 = new Person("王五", 36);
        List<Person> list = new ArrayList<>();
        Collections.addAll(list, per, per1, per2, per3, per4, per5, per6, per7);
        Stream<Person> stream = list.stream();
//       list.stream()与Stream.of()方式返回的流----一个是返回person的流,一个返回的是List<Person>的流
//flatMap的用法---->>>将流放放到一个小流中;
        Stream<List<Person>> list1 = Stream.ofNullable(list);
        //Stream<Object> ss = list1.flatMap(e -> e.stream());

//        这里 Stream<List<Person>> list1 的发行为一个集合.这个集合还可以转换为流,所以在遍历的时候
        Stream<Person> personStream = list1.flatMap(e -> e.stream());
        personStream.forEach(System.out::println);

    }

```

**flatMapToDouble**

将大流中的数据拿出做运算后返回的结果放在flatMapToDouble​中;
比如:

```java
 @Test
    public void StreamFlatMap3() {
        Person per = new Person("张三", 18,78.8);
        Person per1 = new Person("李四", 16,76.9);
        Person per2 = new Person("张三", 18,78.6);
        Person per3 = new Person("李四", 16,88.9);
        Person per4 = new Person("张三", 18,69.9);
        Person per5 = new Person("王五", 16,90.3);
        Person per6 = new Person("张三", 17,89.8);
        Person per7 = new Person("王五", 36,77.7);
        List<Person> list = new ArrayList<>();
        Collections.addAll(list, per, per1, per2, per3, per4, per5, per6, per7);
        //求体重小于80的和
//        要求是doubleStream
        double sum = list.stream().filter(e -> e.getWeight() < 80).flatMapToDouble(e -> DoubleStream.of(e.getWeight())).sum();
        System.out.println(sum);
//求年龄和
        int sum1 = list.stream().flatMapToInt(e -> IntStream.of(e.getAge())).sum();
        System.out.println(sum1);


    }
```

同理-----还有	flatMapToInt​跟	flatMapToLong,不在赘述

小结一下:
flatMap 跟flatMapToInt​/flatMapToLong/flatMapToDouble的用发区别----->>

```
**flatMap是将一个大流拆分成一个小流如上面的  代码
由Stream<List<Person>>拆分成了Stream<Person>,简单的来说就是将List<Person>中的每一个person拿出来作为一个流替换掉原来流中的元素;
如果想要扩大流也是可以的;
flatMapToInt是将流中的元素拿出来做一个运算,并返回一个结果;
同理flatMapToLong/flatMapToDouble;**

```

flatMap是拼接流用的 同时兼具了map的功能

collect的话是用于收集流的结果到某一个数据结构中

比如图中的result的流 要收集到一个list中的话 可以是 result.collect(Collectors.toList())

**forEach**

遍历---->

```java
    @Test
    public void StreamForeach(){
        Person per = new Person("张三", 18,78.8);
        Person per1 = new Person("李四", 16,76.9);
        Person per2 = new Person("张三", 18,78.6);
        Person per3 = new Person("李四", 16,88.9);
        Person per4 = new Person("张三", 18,69.9);
        Person per5 = new Person("王五", 16,90.3);
        Person per6 = new Person("张三", 17,89.8);
        Person per7 = new Person("王五", 36,77.7);
        List<Person> list = new ArrayList<>();
        Collections.addAll(list, per, per1, per2, per3, per4, per5, per6, per7);
        //list.stream().forEach(System.out::println);
        list.stream().forEachOrdered(System.out::println);

    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/14a7e3dbabaf4da69625f7d4a5c84bcd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**Peek()**

```java
 @Test
    public void StreamPeek(){
        Person per = new Person("张三", 18,78.8);
        Person per1 = new Person("李四狗子", 16,76.9);
        Person per2 = new Person("张三牙子", 12,78.6);
        Person per3 = new Person("李老四", 15,88.9);
        Person per4 = new Person("\u5f20\u6c42\u80dc", 18,69.9);
        Person per5 = new Person("王五", 16,90.3);
        Person per6 = new Person("张三", 14,89.8);
        Person per7 = new Person("王老五", 36,77.7);
        List<Person> list = new ArrayList<>();
        Collections.addAll(list, per, per1, per2, per3, per4, per5, per6, per7);
        List<String> collect = list.stream().filter(e -> e.getName().equals("张三")).peek(e-> System.out.println("filter-value"+e)).map(e -> e.getName()).peek(e -> System.out.println("Mapped value: " + e)).collect(Collectors.toList());

        collect.forEach(System.out::println);
        System.out.println("------------------");
        List<String> collect1 = Stream.of("one", "two", "three", "four")
                .filter(e -> e.length() > 3)
                .peek(e -> System.out.println("Filtered value: " + e))
                .map(String::toUpperCase)
                .peek(e -> System.out.println("Mapped value: " + e))
                .collect(Collectors.toList());
        collect1.forEach(System.out::println);
    }


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/921ca1d580b14b279a8fd4d55bacc853.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

peek方法的用处是返回每一个节点时操作的元素,一般用于debug时;
![在这里插入图片描述](https://img-blog.csdnimg.cn/f8bf5cc260b643b88a9e55e6c5e50783.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**skip/sorted**

```java
//跳过几个元素,取剩下的
        Stream<Person> skip = list.stream().skip(2);
      //  skip.forEach(System.out::println);

        Stream<Person> sorted = list.stream().sorted();

        Stream<Person> sorted1 = list.stream().sorted((a, b) -> (int) (a.getWeight() - b.getWeight()));
        sorted1.forEach(System.out::println);
```

takeWhile()

```java
    @Test
    public void StreamtakeWhile() {
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 111, 13, 22, 99, 32, 566, 322, 555, 8888);
//        取奇数,直到第一个不满足的为止,删除剩下的,jdk9时新增的
        Stream<Integer> integerStream = list.stream().takeWhile(e -> e % 2 == 1);
        integerStream.forEach(System.out::println);

    }

```

剩下一个reduce方法需要继续搞一搞

**Reduce()**

用于比较,将不符合条件的全部舍弃,

```java
 @Test
    public void testReduce() {
        String [] str= {"hello","world","javaWH","javaWA","c++","c#"};
        Stream<String> stream = Arrays.stream(str);
//返回字符串中跟基准相比长度最大的,如果都不大与基准,则返回基准值;
       /* String reduce = stream.reduce("standUP", (e, f) -> e.length()>f.length()? e : f);
        System.out.println(reduce);*///standUp
        //返回字符串中长度最大的值
        Optional<String> reduce1 = stream.reduce((e, f) -> e.length() > f.length() ? e : f);
        System.out.println(reduce1.get());
    }
```

<center>

<table><th ><img style="height:60px;width:220px" src="/pic/div/bird.gif"/>

</th><th>

**[java代码练习](/javaprom/javasys.md#java代码进阶-手写系统)**

1. [JAVA--日历打印](/javaprom/javasys.md#java-日历打印)

2. [JAVA--记账系统](/javaprom/javasys.md#java-记账系统)

3. [JAVA--猜数字游戏](/javaprom/javasys.md#java-猜数字游戏)

4. [JAVA--贪吃蛇游戏](/javaprom/javasys.md#java-贪吃蛇游戏)</th><th ><img style="height:60px;width:220px" src="/pic/div/bird.gif"/>

   </th></table></center>