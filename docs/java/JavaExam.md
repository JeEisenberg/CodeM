![](..\pic\div\shark.gif)

# JAVA练习题

## 精选练习题精选

1,编写一个程序，定义局部变量sum，并求出1+2+3+…+99+100之和，赋值给sum，并输出sum；

```java
package compare1;
public class Xiti {
/*编写一个程序，定义局部变量sum，
 * 并求出1+2+3+…+99+100之和，
 * 赋值给sum，
 * 并输出sum的值 */
	public static void main(String[] args) {
		
		int  sum = 0;
		for (int i=0;i<101;i++) {
			sum +=i;			
		}
		System.out.println(sum);
	}
}

```

2,编写程序，要求运行后要输出long类型数据的最小数和最大数

```java
package compare1;
public class Shujuleiixng {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		long Long_max =Long.MAX_VALUE;
		long Long_min =Long.MIN_VALUE;
		System.out.println(Long_max);
		System.out.println(Long_min);
	}
}
```

3,改错题：指出错误之处并对其进行修改（本题改编自**2013**年巨人网络的Java程序员笔试题）

```java
package compare1;
public class Shujuleiixng {
        public static void main(String[]args)
        {
        for(int i=Integer.MIN_VALUE;i<=Integer.MAX_VALUE;++i)
     {////死循环，++i会溢出，所以要界定一个真正的范围或者跳出循环
          boolean isEven=(i%2==0);
          if (i ==Integer.MAX_VALUE) {
        	  break;
          }         
          System.out.println(String.format("i=%d,isEven=%b",i,isEven));
          }
        }
       }

```

4.编写程序，计算表达式“（（12345679*9）>（97654321*3））? true : false”的值。 

```java
package compare1;
public class Xiti {
 	public static void main(String[] args) {
//3，编写程序，计算表达式“（（12345679*9）>（97654321*3））? true : false”的值。 		
 
		 long X= 12345679*9;
		 long Y= 97654321*3;
		 
		System.out.println(X>Y?true:false);//三目运算符
	}
}


```

5.编写程序，实现生成一随机字母（a-z，A-Z），并输出，运行结果如下图所示。

例如: double rand = Math.random(); // rand 储存着[0,1) 之间的一个小数⑵ 大写字母A～Z对应整数65 ～ 90、小写字母a～z对应整数97～ 122。

拓展知识：

⑴ Math.random()返回随机 double 值，该值大于等于 0.0 且小于 1.0。

例如: double rand = Math.random(); // rand 储存着[0,1) 之间的一个小数

⑵ 大写字母A～Z对应整数65 ～ 90、小写字母a～z对应整数97～ 122。

```java
package compare1;//此题涉及到  int 与char之间的转换问题。
public class Xiti {
	public static void main(String[] args) {
		 double a =Math.random();
		if(a<0.5) {
		 int b = (int) (52*a);
		 char letter=(char)(65+b);    	
		 System.out.println(letter);
		}	else {
			int b =(int)(52*(1-a));
			char letter=(char)(97+b);
			System.out.println(letter);
		}
	}
}
	
```

6,请分别使用三元运算和位运算实现 

```java
package compare1;
public class Shujuleiixng {
	public static void main(String[] args) {
int a =(int)(Math.random()*56)+65;
		System.out.println("转换前："+(char) a);
		int b= a>97?a-32:a;
		char c =(char)b;
		System.out.println("转换后:"+c);
	}
}
```

```java
	public static void main(String[] args) {
int a = (int) (65 + Math.random() * 58);//取得随机字母（包含特殊符号，要去掉）
		if(a!=91&&a!=92&&a!=93&&a!=94&&a!=95&&a!=96) {
		System.out.println("转换前的字母是:"+(char )a);
		if(a<=90) {
			System.out.println("转换后的字母是:"+(char )a);
		}else {
			char c=(char) ( (char )a & 0xdf);//0xdf常用于消除英文字母大小写区别，即输入小写字母那么会处理成大写。
		
		System.out.println("转换后的字母是:"+(char )c);
		}
		}
	}
}

```

7,编写程序，使用程序产生1-12之间的某个整数（包括1和12），然后输出相应月份的天数（2月按28天算)

```java
public static void main(String[] args) {
		// TODO Auto-generated method stub
    int month= (int) Math.random()*12+1;
    System.out.println("随机产生一个月份并显示月份: " + month);
    switch(month) {
    case 1:System.out.println(month+"月有31天");
    break;
    case 3:System.out.println(month+"月有31天");
    break;
    case 5:System.out.println(month+"月有31天");
    break;
    case 7:System.out.println(month+"月有31天");
    break;
    case 8:System.out.println(month+"月有31天");
    break;
    case 10:System.out.println(month+"月有31天");
    break;
    case 12:
    	System.out.println(month+"月有31天");
    break;
    
    case 4:System.out.println(month+"月有30天");
    break;
    case 6:System.out.println(month+"月有30天");
    break;
    case 9:System.out.println(month+"月有30天");
    break;
    case 11:System.out.println(month+"月有30天");
    break;
    case 2:System.out.println(month+"月有28天");
    break;
    default:System.out.println("exit");
    break;
	}
	}
	//利用穿透性可以简化一下代码,该怎么做呢?
```

