# JAVA中的字节码

## JAVA中的字节码文件(一)

> 作为一个程序员，写了那么多java代码，我们想过一个java文件编译之后生成的class文件中都包含乃些内容吗？
>
> 说的高大上一点，一个程序员不该满足于会写代码，看懂API，而因该深入代码内部去看底层的东西。。。。

今天我们一起来看----（我也是第一次学习，深入jvm内部去研究一下class文件，同时也有助于我们理解程序是怎样执行的）

先看一段简单的代码

```java
class JvmOne {
	public void say() {
		System.out.println("Hello");
	}
}

public class Test {
	public static void main(String args[]) {
		new JvmOne().say();

	}
}
```

编译之后生成class文件，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709161333846.png)
首先我们来看一下class文件都包含啥-----
class字节码文件也要有一定的规范，详情可见阅读一下jvm规范
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709162737493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

魔数---代表了文件为何种格式的文件，其实每一种类型的文件开头都会有一串字符来确定该文件是何种类型的文件，每种格式文件对应的十六进制文件并不是随机排列的，而是有一定规律的。图像文件最好理解了，每个位置显示什么颜色的像素才能将图像=正确显示出来---
举个例子---jpg文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709173119479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)以上两幅图片用 NotePad打开--

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709173223879.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709173246918.png)
开头字符串是一样的，那为什么不凭借 扩展名呢？因为扩展名很容易被修改，另一个是打开文件后是从头直接翻译的，便于机器能够正确解读文件。

我们接下来再来看 class 文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709173846945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709173858713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
***魔数--magic --前4个字节***
开头为CAFEBABE，说明为class文件，这时候JVM虚拟机才能进行下一步操作，如果不是class文件jvm会直接抛出--  unsupportedClassVersionError

> 高版本的jvm可以兼容低版本的，低版本的运行高版本的可能会出现某些异常。

***版本号--version -魔数后的4个字节***
前两个字节表示次版本号（Minor Version），后两个字节表示主版本号（Major Version）

十六进制     0000003B-------》》  十进制   59，对应 java版本为15.0
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709174837711.png)
版本号对应数据如下----
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709175935976.png)
***常量池***

接着版本号之后是常量池入口，
这也是为什么我们说先加载静态,常量等

常量池中存储数据类型---常量,常量包括字面量和符号引用。
字面量为代码中声明为 final 的常量值，
符号引用如类和接口的全局限定名、字段的名称和描述符、方法的名称和描述符。
常量池整体上分为两部分: 常量池计数器和常量池数据区，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709185644122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
***常量池计数器--***

001C(以JvmOne.class为例)----->28
由于用于统计常量池的大小,因为常量池大小并不是一直不变的,(因为常量池计数器也要在常量池区,所以也会占用一定空间)
虽然计数器显示有28个常量,但是实际只有27个,因为第一个为空,用于不引用任何一个常量的时候,

所以开头前面的信息为---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709191502353.png)

> CAFEBABE                                         0000003B                                   001C
> CAFEBABE                                         0000003B                                   0015



***常量池数据区----***

