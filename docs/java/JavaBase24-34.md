![](..\pic\div\header.gif)

##  24.包的概念及使用

本节内容：

1，包的定义及导入

2，Jar命令的使用

3，静态导入

4，访问控制权限

6，命名规范

24.1包

包实际上就是一根文件夹，在不同的文件夹下可以使用同名的类，那么这就是包的作用；

在java中使用Package语法定义包



```java
package org.chwenyulim;
interface A{
	public void start ();}
class Outer  {
	public void fun(A a) {		a.start();				}
	public void fun2() {
		this.fun(new A() {			public void start() {
				System.out.println("实现接口");			}
		});	}  }
public class Lequ {
	public static void main(String[] args) {
		new Outer().fun2();
	}

```

此时使用package语法在类中定义了一个包，在生成的Class文件时需要将所有的class类放在是顶的包中；

在编译时可以通过以下命令进行打包编译--

javac Lequ.java -d org这样就表示为一个类打包了;

-d表示生成目录，根据package的定义生成

.表示在当前表示的文件夹中生成 

![](..\pic\图片59.png)

这样就将类在当前文件夹下打包了，类的文件夹为org，打包文件夹名为org，生成的class文件在org\chwenyulim

由于编译的类是有包名的，所以必须在工作目录下创建包名对应的文件夹，将源码放到对应的文件夹下，否则会找不到这个类，然后执行javac编译，将生成的class文件打包，生成的class文件放到指定文件夹下；

 这时候运行的时候calss文件就为包名.类名

![](..\pic\图片60.png)

24.2.导入包

import  导入包来使用该包；

```java
package org_chwenyulim_order;
import chwenyu_xuexi.*;//导入包
public class Monitor {
	public static void main(String[] args) {
		Leader lead = new Leader();
		lead.Lead();
	}
}
```

在进行导包操作时，一定注意如果一个包中的类需要被外部访问，那么此类一定要声明为public class 类型；

默认权限类型 以及protect权限只能在本包中访问；

导入一个包中多个类，则只需要将类的位置替换为*，格式import chwenyu_xuexi.*;

使用*和对应类的性能一样，java只会导入包中用到的类；

```java
import chwenyu_xuexi.Leader;//导入包
import vvvv.Leader;
public class Monitor {
	public static void main(String[] args) {
		Leader lead = new Leader();
		lead.Lead();
	}
}

```

当我们导入不同的包中相同名称的类，在编译时就会提示错误--指向不明确所以此时最好明确指出哪个包中的 

```java
import chwenyu_xuexi.Leader;//导入包
import vvvv.*;;
public class Monitor {
	public static void main(String[] args) {
		vvvv.Leader lead = new vvvv.Leader();
		lead.Lead2();
	}
}
```

24.3静态导入

如果一个包中的某个类中的方法都是static类型的或者大部分都是，则就可以使用静态导入；

```java
public class Leader {
	public static void Lead2() {
		System.out.println("我是vvvv中的类");
	}
	public static void lead3() {
		System.out.println("静态导入成功");
	}
}


```

```java
 import  vvvv.*;
public class Monitor {
	public static void main(String[] args) {
		vvvv.Leader lead = new vvvv.Leader();
		String str =lead.Lead2();
		int X=lead.add(8, 8);
		System.out.println(str);
		System.out.println(X);
	}

```

24.4.jar的打包

在开发中，一个系统会有很多的类出现，如果现在直接把这些类给对方看，则肯定不方便，因为太多了，所以一般情况下都会将这些类打包成jar包，以jar包的形式把这些类交给用户使用；

在java中提供了jar命令语法

![](..\pic\图片61.png)

jar -cvf 包名.jar java文件目录

![](..\pic\图片62.png)



-c:创建新的存档

-v生成详细输出到标注输出上

-f指定存档文件名

Lequ.jar是生成的jar文件的名称

如果想使用jar包，则必须配置classpath

方式1--set classpath =.;E:\org\Lequ.jar

以上是通过命令行配置，也可以通过配置环境变量来配置；



24.5系统常用包

1，在java中提供了大量的系统开发包，java.lang;此包中包含很多常用的类，例如String ，此包属于自动导入，但是在JDK1.0的时候是需要手动导入的；

2，Java.lang.reflect;此包为反射机制包，是整个java中最重要的包，此包可以完成大量的底层操作。

3，java.util；此包为工具包，可以方便做各种开发设计。

4，Java.io；IO操作

5，Java.net; 网络编程

6，Java.sql;数据库编程

7，java.text；文本处理

24.6访问权限

在之前已经接触过的三种访问权限：

public ：访问权限最大，公共的可以跨包访问；

private：最小的，只能在本类中访问；

default:默认的，只能在本包中访问；

protect：在本包以及不同包的子类中可以访问;



| No   | 作用域         | private | default | protect | public |
| ---- | -------------- | ------- | ------- | ------- | ------ |
| 1    | 本类           | √       | √       | √       | √      |
| 2    | 同一包的类     |         | √       | √       | √      |
| 3    | 不同包的子类   |         |         | √       | √      |
| 4    | 不同包的非子类 |         |         |         | √      |

24.7.java的命名规范

类的命名--每个单词首字母大写--大驼峰

方法的命名--第一个单词首字母小写，之后每个单词首字母大写--小驼峰

属性的命名--第一个单词首字母小写，之后每个单词首字母大写--小驼峰

常量的命名--所有字母大写

包的命名--所有字母小写              没有包的类是不存在的；

## 25.异常的抛出

25.1RuntimeException

我们在之前编译的代码有时会出现错误，当然在java的doc中也有很多方法都包含了throws关键字



parseInt

public static int parseInt(String s)

​                    throws NumberFormatException

那么既然有此关键字 则应该有try catch 进行处理；

 NumberFormatException是RuntimeException的子类

```
try{
//要检测的代码
}catch(Exceotion e){
//处理异常的代码
}
finally{//代码--此代码一定会被执行
}

```

在使用时没有必要进行异常的处理的编译器就不会去处理。即只要是RuntimeException的异常对象，虽然使用了throws，但是在程序中也可以不使用try catch进行处理；如果抛出的是Exception则必须进行处理 

> (1)如果程序之中产生了异常，那么会自动地由JVM根据异常的类型，实例化一个指定异常类的对象；如果这个时候程序之中没有任何的异常处理操作，则这个异常类的实例化对象将交给JVM进行处理，而JVM的默认处理方式就是进行异常信息的输出，而后中断程序执行。

> ⑵ 如果程序之中存在了异常处理，则会由try语句捕获产生的异常类对象；然后将该对象与try之后的catch进行匹配，如果匹配成功，则使用指定的catch进行处理，如果没有匹配成功，则向后面的catch继续匹配，如果没有任何的catch匹配成功，则这个时候将交给JVM执行默认处理。

> ⑶ 不管是否有异常都会执行finally程序，如果此时没有异常，执行完finally，则会继续执行程序之中的其他代码，如果此时有异常没有能够处理（没有一个catch可以满足），那么也会执行finally，但是执行完finally之后，将默认交给JVM进行异常的信息输出，并且程序中断。

```java
package org_chwenyulim_order;
public class Wind {
    public static void main(String args[]) {
        System.out.println("异常抓取之前");
        try {
            int array[] = new int[5];
            array[0] = 0;
            array[1] = 5;
            array[10] = 10;// 越界异常
            int result = array[1] / array[0];// 分母为0的异常
        } catch (ArithmeticException exception) {
            exception.printStackTrace();
        } catch (ArrayIndexOutOfBoundsException exception) {
            exception.printStackTrace();//打印异常信息
        } finally {
            System.out.println("此处不管是否出错都会执行");
        }
        System.out.println("程序执行结束");    }}

```

25.2.throws关键字

使用throws声明的方法表示此方法不处理异常，而由系统自动将所捕获的异常信息“抛给”上级调用方法。throws使用格式如下。
        访问权限 返回值类型 方法名称（参数列表） throws 异常类
    {        // 方法体；      }

```java
package org_chwenyulim_order;
public class Wind {
    public static void main(String args[]) {
        System.out.println("异常抓取之前");
        int arr[]=new int [3];
        try {
        	setZero(arr,10);
        }catch(ArrayIndexOutOfBoundsException E) {
        	System.out.println("数组下标越界");
        	E.printStackTrace();
        }
        System.out.println("main方法结束");
    }
    private static void setZero(int arr[],int index )throws ArrayIndexOutOfBoundsException{
    	arr [index]=0;
    }   }

```

一旦方法出现异常，setZero()方法自己并不处理，而是将异常提交给它的上级调用者main()方法。而在main方法中，有一套完善的try-catch机制来处理异常。如果上级调用者没有异常处理，会再次找上级，找不到则交给JVM去处理。

```java
public class Ceshi{
public static void main (String args[])throws Exception {
	String str ="123";
	Integer inted =new Integer (str);
	int S=Integer.parseInt(str);
	System.out.println(2*S);}		}
//此方法中有throws关键字，那么意味着程序应该使用try-catch去处理异常，这就要观察异常属于哪个类的子类，只要是runtime的异常对象，那么程序中也可以不使用try-catch进行处理；
//如果抛出的是Exception类的子类，那么就必须进行处理；

```

```java
class Math {
	int i,j;
	public void Div(int i,int j)throws Exception {	
		try {
			double X=i/j;
		System.out.println(X);
			  }
		catch(ArithmeticException ex) {
			System.out.println("分母为0异常");
			//throw ex;
		}	}}
public class Ceshi {
	public static void main(String args[])  {
   		Math math =new Math();
		math.Div(10, 5);			}			}


```

在方法中 通过throws抛出异常，在方法中不会去处理该异常，而是传给上级调用者，上级调用者如果不处理异常，在编译时也会提示出错； 

```java
class Math {
	int i, j;
	public double Div(int i, int j)  throws ArithmeticException{
			return i/j;	}}
public class Ceshi {
	public static void main(String args[])  {
		Math math = new Math();
		try {	double temp =math.Div(10, 0);
		} catch (ArithmeticException ex) {
			System.out.println("上级调用者处理异常" + ex);		}
		finally {System.out.println("");				}	}}

```

如果在主方法处使用了throws关键字，则所有的异常交给JVM虚拟机来处理，所以以下代码结构的理解为： 

```java
class Math {
	public double Div()  throws Exception{	}}
public class Ceshi {
	public static void main(String args[]) throws Exception {
		Math math = new Math();			}}

```

在方法中不去处理异常，而是交给上级调用者，如果上级调用者抛出异常但是很也不处理异常，则会交给JVM去处理；

```java
class Math {
	int i, j;
	public double Div(int i, int j)  throws Exception{
			return i/j;	}}
public class Ceshi {
	public static void main(String args[]) throws Exception {
		Math math = new Math();
		math.Div(10, 0);
System.out.println(math.Div(10, 0));	}}

```

25.3.throw关键字

可以用throw 人为直接抛出一个异常，当执行这条语句时，将会引发一个异常--抛出异常，throw后的代码无法被编译执行。

```java
public class Ceshi{
public static void main (String args[])throws Exception {
	try{
		String str ="123i";
		Integer inted =new Integer (str);
		int S=Integer.parseInt(str);}
	catch (NumberFormatException num){
		throw num;
		System.out.println("我无法被执行");
	}
	finally {
		System.out.println("我必须被执行");	}}		}


```

![](..\pic\图片63.png)

throw总是出现在方法体中，一旦抛出一个异常，程序会在throw语句后面立即终止，后面的语句无法执行；

范例--

```java
public class Ceshi{
public static void main (String args[])throws Exception {
	int X[]=new int [3];
	System.out.println("异常捕捉开始啦！！！");
	try {
		setZero(X,10);//下标越界异常
	}catch(ArrayIndexOutOfBoundsException ex) {
		ex.printStackTrace();		System.out.println();
		throw ex;	}
finally {
		System.out.println("finally 代码块一定会被执行");	}
	System.out.println("main方法结束");}		//不会被执行
public static void setZero(int array[],int index)throws ArrayIndexOutOfBoundsException {
	System.out.println("setZero方法开始");
	try{array[index]=0;}
	catch(ArrayIndexOutOfBoundsException ex ) {
		ex.printStackTrace();throw ex;	}
	finally {	System.out.println("--------------------");		}
	System.out.println("setZero方法结束");//不会被执行}}

```

25.4异常的产生与处理

1，异常的产生原因以及产生的效果

2，异常处理的标准结构

3，throw和throws关键字

4，自定义异常的处理

5，assert关键字--JDK1.4之后加入

25.4.1.为什么要有异常处理

```java
public class Ceshi {
	public static void main(String args[]) throws Exception {
    int i=10;
    int j =0;
    System.out.println("计算开始");
    System.out.println(i/j);
    System.out.println("计算结束");
	}
}
```

在高等数学中，任何数除以0，得到一个无穷大（小）的数，这时，电脑内存会被占满；

那么所谓的异常处理就是程序在出现异常时依然可以正确执行完；

25.4.2异常处理的标准格式

```java
public class Ceshi {
	public static void main(String args[]) throws Exception {
    int i=10;    int j =0;
    System.out.println("计算开始");
    try {
    System.out.println(i/j);}
    catch(ArithmeticException ar){
    	System.out.println("出现异常--"+ar);    }
    finally {
    	System.out.println("我可以没有，如果有那么我一定要被执行");    }
    System.out.println("计算结束");	}}

```

通过异常处理可以很好的控制程序的正确完结；

将上面程序实际化处理一下，输入两个参数，我们在去处理异常

可能出现的异常----ArithmeticException--ArraysIndexOutofBoundsException--NuberformatException

```java
public class Ceshi {
	public static void main(String args[]) throws Exception {	   
		    System.out.println("计算开始");
		    try {
				int i=Integer.parseInt(args[0]);
				int j =Integer.parseInt(args[1]);
		    System.out.println(i/j);}
		    catch(ArithmeticException ar){
		    	System.out.println("出现异常--"+ar);
				System.out.println("被除数不能为0");
		    }
			catch(ArrayIndexOutOfBoundsException ar){
				System.out.println("出现异常--"+ar);
		System.out.println("非法参数，请输入两个数");
			}
			catch(NumberFormatException ar){
				System.out.println("出现异常--"+ar);
				System.out.println("输入错误--请输入纯数字");
			}
		    finally {
		    	System.out.println("我可以没有，如果有那么我一定要被执行");
		    }
		    System.out.println("计算结束");
			}		}


```

25.4.3.异常处理流程

对于java来讲，每当程序出现了异常，实际上都是产生了一个异常类的实例化对象，这种处理方式类似于方法的参数传递，只要参数类型匹配了，就可以使用此catch进行处理。

实际上异常处理的最大父类--Throwable，但是一般开发中不会使用此种方式处理异常，

Thowable有两个子类--Error 和 Exception

   Error一般表示JVM错误

Exception 一般是指程序中的错误，所以开发中如果想要处理异常一般就是指Exception

一般来讲程序在捕获时不要使用Throwable，以为表示的范围太大了；

```java
public class Ceshi {
	public static void main(String args[]) throws Exception {	   
		  System.out.println("计算开始");
		    try {
				int i=Integer.parseInt(args[0]);
				int j =Integer.parseInt(args[1]);
		    System.out.println(i/j);}
			catch(Exception ar){
				System.out.println("其他异常--"+ar);
				System.out.println("此参数非法");
			}
		    finally {
		    	System.out.println("我可以没有，如果有那么我一定要被执行");
		    }
		    System.out.println("计算结束");			}		}


```

在异常捕获时，捕获更广的异常要放在捕获细致的异常后面--catch(Exception ex){}要放在最后

如果要处理异常的话最好分开处理；我们看以下范例代码,

```java
public class Ceshi {
	public static void main(String args[]) throws Exception {	   
	    System.out.println("计算开始");
	    try {
			int i=Integer.parseInt(args[0]);
			int j =Integer.parseInt(args[1]);
	    System.out.println(i/j);}
		catch(Exception ar){
			System.out.println("其他异常--"+ar);
			System.out.println("此参数非法");
		}
		catch(ArrayIndexOutOfBoundsException ar){
	System.out.println(ar);
		}
	    finally {
	    	System.out.println("我可以没有，如果有那么我一定要被执行");
	    }
	    System.out.println("计算结束");
		}
	}


```

25.6异常的标准格式

try{ 含有可能出现异常的代码

}catch(某异常实例){

 throw 该异常实例(“提示信息”);//构造方法exception(String msg)此时该异常抛给了上级调用者；

}

catch(){}.......可以有好多的catch(){}

finally{}

25.7.自定义异常类

一个类只要继承了Exception，则表示一个自定义异常类当发现系统中提供的异常类不够用的时候就可以自定义异常类；

```java
class MyException extends Exception {
	public MyException(String msg) {
		super(msg);
	}
}
public class Ceshi {
	public static void main(String args[]) {
		try {
			throw new MyException("自定义异常");
		} catch (MyException ex) {
			ex.printStackTrace();
		}
	}
}

```

25.8异常类之间的区别

Exception与RuntimeException的区别：

Exception---强制用户必须进行处理

RuntimeException--是Exception的子类，由用户选择是否进行处理；

对方法中抛出的异常（throws Exception）的理解---

一般情况下，方法中抛出异常会默认交给方法调用处处理，而不是在方法体中进行处理；

如果是方法中抛出的父类 Exception，如果方法中不自己去处理异常，而是由调用处去处理，那么在调用处的方法体中也要出现（throws Exception），如果抛出的是明确的异常类型，则调用处不需要添加（throws Exception）

![](..\pic\56.png)

## 26. 反射与对象的克隆

如果想要实现对象的克隆，可以使用Object类之中定义的克隆方法；

如果想要正常的实现克隆操作，那么对象所在的类必须实现Cloneable接口，但是这个接口里面并没有定义任何方法，此接口属于表示接口，值得是一种能力的体现。

Protected Object clone() throws CloneNotSupportedException;

26.1克隆的实现

```java
 class Book implements Cloneable{//可以被复制
	String title;	float price;
	public String getTitle() {
		return title;	}
	public void setTitle(String title) {
		this.title = title;	}
	public float getPrice() {
		return price;	}
	public void setPrice(float price) {
		this.price = price;	}
	public Book(String title,float price){
		this.title=title;
		this.price=price;	}
	protected Object clone()throws CloneNotSupportedException{
		return super.clone();	}//覆写方法
	public String toString() {
		return this.title+this.price;	} }
 public class Myfun {
	public static void main(String args[]) throws CloneNotSupportedException {
Book bookA=new Book("java",56.6f);
Book bookB=(Book)bookA.clone();//转型
bookB.setPrice(100.5f);
System.out.println(bookB.toString());
	}
}

```

创建并返回此对象的副本。“复制”的确切含义可能取决于对象的类别。通常的意图是，对于任何对象x，表达式： x.clone（）！= x将为真，并且该表达式为：
 x.clone（）.getClass（）== x.getClass（）
将是true，但这些不是绝对要求。通常情况是：
 x.clone（）.equals（x）将是true，这不是绝对要求。按照惯例，应通过调用返回对象 super.clone。如果一个类及其所有超类（除外 Object）都遵守此约定，则情况将如此 x.clone().getClass() == x.getClass()。
按照惯例，此方法返回的对象应独立于该对象（将被克隆）

> clone用于类 的方法Object执行特定的克隆操作。首先，如果此对象的类未实现interface Cloneable，则 CloneNotSupportedException抛出。注意，所有数组都被认为是实现接口Cloneable的clone方法，数组类型方法的返回类型T[] 是T[]T，其中T是任何引用或原始类型。否则，此方法将创建此对象的类的新实例，并使用该对象相应字段的内容完全初始化其所有字段，就像通过赋值一样；字段的内容本身不会被克隆。因此，此方法执行此对象的“浅复制”，而不是“深复制”操作。

26.2.反射

Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；

Java反射机制主要提供了以下功能：在运行时判断任意一个对象所属的类；在运行是构造任意一个类的对象；在运行时判断任意一个类所具有的成员变量和方法；在运行时调用任意一个对象的方法；

26.2.1根据对象找到类

```java
import java.util.ArrayList;
class Book{
	public void read() {
		System.out.println("I read book");	}}
public class Monitor {
	public static void main(String[] args) throws Exception {
		ArrayList<Integer> array = new ArrayList<Integer>();
		array.add(1);
		array.add(2);
		System.out.println(array);
		Book book = new Book();
Class<?> str =book.getClass();
System.out.println(str);//可以直接打印出book所在的类
book.read();	}}

```

26.2.2根据类去得到实例化对象

1，利用Object类的getClass()方法，但是在这之前必须实例化指定类；   

```java
class Book {
	public void read() {		System.out.println("I read book");	}}
public class Monitor {	public static void main(String[] args) throws Exception {
		Book book = new Book();	Class<?>cls=book.getClass();	
System.out.println(book.getClass());System.out.println(cls)		;
	}

}
```

2，利用 类.class的形式取得Class类的对象;

```java
class Book {
	public void read() {
		System.out.println("I read book");}}
public class Monitor {
	public static void main(String[] args) throws Exception {
		Class<?>cls =Book.class;
		System.out.println(cls.getClass());
		System.out.println(cls);	}
}
```

3，利用Class类提供的一个方法完成，在系统架构中使用； 

```java
package org_chwenyulim_order;
class Book {
	public void read() {
		System.out.println("I read book");	}}
public class Monitor {
	public static void main(String[] args) throws Exception {
		try {
			Class<?> cls = Class.forName("org_chwenyulim_order.Book");
			System.out.println(cls.getClass());
			System.out.println(cls);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```

```java
package org_chwenyulim_order;
import java.lang.reflect.Constructor;
class Book {
	String name;
	String writer;
	public Book(String name,String writer) {
		this.name=name;
		this.writer=writer;	}
	public void read() {
		System.out.println("I read book");	}
	public String toString() {
		return this.name+"的作者是："+this.writer;	}	}
public class Monitor {
	public static void main(String[] args) throws Exception {
		Class <?>cls =Class.forName("org_chwenyulim_order.Book");
//		//此时返回的是Class的一个对象，我转型成为Book类的对象
//		Book book =(Book)cls.newInstance();
		Constructor<?>con =cls.getConstructor(String.class,String.class);
		//通过构造方法初始化对象
		Book book =(Book)con.newInstance("围城","钱钟书");//
		book.read();
		System.out.println(book);
		System.out.println(con);
	}}

```

**26.3JAVA反射机制应用**

**一个公司要开发本公司APP,某程序员在支付功能处给出了两种解决方案-----**

第一种---------
利用多态来完成

第二种--------
是利用反射机制完成

**代码实现**

**26.31代码实现----多态**

```java
 class AlipayM  implements WoerMa{
    @Override
    public void payOnline() {
        System.out.println("阿里支付");
    }
}
class WechatPayM implements WoerMa {
    @Override
    public void payOnline() {
        System.out.println("微信支付");
    }
}
class WoermaCard implements WoerMa {
    @Override
    public void payOnline() {
        System.out.println("沃尔玛钱包");
    }
}
public class ZhaoHang implements WoerMa{
    @Override
    public void payOnline() {
        System.out.println("招行支付");
    }
}

```

测试方法-----

```java
public class WoermaTest {
    public static void main(String[] args) {
        String string= "微信";
        switch(string){
            case "招行" :
                pay(new ZhaoHang());
                break;
            case "阿里":
                pay(new AlipayM());
                break;
            case "微信":
                pay(new WechatPayM());
                break;
            default:
                pay(new WoermaCard());
        }

    }
    public  static void pay(WoerMa woerMa){
        woerMa.payOnline();
    }

}
```

以上方式虽然用了多态的方式(参数为接口,传入的为参数的实现类)解决了一些问题,但仍然会有维护起来比较繁琐的问题既要维护实现类,又要维护测试类.

**26.32代码实现----反射**

需要修改测试类即可,

以后再次添加支付方式时只需要维护该实现类即可;

```java
public class WoermaTest {
    public static void main(String[] args) {
        String string= "com.woerma.client.ZhaoHang";
        try {
            Class client=Class.forName(string);//通过路径来获得类
          
            Object obj= client.newInstance();
 //在最新版本中通过newInstance()方法直接获得实例已被弃用;
 //此种方式要求的是类中有无参的构造
            Method method=client.getMethod("payOnline");
       method.invoke(obj);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

可以通过获得构造方法来间接获得实例;
**通过获得构造方法来间接获得实例适用于有参和无参的构造;**

```java
public class WoermaTest {
    public static void main(String[] args) {
        String string= "com.woerma.client.ZhaoHang";
        try {
            Class client=Class.forName(string);//通过路径来获得类
            Constructor constructor = client.getConstructor();
            Object obj = constructor.newInstance();
           
            Method method=client.getMethod("payOnline");
       method.invoke(obj);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }


}

```

**26.3创建对象的方式：**

1、使用Class对象的newInstance()方法创建该Class对象的实例，此时该Class对象必须要有无参数的构造方法,在最新的JDK版本中这种方式已被弃用;

2、使用Class对象获取指定的Constructor对象，再调用Constructor的newInstance（）方法创建对象类的实例，此时可以选择使用某个构造方法。如果这个构造方法被私有化起来，那么必须先申请访问，将可以访问设置为true；

**类中有无参构造-----**

```java
class Alibaba{

    public Alibaba() {
        System.out.println("创建对象成功");
    }
}
public class reflecTest {
    public static void main(String[] args) throws ClassNotFoundException {
        //创建对象
        //传统方式
        new Alibaba();
        //反射方式
        Class Ali=Class.forName("com.woerma.client.Alibaba");
         Ali.newInstance();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/652deb47b8674b1ba8c71756270de503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_54,color_FFFFFF,t_70,g_se,x_16)以上通过无参来直接获得类的实例的方式在新版本种已被废弃;





**类中没有无参构造------**

如果依旧是上面的代码----会出现错误,
![在这里插入图片描述](https://img-blog.csdnimg.cn/bea2787341f7413daf27954453f94e9d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_83,color_FFFFFF,t_70,g_se,x_16)

```java
class Alibaba{
String name;
int productNum;

    public Alibaba(String name, int productNum) {
        this.name = name;
        this.productNum = productNum;
        System.out.println(name+"------"+productNum);
        System.out.println("创建有参对象成功");

    }


}
public class reflecTest {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
        //创建对象
        //传统方式
        new Alibaba("圆珠笔",100);
        //反射方式
        Class Ali=Class.forName("com.woerma.client.Alibaba");
        Constructor constructor = Ali.getConstructor(String.class,int.class);//参数类型
       /*或者这个方法----   Constructor constructor = Ali.getDeclaredConstructor(String.class,int.class);*/
        Object o = constructor.newInstance("圆珠笔",100);
         }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7a5bd24bad314ca89957335354d80fdf.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_43,color_FFFFFF,t_70,g_se,x_16)
接下来看构造方法私有化的情况下

```java
class Alibaba{
private String name;
private int productNum;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getProductNum() {
        return productNum;
    }

    public void setProductNum(int productNum) {
        this.productNum = productNum;
    }

    private Alibaba(String name, int productNum) {
        this.name = name;
        this.productNum = productNum;
        System.out.println(name+"------"+productNum);
        System.out.println("创建有参对象成功");

    }
   static Alibaba alibaba= new Alibaba("圆珠笔",100);
public static Alibaba getInstance(){
return alibaba;
}
}
public class reflecTest {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
        //创建对象
        //传统方式
       Alibaba.getInstance();//单例设计
        //反射方式
        Class Ali=Class.forName("com.woerma.client.Alibaba");
     // Constructor constructor = Ali.getConstructor(String.class,int.class);//参数类型
        Constructor constructor = Ali.getDeclaredConstructor(String.class,int.class);
        constructor.setAccessible(true);//获得访问权限
        Object o = constructor.newInstance("圆珠笔",100);


    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d8bbfbf6bd5d438e9f9b55bbaee2cb07.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_53,color_FFFFFF,t_70,g_se,x_16)

**getDeclaredConstructor()与getConstructor()的区别**

从代码中观察----

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

class A{
    String name;
    public A() {
        System.out.println("获得了无参实例");
    }
   private A(String name){
        System.out.println("获得了有参实例");
    }

}
public class ReflectTest0001 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class cla=Class.forName("com.woerma.client.A");
        Constructor constructor = cla.getConstructor(String.class);
        Constructor declaredConstructor = cla.getDeclaredConstructor(String.class);
        constructor.setAccessible(true);
        constructor.newInstance("李四");
        declaredConstructor.setAccessible(true);
        declaredConstructor.newInstance("张三");
    }
}
```

在访问公有构造器时两者都可以,但是当用getConstructor()访问私有构造器时取出现了----错误
![在这里插入图片描述](https://img-blog.csdnimg.cn/b7abc626e8d9414a872a1b1575feb4c0.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_99,color_FFFFFF,t_70,g_se,x_16)所以两者区别如下----
**getConstructor()返回指定参数类型public的构造器。
getDeclaredConstructor()返回指定参数类型的private和public构造器。**
同时要注意在访问私有构造时要先设定方位权限为true,不然getDeclaredConstructor()也不能访问;

由此来看单例设计也并不安全,

**来看枚举类中的反射情况-----**

```java
enum  ThreeColor{
   RED("红"),
    GREEN("绿"),
     YELLOW("黄");
  String desc;
    ThreeColor(String desc){
       this.desc=desc;
   }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }
}
public class ReflectTest0001 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class cla=Class.forName("com.woerma.client.ThreeColor");
        Constructor declaredConstructor = cla.getDeclaredConstructor(String.class);
        declaredConstructor.setAccessible(true);
        declaredConstructor.newInstance("红");
    }
}

```

对枚举而言,连构造器都获得不了,所以没法获得实例,所以对于实例个数限定的可以用枚举来以确保内存的安全性

同理就可以理解     对应其他的getDeclaredxxx()方法和getxxx()方法的区别

**创建对象之前的工作----加载类的字节码信息**

四种方式可以加载类的字节码信息-----

```java
class Student{}

public class Person {
    public static void main(String[] args) throws ClassNotFoundException {
        //方式一----直接通过系统内置类加载器获得
        Class aClass = Student.class;
        //方式二----通过调用对象的getClass()方法获得
        Student per= new Student();
        Class aClass1 = per.getClass();
        System.out.println(aClass);
        //方式三---
        Class aClass2=Class.forName("com.gavin.flect.Student");
        //方式四-----
        ClassLoader classLoader=Person.class.getClassLoader();//获得来的加载器
        Class aClass3 = classLoader.loadClass("com.gavin.flect.Student");//通过类加载器来完成字节码信息的加载
        System.out.println(aClass==aClass1);
        System.out.println(aClass1==aClass2);
        System.out.println(aClass2==aClass3);
    }
}

```

最常用的为第三种;

**通过反射还能获取啥信息?**

简单地说是能获取该类在运行时所有的信息-----属性,方法等;
也能获得父类的一些信息;

**获得属性,方法,注解------**

通过反射获得属性(包括修饰符,属性名)和方法(包括参数类型,返回值)----

```java
package com.gavin.flect;

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

/**
 * @author : Gavin
 * @date: 2021/8/26 - 08 - 26 - 15:47
 * @Description: com.gavin.flect
 * @version: 1.0
 */

 class FatherTest {
    @MySon(name="李二狗他爸",age=48)
    void eat(String food){//default
        System.out.println("我爱吃----"+food);
    }
}
class SonTest extends FatherTest{
     //属性区
     public static String name;
     private int age;
     String gender;
//构造方法区

    public SonTest(String name, int age) {
        this.name = name;
        this.age = age;
    }

    private SonTest(String name) {
        this.name = name;
    }

    //方法区
    @MySon(name="李二狗",age=18)
     void eat(String food){//default
         System.out.println("我爱吃----"+food);
     }
     public String show(String show,int age){
         return "我"+age+"岁就会表演---"+show;
     }
     protected int getAge(){
         return 19;
     }

     private static void call(){
         System.out.println("有事给我打电话");
     }
}
```

**测试代码---获取属性相关信息**

```java
class Test{
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
       Class cla=Class.forName("com.gavin.flect.SonTest") ;
       //获得属性
        Field name = cla.getField("name");//getField()只能获得public修饰的属性
        System.out.println(name);
        System.out.println(name.getName());//获取属性名
        int modifiers = name.getModifiers();//获取属性修饰符
        System.out.println(Modifier.toString(modifiers));//打印修饰符
        System.out.println("1-----------------");
        Field[] shuxing = cla.getDeclaredFields();//getDeclaredFields可获得所有
        for ( Field f:shuxing
             ) {
            System.out.println(f.toString());
        }    
    }
}


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/03830f107182469eb3775a5b7a67ddba.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_56,color_FFFFFF,t_70,g_se,x_16)

**测试代码---获取方法相关信息**

```java
class Test{
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException {
       Class cla=Class.forName("com.gavin.flect.SonTest") ;

        //获取方法
        Method[] dMethods = cla.getMethods();//获取当前类中的所有被public修饰的方法包括父类中的被public修饰的
        for (Method d:dMethods
             ) {
            System.out.println(d);
        }
        System.out.println("1----------------------");
        Method show = cla.getMethod("show", String.class, int.class);//getMethod只能获得public修饰的方法
        System.out.println(show);
        int modifiers = show.getModifiers();//获得方法的修饰符
        System.out.println(Modifier.toString(modifiers));//打印修饰符
        System.out.println("2----------------------");
        Class cla1=Class.forName("com.gavin.flect.SonTest") ;
        Method[] declaredMethods1 = cla1.getDeclaredMethods();//获得当前类中的所有方法---不包括父类
        for (Method d :
                declaredMethods1) {
            System.out.println(d);
        }

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/fe92c020ab734b12b1e3ca9831aa2d08.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_86,color_FFFFFF,t_70,g_se,x_16)

**测试代码---获取注解相关信息**

```java
class Test {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException {
        Class cla = Class.forName("com.gavin.flect.SonTest");