8,编写程序，判断某一年是否是闰年。 

```java
public class Runnian {
	public static void main(String[] args) {
		int x = 2000;
		if (x%4==0&x%100!=0) {
			System.out.println(x+"年是闰年");
			
		}else if(x%400==0) {
			System.out.println(x+"年是闰年");
		}
		else {
			System.out.println(x+"年不是闰年");
		}
	}
}
```

9,编写程序，对int[] a = {25, 24, 12, 76, 98, 101, 90, 28}数组进行排序

```java
package student;
public class Maopao {
////////////////////////冒泡排序法--升序/////////////////////
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int a[] = { 10, 6, 15, 14, 28, 3, 8, 45 };// 定义一个数组
		System.out.println("排序前的为:" + "\n" + "10,6,15,14,28,3,8,45");
		System.out.println("排序后的为:");
		for (int i = 0; i < a.length; i++) {
			for (int j = i + 1; j < a.length - 1; j++) {
				if (a[i] > a[j]) {// 比较相邻的两个大小，
					int temp = a[i];// 将两个中较大的临时保存
					a[i] = a[j];// 将小的提前
					a[j] = temp;
				}
			}
			System.out.print(a[i] + ",");
		}
	}
}


```

10,用Arrays类进行快速排序

```java
import java.util.Arrays;
public class Paixu {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int a[] = { 10, 6, 15, 14, 28, 3, 8, 45 };
		Arrays.sort(a);
		for (int i = 0; i < a.length; i++) {
			System.out.print("排序后的为:"+a[i]+"、");
		}
	}
}
```

11,编写程序，将上述算法稍加改写，将排序算法改成“乱序算法”。两次运行结果分别如下所示。（提示：所谓"乱序"，是跟"排序"相反，乱序为了增加随机性，乱序在生活中模拟随机出现的事件中有很大的应用价值。

```java
package student;
import java.util.Random;
public class Luanxupaixu {//父类
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int a[] = { 12, 15, 8, 78, 23, 45, 23, 1, 7 };
		System.out.println("原数组为:");
		for (int i = 0; i < a.length; i++) {
			System.out.print(a[i] + ",");
		}
		for (int i = 0; i < a.length; i++) {
			Random random = new Random();
			int j = random.nextInt(8);//random.nextInt(8)取0到8之间的整数
			int temp = a[i];
			a[i] = a[j];
			a[j] = temp;
		}
		System.out.println("\n" + "排序后的数组为:");
		printArr(a);
	}
	public static void printArr(int[] a) {//静态方法
		for (int i : a) {
			System.out.print(i + ",");		
	}
	} 
 }    
```

12，求数组中的最大值和最小值 

```java
public class Shuzuzu {
	public static void main(String[] args) {
		// 求数组中的最大值和最小值
		int a[] = { 74, 48, 30, 17, 62 };
		int max = a[0];
		int min = a[0];
		for (int i = 0; i < a.length; i++) {
			System.out.println("数组中的元素有:" + "a[" + i + "]=" + a[i]);
			if (a[i] >= max) {
				max = a[i];
			} else if (a[i] < min) {
				min = a[i];
			}
		}
		System.out.println("数组中最大值是:" + max);
		System.out.println("数组中最小值是:" + min);

```

13,给出7种颜色，随机选择哪一种颜色；

```java
package lianxi;
import java.util.Random;
class Color {//类不能声明为（private）私有化，
static String color []= {"红","橙","黄","绿","蓝","靛","紫"};
  ////////////////方法一///////////////
void getAll() {//这里为默认权限，只允许在同一个包中访问该方法	  
	  for(int i =0;i<color.length;i++) {		  
			 System.out.println(color[i]);		
	  }	
  }
/////////////////方法二//////////////////////////
	  public void select() {//该方法在别的类中、包中也可以被访问；
          Random random = new Random();//在直接使用math.random时总是会出现0，
          int j = random.nextInt(8);
		  //int j=(int) Math.random()*(color.length);
		  System.out.println(color[j]);
	  }
public static void main(String args[]) {
	Color col=new Color();//对象实例化
	System.out.println("七种颜色:"); 
	col.getAll();//调用方法
	System.out.println("随机选择一种颜色:"); 
	col.select();//调用方法
}
}

```

14， 定义一个包含name,age和like属性的Person类，实例化并给对象赋值，然后输出对象属性。

```java
apackage xuexi;
class Person {
	String name;
	int age;
	String like;
	public Person(String name, int age, String like) {
		this.age = age;
		this.name = name;
		this.like = like;
	}
	public String like() {
		return "我是" + this.name + "今年" + this.age + "岁," + "我喜欢吃的水果是" + this.like;
	}
}
public class Xiti {
	public static void main(String args[]) {
		Person P[] = { new Person("张三", 12, "西红柿"), new Person("李四", 18, "苹果"), new Person("王五", 20, "香蕉") };
		for (int i = 0; i < P.length; i++) {
			System.out.println(P[i].like());
		}
	}
}
```