存放的数据类型---也很容易记忆 constant--常量 +对应的基本数据类型, 引用\方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709185748684.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> (1) CONSTANT_Utf8   用UTF-8编码方式来表示程序中所有的重要常量字符串。这些字符串包括： ①类或接口的全限定名， ②超类的全限定名，③父接口的全限定名， ④类字段名和所属类型名，⑤类方法名和返回类型名、以及参数名和所属类型名。⑥字符串字面值
> 表格式：   tag(标志1：占1byte)       length(字符串所占字节的长度，占2byte)      
> bytes(字符串字节序列)
> (2) CONSTANT_Integer、 CONSTANT_Float、 CONSTANT_Long、 CONSTANT_Double 
> 所有基本数据类型的字面值。比如在程序中出现的1用CONSTANT_Integer表示。3.1415926F用 CONSTANT_Float表示。
>
> 　　表格式：   tag       bytes(基本数据类型所需使用的字节序列)
> (3) CONSTANT_Class  使用符号引用来表示类或接口。我们知道所有类名都以
> CONSTANT_Utf8表的形式存储。但是我们并不知道
> CONSTANT_Utf8表中哪些字符串是类名，那些是方法名。因此我们必须用一个指向类名字符串的符号引用常量来表明。
>
> 　　表格式：   tag       name_index(给出表示类或接口名的CONSTANT_Utf8表的索引)
> (4) CONSTANT_String  同 CONSTANT_Class，指向包含字符串字面值的 CONSTANT_Utf8表。
>
> 　表格式：   tag       string_index(给出表示字符串字面值的CONSTANT_Utf8表的索引)
> (5) CONSTANT_Fieldref 、 CONSTANT_Methodref、
> CONSTANT_InterfaceMethodref     指向包含该字段或方法所属类名的
> CONSTANT_Utf8表，以及指向包含该字段或方法的名字和描述符的 CONSTANT_NameAndType 表
> 　　表格式：   tag       class _index(给出包含所属类名的CONSTANT_Utf8表的索引) 
> name_and_type_index(包含字段名或方法名以及描述符的 CONSTANT_NameAndType表 的索引)
> (6) CONSTANT_NameAndType  指向包含字段名或方法名以及描述符的 CONSTANT_Utf8表
> 表格式：   tag       name_index(给出表示字段名或方法名的CONSTANT_Utf8表的索引) 
> type_index(给出表示描述符的CONSTANT_Utf8表的索引) 　　
> 在Java源代码中的每一个字面值字符串，都会在编译成class文件阶段，形成标志号为8(CONSTANT_String_info)的常量表
> 。 当JVM加载 class文件的时候，会为对应的常量池建立一个内存数据结构，并存放在方法区中。同时JVM会自动为CONSTANT_String_info常量表中的字符串常量的字面值
> 在堆中创建新的String对象(intern字符串对象，又叫将字符串对象入池)。然后把CONSTANT_String_info常量表的入口地址转变成这个堆中String对象的直接地址(常量池解析)。

以上引用部分来源自百度百科---https://baike.sogou.com/v43640045.htm?fromTitle=%E5%B8%B8%E9%87%8F%E6%B1%A0

***字面量和符号引用------>常量池主要存放的内容***

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709195719567.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
有了上面的常量内容,还需要知道他们的描述符---

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709201654925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

***常量类型和结构***
**表一**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709202806855.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)常量池中的每一项都是一个表，其项目类型共有14种,如上表,这十四种项目类型所占空间也不一样,但每一项的第一个字节都是一个标志位tag，标识这一项是哪种类型的常量。
**表二----**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709203553569.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

下面我们做一下常量解读---  JvmOne的class文件前四行如下
CAFEBABE0000003B001C0A0002000307
00040C000500060100106A6176612F6C
616E672F4F626A6563740100063C696E
69743E01000328295609000800090700

CAFEBABE---class文件的开头--用于判断是否是class文件;
0000003B-----版本号  ---59  ---对应java版本15.0;
001C---常量计数器--表示常量的数量(要减1哦);
常量计数器之后是常量了---

常量1,  标志位  tag=0A--->10,到表二对应的10 位置查看--->对应的项目类型为CONSTANT_Methodref_info，即类中方法的符号引用,其结构为表二所示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709204938441.png)

> 这里说一下怎么 看表二的内容---
> tag----表示tag 是标志位，标记是哪一种数据结构tag,
>  index表示索引,说明 CONSTANT_Methodref_info有两个索引,指向相应位置

U2,U2表明后面4个字节都是CONSTANT_Methodref_info的---

第1个索引号 00 02------>指向常量池中的第 1 个数据项.

> 注意，常量池的索引是从 1 开始的，所以这里指向的其实是第 1 个数据项：