        Method eat = cla.getDeclaredMethod("eat", String.class);
        Annotation[] declaredAnnotations = eat.getDeclaredAnnotations();//得到该方法上的所有注解----RetentionPolicy.RUNTIME条件下的
        for (Annotation an :
                declaredAnnotations) {
            System.out.println(an);
        }
        Annotation annotation = eat.getAnnotation(MySon.class);
        System.out.println(annotation);
       
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/3f3da87b6c764fb38e61ebb1d092036a.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_58,color_FFFFFF,t_70,g_se,x_16)
**在SonTest类中补充方法funmethod()----获取参数的注解----**

```java
 private void funmethod(@MySon int a,@Myfather(height = 188.8) double b){

    }
```

测试类---

```java
class Test {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException {
        Class cla = Class.forName("com.gavin.flect.SonTest");
        Method funmethod = cla.getDeclaredMethod("funmethod", int.class, double.class);
        Annotation[][] parameterAnnotations = funmethod.getParameterAnnotations();
        for (Annotation [] p:parameterAnnotations
             ) {
            for (Annotation pp:p
                 ) {
                System.out.println(pp);
            }
        }

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/fe0918dd13914820b0c8e719e9430a58.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_55,color_FFFFFF,t_70,g_se,x_16)

**通过反射调用方法**

```java
class Test {
    public static void main(String[] args) throws Exception {
        Class cla = Class.forName("com.gavin.flect.SonTest");//加载类字节码信息
        Constructor de = cla.getDeclaredConstructor(String.class);//获得构造方法
        de.setAccessible(true);//私有构造要设置权限可访问
        Object o = de.newInstance("李二狗");//实例化----可以转型也可以不转
        Method eat=cla.getDeclaredMethod("eat", String.class);//获得方法
      eat.invoke(o,"鱼");//运行方法----invoke(对象,方法参数)

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5cdac684fe44901890285e8451eb51e.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_Q1NETiBAR2F2aW5fTGlt,size_52,color_FFFFFF,t_70,g_se,x_16)


## 27.枚举

语法格式： enum 枚举名 {    要枚举的所有元素           }

enum  WeekDay { “周一”.”周二”.”周三”.”周四”.”周五”.”周六”.”周日” }

只要是WeekDay类型的数据.只能是周一到周日之中的数据；

在java中.enum也被定为一种类因此 也有自己的方法 

```java
package org_chwenyu_learn;
public class Meiju {
	enum Week {
		Sun. Mon.Tue.Wed.Thu.Fri.Sat		}
	// 枚举类型语法格式--enum 枚举名{元素1.元素2};
	// 枚举时一定要将所有元素全部列举。
	public static void main(String[] args) {
		////////////////////////
		// 枚举名 对象名= 枚举名.枚举元素
		Week c1 = Week.Sat;
		switch (c1) {
		case Sat:			System.out.println("今天周六");			break;
		default: {			System.out.println("今天上学");	}		}
		Week[]allWeek=Week.values();//遍历枚举对象		
		for (Week aWeek:allWeek) {			System.out.print(aWeek+".");		}
		System.out.println("");
		System.out.println(c1.getDeclaringClass());//返回c1所在的枚举名
		System.out.println(c1.hashcode());//返回枚举常量的哈希码
		System.out.println(c1.name());//返回枚举常量的名称（在定义时声明的）
		System.out.println(c1.ordinal());//返回枚举常量在枚举声明中的位置.序数从0开始
		System.out.println(c1.toString());//返回枚举常量的名称。}}


```

![](..\pic\图片64.png)

枚举也可以像数组一样进行遍历；通过values()方法进行遍历；

语法格式：

```java
enum Mycolor{“1”.”2”.”3”};

Mycolor [] allcolor =Mycolor.values();

for(Mycolor acolor:allcolor){

System.out.print(acolor);

}

```

1，如果enum定义的枚举类访问权限定义为public.则需要单独形成一个.java文件.即不可与包含main方法的public类同处于同一个文件。如果enum定义的枚举类访问权限为默认类型.即enum关键字前没有修饰符.则enum定义的对象可在同一个包里访问.无需重复定义。例如.前面的范例都在一个包中.则所定义的枚举类型Color（MyColor）事实上仅需要在一个.java文件定义即可.其他用到这个枚举类型的文件可以通过导入（import）的模式来复用这个定义。

```java
public class Meiju {
public enum Week {		Sun. Mon.Tue.Wed.Thu.Fri.Sat		}
	// 枚举类型语法格式--enum 枚举名{元素1.元素2};
	// 枚举时一定要将所有元素全部列举。
	//public static void main(String[] args) 
//当定义一个enum类.就不能在下面再有一个包含main方法的类。

```

```java
abstract class A{
	public A() {
		this.print();	}
	public abstract void print();}
class B extends A{
	private int x =100;
	public B (int x) {
		this.x=x;	}
	@Override
	public void print() {
		// TODO Auto-generated method stub
		System.out.println("x="+x);	}	}
public class Lequ {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
A a =new B(10);  //输出 0
	}}


```

子类对象实例化过程中会先调用父类的

构造方法.之后再执行子类的构造方法

父类构造方法执行完之后实际上视为父类

中的属性初始化了.但是如果未执行完

（即子类的构造方法还没被调用）.那么

子类中的所有属性都无法初始化.则子类

中的属性默认值都为默认值；

27.1.枚举的作用

在C语言中存在一个枚举类型，通过此类型可以限制内容的取值范围，但是在JDK1.5之前Java中并不存在枚举类型；

27.1.1枚举的实现--旧的方式

JDK1.5之前要想实现枚举的操作则需要通过以下的方式实现--单例设计

回顾单例设计模式--

私有属性

私有构造方法

Setter与Gertter

public static 类名 getInstanceof(参数){   } 

```java
class Color{
	private String color;
	private Color(String color) {
		this.setColor(color);	}
	public static final Color RED= new Color("红");
	public static final Color YELLOW= new Color("黄");
	public static final Color GREEN= new Color("绿");
	public String getColor() {
		return color;	}
	public void setColor(String color) {
		this.color = color;	}
	public static Color getInstance( int i) {
		if (i ==0) {
			return RED;			}
		if(i==1) {
			return GREEN;		}
		if(i==2) {
			return YELLOW;		}
		return null;	}}
public class EnumDemo {
	public static void main(String[] args) {
		Color c=Color.getInstance(1);
		System.out.println(c.getColor());	}}

```



另一种实现枚举的方式是通过接口--   

```java
interface Color{
	public  int RED=1;
	public int GREEN=2;
	public static final int YELLOW=0;}

```

但是通过以上方式实现枚举会存在问题--操作结果不正确的问题，而且也不能明确知道枚举的取值范围

![](..\pic\图片65.png)

![图片66](..\pic\图片66.png)

27.2.枚举类的定义

```java
public enum Week{//定义枚举类
	SAT,SUN,MON,TUE,WED,THIR,FRI;
}

```

```java
import java.util.Scanner;
public class WeekDemo {
	public static void main(String[] args) {
		Week a = Week.MON;		Week b = Week.TUE;
		Week c = Week.WED;		Week d = Week.THIR;
		Week e = Week.MON;		Week f = Week.SAT;
		Week g = Week.SUN;
		Scanner input = new Scanner(System.in);
		String S = input.next();
		switch (S) {
		case "a":		case "b":		case "c":		case "d":		case "e":
			System.out.println("工作日");			break;
		default:			System.out.println("周末休息");		}	}

```

27.2.1.Enum类和enum关键字的区别

使用关键字定义枚举，实际上是继承了Enum类；

```java
Class Enum<E extends Enum<E>>
java.lang.Object
java.lang.Enum<E>
Type Parameters:
E - The enum type subclass
All Implemented Interfaces:
Serializable, Comparable<E>
public abstract class Enum<E extends Enum<E>>
extends Object
implements Comparable<E>, Serializable
```

```java
Constructors
Modifier	Constructor	Description
protected	Enum (String name, int ordinal)	
Sole constructor.
```

在Enum类的构造方法中是受保护的类型，实际上对于每一个枚举对象，一旦声明之后，就表示调用此构造方法，所有的编号采用自动编号的方式进行； 

![](..\pic\图片67.png)

valueOf

```java
public static <T extends Enum<T>> T valueOf(Class<T> enumType, String name)
```

返回值：

具有指定名称的指定枚举类型的枚举常量

![](..\pic\图片68.png)

27.3.类集对枚举的支持

在JDK1.5之后增加了两个类集的操作类，EnumSet、EnumMap，

27.3.1.EnumMap

定义--

```java
Class EnumMap<K extends Enum<K>,V>
java.lang.Object
java.util.AbstractMap<K,V>
java.util.EnumMap<K,V>
All Implemented Interfaces:
Serializable, Cloneable, Map<K,V>
public class EnumMap<K extends Enum<K>,V>
extends AbstractMap<K,V>
implements Serializable, Cloneable
```

![](..\pic\图片69.png)

用于枚举类型键的专用映射实现。枚举映射中的所有键必须来自在创建映射时显式或隐式指定的单个枚举类型。枚举映射在内部表示为数组。这种表示非常紧凑和高效。
枚举映射按其键的自然顺序（声明枚举常量的顺序）进行维护。这反映在集合视图（keySet（）、entrySet（）和values（）） 返回的迭代器中。

集合视图返回的迭代器是弱一致的：它们永远不会抛出ConcurrentModificationException，并且它们可能会或可能不会显示迭代过程中对地图进行任何修改的影响。

不允许使用空键。尝试插入空键将引发NullPointerException。但是，尝试测试是否存在空键或删除空键将正常运行。允许空值。

与大多数集合实现一样，它不同步。如果多个线程同时访问枚举映射，并且至少有一个线程修改了映射，则应在外部同步该映射。这通常是通过在自然封装枚举映射的某些对象上进行同步来实现的。如果不存在这样的对象，则应使用Collections.syncedMap（java.util.Map<K， V>）方法对映射进行"包装"。最好在创建时执行此操作，以防止意外的未同步访问：EnumMap

```java
 Map<EnumKey, V> m
     = Collections.synchronizedMap(new EnumMap<EnumKey, V>(...));
```

```java
import java.util.*;
import java.util.Map.Entry;
public class WeekDemo {
	public static void main(String[] args) {
		EnumMap<Week,String> emap=new EnumMap<Week,String>(Week.class);//指定枚举类型
		emap.put(Week.MON, "星期一");
		emap.put(Week.FRI, "星期五");
		emap.put(Week.SUN, "星期日");
		Set<Map.Entry<Week, String>>set =emap.entrySet();//set接口取出Map.Entry
		Iterator<Map.Entry<Week, String>>ite=set.iterator();
		while(ite.hasNext()) {
			Map.Entry<Week, String>me=(Map.Entry<Week, String>)ite.next();
			System.out.print(me.getKey()+"------->");
			System.out.println(me.getValue());
		}
		for (Entry<Week, String> entry : set) {
			System.out.println(entry);		}	}}


```

27.3.2.EnumSet

定义--

```java
Class EnumSet<E extends Enum<E>>
java.lang.Object
java.util.AbstractCollection<E>
java.util.AbstractSet<E>
java.util.EnumSet<E>
All Implemented Interfaces:
Serializable, Cloneable, Iterable<E>, Collection<E>, Set<E>
public abstract class EnumSet<E extends Enum<E>>
extends AbstractSet<E>
implements Cloneable, Serializable
```

用于枚举类型的专用Set实现。枚举集中的所有元素都必须来自在创建枚举集时显式或隐式指定的单个枚举类型。枚举集在内部表示为位向量。这种表示非常紧凑和高效。此类的空间和时间性能应该足够好，以允许其用作基于传统的"位标志"的高质量、免类型替代方案。即使是批量操作（如 和 ），如果其参数也是枚举集，也应该非常快地运行。intcontainsAllretainAll
该方法返回的迭代器按元素的自然顺序（声明枚举常量的顺序）遍历元素。返回的迭代器是弱一致的：它永远不会抛出ConcurrentModificationException，并且它可能会或可能不会显示迭代过程中发生的对集合的任何修改的效果。iterator

不允许使用空元素。尝试插入空元素将引发NullPointerException。但是，尝试测试是否存在 null 元素或删除 null 元素将正常运行。

与大多数集合实现一样，它不同步。如果多个线程同时访问枚举集，并且至少有一个线程修改了该集，则应在外部同步该枚举集。这通常是通过在自然封装枚举集的某些对象上进行同步来实现的。如果不存在这样的对象，则应使用Collections.syncset（java.util.Set<T>）方法对集合进行"包装"。最好在创建时执行此操作，以防止意外的未同步访问：EnumSet

 

```java
Set<MyEnum> s = Collections.synchronizedSet(EnumSet.noneOf(MyEnum.class));
```

实现说明：所有基本操作都在常量时间内执行。它们可能（尽管不能保证）比它们的HashSet对应物快得多。即使批量操作在恒定时间内执行，如果其参数也是枚举集。

此类是Java 集合框架 的成员。

```java
import java.util.*;
public class Weekend {
	public static void main(String[] args) {
		EnumSet<Week> eset = EnumSet.allOf(Week.class);//创建一个枚举集，其中包含指定元素类型中的所有元素。
EnumSet<Week>weeks = eset.clone();//返回一个克隆
		Iterator<Week> itt = eset.iterator();
		while (itt.hasNext()) {
			// Map.Entry<Week,String >mm=(Map.Entry<Week, String>)itt.next();
			System.out.println(itt.next());		}	}}

```

```java
public static <E extends Enum<E>> EnumSet<E> allOf(Class<E> elementType)
Creates an enum set containing all of the elements in the specified element type.
//以上是将全部内容设置到集合之中，当然也可以指定类型的空集合

```



```java
public class Weekend {
	public static void main(String[] args) {
		EnumSet<Week> eset = EnumSet.noneOf(Week.class);// 表示此类型的空集合
		Iterator<Week> itt = eset.iterator();
		EnumSet<Week> weeks = eset.clone();// 返回一个克隆
		for (Week week : eset) {
			System.out.println(eset);		}
		while (itt.hasNext()) {
			// Map.Entry<Week,String >mm=(Map.Entry<Week, String>)itt.next();
			System.out.println(itt.next());		}	}}


```

 

27.4.枚举的深入

27.4.1定义构造方法

枚举中可以直接定义构造方法，一旦构造法官法定义之后，则所有的枚举对象都要调用此构造方法；枚举中的构造方法不能呢个狗定义为public，因为外部不能调用枚举的构造方法，那么此时每一个枚举对象都有其自己的name属性；

```java
public enum Week {// 定义枚举类
	// 一旦有枚举构造，那么就要使用枚举的构造方法
	SAT("星期六"), SUN("星期日"), MON("星期一"), 
	TUE("星期二"), WED("星期三"), THIR("星期四"), 
	FRI("星期五");
	private String name;
	public String getName() {
		return name;	}
	public void setName(String name) {
		this.name = name;	}
	/*
	 * 枚举类的构造方法是不能用public声明的， 因为外部不能够调用枚举的构造方法
	 */
	Week(String name) {// 一旦定义枚举构造，那么就要使用枚举的构造方法
		this.name = name;	}}
public class Weekend {
	public static void main(String[] args) {
		for (Week week : Week.values()) {
			System.out.print(week.ordinal() + "--");
			System.out.println(week + "-----" + week.getName());
		}	}}

```

27.4.2.枚举实现接口

一个枚举实现一个接口之后，各个枚举对象都必须分别实现接口中的抽象方法；

```java
public interface Info {
	public String getWeek();
}
public enum Week implements Info {// 定义枚举类
	// 一旦有枚举构造，那么就要使用枚举的构造方法
	SAT {
		public String getWeek() {
			return "星期六";
		}
	},
	SUN {
		public String getWeek() {
			return "星期日";
		}
	},
	MON {
		public String getWeek() {
			return "星期一";
		}
	},
	FRI {
		public String getWeek() {
			return "星期一";
		}
	}}

```

```java
public class WeekDemo1 {
	public static void main(String[] args) {
for (Week weeks : Week.values()) {
	System.out.println(weeks.ordinal()+"--"+weeks.getWeek());
}	}}

```



27.4.3.在枚举中定义抽象方法

在枚举中也可以定义多个抽象方法，则枚举中的各个对象必须单独实现此方法；

```java
public enum Week {// 定义枚举类
	//枚举类一定要先定义枚举的内容；
	// 枚举类实现接口，那么各个枚举对象也要实现该方法
	SAT {
		public String getWeek() {
			return "星期六";
		}
	},
	SUN {
		public String getWeek() {
			return "星期日";
		}
	},
	MON {
		public String getWeek() {
			return "星期一";
		}
	},
	FRI {
		public String getWeek() {
			return "星期一";
		}
	};
	public abstract String getWeek();
}
public class WeekDemo1 {
	public static void main(String[] args) {	
for (Week weeks : Week.values()) {
	System.out.println(weeks.ordinal()+"--"+weeks.getWeek());
}	}
}
/使用枚举可以限制那个的取值范围
可以在枚举中定义抽象方法和接口
类集对枚举本身也是有良好的支持的
使用enum关键字也就相当于继承Enum类*/

```

## 28.JAVA类集框架

1，Collection接口，List接口，Set接口的作用以及关系

2，Map接口的作用

3，类集的四种输出方式以及使用区别

4，类集的常用子类

28.1类集

类集的概念是从JDK1.2之后正式完善的一套开发架构，其基本作用就是完成了一个动态的对象数组，利民的数据元素可以动态增加。

在类集中提供了一下几种接口：

单值操作接口：Collection 、List、Set     其中List和Set接口是Collection接口的子接口

一对值的操作接口：Map

排序的操作接口：SortedMap 、SortedSet

输出的接口：Iterator、 ListItertor、Enumeration

队列：Queue

28.1.1.List接口

常用List接口子类 ArrayList、Vector 、LinkedList

```java
Interface List<E>
Type Parameters:
E - the type of elements in this list
All Superinterfaces:
Collection<E>, Iterable<E>
All Known Implementing Classes:
AbstractList, AbstractSequentialList, ArrayList, AttributeList, CopyOnWriteArrayList, LinkedList, RoleList, RoleUnresolvedList, Stack, Vector
public interface List<E>
extends Collection<E>
```

有序集合（也称为序列）。此界面的用户可以精确控制每个元素在列表中的插入位置。用户可以按元素的整数索引（在列表中的位置）访问元素，并在列表中搜索元素。
与集合不同，列表通常允许重复的元素。更正式地说，列表通常允许元素对，并且如果它们允许空元素，它们通常允许多个空元素。有人可能希望通过在用户尝试插入运行时异常时抛出运行时异常来实现禁止重复的列表，这并非不可想象，但我们预计这种用法很少见。e1e2e1.equals(e2)

除了接口中指定的规定之外，该接口还对 、 、 、 和方法的协定进行了额外的规定。为方便起见，此处还包含其他继承方法的声明。ListCollectioniteratoraddremoveequalshashCode

该接口提供了四种用于对列表元素进行位置（索引）访问的方法。列表（如 Java 数组）从零开始。请注意，这些操作可能会在与某些实现（例如类）的索引值成比例的时间内执行。因此，如果调用方不知道实现，则循环访问列表中的元素通常比通过列表编制索引更可取。ListLinkedList

该接口提供了一个特殊的迭代器，称为 ，除了接口提供的正常操作外，它还允许元素插入和替换以及双向访问。提供了一个方法来获取从列表中的指定位置开始的列表迭代器。ListListIteratorIterator

该接口提供了两种方法来搜索指定的对象。从性能的角度来看，应谨慎使用这些方法。在许多实现中，它们将执行昂贵的线性搜索。List

该接口提供了两种方法，用于在列表中的任意点有效地插入和删除多个元素。List

注：虽然允许列表本身作为要素，但建议极为谨慎：和方法在此类列表中不再有明确定义。equalshashCode

某些列表实现对它们可能包含的元素有限制。例如，某些实现禁止 null 元素，而某些实现对其元素的类型有限制。尝试添加不符合条件的元素会引发未经检查的异常，通常为 或 。尝试查询是否存在不合格的元素可能会引发异常，或者可能只是返回 false;一些实现将表现出前一种行为，而另一些实现将表现出后者的行为。更一般地说，尝试对不合格的元素执行操作，如果该元素的完成不会导致将不合格的元素插入到列表中，则可能会引发异常，或者可能会成功，这取决于实现。此类异常在此接口的规范中标记为"可选"。NullPointerExceptionClassCastException

不可修改的列表
List.of和List.copyOf静态工厂方法提供了一种创建不可修改列表的便捷方法。这些方法创建的实例具有以下特征：List

它们是不可修改的。不能添加、删除或替换元素。调用 List 上的任何突变体方法将始终导致被抛出。但是，如果包含的元素本身是可变的，则可能导致列表的内容显示为更改。UnsupportedOperationException
他们不允许元素。尝试使用元素创建它们会导致 。nullnullNullPointerException
如果所有元素都是可序列化的，则它们是可序列化的。
列表中元素的顺序与提供的参数或提供的数组中元素的顺序相同。
它们基于价值。调用方不应对返回的实例的标识进行任何假设。工厂可以自由地创建新实例或重用现有实例。因此，对这些实例执行的标识敏感操作（引用相等 （）、标识哈希代码和同步）不可靠，应避免使用。==
它们按照"序列化表单"页上的指定进行序列化。

```java
import java.util.ArrayList;
public class Ceshi {
	public static void main(String args[]) throws Exception {
ArrayList<String>list=new ArrayList();//建立一个初始容量为10的空列表
//E -此列表中元素的类型
//add(E e);将指定的元素追加到此列表的末尾（可选操作）。
list.add("一个元素");
list.add("需要移除的元素");
System.out.println(list);
System.out.println("----插入一个元素----");
list.add(1, "插入的元素");
System.out.println(list);
System.out.println("移除一个元素");
list.remove(2);
//list.add(10,"第十个元素");只能插在已有序号的后面，不能越过空的去插
System.out.println(list);
System.out.println("清空所有元素");
list.clear();//清空List有元素
list.add("清空后再次插入的元素");
System.out.println(list);
ArrayList<String> list2 =new ArrayList ();
list2.add(0, "0");
list2.add("0");
list.addAll(list2);
list.addAll(0, list);//可以插入自身的元素
System.out.println("将list2全部元素添加到list末尾"+list);	Object []obj=list.toArray();
for (String string : list) {
	System.out.println(string);}
System.out.println("要查找的元素为---"+list.get(0));}}

```

```java
	public static void main(String args[])  {
ArrayList<String>list=new ArrayList<String>();//建立一个初始容量为10的空列表
ArrayList<String>list2=new ArrayList<String>();
list.add("首先");list.add("首先1");list.add("首先2");list.add("首先3");
list2=(ArrayList<String>) list.clone();//	返回此ArrayList实例的浅表副本。
System.out.println(list2);
System.out.println("list2与list引用同一个对象？--"+list2.equals(list));
	System.out.println("list2是包含“首先”元素--"+list2.contains("首先"));
	list2.add("添加元素");
	System.out.println("list2是包含list的所有元素--"+list2.containsAll(list));	
	//ArrayList如有必要，增加此实例的容量，以确保它至少可以容纳最小容量参数指定的元素数量。
	list.set(0, "element");
	System.out.println("替换后的元素集合为 "+list);
	list.removeAll(list2);
	System.out.println(list);
	System.out.println(list.size());	
System.out.println(list.sublist(0,1));//截取指定区间的元素}}

```

28.1.1.1.ArrayList

ArraysList是List---AbstractList--常用的一个接口，其定义如下

public class ArrayList<E>extends AbstractList<E>

implements List<E>, RandomAccess, Cloneable（可克隆）, Serializable（可序列化）

使用ArraysList实例化

```java
import java.util.*;
public class Wind {
    public static void main(String args[]) {
        ArrayList<String>all=new ArrayList<String>();//为List接口实例化
        all.add("0");//增加元素，Collection接口定义
        all.add(1, "1");//增加元素，List接口定义
        all.add("元素");
        all.addAll(all);//增加元素Collection 接口的定义
        all.remove(2);//按索引删除List接口定义
        all.remove("不存在的元素");//删除元素--Collection 接口的定义
        all.remove("元素");//删除元素--Collection 接口的定义
        System.out.println(all); 
//取出所有数据--循环输出
		for (int i =0;i<all.size();i++) {
			System.out.println(all.get(i));		}
System.out.println("-----------------");
		Object obj[]=all.toArray();//将集合转变为数组
		for (Object object : obj) {
			System.out.println(object);		}
System.out.println("是否包含元素“1”--"+all.contains("1"));
		System.out.println("截取后为--"+all.subList(1, 3));
       all.removeAll(all);//删除元素--Collection 接口的定义
        System.out.println("全被移除了"+all);    } }

```

此代码中程序没有数组长度限制，可以添加和删除元素

在以上代码使用remove(Object obj)的时候使用的是String，所以可以删掉，如果现在是一个自定义的匿名对象，则可能无法删除；

集合的其他操作方法判断元素是否存在--contains()方法

List接口中定义了subList()方法

完成输出数据----

在List接口中提供了一个get(int index)方法，此方法可以根据索引位置取出数据 

在Collection接口中也规定了两个可以将集合变为数组的操作--toArray

也可以在输出的时候指定泛型的类型，这样操作起来也比较方便；

![](..\pic\图片70.png)



以上代码在输出是指定泛型类型----代码就变为-以下 

![](..\pic\图片71.png)



```java
import java.util.Enumeration;
import java.util.TreeSet;
import java.util.Vector;
import java.util.Iterator;
class Book {
    private String title;
    private float price;
    public Book(String title, float price) {
        this.title = title;
        this.price = price;    }
    public String getTitle() {
        return title;    }
    public float getPrice() {
        return price;    }
    public String toString() {
 return "书名" + this.title + "价格" + this.price;    }}
public class Jishi {
    public static void main(String args[]) {
        // HashSet<Book> hs = new HashSet<Book>();
        // hs.add(new Book("java", 12.5f));
        // hs.add(new Book("C++", 25.5f));
        // hs.add(new Book("C#", 30.5f));
        // hs.add(new Book("PHP", 45.5f));
        // hs.add(new Book("Swift", 62.5f));


```



28.1.1.2.旧的子类Vector

Vector类是一个元老级的操作类，JDK1.0时就已经存在，JDK1.2之后才出现了ArrayList类，因为一部分人已经习惯使用Vector类，于是java开发者在定义类集的时候又让Vector类多实现了一个List接口，所以此类可以直接为List直接实例化；

![](..\pic\图片72.png)

在Vector中也有自己特有的增加、删除元素的方法

public void addElement([E](file:///C:/Users/linch/Desktop/jdk-15.0.1_doc-all/docs/api/java.base/java/util/Vector.html) obj)

public boolean removeElement([Object](file:///C:/Users/linch/Desktop/jdk-15.0.1_doc-all/docs/api/java.base/java/lang/Object.html) obj)

public void removeAllElements()

这些方法跟List中是以一样的，所以我们就经常用List中的方法为标准进行使用；

```java
        // Book book[] = hs.toArray(new Book[] {});
        // for (int i = 0; i < book.length; i++) {
        // Book book1 = book[i];
        // System.out.println(book1);
        // }
        // Iterator<Book> it = hs.iterator();
        // while (it.hasNext()) {
        // System.out.println(it.next());
        // }
        Vector<Book> hs = new Vector<Book>();
        hs.add(new Book("java", 12.5f));
        hs.add(new Book("C++", 25.5f));
        hs.add(new Book("C#", 30.5f));
        hs.add(new Book("PHP", 45.5f));
        hs.add(new Book("Swift", 62.5f));
        // Vector 才能使用elements()方法
        Enumeration<Book> con = hs.elements();
        while (con.hasMoreElements()) {
            System.out.println(con.nextElement());
        }    }}


```

28.1.1.3ArrayList与Vector的区别

ArrayList与Vector都是List接口的子类，区别在于----

| NO   | 区别点   | ArrayList                              | Vector                                          |
| ---- | -------- | -------------------------------------- | ----------------------------------------------- |
| 1    | 推出时间 | JDK1.2之后                             | JDK1.0时推出，属于旧的操作类                    |
| 2    | 操作     | 异步操作，执行效率高，安全性不如Vector | 同步操作，执行效率不如ArrayList高，但是安全性好 |
| 3    | 性能     | 高                                     | 相对较低                                        |
| 4    | 安全     | 非线程安全的操作                       | 线程安全的操作                                  |
| 5    | 输出     | Iterrator、ListIterrator、foreach      | Iterrator、ListIterrator、foreach、Enumeration  |

28.1.1.4.LinkedList和Queue接口

LinkedList完成的是一个链表操作，可以方便的找到表头之类的基本操作。

此类的定义如下--

```java
Class LinkedList<E>
java.lang.Object
java.util.AbstractCollection<E>
java.util.AbstractList<E>
java.util.AbstractSequentialList<E>
java.util.LinkedList<E>
Type Parameters:
E - the type of elements held in this collection
All Implemented Interfaces:
Serializable, Cloneable, Iterable<E>, Collection<E>, Deque<E>, List<E>, Queue<E>
public class LinkedList<E>
extends AbstractSequentialList<E>
implements List<E>, Deque<E>, Cloneable, Serializable
```

```java
import java.util.*;
public class Wind {
	public static void main(String args[]) {
		LinkedList<String> all = new LinkedList<String>();// 为List接口实例化
		all.add("0");all.addFirst("2");	all.add(1, "1");all.addLast("元素");
		all.addAll(all);	System.out.println(all);
		System.out.println("链表头--"+all.getFirst());
		System.out.println("链表尾--"+all.getLast());
		System.out.println("指定1位置的元素--"+all.get(1));
		all.offer("尾部");//"将指定元素插到链表尾部"
		all.offerFirst("哈");//在链表头添加元素
		all.offerLast("笑");//在链表尾部添加元素
		System.out.println("检索de第一个元素--"+all.peek());//检索但不删除此列表的第一个元素，如果此列表为空，则返回null。
		System.out.println("检索de第一个元素--"+all.peekFirst());
		System.out.println("检索de最后一个元素--"+all.peekLast());
		all.push("我在");//在表头添加元素
		System.out.println("删除并返回链表第一个元素--"+all.pop());
			System.out.println("poll之前--"+all);
		for(int i=0;i<all.size();i++) {
			System.out.print(all.poll()+"、");		}
		System.out.println("\n"+"poll--之后"+all);
			}}

```

28.1.2.Set接口

```java
Interface Set<E>
Type Parameters:
E - the type of elements maintained by this set
All Superinterfaces:
Collection<E>, Iterable<E>
All Known Subinterfaces:
EventSet, NavigableSet<E>, SortedSet<E>
All Known Implementing Classes:
AbstractSet, ConcurrentHashMap.KeySetView, ConcurrentSkipListSet, CopyOnWriteArraySet, EnumSet, HashSet, JobStateReasons, LinkedHashSet, TreeSet
public interface Set<E>
extends Collection<E>
```

Set接口最大的特点是里面没有任何重复的元素；在Set接口中有以下的两个子类是最常用的子类--

TreeSet--有序存放

HashSet--散列存放

Set接口本身并没有对Collection接口做任何扩充

28.1.2.1.HashSet散列存储

```java
Class HashSet<E>
java.lang.Object
java.util.AbstractCollection<E>
java.util.AbstractSet<E>
java.util.HashSet<E>
Type Parameters:
E - the type of elements maintained by this set
All Implemented Interfaces:
Serializable, Cloneable, Iterable<E>, Collection<E>, Set<E>
Direct Known Subclasses:
JobStateReasons, LinkedHashSet
public class HashSet<E>
extends AbstractSet<E>
implements Set<E>, Cloneable, Serializable
```



```java
import java.util.*;
public class Wind {
	public static void main(String args[]) {
		Set<String> ha = new HashSet<String>();// 为Set接口实例化
		ha.add("1");
		ha.add("b");
		ha.add("C");
		ha.add("d");
		ha.add("1");
		ha.add("A");//增加重复元素
		System.out.println(ha);			}}

```

此类实现由哈希表（实际上是实例）支持的接口。它不保证集合的迭代顺序;特别是，它不保证订单将随着时间的推移而保持不变。此类允许该元素。SetHashMapnull
此类为基本操作 （， 和 ） 提供恒定的时间性能，假定哈希函数在存储桶中正确分散元素。迭代此集需要与实例大小（元素数）之和加上后备实例的"容量"（存储桶数）成比例的时间。因此，如果迭代性能很重要，则不要将初始容量设置得太高（或负载因子太低）非常重要。addremovecontainssizeHashSetHashMap

请注意，此实现不同步。如果多个线程同时访问一个哈希集，并且至少有一个线程修改了该集，则必须在外部同步该哈希集。这通常是通过在自然封装集合的某些对象上进行同步来实现的。如果不存在此类对象，则应使用Collections.syncedSet方法对集合进行"包装"。最好在创建时执行此操作，以防止意外地访问集合的不同步：

   

```
Set s = Collections.synchronizedSet(new HashSet(...));
```


该类的方法返回的迭代器是快速失败的：如果在创建迭代器后的任何时间修改集合，则除了通过迭代器自己的方法之外，以任何方式，迭代器都会抛出ConcurrentModificationException。因此，面对并发修改，迭代器会快速而干净地失败，而不是在未来不确定的时间冒着任意、非确定性行为的风险。iteratorremove

请注意，无法保证迭代器的快速失败行为，因为一般来说，在存在不同步的并发修改的情况下，不可能做出任何硬保证。快速失败迭代器在尽最大努力的基础上进行投掷。因此，编写一个依赖于此异常的正确性的程序是错误的：迭代器的快速失败行为应该仅用于检测错误。



28.1.2.2.TreeSet 有序存储

```java
Class TreeSet<E>
java.lang.Object
java.util.AbstractCollection<E>
java.util.AbstractSet<E>
java.util.TreeSet<E>
Type Parameters:
E - the type of elements maintained by this set
All Implemented Interfaces:
Serializable, Cloneable, Iterable<E>, Collection<E>, NavigableSet<E>, Set<E>, SortedSet<E>
public class TreeSet<E>
extends AbstractSet<E>
implements NavigableSet<E>, Cloneable, Serializable
```

```java
import java.util.*;
public class Wind {
	public static void main(String args[]) {
		Set<String> ha = new TreeSet<String>();// 为List接口实例化
		ha.add("1");		ha.add("b");		ha.add("C");
		ha.add("d");		ha.add("1");
		ha.add("A");//增加重复元素
		System.out.println(ha);}}

```

当用TreeSet去存储数据时，如果没有定义排序（compare()）方法，那么就会出现类转换异常，因为程序不知道该怎么排序；因为一个自定义的类本身不知道怎么排序；--所以自定义类要实现Comparable接口，并覆写compareTo()方法或者compare(Object obj1,Object obj2) 

```java
import java.util.Arrays;
import java.util.Set;
import java.util.TreeSet;
class Book implements Comparable<Book> {
    private String title;    private float price;
    public Book(String title, float price) {
        this.title = title;        this.price = price;    }
    public String toString() {
        return this.title + this.price;    }
     public int compareTo(Book b) {
        if (this.price > b.price) {            return -1;
        } else if (this.price == b.price) {
            return 0;        } else {
            return 1;        }    }}
    
public class Wind {
public static void main(String args[]) {
        Set<Book> book = new TreeSet<Book>();
        book.add(new Book("Java", 59.6f));
        book.add(new Book("Phyton", 52.6f));
        book.add(new Book("C++", 58.6f));
        book.add(new Book("JAVAWeb",52.6f));
        Object obj[] = book.toArray();
System.out.println("------排序后的顺序------");
        Arrays.sort(obj);        System.out.println(Arrays.toString(obj));    }}


```



在存储序列中添加了四个与元素，可输出只有三个，因为Phython和JAVAWeb价格一样程序默认两个元素是一样的（Set接口中不允许有重复元素）---所以我们要在compareTo中相等的部分再添加判断条件；

```java
public int compareTo(Book b) {
		if (this.price > b.price) {			return -1;
		} else if (this.price == b.price) {
return this.title.compareTo(b.title);//继续向下比较，比较title		} else {			return 1;	}	}}

book.add(new Book("Java", 59.6f));
		book.add(new Book("Phyton", 52.6f));
		book.add(new Book("C++", 58.6f));
		book.add(new Book("JAVAWeb", 52.6f));
		book.add(new Book("Java", 59.6f));//增加重复元素

```

此时已经完成了排序，并且不允许有重复元素的操作，这是通过Comparable接口完成的，类本身并没有重复元素的限制；

分析--如果我将TreeSet(有序存储，要进行排序)改为HashSet(散列存储，不进行排序)

![](..\pic\图片73.png)

仍然存在重复元素的问题--原因是new Book()产生的两个不同的实例对象，所以HashSet认为他两个不同----TreeSet因为要进行排序，能分辨不同元素是靠着compareTo来完成的； 

28.1.2.3.关于重复元素的说明

如果将TreeSet替换为HashSet，则会发现存在重复元素，那么我在类中覆写.equals()方法；而且如果还需要完成对象元素重复的判断还需要覆写Object类中的hashcode()方法；

因此也就说明了一个类中判断是否是重复元素是靠着

.equals()方法和hashcode()方法来共同作用来实现的其中String类中已经自动覆写了上述两个方法，其他自定义类则要自己覆写;



```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;
class Book implements Comparable<Book> {
	private String title;	private int price;
	public Book(String title, int price) {
this.title = title;		this.price = price;	}
	public String toString() {
		return this.title + this.price;	}
	public int hashcode() {//哈希值是用公式计算出来的
	return   this.title.hashcode()*this.price;	}
public boolean equals(Object obj) {
		if(this==obj) {	return true;		}
if(!(obj instanceof Book)) {	return false;		}
		Book book =(Book)obj;

    if(this.title.equals(book.title)&&this.price==book.price) {
			return true;		}
		else {	return false ;}	}
	public int compareTo(Book b) {
		if (this.price > b.price) {
			return -1;
	} else if (this.price == b.price) {
			return this.title.compareTo(b.title);//继续向下比较，比较title
} else {return 1;	}	}	}

```

28.1.3.比较器--自定义排序

需要为多个对象排序时必须设置排序规则，而排序规则就可以通过比较器进行设置，而在Java之中比较器提供了两种：Comparable和Comparator。

28.1.3.1.Comparable接口

Comparable是一个要进行多个对象比较的类需要默认实现的一个接口，这个接口的定义如下。

​        public interface Comparable<T> {

​            public int compareTo(T o) ;        }

从接口的定义格式上来看，可以发现如果想实现对象属性的排序功能，要实现Comparable接口，并覆写compareTo(To)方法。此方法返回的是一个int类型的数据，该返回值只能是以下三种情况之一。 

⑴ 相等：0；⑵ 大于：1；⑶ 小于：-1。

实际上只要满足而等于0,小于0和大于0三种情况都可以

```java
import java.util.Arrays;
import java.util.Vector;
class Book implements Comparable<Book> {
    private String title;
    private float price;
    public Book(String title, float price) {
        this.title = title;
        this.price = price;    }
    public String toString() {
        return this.title + this.price;    }
    // 覆写compareTo方法，
    // 默认的为大于时为1，等于为 0，小于时为-1---升序排列
    // 覆写后 可以修改为大于时为-1，等于为 0，小于时为1---降序排列


```

```java
    public int compareTo(Book b) {
        if (this.price > b.price) {            return -1;
        } else if (this.price == b.price) {
            return 0;        } else {
            return 1;        }    }}
public class Arraysortedby {
    public static void main(String args[]) {
        Vector<Book> book = new Vector<Book>();
        book.add(new Book("Java", 59.6f));
        book.add(new Book("Phyton", 52.6f));
        book.add(new Book("C++", 58.6f));
        Object obj[] = book.toArray();
System.out.println("------排序后的顺序------");
        Arrays.sort(obj);        System.out.println(Arrays.toString(obj));  }}


```



28.1.3.2.Comparator挽救的比较器接口

如果说现在假设有一个类已经开发完，在此类使用了很久之后，忽然有一天需要实现对这个类对象数组的排序，但是由于此类并没有实现Comparable接口，所以现在一定无法利用Arrays类的sort()方法完成排序，并且这个类由于某些原因也无法再进行修改了。在这种情况下如果还想完成排序的功能，那该怎么办？

Comparator定义-- public interface Comparator<T> {public int compare(T o1, T o2);

​      public boolean equals(Object obj) ;        }

Comparator接口定义了两个方法。compare( )和equals( )。这里给出的compare( )方法按顺序比较了两个元素。      int compare(Object obj1, Object obj2)obj1和obj2是被比较的两个对象。当两个对象相等时，该方法返回0；当obj1大于obj2时，返回一个正值；否则，返回一个负值。如果用于比较的对象的类型不兼容的话，该方法会引发一个ClassCastException异常。通过改写compare( )，可以改变对象排序的方式。例如，通过创建一个颠倒比较输出的比较方法，可以实现按逆向排序。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
class Book {
    private String title;
    private float price;
    public Book(String title, float price) {
        this.title = title;
        this.price = price;    }
    public void setTitle(String title) {
        this.title = title;    }
    public void setPrice(float price) {
        this.price = price;    }
    public String getTitle() {
        return title;    }
    public float getPrice() {
        return price;    }
    public String toString() {
        return this.title + this.price;    }}
class BookComparator implements Comparator<Book> {
    public int compare(Book bk, Book BK) {
        if (bk.getPrice() > BK.getPrice()) {
            return 1;        } else if (bk.getPrice() == BK.getPrice()) {
            return 0;        }
        return -1;    }}
public class Ceshi {
    public static void main(String args[]) {
ArrayList<Book> book = new ArrayList<Book>();
        book.add(new Book("Java", 56.6f));
        book.add(new Book("C++", 52.7f));
        book.add(new Book("PHP", 88.6f));
        // 这一步要将Object[]数组转换为Book，
  Book BBK[] = book.toArray(new Book[]{});
    Arrays.sort(BBK, new BookComparator());
        for (Book book2 : BBK) {
        	System.out.println(book2);
		}  }}

```

28.1.4.Collection接口

我们先来看Collection接口，此接口可以被好多类实现，比较重要的就是迭代

```java
Interface Collection<E>
Type Parameters:
E - the type of elements in this collection
All Superinterfaces:
Iterable<E>
All Known Subinterfaces:
BeanContext, BeanContextServices, BlockingDeque<E>, BlockingQueue<E>, Deque<E>, EventSet, List<E>, NavigableSet<E>, Queue<E>, Set<E>, SortedSet<E>, TransferQueue<E>
```

此接口下有两个接口List和Set，每个有各自的特点，在使用上完全不一样；

Collection接口中定义了如下方法：

![](..\pic\图片74.png)

在JDK1.5之后，Collection接口使用了泛型技术，那么这样做的目的是可以保证接口中的操作内容更加安全，因为最早为了保证Collection接口中可以接受任意的内容，座椅使用了Object进行接收，但是因为其可以接收任意的内容，所以在使用中就有可能在一个集合中插入不同的对象，在取出的时候就有可能出现类转换异常，所以在实际的类集操作中一定要指明其具体的泛型类型；

从现代的开发中来看，已经很少去用Collection完成功能了，基本上都是用其子接口List或者Set接口来完成；

 

 

28.1.4.Map接口

```java
Interface Map<K,V>
Type Parameters:
K - the type of keys maintained by this map
V - the type of mapped values
All Known Subinterfaces:
Bindings, ConcurrentMap<K,V>, ConcurrentNavigableMap<K,V>, NavigableMap<K,V>, SortedMap<K,V>
All Known Implementing Classes:
AbstractMap, Attributes, AuthProvider, ConcurrentHashMap, ConcurrentSkipListMap, EnumMap, HashMap, Hashtable, Headers, IdentityHashMap, LinkedHashMap, PrinterStateReasons, Properties, Provider, RenderingHints, ScriptObjectMirror, SimpleBindings, TabularDataSupport, TreeMap, UIDefaults, WeakHashMap
public interface Map<K,V>
```



Collection接口是保存单值的的最大接口，Map是保存一对值，所有的内容是以key→value的形式保存的；

在Map操作中 类似于  电话本 通过key找到值

Key          Value

张三         123456

李四         654132

王五         78945621

Map接口本身有三个常用的子类：

HashMap、 HashTable、TreMap

Map

将键映射到值的对象。映射不能包含重复的键;每个键最多可以映射到一个值。
此接口取代了类，该类是一个完全抽象的类，而不是一个接口。

该接口提供了三个集合视图，允许将地图的内容视为一组键、值集合或一组键值映射。地图的顺序定义为地图集合视图上的迭代器返回其元素的顺序。

不可修改的地图
Map.of、Map.ofEntries和Map.copyOf静态工厂方法提供了一种创建不可修改地图的便捷方法。这些方法创建的实例具有以下特征：Map

- 它们是不可修改的。不能添加、删除或更新键和值。在 Map 上调用任何突变体方法将始终导致被抛出。但是，如果包含的键或值本身是可变的，则可能会导致 Map 的行为不一致或其内容显示为更改。UnsupportedOperationException
- 它们不允许键和值。尝试使用键或值创建它们会导致 。nullnullNullPointerException
  如果所有键和值都是可序列化的，则它们是可序列化的。
- 它们在创建时拒绝重复的密钥。传递给静态工厂方法的重复密钥会导致 。IllegalArgumentException
  映射的迭代顺序未指定，可能会发生更改。
- 它们基于价值。调用方不应对返回的实例的标识进行任何假设。工厂可以自由地创建新实例或重用现有实例。因此，对这些实例执行的标识敏感操作（引用相等 （）、标识哈希代码和同步）不可靠，应避免使用。==
- 它们按照"序列化表单"页上的指定进行序列化。

28.1.4.1.HashMap

```java
Class HashMap<K,V>
java.lang.Object
java.util.AbstractMap<K,V>
java.util.HashMap<K,V>
Type Parameters:
K - the type of keys maintained by this map
V - the type of mapped values
All Implemented Interfaces:
Serializable, Cloneable, Map<K,V>
Direct Known Subclasses:
LinkedHashMap, PrinterStateReasons
public class HashMap<K,V>
extends AbstractMap<K,V>
implements Map<K,V>, Cloneable, Serializable
```

基于哈希表的接口实现。此实现提供所有可选的映射操作，并允许值和键。（该类大致等效于 ，只是它不同步并允许 null。此类不保证地图的顺序;特别是，它不保证订单将随着时间的推移而保持不变。

如果多个线程同时访问哈希映射，并且至少有一个线程在结构上修改了映射，则必须在外部同步该映射。（结构修改是添加或删除一个或多个映射的任何操作;仅更改与实例已包含的键关联的值不是结构修改。这通常是通过在自然封装映射的某个对象上进行同步来实现的。如果不存在此类对象，则应使用Collections.syncedMap方法对映射进行"包装"。最好在创建时执行此操作，以防止意外访问地图不同步：

```java
   Map m = Collections.synchronizedMap(new HashMap(...));
```


所有此类的"集合视图方法"返回的迭代器都是快速失败的：如果在创建迭代器后的任何时间对映射进行结构修改，则除了通过迭代器自己的方法之外，迭代器将抛出ConcurrentModificationException。因此，面对并发修改，迭代器会快速而干净地失败，而不是在未来不确定的时间冒着任意、非确定性行为的风险。

```java
import java.util.*;
public class Wind {
	public static void main(String args[]) {
	Map<String,Integer>Student =new HashMap<String,Integer>();
	Student.put("张1", 16);
	Student.put("李2", 17);
	Student.put("张1", 19);//键值重复就覆盖
	Student.put("赵4", 18);
	Student.put("赵5", 18);//value重复
	Student.put("赵6", 18);
	Student.put("赵6", 18);//都重复	
	Integer value =Student.get("张1");//通过key查找Value
	Integer value1 =Student.get("张21");//
	System.out.println(Student);
	System.out.println("张1的年龄---"+value);
	System.out.println("查找张21--"+value1+"(不存在)");	}}

```

```java
/public V put(K key, V value)将指定值与该映射中的指定键相关联。如果该映射先前包含该键的映射，则将替换旧值。
public V get返回指定键所映射的值，或者null此映射不包含该键的映射。
更正式地，如果此映射包含从键映射 k到一个值v，使得(key==null ? k==null : key.equals(k))，则此方法返回v; 否则返回null。（最多可以有一个这样的映射。）
的返回值null并不一定 表明该映射不包含该键的映射关系; 映射也可能将键显式映射到null。该containsKey操作可用于区分这两种情况。
(Object key)*/



```

Map中键值不能重复，如果重复则属于覆盖，Map最大的特点是用于查找操作，如果找到了则返回内容否则返回空 ;



将key变为Set类型的操作--

方法--public Set<K> keySet()--将Key值以Set的形式取出

```
import java.util.*;
public class Wind {
	public static void main(String args[]) {
	Map<String,Integer>Student =new HashMap<String,Integer>();
	Student.put("张1", 16);
	Student.put("李2", 17);
	Student.put("张1", null);//键值重复就覆盖
	Student.put("赵4", 18);
	Student.put("赵5", 18);//value重复
	Student.put("赵6", 18);
	Student.put("赵6", 18);//都重复	
	Integer value =Student.get("张1");//通过key查找Value
	Integer value1 =Student.get("张21");//
	System.out.println(Student);
	Set <String>set =Student.keySet();//返回全部的Key
	Iterator<String> iter =set.iterator();
	while(iter.hasNext()) {
		System.out.println(iter.next());	}	}}

```

通过以上方式取出key值，还可以通过以下方法将值取出-- 

![](..\pic\图片75.png)

从运行结果上我们发现HashMap存储的值属于无序存储；

将Value通过Collection接口返回--Value()方法----

定义--public Collection<V> values()

返回Collection此映射中包含的值的视图。集合由地图支持，因此对地图的更改会反映在集合中，反之亦然。如果在对集合进行迭代时修改了映射（通过迭代器自己的remove操作除外），则迭代的结果是不确定的。收集支撑元件移除，即从映射中相应的映射，经由Iterator.remove， Collection.remove，removeAll， retainAll和clear操作。它不支持add或addAll操作。

```java
import java.util.*;
public class Wind {
	public static void main(String args[]) {
	Map<String,Integer>Student =new HashMap<String,Integer>();
	Student.put("张1", 16);
	Student.put("李2", 17);
	Student.put("张1", null);//键值重复就覆盖
	Student.put("赵4", 18);
	Student.put("赵5", 18);//value重复
	Student.put("赵6", 18);
	Student.put("赵6", 18);//都重复	
	System.out.println(Student);
	Collection <Integer>set =Student.values();//返回全部的键值
	Iterator<Integer> iter =set.iterator();
	while(iter.hasNext()) {
		Integer val =iter.next();
		System.out.println(val);	}	}}

```

28.1.4.2.旧的子类--Hashtable

```java
Class Hashtable<K,V>
java.lang.Object
java.util.Dictionary<K,V>
java.util.Hashtable<K,V>
Type Parameters:
K - the type of keys maintained by this map
V - the type of mapped values
All Implemented Interfaces:
Serializable, Cloneable, Map<K,V>
Direct Known Subclasses:
Properties, UIDefaults
public class Hashtable<K,V>
extends Dictionary<K,V>
implements Map<K,V>, Cloneable, Serializable
```



HashTable,最早的映射操作类--操作跟HashMap一样

```java
import java.util.*;
public class Wind {
	public static void main(String args[]) {
Map<String, Integer> Student = new Hashtable<String, Integer>();
		Student.put("张1", 16);		Student.put("李2", 17);
		Student.put("张1", 18);		Student.put("赵4", 18);
		Student.put("赵5", 18);		Student.put("赵6", 18);
			System.out.println(Student);
		Collection<Integer> set = Student.values();
		Iterator<Integer> iter = set.iterator();
		while (iter.hasNext()) {
			Integer val = iter.next();
			System.out.println(val);	}	}}


```

Hashtable中value值不能为null 

此类实现一个哈希表，该表将键映射到值。任何非对象都可以用作键或值。null
若要成功存储和检索哈希表中的对象，用作键的对象必须实现方法和方法。hashCodeequals

的实例有两个影响其性能的参数：初始容量和负载系数。容量是哈希表中的存储桶数，初始容量只是创建哈希表时的容量。请注意，哈希表是打开的：在"哈希冲突"的情况下，单个存储桶存储多个条目，必须按顺序搜索这些条目。负载系数是衡量哈希表在容量自动增加之前允许其获得的完整程度的度量。初始容量和负载因子参数只是对实现的提示。有关何时以及是否调用重述方法的确切详细信息取决于实现。Hashtable

通常，默认负载系数 （.75） 在时间和空间成本之间提供了良好的权衡。较高的值会减少空间开销，但会增加查找条目的时间成本（这在大多数操作中都有所反映，包括 和 ）。Hashtablegetput

初始容量控制着浪费空间与操作需求之间的权衡，这非常耗时。如果初始容量大于将包含的最大条目数除以其负载系数，则不会发生任何操作。但是，将初始容量设置得太高会浪费空间。rehashrehashHashtable

如果要将许多条目创建为 a，则创建具有足够大容量的条目可能比让它根据需要执行自动重述以增加表更有效率地插入条目。Hashtable

此示例创建数字的哈希表。它使用数字的名称作为键：




```java
 Hashtable<String, Integer> numbers
     = new Hashtable<String, Integer>();
   numbers.put("one", 1);
   numbers.put("two", 2);
   numbers.put("three", 3);
```


若要检索数字，请使用以下代码：




```java
  Integer n = numbers.get("two");
   if (n != null) {
     System.out.println("two = " + n);
   }
```


由所有此类的"集合视图方法"返回的集合方法返回的迭代器是快速失败的：如果在创建迭代器后的任何时间对 Hashtable 进行结构修改，除了通过迭代器自己的方法之外，迭代器将以任何方式引发ConcurrentModificationException .因此，面对并发修改，迭代器会快速而干净地失败，而不是在未来不确定的时间冒着任意、非确定性行为的风险。Hashtable的键和元素方法返回的枚举不是快速失败的;如果在创建枚举后的任何时间对 Hashtable 进行了结构修改，则枚举的结果未定义

 

 

 

28.1.4.3.HashMap与Hashtable的区别

HashMap与Hashtable都是Map接口的子类，区别在于----

| NO   | 区别点   | HashMap                                    | Hashtable                                       |
| ---- | -------- | ------------------------------------------ | ----------------------------------------------- |
| 1    | 推出时间 | JDK1.2之后                                 | JDK1.0时推出，属于旧的操作类                    |
| 2    | 操作     | 异步操作，执行效率高，安全性不如Hashtabler | 同步操作，执行效率不如ArrayList高，但是安全性好 |
| 3    | 性能     | 高                                         | 相对较低                                        |
| 4    | 安全     | 非线程安全的操作                           | 线程安全的操作                                  |

从实际开发来讲，Map接口最常用的就是HashMap；

28.1.4.4.TreeMap

TreeMap使用时可以进行按照Key的方式进行排序。

定义--

public class TreeMap<K,V>extends AbstractMap<K,V>implements NavigableMap<K,V>, Cloneable, Serializable

基于红黑树的

NavigableMap

实现。映射是根据其键的自然顺序来排序的，或者是

Comparator

根据映射创建时提供的映射来排序的，具体取决于所使用的构造函数。

```java
public class Wind {
	public static void main(String args[]) {
		Map<String, Integer> Student = new TreeMap<String, Integer>();
		Student.put("A-张6", 16);		Student.put("D-李2", 17);
		Student.put("F-张3", 18);// 键值重复就覆盖
		Student.put("B-赵4", 18);		Student.put("C-赵7", 18);// value重复
		Student.put("E-赵6", 20);		System.out.println(Student);
		Collection<Integer> set = Student.values();		Iterator<Integer> iter = set.iterator();
		while (iter.hasNext()) {
			Integer val = iter.next();
			System.out.println(val);	}	}}

```

如果给定一个自定义类，那么是否可以实现直接排序呢？----类必须实现Compareable接口
注意事项：
Map不能直接使用iterator输出，在集合的标准操作中最好使用Iterator输出，但是在Map接口中并没有明确定义出该操作，所以必须要深入了解Map的存储机制，在Map中虽然是一对值的形式出现的，但是程序真正保存的还是一个单独的对象即：程序将key--value放在了一个对像之中，之后将此对象加入到集合里。

Map.Entry  为Map实体
public static interface Map.Entry<K,V>映射条目（键值对）。该Map.entrySet方法返回地图的集合视图，该地图的元素属于此类。获取对地图条目的引用的 唯一方法是从此collection-view的迭代器中进行。这些Map.Entry对象仅在迭代期间有效。更正式的说，如果迭代器返回条目后修改了后备映射，则映射条目的行为是不确定的，除非通过setValue对映射条目的操作。

所以就可以给出Map接口使用Iterator输出的标准操作；注意--Map不能直接通过Iteraor直接输出Key和Value，必须通过Map.Entry的getKey()和getValue()得到； 

![](..\pic\图片76.png)

- 通过Map接口 Set<Map.Entry<K,V>> entrySet()取得Set集合
- 通过Set接口为Iterator进行初始化操作
- 通过Iterator取出一个Map.Entry
- 通过Map.Entry进行Key与Value的分离 ---通过getKey()和getValue()方法进行分离

```java
import java.util.*;
public class Wind {
	public static void main(String args[]) {
		Map<String, Integer> Student = new TreeMap<String, Integer>();
		Student.put("A-张6", 16);		Student.put("D-李2", 17);
		Student.put("F-张3", 18);// 键值重复就覆盖
		Student.put("B-赵4", 18);		Student.put("C-赵7", 18);// value重复
		Student.put("E-赵6", 20);
		Set<Map.Entry<String, Integer>> allset = null;
		allset = Student.entrySet();//
		Iterator<Map.Entry<String, Integer>> iter = allset.iterator();
		while (iter.hasNext()) {
			Map.Entry<String, Integer> me = iter.next();
			System.out.println(me.getKey() + "-----" + me.getValue());	}}}

```

```java
import java.util.*;
public class Leader {
	public static void main(String args[]) {
		Map<String, Integer> map = new HashMap<String, Integer>();
		map.put("张三", 18);// 自动装箱
		map.put("李四", 19);		map.put("王五", 20);		map.put("赵六", 21);
				 //* 要求输出所有人key 将键值存储为集合，通过集合输出
		 		Set<String> set = map.keySet();// 通过set接口取出key值
		Iterator<String> ite = set.iterator();// 迭代，需要用Iterrator接收
		while (ite.hasNext()) {
			System.out.println(ite.next());		}
		//* 要求输出所有的Value 用Collection接口接收
		Collection<Integer> st = map.values();
		Iterator<Integer> it = st.iterator();
		while (it.hasNext()) {
			System.out.println(it.next());		}
			 //* 通过Map.Entry取出key与value 先实例化Set接口
		 	Set<Map.Entry<String, Integer>> me = null;//
		me = map.entrySet();//通过Set接口初始化Map.Entry
		Iterator<Map.Entry<String, Integer>> ist = me.iterator();
		while (ist.hasNext()) {
			//* 实际上Map.Entry是将一对值存储为一个值， 所以在取出值之前要先指定下一个元素泛型类型
			Map.Entry<String, Integer> mm = ist.next();
			System.out.println(mm.getKey() + "----->" + mm.getValue());	}}}


```

在JDK1.5之后可以通过foreach直接输出全部内容； 

```java
import java.util.*;
public class Leader {
	public static void main(String args[]) {
		Map<String, Integer> map = new HashMap<String, Integer>();
		map.put("张三", 18);	map.put("李四", 19);		
		map.put("王五", 20);		map.put("赵六", 21);	
		for(Map.Entry<String,Integer> me:map.entrySet()) {
			System.out.println(me.getKey() + "----->" + me.getValue());	}

```

使用非系统类，并在Map中设置多个对象，使用String作为键值 

```java
import java.util.*;
class Book implements Cloneable {// 可以被复制
	String title;	int price;
	public String getTitle() {
		return title;	}
	public void setTitle(String title) {
		this.title = title;	}
	public float getPrice() {
		return price;	}
	public void setPrice(int price) {
		this.price = price;	}
	public Book(String title, int price) {
		this.title = title;
		this.price = price;	}
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();	}
	public String toString() {
		return this.title + this.price;	}}
public class Myfun {
	public static void main(String args[]) throws CloneNotSupportedException {
		Book bookA = new Book("java", 56);
		Book bookB = (Book) bookA.clone();
		bookB.setPrice(100);
	System.out.println(bookB.toString());
System.out.println("-------楚汉界限-------");
		Map<String, Book> map = new HashMap<String, Book>();
		map.put("No.1", new Book("Java", 56));
		map.put("No.2", new Book("JavaWeb", 66));
		map.put("No.3", new Book("Swift", 76));
		System.out.println(map.get("No.1"));
//在String的操作中可以通过匿名对象找到Value的值
		System.out.println(map.get(new String("No.2")));}}


```

在键值为String的操作中可以通过匿名对象找到Value的值，那么问可以发现，在Map中可以通过匿名对象找到一个Key对应的Value；反之该如何操作呢？--即将Map的泛型类型交换位置； 

```java
Map<Book, String> map = new HashMap<Book, String>();
		map.put(new Book("Java", 56), "No.1");
		map.put(new Book("JavaWeb", 66),  "No.2");
		System.out.println(map.get(new Book("Java", 56)));
//在String的操作中可以通过匿名对象找到Value的值
		System.out.println(map.get(new Book("JavaWeb", 66)));}}

```

现在发现无法通过Key找到Value----因为  系统通过hashcode 和equal()来区别是否是同一个对象

因此要实现hashcode()和equals()方法的覆写；

```java
import java.util.*;
class Book implements Cloneable {// 可以被复制
	String title;	int price;
	public String getTitle() {
		return title;	}
	public void setTitle(String title) {
		this.title = title;	}
	public float getPrice() {
		return price;	}
	public void setPrice(int price) {
		this.price = price;	}
	public Book(String title, int price) {
		this.title = title;
		this.price = price;	}
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();	}
	public String toString() {
		return this.title + this.price;	}

	public int hashcode() {
		return this.title.hashcode()*this.price;	}
	public boolean equals(Object obj) {
		if (this==obj) {return true;		}
if (!(obj instanceof Book)) {	return false;	}
		Book book =(Book)obj;//转型		if(this.title.equals(book.title)&&this.price==book.price) {	return true;		}
		else {return false;		}	}}
public class Myfun {
	public static void main(String args[]) throws CloneNotSupportedException {
		Book bookA = new Book("java", 56);
		Book bookB = (Book) bookA.clone();
		bookB.setPrice(100);
		System.out.println(bookB.toString());
		System.out.println("-------楚汉界限-------");
		

```

28.1.4.5.Map的子类--IdentityHashMap

```java
Class IdentityHashMap<K,V>
java.lang.Object
java.util.AbstractMap<K,V>
java.util.IdentityHashMap<K,V>
All Implemented Interfaces:
Serializable, Cloneable, Map<K,V>
public class IdentityHashMap<K,V>
extends AbstractMap<K,V>
implements Map<K,V>, Serializable, Cloneable
```

默认情况下Map集合中Key是不允许重复的，但是在Class IdentityHashMap，通过此类加入的内容，如果加入的对象地址不相等则不认为是同一个对象。

此类使用哈希表实现接口，在比较键（和值）时使用引用相等代替对象相等。换言之，在 a 中，两个键和 被视为相等，当且仅当 。（在正常实现中（如）两个键，并且当且仅当时才被视为相等。

```
MapIdentityHashMapk1k2(k1==k2)MapHashMapk1k2(k1==null ? k2==null : k1.equals(k2))
```


此类不是通用的Map实现！虽然此类实现了Map接口，但它有意违反了Map 的通用合约，该合约要求在比较对象时使用equals方法。此类设计为仅在需要引用相等语义的极少数情况下使用。

这类的典型用法是拓扑保留对象图转换，如序列化或深度复制。若要执行此类转换，程序必须维护一个"节点表"，该表跟踪已处理的所有对象引用。节点表不得将不同的对象相等，即使它们恰好相等。此类的另一个典型用途是 维护代理对象。例如，调试工具可能希望为正在调试的程序中的每个对象维护一个代理对象。

此类提供所有可选的映射操作，并允许值和键。此类不保证地图的顺序;特别是，它不保证订单将随着时间的推移而保持不变。nullnull

此类为基本操作 （ 和 ） 提供常量时间性能，假设系统标识哈希函数 （System.identityHashCode（Object） ）将元素正确地分散到存储桶中。getput

此类有一个优化参数（这会影响性能，但不会影响语义）：预期最大大小。此参数是映射预期包含的最大键值映射数。在内部，此参数用于确定最初构成哈希表的存储桶数。预期最大大小与存储桶数之间的精确关系未指定。

如果映射的大小 （键值映射的数量） 充分超过预期的最大大小，则存储桶的数量将增加。增加存储桶的数量（"重新哈希"）可能相当昂贵，因此创建具有足够大的预期最大大小的身份哈希映射是值得的。另一方面，对集合视图的迭代需要与哈希表中的存储桶数成比例的时间，因此，如果您特别关注迭代性能或内存使用情况，则不要将预期的最大大小设置得太高。

请注意，此实现不同步。如果多个线程同时访问标识哈希映射，并且至少有一个线程在结构上修改了映射，则必须在外部同步该映射。（结构修改是添加或删除一个或多个映射的任何操作;仅更改与实例已包含的键关联的值不是结构修改。这通常是通过在自然封装映射的某个对象上进行同步来实现的。如果不存在此类对象，则应使用Collections.syncedMap方法对映射进行"包装"。最好在创建时执行此操作，以防止意外访问地图不同步：

   Map m = Collections.synchronizedMap(new IdentityHashMap(...));
由所有此类的"集合视图方法"返回的集合方法返回的迭代器是快速失败的：如果在创建迭代器后的任何时间对映射进行结构修改，除了通过迭代器自己的方法之外，迭代器将抛出ConcurrentModificationException .因此，面对并发修改，迭代器会快速而干净地失败，而不是在未来不确定的时间冒着任意、非确定性行为的风险。iteratorremove

请注意，无法保证迭代器的快速失败行为，因为一般来说，在存在不同步的并发修改的情况下，不可能做出任何硬保证。快速失败迭代器在尽最大努力的基础上进行投掷。因此，编写一个依赖于此异常的正确性的程序是错误的：快速失败迭代器应该仅用于检测错误。ConcurrentModificationException

实现说明：这是一个简单的线性探测哈希表，例如在Sedgewick和Knuth的文本中所述。数组交替保存键和值。（与使用单独的数组相比，这对于大型表具有更好的位置。对于许多 JRE 实现和操作组合，此类将产生比HashMap（使用链接而不是线性探测）更好的性能。

```java
public class Myfun {
	public static void main(String args[]) throws CloneNotSupportedException {
		Map<Book, String> map = new IdentityHashMap<Book, String>();
		map.put(new Book("Java", 56), "No.1");
		map.put(new Book("Java", 56),  "No.2");
		System.out.println(map);
		}}

```

```java
public class Myfun {
	public static void main(String args[]) throws CloneNotSupportedException {
		Map<Book, String> map = new HashMap<Book, String>();
		map.put(new Book("Java", 56), "No.1");
		map.put(new Book("Java", 56),  "No.2");
		System.out.println(map);
		}}


```

28.1.4.6.类集知识点总结  类集的设置的主要目的：动态的对象数组

类集的接口--

Collection--是存放单值的最大父类接口

其子类-- List  接口存放的内容是允许重复的，List接口的子类--ArrayList、Vector、LinkedList（实现了Queue接口）

另一个在子类--Set   接口存放的内容不允许重复（如果有重复则会覆盖旧的内容）依靠equals()和 hashcode()方法来完成重复元素的判断

Set接口的子类--HashSet （散列存放）、TreeSet（有序的存放，依靠Comparable接口实现有排序操作）

Map接口是否存放一对值的接口也可以成为二元偶对象

在Map下的子类

--HashMap--无序存放键值不能重复

 --TreeSet--有序存放 按照Key值进行排序，--也是按照Comparable接口实现排序

任何一个非系统类，若要实现排序则必须实现Comarable接口并覆写 compareTo(Object obj)或者在该类外实现Comparetor覆写cmpare(Object obj1,Object obj2)方法

任何一个非系统的类作为Key，则必须覆写equals()和hashcode().

输出接口--

Iterator：可以直接通过 Collection接口进行实例化操作

ListIterator：只能呢个通过List接口进行实例化的操作

Enumeration:古老的输出接口

foreach：JDK之后的新支持

28.1.4.6.1集合的操作工具类--Collections（补充）



```java
Class Collections
java.lang.Object
java.util.Collections
public class Collections
extends Object
```

此类专门由对集合进行操作或返回集合的静态方法组成。它包含对集合、"包装器"（返回由指定集合支持的新集合）以及其他一些可能性和结束进行操作的多态算法。
如果提供给此类的集合或类对象为 null，则此类的方法都会引发 a。NullPointerException

此类中包含的多态算法的文档通常包括实现的简要说明。这样的描述应该被视为实现说明，而不是规范的一部分。实现者应该可以自由地替换其他算法，只要规范本身得到遵守。（例如，所使用的算法不必是合并排序，但必须稳定。sort

此类中包含的"破坏性"算法（即修改它们在其上运行的集合的算法）被指定为在集合不支持适当的突变基元（如方法）时抛出。如果调用对集合没有影响，则这些算法可能会（但不是必需的）引发此异常。例如，在已排序的不可修改列表上调用该方法可能会也可能不会引发

```java
import java.util.Collections;
import java.util.List;
public class CollectionsDemo {
	public static void main(String[] args) {
		List<String> allList = Collections.emptyList();// 返回空的List集合
		allList.add("1");	}}//没法增加数据

```

emptyList

public static final <T> List<T> emptyList()

Returns an empty list (immutable). This list is serializable.返回一个空列表（不可变）。该列表是可序列化的。

This example illustrates the type-safe way to obtain an empty list:

​     List<String> s = Collections.emptyList();

通过以上的方法获得的集合是不可被修改的，因为此类是没有对add()进行实现；如果要添加数据只能通过  先实例化List或者Set接口在通过Collections.addAll()添加

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
public class CollectionsDemo {
	public static void main(String[] args) {
		List<String> allList = new ArrayList<String>();// 返回空的List集合
		allList.add("B-Hello");		allList.add("C-Hello");		allList.add("A-Hello");
		System.out.println("---Collections添加之前---");
		System.out.println(allList);
Collections.addAll(allList, "E-Hello","D-Hello");//通过addAll在已有List接口上添加元素
		System.out.println("---Collections添加之后---");
		System.out.println(allList);	}}

```

对于List接口本身来讲是没有序列的，顺序按照增加的顺序进行排放，如果此时需要对List接口内容进行排序的话，则可以使用Collections类中的sort方法进行排序； 

![](..\pic\图片77.png)

sort()方法--

public static <T extends Comparable<? super T>>void sort(List<T> list)--按照自然排序

public static <T> void sort(List<T> list,Comparator<? super T> c)--指定排序方法

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
public class CollectionsDemo {
	public static void main(String[] args) {
		List<String> allList = new ArrayList<String>();// 返回空的List集合
		allList.add("B-Hello");
		allList.add("C-Hello");
		allList.add("A-Hello");
		System.out.println("---Collections添加之前---");
		System.out.println(allList);
		Collections.addAll(allList, "E-Hello","D-Hello");//通过addAll在已有List接口上添加元素
		System.out.println("---Collections添加之后---");
		System.out.println(allList);	
		System.out.println("----经Collections.sort()排序后----");
		Collections.sort(allList);//也可以指定排序方式
		System.out.println(allList);	}}

```

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

此类在实际开发中用的比较少，但是一定的掌握；

28.1.4.7.equals()和 hashcode()方法(重要)

equals和 hashcode是java中一个对象的两个基本方法和core java的重要组成部分。equals用来比较对象的相等性，hashcode用来生成相应对象的整数形编码。equals 和 hashcode在core java中有广泛的应用，例如在hashmap中插入和取回数据。

equals方法覆写遵循以下规则:

(1)自反性。一个对象必须和它自身相等；

(2)  对称性。如果a.equals(b) 为true 那么 b.equals(a) 也必须为true;

(3)  传递性。如果 a.equals(b) 为true 并且 b.equals(c) 为 true 那么 c.equals(a) 也必须 为 true；

(4)  一致性。除非字段的值改变，否则多次调用equals()方法应该返回相同的结果。

(5)  Null比较。任何对象和null比较必须返回false,并且不能够出现NullPointerException结果。

equals 和 hashcode的关系

equals()和hashcode()必须遵守以下规则:

(1)   如果两个对象执行equals()方法是相等的,那么执行hashcode()方法的结果也必须是相等的；

(2)  如果两个对象执行equals()方法不相等，那么执行hashcode()方法的结果可以相等也可以不相等。

重写equals方法的步骤

这是大多数java程序员重写equals方法的标准做法:

(1)  做this检查。如果是this则返回true;

(2)  做null检查。如果是null则返回false;

(3) 做instanceof检查。如果instanceof 返回false,那么equals方法就返回false。在做了一些研究之后，我发现可以用getClass()方法代替instanceof 来比较类型的相等性，因为instanceof 在检查子类的时候也会返回true。所以在需要商业逻辑的时候它并不是一个严格的相等关系。如果你的类是不变类，没有类会继承它，那么使用instanceof 时合适的。所以可以使用一下方式代替instanceof :

if((obj == null) || (obj.getClass() != this.getClass()))

​        return false;

(4)  对象类型转换。

(5)  以数值型属性开始比较每个属性值，因为数值型属性比较最快而且在结合检查的时候可以使用短路操作。如果第一个属性不匹配，那么就可以返回false而不需要匹配剩余的属性。在每个属性调用equals方法前也要记得做null检查，避免在递归equals检查时出现NullPointerException。

覆写equlas方法的代码样例

```java
/ 
 * Person class with equals and hashcode implementation in Java
 * @author Javin Paul
 */
public class Person {
    private int id;
    private String firstName;
    private String lastName;
    public int getId() { return id; }
    public void setId(int id) { this.id = id;}
    public String getFirstName() { return firstName; }
    public void setFirstName(String firstName) { this.firstName = firstName; }
    public String getLastName() { return lastName; }
    public void setLastName(String lastName) { this.lastName = lastName; }
    @Override
    public boolean equals(Object obj) {
        if (obj == this) {
            return true;
        }
        if (obj == null || obj.getClass() != this.getClass()) {
            return false;
        }
 
        Person guest = (Person) obj;
        return id == guest.id
                && (firstName == guest.firstName 
                     || (firstName != null && firstName.equals(guest.getFirstName())))
                && (lastName == guest.lastName 
                     || (lastName != null && lastName .equals(guest.getLastName())));
    }
    @Override
    public int hashcode() {
        final int prime = 31;
        int result = 1;
        result = prime * result
                + ((firstName == null) ? 0 : firstName.hashcode());
        result = prime * result + id;
        result = prime * result
                + ((lastName == null) ? 0 : lastName.hashcode());
        return result;
    }
    }

```

在覆写 equals时的常见错误

(1)  重载了equals方法而没有重写它

这是我见到的覆写equals方法最常见的错误。equasl的语法是public boolean equals(Object obj)，但许多人无意间重载了equals方法 public boolean equals(Person obj)。这个错误因为静态绑定而非常难以察觉。

 

(2)  第二个错误是在覆写equals方法时没有为成员变量没有做null检查，最终在调用equals方法时导致 NullPointerException。正确的做法是:

firstname == guest.firstname || (firstname != null && firstname.equals(guest.firstname)));

(3)  只覆写equals()方法而没有覆写hashcode方法。你必须同时覆写equals和hashcode的方法，要不然这个对象就不能在HashMap中作为一个key,因为HashMap是依赖这两个方法的。

(4)  最后一个错误是在覆写equals方法的时候没有保持equals方法和compareTo()的一致性，这不是正常的要求，只是为了服从Set的约定避免重复。SortedSet 的实现例如TreeSet使用compareTo方法来比较两个对象，例如字符串。如果compareTo方法和equals方法没有保持一致，那么TreeSet就会允许重复，这样就损坏了Set不能重复的约定。想要学习更多可以查看Things to remember whileoverriding compareTo in Java

在写equals方法时的5个小提示

1）大多数的IDE，例如NetBeans, Eclipse 和 IntelliJ IDEA提供生成equals和hashcode方法的支持。在In Eclipse do the right click-> source-> generate hashcode() and equals().

2）如果你的类中有一些唯一的商业主键，那么在equals方法中比较这些主键字段就足够了而不需要比较所有的字段。例如“id”是每个Person唯一的，那么只需要比较id就可以鉴别两个Person是不是相同的。

3）在覆写hashcode方法时，要确保你使用的所有字段都是在equals方法里是相同的。

4）String和包装类例如Integer,Float和Double需要覆写equals方法而StringBuffer不需要

5）只要有可能，要努力通过final使你的字段不可变。equals方法在不可变字段上要比可变字段安全的多。

## 29.JAVA中的比较器

**29.1 ==与equals方法**

> 其实比较难理解部分往往也是最容易的部分,这话有些绕,但是事实就是这样,今天跟我一起来看源码,画内存来感受一下;

看段代码---

```java
package baozhuang;
import java.util.Random;
public class Test {
    public static void main(String args[]) {
// ==比较--------------------------------
        //---比较基本数据类型
        int aa=12;
        int bb=12;
        int cc=24;
        System.out.println("aa==bb---->>>"+(aa==bb));//true
        System.out.println("aa==cc---->>>"+(aa==cc));//false
        boolean q=false;
        boolean p=false;
        System.out.println("q==p---->>>"+(p==q));//true

        // ==引用数据类型比较的是地址,地址不一样为false;
        String str0= "abc";
        String str1="abc";
        String str2= new String ("abc");
        String str3=new String("abc");
        System.out.println("str0==str0----->"+(str0==str0));//true
        System.out.println("str0==str1----->"+(str0==str1));//true
        System.out.println("str2==str3------>"+(str2==str3));//false
        System.out.println("str1==str2------->"+(str1==str2));//false
        System.out.println();

//        .equals()方法-----------------------

        System.out.println(str0.equals(str1));//true
        System.out.println(str2.equals(str3));//true
        System.out.println(str1.equals(str2));//true
        System.out.println("abc".equals(str0));//true
        System.out.println("abc".equals(str2));//true
    }
}
```

我们一部分一部分看----
先看基本数据类型---

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210711211859421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



如上例中的int aa = 12; int bb = 12;  int cc = 24;，分析如下:

> 说明-----由于本次分析的是全局变量,所以开辟空间是储存在堆中的,
> 如果是局部变量,对于是方法私有的--生命周期短于全局变量,所以在栈中存放.
> 本次分析的是全局变量,设想如果上述代码放到一个方法里,只是存放位置变了,其对结果没有任何影响;

编译器先处理int aa = 12；首先它会在堆中创建一个变量为aa的引用，然后查找有没有字面值为12的地址，没找到，就开辟一个空间存放12这个字面值的地址，然后将aa指向12的地址。
接着处理int bb = 12；在创建完bb这个引用变量后，由于在堆中已经有12这个字面值，便将bb直接指向12的地址。这样，就出现了aa与bb同时均指向12的情况。
再次处理int cc=24,在创建完cc这个引用变量后,堆中没有24这个字面值,于是开辟空间村饭24这个字面值;
重修修改aa这个引用变量当aa = 24;时，bb不会等于24，还是等于12;

> 结果如下--

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210711212110732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

再来分析引用数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210711213107516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

new  会在堆中开辟空间,所以new的时候一定要注意,有可能浪费空间;

接下来分析.equals()方法,此方法很常用,常常结合hashcode来使用;

先看.equals的源码;
从源头看起---万类鼻祖--Object中的equals方法

```java
 public boolean equals(Object obj) {
        return (this == obj);
    }
```

由源码得知比较的是地址,但是其子类肯能覆写了equals方法,比如String类

来看一下String类的equals方法

```java
 public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String aString = (String)anObject;
            if (!COMPACT_STRINGS || this.coder == aString.coder) {
                return StringLatin1.equals(value, aString.value);
            }
        }
        return false;
    }

```

> 由于我使用的是JDK1.8之后的版本-JDK15.0,对于equals方法有了一些修改,仔细产看源码发现修改之后的equals方法跟以前大不一样了;

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210711213842622.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
JDK 1.8之后引入了Compact Strings来取代Compressed Strings，它的实现更彻底，完全使用byte[]来替代char[]，同时新引入了一个字段coder来标识是LATIN1还是UTF16

```java
 /**
     * The identifier of the encoding used to encode the bytes in
     * {@code value}. The supported values in this implementation are
     *
     * LATIN1
     * UTF16
     *
     * @implNote This field is trusted by the VM, and is a subject to
     * constant folding if String instance is constant. Overwriting this
     * field after construction will cause problems.
     */
    private final byte coder;
```

重点在返回值是StringLatin1调用了equals()方法,StringLatin1是一个final类,该类覆写了equals方法

![在这里插入图片描述](..\pic\88.png)



JDK1.8之后String类型的equal方法底层使用byte数组实现的,   (这样做的好处是直接通过字class节码文件进行比较,而不用在进一步转换了,提高了效率.)

在网上发现一篇关于Compact Strings的文章很好,大伙可以看一下;
先来一小段引用

> > Implementation details At a high-level the proposal would allow to strings to be stored in 2 formats:
>
> Regular - i.e. Unicode encoded, as they are currently stored by the
> CLR Compact - ASCII, ISO-8859-1 (Latin-1) or even another format When
> you create a string, the constructor would determine the most
> efficient encoding and encode the data in that format. The formant
> used would then be stored in a field, so that the encoding is always
> known (CLR strings are immutable). That means that each method within
> the string class can use this field to determine how it operates, for
> instance the pseudo-code for the Equals method is shown below:

```java
> public boolean Equals(string other)  {
>     if (this.type != other.type)
>        return false;
>     if (type == ASCII)
>         return StringASCII.Equals(this, other);
>     else 
>         return StringLatinUTF16.Equals(this, other); }  
```

> This shows a nice property of having strings in two formats; some
> operations can be
> short-circuited, because we know that strings stored in different
> encodings won’t be the same.
>
> Advantages less overall memory usage (as-per @davidfowl “At the top of
> every ASP.NET profile… strings!”) strings become more cache-friendly,
> which may give better performance Disadvantages Makes some operations
> slower due to the extra if (type == ...) check needed Breaks the fixed
> keyword, as well as COM and P/Invoke interop that relies on the
> current string layout/format If very few strings in the application
> can be compacted, this will have an overhead for no gain

引用部分的原文链接----
https://mattwarren.org/2016/09/19/Compact-strings-in-the-CLR/

**29.2.JAVA中的compareTo**

> 说到equals方法,这是java必须回的方法---重写用的最多,而当我们越往后学习,我们会发现compareTo 方法也同样重要;
> 今天为带着学习得心态去查看CompareTo的源码--- 代码部分很简单,重要的是看源码---源码写的思路

```java
public class Test {
    public static void main(String args[]) {

        String str0= "abc";
        String str1="abc";
 String str2="abcd";
        int i = str0.compareTo(str1);
        System.out.println(i);
        int j =str2.compareTo(str0);
        System.out.println(j);
    }
}
```

运行结果---

```bash
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
0
1
```

我们重点看一下compareTo 的源码部分---

```java
public int compareTo(String anotherString) {
        byte v1[] = value;
        byte v2[] = anotherString.value;
        byte coder = coder();
        if (coder == anotherString.coder()) {
            return coder == LATIN1 ? StringLatin1.compareTo(v1, v2)
                                   : StringUTF16.compareTo(v1, v2);
        }
        return coder == LATIN1 ? StringLatin1.compareToUTF16(v1, v2)
                               : StringUTF16.compareToLatin1(v1, v2);
     }
```

我们观察后发现,JDK1.8之后关于equals与compareTo方法都做了改动---改动的地方就是与编码方式有关,底层是二进制byte字节码

开始比较的思路是不变的--转变为byte数组,然后比较编码方式,根据不同的编码方式调用不同的方法;

这里进分析一种编码格式--LATIN1

![1642861402377](..\pic\88)
分析结束,在底层的话你可以去看一下coder,         (StringUTF16,StringLatin1 跟字节码编码格式有关)

**29.3.内部比较器与外部比较器**

> 在日常工作中，我们会经常比较数据的大小，今天我们来看一下java中的比较器的底层原理是什么。。

我们知道基本数据类型的大小比较是直接通过两个数相减的得到两个数大小信息的，那如果是引用信息该如何人比较呢？

首先来看基本数据类型的包装类---他们都实现了Comparebal接口

整型--以Integer为例，Short，Long， Byte原理一样（都是Number的子类，只不过能比较的范围有所不同）

```java
public class Test {
    public static void main(String[] args) {         
        Integer a = 11;
        Integer b = 15;
        System.out.println(a.compareTo(b));
       
    }
}

```

compareTo方法的实现---调用了compare方法

> public int compareTo(Integer anotherInteger) {
>         return compare(this.value, anotherInteger.value);
>     }
> value为int类型的，自动拆箱；
> 往compare方法里面传入两个参数，

> public static int compare(int x, int y) {
>         return (x < y) ? -1 : ((x == y) ? 0 : 1);
>     }
>     
> ***x<y返回-1，x=y返回0，否则返回1***

再来看浮点型的---以Double为例，Float原理一样；

```java
package com.Date.Test;
public class Test {
    public static void main(String[] args) {
        Double c = 12.5;
        Double d = 15.5;
        System.out.println(c.compareTo(d));
     }
}

```

compareTo方法的源码--

> public int compareTo(Double anotherDouble) {
>         return Double.compare(value, anotherDouble.value);
>     }

也是调用compare方法

```java
 public static int compare(double d1, double d2) {
        if (d1 < d2)
            return -1;           // Neither val is NaN, thisVal is smaller
        if (d1 > d2)
            return 1;            // Neither val is NaN, thisVal is larger

        // Cannot use doubleToRawLongBits because of possibility of NaNs.
        long thisBits    = Double.doubleToLongBits(d1);
        long anotherBits = Double.doubleToLongBits(d2);

        return (thisBits == anotherBits ?  0 : // Values are equal
                (thisBits < anotherBits ? -1 : // (-0.0, 0.0) or (!NaN, NaN)
                 1));                          // (0.0, -0.0) or (NaN, !NaN)
    }
```

浮点型数据多了下面的方法
***doubleToLongBits方法返回个long类型的数***

public static long doubleToLongBits(double value) {
        if (!isNaN(value)) {
            return doubleToRawLongBits(value);
        }
        return 0x7ff8000000000000L;
    }

public static boolean isNaN(double v) {
        return (v != v);
    }
    public static native long doubleToRawLongBits(double value);
       
doubleToRawLongBits()方法是一个本地方法（native修饰）该函数根据保留Not-a-Number(NaN)值的IEEE 754浮点“double format”位布局返回指定浮点值的表示形式。

doubleToLongBits是将浮点型数据在内存中拿过来转为long型数据然后进行比较，即如果
当isNaN(value)为false的时候即这个数是一个正常的数，返回value，否则返回0x7ff8000000000000L;，所以我们在用compare比较两个不是正常数的数据的时候因为doubleToLongBits的返回值一样，所以compare返回值为0

小结，如果Double两个数为正常的数，他们相等时调用compareTo时返回0，如果两个为非正常的数--即不是数，返回的结果也是0，（非正常的数通过转换为long来进行比较）
验证代码如下--

```java
 Double qq=Math.sqrt(-2);
        Double qw=Math.sqrt(-4);
        System.out.println(qq.compareTo(qw));//返回0
```

isNaN方法很有意思
来看一下吧---

```java
public class Test {
    public static void main(String[] args) {
        double sqrt = Math.sqrt(-1d);
        System.out.println(sqrt==sqrt);
        System.out.println("-----------------");
        Double sqt=Math.sqrt(-1d);
        boolean naN = sqt.isNaN();
        System.out.println("isNaN()-------"+naN);
        System.out.println(sqt==sqt);
        System.out.println("--------------------");
        Double  sq=Math.sqrt(-1d);
        System.out.println("equals方法-------"+sqt.equals(sq));
        System.out.println(sqt.compareTo(sq));

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/862ad1b291393bd82a5c27b6d34e86f6.png)
我们会发现当一个数或者经过运算后不是数的时候的时候，
基本数据类型     V!=V  结果为true
包装类          V！=V  结果为 false

所以当Double类型为为NaN时，返回

再来看Boolean类，它的父类是Object，

```java
package com.Date.Test;
public class Test {
    public static void main(String[] args) {      
        Boolean s = true;
        Boolean r = false;
        System.out.println(s.compareTo(r));
    }
}

```

public int compareTo(Boolean b) {
        return compare(this.value, b.value);
    }
    
 public static int compare(boolean x, boolean y) {
        return (x == y) ? 0 : (x ? 1 : -1);
    }
比较方式跟整型一样不过只有两个值--true与false

***String的比较器-----***

```java
public class Test {
    public static void main(String[] args) {
       String str ="0aaaa";
       String str0 ="1AAAA";
        System.out.println(str.compareTo(str0));
        String q="aaaaa";
        String w="AAA";
        System.out.println(q.compareTo(w));
    }
}


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/f60e536bca3e3d4866ccd52addc81756.png)

public int compareTo(String anotherString) {
        byte v1[] = value;
        byte v2[] = anotherString.value;
        byte coder = coder();
        if (coder == anotherString.coder()) {
            return coder == LATIN1 ? StringLatin1.compareTo(v1, v2)
                                   : StringUTF16.compareTo(v1, v2);
        }
        return coder == LATIN1 ? StringLatin1.compareToUTF16(v1, v2)
                               : StringUTF16.compareToLatin1(v1, v2);
     }
先将字符串转换为byte数组，然后判断编码格式，根据编码格式在做比较

以UTF16编码为例---在 StringUTF16类中

  

```java
public static int compareToUTF16(byte[] value, byte[] other) {
        int len1 = length(value);
        int len2 = StringUTF16.length(other);
        return compareToUTF16Values(value, other, len1, len2);
    }
```

获取字符串长度，
然后比较入果长度不一样，返回两个字符串长度的差，长度一样的话比较值---一个一个比较，当比较到第一个不同的值时，返回这两个元素的ASCII码差值；

```java
 private static int compareToUTF16Values(byte[] value, byte[] other, int len1, int len2) {
        int lim = Math.min(len1, len2);
        for (int k = 0; k < lim; k++) {
            char c1 = getChar(value, k);
            char c2 = StringUTF16.getChar(other, k);
            if (c1 != c2) {
                return c1 - c2;
            }
        }
        return len1 - len2;
    }
```

> 以上就是比较器的方法比较，同时也要回覆写比较器   记住实现Compareable接口，返回值  0，-1,1然后具体比较要看具体需要；

**29.4自定义比较器**

***内部比较器***

```java
/**
 * @Auther: GavinLim
 * @Date: 2021/7/10 - 07 - 10 - 10:29
 * @Description: com.Date.Test
 * @version: 1.0
 */

//内部比较器
class Students implements Comparable<Students> {
    private int SchoolId;
    private String name;
    private int age;

    public Students(int schoolId, String name, int age) {
        SchoolId = schoolId;
        this.name = name;
        this.age = age;
    }

    public int getSchoolId() {
        return SchoolId;
    }

    public void setSchoolId(int schoolId) {
        SchoolId = schoolId;
    }

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
//内部比较器
    @Override
    public int compareTo(Students o) {
        return this.getAge() - o.getAge();
    }

//    @Override
//    public int compareTo(Students o) {
//        return this.getName().compareTo(o.getName());
//    }
}

public class Test {
    public static void main(String[] args) {
Students stu= new Students(1001,"小明",10);
Students stu1= new Students(1002,"小花",11);
        System.out.println(stu.compareTo(stu1));//-1
    }
}

```

***外部比较器***

```java
import java.util.Comparator;

class Students  {
    private int SchoolId;
    private String name;
    private int age;

    public Students(int schoolId, String name, int age) {
        SchoolId = schoolId;
        this.name = name;
        this.age = age;
    }

    public int getSchoolId() {
        return SchoolId;
    }

    public void setSchoolId(int schoolId) {
        SchoolId = schoolId;
    }

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

}

class CompareAge implements Comparator<Students> {
    @Override
    public int compare(Students o1, Students o2) {

        return o1.getAge()-o2.getAge();
    }
}

public class Test {
    public static void main(String[] args) {
Students stu= new Students(1001,"小明",18);
Students stu1= new Students(1002,"小花",11);
CompareAge compareAge= new CompareAge();
        System.out.println(compareAge.compare(stu,stu1));
    }
}

```



```java
 @FunctionalInterface public

 interface Comparator<T>    //功能性接口
```

```java
 public interface Comparable<T> //普通接口
```

> 外部比较器实现了 Comparator接口，内部比较实现了Compareable接口 

外部比较器与内部比较器的比较--
外部比较器容易扩展（继承、实现等），而且容易维护，
而内部比较器因为在类中的实现了方法，如果要继承和实现以完成其他功能就得在类中添加其他方法；

接下来是TreeSet集合中的比较器---

```java
public class Test {
    public static void main(String[] args) {
      
      TreeSet<String>treeSet= new TreeSet<>();
      treeSet.add("ayayou");
      treeSet.add("eyayou");
      treeSet.add("hyayou");
      treeSet.add("cyayou");
      treeSet.add("kyayou");
      treeSet.add("eyayou");
        System.out.println(treeSet.size());
                for (String s : treeSet) {
            System.out.println(s);
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/0158da09bae625dc1caf231a61706334.png)
TreeSet添加数据是按照升序排列的,
 TreeSet构造中添加了new TreeMap<>(),TreeMap的构造中添加了一个比较器,是一个内部比较器

> public TreeSet() {
>        this(new TreeMap<>());
>    }
>
> ```
> public TreeMap() {
>    comparator = null;
> 
> ```
>
>    }

自定义类的比较器再TreeSet中实现排序--

```java
package setTest;

import java.util.*;

/**
 * @author : Gavin
 * @date: 2021/7/19 - 07 - 19 - 18:45
 * @Description: setTest
 * @version: 1.0
 */
import java.util.Comparator;

class Students  {
    private int SchoolId;
    private String name;
    private int age;

    public Students(int schoolId, String name, int age) {
        SchoolId = schoolId;
        this.name = name;
        this.age = age;
    }

    public int getSchoolId() {
        return SchoolId;
    }

    public void setSchoolId(int schoolId) {
        SchoolId = schoolId;
    }

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

    @Override
    public String toString() {
        return "Students{" +
                "SchoolId=" + SchoolId +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

class CompareAge implements Comparator<Students> {
    @Override
    public int compare(Students o1, Students o2) {

        return o1.getAge()-o2.getAge();
    }

}

public class Test {
    public static void main(String[] args) {
        Students stu= new Students(1001,"小明",18);
        Students stu1= new Students(1002,"小花",11);
        Students stu2= new Students(1003,"小花1",12);
        Students stu3= new Students(1004,"小花2",13);
        Students stu4= new Students(1005,"小花3",11);
        Students stu5= new Students(1006,"小花4",10);
        Students stu6= new Students(1007,"小花4",12);
        Students stu7= new Students(1008,"小花5",19);
        Students stu8= new Students(1009,"小花6",17);
        CompareAge compareAge= new CompareAge();

      TreeSet<Students>treeSet= new TreeSet<>(compareAge);//指定自定义比较器
      treeSet.add(stu);
      treeSet.add(stu1);
      treeSet.add(stu2);
      treeSet.add(stu3);
      treeSet.add(stu4);
      treeSet.add(stu5);
      treeSet.add(stu6);
      treeSet.add(stu7);
      treeSet.add(stu8);

        System.out.println(treeSet.size());

        for (Students s : treeSet) {
            System.out.println(s);
        }
    }
}
```

如果是内部比较器的话,Students要实现Comparable接口

```java
import java.util.*;

/**
 * @author : Gavin
 * @date: 2021/7/19 - 07 - 19 - 18:45
 * @Description: setTest
 * @version: 1.0
 */
import java.util.Comparator;

class Students implements Comparable<Students> {
    private int SchoolId;
    private String name;
    private int age;

    public Students(int schoolId, String name, int age) {
        SchoolId = schoolId;
        this.name = name;
        this.age = age;
    }

    public int getSchoolId() {
        return SchoolId;
    }

    public void setSchoolId(int schoolId) {
        SchoolId = schoolId;
    }

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

    @Override
    public String toString() {
        return "Students{" +
                "SchoolId=" + SchoolId +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public int compareTo(Students o) {
        if(this.getAge()==o.getAge()){
            return 0;
        }
        else if(this.getAge()>o.getAge()){
            return -1;
        }else {
            return  1;
        }
    }
}

//class CompareAge implements Comparator<Students> {
//    @Override
//    public int compare(Students o1, Students o2) {
//
//        return o1.getAge()-o2.getAge();
//    }

//}

public class Test {
    public static void main(String[] args) {
        Students stu= new Students(1001,"小明",18);
        Students stu1= new Students(1002,"小花",11);
        Students stu2= new Students(1003,"小花1",12);
        Students stu3= new Students(1004,"小花2",13);
        Students stu4= new Students(1005,"小花3",11);
        Students stu5= new Students(1006,"小花4",10);
        Students stu6= new Students(1007,"小花4",12);
        Students stu7= new Students(1008,"小花5",19);
        Students stu8= new Students(1009,"小花6",17);
//        CompareAge compareAge= new CompareAge();

      TreeSet<Students>treeSet= new TreeSet<>();//指定自定义比较器
      treeSet.add(stu);
      treeSet.add(stu1);
      treeSet.add(stu2);
      treeSet.add(stu3);
      treeSet.add(stu4);
      treeSet.add(stu5);
      treeSet.add(stu6);
      treeSet.add(stu7);
      treeSet.add(stu8);

        System.out.println(treeSet.size());

        for (Students s : treeSet) {
            System.out.println(s);
        }
    }
}
```

按照年龄降序排列,同时剔除了年两相同的学生
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/38ecf68cee2c1c2ee97cb89ed030a528.png)
可以尝试下匿名内部类或者定义功能接口,lambda表达式去完成;
TreeSet二叉树底层遍历是中序遍历得到的

> 二叉树的遍历---
> 中序遍历、、先序遍历、、后续序遍历
>    中序遍历按访问左、根、右进行遍历的 
>    先序遍历是根、左、右 
>    后序遍历是左、右、根



## 30.Annotation

1，Anotation的作用和内建的三个Annotation；

2，自定义Annotation的格式；

3，Annotation与反射机制；

JDK1.5之后的新特性--

枚举，自动装箱、拆箱、foreach，可变参数，静态导入，泛型

Annotation实际上是一种注释语法，在java中最早的程序提倡程序与配置代码相分离，而最新的理论是将所有的配置写入到程序之中，如果要想完成这样的功能就要使用Annotion

**30.1.1系统内建的Annotation**

在JDK1.5之后增加了三个Annotation：@Override、@Deprecated、@SupressWarning

**30.1.1.1.@Override--覆写的**

@Override表示正确的覆写操作，例如现在有如下两个类;

```java
public class PersonDemo {
	public String Say() {
		return "I love you !!!";
	}
}
```

现在要求增加此类的子类，并且在子类中覆写Say()方法： 

```java
public class AnnotationDemo extends PersonDemo{
	public static void main(String[] args) {
	}
	@Override
	public String Say() {
		return "我站在桥上看风景";	}}


```

现在我正确覆写了Say()方法，但是有以下可能，若我将Say不小心写成了say，那么这就不算是覆写了，在系统中为了保证程序可以正确执行操作，那么就可以用@Override来表示方法属于覆写操作 

```java
public class PersonD{
	public static void main (String []args) {
		Person per = new Student();//父类通过子类实例化
		System.out.println(per.Say());	}}
public class PersonD{
	public static void main (String []args) {
		Person per = new Student();//父类通过子类实例化
		System.out.println(per.Say());	}}
		class Student extends Person{
	@Override
	public String say() {
		return "我被覆写了";	}}
 class Person {
	public String Say() {
		return "I love you !!!";	}}



```

![](..\pic\图片78.png)

 

**30.1.1.2.@Deprecated--不建议使用的**

@Deprecated，表示不建议使用的操作；例如线程中的stop(),resum().supend()方法是不建议使用的

```
public class PersonD {
	public static void main(String[] args) {
	}
	@Deprecated//表示此方法不建議使用
	public String getName() {
		return "Hello World";
	}
}
```

使用@Deprecated声明标志此方法是不建议使用的操作，如果使用的话只会提示警告信息，但并不影响使用（会将该方法划一横线）

**30.1.1.3.@SupressWarning--压制警告**

@SupressWarning表示的是压制警告，如果出现警告，则压制警告，不出现警告信息

使用SupressWarnings可以压制很多个警告

通过SupressWarnings可以发现在此类中存在Value的字符串

PersonD中既继承了Personp类又实现了序列化接口，所以会出现多个警告，上面提到过SupressWarnings可以压制很多个警告，所以以下代码我们可以发现--@SupressWarning是以数组的形式来存储，可以明确表示出@SupressWarning中的value属性;



**30.1.1.4自定义Annotation**

我们来看一下系统中的三种Annotation定义--

```java
Annotation Type Override

@Target(METHOD)

@Retention(SOURCE)

public @interface Override



Annotation Type SuppressWarnings

@Target({TYPE,FIELD,METHOD,PARAMETER,CONSTRUCTOR,LOCAL_VARIABLE,MODULE})

@Retention(SOURCE)

public @interface SuppressWarnings
    
    Annotation Type Deprecated
@Documented
@Retention(RUNTIME)
@Target({CONSTRUCTOR,FIELD,LOCAL_VARIABLE,METHOD,PACKAGE,MODULE,PARAMETER,TYPE})
public @interface Deprecated


```

定义Annotation 的语法 如 下--

public @interface 名称()

下面按照此格式定义一个简单的Annotation

```java
public @interface myAnnotation{

}
```

```java
@myAnnotation
public class Xixi {
public String getVoice() {	return "你好--世界";}
	public static void main(String[] args) {			}}

```



在一个Annotation中可以定义多个属性-- 

```java
public @interface myAnnotation{
	public String key();
	public String value();
}

```

 

在此Annotation中定义了key 和 value两个变量，则此时如果想要使用Annotation的话，则必须明确的是给出具体值的内容。

```java
@myAnnotation(key ="Hello",value="World")
public class Xixi {
public String getVoice() {
	return "你好--世界";}
	public static void main(String[] args) {
			}}

```



如果现在希望为一个Annotation设置默认内容的话，可以通过Default来完成

```java
public @interface myAnnotation{
	public String key()default("Hello");
	public String value()default("World");
}

@myAnnotation//如果没有设置内容，则将默认值取出继续使用
public class Xixi {
public String getVoice() {
	return "你好--世界";}
	public static void main(String[] args) {			}}

```



Annotation中的变量可以通过枚举进行指定取值范围

1,定义好枚举类

```java
public enum Color {
	RED,GREEN,BLUE;
}


```

 

2，枚举将限制Annotation的取值

```java
public @interface myAnnotation{
	public String key()default("Hello");
	public String value()default("World");
	public Color color()default Color.RED;
}


```



如果在xixi类中使用此Annotation的时候并没有指定枚举规定的取值范围的内容，则会出现错误！ 

```java
@myAnnotation(key="Hello",value="world",color=Color.BLUE)
public class Xixi {
public String getVoice() {
	return "你好--世界";}
	public static void main(String[] args) {			}}

```

![](..\pic\图片79.png)

在Annotation中的变量还可以使用一个数组的形式进行表示 ![](..\pic\图片80.png)

则在以后使用的时候必须按照数组的形式使用，--即必须出现数组的形式，其他的可以不出现 ;

```java
@myAnnotation(DDV={"qwer","asdf"})
public class Xixi {
public String getVoice() {
	return "你好--世界";
}
	public static void main(String[] args) {
			}}


```

**30.1.1.5.@Retention和@RetentionPolicy**

在Java.annotation包中定义了所有与Annotation有关的操作

Retention本身是一个Annotation。其中的取值是通过RetationPolicy这个枚举类型指定

```java
Annotation Type Retention
@Documented
@Retention(RUNTIME)
@Target(ANNOTATION_TYPE)
public @interface Retention


Enum RetentionPolicy
java.lang.Object
java.lang.Enum<RetentionPolicy>
java.lang.annotation.RetentionPolicy
All Implemented Interfaces:
Serializable, Comparable<RetentionPolicy>, Constable
public enum RetentionPolicy extends Enum<RetentionPolicy>


```

在Retention中定义了以下三个范围

SOURCE--只在源代码中起作用

public static final RetentionPolicy SOURCE

CLASS--只在编译之后的Class中起作用

public static final RetentionPolicy CLASS

RUNTIME--在运行的时候起作用

public static final RetentionPolicy RUNTIME                                                                     

如果一个Annotation想要起作用，则必须使用RUNTIME范围;

**30.2反射与Annotation**

一个Annotation如果想起作用，则必须依靠反射机制，通过反射可以取得在一个方法上生命的Annotation的全部内容；

在Filed、Method、Constructor的父类上定义了以下与Annotation反射操作的相关方法

1，取得全部的Annotation

getAnnotation

public <T extends Annotation> T getAnnotation(Class<T> annotationClass)

2，判断是否是指定的Annotation

isAnnotationPresent

public boolean isAnnotationPresent(Class<? extends Annotation> annotationClass)

**30.2.1取得全部的Annotation**

​             

现在使用一个方法使用三个内建的

Annotataion声明

```java
public class Xixi {
	@Override
	@Deprecated
	@SuppressWarnings( value = { "2" })
public String toString() {
	return "你好--世界";
}
	public static void main(String[] args) {
			}}

```

以上三个内建Annotation只有@Deprecated是RUNTIM类型，证明，如果程序在运行的时候当取得了全部的Annotation，只能取得一个--@Deprecated

```java
import java.lang.annotation.Annotation;
import java.lang.reflect.Method;
public class Information {
	public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, SecurityException {
		Class<?> cls = Class.forName("vvvv.Xixi");
		System.out.println(cls);
		Method toStringMethod = cls.getMethod("toString");
		Annotation ant[] = toStringMethod.getAnnotations();// 取得全部的Annotation
		for (Annotation annotation : ant) {
			System.out.println(annotation);
		}	}}

```

**30.2.2使用自定义的Annotation**

在一个自定义的Annotation编写时候如果要让其有意义，则必须使其范围被声明为RUNTIME

```java
import java.lang.annotation.*;
@Retention(value=RetentionPolicy.RUNTIME)
//通过RetentionPolicy指定Annotion的类型范围
public @interface myAnnotation{
	public String key()default("Hello");
	public String value();}

```

```java
public class Xixi {
	@Override
	@Deprecated
	@SuppressWarnings(value = { "ce" })
	@myAnnotation( value = "ce")
	public String toString() {
		return "你好--世界";	}
	public static void main(String[] args) {	}}

public class Xixi {
	@Override
	@Deprecated
	@SuppressWarnings(value = { "ce" })
	@myAnnotation( value = "ce")
	public String toString() {
		return "你好--世界";	}
	public static void main(String[] args) {	}}


```

```java
import java.lang.annotation.Annotation;
import java.lang.reflect.Method;
public class Information {
	public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, SecurityException {
		Class<?> cls = Class.forName("vvvv.Xixi");
		System.out.println(cls);
		Method toStringMethod = cls.getMethod("toString");
		Annotation ant[] = toStringMethod.getAnnotations();// 取得全部的Annotation
		for (Annotation annotation : ant) {
			System.out.println(annotation);		}	}}


```

要取出程序中指定的Annotation类型的值 

```java
import java.lang.annotation.Annotation;
import java.lang.reflect.Method;
public class Information {
	public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, SecurityException {
		Class<?> cls = Class.forName("vvvv.Xixi");
		System.out.println(cls);
		Method toStringMethod = cls.getMethod("toString");
		Annotation ant[] = toStringMethod.getAnnotations();// 取得全部的Annotation
		for (int i=0;i<ant.length;i++) {
			if(toStringMethod.isAnnotationPresent(myAnnotation.class)) {
				myAnnotation my =null;//声明Annotation对象为空
				my = toStringMethod.getAnnotation(myAnnotation.class);
				String key =my.key();
				String value =my.value();
				System.out.println(key+"--------"+value);
			}	}}}

```

**30.2.3.深入Annotation**

在Java.lang.annotation中存在一下的Annotation，Target、Documented、Inherited

**30.2.3.1.@Targe**t

```java
@myAnnotation(value = "ce")//在类的位置上使用
public class Xixi {
	@myAnnotation(value = "ce")//在属性的位置上使用
	private String name;
	@myAnnotation(value = "ce")//在方法的位置上使用
	public String toString() {
		return "你好--世界";	}

```

因为可以在任意位置使用，则在操作的时候就会出现一些问题；例如一些Annotation是希望在方法的声明上使用，那么此时就必须设置Annotation的作用范围（前边学到自定义的Annotation过指定作用范围格式为@Retention(value=RetentionPolicy.RUNTIME)）
在@Target注释中，存在ElementType类型的变量--七种变量类型
1，ANNOTATION_TYPE--只能在Annotation中出现

> public static final ElementType ANNOTATION_TYPE

Annotation type declaration

2，CONSTRUCTOR--只能在构造方法中出现

> public static final ElementType CONSTRUCTOR

Constructor declaration

3，LOCAL_VARIABLE--本地变量

> public static final ElementType LOCAL_VARIABLE

Local variable declaration

4，METHOD--只能在方法中出现

> public static final ElementType METHOD

Method declaration

5，PARAMETER--只能在参数声明上使用

> public static final ElementType PARAMETER

Formal parameter declaration

6，PACKAGE--只能在包中使用

> public static final ElementType PACKAGE

Package declaration

7，TYPE--只能在类、接口中使用

> public static final ElementType TYPE

Class, interface (including annotation type), enum, or record declaration

在JDK1.5之后又增加了一些其他内容\--

```java
import java.lang.annotation.*;
@Target(value=ElementType.METHOD)//定义此Annotation注释只能在方法中声明
@Retention(value=RetentionPolicy.RUNTIME)//通过Retention指定Annotion的类型范围
public @interface myAnnotation{
	public String key()default("Hello");
	public String value();
}

```

由于Target中定义了Annotation的作修饰范围，所以以下的自定义声明就会提示出现错误 

```java
@myAnnotation(value = "ce")//在类的位置上使用
public class Xixi {
	@myAnnotation(value = "ce")//在属性的位置上使用
	private String name;
	@myAnnotation(value = "ce")//在方法的位置上使用
	public String toString() {
		return "你好--世界";	}

```

![](..\pic\图片81.png)

如果想要扩大范围，则可以指定多个范围 

```java
import java.lang.annotation.*;
@Target(value= {ElementType.METHOD,ElementType.TYPE,ElementType.CONSTRUCTOR})
@Retention(value=RetentionPolicy.RUNTIME)//通过Retention指定Annotion的类型范围
public @interface myAnnotation{
	public String key()default("Hello");
	public String value();
}


```

**30.2.3.2.@Documented**

此种Annotation表示的是文档注释  

```java
import java.lang.annotation.*;
@Documented
public @interface myAnnotation {
	public String key() default ("Hello");
	public String value();}
```

在使用类中可以加入文档注释

```java
@myAnnotation(value = "KEY")
public class Xixi {
	private String name;
		/*
	 * 本方法是覆写toString
	 */
	public String toString() {
		return "你好--世界";
	}

```

加入文档那个注释最大的好处就是可以在doc文档中出现 

![](..\pic\图片82.png)

![](..\pic\图片83.png)

**30.2.3.3.@Inherited**

表示一个Annotation能否被其子类继续继承下去，如果没有此注释，则此Annotation根本就无法被继承；

```java
import java.lang.annotation.*;
@Inherited//此Annotation可以被子类所继承；
@Documented
@Target(value= {ElementType.METHOD,ElementType.TYPE})
@Retention(value=RetentionPolicy.RUNTIME)//指定作用范围
public @interface myAnnotation {
	public String key() default ("Hello");
	public String value();
}


```

```java
import java.lang.annotation.Annotation;
public class Infoo extends Xixi {
	public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, SecurityException {
		Class<?> cls = Class.forName("vvvv.Xixi");
		Annotation ant[] = cls.getAnnotations();// 取得全部的Annotation
		for (int i = 0; i < ant.length; i++) {
			System.out.println(ant[i]);		}	}}

```

## 31. 进程与线程

1，掌握多线程的两种实现方式；

2，了解多线程的基本操作；

3，了解同步及死锁的概念；

**31.1进程与线程**

从计算机发展来看，经历了--

单进程处理：最传统的DOS系统中只要有病毒出现，则立刻会有反应--因为在DOS系统中属于单进程处理，即在同一个时间点上只能有一个程序在执行；

多进程处理：windows操作系统中是一个多进程，例如，假设windows中出现病毒，则程序可以照常使用--

![](..\pic\图片84.png)

那么对于资源来讲，所有的IO设备、CPU等只有一个，对于多进程来讲，在同一个时间段上会有多个程序运行，但是在同一个时间点上只能有一个程序运行；

线程是在进程基础上进一步划分，举个不太恰当的例子便于理解： word中的拼写检查是在程序运行中运行的；

如果进程消失了，则线程也就消失了，如果线程消失了，则进程会依然执行为未必会消失，所以JAVA中提供了线程与进程操作；

**31.2.线程实现的两种方式**

在java中可以有两种方式实现多线程操作，一种是继承Thread类，另一种是是实现Runnable接口

**31.2.1.Thread类**

Thread类是在java.lang中定义的----

```java
Class Thread
java.lang.Object
java.lang.Thread
All Implemented Interfaces:
Runnable
Direct Known Subclasses:
ForkJoinWorkerThread
public class Thread extends Object implements Runnable

```

一个类如果继承了Thread类，同时覆写了本类中的run()方法，则就可以实现多线程操作

```java
class myThread extends Thread {
	private String name;// 定义name属性
	public myThread(String name) {
		this.name = name;	}
	public void run() {// 覆写run方法
		long start = System.currentTimeMillis();
		for (int i = 0; i < 2; i++) {// 循环2次
			System.out.println("Tread运行" + name + ",i=" + i);		}
		long end = System.currentTimeMillis();
		System.out.println(end - start);	}}

```

以上的类已经实现了多线程的操作类，那么下面该怎么去启动多线程呢？

1，通过创建对象调用进程，

2，反射机制去获得对象调用进程；

我们来看代码--

```java
public class ThreaDemo {
	public static void main(String[] args) {
		myThread my0 = new myThread("线程A");
myThread my1 = new myThread("线程B");
myThread my2 = new myThread("线程C");
long start = System.currentTimeMillis();
my0.run();//调用线程体
my1.run();
my2.run();
long end = System.currentTimeMillis();
System.out.println((end - start)+"ms");	}}

```

由运行结果我们发现，程序会依次执行，也就是说没有实现交互运行；
从JDK文档中可以发现，一旦调用start()方法，则会通过JVM虚拟机找到run()方法，
public void start()
Causes this thread to begin execution; the Java Virtual Machine calls the run
The result is that two threads are running concurrently: the current thread (which returns from the call to the start method) and the other thread (which executes its run method).
It is never legal to start a thread more than once. In particular, a thread may not be restarted once it has completed execution.

 使用start方法启动线程-- 

```java
public class ThreaDemo {
	public static void main(String[] args) {
myThread my0 = new myThread("线程A");
myThread my1 = new myThread("线程B");
myThread my2 = new myThread("线程C");
long start = System.currentTimeMillis();
my0.start();//调用线程体
my1.start();my2.start();long end = System.currentTimeMillis();
System.out.println((end - start)+"ms");	}}

```

​                                                                                             

此时程序可以正常进行交互式运行了，但是需要思考的是为什么要用

start()来启动多线程呢？

在JDK安装路径下src.zip是java全部源代码

```java
 public synchronized void start() {//定义方法
	           if (threadStatus != 0)//启动时如果线程状态不等于0，则抛出非法线程异常
	            throw new IllegalThreadStateException();
	        group.add(this);//将线程添加到进程组里

	        boolean started = false;
	        try {
	            start0();//启动start0(),
	            started = true;
	        } finally {
	            try {
	                if (!started) {//如果started为false则线程启动失败
	                    group.threadStartFailed(this);
	                }
	            } catch (Throwable ignore) {//不做任何处理	              
	            }	        }	    }
	    private native void start0();//使用native方法没有方法体；
	    @Override
	    public void run() {
	        if (target != null) {
	            target.run();	   }  }

```

对一个线程用run启动多次是可以的，但是用

start启动则会抛出异常

操作系统有很多种，windows，Linux，Unix，既然多线程呢个操作中要进行CPU资源的抢占，也就是说要等待CPU调度，那么这些调度的操作是由操作系统底层实现的，所以在JAVA中根本就没法实现，所以java开发者在设计是定义了native关键字，使用此关键字表示可以调用操作系统底层的函数，那么这样的技术成为JNI技术（Java Native Interface）

而且此方法在执行的时候将调用run()方法完成，由系统默认调用的；

但是，第一种操作中由于各最大的限制，一个类只能继承一个父类，如果要实现多线程又要继承其他类，那么怎么实现的呢？

**31.2.2.Runnable 接口**

在实际开发中一个多线程的操作类很少去使用Thread类去完成，二是通过Runnable接口完成，观察Runnable接口的定义--

```java
public interface Runnable{

Public void run()

}

```



```java
class Mythread implements Runnable{
private String name;
public Mythread(String name) {
	this.name=name;}
	@Override
	public void run() {
		// TODO Auto-generated method stub
		for (int i =0;i<10;i++) {
			System.out.println("thread运行"+name+"，i="+i);
		}	}}

```

以上就实现了Runnable接口，name该怎么去启动线程呢？
但是现在使用的Runnable定义的子类中并没有Start方法，只有在Thread类中才有；在Thread类中存在以下的构造方法，此构造方法接受Runnable的子类实例，也就是说现在可以通过Thread类来启动Runnable实现多线程（启动多线程 ----必须用到start()来协调系统的资源调配（因为start方法中start0的修饰符为native--调用系统内部来实现资源调配）但是由于Start方法只在Thread类中才有，所以只有通过Thread来接收Runnable的子类实例，以此来调用start方法）
public Thread(Runnable target)---通过构造方法接收Runnable实例

```java
public class PlayGame   {
	public static void main(String[] args) {
Mythread my0 = new Mythread("线程A");
Mythread my1 = new Mythread("线程B");
new Thread (my0).start();
new Thread(my1).start();	}}


```

所以这个时候实现了多线程；

**31.2.3.两种实现多线程的区别**

在程序开发中实现多线程肯定是一实现Runnable接口作为标准，因为实现Runnable接口相比于继承Thread类有以下好处：

1，避免单继承局限，一个类可以实现多个接口

2，适合于资源的共享（重要----多个线程共享资源）

卖票案例

```java
 class Ticket extends Thread{
private int ticket = 5;//5張票
 public void run() {
	for (int i =0;i<10;i++) {
		if(this.ticket>0) {
			System.out.println("卖票："+this.ticket--);
		}	}	}   }

```

下面建立三个线程同时卖票； 

```java
public class Sell{
	public static void main (String args[]) {
		Ticket T0 =new Ticket();
		Ticket T1 =new Ticket();
		Ticket T2 =new Ticket();
		T0.start();
		T1.start();
		T2.start();	}}

```

发现现在一共卖了15张票，但是实际上只有5张票，所以这就证明了每一个线程都买自己的票，没有达到资源共享的目的；

如果现在实现的是Runnable接口的话，那么就可以实现资源共享的;

```java
 class Ticket implements Runnable{
private int ticket = 5;//5張票
 public void run() {
	for (int i =0;i<10;i++) {
		if(this.ticket>0) {
			System.out.println("卖票："+this.ticket--);
		}	}	}   }
public class Sell{
	public static void main (String args[]) {
		Ticket T0 =new Ticket();
		Ticket T1 =new Ticket();
		Ticket T2 =new Ticket();
		new Thread(T0).start();
		new Thread(T1).start();
		new Thread(T2).start();	}}

```

由结果可以发现，虽然现在程序中有三个线程，三个线程一共卖了5张票，即---由Runnable实现线程可以达到资源共享的目的；
实际上Runnable接口和Thread类之间还是存在联系的---
public class Thread   extends Object  implements Runnable
Thread也是 Runnable的子类

![](..\pic\图片85.png)

**31.2.4.线程的操作方法**

对于线程来讲，所有的操作方法都是在Thread类中定义的，所以如果想明确理解操作方法，则要深入了解Thread类

**31.2.4.1.线程的名字**

设置和取得名字

setName---设置线程名字

```java
public final void getName
```

通过Thread--构造方法设置名字

```java
public Thread(Runnable target, String name)
```

在线程的操作中因为哦操作的不确定，所以系统提供了取得当前线程的操作

getName()--取得线程的名字

```java
public final String getName() setName(String name)
```

对于线程的名字一般是在启动前进行设置，最好不要设置相同的名字，最好不要为一个线程改名字 

```java
class Ticket implements Runnable {
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.print(Thread.currentThread().getId() + "----");
			System.out.println(Thread.currentThread().getName() + "线程正在运行");	}	}}
public class Sell {
	public static void main(String args[]) {
		Ticket T0 = new Ticket();
		Thread thread0 = new Thread(T0, "线程A");
		Thread thread1 = new Thread(T0, "线程B");
		Thread thread2 = new Thread(T0, "线程C");
		thread0.start();
		thread1.start();
		thread2.start();	}}

```

```java
class Ticket implements Runnable {
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.print(Thread.currentThread().getId() + "----");
			System.out.println(Thread.currentThread().getName() + "线程正在运行");
		}	
}
}
public class Sell {
	public static void main(String args[]) {
		Ticket T0 = new Ticket();
		new Thread (T0,"自定义线程").start();
		T0.run();	//直接通过对象访问
}}

```

结论：在程序运行时主方法实际就是一个主线程

Java是怎么实现多线程的？实际上对于java来讲，每一次执行Java命令，对于操作系统来讲，都将启动一个JVM进程，那么主方法实际上只是这个进程的进一步划分，

问题--

在Java执行中一个Java程序至少启动几个线程？---主线程和垃圾回收线程

**31.2.4.2.线程的休眠**

让一个线程稍微休息一下，之后棋类继续工作，成为休眠

Public static void sleep(Long  millis) throw InterruptedException

在使用方法需要进行 try  -catch

程序的进行将变得缓慢，其中

try --- catch  中catch捕捉到的异常是InterruptedException线程中断

```java
class Ticket implements Runnable {
	public void run() {
		for (int i = 0; i < 10; i++) {
			try {
			Thread.sleep(500);
			}catch(InterruptedException ex){
				ex.printStackTrace();			}
			System.out.print(Thread.currentThread().getId() + "----");
			System.out.println(Thread.currentThread().getName() + "线程正在运行");
		}	}}
public class Sell {
	public static void main(String args[]) {
		Ticket T0 = new Ticket();
		new Thread (T0,"自定义线程").start();
		T0.run();	}}

```

**31.2.4.3.线程的中断**

在Sleep方法中存在线程中断异常--

```java
public class InterruptedException extends Exception
```

在活动之前或活动期间，线程在等待，休眠或以其他方式占用并且线程被中断时抛出。有时，一种方法可能希望测试当前线程是否已被中断，如果已中断，则立即抛出此异常。以下代码可用于实现此效果：

  if（Thread.interrupted（））//清除中断状态！

​      抛出新的InterruptedException（）;

一个线程需要被另外一个线程中断--

```java
class Ticket implements Runnable {
	public void run() {
		System.out.println("1run方法开始运行");
		for (int i = 0; i < 10; i++) {
			try {
				System.out.println("2线程将要休眠25s");
				Thread.sleep(2500);// 线程休眠25s
				System.out.println("3线程正常休眠25s");
			} catch (InterruptedException ex) {
				System.out.println("4线程休眠被中断");
				// ex.printStackTrace();
				return;// 返回方法调用处
			}
			System.out.println("5正常结束run方法体");		}	}}
public class Sell {
	public static void main(String args[]) throws InterruptedException {
		Ticket T0 = new Ticket();
		Thread thread = new Thread(T0, "线程A");// 将线程保存下来
		thread.start();// 启动线程
		try {
			thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();		}
		thread.interrupt();// 中断线程	
		}}

```

**31.2.4.4.线程的优先级**

在多线程操作中，代码实际上都是存在优先级的，一般来讲，优先级高的可能会先执行，在线程中使用以下方法设置优先级setPriority

public final void setPriority(int newPriority)

对于优先级来说，在程序中有如下三种：

高------MAX_PRIORITY     public static final int -MAX_PRIORITY

中-----NORM_PRIORITY    public static final int NORM_PRIORITY 

低

-----MIN_PRIORITY      public static final int MIN_PRIORITY

设置优先级 

```java
class Ticket implements Runnable {
	public void run() {
		for (int i = 0; i < 10; i++) {
			try {
				Thread.sleep(600);//加入延迟操作
			} catch (InterruptedException e) {
				e.printStackTrace();			}
			System.out.println(Thread.currentThread() + "线程正在运行");		}	}}
public class Sell {
	public static void main(String args[]) throws InterruptedException {
		Ticket T0 = new Ticket();
		Thread thread0 = new Thread(T0, "线程A");// 将线程保存下来
		Thread thread1 = new Thread(T0, "线程B");
		Thread thread2 = new Thread(T0, "线程C");
		thread2.setPriority(Thread.MAX_PRIORITY);// 最高优先级
		thread1.setPriority(Thread.MIN_PRIORITY);// 最小最高优先级
		thread0.setPriority(Thread.NORM_PRIORITY);// 中优先级
		thread0.start();
		thread1.start();
		thread2.start();	}}

```

各个线程都可以线程的优先级，主方法main是一个怎样的优先级？ 

```java
public class Games {
	public static void main(String args[]) {
System.out.println(Thread.currentThread());}}

```

```java
public class Games {
	public static void main(String args[]) {
System.out.println(Thread.currentThread());
System.out.println("Thread.MAX_PRIORITY的优先级"+Thread.MAX_PRIORITY);
System.out.println("Thread.NORM_PRIORITY的优先级"+Thread.NORM_PRIORITY);
System.out.println("Thread.MIN_PRIORITY的优先级"+Thread.MIN_PRIORITY);
}}

```

从输出结果可以发现  main方法的优先级为5---NORM_PRIORITY 

但是并不时优先级越大,就一定先执行;

**31.3.同步与死锁**

**31.3.1.问题的引出**

```java
class BBK implements Runnable {
	private int num = 10;
	@Override
	public void run() {
		System.out.println("开始卖票");
		for (int i = 0; i < 10; i++) {
			if (this.num > 0) {// 如果还有票，则开始卖票
			System.out.println(this.num-- + "张票");	}}}}
public class ThreaDemo {
	public static void main(String[] args) {
		BBK bbk = new BBK();
		new Thread(bbk, "票贩子A").start();
		new Thread(bbk, "票贩子B").start();
		new Thread(bbk, "票贩子C").start();	}}

```

假设存在网络延迟的话，那么会怎么样呢？ 

```java
class BBK implements Runnable {
	private int num = 10;
	@Override
	public void run() {
		System.out.println("开始卖票");
		for (int i = 0; i < 10; i++) {
			if (this.num > 0) {// 如果还有票，则开始卖票
				try {
					Thread.sleep(300);//延迟
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			System.out.println(this.num-- + "张票");	}}}}
public class ThreaDemo {
	public static void main(String[] args) {
		BBK bbk = new BBK();
		new Thread(bbk, "票贩子A").start();
		new Thread(bbk, "票贩子B").start();
		new Thread(bbk, "票贩子C").start();	}}

```

造成此类问题的根本原因在于，判断剩余票数和修改票数之间加入例如延迟操作-- 



在Java中可以通过同步代码的方式进行代码的加锁操作--

1---同步代码块

2---同步方法

**31.3.2.同步解决**

同步代码块：

使用synchronized关键字进行同步代码块的声明，但是使用此操作时必须明确指出要锁定的是哪个对象，一般就是以当前对象为主；

Synchronized(对象){   

需要同步的代码}

使用同步代码块重写上述代码--

```java
class BBK implements Runnable {
	private int num = 10;
	@Override
	public void run() {
		System.out.println("开始卖票");
		synchronized (this) {
			for (int i = 0; i < 15; i++) {
				if (this.num > 0) {// 如果还有票，则开始卖票
					try {
						Thread.sleep(300);
					} catch (InterruptedException e) {
						e.printStackTrace();					}
					m.out.println(this.num-- + "张票");	
}
}
}
}
}
public class ThreaDemo {
	public static void main(String[] args) {
		BBK bbk = new BBK();
		new Thread(bbk, "票贩子A").start();
		new Thread(bbk, "票贩子B").start();
		new Thread(bbk, "票贩子C").start();	}}

```

以上确实解决了同步的问题，但是在Java中也可以通过增加同步方法的方式来完成同步

形式如下----- 

public synchronized void sale() {  要同步的代码  }

```java
class BBK implements Runnable {
	private int num = 10;
	@Override
	public void run() {
		System.out.println("开始卖票");
				for (int i = 0; i < 15; i++) {
				this.sale();//调用同步方法
			}}
	public synchronized void sale() {//增加同步方法
		if (this.num > 0) {// 如果还有票，则开始卖票
			try {
				Thread.sleep(300);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();			}
			System.out.println(this.num-- + "张票");	
}
}
}
public class ThreaDemo {
	public static void main(String[] args) {
		BBK bbk = new BBK();
		new Thread(bbk, "票贩子A").start();
		new Thread(bbk, "票贩子B").start();
		new Thread(bbk, "票贩子C").start();	}}

```

此时，实际傻上就可以给出JAVA中方法定义的完整格式了。

1--访问权限   

2--Staitic、final、synchronized、native

3--返回值类型或void（参数列表）throws 异常

使用同步的好处是能保证资源的完整性，但是过多的使用同步也会出现问题--

**31.3.3.死锁**

在程序中过多的同步会产生死锁的问题，那么死锁属于程序运行时发生的一种状态，只需要观察到死锁的最终运行状态即可，不必深入研究；



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
		new ThreaDemo();	}}

```

产生死锁的必要条件：

1,互斥条件：进程要求对所分配的资源进行排它性控制，即在一段时间内某资源仅为一进程所占用。

2,请求和保持条件：当进程因请求资源而阻塞时，对已获得的资源保持不放。

3,不剥夺条件：进程已获得的资源在未使用完之前，不能剥夺，只能在使用完时由自己释放。

4,环路等待条件：在发生死锁时，必然存在一个进程--资源的环形链。

**31.3.4.消费者与生产者**

在多线程中经典的操作案例--生产者生产内容，但是消费者不断取出内容

**31.3.4.1.基本实现方式**

现在假设现在生产的信息保存在Info类之中，则在生产者要有Info类的引用，消费者也要有Info类的引用

生产者不断产生信息，消费者不断取出--

```java
class Info {
	private String Title = "林春雨";
//private String Title ="www.baidu.com";
	private String Profe = "老师";
	public String getTitle() {
		return Title;	}
	public void setTitle(String title) {
		Title = title;	}
	public String getProfe() {
		return Profe;	}
	public void setProfe(String profe) {
		Profe = profe;	}}
class Custermor implements Runnable {
	private Info info;
	public Custermor(Info info) {
		this.info = info;	}
	@Override
	public void run() {
		// TODO Auto-generated method stub
		for (int x = 0; x < 100; x++) {
			try {				Thread.sleep(300);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();			}
			System.out.println(this.info.getTitle() + "-->" + this.info.getProfe());
		}	}}

```

```java

class Producer implements Runnable {
	private Info info = null;
	public Producer(Info info) {
		this.info = info;	}
	@Override
	public void run() {
		for (int x = 0; x < 100; x++) {
			if (x % 2 == 1) {
				this.info.setProfe("www.baidu.com");
				try {					Thread.sleep(300);
				} catch (InterruptedException e) {
					e.printStackTrace();				}
				this.info.setTitle("百度");
			} else {
				this.info.setProfe("李二苟");
				try {
					Thread.sleep(300);
				} catch (InterruptedException e) {
					e.printStackTrace();				}
				this.info.setTitle("学生");		}		}	}}
public class Test {
	public static void main(String[] args) {
		Info info = new Info();
		Producer pro = new Producer(info);
		Custermor cus = new Custermor(info);
		new Thread(pro).start();
		new Thread(cus).start();	}}

```

从以上代码中可以发现一下几个问题：

生产的内容有可能出现不一致的情况--出现了重复取值和重复设置的问题，为了保证程序操作数据的完整性，可以直接在Info中设置一个同步方法，专门完成同步的问题;

但是但生产者和消费者设置的休眠时间不一样的时候，也会发生重复取的问题和重复设置的问题，那么该如何去掉这些问题呢？---通过Object类实现

**31.3.4.2.Object对线程的支持**

对于唤醒线程有两个操作--notify()，notifyAll()

notify()----依次排序等到操作

notify

public final void notify()

notifyAll()---按照优先级进行排序，优先级高的有可能先执行

public final void notifyAll()

```java
public class Info {
	private String Title = "zzt";
	private String Profe = "老师";
	private boolean flag = false;// 默认
	/
	 * 1，flag = true表示可以生产但不能取走 2，flag = false表示可以取走，但是不能生产
	 * @param Title
	 * @param Profe
	 */
	public synchronized void set(String Title, String Profe) {// 同步设置
		if (flag = false) {// 已经生产过了，需要等待
			try {super.wait();
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();	}		}


```

```java
		this.setTitle(Title);
		try {
			Thread.sleep(300);
		} catch (InterruptedException e) {
		this.setProfe(Profe);
		this.flag=false;//不能生产了
		super.notify();	}	}
	public synchronized void get() {// 同步取得
			if (flag = true) {
			try {
				super.wait();
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();	}		}
			System.out.println(this.Title + this.Profe);
			this.flag=true;//不能取了
			super.notify();	}
	public String getTitle() {
		return Title;	}
	public void setTitle(String title) {
		Title = title;	}
	public String getProfe() {
		return Profe;	}
	public void setProfe(String profe) {
		Profe = profe;	}}


```

## 32. JavaIO

Java 基础部分有四个部分是必须掌握的

1，面向对象：包括各个概念及使用

2，Java类集

3，Java IO

4，JDBC

本章学习内容--

1，java IO 的主要分类

2，Files 类

3，RandomAccessFile

4，字节流和字符流

**32.1.  Java IO**

在Java IO 实际上很好的体现了面形对象的设计思想；

一个接口和抽次类的行为有子类决定，呢么根据子类的实例化不同完成的功能也不同；

Java IO中所有的操作类都放在java.io包中。

主要的类和接口是 File、InputSteam、OutputSteam、Reader、Writer、Serialzable接口

**32.1.1.File类**

File类是在整个java.io包中是一个独立的类，此类的主要功能是完成与平台无关的文件操作---创建文件、删除文件

public File([String](file:///E:/%25E5%2592%2596%25E5%2595%25A1/360Download/jdk-15.0.1_doc-all/docs/api/java.base/java/lang/String.html) pathname)

File通过将给定的路径名字符串转换为抽象路径名来创建新实例。如果给定的字符串为空字符串，则结果为空的抽象路径名。

**32.1.1.1创建文件**

参数：

pathname -路径名字符串

抛出：

NullPointerException-如果pathname参数是null

在File类中提供了一下构造方法，在使用时要指定具体路径--

例如，现在要在D盘上创建一个Demo.txt文件

```java
import java.io.File;
import java.io.IOException;
public class IOprogect  {
public static void main(String args[]) {
	File file = new File("D:\\Demo.txt");
	try {
		file.createNewFile();//创建文件
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}}}

```

**32.1.1.2.删除文件**

delete

public boolean delete()

删除此抽象路径名表示的文件或目录。如果此路径名表示目录，则该目录必须为空才能删除。

请注意，Files该类定义了 当无法删除文件时delete抛出的方法IOException。这对于错误报告和诊断为什么无法删除文件很有用。

但是以上的两个操作完成了创建和删除，但是在不同的操作系统中文件的分隔符是不一样的；

Windows 中：\

Linux中：/

为了解决不同系统分隔符的要求，File类中提供了以下几个常量;



```java
import java.io.File;
import java.io.IOException;
public class IOprogect  {
public static void main(String args[]) {
	File file = new File("D:\\Demo.txt");
	try {
		file.createNewFile();//创建文件
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}
	file.delete();//删除文件
}}


```

```java
pathSeparator --路径分隔符  （:）
public static final String pathSeparator
pathSeparatorChar
public static final char pathSeparatorChar
Separator--分隔符 (/或者\)
public static final String separator
separatorChar 
public static final char separatorChar

```

![](..\pic\图片88.png)

所以，对于实际开发来讲，必须使用这样的常量进行声明；

正经的常量应该字母全部大写，但是此处并没有，这就是java历史发展的原因，所以开发时必须进行常量的声明，

File.Separator进行分割

**32.1.1.3.判断文件是否存在**

exists

public boolean exists()---

测试此抽象路径名表示的文件或目录是否存在

```java
import java.io.File;
import java.io.IOException;
public class IOprogect  {
public static void main(String args[]) {
	File file = new File("D:"+File.separator+"Demo.txt");
	if(file.exists()) {
		System.out.println("文件已存在");	}
	try {
		file.createNewFile();//创建文件
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();	}}}

```

现在要求编写一个程序，此程序的主要功能如下，如果文件存在，则删除，如果文件不存在则创建一个新文件--- 

```java
import java.io.File;
import java.io.IOException;
public class IOprogect  {
public static void main(String args[]) {
	File file = new File("D:"+File.separator+"Demo.txt");
	if(file.exists()) {
		System.out.println("文件已存在");
		try {
			Thread.sleep(10000);
		} catch (InterruptedException e) {
		e.printStackTrace();}
		file.delete();
		System.out.println("文件已删除");	}
	else {
		System.out.println("文件不存在");
		try {
			file.createNewFile();
		} catch (IOException e) {
						e.printStackTrace();		}
		System.out.println("创建新文件");
	}}}


```



以上程序会存在延迟，因为要调用系统来等待被分配资源；

**32.1.1.4.判断路径是文件还是文件夹**

isDirectory

public boolean isDirectory()

测试此抽象路径名表示的文件是否为目录。

如果需要将I / O异常与文件不是目录的情况区分开，或者同时需要同一文件的多个属性，则Files.readAttributes可以使用该方法。

isFile

public boolean isFile()

测试此抽象路径名表示的文件是否为普通文件。如果文件不是目录，并且满足其他与系统有关的条件，则该文件是正常的。由Java应用程序创建的任何非目录文件都保证是普通文件。

如果需要将

I / O

异常与文件不是普通文件的情况区分开，或者同时需要同一文件的多个属性，则

Files.readAttributes

可以使用该方法。

```java
import java.io.*;
public class IsFile {
	public static void main(String args[]) {
		File file0 = new File("E:" + File.separator + "Demo.txt");
		File file1 = new File("E:" + File.separator + "Docs");
		File file2 = new File("E:" + File.separator + "Demo.java");
		try {			file0.createNewFile();			file1.createNewFile();
			file2.createNewFile();
		} catch (IOException e) {	e.printStackTrace();		}
		System.out.println("Demo.txt是个目录吗---》？" + file0.isDirectory());
		System.out.println("Doc是一个文件吗？---》" + file0.isFile());
		System.out.println("Demo.java是一个文件吗？---》" + file0.isFile());
		try {			Thread.sleep(1000);
		} catch (InterruptedException e) {		e.printStackTrace();	}
//		file0.delete();		file1.delete();		file2.delete();
	}}

```

**32.1.1.5.列出目录中的内容**

在程序中可以使用如下方法进行目录的列表操作

list

public String[] list()

返回一个字符串数组，用于命名此抽象路径名表示的目录中的文件和目录。

不能保证结果数组中的名称字符串会以任何特定顺序出现；尤其不能保证它们按字母顺序出现。

请注意，Files该类定义了newDirectoryStream打开目录并遍历目录中文件名的方法。在使用非常大的目录时，这可能会使用较少的资源，而在使用远程目录时，它可能会响应更快。

listFiles

public File[] listFiles()

返回一个抽象路径名数组，该数组表示此抽象路径名表示的目录中的文件。

不能保证结果数组中的名称字符串会以任何特定顺序出现；尤其不能保证它们按字母顺序出现。

请注意，Files该类定义了newDirectoryStream打开目录并遍历目录中文件名的方法。当使用非常大的目录时，这可能会使用较少的资源。

 

```java
import java.io.*;
public class ListDemo {
	public static void main(String[] args) {
		File file = new File("E:" + File.separator + "org");
		String path[] = file.list();
		for (String string : path) {
			System.out.println(string);}	}}

```

此时用list()列出的是目录下的文件和文件夹名称；

用listFiles()----

```java
import java.io.*;
public class ListDemo {
	public static void main(String[] args) {
		File file = new File("E:" + File.separator + "org");
		String path[] = file.list();
		File path1[] = file.listFiles();
		for (File file2 : path1) {
			System.out.println(file2);		}	}}

```

以上两个列出的都是文件目录，但是用listFiles()可以文件夹目录形式表示；

如果想要操作文件，使用后者更为方便，因为可以找到File类的对象，实际上就找到了完整的路径；

**32.1.1.6创建目录**

使用mkdir()进行创建--

mkdir

```java
import java.io.*;
public class ListDemo {
	public static void main(String[] args) {
	File file = new File("E:" + File.separator + "orgorg");
		file.mkdir();
	}
}

```

在windows中有很多文件有后缀，例如jpg，doc等，实际上这些文件的后缀与文件本身并没有任何实际联系，加入后缀只是为了方便程序的管理；

 

```java
import java.io.*;
public class ListDemo {
	public static void main(String[] args) {
		File file = new File("E:" + File.separator + 
				"orgorg"+File.separator+"Demo"+File.separator+"Java.java");
		file.getParentFile().mkdir();//创建E盘下的父类文件orgorg
		try {
			file.createNewFile();//创建文件
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}	}}


```

在创建目录的时候此目录的所有文件路径包括各子文件夹的中的文件也都要列出

也可以创建文件，在创建文件之前要先找到父类目录；

```java
import java.io.*;
public class ListDemo {
	public static void main(String[] args) {
		File file = new File("C:" + File.separator+"Program Files");
		lists(file);
	}
	public static void lists(File file) {
		if (file.isDirectory()) {
			File files[] = file.listFiles();
			if (files != null) {
				for (int i = 0; i < files.length; i++) {
					lists(files[i]);		}			}
			System.out.println(file);
		}	}}


```

**32.1.2.RandomAccessFile类**

```java
Class RandomAccessFile
java.lang.Object
java.io.RandomAccessFile
All Implemented Interfaces:
Closeable, DataInput, DataOutput, AutoCloseable
public class RandomAccessFile
extends Object
implements DataOutput, DataInput, Closeable
```

完成随机的读写操作

> 此类的实例支持读取和写入随机访问文件。随机存取文件的行为类似于存储在文件系统中的大型字节数组。有一种游标，或索引到隐含数组中，称为文件指针;输入操作从文件指针开始读取字节，并将文件指针推进到读取的字节数之后。如果在读/写模式下创建随机存取文件，则输出操作也可用;输出操作从文件指针开始写入字节，并将文件指针推进到写入的字节之后。写入隐含数组当前末尾的输出操作会导致数组扩展。文件指针可以由方法读取，也可以由方法设置。getFilePointerseek

> 对于此类中的所有读取例程，通常都是如此，如果在读取所需的字节数之前到达文件末尾，则会抛出（这是一种）。如果由于文件结尾以外的任何原因无法读取任何字节，则会抛出除其他字节。特别是，如果流已关闭，则可能会抛出。EOFExceptionIOExceptionIOExceptionEOFExceptionIOException

如果要想实现随机读取则在存储数据的时候要保证数据长度的一致，否则无法实现功能。 

![](..\pic\图片89.png)

```java
//构造
public RandomAccessFile(File file,String mode) throws FileNotFoundException

public RandomAccessFile(String name,String mode) throws FileNotFoundException

```

需要接收File类的实例，并指定操作模式:
 读模式---r

写模式-w
 读写模式-rw

最重要的是读写模式，如果操作的文件不存在，则会帮用户自动创建;

**32.1.2.1.用RandomAccessFile类的写入操作**

使用从DataOutput接口中实现的一系列的writeXxx()方法写入数据

```java
import java.io.*;
public class RandomAccessFileDemo {
	public static void main(String args[]) throws Exception {
		File file = new File("F:" + File.separator + "Demo.doc");// 指定操作文件
		RandomAccessFile raf = new RandomAccessFile(file, "rw");// 以读写的形式操作
		System.out.println("--写入第一条数据--");
		String name = "zhangsan";
		int age = 20;
		raf.writeBytes(name);// 以字节形式将字符串写入
		raf.writeInt(age);// 写入整形数据
		System.out.println("--写入二条数据--");
		String name1 = "lisi    ";
		int age1 = 22;
		raf.writeBytes(name1);// 以字节形式将字符串写入
		raf.writeInt(age1);// 写入整形数据
		System.out.println("--写入三条数据--");
		String name2 = "wangwu  ";
		int age2 = 25;
		raf.writeBytes(name2);// 以字节形式将字符串写入
		raf.writeInt(age2);// 写入整形数据
		raf.close();//文件操作的最后一定要关闭，不关闭的话是无法释放资源的
	}
}

```

以上代码运行结果我们可以发现运行后用系统内置文件打开，读取的并不完全是我们写入的数据，因此要想得到我们写入的完整数据，那么我们就要用RandomAccessFile类来读取；

**32.1.2.2.用RandomAccessFile类的读取操作**

从DataInput接口中实现的一系列的readXxx()方法读取数据--

在RandomAccessFile类中因为可以实现随机的读取，所以有一系列的控制方法（控制文件读取点）

Seek---回到读取点

public void seek(long pos)   throws IOException

skipBytes--跳过多少个字节

public int skipBytes(int n)   throws IOException

下面进行读取操作--读取的时候不能读取字符串，所以要用字节的方式读取-- 

```java
import java.io.*;
public class RandomAccessFileDemo {
	public static void main(String args[]) throws Exception {
		File file = new File("F:" + File.separator + "Demo.doc");// 指定操作文件
		RandomAccessFile raf = new RandomAccessFile(file, "r");// 以读写的形式操作
	byte b[]= new byte[8];//定义字节数组
	String name =null;
	int age =0;	raf.skipBytes(12);//跨过12个字节，即读取第二个人的信息
	System.out.println("第二个人的信息--");
	for(int i =0;i<8;i++) {//由于不能直接读取字符串，所以只能将字符串以字符得形式取出
		b[i]=raf.readByte();}   age=raf.readInt();//读取字节//读取数字
		System.out.println("\t|-姓名:"+new String(b) );
	System.out.println("\t|-年龄:"+age);	raf.seek(0);//回到起始位置
	System.out.println("第一个人的信息--");
	for(int i =0;i<8;i++) {
		b[i]=raf.readByte();}//读取字节
		age=raf.readInt();//读取数字
	System.out.println("\t|-姓名:"+new String(b) );
	System.out.println("\t|-年龄:"+age);raf.close();	}}

```

下面进行读取操作--读取的时候不能读取字符串，所以要用字节的方式读取--

 

```java
import java.io.*;
public class RandomAccessFileDemo {
	public static void main(String args[]) throws Exception {
		File file = new File("F:" + File.separator + "Demo.doc");// 指定操作文件
		RandomAccessFile raf = new RandomAccessFile(file, "r");// 以读写的形式操作
	byte b[]= new byte[8];//定义字节数组
	String name =null;
	int age =0;	raf.skipBytes(12);//跨过12个字节，即读取第二个人的信息
	System.out.println("第二个人的信息--");
	for(int i =0;i<8;i++) {//由于不能直接读取字符串，所以只能将字符串以字符得形式取出
		b[i]=raf.readByte();}   age=raf.readInt();//读取字节//读取数字
		System.out.println("\t|-姓名:"+new String(b) );
	System.out.println("\t|-年龄:"+age);	raf.seek(0);//回到起始位置
	System.out.println("第一个人的信息--");
	for(int i =0;i<8;i++) {
		b[i]=raf.readByte();}//读取字节
		age=raf.readInt();//读取数字
	System.out.println("\t|-姓名:"+new String(b) );
	System.out.println("\t|-年龄:"+age);raf.close();	}}

```

在文件中提供了一个指针来完成随机读取的操作；

**32.1.3.字节流和字符流**

RandomAccessFile可以完成读写操作，但是操作起来毕竟很麻烦，所以在Java中要想进行IO操作，一般使用字符流和字节流进行操作；

在整个IO包中，流的操作分为两种

字节流：

1，字节输出流OutputStream,字节输入流InputStream

2，字符流：一个字符等于两个字节

3，字符输出流 Writer,字符输入流Reader

**32.1.3.1.1.IO的操作步骤**

在Java中使用IO操作的步骤

1，使用File找到一个文件、

2，使用字节流或者字符流的子类为OutputStream、InputStream、Writer或者Reader进行实例化操作

3，使用读或者写的操作

4，关闭；close()，在流的操作中最终必须进行关闭

**32.1.3.1.1字节流--输出流OutputStream**

在java中OutputStream为字节流--输出流的最大父类

public abstract class OutputStream extends Object implements Closeable, Flushable

此类为抽象类，所以使用时需要用子类进行实例化操作--子类有 如下

Direct Known Subclasses:

ByteArrayOutputStream, FileOutputStream, FilterOutputStream, ObjectOutputStream, PipedOutputStream

如果此时要完成文件的输出操作，则使用FileOutputStream为OutputStream进行实例化操作

构造方法--

public FileOutputStream(File file) throws FileNotFoundException

OutputStream提供了以下的写入数据方法-

1，写入全部字节数组--  public void write(byte[] b) throws IOException

2，写入部分字节数组--public void write(byte[] b,int off,int len)  throws IOException

3，写入一个数据--public abstract void write(int b)  throws IOException

```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;
public class FileOutputStreamDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		OutputStream out = null;// 声明字节输出流
		out = new FileOutputStream(file);// 通过子类实例化
		String str = "Hello World!!!";// 要写入的数据
		byte b[] = str.getBytes();// 将字符串变为byte数组
		out.write(b);// 写入数据
		out.close();	}}

```

```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;
public class FileOutputStreamDemo {
	public static void main(String[] args) throws Exception {
			File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		OutputStream out = null;// 声明字节输出流
		out = new FileOutputStream(file);// 通过子类实例化
		String str = "Hello World!!!";// 要写入的数据
		byte b[] = str.getBytes();// 将字符串变为byte数组
		for(int i=0;i<b.length;i++) {
			out.write(b[i]);// 写入数据
		}
		out.close();	}}

```

但是以上在执行的时候会被新的内容替换，而不是继续在原来的基础上继续写，如果要追加内容，则需要观察FileOutputStream的构造方法； ![](..\pic\图片90.png)

FileOutputStream

public FileOutputStream(File file,boolean append) throws FileNotFoundException

如果将append的内容设置为true，则表示追加内容

```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;
public class FileOutputStreamDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		OutputStream out = null;// 声明字节输出流
		out = new FileOutputStream(file,true);// 通过子类实例化
		String str = "追加的内容!!!";// 要写入的数据
		byte b[] = str.getBytes();// 将字符串变为byte数组
		for(int i=0;i<b.length;i++) {
			out.write(b[i]);// 写入数据
		}
		out.close();	}}