15，定义一个book类，包括属性title（书名）和price（价格），并在该类中定义一个方法printInfo()，来输出这2个属性。然后再定义一个主类，其内包括主方法，在主方法中，定义2个book类的实例bookA和bookB，并分别初始化title和price的值。然后将bookA赋值给bookB，分别调用printInfo(),查看输出结果并分析原因。

```java
 class Book{
	String Title;
	double Price=102.5;
	double Discount=100.6;
	public Book(String T,double P,double E) {
		this.Title=	T;
		this.Price=P;
		this.Discount=E;
	}
	public String printInfo() {
		return"|书的名字是:"+this.Title+"\n"+
	          "|"+"零售价格"+this.Price+"￥"+"\n"+
			  "|"+"打折后是"+this.Discount+"￥"+"\n";
	}
}
public class D2t {
	public static void main(String args[]) {
  Book  book1=new Book("《java》",65.2,58.4);
  Book book2 =new Book("《幻城》",48.2,46.5);
  book2 =book1;
  double sum=book1.Discount+book2.Discount;
  System.out.println(book1.printInfo());
  System.out.println(book2.printInfo());
  System.out.println("您共消费:"+sum);
}
}
```

16，定义一个book类，包括属性title（书名）、price（价格）及pub（出版社），pub的默认值是“天天精彩出版社”，并在该类中定义方法getInfo()，来获取这三个属性。再定义一个公共类BookPress，其内包括主方法。在主方法中，定义3个book类的实例b1，b2和b3，分别调用各个对象的getInfo()方法，如果“天天精彩出版社”改名为“每日精彩出版社”，请在程序中实现实例b1，b2和b3的pub改名操作。完成功能后，请读者思考一下，如果book类的实例众多，有没有办法优化这样的批量改名操作？

```java
class Book {
	String Title;
	double Price;
	String Pub = "天天精彩出版社";
	public Book(String Title, double Price, String Pub) {
		this.Title = Title;
		this.Price = Price;
		this.Pub = Pub;
	}
	String getInfo() {
		return "书名:" + "《" + this.Title + "》" + "\n" + "|" + "建议零售价:" + this.Price + "￥" + "|" + "出版社是:" + this.Pub;
	}
}
public class BookPress {
	public static void main(String args[]) {
		///////////方式一//////////
//		Book b1 = new Book("java", 47.8, "天天精彩出版社");
//		Book b2 = new Book("C++", 56.8, "每日精彩出版社");
//		Book b3 = new Book("C##", 66.8, "每日精彩出版社");
//		b2.Pub = b1.Pub;
//		System.out.println(b1.getInfo());
//		System.out.println(b2.getInfo());
//		System.out.println(b3.getInfo());
		//////////////////////////方式二/////////////////
Book b[]= {new Book("java", 47.8, "天天精彩出版社"),new Book("C++", 56.8, "天天精彩出版社"),new Book("C##", 66.8, "天天精彩出版社")};
    for(int i=0;i<b.length;i++) {
    	
    	if (i==0) {
    		b[i].Pub="每日精彩出版社";	
    	}else if(i%2!=0){
    		b[i].Pub="我爱我家出版社";   		
    	}
    	System.out.println(b[i].getInfo());
    }

	}
}


```

17，编程实现，现在有如下的一个数组。
        int oldArr[]={1,3,4,5,0,0,6,6,0,5,4,7,6,7,0,5} ;
要求将以上数组中值为0的项去掉，将不为0的值存入一个新的数组，生成的新数组为int newArr[]={1,3,4,5,6,6,5,4,7,6,7,5} 

```java
package xuexi;
    class Chengji {
	void PrintArr(int[] Arr) {
		System.out.println("原数组为");
		for (int i = 0; i < Arr.length; i++) {
			System.out.print(Arr[i] + ",");
		}
		System.out.println();
	}
int[] Sorty(int[] Arr) {
		System.out.println("剔除0后");
		for (int i = 0; i < Arr.length; i++) {
			int j = 0;
			if (Arr[i] != 0) {
				Arr[j] = Arr[i];
				j++;
			} else {
				i++;
			}
			System.out.print(Arr[i] + ",");
		}
		return Arr;
	}
}

public class D18t {
	public static void main(String[] args) {
		int[] oldArr = { 1, 3, 4, 5, 0, 0, 6, 6, 0, 5, 4, 7, 6, 7, 0, 5 };
		Chengji CJ = new Chengji();
		CJ.PrintArr(oldArr);
		CJ.Sorty(oldArr);
		int []B=CJ.Sorty(oldArr);
			}
}
```

18,编程实现，要求程序输出某两个整数之间的随机数。

```java
package xuexi;
import java.util.Random;
import java.util.Scanner;
public class D19t {
	public static void main(String[] args) {
		System.out.println("请输入一个数整数");
Scanner S1=new Scanner(System.in);//输入一个数
int M;
M=S1.nextInt();
Random random =new Random();
System.out.println(random.nextInt(M));
S1.close();
	}
}

```