第二个索引号 00 03------->指向常量池中的第 2 个数据项

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709211042575.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70#pic_center)
其实就是main方法和类JvmOne 的地址

第二个常量---标志位tag=07------>CONSTANT_Class_info--类的引用(其实是构造方法的引用),一个U2,表明后面有两个字节都是CONSTANT_Class_info的,
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709211425552.png)
0004---->指向常量池中的第 3 个数据项

第三个常量---标志位tag=0C----->12即CONSTANT_NameAndType_info,结构如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709212230467.png)两个U2,表明后面 4个字节都是CONSTANT_NameAndType_info，
0005------>指向常量池中的第4 个数据项.
0006------>指向常量池中的第 5 个数据项.
这里就不一一解读了
再往下可以自己尝试一下解读

下面通过: javap -verbose Test 命令查看 JVM 反编译后的常量池，可以看到反编译结果可以将每一个类型和值都很明确的呈现出来

```c
D:\>javap -verbose Test
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
Classfile /D:/Test.class
  Last modified 2021年7月9日; size 300 bytes
  SHA-256 checksum f61f44aabb28cb7011db3b27f2fcaa43cbb1008f37339178cc95a36ae9c3ca1c
  Compiled from "Test.java"
public class Test
  minor version: 0
  major version: 59
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER   //权限
  this_class: #13                         // Test
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool://常量池
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Class              #8             // JvmOne
   #8 = Utf8               JvmOne
   #9 = Methodref          #7.#3          // JvmOne."<init>":()V
  #10 = Methodref          #7.#11         // JvmOne.say:()V
  #11 = NameAndType        #12:#6         // say:()V
  #12 = Utf8               say
  #13 = Class              #14            // Test
  #14 = Utf8               Test
  #15 = Utf8               Code
  #16 = Utf8               LineNumberTable
  #17 = Utf8               main
  #18 = Utf8               ([Ljava/lang/String;)V
  #19 = Utf8               SourceFile
  #20 = Utf8               Test.java

```

***常量池之后是访问权限***
用两个字节来表示，其标识了类或者接口的访问信息，比如：该Class文件是类还是接口，是否被定义成public，是否是abstract，如果是类，是否被声明成final等等。各种访问标志如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210709212818685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
我们直接跳过常量池,来看访问权限部分--
我们以 Test.class文件为例

![1642860399244](C:\Users\Gavin\AppData\Local\Temp\1642860399244.png)



其值为：0x0021,是0x0020和0x0001的并集，即这是一个Public的类

今天就先到这里,下回在区分析剩下的部分----部分知识来源于网络,感谢各位大佬提供的思想火花;

日拱一兵,只进不退;

## JVM中的字节码文件(二)



java的发展史就不必多说了,现在的java可以说是由跨平台到现在的跨语言,实现了跨越式的发展,jvm功不可没呀,现在的Jvm有很多版本,

> Hotspot  官方版本 Jrockit 被Oracle收购了,合并在Hotspot中
>  J9 IBM公司的开发的 Microsoft  
> VM TaoBao VM   hotspot淘宝深度定制版(阿里官方对java有很高的要求)
> LiqudVm--直接针对硬件层面的虚拟机,运行速度快
>  azul zing  --垃圾回收机制很棒.

但是目前我们用的最多的时hotspot,可能因为免费吧.

java之所以能够实现跨平台是因为class文件,各平台可以直接将clas文件拿过来用,不用再次编译了,现在也有很多语言经过编译器也能生成字节码文件然后放在jvm中运行.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210719220945507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
所以很有必要研究以下class文件---上一篇简单说了一下class文件,今抽空详细写了一下class文件的具体信息,为了简单起见,没有写很复杂的代码---