```

  

\r\n为换行操作

**32.1.3.1.2字节流--输入流InputStream**

使用InputStream可以读取输入流的内容；

此类的定义--

public abstract class InputStream extends Object implements Closeable

此类也是一个抽象类，如果要想使用此类，则也必须通过子类将其实例化，如果现在是文件操作，则使用的是FileInputStream

子类如下：

Direct Known Subclasses:

AudioInputStream, ByteArrayInputStream, FileInputStream, FilterInputStream, ObjectInputStream, PipedInputStream, SequenceInputStream, StringBufferInputStream

FileInputStream的定义-----

public class FileInputStream extends InputStream

FileInputStream----构造方法定义

public FileInputStream(File file)     throws FileNotFoundException

那么实例化之后就可以通过如下方法读取数据--

1，将内容读入到数组字节中---public int read(byte[] b)         throws IOException

2，每次读一个数据--public int read(byte[] b,int off,int len)         throws IOException

范例，将文件的内容进行读取--- 

```java
import java.io.*;
public class FileIntputStreamDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		InputStream input = null;// 声明字节读取流
		input =new FileInputStream(file);//通过子类实例化
		byte b[]=new byte[1024];//开辟空间接收读取的内容
		input.read(b);//将内容读入到byte数组中
		System.out.println(new String (b));
		input.close();	}}