19.老师在取得了五位同学考试成绩，但是在系统中只输入了四位同学的成绩，要求老师再次输入成绩，返回结果为五位同学成绩按照降序进行排列；
代码1：

```java
import java.util.Arrays;
import java.util.Scanner;
public class Charu {
	public static void main(String[] args) {
		int ss[] = new int[5];
		ss[0] = 120;
		ss[1] = 108;
		ss[2] = 128;
		ss[3] = 116;
		int score;// 要插入的成绩
		System.out.println("请输入要插入的学生数学成绩：");
		System.out.println(System.in);
		Scanner input = new Scanner(System.in);
		score = input.nextInt();
		int temp;
		//冒泡排序法找到前五名的成绩，
		for (int i = 1; i < ss.length; i++) {// 控制比较轮数
			for (int j = 0; j < ss.length - i; j++) {// 控制比较个数
				if (ss[j] <= ss[j + 1]) {
					temp = ss[j];
					ss[j] = ss[j + 1];
					ss[j + 1] = temp;
				}
			}
		}
int index=ss.length-1;
for(int i=0;i<ss.length;i++) {
	if (score >=ss[i]) {
		index=i;
		break;
	}
}
	for(int i=ss.length-1;i>index;i--) {
		ss[i]=ss[i-1];
	}
		ss[index]=score;
		System.out.println("前五名成绩为：");
		for (int i : ss) {
			System.out.print(i + ",");
		}
		input.close();//在用完scanner后要将其关闭，否则会占用运行空间
}}
/**	思考，如果调用Arrays类怎么操作？
		  Arrays.fill(数组名,var)
		  Arrays.fill(数组名,位置1,位置2插入的元素)
		  表示从位置1开始插入一个元素，到位置2结束（包含位置1，不包含位置2）
		 那么我们通过已有的方法能不能去简化上述代码呢？
		 我们需要知道我们在那里插入该成绩，因此我们仍然需要去比较大小
		 在跟要插入的数比较大小之前我们可以借助Arayys类去快速排序(升序)
		 因为要插入一个数，我们的数组长度只能是一个动态的，，在原来基础上长度+1，
		  那么原数组在初始化是有一个位置未被赋值，系统会赋予默认值0，
		  在快速排序之后我们得到的一定是开头为0的，那么我们在0的位置插入我们要插入的值，然后我们在进行排序*/
		  
		 

```

```java
package xuexi;
import java.util.Arrays;
import java.util.Scanner;
public class Sikao {
	public static void main(String[] args) {
		int ss[] = new int[5];
		ss[0] = 120;
		ss[1] = 108;
		ss[2] = 128;
		ss[3] = 116;
		int score;
		int temp;
		System.out.println("请输入要插入的学生数学成绩：");
		System.out.println(System.in);
		Scanner input = new Scanner(System.in);
		score = input.nextInt();
		Arrays.sort(ss);
		
	 Arrays.fill(ss, 0, 1, score);
	 Arrays.sort(ss);
	   //System.out.println(Arrays.toString(ss));
		for (int i=1;i<ss.length;i++) {
			for (int j=0;j<ss.length-i;j++) {
				if(ss[j]<ss[j+1]) {
					temp=ss[j];
					ss[j]=ss[j+1];
					ss[j+1]=temp;
				}
			}
		}
		System.out.println("前五名学生的成绩是");
		for (int i : ss) {
			System.out.print(i+"\t");
		}
		input.close();
		}
	}

//跟上述代码相比，我们省略了绿色部分的代码，通过ARRAYS类我们简化了一下下代码

```

20,判断电子邮件地址是否合法：

```java
public class Yu {
	public static void main(String[] args) {
		String str ="1234123asdsADCsad44569_";
		String regex ="\\d+";
		System.out.println(str.matches(regex));//判断是否为数字
		String zimu="abcABDdefghigklmn";
	System.out.println(zimu.matches("[a-zA-Z]+"));//判断是否为字母
		System.out.println(str.matches("\\w+"));//字母数字下划线
		String i="casdsadsdsad1232`";
		System.out.println(i.matches(".+"));//匹配任意字符
		System.out.println(str.replaceAll("[a-z]", ""));
		String mail ="linchunyu129@163.com";
		String Email ="xiaonuan@sina.net";
		String emailAddress="hanppaynewyear123@126.net";
		String reg ="\\w+@\\w+.((com)|(net.cn)|(net))";
		System.out.println(mail.matches(reg));
		System.out.println(Email.matches(reg));
		System.out.println(emailAddress.matches(reg));
	}
}
```

21，分别以一下的形式输出当前时间：
2021-02-21    
2 0 2 1 - 0 2 - 2 1
2021年2月21日  
2021年2月21日  16时40分123毫秒 

```java
import java.util.Calendar;
import java.util.GregorianCalendar;
 class Times{
	 StringBuffer str=new StringBuffer();
	 Calendar calendar =new GregorianCalendar();
	public String getTimes() {//2021.02.21
	str.append(calendar.get(Calendar.YEAR)).append("-");
		str.append(this.addZero((calendar.get(Calendar.MONTH)+1),2)).append("-");
		str.append(this.addZero(calendar.get(Calendar.DAY_OF_MONTH),2));
		return str.toString();
		
}
	private String addZero(int temp,int len) {//传0的操作不希望被外部看见，所以
		StringBuffer buffer = new StringBuffer();
		buffer.append(temp);
		while (buffer.length()<len) {
			buffer.insert(0, 0);
		}
		return buffer.toString();
	}
}
public class Yu {
	public static void main(String[] args) {
	Times time =new Times();
	System.out.println(time.getTimes());
	
	}
}