```java
import java.util.Scanner;

public class Learn {
    public static void main(String[] args) {

            System.out.println("请输入三个系数");
            Scanner sc= new Scanner(System.in);
            double a= sc.nextDouble();
            double b= sc.nextDouble();
            double c= sc.nextDouble();
            double d=b*b-4*a*c;
            if(d>=0){
                double x=((-b+Math.sqrt(d))/(2*a));
                double y=((-b-Math.sqrt(d))/(2*a));
                System.out.println(x+"------"+y);
            }else{
                System.out.println("无根");
            }
        }
    }

```

在控制台查看

```java
D:\>javap -v Test
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
Classfile /D:/Test.class
  Last modified 2021年10月1日; size 1285 bytes
  SHA-256 checksum db64f2d5abb23dc63cdc7dfb9da54abc3f7da6f55fdb5b99cac0bb4ca0c4d17d
  Compiled from "Test.java"
public class Test
  minor version: 0
  major version: 60
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #50                         // Test
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 3
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Fieldref           #8.#9          // java/lang/System.out:Ljava/io/PrintStream;
   #8 = Class              #10            // java/lang/System
   #9 = NameAndType        #11:#12        // out:Ljava/io/PrintStream;
  #10 = Utf8               java/lang/System
  #11 = Utf8               out
  #12 = Utf8               Ljava/io/PrintStream;
  #13 = String             #14            // 请输入三个系数
  #14 = Utf8               请输入三个系数
  #15 = Methodref          #16.#17        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #16 = Class              #18            // java/io/PrintStream
  #17 = NameAndType        #19:#20        // println:(Ljava/lang/String;)V
  #18 = Utf8               java/io/PrintStream
  #19 = Utf8               println
  #20 = Utf8               (Ljava/lang/String;)V
  #21 = Class              #22            // java/util/Scanner
  #22 = Utf8               java/util/Scanner
  #23 = Fieldref           #8.#24         // java/lang/System.in:Ljava/io/InputStream;
  #24 = NameAndType        #25:#26        // in:Ljava/io/InputStream;
  #25 = Utf8               in
  #26 = Utf8               Ljava/io/InputStream;
  #27 = Methodref          #21.#28        // java/util/Scanner."<init>":(Ljava/io/InputStream;)V
  #28 = NameAndType        #5:#29         // "<init>":(Ljava/io/InputStream;)V
  #29 = Utf8               (Ljava/io/InputStream;)V
  #30 = Methodref          #21.#31        // java/util/Scanner.nextDouble:()D
  #31 = NameAndType        #32:#33        // nextDouble:()D
  #32 = Utf8               nextDouble
  #33 = Utf8               ()D
  #34 = Double             4.0d
  #36 = Methodref          #37.#38        // java/lang/Math.sqrt:(D)D
  #37 = Class              #39            // java/lang/Math
  #38 = NameAndType        #40:#41        // sqrt:(D)D
  #39 = Utf8               java/lang/Math
  #40 = Utf8               sqrt
  #41 = Utf8               (D)D
  #42 = Double             2.0d
  #44 = InvokeDynamic      #0:#45         // #0:makeConcatWithConstants:(DD)Ljava/lang/String;
  #45 = NameAndType        #46:#47        // makeConcatWithConstants:(DD)Ljava/lang/String;
  #46 = Utf8               makeConcatWithConstants
  #47 = Utf8               (DD)Ljava/lang/String;
  #48 = String             #49            // 无根
  #49 = Utf8               无根
  #50 = Class              #51            // Test
  #51 = Utf8               Test
  #52 = Utf8               Code
  #53 = Utf8               LineNumberTable
  #54 = Utf8               main
  #55 = Utf8               ([Ljava/lang/String;)V
  #56 = Utf8               StackMapTable
  #57 = Class              #58            // "[Ljava/lang/String;"
  #58 = Utf8               [Ljava/lang/String;
  #59 = Utf8               SourceFile
  #60 = Utf8               Test.java
  #61 = Utf8               BootstrapMethods
  #62 = MethodHandle       6:#63          // REF_invokeStatic java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
  #63 = Methodref          #64.#65        // java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
  #64 = Class              #66            // java/lang/invoke/StringConcatFactory
  #65 = NameAndType        #46:#67        // makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
  #66 = Utf8               java/lang/invoke/StringConcatFactory
  #67 = Utf8               (Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
  #68 = String             #69            // \u0001------\u0001
  #69 = Utf8               \u0001------\u0001
  #70 = Utf8               InnerClasses
  #71 = Class              #72            // java/lang/invoke/MethodHandles$Lookup
  #72 = Utf8               java/lang/invoke/MethodHandles$Lookup
  #73 = Class              #74            // java/lang/invoke/MethodHandles
  #74 = Utf8               java/lang/invoke/MethodHandles
  #75 = Utf8               Lookup
{
  public Test();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 3: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=8, locals=14, args_size=1
         0: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #13                 // String 请输入三个系数
         5: invokevirtual #15                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: new           #21                 // class java/util/Scanner
        11: dup
        12: getstatic     #23                 // Field java/lang/System.in:Ljava/io/InputStream;
        15: invokespecial #27                 // Method java/util/Scanner."<init>":(Ljava/io/InputStream;)V
        18: astore_1
        19: aload_1
        20: invokevirtual #30                 // Method java/util/Scanner.nextDouble:()D
        23: dstore_2
        24: aload_1
        25: invokevirtual #30                 // Method java/util/Scanner.nextDouble:()D
        28: dstore        4
        30: aload_1
        31: invokevirtual #30                 // Method java/util/Scanner.nextDouble:()D
        34: dstore        6
        36: dload         4
        38: dload         4
        40: dmul
        41: ldc2_w        #34                 // double 4.0d
        44: dload_2
        45: dmul
        46: dload         6
        48: dmul
        49: dsub
        50: dstore        8
        52: dload         8
        54: dconst_0
        55: dcmpl
        56: iflt          111
        59: dload         4
        61: dneg
        62: dload         8
        64: invokestatic  #36                 // Method java/lang/Math.sqrt:(D)D
        67: ldc2_w        #42                 // double 2.0d
        70: dload_2
        71: dmul
        72: ddiv
        73: dadd
        74: dstore        10
        76: dload         4
        78: dneg
        79: dload         8
        81: invokestatic  #36                 // Method java/lang/Math.sqrt:(D)D
        84: ldc2_w        #42                 // double 2.0d
        87: dload_2
        88: dmul
        89: ddiv
        90: dsub
        91: dstore        12
        93: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
        96: dload         10
        98: dload         12
       100: invokedynamic #44,  0             // InvokeDynamic #0:makeConcatWithConstants:(DD)Ljava/lang/String;
       105: invokevirtual #15                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
       108: goto          119
       111: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
       114: ldc           #48                 // String 无根
       116: invokevirtual #15                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
       119: return
      LineNumberTable:
        line 6: 0
        line 7: 8
        line 8: 19
        line 9: 24
        line 10: 30
        line 11: 36
        line 12: 52
        line 13: 59
        line 14: 76
        line 15: 93
        line 16: 108
        line 17: 111
        line 19: 119
      StackMapTable: number_of_entries = 2
        frame_type = 255 /* full_frame */
          offset_delta = 111
          locals = [ class "[Ljava/lang/String;", class java/util/Scanner, double, double, double, double ]
          stack = []
        frame_type = 7 /* same */
}
SourceFile: "Test.java"
BootstrapMethods:
  0: #62 REF_invokeStatic java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;
    Method arguments:
      #68 \u0001------\u0001
InnerClasses:
  public static final #75= #71 of #73;    // Lookup=class java/lang/invoke/MethodHandles$Lookup of class java/lang/invoke/MethodHandles
```

![](C:\Users\Gavin\Pictures\Camera Roll\13.png)

## 