```

以上代码运行后，发现有很多空格，是因为一开始指定的byte数组太大，那则么解决呢？

指定读取长度最大为数据的长度

```java
import java.io.*;
public class FileIntputStreamDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		InputStream input = null;// 声明字节读取流
		input =new FileInputStream(file);//通过子类实例化
		byte b[]=new byte[1024];//开辟空间接收读取的内容
		int len =input.read(b);//将内容读入到byte数组中
		System.out.println(new String (b,0,len));
		input.close();
	}
}

```

这是一种常见的的读取形式，但是以上代码有一个缺点，会受到开辟空间的限制，如果现在想要动态开辟数组空间，可以根据文件的大小来决定，采用read()方式； 

```java
import java.io.*;
public class FileIntputStreamDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		InputStream input = null;// 声明字节读取流
		input =new FileInputStream(file);//通过子类实例化
		byte b[]=new byte[(int)file.length()];//开辟空间接收读取的内容
		for(int i=0;i<b.length;i++) {
			b[i]=(byte)input.read();		}
		System.out.println(new String (b));
		input.close();	}}


```

**32.1.3.1.3字符流--输出流--Writer**

Writer类是在IO包中操作字符的最大父类，主要功能是完成字符流的输出

定义--

public abstract class Writer extends Object implements Appendable, Closeable, Flushable

Direct Known Subclasses:

BufferedWriter, CharArrayWriter, FilterWriter, OutputStreamWriter, PipedWriter, PrintWriter, StringWriter

与OutputStream一样，都属于抽象类，如果要进行文件中的保存，则使用FileWriter。

常用写入操作方法---public void write(String str)      throws IOException

通过子类实例化--public class FileWriter extends OutputStreamWriter

```java
import java.io.*;
public class FileWriterDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		Writer write = null;// 生名字符流
		write = new FileWriter(file,true);// 通过子类实例化
		write.write("\r\n字符流");
		write.close();	}}