```

22,给定字符串执行以下操作：
字符串的长度
"第一个字符是
最后一个字符是
第一个单词
crazy的位置

```java
public class Yu {
	public static void main(String[] args) {
		String name ="My name is Networkcrazy";
		System.out.println("字符串的长度"+name.length());
		System.out.println("第一个字符是"+name.charAt(0));
		System.out.println("最后一个字符是"+name.charAt(name.length()-1));
		System.out.println("第一个单词"+name.substring(0, 2));
		System.out.println(name.contains("crazy"));
		System.out.println("crazy的位置"+name.indexOf("crazy", 0));
	}
}
```

23,此目录的所有文件路径包括各子文件夹的中的文件也都要列出

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
					lists(files[i]);
				}
			}
			System.out.println(file);

		}
	}
}


```

24,编写一段程序，使用ArrayList类存储以下元素：“one”、“two”、“three”、“four”，并通过Iterator迭代输出ArrayList中的内容。

```java
import java.util.*;
public class Study {
	public static void main(String[] args) {
		List<String> list = new ArrayList<>();
		list.add("one");		list.add("two");
		list.add("three");		list.add("four");
		Iterator<String> it = list.iterator();
		while (it.hasNext()) {
			System.out.println(it.next());
		}
	}

}
```

25,定义Student类，该类不实现Comparable接口，定义一个Comparator类比较两个Student对象所在班级名称和名字，班级名相同时用名字进行排序，使用List接口（在用TreeSet）观察排序的结果。
猜想--用名字排序的话，是不是TreeSet自动实现的？

```java
import java.util.*;
class Student {
	private int className;// 班级
	private String name;// 学生名字
	public Student(int className, String name) {
		this.className = className;
		this.name = name;	}
	public int getClassName() {
		return className;	}
	public String getName() {
		return name;	}
	@Override
	public String toString() {
		return this.className + "班<-->姓名：" + this.name;	}
	@Override
	public boolean equals(Object obj) {
		if (this == obj) {
			return true;		}
		if (!(obj instanceof Student)) {
			return false;		}
		Student stu = (Student) obj;
		if (this.className == stu.className && this.name.equals(stu.name)) {
			return true;		}
		return false;	}
	@Override
	public int hashCode() {
		return this.className * this.name.hashCode();	}}
class StudentComparator implements Comparator<Student> {
	@Override
	public int compare(Student stu0, Student stu1) {
		if (stu0.getClassName() > stu1.getClassName()) {
			return 1;		}
		if (stu0.getClassName() < stu1.getClassName()) {
			return -1;		}
		return 0;	}}

```

```java
public class Study {
	public static void main(String[] args) {
		//Set<Student>students = new TreeSet<Student>();//出现类转换异常，
		List<Student> students = new ArrayList<>();
		students.add(new Student(1, "张飞"));
		students.add(new Student(4, "赵子龙"));
		students.add(new Student(1, "李二狗"));
		students.add(new Student(3, "刘备"));
		students.add(new Student(1, "孙权"));
		students.add(new Student(2, "周仓"));
		students.add(new Student(5, "诸葛亮"));
		students.add(new Student(3, "吕蒙"));
		students.add(new Student(2, "李典"));
		Student[] stu = students.toArray(new Student[] {});
		Arrays.sort(stu,new StudentComparator());//定义了排序方法
		for (int i = 0; i < stu.length; i++) {
			System.out.println(stu[i]);		}	}}

/**为什么用TreeSet时定义在类外会产生转换异常？

Set<Student> students = new TreeSet<Student>(new StudentComparator());

需要在这里指定排序规则，否则会出现类转换异常*/


```

```java
public boolean equals(Object obj) {
		if (this == obj) {
			return true;		}
		if (!(obj instanceof Student)) {
			return false;		}
		Student stu = (Student) obj;
		if (this.className == stu.className && this.name.equals(stu.name)) {
			return true;		}
		return false;	}
	@Override
	public int hashCode() {
		return this.className * this.name.hashCode();	}}
class StudentComparator implements Comparator<Student> {
	@Override
	public int compare(Student stu0, Student stu1) {
		if (stu0.getClassName() > stu1.getClassName()) {
			return 1;		}
		if (stu0.getClassName() < stu1.getClassName()) {
			return -1;
		} else if (stu0.getClassName() == stu1.getClassName()) {
			if (stu0.hashCode() > stu1.hashCode()) {
				return 1;
			} else if (stu0.hashCode() < stu1.hashCode()) {
				return -1;
			} else {
				return 0;}}
		return 0;}}
public class Study {
	public static void main(String[] args) {
		Set<Student> students = new TreeSet<Student>(new StudentComparator());
		// List<Student> students = new ArrayList<>();
		students.add(new Student(1, "张飞"));
		students.add(new Student(4, "赵子龙"));
		students.add(new Student(1, "李二狗"));
		students.add(new Student(3, "刘备"));
		students.add(new Student(1, "孙权"));
		students.add(new Student(2, "周仓"));
		students.add(new Student(5, "诸葛亮"));
		students.add(new Student(3, "吕蒙"));
		students.add(new Student(2, "李典"));
		Student[] stu = students.toArray(new Student[] {});
		Arrays.sort(stu, new StudentComparator());// 定义了排序方法
		for (int i = 0; i < stu.length; i++) {
			System.out.println(stu[i]);		}	}}


```

