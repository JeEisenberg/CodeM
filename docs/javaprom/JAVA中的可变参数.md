# JAVA中的可变参数

其实我们一开始就接触到了可变参数,就是main方法中的String []args

## main方法

> 有时候你深入研究一下，模棱两可的话，面试的时候容易抓瞎，对于程序员来讲，弄懂底层的逻辑对于语言的学习是很有帮助的；> 这也难怪大厂基本都会问道底层源码的东西---

先来看代码--

```java
package method;

public class MainlyM {
	public static void main(String[] args) {		
		System.out.println(args.length);
	}

}

```

既然main()方法中穿的是一个String类型的数组，那么我们尝试看一下这个数组中都有些什么，---直接打印args.length
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021062915103189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
我们会发现该数组长度为0，但不是null，正常情况下声明一个数组而不为该数组进行赋值，我们会发现 空指向异常

```java
public class TestDTM {

	public static void main(String[] args) {
	
		int array[]=null;	
		System.out.println(array.length);
	}
}

```

```bash
D:\>javac TestDTM.java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8

D:\>java TestDTM
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
Exception in thread "main" java.lang.NullPointerException: Cannot read the array length because "<lo
cal1>" is null
        at TestDTM.main(TestDTM.java:10)

```

注意int array []= new array[0];与int array[]=null；是有本质区别的；

```java

public class TestDTM {
	public static void main(String[] args) {
			int array[]=new int[0];
			System.out.println(array.length);
	}
}

```

```c
D:\>javac TestDTM.java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8

D:\>java TestDTM
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
0
```

所以在我们向main()中传入参数时候--

```java

public class TestDTM {
	public static void main(String[] args) {
		for(String str:args)
		System.out.println(str);
		System.out.println(args.length);
	}
}

```

```bash
D:\>javac TestDTM.java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8

D:\>java TestDTM aa bb ccc ddde f "q q"
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
aa
bb
ccc
ddde
f
q q
6
```

默认参数是以空格进行断开的，如果参数带空格，那么要以引号将其包围；

## 可变参数

可变参数在jdk1.5之后才有的，对于可变参数平常用到的也不多；
知道可变参数解决了哪一些问题就好了；
可变参数的格式 数据类型 ... 变量名（实际可以将可变参数当做数组来处理）
1--解决了部分方法重载 的问题；
2--可变参数和其他参数放在一起时，需要将可变参数放在后面；

```java
package com.array.sort;

/**
 * @Auther: GavinLim
 * @Date: 2021/6/27 - 06 - 27 - 15:12
 * @Description: com.array.sort
 * @version: 1.0
 */

public class Kuaijiejian {

    public static void main(String[] args) {
method(12,12,1,2,3445,5,677,7,432);
method (15,new int[]{1,2,3,4,5,6,7});
    }
    public static void method(int num,int...num1){
        System.out.println(num+"\t");
        for (int i:num1
             ) {
            System.out.print(i+"\t");
        }
        System.out.println();

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210629160338218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 上面执行结果是一样的，所以实际可以将可变参数当做数组来处理

如果放在前面，就会产生问题，
举个例子--
如果参数都是int类型的，可变参数可以接收所有的数据，剩下的参数就分配不到参数值了；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210629155912946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)如果参数类型不一样，可变参数放在前面那么就会出现类型不兼容的异常；
所以有多个参数时可变参数要放在后面；
可不可以有两个或多个可变参数能？
不能--因为可变参数要放在参数的在最后，这样就无法确定哪个可变参数放在最后了；

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210629160639203.png)共同学习共同进步；