```

FileWriter常用构造方法--

写入内容--public FileWriter(File file)           throws IOException

可以追加内容--public FileWriter(File file,boolean append)           throws IOException

**32.1.3.1.4字符流--输入流--Reader**

public abstract class Reader extends Object  implements Readable, Closeable

public class FileReader  extends InputStreamReader

是一个抽象类，要是现在进行文件的读取，则通过FileReader

构造方法--

```java
public FileReader(File file)   throws FileNotFoundException

public FileReader(String fileName)           throws FileNotFoundException

public FileReader(String fileName,Charset charset)           throws IOException

```

读取方法--

读取一组字符：--public int read(char[] cbuf)         throws IOException



```java
import java.io.*;
public class FileReaderDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "writer.doc");// 要操做的文件
		Reader read0 = new FileReader(file);//实例化对象
		char[] sha = new char[100];//开辟空间接收数据
		int len = read0.read(sha);//返回读取到的长度
		System.out.println(new String(sha, 0, len));//转换成字符串输出
		read0.close();	}}

```

读取一个个字符：--public int read()         throws IOException 

```java
import java.io.*;
public class Liu {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "writer.doc");
		Reader re = new FileReader(file);//shilihua
		char []ch =new char[100];//kaipikongjian cunchu
			for(int i =0;i<(int)file.length();i++) {
			ch[i]=(char)re.read();
			System.out.print(ch[i]);		}
		re.close();	}}