26,定义一个Student类（包含班级和姓名属性），然后定义多个StudentItem对象，并以姓名为键，对象为值添加到HashMap中，分别实现对该集合的查看、修改和删除。

```java
import java.util.*;
class Student {
	private int className;// 班级名字
	private String name;// 学生名字
	public Student(int className, String name) {
		this.className = className;
		this.name = name;	}
	public int getClassName() {
		return className;	}
	public String getName() {
		return name;	}
	@Override
	public String toString() {
		return this.className + "班<-->姓名：" + this.name;	}}
public class Study {
	public static void main(String[] args) {
		Map<String, Student> students = new HashMap<>();
		students.put("张飞", new Student(1, "张飞"));
		students.put("赵子龙", new Student(4, "赵子龙"));
		students.put("李二狗", new Student(1, "李二狗"));
		students.put("刘备", new Student(3, "刘备"));
		students.put("孙权", new Student(1, "孙权"));
		students.put("周仓", new Student(2, "周仓"));
		students.put("诸葛亮", new Student(5, "诸葛亮"));
		students.put("吕蒙", new Student(3, "吕蒙"));
		students.put("李典", new Student(2, "李典"));
		students.replace("李二狗", new Student(1, "李二狗"),new Student(6, "李狗") );//替换
		students.put("张飞", new Student(2,"张飞"));//信息修改
		students.replace("孙权", new Student(2, "孙权"));
		students.remove("刘备");
		Set<Map.Entry<String, Student>> set = students.entrySet();
		Iterator<Map.Entry<String, Student>> it = set.iterator();
		while (it.hasNext()) {
			Map.Entry<String, Student> me = (Map.Entry<String, Student>) it.next();
			System.out.println(me.getKey() + "的信息----->" + me.getValue());		}		}
}


```

27,读取与写入数据

```java
import java.io.*;
public class Xieru {
	public static void main(String[] args) throws IOException {
		File file = new File("F:" + File.separator + "Write.doc");// 指定文件
		RandomAccessFile ra = new RandomAccessFile(file, "rw");// 开始读写操作
		System.out.println("正在写入第一组数据。。。。。");
		String name0 = "zhangsan  ";
		int age0 = 18;
		ra.writeBytes(name0);// 将String以字节的形式写入
		ra.write(age0);
		System.out.println("第一组数据写入完成");
		System.out.println("正在写入第二组数据。。。。。");
		String name1 = "lisi      ";
		int age1 = 23;
		ra.writeBytes(name1);// 将String以字节的形式写入
		ra.write(age1);// 写入数字
		System.out.println("第二组数据写入完成");
		String name2 = "wangerxiao";
		int age2 = 19;
		ra.writeBytes(name2);// 将String以字节的形式写入
		ra.write(age2);// 写入数字
		System.out.println("数据写入完成");
		ra.close();
	}
}

```

```java
import java.io.File;
import java.io.RandomAccessFile;
public class Duqu {
	@SuppressWarnings({ "resource", "unused" })
	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
		File file = new File("F:" + File.separator + "Write.doc");// 指定文件
		RandomAccessFile ra = new RandomAccessFile(file, "r");// 开始读写操作
		String name = null;
		int age = 0;
		byte b[] = new byte[10];
				System.out.println("第三个人的信息--");
		ra.skipBytes(22);
		for (int i = 0; i < 10; i++) {
			b[i] = ra.readByte();
		}
		age=ra.read();
		System.out.println("\t|-姓名:"+new String(b)+"年龄:"+age);
		System.out.println("第一个人的信息--");
		ra.seek(0);
		for (int i = 0; i < 10; i++) {
			b[i] = ra.readByte();
		}
		age=ra.read();
		System.out.println("\t|-姓名:"+new String(b)+"年龄:"+age);
		System.out.println("第二个人的信息--");
//		ra.skipBytes(12);
		for (int i = 0; i < 10; i++) {
			b[i] = ra.readByte();
		}
		age=ra.read();
		System.out.println("\t|-姓名:"+new String(b)+"年龄:"+age);
	}

}


```

28,编写一个多线程处理的程序，其他线程运行10秒后，使用main方法中断其他线程。

