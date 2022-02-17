# JAVA中的代码块

先来说明一下代码块的类型--

## 1,静态代码块--

方法之外的--- 用static修饰的代码块，

> --静态块主要用于 类之前就加载的--比如数据库连接的初始化信息,一些全局的代码;

形式如下--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210630204054620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

## 2,构造代码块--

放在方法之外的代码块--（没有static修饰的）

> --构造块主要用于(由于是在构造方法之前执行,可以对构造方法做一些文章)

形式如下--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210630204604206.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

## 3,普通块--

放在方法区中，作用是限制方法区中变量的作用范围
形式如下--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210630204716252.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

下面在说一下构造方法，当我们调用构造方法时，构造方法中的内容也会被执行（不过日常中我们习惯在构造方法中只写关于构造方法的内容，而不写其他无关的）  如以下代码：

```java
public class Person1 {
    public Person1() {
        System.out.println("构造方法执行啦---");
    }
    public static void main(String[] args) {
Person1 p= new Person1();

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021063020522867.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
下面做一下完整等的演示，看看各个代码块执行的顺序；

```java
package com.gavin.exercise;

/**
 * @author : Gavin
 * @date: 2021/6/30 - 06 - 30 - 12:51
 * @Description: com.gavin.exercise
 * @version: 1.0
 * @describe 代码块
 */


public class Test {
    {
        System.out.println("---构造块2执行");
    }
    public Test() {
                System.out.println("---构造方法执行了");
    }

    public void find() {
        //System.out.println("方法区中---");
        {//限制变量作用范围的
            int a = 100;
            System.out.println("find()方法区中的普通块执行了");
        }
        //  System.out.println(a);//这里访问不到属性a

    }

    public static void main(String[] args) {
        {
            System.out.println("---main()中普通块1执行");
        }

      //  Test t= new Test();
       // t.find();
      //  Test tt= new Test();
        {
            System.out.println("---main()中普通块2执行");
        }

    }

    static {//只能访问静态属性\方法
        System.out.println("---静态块执行");

    }

    {
        System.out.println("---构造块1执行");
    }

}



```

当在main方法中没有调用任何方法或属性时候--执行结果如下--

```cpp
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
---静态块执行
---main()中普通块1执行
---main()中普通块2执行
Process finished with exit code 0
```

> 没有调用任何属性或方法时候，我们会发现静态代码块先执行，然后是main()方法中的普通块。因为程序的入口在main所以main()中的普通块被执行了，但是find()方法中的普通块没有被执行，所以普通块跟着方法被一块执行；

下面我们来调用一下属性或者方法；

```java
//其他代码省略，只看main方法中的
 public static void main(String[] args) {
        {
            System.out.println("---main()中普通块1执行");
        }
        Test t= new Test();
        t.find();
        Test tt= new Test();
        {
            System.out.println("---main()中普通块2执行");
        }
    }

```

执行结果如下--

```bash
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
---静态块执行
---main()中普通块1执行
---构造块2执行
---构造块1执行
---构造方法执行了
find()方法区中的普通块执行了
---构造块2执行
---构造块1执行
---构造方法执行了
---main()中普通块2执行

Process finished with exit code 0
```



我们可以发现执行执行顺序--
1        静态块--而且只加载一次，

2        main()中普通块1执行

3        构造块2执行

4        构造块1执行
       

> main()中普通块1执行,该到了对象 t ，顺理成章的跑去调用Test的构造方法了，但是构造方法中的内容还没被执行就执行了构造块2和构造块1，（说明构造块先于构造方法执行）

5，构造方法执行

6,实例对象调用的方法find()执行了

7，main()中普通块2执行

8,---构造块2执行  
9,构造块1执行  
10,构造方法执行了  
11,main()中普通块2执行

> 总结-- 
> 1,静态代码块跟放置的位置无关----先执行. 
> 2,构造代码块先于构造方法执行,放置的位置决定了构造哪个代码块先执行.
> 3,普通代码块跟着方法一块被执行,执行顺序跟位置有关;

彩蛋--
我们来debug一下静态块--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210630213059680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
直接执行完毕--这也说明静态块先被加载到内存中,下一步直接就是main()方法执行--over