```

**32.1.3.1.5字符流--字节流的区别**

以上操作代码有两组，那么实际上应该使用哪一组更好呢？

范例--程序向文件中保存内容

使用OutPutStream完成



```
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;
public class FileOutputStreamDemo {
	public static void main(String[] args) throws Exception {
			File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		OutputStream out = null;// 声明字节输出流
		out = new FileOutputStream(file);// 通过子类实例化
		String str = "Hello World!!!";// 要写入的数据
		byte b[] = str.getBytes();// 将字符串变为byte数组
		for(int i=0;i<b.length;i++) {
			out.write(b[i]);// 写入数据
		}			}}


```

以上程序在操作的时候并没有关闭操作，发现内容可以正常输出，那么字符流呢？ 

```java
import java.io.*;
public class FileWriterDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		Writer write = null;// 生名字符流
		write = new FileWriter(file,true);// 通过子类实例化
		write.write("\r\n字符流");
			}}


```

以上字符流并没有关闭，现在执行后，并不存在内容，意味着并没有输出，

但是现在使用Writer中的flush()方法--刷新流

```java
import java.io.*;
public class FileWriterDemo {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Demo.txt");// 要操做的文件
		Writer write = null;// 生名字符流
		write = new FileWriter(file);// 通过子类实例化
		write.write("\r\n字符流");
		//write.close();
		write.flush();//刷新流
				}}

```

实际最早的操作中并没有刷新流，但是因为使用了关闭，所以会强制刷新，刷新的是缓冲区（内存）

所以得出以下结论----

字节流在操作做中直接与文件本身关联，不使用缓冲区

-----字节直接操作文件

字符流在操作的时候是通过缓冲区与文件操作

--------字符通过缓存操作文件

综合来讲，在传输或者在硬盘上保存的内容都是以字节的形式存在的，所以字节流的操作较多，但是在操作中文的时候使用字符流较方便；

总结--

```
1，File类主要作用是完成文件的操作，与文件本身有关；
```

```
2，RandomAccessFile类完成的是随机的读取操作，相当于在文件中设置了一个指针，通过移动指针来完成对数据的随机读取操作；
```

```
        实现了DateInput、DateOutput类
```

```
3，字节流和字符流：
```

```
---字节流：InputStream、OutputStream
```

```
---字符流：Reader、Writer
```

```
以上四个类都是抽象类，抽象类的特点就是根据实例化他的子类来完成不同的功能；如果是对文件的操作，则使用FileXxx；
```

```
字符路在操作时使用了缓存，字节流则是直接实现底层的操作；
```

4，在java中所有的Io操作都是在IO包中

5，File类表示与平台无关的文件操作，是负责文件的本身而不负责文件内容

6，OutputStream和InputStream是字节的输出输入流（最大的父类），通过FileXx实例化

7，Writer与Reader是字符的输出和输入流，通过FileXx实例化。

8，字节流是操作文件本身，字符流是需要通过内存操作来完成对文件的操作；

​        程序范例--使用IO操作复制文件

代码开发是使用JAVA的初始化参数完成，例如定义Copy类，执行

java  Copy 源文件  目标文件

实习那个文件复制有两种方式--

1，读完后直接写入，但是如果文件过大，则不太合适

2，边读边写，使用此种方式最合适（实际在操作系统中拷贝文件也是边读边写的）

```java
import java.io.*;
public class CopyDemo {
	public static void main(String[] args) throws Exception {
		if (args.length != 2) {// 如果参数不是两个
			System.out.println("操作有误，请重新输入要拷贝的文件路径");
			System.out.println("参考写法--java Copy 源文件路径 目标文件路径");
			System.exit(1);// 系统退出
		}
		if (args[0].equals(args[1])) {
			System.out.println("无法复制自身文件");
			System.exit(1);		}
		File file1 = new File(args[0]);
		if (file1.exists()) {
			File file2 = new File(args[1]);// 找到目标文件路径
			InputStream input = new FileInputStream(file1);
			OutputStream output = new FileOutputStream(file2);
			int temp = 0;// 定义一个整数来表示接受的内容
			while ((temp = input.read() )!= -1) {// 表示还有内容可以继续读
				output.write(temp);// 写入数据
			}
			System.out.println("文件复制成功");
			input.close();
			output.close();
		} else {
			System.out.println("源文件不存在");		}}}

```

内存操作流、管道流、字节字符转换流、字符编码问题、打印流、对象序列化、System类对IO的支持

**32.1.4   内存操作流**

FileInputStream和FileOutputStream所操作的是目标文件，那么如果现在要将一些临时信息要求通过IO流保存在文件之中，则肯定不太合理，因为操作之后还要把文件删除，所以此时IO中提供了内存操作流，通过内存操作流和输出流来输出目标内存；

使用ByteArrayOutputStream和ByteArrayInputStream完成内存的操作流

​     在内存操作流中所有的输入和输出都是以内存作为操作的源头

​     ByteArrayOutputStream用于从内存向程序输出的

​     ByteArrayInputStream用于从程序向内存写入

 ByteArrayInputStream---

public class ByteArrayInputStream extends InputStream写入

构造方法--public ByteArrayInputStream(byte[] buf)--表示把内容向内存之中写入

ByteArrayOutputStream---

public class ByteArrayOutputStream extends OutputStream

构造方法--public ByteArrayOutputStream()，其基本作用就是和OutPutStream一样，一个个读取数据

：使用内存操作流iwanchengyige字符串小写字母变为大写字母的操作； 

```java
import java.io.*;
public class ByteArrays {
	public static void main(String[] args) {
		String str = "helloworld";// 定义字符串全部由小写字母组成
		ByteArrayInputStream bai = null;// 内存输出流
		ByteArrayOutputStream bao = null;// 内存输入流
		bai = new ByteArrayInputStream(str.getBytes());// 将内容保存在内存之中
		bao = new ByteArrayOutputStream();// 内存输出流
		int temp = 0;		while ((temp = bai.read()) != -1) {// 依次读取
			char c = (char) temp;// 接收字符
			bao.write(Character.toUpperCase(c));}		String newstr = bao.toString();
		System.out.println(newstr);	}}

```

内存操作流在JavaS阶段是感觉不出有什么作用，但是字啊学习JAVA web中的AJAX技术的时候，会结合XML解析和javaScript，AJX完成一些动态效果的；

String newstr = bao.toString();//内存的取出

**31.15.管道流**

管道流就是进行两个线程之间的通讯，使用 PipedInputStream 和 PipedOutputStream两个类完成；

这两个类使用时基本跟InputStream和OutputStream类似，唯一的区别就在于连接管道的方式上

两个类的定义--

```java
public class PipedInputStream  extends InputStream

public class PipedOutputStream  extends OutputStream

```

构造方法--

```java
public PipedOutputStream()--

public PipedOutputStream(PipedInputStream snk)   throws IOException 

```

![](..\pic\图片91.png)

通过以下方法来完成管道的连接--

public void connect(PipedInputStream snk)        throws IOException

大体思路是--建立两个线程（继承Thread或者实现Runnable）

通过管道，一个发送信息，一个接收信息，如果要互相传递信息，则在各自的类里要相互建立对方的子列实例；

```java
import java.io.*;
class send implements Runnable {// 发送数据的线程
	private PipedOutputStream pos = null;
	public send() {
		this.pos = new PipedOutputStream();	}
	public PipedOutputStream getPipedOutputStream() {
		return this.pos;	}
	@Override
	public void run() {
		String str = "helloworld";// 要发送的数据
		try {			this.pos.write(str.getBytes());// 发送
		} catch (IOException e) {				e.printStackTrace();		}
		try {			this.pos.close();// 关闭流
		} catch (IOException e) {			e.printStackTrace();	}	}}
class receive implements Runnable {// 接收数据的线程
	private PipedInputStream pis = null;
	public receive() {
		this.pis = new PipedInputStream();	}
	public PipedInputStream getPipedInputStream() {
		return this.pis;	}
	@Override
	public void run() {
			byte b[] = new byte[1024];		int len = 0;
		try {			len=this.pis.read(b);// 内容读取
		} catch (IOException e) {	e.printStackTrace();		}
		try {			this.pis.close();		} catch (IOException e) {
						e.printStackTrace();		}	
	System.out.println(new String(b, 0, len));	}}

```

```java
public class ThreadConnectDemo {
	public static void main(String[] args) {
		send se= new send();
		receive re= new receive();
		try {
	se.getPipedOutputStream().connect(re.getPipedInputStream());
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();		}
		new Thread(se).start();//启动线程
		new Thread(re).start();//启动线程
	}}


```

使用OutPutStream可以完成数据的输出，但是现在如果有一个float型数据要输出，怎么办？
虽然现在提供了输出流的操作类，但是这个类本身的输出支持并不十分强大，所以如果现在要输出浮点型数据，则需要用打印流
打印流分为两种；--PrintStream   ----PrintWriter
定义---

```java
public class PrintStream extends FilterOutputStream implements Appendable, Closeable

public class PrintWriter extends Writer

```


 PrintStream是FilterOutputStream的子类，继续观察其构造方法
构造方法--
public PrintStream(OutputStream out)
再次方法中要接受OutputStream 的子类引用，
实际上PrintStream属于装饰，根据实例化，PrintStream类对象不同，输出的位置不同，
范例--假设使用PrintStream向文件输出



```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.PrintStream;
public class PrintStreamed {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "DDemo.txt");
		PrintStream out = new PrintStream(new FileOutputStream(file));
		out.print("Hello");
		out.println("World");
		out.print(10);		
out.print(22.56);
		out.close();	}}

```

所以使用打印流输出更为方便；

在JDK1.5之后进行了更新可以使用格式化输出--

提供了以下两种方法，可以设置格式和多个参数

```java
public PrintStream printf(String format,Object... args)

public PrintStream printf(Locale l,String format,Object... args)

```

```java
import java.io.*;
public class PrintStreamed {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "DDemo.txt");
		PrintStream out = new PrintStream(new FileOutputStream(file));
		String name ="盎然";		int age  =25;		float score =99.5f;		char sex='N';
		out.printf("姓名:%s;年龄:%d;成绩:%5.2f;性别:%c ",name,age,score,sex);
		out.close();	}}


```

在打印流中，根据其实例化子类的不同来实现不同的功能；

**31.16.字符编码**

在程序中如果字符编码没有处理完整，则肯定会造成乱码，常见的编码格式有以下几种：--

UTF-8--包含了以下代码--

IOS 8859-1包含全部的英文编码

GBK/GBK2312-表示中文，GBK表示简体中文和繁体中文，GBK2312指标是简体中文

如果程序中操作的编码与本地操作的编码不统一，那么操作的时候就有可能出现乱码--

​             

所以在操作之前我们要得到系统的编码格式再做处理可以作如下处理

```java
public class Zifuliu {
	public static void main(String[] args) {
		// System.out.println(System.getProperties().toString()+"\n\t");
		System.getProperties().list(System.out);	}}//列出系统信息

```

所以，在操作的时候默认的编码就是系统用的编码，如果要在程序中使用的时候，那么编码也要是系统用的编码，例如，当系统用的GBK2312,那么当我们用GBK编码程序的时候在这样的系统上运行就有可能出现乱码，例如-- 

```java
import java.io.*;
public class Zifuliu {
	public static void main(String[] args) throws Exception {
		File file = new File("E:" + File.separator + "Hello.txt");
	OutputStream out = new FileOutputStream(file);		String name = "中国你好";
		out.write(name.getBytes("ISO8859-1"));// 在转字节的时候指定编码格式
		out.close();	}}

```

编码不统一，那么在解析的时候就会出现乱码，所以乱码造成的根本原因就在于程序与本机环境的编码不统一；

**31.17新的类--Scanner**

Scanner是java.util包中提供的一个类，用此类可以方便的完成输入流的输入操作

public final class Scanner extends Object implements Iterator<String>, Closeable

通过查看构造方法，我们可以发现Scanner可以接受很多流的输入

![](..\pic\图片92.png)

```java
import java.util.*;
public class Zifuliu {
	public static void main(String[] args) {
		System.out.println("请输入一个数--");
		Scanner sc = new Scanner(System.in);
		int i = sc.nextInt();
		sc.close();
		System.out.println(i);	}}

```

```java
import java.util.*;
public class Zifuliu {
	public static void main(String[] args)throws Exception {
		 String input = "1 fish 20 fish red fish blue fish";
	     Scanner s = new Scanner(input).useDelimiter("\\s*fish\\s*");
	     System.out.println(s.nextInt());
	     System.out.println(s.nextInt());
	     System.out.println(s.next());
	     System.out.println(s.next());
	     s.close();	}}

```

![](..\pic\图片93.png)

```java
import java.util.*;
public class Zifuliu {
	public static void main(String[] args)  {
		Scanner scan = new Scanner(System.in);
		int i =0;
		if (scan.hasNextFloat()) {
			i=scan.nextInt();
			System.out.println("i="+i);		}
		scan.close();	}}//当输入的不符合要求时，不会接收所输入的内容

```

例如，有一个接收字符串的操作 

```java
import java.util.*;
public class Zifuliu {
	public static void main(String[] args) throws Exception {
		Scanner scan = new Scanner(System.in);		String str =null;
		if (scan.hasNext()) {
			str=scan.next();}
			System.out.println("str="+str);		
		scan.close();	}}

```

 

使用此类也可以使用正则规则匹配--

```java
import java.util.*;
public class Zifuliu {
	public static void main(String[] args) throws Exception {
		Scanner scan = new Scanner(System.in);
		String str = null;
		if (scan.hasNext("\\d(4)-\\d(2)-\\d(2)")) {
			str = scan.next();	}	System.out.println("str=" + str);		scan.close();	}}


```

由于Scanner可以接收流的操作，所以Scanner也可以读取文件内容； 

```java
import java.util.*;import java.io.*;
public class Zifuliu {
	public static void main(String[] args) throws Exception {
	File file = new File("E:"+File.separator+"Hello.txt");
		Scanner scan = new Scanner(new FileInputStream(file));
		StringBuffer str = new StringBuffer();
		while (scan.hasNext()) {			str.append(scan.next()+"\n");		}
		System.out.println( str.toString());		scan.close();	}}

```

以上代码运行后，不太符合要求，因为程序把缓缓那个也当作分隔符了，所以使用Scanner来去读文件内容的时候，要注意！！分隔符的处理。默认是按照回车和空行进行分割的;

```java
import java.util.*;import java.io.*;
public class Zifuliu {
	public static void main(String[] args) throws Exception {
	File file = new File("E:"+File.separator+"Hello.txt");
		Scanner scan = new Scanner(new FileInputStream(file));
		StringBuffer str = new StringBuffer();
		scan.useDelimiter("\n");//以用户的\n作为分隔符
		while (scan.hasNext()) {			
            str.append(scan.next()+"\n");		}
		System.out.println( str.toString());	
        scan.close();	}}

```

```java
package HuiXin;
import java.io.File;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file= new File("D:"+File.separator+"huixi"+File.separator+"LivelyWallPaper");
        File file1= new File("D:/HuiXin.doc");

        if (!file1.exists()){
            file1.createNewFile();
        }
        System.out.println("文件是否可执行--"+file.canExecute());
        System.out.println("文件绝对路径--"+file.getAbsolutePath());
        System.out.println("文件相对路径--"+file.getPath());

        System.out.println("文件是否可读--"+file.canRead());
        System.out.println("文件是否可写--"+file.canWrite());
        System.out.println("文件地址比较--"+file.compareTo(file1));
        System.out.println("文件绝对文件--"+file.getAbsoluteFile());
        File file2= new File("D:/Study.doc");
        System.out.println("文件重命名--"+file1.renameTo(file2));//可以实现好几种功能
        System.out.println("文件名--"+file.getName());
        System.out.println("------------"+file.getCanonicalPath());
        System.out.println(file.getCanonicalFile());
        System.out.println("文件根目录--"+file.getParent()+"返回值为String");
        System.out.println("文件的上级目录--"+file.getParentFile()+"返回值为File类的实例");

        System.out.println("文件是否是一个目录--"+file.isDirectory());

        System.out.println("文件是否是一个文件--"+file.isFile());
        System.out.println("文件是否隐藏--"+file.isHidden());

        System.out.println(file.getTotalSpace());//如果是文件，则返回0
        File file3 = new File("C:");
        System.out.println("盘符剩余空间--"+file3.getFreeSpace());//抽象路径名表示的剩余空间大小
        System.out.println("盘符总空间--"+file3.getTotalSpace());//抽象路径名表示的分区的总大小,
        System.out.println("盘符未分配空间--"+file3.getUsableSpace());//抽象路径名表示的分区的未分配的总大小,

    }
}


```

小细点---

 ***1. 带扩展名与不带的区别，***
 但是最终都反应的是所在盘符的空间状态；//当文件或者路径不存在时返回0L

```java
File f= new File("D:\\Study.doc");
        File file3 = new File("C:");

        System.out.println("盘符剩余空间--"+file3.getFreeSpace());//抽象路径名表示的剩余空间大小
        System.out.println("盘符总空间--"+file3.getTotalSpace());//抽象路径名表示的分区的总大小,
        System.out.println("盘符未分配空间--"+file3.getUsableSpace());//抽象路径名表示的分区的未分配的总大小,
        System.out.println("盘符剩余空间--"+f.getFreeSpace());//抽象路径名表示的剩余空间大小
        System.out.println("盘符总空间--"+f.getTotalSpace());//抽象路径名表示的分区的总大小,
        System.out.println("盘符未分配空间--"+f.getUsableSpace());//抽象路径名表示的分区的未分配的总大小,

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/16a482dc42a74ca3841645490364e4c3.png)

细节区别---
写全扩展名与没写全的区别
文件没写全扩展名且目录下又没有该文件夹--

![](..\pic\mysql_pic\5.png)
如果写全了扩展名
结果如下----