```java
class Book implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {
            try {
                Thread.sleep(10000);

            } catch (Exception ex) {
                System.out.println("wobeizhongdaunle");
            }

            System.out.println(i + "---" + Thread.currentThread().toString() + "Running");
        }
    }
}

public class Duoxiancheng implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 20; i++) {

            System.out.println(Thread.currentThread().toString() + "Running");     }    }

    public static void main(String[] args) {
        Book b = new Book();
        Duoxiancheng d = new Duoxiancheng();
        Thread t = new Thread(b);
        t.start();
        Thread tt = new Thread(d);
        tt.start();
        try {
            Thread.sleep(1000);

        } catch (Exception ex) {
            System.out.println("wobeizhongdaunle");
        }
        t.interrupt();    }}

```

29,建立两个线程之间的通讯

```java
import java.io.*;
class Sender extends Thread {// 发送数据
    private PipedOutputStream out = new PipedOutputStream();// 实例化管道输出流，产生数据，向管道发送数据
    public PipedOutputStream getOutputStream() {
        return out;   }

    public void run() {
        String str = "你好，世界！！！！";
        System.out.println("我要发送的内容是--" + str);
        try {
            out.write(str.getBytes());
            out.close();
        } catch (Exception ex) {
            ex.printStackTrace();    }    }}
class Receiver extends Thread {
    PipedInputStream in = new PipedInputStream();
    public PipedInputStream getInputStream() {
        return in;    }
    public void run() {
        byte b[] = new byte[1024];// 开辟空间接收数据
        try {
            int len = in.read(b);
            System.out.println("我接收到的数据是--");
            System.out.println(new String(b, 0, len));
            in.close();
        } catch (Exception e) {
            e.printStackTrace();    }   }}
public class Test {
    public static void main(String[] args) {
        try {
            Sender send = new Sender();
            Receiver rec = new Receiver();
            PipedOutputStream out = send.getOutputStream();
            PipedInputStream in = rec.getInputStream();
            in.connect(out);
            send.start();
            rec.start();
        } catch (Exception ex) {
            ex.printStackTrace();     }    }}

```

30, 列出给定任意文件（夹）下所有的.java文件 

```java
/**
 * 列出给定任意文件（夹）下所有的.java文件
 */
import java.io.File;
public class FileDemoed {
	public static void main(String[] args) {
		File file = new File("E:\\Login");// 指定文件夹
		Lists(file);	}
	public static void Lists(File file) {
		if (!file.exists()) {// 首先判断文件（夹）是否存在,不存在，就不继续往下运行了
			System.out.println("文件不存在");
		} else {// 存在的话，还要继续判断
			/**
			 * 如果先判断是目录的话， 先判断该目录是否为空目录 不为空，则列出文件夹下所有内容，如果文件夹下还有其他文件夹，则继续往下
			 * 如果先判断是文件的话，则要找到父类目录，然后列出父类目录下的所有内容
			 * 			 */
			if (file.isDirectory()) {// 如果是目录，则列出
				File files[] = file.listFiles();
				if (files != null) {
					for (int i = 0; i < (int) files.length; i++) {
						Lists(files[i]);
						if (files[i].getName().matches("\\w+.txt")) {
							System.out.println(files[i]);}}}}}}}

```

31,序列化多个对象

```java
import java.io.*;
class Book implements Serializable {
	private static final long serialVersionUID = 1L;
	private String title;// 书名
	private float price;// 价格
	private String publish;// 出版社
	public Book(String title, String publish, float price) {
		this.title = title;
		this.publish = publish;
		this.price = price;	}
	public String toString() {
		return "书名-《" + this.title + "》" + 
	"\n|"+ "出版社--" + this.publish + "出版社" + "\n|"
				+ "建议零售价" + this.price + "元";	}}
public class Duogexuliehua {
	public static void main(String[] args) throws Exception {
		// 如果要序列化多个对象，则就需要用到数组，即将数组作文一个整体先传进
		Book books[] = { new Book("围城", "边城出版社", 18.8f), new Book("西游记", "山东出版社", 28.2f),
				new Book("Java入门", "电子科技出版社", 68.6f) };
		set(books);
		Book boo[] = (Book[]) Dset();
		print(boo);	}
	public static void set(Object obj[]) throws Exception {
		// ------序列化----
		File file = new File("SerializBook");// 指定存储文件
		OutputStream out = new FileOutputStream(file);
		ObjectOutputStream oos = new ObjectOutputStream(out);
		oos.writeObject(obj);
		oos.close();	}
	public static Object Dset() throws Exception {
		// ------逆序列化----
		Object temp = null;
		File files = new File("SerializBook");// 指定读取文件
		InputStream in = new FileInputStream(files);
		ObjectInputStream ois = new ObjectInputStream(in);
		temp = ois.readObject();
		ois.close();
		return temp;	}
	public static void print(Book[] books) {
		for (Book b : books) {
			System.out.println(b.toString());}}}


```

32,建立一个简单的服务器端程序；

