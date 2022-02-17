

# JAVA中的关键字

## static

```java
package com.array.sort;

/**
 * @Auther: GavinLim
 * @Date: 2021/6/30 - 06 - 30 - 17:59
 * @Description: com.array.sort
 * @version: 1.0
 */
public class jingtai {
    int a=10;
    static int age=100;
    public static void main(String[] args) {
        jingtai.addPerson();
        System.out.println(new jingtai().deletePerson());
        System.out.println(new jingtai().a);

    }
    public static void addPerson(){
        //a=2;//这里静态方法中不能掉用费静态属性，
        age=200;
        System.out.println("age="+age);
    }
    public  int deletePerson(){
        this.a=1000;
        System.out.println("age="+age);
        return a;
    }
}

```

static修饰的属性或者变量 其实是属于类的，在类加载之前就已经被加载了；
我们来debug一下，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210630183526509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)为什么静态的方法不能访问非静态属性呢？
因为非静态属性的访问是要有具体实例对象的，在没有具体实例对象产生之前，静态属性就已经被加载了

> 比方说---静态方法addPerson()要访问 属性a ，实际上这时候属性a还没有被加载到内存里，因此就没法被访问。
> 反之非静态方法可以访问静态属性，因为静态属性已经在类之前加载完毕了；
> 总结静态属性在类加载之前就被加载到静态常量区---被共享的属性；

内存是这样的--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210630184059947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
分析瑕疵在所难免，还请大神们多多指教；

共同学习，共同进步，日拱一兵，只进不退；



## final

java中final关键字可以修饰：
1，基本数据类型
2，引用数据类型
3，方法
4，类

一个一个分析---
①基本数据类型---
一旦定义就不能修改
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070215295439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

②引用数据类型--
一旦定义也不能修改--这里是指地址
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070215314389.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

当属性被final修饰时，必须被初始化，否则报错

③ 方法，被final修饰的方法不能被重写但可以重载
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070215342445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702154840955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



④类，类被声明为final则该类无法被继承,所以抽象类不能被final 修饰

> ，如果一个类你永远不会让他被继承，就可以用final进行修饰。final类中的成员变量可以根据需要设为final，但是要注意final类中的所有成员方法都会被隐式地指定为final方法。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702154157824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)





```java
package finallyvar;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/2 - 07 - 02 - 14:15
 * @Description: finallyvar
 * @version: 1.0
 */
class Dog {
String name;
}

public class ceshi {
    public static void main(String[] args) {
        final Dog D = new Dog();
        D.name="旺财";

        Dog dog = new Dog();
       // ceshi.shout(D);
        ceshi.shou(dog);
        System.out.println(D);

    }

    public static void shout(Dog d0) {//
       d0 = new Dog();

        System.out.println(d0);

    }

    public static void shou(final Dog d2) {
        System.out.println("wawatwa");
        System.out.println(d2);
    }
}

```

内存分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210702154546738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 下面这段话摘自《Java编程思想》第四版第143页：
> 　　“使用final方法的原因有两个。第一个原因是把方法锁定，以防任何继承类修改它的含义；第二个原因是效率。在早期的Java实现版本中，会将final方法转为内嵌调用。但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升。在最近的Java版本中，不需要使用final方法进行这些优化了。“



## import

关于普通导入--import

一个类中要想使用别的类中的属性或者方法,就需要将该类所在的包进行导入操作---使用 import 来导入不同包的类以方便使用他;

***> 普通导入的方式---***

>  一-------精确导入--某一个包中的某个类
>  ***import 包名.类名;***
>  二----------泛式导入--某包下的所有类.
>  ***import.包名.****

### import static 

***静态导入的方式***
 一-------精确导入---某个包中的静态方法
***import static java.lang.Math.round;***
二----------泛式导入--某包下的所有静态属性和方法.
***import static java.lang.Math.*;***

```java
package baozhuang;

/**
 * @author : Gavin
 * @date: 2021/7/7 - 07 - 07 - 21:36
 * @Description: baozhuang
 * @version: 1.0
 */
//静态导入---导入该类下所有的静态方法

import java.util.Date;

import static java.lang.Math.round;
import static java.lang.Math.*;

public class Test {
    public static void main(String args[]) {
        Date date = new Date();//
        System.out.println(date);
        System.out.println("四舍五入--" + round(12.8));
        System.out.println("向上取整--" + ceil(12.8));

        System.out.println("向上取整--" + floor(12.8));
        System.out.println("绝对值---" + abs(-1267));
        System.out.println(random(100.67));

    }
}
```

实际上静态导入也不太常用,

1.使用静态导入，牺牲了代码的阅读性，不容易区分该属性或方法是属于当前类的还是属于静态导入的。

2.使用静态导入的时候，如果引入了和当前类的信息冲突的内容，或者静态导入了多个类有相同的静态信息。总之，是会导致编译器模糊不清的情况，这时候就还是需要用 类名.静态信息 的方式来加以区分。

看以下代码---round().

```java
package baozhuang;

/**
 * @author : Gavin
 * @date: 2021/7/7 - 07 - 07 - 21:36
 * @Description: baozhuang
 * @version: 1.0
 */
//静态导入---导入该类下所有的静态方法

import static java.lang.Math.*;

public class Test {
    public static void main(String args[]) {
        System.out.println("四舍五入--" + round(12.8));
        System.out.println("向上取整--" + ceil(12.8));

        System.out.println("向上取整--" + floor(12.8));
        System.out.println("绝对值---" + abs(-1267));
        System.out.println(random(100.67));

    }

    public static double random(double a) {
        return a;
    }
}
```

运行结果--

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
四舍五入--13
向上取整--13.0
向上取整--12.0
绝对值---1267
100.67
```

除非需要用到大量的静态方法,一般建议是不使用静态导入的;