![在这里插入图片描述](https://img-blog.csdnimg.cn/058ca983ac1642288257fe210ab7ef03.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

  ***2，获得绝对路径和相对路径的区别--***

```java
 File file4= new File("AQ.ppt");
        System.out.println("当前File对象的绝对路径名的字符串---"+file4.getAbsolutePath());
        System.out.println("当前File对象的绝对路径名File实例---"+file4.getAbsoluteFile());
        System.out.println("当前File对象的相对路径名的字符串---"+file4.getPath());
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/82646256ddaa4f3385d3d3b0a9008440.PNG#pic_center)

***3，getParent()与getParentFile()的区别***

返回值类型不同--但是输出值一样，因为输出是调用了toString（）
public String getParent() {//返回值类型为String

public File getParentFile() {//返回值类型为File

***4，getCanonicalPath()与getCanonicalFile()区别***

```java
 System.out.println("文件规范路径--"+file.getCanonicalPath());//String
        System.out.println("文件规范路径--"+file.getCanonicalFile());//File
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/db0a2fec86734201a35d04ca36474147.png)
区别也是返回值不一样；

***5，renameTo（）方法的应用***

需要两个File实例---

对于文件---扩展名一样（如果不带扩展名，jvm视为文件夹名）

```java
  File file1= new File("D:/HuiXin.doc");
File file2= new File("D:/Study.doc");
        System.out.println("文件重命名--"+file1.renameTo(file2));//可以实现好几种功能
```

如果文件路径一样----
![在这里插入图片描述](https://img-blog.csdnimg.cn/1048bad4d3af4dfb9e84edc515405cfb.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
对于文件夹--

```java
File file1= new File("D:/HuiXin");
        File file2= new File("D:/Study");
        System.out.println("文件重命名--"+file1.renameTo(file2));//可以实现好几种功能
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/4bd5b45890ea485a87cd883ca2c58ded.png)
如果文件路径不一样，可以实现文件的剪切或者剪切后的重命名

```java
        File file1= new File("D:/Study.doc");
        File file2= new File("D:/Study/DayDayUp.doc");
        System.out.println("文件重命名--"+file1.renameTo(file2));//可以实现好几种功能
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f6306fbdfeb94f2ba2492055c0cf6b0c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

***6，list()与listFiles()的区别***

先看代码--

```java
import java.io.File;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file1= new File("D:\\Program Files");
        String[] list = file1.list();
        for (String s : list) {
            System.out.println(s);
        }
        System.out.println("-------------分割线----------------");
        File[] files = file1.listFiles();
        for (File file : files) {
            System.out.println(file);
        }

    }
}


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a8add303a9dc467ca5f30f9d39db140d.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

public String[] list() {//返回String[]

public File[] listFiles() {//返回File[]
由方法返回值以及输出结果可以看出返回值不一样，结果--一个不带路径，一个带路径

list以及listFiles还有其他方法--带参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/9a69918e001b4dcc840e211c22de1687.png)
***再来看带参数的方法--***

***list(FilenameFilter fileter)***
@FunctionalInterface
public interface FilenameFilter {//一个功能接口

通过方法名--文件名过滤--实代码

```java
package HuiXin;

import java.io.File;
import java.io.FilenameFilter;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file1 = new File("C:\\Program Files\\XMind ZEN");
        String[] list = file1.list();
        for (String s : list) {
            System.out.println(s);
        }
        System.out.println("-------------过滤之后的----------------");

        FilenameFilter filenameFilter = new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                String filename = name.toLowerCase();
                if (filename.endsWith(".dll")) {
                    if (filename.startsWith("d")|filename.startsWith("f")) {
                        return true;
                    }
                }
                return false;
            }
                   };
        String[] list1 = file1.list(filenameFilter);
        for (String s : list1) {
            System.out.println(s);
        }
    }
}

```

过滤之后的结果如下--
![在这里插入图片描述](https://img-blog.csdnimg.cn/0dc71bc1d0ac4b9d98c89bf6b628c99f.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
***listFiles(FilenameFilter fileter)***
public File[] listFiles(FilenameFilter filter) {//返回值为File[]

```java
File[] files = file1.listFiles(filenameFilter);
        for (File file : files) {
            System.out.println(file);
        }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/5bd53b188c9840cd8833a00c50f20abf.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



***listFiles( FileFilter fileFilter)***

```java
package setTest;

import java.io.File;
import java.io.FileFilter;


public class Test {
    public static void main(String[] args)  {
        File file1 = new File("C:\\Program Files\\XMind ZEN");
        String[] list = file1.list();
        for (String s:list
             ) {
            System.out.println(s);
            if(s.equals("XMind ZEN.exe")){
                System.out.println("-------------过滤之后的----------------");
            }
        }
            FileFilter fileFilter = new FileFilter() {
                @Override
                public boolean accept(File pathname) {
                    boolean flag=pathname.getName().toLowerCase().endsWith("exe");
                    return flag ;
                }
            };

            File[] listFiles = file1.listFiles(fileFilter);
            for (File listFile : listFiles) {
                System.out.println(listFile);
            }

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f2cfdf35d6a3455aac93a08ff943a5e9.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
7,mkdir()与mkdirs()的区别

```java
File file1 = new File("C:\\D\\E\\F");
   boolean mkdir = file1.mkdir();
        System.out.println(mkdir);
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/cbd89ffc56074924bd6e975a208c0340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
  File file1 = new File("C:\\D");
        boolean mkdir = file1.mkdir();
        System.out.println(mkdir);
        File file = new File("C:\\DD\\EE\\FF");
        boolean mkdirs = file.mkdirs();
        System.out.println(mkdirs);
        
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e6336878478a48d293d7fff4b48ebb50.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

区别----
mkdir()是创建单目录的,mkdirs()是创建多级目录的;



看一下源码--返回值不一样
![在这里插入图片描述](https://img-blog.csdnimg.cn/017250ed504d4f9584aae49b619f07ea.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
接下来回顾一下dos中的命令mkdir

![在这里插入图片描述](https://img-blog.csdnimg.cn/8fb3a53bd1704419be03a2cd66cd73ae.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
删除--
![在这里插入图片描述](https://img-blog.csdnimg.cn/d9d27879dc4148ab804665e9b42b9c0e.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/b0f66d322655412faa54f2ffd2d520c6.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

**FlieReader和FileWriter**

![在这里插入图片描述](https://img-blog.csdnimg.cn/13485ecb07e54912b16a6e52e97c9ff5.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)很简单--输入输出流，输入输出的大小---字符和字节的区别。
下表内容看起来很多，但是基本操作是一样的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/27db6835e69a4b1f9496248c8640cb4c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)先一个一个的看--由于上一篇文章学习了File，接着学习FlieReader和FileWriter

***FlieReader***

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file= new File("E:\\Hello.txt");//找到文件位置
        FileReader fr= new FileReader(file);//创建字符输入流，将文件传进去开始读
        int a=fr.read();
        while(a!=-1){//文档结尾--   -1为结束标志
            System.out.print((char)a);
            a=fr.read();
        }
        fr.close();
    }
}

```

***FileWriter***

```java
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file= new File("E:\\Hello.txt");//找到文件位置
        FileReader fr= new FileReader(file);//创建字符输入流，将文件传进去开始读
        int a=fr.read();
        while(a!=-1){
            System.out.print((char)a);
            a=fr.read();
        }
        fr.close();

File file1= new File("E:\\Hello.txt");//找到文件位置
        FileWriter fw= new FileWriter(file1);//创建字符输出流，写入
        fw.write("我爱你中国");
        fw.close();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8e53dd3dad094d859d35dcaead36e37f.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
***FileReader中常用方法***
read()方法---有四种，根据参数来一个个区分
![在这里插入图片描述](https://img-blog.csdnimg.cn/63c0eb4db5c44c119e3bb5590d1734ce.png)
***read（）--读取首个字符（其实也类似于有一个光标）***

```java
  File f= new File("E:\\Hello.txt");//定位文件
        FileReader fr= new FileReader(f);//读取文件
        int read = fr.read();
        int read1 = fr.read();
        int read2 = fr.read();
        int read3 = fr.read();
        int read4 = fr.read();
        int read5 = fr.read();
        System.out.println(read);
        System.out.println(read1);
        System.out.println(read2);
        System.out.println(read3);
        System.out.println(read4);
        System.out.println(read5);
        fr.close();//关闭流
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/59eba53e744c4a29917527ba6f13dc00.png)

可以发现read是一个一个读取的，要想全部读取完毕，就要用到循环，同时也发现结尾是以 -1 而作为标记的，所以---

```java
 while(read!=-1){
           System.out.print((char)read);
           read=fr.read();
       }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a845e4e2c119409fa5b23a898e088bdf.png)

***## read（char []ch）***

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
public class Test {
    public static void main(String[] args) throws IOException {
        File f = new File("E:\\Hello.txt");//定位文件
        FileReader fr = new FileReader(f);//读取文件
        char p[] = new char[5];
        int read = fr.read(p);//每次读取的有效长度
       /* while(read!=-1){
          for(int i=0;i<read;i++)  {
              System.out.print(p[i]);
          }
            read = fr.read(p);
       }*/
        while (read != -1) {
            for (int i = 0; i < p.length; i++) {
                System.out.print(p[i]);
            }
            read = fr.read(p);
        }
        fr.close();
    }
}
```

以上这两种方式有什么区别呢？

![在这里插入图片描述](https://img-blog.csdnimg.cn/ac39dd241be741ac82c21d2ac440ebb9.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
read中传入char[]数组是，是以该数组长度为单位进行读取，
read是取实际长度，如果是i<p.length,那么始终是以五个为一组进行读取，如果读到最后不够五个字符串，那么就在指定位置上替换上一次的读到的数据；所以实际中是以i<read来进行操作的；

***## read（CharBuffer []ch）***
字节缓冲

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.nio.CharBuffer;

public class Test {
    public static void main(String[] args) throws IOException {
        File f = new File("E:\\Hello.txt");//定位文件
        FileReader fr = new FileReader(f);//读取文件
        char[]ch=new char[5];
        int read = fr.read(CharBuffer.wrap(ch));//字节缓冲
        System.out.println(read);
             while(read!=-1){
                 for(int i=0;i<read;i++){
                     System.out.print(ch[i]);
                 }
                 read= fr.read(CharBuffer.wrap(ch));
             }
        fr.close();
    }
}
```

CharBuffer 是将字符读入指定的字符缓冲区，wrap()方法用于创建缓冲区
![在这里插入图片描述](https://img-blog.csdnimg.cn/bb16a5871efe4bab99c19226931e34c9.png)
wrap()方法实际也是调用了wrap的如下方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/be5f888a87184834a9d0507e65a51b14.png)

```java
public static CharBuffer wrap(char[] array) {
        return wrap(array, 0, array.length);
    }
```

可以指定缓冲区的大小，也可以不指定，将所有字节数组的空间作为缓冲区

其他方法与对应的方法类似；

> mark()方法---  将尝试将流重新定位到这一点。 并不是所有的   字符输入流支持mark()操作。     @param
> read AheadLimit可能的字符数限制   阅读时仍保留标记。 后   读了这么多字，试图   重置流可能会失败。    
> 如果流不支持mark()，   或者发生其他I/O错误



```java
import java.io.*;
public class Test {
    public static void main(String[] args) throws IOException {
        File file= new File("D:\\Test\\Hello.txt");
        FileReader fileReader= new FileReader(file);
        long start=System.currentTimeMillis();
        int read=fileReader.read() ;
        while(read!=-1){
            System.out.print((char)read);
            read=fileReader.read();
        }
        fileReader.close();
        long end=System.currentTimeMillis();
        System.out.println(end-start);
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/ef44c95b598042e89b0a7df2d7dbc46b.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

这时候也可以通过字节(数组)缓冲来完成,设置合理的缓冲大小,可以加快读取速度,设置过大占用内存就比较大,

```java
import java.io.*;
public class Test {
    public static void main(String[] args) throws IOException {
       File file= new File("D:\\Test\\Hello.txt");
        FileReader fileReader= new FileReader(file);
        char []reads = new char[84];
         long start=System.currentTimeMillis();
        int read = fileReader.read(reads);
        for (int i = 0; i <reads.length ; i++) {
            System.out.print(reads[i]);
            read=fileReader.read(reads);
        }
        fileReader.close();
         long end=System.currentTimeMillis();
          System.out.println(end-start);
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6965cfb5b5564c90bd331a684f92e985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
当文件内容比较少时,两者读取速度差不多,但是当文件内容很多时,字符缓冲流的速度就要比单个字符读取要快;

例如,读取一个180多兆的文件-----差距就很明显;
![在这里插入图片描述](https://img-blog.csdnimg.cn/c29ad0a3f81843fe8fb2abbfebb5eb69.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



```java
public FileReader(File file) throws FileNotFoundException {

       super(new FileInputStream(file));

   }

```



FileReader(File file) 调用的是 FileInputStream(file)



**以上大多都是字节输入流,下面是对字节输出流的操作;**



```java
public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\Test\\Hello.txt");
        FileWriter fw = new FileWriter(file,false);
        fw.write(101);//写入整形--char=e
        fw.write("我爱你中国");//写入字符串
        String str = "亲爱的母亲";
        char[] chars = str.toCharArray();
        fw.write(chars);//写入字节数组
        fw.write(str, 1, 2);//写入str的部分内容
        fw.write(chars, 1, 3);//写入字节数组的部分内容
        fw.close();
        FileReader fr = new FileReader(file);
        int read = fr.read();
        while (read != -1) {
            System.out.print((char)read);
            read=fr.read();
        }
        fr.close();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/cc28a973815d4f17a52e4461b2ec8fd7.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
如果文件按不存在的话,会自己创建,    当我们再次添加内容的时候----

```java
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\Test\\Hello.txt");
        FileWriter fw = new FileWriter(file);
               fw.write("我为你流泪也为你自豪");//写入字符串
               fw.close();
       
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c2f222d2b4a8462294bfef9acec481ef.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

原来的内容被删除(覆盖)了,然或添加上新内容,原因竟然是----

![](..\pic\mysql_pic\6.png)

> ```java
> public FileWriter(File file, boolean append) throws IOException {
>         super(new FileOutputStream(file, append));
>     }
> ```
>
> 

**综合应用----用字符输入流和输出流完成文件的复制**



输入输出都有了,下面时完成文件的复制(还记得File文件的重命名操作吗?---完成文件(文件夹)的剪切操作)

一个一个字符写入,(速度慢)

```java
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\Test\\Hello.txt");//找到文件
        FileReader fd = new FileReader(file);//读取源文件
        FileWriter fw = new FileWriter("D:\\Test\\Hello.doc");//要写入的文件
        int read = fd.read();
        while (read != -1) {
            fw.write((char) read);
            read = fd.read();
        }
        fw.close();//关闭流--后用先关
        fd.close();
        
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/5b6a25cd9c924f788acebb338acb9f62.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
做如下改动即可,

```java
    char[]a= new char[10];
        int read = fd.read(a);
        while (read != -1) {
            fw.write(a,0,read);
            read = fd.read(a);
        }

```



当然还有其他方法,可以自己去逐渐实现;

小彩蛋---下篇文章预告---

```java
package HuiXin;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\慧鑫教育\\知识点测试卷1.txt");
        FileInputStream is = new FileInputStream(file);
        //byte v[] = new byte[3];
        int n = is.read();
        while (n != -1) {
            System.out.println(n);
            n = is.read();
        }
        is.close();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20b78e4b1074440897a495472b1978e0.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
将字节转换为字符还需转换流，如果不用转换流转化，而直接强转为字符   会出现英文正常，中文乱码；

![在这里插入图片描述](https://img-blog.csdnimg.cn/c090402ec0f943948c683fb5a9eecd74.png)





**write()与append()的区别**

![在这里插入图片描述](https://img-blog.csdnimg.cn/460516cb03b048d9a8fa7829b95b2527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/fea069df43fa4cd6857d0873bb957dd4.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
第一眼区别-------

返回值不同;

实现的功能上-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/bce43eb2938c47ccb890cb79d29652f9.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
write()如果构造上没有明确append参数,则默认为false即不再结尾追加,而是(删除)覆盖先前添加的,然后在添加新的;
![在这里插入图片描述](https://img-blog.csdnimg.cn/9036c074503c4bc081f9787f7f5ce626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
试了一下,append()实现的功能根write()一样,他俩的区别--
1,返回值不一样;
2.append可以添加null的字符串，输出为"null"
3.而write会在编译期间抛出异常;



**文件复制（字节流）的几种方式**

**一个一个字符的复制**

```java
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\a\\b1.txt");
        FileReader fr= new FileReader(file);
        FileWriter fw= new FileWriter("D:\\复制文档.docx");
               int read = fr.read();
        while(read!=-1){
            fw.write(read);
            read=fr.read();
        }
        fw.close();
        fr.close();
    }
}
```

**利用缓冲数组方式（两种）**

方式一：

```java
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\a\\b1.txt");
        FileReader fr= new FileReader(file);
        FileWriter fw= new FileWriter("D:\\复制文档.docx");
        char c[] = new char[10];
        int read = fr.read(c);
        while(read!=-1){
            fw.write(c,0,read);//写入实际读取的有效长度
            read=fr.read(c);
        }
        fw.close();
        fr.close();
    }
}
```

方式二：

```java
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\a\\b1.txt");
        FileReader fr= new FileReader(file);
        FileWriter fw= new FileWriter("D:\\复制文档.docx");
        char c[] = new char[10];
        int read = fr.read(c);
        while(read!=-1){
            fw.write(new String (c,0,read));//将读取的转换为字符串在进行写入
            read=fr.read(c);
        }
        fw.close();
        fr.close();
    }
}
```



**缓冲流/处理流**

**FileInputStream与FileOutputStream**

```java
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:Test.txt");
        FileInputStream is = new FileInputStream(file);
        //byte v[] = new byte[3];
        int n = is.read();
        while (n != -1) {
            System.out.print((char) n);
            n = is.read();
        }
        is.close();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b0316664c1354c4e8bfa24c6d2ca029e.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
字节流适合读取非文本文件（比如图片、视频、音频等）----本例一图片为例：

```java
package HuiXin;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\117357.jpg");
        FileInputStream is = new FileInputStream(file);
        //byte v[] = new byte[3];
        int n = is.read();
        while (n != -1) {
            System.out.print( n);
            n = is.read();
        }
        is.close();
    }
}
```



接下来完成文件的复制---

```java
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\117357.jpg");
        FileInputStream is = new FileInputStream(file);
        FileOutputStream fos = new FileOutputStream("D:\\宝宝.jpg");
        long start = System.currentTimeMillis();
        int n = is.read();
        while (n != -1) {
            fos.write(n);
            n = is.read();
        }
        long end = System.currentTimeMillis();
        System.out.println(end - start);
        fos.close();
        is.close();
    }
}
```

> 复制时间对比

```java
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\117357.jpg");
        FileInputStream is = new FileInputStream(file);
        FileOutputStream fos = new FileOutputStream("D:\\宝宝.jpg");
        long start = System.currentTimeMillis();
        byte b[]=new byte[100];//缓冲
        int n = is.read(b);

        while (n != -1) {
            fos.write(b,0,n);
            n = is.read(b);
        }
        long end = System.currentTimeMillis();
        System.out.println(end - start);
        fos.close();
        is.close();

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/df0633c3a41e41d6bf2e5c5d4575c954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

所以最好用缓冲数组来读取，时间快；

**read()方法**

![在这里插入图片描述](https://img-blog.csdnimg.cn/1910f34108ad442c90b7b37c425f0c39.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> read（byte []b,int off,int len）----表示从输入流当中最多将 len 个字节的数据读取到一个 byte
> 数组当中



**BufferedInputStream 与BufferedOutputStream**

```java
import java.io.*;
public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("D:\\117357.jpg");
        FileInputStream is = new FileInputStream(file);
        byte b[]= new byte[1024*1000];
        BufferedInputStream bis= new BufferedInputStream(is);//字节缓冲输入流
        File file1= new File("D:\\亲爱的祖国.jpg");
        FileOutputStream fis= new FileOutputStream(file1);
BufferedOutputStream bos= new BufferedOutputStream(fis);//字节缓冲输出流
        int len = bis.read(b);
        while (len!=-1){
            //System.out.println(len);
            bos.write(b,0,len);
            len=bis.read(b);
        }
        bis.close();
        bos.close();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/ea2d992af46742b0bac3b6d328821dff.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70) 

**BufferedReader 与BufferedWriter**

再来，字符缓冲流

```java
package HuiXin;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\since的用法归纳及例句.txt");
        File file1= new File("D:/SINCE.txt");
        FileReader fr= new FileReader(file);
        FileWriter fw= new FileWriter(file1);
        int len = fr.read();
        long start = System.currentTimeMillis();
        while(len!=-1){
            fw.write(len);
            len=fr.read();

        }
        long end = System.currentTimeMillis();
        System.out.println(end-start);
        fw.close();
        fr.close();
    }
}
```

用时---
![在这里插入图片描述](https://img-blog.csdnimg.cn/fc5939f9643d4ea5b69ae1c5da09feb5.png)

```java
package HuiXin;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\since的用法归纳及例句.txt");
        File file1= new File("D:/SINCE.txt");
        FileReader fr= new FileReader(file);
        FileWriter fw= new FileWriter(file1);
        char c[]= new char[1024];//设置缓冲数组--一次读取大小
        int len = fr.read(c);
        long start = System.currentTimeMillis();
        while(len!=-1){
            fw.write(c,0,len);
            len=fr.read(c);

        }
        long end = System.currentTimeMillis();
        System.out.println(end-start);
        fw.close();
        fr.close();


    }
}
```

用时---
![在这里插入图片描述](https://img-blog.csdnimg.cn/62b38eab6ed846f3a89483146d13836e.png)



```java
package HuiXin;
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\since的用法归纳及例句.txt");
        File file1= new File("D:/SINCE.txt");
        FileReader fr= new FileReader(file);
        BufferedReader bfr= new BufferedReader(fr);//套起来
        FileWriter fw= new FileWriter(file1);
        BufferedWriter bfw= new BufferedWriter(fw);//套起来
        int len = bfr.read();
        long start = System.currentTimeMillis();
        while(len!=-1){
            bfw.write(len);
            len=bfr.read();

        }
        long end = System.currentTimeMillis();
        System.out.println(end-start);
        fw.close();
        fr.close();
           }
}
```

用时---
![在这里插入图片描述](https://img-blog.csdnimg.cn/da5bd1efaf154a9f9851b8f78b035dd0.png)

> 在复制文件时的效率---
>  一般形况下，一个一个读的小于缓冲数组缓冲数组小于缓冲流、 设置缓冲的好处是减少硬盘的读取和写入次数；
> 当我们数组缓冲流设置过大时也可以减少时间但是比较占用内存空间；

来看一下源码

![](..\pic\mysql_pic\7.png)

**字符/字节转换流**

```java
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\Hello.txt");
       FileInputStream fis= new FileInputStream(file);//字节读取
       InputStreamReader isr= new InputStreamReader(fis);//将字节流转换为字符流,需要指定一个编码格式，与文件编码统一，不然出现乱码，如果不指定，则默认与程序编码格式一致
        char c[]= new char[512];
        int len = isr.read(c);
        while(len!=-1){
            System.out.print(new String(c,0,len));
            len = isr.read(c);
        }

    }
}
```



复制文件--

```java
package HuiXin;

import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\Hello.txt");//源文件--文本文件
        File file1 = new File("E:/World.txt");//目标文件

        FileInputStream fis = new FileInputStream(file);//字节读取
        InputStreamReader isr = new InputStreamReader(fis, "utf-8");//将字节转换为字符
//        isr.read();
        FileOutputStream fos = new FileOutputStream(file1);
        OutputStreamWriter osw = new OutputStreamWriter(fos, "utf-8");


        char[] c = new char[1024];
        int len = isr.read(c);
        while (len != -1) {
            osw.write(c, 0, len);
            len = isr.read(c);
        }
        osw.close();
        isr.close();
    }
}
```

**如果不关闭流会对操作有什么影响？？**

![](..\pic\mysql_pic\8.png)

如果不关闭流，那么写入的数据保留在内存中，并没有真正写入文件，要么关闭流，要么刷新一下;

**IO流中的System.out**

> 在我们刚开始学习java的时候就要求我们打印“HelloWorld”，你还记得是怎么打印的吗？

> System.out.println("HelloWorld");

那他底层是怎么实现的呢？

先看System类---

```java
public final class System {。。。。。。。。。}
```

是一个断子绝孙类。。

再来看out

```java
    public static final PrintStream out = null;
```

是一个静态的输出流对象，说明可以通过System直接调用  System.out,他的返回值--PrintStream 对象；

```java
import java.io.PrintStream;

public class Test {
    public static void main(String[] args) {
        PrintStream out = System.out;
        out.print("Hello");
        out.println("World");         
            }
}
```

有了输出那么就有输入流---in，还记得我们之前学的扫描器吗？

```java
import java.util.Scanner;

public class Test {
    public static void main(String[] args) {
        Scanner sc= new Scanner(System.in);//从控制台输入
        String next = sc.next();//扫描器扫描--接收
        System.out.println(next);//打印
    }
}
```

扫描器只是起到一个扫描的作用，扫描器还可以扫描其他文件---

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws FileNotFoundException {
        File file= new File("E:\\Hello.txt");
        FileReader fr = new FileReader(file);
        Scanner sc= new Scanner(fr);//扫描文件

        while(sc.hasNext()){
            System.out.println(sc.next());//打印

        }


    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cd65a4affd2424d8ed7ca0b6b2847d1.png)



**控制台信息写入文件**

因为控制台输入，所以一般用文本处理就可以了这里是为了复习一下

```java
import java.io.*;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws IOException {
        System.out.println("请输入文本---");
        File file = new File("D:/新建文本文档.txt");
        InputStream in = System.in;
        Scanner sc = new Scanner(in);
        InputStreamReader isr = new InputStreamReader(in);//字节转换为字符
        char []c= new char[1024*8];
        int len = isr.read(c);
        FileWriter fw = new FileWriter(file);
        fw.write(new String(c,0,len));
        fw.close();
        sc.close();
    }
}
```

如果是使用扫描器进行扫描---

```java
import java.io.*;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws IOException {
        System.out.println("请输入文本---");
        File file = new File("D:/新建文本文档.txt");
        InputStream in = System.in;
        Scanner sc = new Scanner(in);
        InputStreamReader isr = new InputStreamReader(in);//字节转换为字符
       /* char[] c = new char[1024 * 8];
        int len = isr.read(c);*/
        FileWriter fw = new FileWriter(file);
//        String next = sc.next();
        String next = sc.nextLine();
        while(sc.hasNext()) {
            fw.write(next);
            fw.flush();
            next = sc.nextLine();

        }

        fw.close();
        sc.close();
    }
}
```

但是这种结果会有最由于一行没有写进文件中。

```java
import java.io.*;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws IOException {
        System.out.println("请输入文本---");
        File file = new File("D:/新建文本文档.txt");
        InputStream in = System.in;
        Scanner sc = new Scanner(in);
        InputStreamReader isr = new InputStreamReader(in);//字节转换为字符
       /* char[] c = new char[1024 * 8];
        int len = isr.read(c);*/
        FileWriter fw = new FileWriter(file);
//        String next = sc.next();
        String next = sc.nextLine();
        while(next!=null) {
            fw.write(next);
            fw.flush();
            next = sc.nextLine();

        }

        fw.close();
        sc.close();
    }
}
```

所以，当next不为空的时候，就解决了上面的问题；

但是又有一个问题，什么时候结束录入呢？

```java
import java.io.*;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) throws IOException {
        System.out.println("请输入文本---");
        File file = new File("D:/新建文本文档.txt");
        InputStream in = System.in;
        Scanner sc = new Scanner(in);
        InputStreamReader isr = new InputStreamReader(in);//字节转换为字符
       /* char[] c = new char[1024 * 8];
        int len = isr.read(c);*/
        FileWriter fw = new FileWriter(file);
//        String next = sc.next();
        String next = sc.nextLine();
        while(!(next.equals("结束")|next.equals("exit"))) {
            fw.write(next);
            fw.write("\n");
            fw.flush();
            next = sc.nextLine();
        }
        fw.close();
        sc.close();
    }
}
```

**while(!(next.equals("结束")|next.equals("exit")))我们可以换行后输入结束或者exit来作为结束的标志**

其他-----为了加快读取和写入速度可以套上个缓冲管道；



**基本数据操作流，序列化和反序列化**

> 各种各样的数据流---学完你懵不懵？-------------

**基本数据操作流---Data**

```java
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {

        File file= new File("D:/Demo.txt");
        OutputStream ops= new FileOutputStream(file);
        DataOutputStream dos=new DataOutputStream(ops);
        dos.writeBoolean(false);
        dos.writeUTF("今天");
        dos.writeDouble(10.25);
        dos.writeByte(100);
        dos.close();

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/60d74f4cc8374b6cb01031ce4fcc87b7.png)
这写入了些啥？
看不懂---乱码，因为是给机器看的，结局相和谐数据需要用到DataIntputStream

```java
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {

        File file= new File("D:/Demo.txt");
      InputStream is= new FileInputStream(file);
      DataInputStream dis= new DataInputStream(is);
      //按照顺序读取
        System.out.println(dis.readBoolean());
        System.out.println(dis.readUTF());
        System.out.println(dis.readDouble());
        System.out.println(dis.readByte());
dis.close();

    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e2cdd0ff3c5e4fc5b1358dd00562e473.png)

public class DataOutputStream extends FilterOutputStream implements DataOutput {
public class DataInputStream extends FilterInputStream implements DataInput {
DataIntput 接口用于将数据从一系列字节转换为 Java 基本类型，并将这些字节读取为二进制流。
DataOutput 接口用于将数据从任意 Java 基本类型转换为一系列字节，并将这些字节写入二进制流。同时还提供了一个将 String 转换成 UTF-8 修改版格式并写入所得到的系列字节的工具。

对于此接口中写入字节的所有方法，如果由于某种原因无法写入某个字节，则抛出 IOException。



对象序列化----

```java
import java.io.*;

public class Test {
    public static void main(String[] args) throws IOException {

        File file= new File("D:/Hello.txt");//目标文件
        OutputStream is= new FileOutputStream(file);//开辟输出流
        ObjectOutputStream oos= new ObjectOutputStream(is);//序列化
        oos.writeObject(new String("我的老家，就住在这个屯"));
        oos.close();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9f1e428fbf124d1693c675ee4796e211.png)

生成的文件看不懂，再写一个程序读取就可以了；

```java
package HuiXin;

import java.io.*;

public class Test01 {
    public static void main(String[] args) throws IOException, ClassNotFoundException {

        File file= new File("D:/Hello.txt");
        InputStream is= new FileInputStream(file);
        ObjectInputStream ois= new ObjectInputStream(is);
      String per = (String) (ois.readObject());
        System.out.println(per);
        ois.close();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/bcc614fd887940a88f53ef9d5ec21409.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/819fd3c814f14033bad4ff2149255790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

自定义类序列化---要实现Serializable接口

```java
import java.io.*;

class   Person implements Serializable {
    private static final long serialVersionUID = 1L;
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
}


public class Test {
    public static void main(String[] args) throws IOException {
        File file= new File("D:/Hello.txt");//目标文件
        OutputStream is= new FileOutputStream(file);//开辟输出流
        ObjectOutputStream oos= new ObjectOutputStream(is);//序列化
        oos.writeObject(new Person("孙悟空",18,"男"));
        oos.close();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/77c80d5d022341599c9f76abe7fc3d90.png)
接下来反序列化--

```java
import java.io.*;

public class Test01 {
    public static void main(String[] args) throws IOException, ClassNotFoundException {

        File file= new File("D:/Hello.txt");
        InputStream is= new FileInputStream(file);
        ObjectInputStream ois= new ObjectInputStream(is);
      Person per = (Person) (ois.readObject());
        System.out.println(per);
        ois.close();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/660540c6692441c6a69961b7a7d95a46.png)

```java
import java.io.*;

class   Person implements Serializable {