```java
import java.net.*;
import java.io.*;
public class SeverSockets {
	public static void main(String[] args) {
		ServerSocket sever = null;// 实例化服务端
		Socket client = null;// 实例化客户端
		System.out.println("正在等待连接。。。。。");
		try {
			sever = new ServerSocket(8080);
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} // 开辟8080端口以便于访问
		try {
			client = sever.accept();
		} catch (Exception ex) {
			System.out.println("端口没找到");
		}
		// 表示服务器端接收客户端请求并返回数据
		//// 怎么接收数据并返回数据？
		OutputStream out = null;// 服务器接受的是客户端的输出流
		PrintStream pout = null;// 接收到数据后作出反馈，打印出数据
		try {
			out = client.getOutputStream();
			pout = new PrintStream(out);
		} catch (Exception exception) {
			System.out.println("客户端没有发送请求");
		}

		pout.println("别在该努力奋斗的年纪选择了安逸！！！！");
		pout.close();
		try {

			sever.close();
			client.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();		}	}}

```

33,重写工厂设计模式

```java
package compant;
import javax.swing.*;
import java.awt.*;
interface Huitu {
    public void paint(Graphics g);//定制标准
}
class Oval extends JFrame implements Huitu {
       private static final long serialVersionUID = 1L;
    public Oval() {
        this.setSize(400, 400);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setBackground(Color.BLACK);
        this.setVisible(true);
    }
    @Override
    public void paint(Graphics g) {
        g.setColor(Color.RED);
        g.drawOval(50, 50, 80, 80);    }}
class Conet extends JFrame implements Huitu {
    private static final long serialVersionUID = 2L;
    public Conet() {
        this.setSize(200, 200);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setBackground(Color.BLACK);
        this.setVisible(true);    }
    @Override
    public void paint(Graphics g) {
        g.setColor(Color.RED);
        g.drawRect(50, 50, 60,60);    }}
class Factory {
    Object obj = null;
    public void factory(Object obj) {
        Object obj1 = obj.getClass();
        String classname =  obj1.toString();
        if (classname == null) {
            System.out.println("错误");        }
        if ("Oval".equals(classname)) {
            new  Oval().repaint();
        } else if ("Conet".equals(classname)) {
            Conet c = new Conet();
            c.repaint();        }    }}
public class Painted {
    public static void main(String[] args) {
        Factory fac = new Factory();
        fac.factory(new Oval());    }}
class TestIt
{
    public static void main ( String[] args )
    {
        int[] myArray = {1, 2, 3, 4, 5};
        ChangeIt.doIt( myArray );
        for(int j=0; j<myArray.length; j++)
            System.out.print( myArray[j] + " " );
    }
}
class ChangeIt
{
    static void doIt( int[] z )
    {
        z = null ;
    }
}


```

34,代码实现顺序排序、冒泡排序、顺序查找

```java
import java.util.Arrays;
public class sort_1 {
    public static void main(String[] args) {
  int array[] = new int[]{1, 14, 23, 5, 5, 85, 4, 897, 74, 8, 78, 5, 45, 456, 45, 785, 85, 878978, 45, 748578, 12};//开辟数组
        sortDemo(array);//对它进行排序
        for (int b : array  ) {
            System.out.print(b + ",");        }
        System.out.println();
        int arr[] = {10, 52, 5, 2, 5, 65, 635232, 32, 5621, 56, 21, 6561, 56, 21, 62, 002, 8596, 2, 3, -1};
        sortDemoed(arr);
        for (int a : arr ) {
            System.out.print(a + "-");        }
        System.out.println();
       int index= indexOf(arr,5);
        System.out.println(index);
        int y= Arrays.binarySearch(array,85);//二分法查找元素，一定要最数组进行排序
        System.out.println(y);    }
    //排序方法--选择排序法
    public static void sortDemo(int array[]) {
        for (int i = 0; i < array.length - 1; i++) {//第一个元素
            for (int j = i + 1; j < array.length; j++) {
                if (array[i] > array[j]) {//升序排序
                    int temp = array[j];
                    array[j] = array[i];
                    array[i] = temp;                }            }        }    }
    //冒泡排序
    public static void sortDemoed(int array[]) {
//控制循环了多少次
        for (int i = 0; i < array.length - 1; i++) {
            //控制每一堂的比较发生了多少次
            for (int j = 0; j < array.length - 1 - i; j++) {
                if (array[j] > array[j + 1]) {
                    int temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;            }            }        }    }
//顺序查询
    public static int indexOf(int array[], int element) {//顺序查询
        for (int i = 0; i < array.length; i++) {
            if (array[i] == element) {
                return i;            }        }
        return -1;    }}


```

35,打印出1000之内所有的”水仙花数”，所谓”水仙花数”是指一个三位数，

- 其各位数字立方和等于该数本身。例如：153是一个”水仙花数”，
- 因为153=1的三次方＋5的三次方＋3的三次方。

```java
public class test2 {
    public static void main(String[] args) {
        int a;
        int b;
        int c;
        for (int result = 100; result < 1000; result++) {
            a = result / 100;//百位
            b = result / 10 - 10 * a;//十位
            c = result - 100 * a - 10 * b;//个位
            if (a * a * a + b * b * b + c * c * c == result) {
                System.out.println(result);
            }
        }
    }
}
```

![](..\pic\div\weichatcode.png)

![](..\pic\div\guanzhu.gif)