    private static final long serialVersionUID = 792646246687698667L;
   transient String name;
    static int age;
    String gender;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

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
}



public class Test {
    public static void main(String[] args) throws IOException {
        File file= new File("D:/Hello.txt");//目标文件
        OutputStream is= new FileOutputStream(file);//开辟输出流
        ObjectOutputStream oos= new ObjectOutputStream(is);//序列化
        oos.writeObject(new Person("孙悟空",18,"男"));
        oos.close();
    }
}
```

```java
import java.io.*;

public class Test01 {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        File file= new File("D:/Hello.txt");
        InputStream is= new FileInputStream(file);
        ObjectInputStream ois= new ObjectInputStream(is);
      Person per = (Person) (ois.readObject());
        System.out.println(per);
        ois.close();
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/418cc3e6a95b4fa6ac37cc4ad67b7927.png)



总结---
自定义数据序列化的时候必须要类中所有属性都要能够序列化；
静态属性---static的数据不能被序列化；
transenit修饰的也不能被序列化；



## 国际化程序的实现（理解）

一个程序可以适应不同国家的语言要求，那么这样的程序成为国际化程序；

如果要想实现国家化程序则必须依靠Local\ResourceBoundle\MessageFormat几个完成，并结合属性文件的；

**Locale**

Locale 表示的是一个地区，也就是说在国际化程序开发中通过Locale指定当前所在的地区，世界上都存在一个国家的编号，例如--

中国：zh CN

美国：en US

实际上浏览器中都会有不同地区的编号

![](..\pic\图片94.png)

如果要想创建Locale类的对象--

1，直接通过取得本机来取得--public static Locale getDefault()    通过方法取得

2，通过指定语言来取得--public Locale(String language)     通过构造方法指定

![](..\pic\图片95.png)

如果要想实现国际化，只靠Locale类是不够的，还需要配合属性文件存在，一个地区的Locale对戏那个都应该对应一个属性文件；

找到属性文件之后，下一步就需要将具体内容读取出来，所有的内容都需要依靠ResourceBundle类来读取。

**ResourceBundle**

此类是Java.util包中；

public abstract class ResourceBundle extends Object

使用时会通过静态方法取得实例，主要有两种--

1，根据本机的Locale取得属性文件--

public static final ResourceBundle getBundle(String baseName)

2，根据指定的Locale取得属性文件--

public static final ResourceBundle getBundle(String baseName,Locale locale)

怎么访问属性文件呢？

属性文件--Message.properties

Key 为Info，Value为“你好！”

在使用ResourceBundle 读取的时候要根据Key进行读取，方法是：

public final String getString(String key)

```java
import java.util.*;
public class LocaleDemo {
	public static void main(String[] args) {
		Locale lc = Locale.getDefault();//静态引用，得到默认的Locale
		//要找到Message属性文件，省略了后缀，并指定了区域
        ResourceBundle rb = ResourceBundle.getBundle("Message",lc);
        String value = rb.getString("info");//根据Key找到内容
        System.out.println("找到的内容为----》"+value);	}}


```

但是一般属性文件中是不会出现中文字符的，如果出现汉字则必须进行转码操作，转换成UNICODE编码。在JDK1.8之前JAVA中提供了转换工具，之后的JDK中支持了UTF-8编码，也就不用转换了；

**国际化操作**

现在假设如果是中文环境，则显示”你好！”，如果是英文环境则显示”Hello!”，如果要想完成此程序，则首先应该建立两个是属性文件，这两个属性文件有命名要求：属性名称，区域名称.properties

Message_zh_CN.properties--表示中文文件

Message_en_US.properties--表示英文文件

```java
import java.util.*;
public class LocaleDemo {
	public static void main(String[] args) {
		Locale loc = Locale.getDefault();//得到环境属性
		ResourceBundle re=ResourceBundle.getBundle("Message_zh_CN", loc);
		System.out.println(re.getLocale());
		Locale ch = new Locale("zh","CN");//指定中文环境
		Locale en = new Locale("en","US");//指定英文环境
		//要找到Message属性文件，省略了后缀，并指定了区域
        ResourceBundle chrb = ResourceBundle.getBundle("Message",ch);
        ResourceBundle enrb = ResourceBundle.getBundle("Message",en);
        String zhvalue = chrb.getString("info");//根据Key找到内容
        String envalue = enrb.getString("info");//根据Key找到内容
        System.out.println("中文内容为----》"+zhvalue);
        System.out.println("英文内容为----》"+envalue);	}}

```

实际上在整个国际化程序中属性文件是最关键的，因为可以通过不同的语言进行自动的文字装载 

```java
import java.util.*;
public class LocaleDemo {
	public static void main(String[] args) {
		Locale lc = Locale.getDefault();//静态引用，得到默认的Locale
		ResourceBundle rb = ResourceBundle.getBundle("Message",lc);

```

当指定的属性文件找不到的时候，会默认寻找下一个；

**动态文本**

之前所有内容实际上在属性文件中保存的值都是固定的，如果现在希望根据某些内容进行动态的显示

例如： 你好，XXX

那么，此时就可以用占位符的方式完成，在属性文件中加入{编号}即可.

Message_zh_CN.properties

info=你好，{1},{2},{3}

Message_en_US.properties

info=Hello,{1},{2},{3}

之后就可以使用MessageFormat进行文字的格式化

public class MessageFormat extends Format--此类在Java.text包中

使用此方法完成动态文本的增加--public static String format(String pattern, Object... arguments)动态文本操作

```java
package games;
import java.text.MessageFormat;
import java.util.*;
public class LocaleDemo {
	public static void main(String[] args) {
		Locale loc = Locale.getDefault();//得到环境属性
		ResourceBundle re=ResourceBundle.getBundle("Message_zh_CN", loc);
		System.out.println(re.getLocale());
		Locale ch = new Locale("zh","CN");//指定中文环境
		Locale en = new Locale("en","US");//指定英文环境
		//要找到Message属性文件，省略了后缀，并指定了区域
        ResourceBundle chrb = ResourceBundle.getBundle("Message",ch);
        ResourceBundle enrb = ResourceBundle.getBundle("Message",en);
        String zhvalue = chrb.getString("info");//根据Key找到内容
        String envalue = enrb.getString("info");//根据Key找到内容
        System.out.println("中文内容为----》"+MessageFormat.format(zhvalue, "华硕","笔记本"));
        System.out.println("英文内容为----》"+MessageFormat.format(envalue, "ASUS","NoteBook"));	}}


```

```java
import java.util.ListResourceBundle;
public class Message_en_US extends ListResourceBundle {
	Object[][] date = { { "info", "Hello,{0},age" } };
	@Override
	protected Object[][] getContents() {
		return this.date;	}}

```

在程序中可以通过存储全部的资源内容，那么可以通过继承ListResourceBundle来完成

如果在一个classpath中同时存在

Message.properties\Message_zh_CN.properties\Message.java\Message_zh_CN.java那么找的顺序是

1，Message_zh_CN.java   2，Message.java3，Message_zh_CN.properties

2，Message.properties

但是从实际角度来看一般不会使用一个类来表示资源文件内容，都是用.properties文件保存；

## 33.对象序列化（重点）

所谓的对象序列化（在某些书籍中也叫串行化），是指在内存之中保存的对象转化为二进制数据流的形式的一种操作。通过将对象序列化，可以方便地实现对象的传输及保存。但是在

Java

之中并不是所有的类的对象都可以被序列化，如果一个类对象需要被序列化，则此类一定要实现

java. io.Serializable接口。

但是这个接口里面也没有定义任何的方法，所以此接口依然属于标识接口，表示一种能力；

```java
import java.io.*;
class Person implements Serializable {
	private static final long serialVersionUID = 1L;
	private String name;	private String sex;
	transient private int age;//年龄信息被忽略，不能序列化
	public Person(String name,String sex, int age) {
		this.name = name;		this.age = age;		this.sex=sex;}
	public String toString() {
		return "名字--" + this.name +" ----->"+"年龄--" + this.age;	}}
public class XuliehuaAble {
	public static void main(String[] args) throws Exception {
		File file = new File("Serializeable");// ------序列化-----
		OutputStream out = new FileOutputStream(file);
		ObjectOutputStream oos = new ObjectOutputStream(out);// 参数接受字节流实例
		oos.writeObject(new Person("单身狗", "男",28));
		oos.close();		out.close();//------逆向序列化------
		InputStream in = new FileInputStream(file);// 参数接受字节流实例
		ObjectInputStream ois = new ObjectInputStream(in);
		Object obj = ois.readObject();// 讀取
		Person per = (Person) obj;
		System.out.println(per);		ois.close();	}}

```




编译之后会生成一个二进制文件用来存储序列化的信息

![](..\pic\图片96.png)

�� sr Person        L namet _x0012_Ljava/lang/String;L sexq ~ xpt  单身狗t 男---此为二进制文件的内容，我们可以发现字符串会被显示，因为电脑编码的原因，直接将字符串 

年龄信息序列化时被忽略，--关键字transient

序列化之后可以进行反序列化操作来取出相应的信息；

既然可以对一个对象序列化，那么是否可以对多个对象序列化呢？---用对象数组的形式

```java
import java.io.*;
class Person implements Serializable {
	private static final long serialVersionUID = 1L;
	private String name;	private String sex;	private int age;//
	public Person(String name, String sex, int age) {
		this.name = name;		this.age = age;		this.sex = sex;	}
	public String toString() {
		return "名字--" + this.name + "--性别--" + this.sex + "年龄--" + this.age;	}}
public class XuliehuaAble {
	public static void main(String[] args) throws Exception {
		Person per[] = { new Person("单身狗", "男", 28), new Person("单身二丫", "女", 28) };
		set(per);		Person p[] = (Person[]) Dset();		print(p);	}
		


```

```java
public static void set(Object obj) throws Exception {// ------序列化-----
		File file = new File("Serializeable");		OutputStream out = new FileOutputStream(file);
		ObjectOutputStream oos = new ObjectOutputStream(out);// 参数接受字节流实例
		oos.writeObject(obj);
	oos.close();		out.close();	}
	public static Object Dset() throws Exception {	// ------逆向序列化------
		Object temp = null;
		File file = new File("Serializeable");
		InputStream in = new FileInputStream(file);// 参数接受字节流实例
		ObjectInputStream ois = new ObjectInputStream(in);
		temp = ois.readObject();// 讀取
		ois.close();		return temp;		}
public static void print(Person[] per) {
		for (Person p : per) {
			System.out.println(p.toString());		}	}}
```

**字符流和字节流的转换（插入内容）**

```java
import java.io.*;
public class Writ {
    public static void main(String[] args) throws Exception {
        BufferedReader bReader = new BufferedReader(new InputStreamReader(System.in));
        /* 参数为reader类，但是用于输入的system.in为 字节输入流， 由于要将要输入的内容转换为字符，所以要包装成字节转字符的实例* */
        String str = null;
        System.out.println("请输入一个数--");
        str = bReader.readLine();
        System.out.println("你输入的数是---" + str);
        int i = 0;
        i = Integer.parseInt(str);
        System.out.println("这个数的平方是--" + i * i);    }}

```



## Java小程序--Applet（了解）

现在已经被淘汰了,但已经被淘汰了的技术为什么要学习呢?

任何技术的发展都是在前者的基础上发展而来的,这里主要是 通过了解apple的生命周期为以后javaweb打些基础;



编写格式--必须继承applet类才能在网页中加载使用

编写时没有main方法，需要配合HTML文件使用

范例

```java
import java.applet.Applet;
import java.awt.Graphics;
public class JavaApp extends Applet {
	public void paint(Graphics g) {
		g.drawString("將進酒", 100, 100);
		g.drawArc(80, 50, 40, 30, 60, 368);	}}


```

```html
<html>
<head>
<title>Applet</title>
</head>
<body>
<applet CODE="JavaApp.class"width=300"height="100"></applet>
</body>
</html>


```

一个applet的生命周期涉及 init()\start()\stop()\destroy()等四种方法，这四种方法都是Applet类的成员，可以被继承也可以覆写，除此之外，为了在Applet程序中实现输出，每个Applet程序中还需要重写paint方法。

在Applet类中没有体供init()\start()\stop()\destroy()、paint()方法的任何实现，且他们都是被浏览器或者appletviwer调用的，所以这几个方法要完成的功能应由编程人员自行编写；

⑴ public void init()

init()方法是Applet运行的起点。当启动Applet程序时，系统首先调用此方法，以执行初始化任务。

⑵ public void start()

start()方法是表明Applet程序开始执行的方法。当含有此Applet程序的Web页被再次访问时调用此方法。因此，如果每次访问Web页都需要进行一些操作的话，就需要在Applet程序中重写该方法。在Applet程序中，系统总是先调用init()方法，后调用start()方法。

⑶ public void stop()

stop()方法用于使Applet停止执行。当含有该Applet的Web页被其他页代替时也要调用该方法。

⑷ public void destroy()

destroy()方法收回Applet程序的所有资源，即释放已分配给它的所有资源。在Applet程序中，系统总是先调用stop()方法，后调用destroy()方法。

⑸ paint(Graphics g)

paint(Graphics g)方法用于使Applet程序在屏幕上显示某些信息，如文字、色彩、背景或图像等。参数g是Graphics类的一个对象实例，实际上可以把g理解为一个画笔。对象g中包含了许多绘制方法，如drawString()方法就是输出字符串。

⑹ repaint()

repaint()方法的功能是：程序首先清除用paint()方法以前所画的内容，然后再调用paint()方法。



了解运行思路就可以了 ,代码可以不去练习,因为现在浏览器基本都将Java脚本给屏蔽了



## 34.网络编程

1，TCP程序的实现

2，将多线程应用在网络开发中

3，UDP的程序实现

**34.1网络编程的概念**

将不在一起的主机进行互联，在网络上通讯需要使用协议，常见的通讯协议：TCP、UDP

TCP---属于可靠的链接，使用三方握手的方式完成链接的确认。

UDP--属于不可靠的链接。

对于网络程序的开发有两种架构--

C/S--客户端/服务器端，对于这种程序需要开发两套代码 一套是客户端，一套是服务器，维护也要两套；

B/S--浏览器/服务器，就类似于论坛，开发和维护的时候只需要一套

**34.1.1.TCP程序的实现**

在java中所有的网络开发包保存在Java.net包中，在此包中使用SeverSocket和Socket类完成服务器和客户端的开发；

**34.1.2.简单的TCP程序**

如果想要开发TCP程序，则首先开发服务器端，在服务器端，要使用ServerSocket进行客户端的连接与接收，每一个客户端程序都是用Socket对象表示；

public class ServerSocket extends Object implements Closeable

构造方法--

public ServerSocket()  throws IOException --创建一个没有绑定端口的的服务套

Creates an unbound server socket.

public ServerSocket(int port)  throws IOException

public ServerSocket(int port,int backlog,InetAddress bindAddr) throws IOException

 

如果此时要想进行连接的操作，可以通过

telnet命令完成\---

```java
import java.io.IOException;
import java.io.OutputStream;
import java.io.PrintStream;
import java.net.*;
public class Tcpdemo {
	public static void main(String[] args) throws IOException {
		///// 建立服务器端//////
		ServerSocket server = new ServerSocket(8888);// 实例化端口,在888端口开启服务
		Socket client = null;// 表示开辟的客户端
		System.out.println("等待连接......");
		client = server.accept();// 接收客户端的连接
		OutputStream out = client.getOutputStream();// 得到客户端的输出流
		PrintStream pout = new PrintStream(out);// 返回数据
		pout.println("Hello World");
		pout.checkError();
		client.close();
		server.close();	}}

```

此程序执行一次将关闭；

以上是通过telnet命令来完成服务器的访问，也可以通过单独的客户端程序进行访问；

```java
	import java.io.*;import java.net.*;
public class TCPclient {
	public static void main(String[] args) throws Exception {
		Socket client = new Socket("localhost", 8888);// 表示连接主机的接口
	BufferedReader br = new BufferedReader(new InputStreamReader(client.getInputStream()));
String str = br.readLine();// 接受回應的内容
		System.out.println("内容是" + str);
		client.close();	}}



```

**34.1.3.Echo程序--回应程序**

Echo程序也是通过Socket和ServerSocket类完成的，输入的内容发送到服务器端之后，在前面加上”ECHO”字符串再返回；

对于服务器而言，客户端的输出时服务器端的输入流，服务器的输出流时客户端的输入流

```java
import java.io.*;
import java.net.*;
public class EchoDemo {
	public static void main(String[] args) throws IOException {
ServerSocket server = new ServerSocket(8080);		
	Socket client = null;// 表示开辟的客户端
			boolean flag = true;
			while(flag) {	
			System.out.println("等待连接......");
	client = server.accept();
		BufferedReader bf = new BufferedReader(new InputStreamReader(client.getInputStream()));//得到客户端的输入流
OutputStream out = client.getOutputStream();// 得到客户端的输出流
	PrintStream pout = new PrintStream(out);// 返回并打印数据
			boolean temp = true;
			while(temp) {//循环接受用户的输入内容并回应
				String str = bf.readLine();
				if (str==null||"".equals(str)) {
					temp=false;	break;				}
				if("bye".equals(str)) {temp=false;	break;	}
				pout.println("ECHO:"+str);}//回送信息
			pout.close();	client.close();}	server.close();}}
//此时客户端也需要输入和输出流；
//服务器但要先存在（运行），否则会出错



```

```java
import java.io.*;
import java.net.*;
public class EacoClient {
	public static void main(String[] args) throws Exception, IOException {
		Socket client = new Socket("localhost", 8080);// 表示连接主机的接口
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		PrintStream out = new PrintStream(client.getOutputStream());
		boolean flag =true;
		while (flag) {
			System.out.println("请输入要发送的内容:");
			String str = br.readLine();// 接受回應的内容
			if (str==null||"".equals(str)) {
				flag=false;
				break;				}
			if("bye".equals(str)) {
				flag=false;
				break;				}
			out.println("内容是" + str);
			System.out.println("ECHO:"+str);
		}		
		client.close();	}}

```

但是，以上只适用于一个线程使用，如果有多个用户则肯定无法同时连接，这就是传统单线程的连接，要想多个用户使用，则每一个客户端应该是一个线程表示出来；

**34.1.4.加入多线程**

每一个客户端都使用一个线程对象表示--

之后服务器端在每次接收到客户端请求后重新启动一个线程，这样就可以使得更多客户端访问

所以一个服务器要使用一个多线程；

```java
import java.net.*;
import java.io.*;
public class EacoSocket implements Runnable {
	private Socket client;
	public EacoSocket(Socket client) {
		this.client = client;	}
	public static void main(String[] args) {
			}
	@Override
	public void run() {
			try {
			BufferedReader bf = new BufferedReader(new InputStreamReader(client.getInputStream()));// 得到客户端的输入流
			OutputStream out = client.getOutputStream();// 得到客户端的输出流
			PrintStream pout = new PrintStream(out);// 返回并打印数据
			boolean temp = true;
			while (temp) {// 循环接受用户的输入内容并回应
				String str = bf.readLine();
				if (str == null || "".equals(str)) {
					temp = false;
					break;				}
				if ("bye".equals(str)) {
					temp = false;
					break;				}
				pout.println("ECHO:" + str);
			} // 回送信息
			pout.close();
			client.close();
		} catch (Exception ex) {
		}	}}

import java.io.*;
import java.net.*;
public class EchoDemo {
	public static void main(String[] args) throws IOException {
		ServerSocket server = new ServerSocket(8080);
		boolean flag = true;
		while (flag) {
			System.out.println("等待连接......");
			new Thread(new EacoSocket(server.accept())).start();
		}
		server.close();	}}


```

**34.1.5.UDP程序实现**

UDP使用数据报的形式实现，那么需要使用以下两个类

数据报的内容：DatagramPacket

发送和接收数据报：DatagramSocket

在开发TCP程序时时现运行服务器端再运行客户端，而UDP是先运行客户端，再运行服务器端

```java
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.SocketException;
public class UDPdemo {
	public static void main(String[] args) throws Exception {
		DatagramSocket socket =null;
socket = new DatagramSocket(3000);
DatagramPacket packet =new DatagramPacket(new byte[1024], 1024);//开辟空间
socket.receive(packet);
System.out.println(new String(packet.getData()));//接收数据
	}
}
```

## 35.JAVAWEB

**JAVAWeb中的IO流**

> 两台设备之间时如何通信的?靠意念吗?

既然要通信,那就的遵循通信规则,坦诚布公;由于每个用户是在应用层及逆行数据的获取,底层还有其他层;

> 应用层获取数据,传到传输层,在通过网络层将数据进行包装,最后通过物理层将数据打包好后发出去,接着由另一个设备接收然后再层层打开包装最后展现在应用层上面去;
> 简单的通信模型就完成了;

![在这里插入图片描述](https://img-blog.csdnimg.cn/f5537ef2f4064dabaec790b85999c39b.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
举个例子,现在用的最多的微信,你我聊天,信息是怎样精准的传到你的设备上的???

![在这里插入图片描述](https://img-blog.csdnimg.cn/058ee46bd447439785bf67eae8d392c3.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
类比----
之前IO流 的时候是要包装文件地址/或者文件地址然后在此基础上进行操作;

换成网络就是要将ip或者域名包装成机器能够识别的地址然后在进行操作------
![在这里插入图片描述](https://img-blog.csdnimg.cn/2494c768bdd146ddacc827864f81ca4c.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
public class Test {
    public static void main(String[] args) {
//        InetAddress ia= new InetAddress();//因为InetAddress的构造方法是default修饰的,不能够跨包使用
        //还好提供给了其他方法来获得InetAddress实例
        try {
            InetAddress byName = InetAddress.getByName("172.20.10.10");//封装IP
            InetAddress byName1 = InetAddress.getByName("localhost");//本地IP
            InetAddress byName2 = InetAddress.getByName("LAPTOP-GAVIN");//获取本电脑IP
            InetAddress byName3 = InetAddress.getByName("www.baidu.com");//获得百度IP
            InetAddress byName4 = InetAddress.getByName("www.sina.com.cn");//获得渣浪IP
            System.out.println(byName);
            System.out.println(byName1);
            System.out.println(byName2);
            System.out.println(byName3);
            System.out.println(byName4);
            System.out.println(byName.getAddress());//获得原始IP地址
            System.out.println(byName3.getHostAddress());//获得主机地址
            System.out.println(byName3.getHostName());//获得域名


        } catch (UnknownHostException e) {
            e.printStackTrace();
        }

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/eeab61ebcdcb4edc9e65154d7e2f8b68.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

**TCP/IP**

**单向通信**

这算时间在不断学习,发现越发内卷,都说程序员是新一代农民工,现在只能多搬搬砖了.......

我们几乎每天都在上网,普通人只知道上网,却不知道设备与设备之间是怎样沟通的,巧了之前我也是一个小白,现在斗胆来画个图(简易版的)
两台机器之间的通信----

![在这里插入图片描述](https://img-blog.csdnimg.cn/ac49d668c0c649f3ad9fb9c802a2a51b.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)差不多了,在扯多了就圆不回来了.

下面写一个两台机器之间的通信原理-----利用套接字让两台设备进行单向沟通

```java
public class tesTip {
    public static void main(String[] args) {
        Socket client =null;//开辟客户端,像服务器发送
        DataOutputStream dos=null;
        try {
           client= new Socket("localhost",8888);
            OutputStream outputStream = client.getOutputStream();//客户端向服务器发数据(要知道服务器的地址和端口号)
            dos= new DataOutputStream(outputStream);
            dos.writeUTF("尔曹身与名俱灭,");
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
            try {
                dos.close();
                client.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }

    }
}

```

```java
import java.io.DataInputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class Iptest001 {
public static void main(String[] args) {
	ServerSocket server=null;
	Socket client= null;
	DataInputStream dis=null;
	try {
		server= new ServerSocket(8888);//服务器在8888端口等候操作
		client=server.accept();
		 dis=new DataInputStream(client.getInputStream());
		
		System.out.println(dis.readUTF());
		
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}finally {
		try {
			dis.close();
			client.close();
			server.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e725644116164541bef49521a3806575.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1c2d95fc40494f3d93010d3966602b5e.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)在单向通信的基础上进行双向通信----

**双向通信**

```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class Iptest001 {
public static void main(String[] args) {
	ServerSocket server=null;
	Socket client= null;
	DataInputStream dis=null;
	DataOutputStream dos= null;
	try {
		//接收客户端发送的数据
		server= new ServerSocket(8888);//服务器在8888端口等候操作
		 System.out.println("等待客户端的鏈接.....");
		client=server.accept();//等待客户端的连接,
		 dis=new DataInputStream(client.getInputStream());//连接上之后接收输入的流
		 try {
			
			Thread.sleep(10000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("客户端发送的数据--"+dis.readUTF());
		
		//向客户端反馈数据
		dos= new DataOutputStream(client.getOutputStream());
		dos.writeUTF("不废江河万古流.");
		
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}finally {
		try {
			dis.close();
			dos.close();
			client.close();
			server.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/90c4cf6d0ba34916ad267a9e0afb9edd.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```java
import java.io.*;
import java.net.Socket;

/**
 * @author : Gavin
 * @date: 2021/8/19 - 08 - 19 - 15:12
 * @Description: IpTest
 * @version: 1.0
 */
public class tesTip {
    public static void main(String[] args) {
        Socket client = null;//开辟客户端,像服务器发送
        DataOutputStream dos = null;
        DataInputStream dis = null;
        try {

            //向服务器发送数据----
            client = new Socket("localhost", 8888);
            OutputStream outputStream = client.getOutputStream();//客户端向服务器发数据(要知道服务器的地址和端口号)
            dos = new DataOutputStream(outputStream);
            dos.writeUTF("尔曹身与名俱灭,");
            try {
                System.out.println("等待服務器反饋.....");
                Thread.sleep(10000);
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
            //接收服务器发送来的反馈数据
            InputStream inputStream = client.getInputStream();
            dis = new DataInputStream(inputStream);
            System.out.println("服务器返回的数据--" + dis.readUTF());


        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                dos.close();
                dis.close();
                client.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/946ab6a9e2c24754955d92627a58c25f.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 以上就是简单的单向和双向通信 最主要的是整明白getOutputStream 和getInputStream  的区别;

**复杂的双向通信**

实际运用的时候并没有以上这么简单,举个稍微比上述复杂点的,

模拟APP登录-----输入用户名和密码;

实现原理就是---
1,客户端登陆时检查与服务器的通信,
2,连接成功后执行登录,输入用户名和密码,并将数据发送给服务器
3,服务器端检测用户名和密码,并返回一个结果给客户端,
4,客户端根据结果来决定是否进入主页面;
![在这里插入图片描述](https://img-blog.csdnimg.cn/e9b2ac1554034d5497313f95589835aa.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



```java
import java.io.*;
import java.net.Socket;
import java.util.Scanner;

/**
 * @author : Gavin
 * @date: 2021/8/19 - 08 - 19 - 15:12
 * @Description: IpTest
 * @version: 1.0
 */
public class tesTip {
    public static void main(String[] args) {
        Socket client = null;//开辟客户端,接收服务器发来的数据
        DataOutputStream dos=null;
        ObjectOutputStream oos = null;
        DataInputStream dis= null;
        ObjectInputStream ois = null;
          String name = null;
        String passWord = null;
        User user = new User(name, passWord);

        try {
            //向服务器发送请求连接----
            client = new Socket("localhost", 8888);//发送地址

            OutputStream outputStream = client.getOutputStream();
            dos = new DataOutputStream(outputStream);
            //向服务器发送数据
            dos.writeUTF("客户端正在请求连接服务器........");
            try {
                System.out.println("等待服务器反馈.....");
                Thread.sleep(10);
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }

            //接收服务器发送来的反馈数据
            InputStream inputStream = client.getInputStream();
            dis = new DataInputStream(inputStream);
            String s = dis.readUTF();
            System.out.println("服务器返回的数据--" + s);
            //根据服务器反馈选择是否要出现登录界面
            if ("是".equals(s)) {
                Scanner sc = null;
                System.out.println("请输入账号----");
                sc = new Scanner(System.in);
               //name=sc.next();
                user.setAdminName(sc.next());//接收用户名;
                System.out.println("请输入账号密码----");
                sc = new Scanner(System.in);
                //passWord=sc.next();
                user.setPassWord(sc.next());//接收密码
                //接收完毕之后向服务器发送数据
                oos=new ObjectOutputStream(client.getOutputStream());
                oos.writeObject(user);//
                //发送完数据后等待接收服务器的反馈
                System.out.println("正在连接服務器.....");
                try {
                    Thread.sleep(1000);//模拟等待时间
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                //接收服务器的反馈
                dis=new DataInputStream(client.getInputStream());
                String s1 = dis.readUTF();
                if("是".equals(s1)){
                    System.out.println("登陆成功");
                }else{
                    System.out.println("登陆失败");
                }
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                dos.close();

                oos.close();
                dis.close();
                client.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }

    }
}

```

```java
package IpTest;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class Iptest001 {
public static void main(String[] args) {
	ServerSocket server=null;
	Socket client= null;
	DataInputStream dis=null;
	ObjectInputStream ois=null;
	DataOutputStream dos= null;
	ObjectOutputStream oos= null;
	try {
		//接收客户端发送的数据
		server= new ServerSocket(8888);//服务器在8888端口等候操作
		 System.out.println("等待客户端的鏈接.....");
		client=server.accept();//等待客户端的连接,
		 dis=new DataInputStream(client.getInputStream());//连接上之后接收输入的流
		 try {
			
			Thread.sleep(100);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("客户端请求的数据--"+dis.readUTF());
		
		//向客户端反馈数据
		dos= new DataOutputStream(client.getOutputStream());
		dos.writeUTF("是");
		
		//接着接收客户端发来的数据
		try {//获取数据流
			ois=new ObjectInputStream(client.getInputStream());
			User user=(User)ois.readObject();
			if("zhangsan".equals(user.getAdminName())&"123456".equals(user.getPassWord())) {
				dos.writeUTF("是");
			}else {
				dos.writeUTF("否");
			}
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}finally {
		try {
			dis.close();
			dos.close();
			ois.close();
			client.close();
			server.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b9b75a92ac9e434ca5ae5e5a56383285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

**UDP**

话不多说看代码----
写一个学生端

```java
import java.io.IOException;
import java.net.*;


/**
 * @author : Gavin
 * @date: 2021/8/20 - 08 - 20 - 10:39
 * @Description: IpTest
 * @version: 1.0
 */
public class StudentSend {
    public static void main(String[] args) {
        System.out.println("學生登陸---");
        DatagramSocket dgs=null;
        InetAddress localhost =null;
        try {
            dgs=new DatagramSocket(8888);//准备好了端口号
           String str= "HELLOWORLD!";//准备数据包
            byte[]b=str.getBytes();
            //开始发送
           localhost= InetAddress.getByName("localhost");
            //向localhost:9999发送数据
                DatagramPacket dp= new DatagramPacket(b,b.length, localhost,9999);
         dgs.send(dp);
         dgs.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

```

再写一个老师端--

```java
import java.io.IOException;
import java.net.*;

/**
 * @author : Gavin
 * @date: 2021/8/20 - 08 - 20 - 10:39
 * @Description: IpTest
 * @version: 1.0
 */
public class TeacherReceive {
    public static void main(String[] args) {
System.out.println("老师上线了");
        DatagramSocket  dgs=null;
        InetAddress localhost= null;

        try {
            //老师在9999端口接收学生在8888端口发送的数据
            dgs= new DatagramSocket(9999);
            //开辟一个空包用于接收数据
            byte b[]= new byte[1024];
            //接收数据的准备工作
            localhost=InetAddress.getByName("localhost");
            //准备接收localhost:8888发来的数据
            DatagramPacket dp= new DatagramPacket(b,b.length,localhost,8888);
            //接收完毕---取出内容
            dgs.receive(dp);

            byte data[]=dp.getData();
            String str= new String(data,0,dp.getLength());
            System.out.println(str);
            dgs.close();

        } catch (SocketException | UnknownHostException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

结果如下----

```bash
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
老师上线了
HELLOWORLD!

```

双向通信

```java
import java.io.IOException;
import java.net.*;


/**
 * @author : Gavin
 * @date: 2021/8/20 - 08 - 20 - 10:39
 * @Description: IpTest
 * @version: 1.0
 */
public class StudentSend {
    public static void main(String[] args) {
        System.out.println("学生上线了");

        DatagramSocket dgs=null;
        DatagramPacket dgp=null;
            try {
            //开启学生端---指定为8888端口
            dgs=  new DatagramSocket(8888);
            //准备要发送的数据
            String stuData="Hello Teacher";
            //发送数据前的准备---将要发送的数据包装成字节数组
            byte []bytes= stuData.getBytes();
            //发送数据前的准备--要发往的地址
            InetAddress localhost = InetAddress.getByName("localhost");
            //打包数据--往localhost:9999端口发送数据
            dgp=new DatagramPacket(bytes,bytes.length,localhost,9999);
           //发送数据
            dgs.send(dgp);

            //接收教师端返回的数据
                try {
                    Thread.sleep(10);//模拟网络延迟
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                //接收数据前的准备工作---开辟空间
            byte teaData[]= new byte[1024];

              //接收数据并打包
              dgp=new DatagramPacket(teaData,teaData.length,localhost,9999);
             //接收的动作
              dgs.receive(dgp);
                //打包数据
              teaData=dgp.getData();
              //取出数据
                String str= new String(teaData,0,dgp.getLength());
                System.out.println("老师端返回的数据--"+str);
            } catch (SocketException | UnknownHostException e) {
            e.printStackTrace();
        } catch (IOException e) {
                e.printStackTrace();
            }
            finally{
                dgs.close();
            }
    }
}

```

```java
import java.io.IOException;
import java.net.*;

/**
 * @author : Gavin
 * @date: 2021/8/20 - 08 - 20 - 10:39
 * @Description: IpTest
 * @version: 1.0
 */
public class TeacherReceive {
    public static void main(String[] args) {
System.out.println("老师一直在线....");
DatagramSocket dgs=null;
DatagramPacket dgp=null;
try {
	//开辟老师端
	dgs= new DatagramSocket(9999);
	//接收数据前的准备--开辟空间用于接收数据
	byte[]data= new byte[1024];
	//接收的数据来自于哪里?
	InetAddress localhost=InetAddress.getByName("localhost");
	//准备接收数据---来自于学生端localhost:8888端口
	dgp= new DatagramPacket(data,data.length,localhost,8888);
	//接收数据并打包
	dgs.receive(dgp);
	//取出实际的数据长度
	data=dgp.getData();
	String str= new String (data,0,dgp.getLength());
	
	System.out.println("学生发送的数据---"+str);
	
	//准备向学生端反馈数据
	
	//准备要发送的数据
	String teaData="Hello Student";
	//发送数据前的准备---将要发送的数据包装成字节数组
    byte []bytes= teaData.getBytes();
   
   
    //打包数据--往localhost2:8888端口发送数据
    dgp=new DatagramPacket(bytes,bytes.length,localhost,8888);
   //发送数据
    dgs.send(dgp);
	
} catch (IOException e) {
	// TODO Auto-generated catch block
	e.printStackTrace();
}
finally {
	dgs.close();
}
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/1fca2cb90cd94b2496e6363703a5ff8f.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

#  

写完之后再看具体的知识点-----

public class DatagramSocket extends Object implements Closeable
此类表示用于发送和接收数据报数据包的套接字。 

> 数据报套接字是**分组传送服务的发送或接收**点。 在数据报套接字上发送或接收的每个数据包都被单独寻址和路由。
> 从一个机器发送到另一个机器的多个分组可以不同地路由，并且**可以以任何顺序到达。**

**UDP传输的特点**

从描述上可以发现UDP传输时的特点:

> **1,分组传送和接收; 
> 2,可以以任何顺序到达----信息发送和接收的顺序可能出现不一致的额请跨国即传输是不可靠的; 
> 3,可以分组分别将信息发送给不同的设备----即是优点也是缺点;**

**构造方法**

  DatagramSocket() 
构造数据报套接字并将其绑定到本地主机上的任何可用端口。 

protected  DatagramSocket(DatagramSocketImpl impl) 
使用指定的DatagramSocketImpl创建一个未绑定的数据报套接字。  

  DatagramSocket(int port) 
构造数据报套接字并将其绑定到本地主机上的指定端口。  

  DatagramSocket(int port, InetAddress laddr) 
创建一个数据报套接字，绑定到指定的本地地址。  

  DatagramSocket(SocketAddress bindaddr) 
创建一个数据报套接字，绑定到指定的本地套接字地址。  

```java
DatagramSocket s = new DatagramSocket(); 
s.bind(new InetSocketAddress(8888));

DatagramSocket s1 = new DatagramSocket(8888);
//这两种情况都将创建一个DatagramSocket，可以在UDP端口8888上接收广播
```

**常用方法----**

void bind(SocketAddress addr) 
将此DatagramSocket绑定到特定的地址和端口。  

void close() 
关闭此数据报套接字。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/7abf0a74494449b7a21a652e786bb829.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

InetAddress getInetAddress() 
返回此套接字连接到的地址。  

InetAddress getLocalAddress() 
获取套接字所绑定的本地地址。  

int getLocalPort() 
返回此套接字绑定到的本地主机上的端口号。  

SocketAddress getLocalSocketAddress() 
返回此套接字绑定到的端点的地址。  

int getPort() 
返回此套接字连接到的端口号。 

boolean isBound() 
返回套接字的绑定状态。  

boolean isClosed() 
返回套接字是否关闭。  

boolean isConnected() 
返回套接字的连接状态。  

void receive(DatagramPacket p) 
从此套接字接收数据报包。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ea21fc7f43e4e9489cdd468cfab7350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

void send(DatagramPacket p) 
从此套接字发送数据报包。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/0220c7b022e34a77b27ac0e36512c6cf.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/java/JavaBase9-23.md#_9抽象类)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage2.png)</div>](java/JavaExam.md#精选练习题精选)

</center>

![](..\pic\div\rabbit.gif)

