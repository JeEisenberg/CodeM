# JAVA代码进阶--手写系统

## JAVA--日历打印

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

[日历resource下载](https://gitee.com/varyu-program/CodeMar/blob/master/calendar/Rili.java)

## JAVA--记账系统

> 程序员就要多撸代码，以便在脑海中形成深刻记忆，昨晚试着撸了一个小小的记账系统，内容很简单，主要负责简单的记账，不过没用到持久层，后续会做持久层的版本，先看个简单的把。

```java
package com.shayu.note;

import java.util.Scanner;

public class SharkSystem {

    double shouru = 0;
    double zhichu = 0;
    String shouruSM;
    String zhichuSM;

    double balance;
    String details = "";//用来接收所有输入的内容
    boolean flag = true;//定义标记

    public void StartSystem() {//定义方法
//        //下面开始循环
        while (flag) {
            //开机界面
            System.out.println("-----欢迎使用鲨鱼记账系统-----");
            System.out.println("1，收支明细");
            System.out.println("2，登记收入");
            System.out.println("3，登记支出");
            System.out.println("4，退出系统");
            System.out.println("请按照提示选择你要使用的的功能--");
            Scanner scanner = new Scanner(System.in);//判断用户输入是否符合要求
            int choice = scanner.nextInt();//选择
            //检测用户输入的数据并返回相应的请求结果
            while (choice != 1 & choice != 2 & choice != 3 & choice != 4) {
                System.out.println("请重新输入--");
                int newchoice = scanner.nextInt();
                choice = newchoice;
            }


            //定义具体的功能模块//输入对应数字选择功能
            switch (choice) {
                case 1://收支明细
                    System.out.println("欢迎使用小鲨鱼收支记账系统》》》");
                    System.out.println("收支明细--");
                    //之后再次获取用户输入的内容
                    //这里打印明细
                    if (details != "") {
                        System.out.println(details.substring(0, details.length() - 1));
                    } else {
                        System.out.print(details);
                    }

                    break;
                case 2://登记收入
                    System.out.println("请输入您的收入--");
                    shouru = scanner.nextDouble();
                    System.out.println("请输入收入说明--");
                    shouruSM = scanner.next();
                    System.out.println("-登记完成-");
                    balance += shouru;
                    details = details + "收入：" + shouru + ",收入说明：" + shouruSM + ",账户余额：" + balance + "\n";
                    break;
                case 3://登记支出
                    System.out.println("请输入您的支出--");
                    zhichu = scanner.nextDouble();
                    System.out.println("请输入支出说明--");
                    zhichuSM = scanner.next();
                    System.out.println("-登记完成-");
                    balance -= zhichu;
                    details = details + "支出：" + zhichu + ",支出说明：" + zhichuSM + ",账户余额：" + balance + "\n";
                    break;
                case 4://退出系统

                    System.out.println("记账系统》》》您确定要退出系统？(Y/N)");

                    //判断用户输入Y/N
                    String isExist = scanner.next();
                    switch (isExist) {
                        case "Y":
                            System.out.println("系统已退出，欢迎您下次继续使用！");
                            scanner.close();
                           System.exsit(1); //1正常退出，0异常退出
                    }
                    
            }
            
        }
    }
}
```

~~~java
package com.shayu.note;

import java.util.Scanner;

//鲨鱼记账系统
public class Shark {
    public static void main(String[] args) {
        SharkSystem shark=new SharkSystem();
        shark.StartSystem();
    }

}

```//加入持久层等待后续更新---

~~~

[记账resource下载](https://gitee.com/varyu-program/CodeMar/blob/master/note/%E5%B0%8F%E9%B2%A8%E9%B1%BC%E8%AE%B0%E8%B4%A6%E7%B3%BB%E7%BB%9F.zip)

## JAVA--猜数字游戏

> 主要是回顾嵌套循环，跟之前写的Shark记账系统是一样的思路，只不过用到的方法有些许差别--
>
> 鲨鱼记账系统参考链接
> https://blog.csdn.net/weixin_54061333/article/details/118176551

```java
javapackage guess.game;
import java.util.Scanner;

public class GuessGame {

    //定义游戏方法
    public void StartGame() {

        int myHeartNum = (int) (Math.random() * 101);//在0-100之间随机产生一个整数
        System.out.println("请输入一个0-100的整数");
        //接收输入的数
        Scanner scanner = new Scanner(System.in);
        int input = scanner.nextInt();
        boolean flag = true;//定义开关
        while (flag) {
            //判断输入的数是否符合要求
            while (!(input >= 0 && input <= 100)) {
                System.out.println("输入错误，请重新输入--");
                input = scanner.nextInt();
            }
            if (input == myHeartNum) {
                System.out.println("好厉害，猜一次就猜对了");
                flag = false;
            }


            if (myHeartNum > input) {
                System.out.println("你输入的" + input + "比这个数小");
                System.out.println("请再次尝试--");
                input = scanner.nextInt();

            }
            if (myHeartNum < input) {
                System.out.println("你输入的" + input + "比这个数大");
                System.out.println("请再次尝试--");
                input = scanner.nextInt();

            }
            if (input == myHeartNum) {
                System.out.println("恭喜你终于猜对了");
                flag = false;
                scanner.close();
                System.exit(1);
            }
        }
    }
}
```

由此有次可以展开设计很多简易的系统，比如说记账系统，点餐系统，系统越复杂用到的知识点就越多。

简易的系统的缺点就是只能在当前运行的时候有效，下一次不能将之前的数据加载出来，没有做到持久化。

慢慢来---接下来的工作就是将Shark记账系统做一下持久化--链接数据库；

[猜数字resource下载](https://gitee.com/varyu-program/CodeMar/blob/guess/guess/com.guess.game.zip)

## JAVA--贪吃蛇游戏

**JAVA中的Swing编程**

> hello 大家好,作为一只单身程序员,我只能说刚刚踏入了程序员的门槛,还有许多知识要学--比如说....想说的太多,我却欲言又止!
> 少罗嗦直接看东西---

带着问题去学习才能更高效的记忆---理解万岁这句话还真是万能的.

> 第一个问题(包含两小问)--- 
> 1,java中如何建立一个窗口----2,新建的窗口怎么看不到?

我先介绍两种方式
frame 和jframe

```java
public class Tank {
    public static void main(String[] args) {
        Frame frame = new Frame();
         JFrame jf = new JFrame();
```

运行后啥也看不到--

> 因为窗口加载到内存中,没有设置显示,所以看不到 啥也看不到,所以没法截图--还有一点,运行之后因为没有用到,可能被 JVM当作垃圾回收了;

设置为可见状态---

```java
public class Tank {
    public static void main(String[] args) {
        Frame frame = new Frame();
        //frame.show();
        frame.setVisible(true);
         JFrame jf = new JFrame();
          //jf.show();//老的方法
        jf.setVisible(true);//新方法
```

show()方法比较古老--现在已经不用,
建议用setVisible()来控制窗口是否可见;

运行结果如图--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210706194225664.png)
由于没有设置大小和位置,两个窗口是重合的,而且大小也不能被改变;
其实这是两个窗口--

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070619464431.png)
构造方法----

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210706202047509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 第二个问题,(包含三小问)--- 
> 1,窗口的标题怎么设置?
> 2,窗口的大小怎么设置?
> 3,窗口怎么关闭?跟我们平时一样吗?
>
> 

```java
package com.gavin.tank;

import javax.swing.*;

import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

/**
 * @author : Gavin
 * @date: 2021/7/6 - 07 - 06 - 19:08
 * @Description: com.gavin.tank
 * @version: 1.0
 */
public class Tank {
    public static void main(String[] args) {
        Frame frame = new Frame();
        //frame.show();
        frame.setVisible(true);
        frame.setResizable(true);
        frame.setTitle("我的窗口1");
    frame.setBounds(50,50,400,400);

        JFrame jf = new JFrame();
        //jf.show();//老的方法
        jf.setVisible(true);//新方法
        jf.setResizable(false);
    frame.setTitle("我的窗口2");
jf.setBounds(200,200,500,500);

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210706195308842.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
以上设置完毕,我们发现没法关闭窗口---点击右上角  X号,没反应---
关闭窗口需要加入一个窗口监听器--addWindowListener();
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210706195525720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
该方法要求我们传入一个窗口对象--

```java
frame.addWindowListener(new WindowListener() {
            @Override
            public void windowOpened(WindowEvent e) {
                
            }

            @Override
            public void windowClosing(WindowEvent e) {

            }

            @Override
            public void windowClosed(WindowEvent e) {

            }

            @Override
            public void windowIconified(WindowEvent e) {

            }

            @Override
            public void windowDeiconified(WindowEvent e) {

            }

            @Override
            public void windowActivated(WindowEvent e) {

            }

            @Override
            public void windowDeactivated(WindowEvent e) {

            }
        });
```

写完之后会自动带出几种实现方法----
往前倒腾一下--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210706195800294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
WindowListener为一个接口,匿名类实现了该对象---
这样做的好处是运行一次即结束,垃圾回收机制会自动将他回收,运行完毕后不占地方;
根据方法我们就知道方法的含义了,这里只说我们现在长用的--打开关闭
![在](https://img-blog.csdnimg.cn/20210706200149771.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
关于windowsListener,他还有自己的实现类--不过是个抽象类,抽象类的好处是---可以根据需要区覆写接口中的方法
![所以](https://img-blog.csdnimg.cn/20210706200335707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
下面贴一下完整代码--

```java
package com.gavin.tank;

import javax.swing.*;

import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

/**
 * @author : Gavin
 * @date: 2021/7/6 - 07 - 06 - 19:08
 * @Description: com.gavin.tank
 * @version: 1.0
 */
public class Tank {
    public static void main(String[] args) {
        Frame frame = new Frame();
        //frame.show();
        frame.setVisible(true);
        frame.setResizable(true);
        frame.setTitle("我的窗口1");
        frame.setBounds(50,50,400,400);
        frame.addWindowListener(new WindowListener() {
            @Override
            public void windowOpened(WindowEvent e) {

            }

            @Override
            public void windowClosing(WindowEvent e) {
System.exit(0);
            }

            @Override
            public void windowClosed(WindowEvent e) {

            }

            @Override
            public void windowIconified(WindowEvent e) {

            }

            @Override
            public void windowDeiconified(WindowEvent e) {

            }

            @Override
            public void windowActivated(WindowEvent e) {

            }

            @Override
            public void windowDeactivated(WindowEvent e) {

            }
        });
        JFrame jf = new JFrame();
        //jf.show();//老的方法
        jf.setVisible(true);//新方法
        jf.setResizable(false);
        jf.setTitle("我的窗口2");
        jf.setBounds(200,200,500,500);
        jf.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                super.windowClosing(e);
            }
        });


    }
}

```

未完待续--

> 友情提醒--关于swing编程现在真的用的很少,感兴趣可以看一下,不感兴趣也最好了解一下.

写在本篇最后---
写这个的目的是什么呢?
开发小游戏--以便熟练掌握java基础知识,验证自己掌握多少,仅此而已



**贪吃蛇游戏开发**

> 首先说一下，经典游戏贪吃蛇--原理很简单不罗嗦，我们是来看东西的。
> 游戏需要一个容器来承载，其次是我们需要为游戏准备一些必要的东西，贪吃蛇首先的要有蛇--准备蛇的图片素材，为了便于开发建议蛇的每个部位的图片大小一样；

> 1，准备好上述素材之后开始创建容器--Jframe

看代码--

```java
package com.snake;

import javax.swing.*;
import java.awt.*;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 8:27
 * @Description: com.snake
 * @version: 1.0
 */
public class StartGame {
    public static void main(String[] args) {
        //创建窗体
        JFrame jFrame= new JFrame();
        //设置窗口标题
        jFrame.setTitle("贪吃蛇游戏   designed by Gavin");
        //设置窗体大小，由于要考虑到不同的屏幕尺寸，所以首先要获得运行时电脑的屏幕参数
        int  height =Toolkit.getDefaultToolkit().getScreenSize().height;
        int width =Toolkit.getDefaultToolkit().getScreenSize().width;
        jFrame.setBounds((width-800)/2,(height-800)/2,800,800);
        //设置窗体大小不可调节
        jFrame.setResizable(false);

        //默认窗体不可见，设置成可见的
        jFrame.setVisible(true);
    }
}

```

但是关闭窗体后程序并没有结束
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705083648617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

我们想要的是--程序关闭后游戏即退出--不然占内存呀！！
完善之后的代码---

```java
package com.snake;

import javax.swing.*;
import java.awt.*;


public class StartGame {
    public static void main(String[] args) {
        //创建窗体
        JFrame jFrame = new JFrame();
        //设置窗口标题
        jFrame.setTitle("贪吃蛇游戏   designed by Gavin");
        //设置窗体大小，由于要考虑到不同的屏幕尺寸，所以首先要获得运行时电脑的屏幕参数
        int height = Toolkit.getDefaultToolkit().getScreenSize().height;
        int width = Toolkit.getDefaultToolkit().getScreenSize().width;
        jFrame.setBounds((width - 800) / 2, (height - 800) / 2, 800, 800);
        //设置窗体大小不可调节
        jFrame.setResizable(false);
        //设置窗体关闭游戏退出
        jFrame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        //默认窗体不可见，设置成可见的
        jFrame.setVisible(true);
    }
}

```

> ,2，接下来---封装小蛇---这是一种思想，如果要把图片、文件等加载到程序中就要将他封装成具体的对象，封装是先通过封装地址，在通过地址封装为具体对象的；



封装之前先导入素材--可以单独创建一个包用来存放素材；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705084232634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

这里是用一个包专门来放素材的---方便查找；
封装之前我们要明白代码是经过编译之后运行的，所以我们存放素材的地址并不是编译之后的地址--做一个测试类--

```java
import java.net.URL;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 8:45
 * @Description: com.snake
 * @version: 1.0
 */
public class UrlTest {
    public static void main(String[] args) {
        URL headerUrl=SnakeImages.class.getResource("/");
        System.out.println(headerUrl);
    }
}

```

执行结果如下---

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
file:/C:/Users/Administrator/IdeaProjects/SnakeEatGame/out/production/SnakeEatGame/
```

所以在封装之前我们要预编译一下看图片被加载到哪里了；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705085142692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

预编译之后我们会发存放class文件的out文件夹下多了一个文件夹---
这个snakeImages就是存放图片的位置；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705085300506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

图片编译前存放位置（绝对路径）
C:\Users\Administrator\IdeaProjects\SnakeEatGame\src\snakeImage\header.png
图片编译后存放位置（绝对路径）
C:/Users/Administrator/IdeaProjects/SnakeEatGame/out/production/SnakeEatGame/

对比后我们发现的确不一样，由于运行时看的是编译后的地址，所以要先找地址而不能直接封装图片。

> 3，接下来开始封装---

```java
package com.snake;

import javax.swing.*;
import java.awt.*;
import java.net.URL;
import java.sql.SQLOutput;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 8:44
 * @Description: com.snake
 * @version: 1.0
 */
public class SnakeImages {
    public static URL headerUrl = SnakeImages.class.getResource("/snakeImage/header.png");
    public static ImageIcon headerImg = new ImageIcon(headerUrl);

    public static URL upUrl = SnakeImages.class.getResource("/snakeImage/up.png");
    public static ImageIcon upImg = new ImageIcon(upUrl);

    public static URL downUrl = SnakeImages.class.getResource("/snakeImage/down.png");
    public static ImageIcon downImg = new ImageIcon(downUrl);

    public static URL rightUrl = SnakeImages.class.getResource("/snakeImage/right.png");
    public static ImageIcon rightImg = new ImageIcon(rightUrl);

    public static URL leftUrl = SnakeImages.class.getResource("/snakeImage/left.png");
    public static ImageIcon leftImg = new ImageIcon(leftUrl);

    public static URL bodyUrl = SnakeImages.class.getResource("/snakeImage/body.png");
    public static ImageIcon bodyImg = new ImageIcon(bodyUrl);

    public static URL foodUrl = SnakeImages.class.getResource("/snakeImage/food.png");
    public static ImageIcon foodImg = new ImageIcon(foodUrl);
    
}

```

> 4， 封装完毕之后，要画一个面板，可以把窗体理解为容器，面板才是游戏运行是的界面--

新建一个面板类--并不是随便一个类就可以成为面板，需要继承Jpanel类
面板中定义蛇的厨师位置等信息

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.util.Random;

/**
 * @Auther: GavinLim
 * @Date: 2021/7/5 - 07 - 05 - 9:07
 * @Description: com.snake
 * @version: 1.0
 */
public class GamePanel extends JPanel {
    int length;//蛇的长度
    //蛇的坐标
    int snakeX[] = new int[10000];
    int snakeY[] = new int[10000];
    //蛇头的方向
    String direction;
    //游戏是否开始--默认没开始
    boolean isStart = false;
    //蛇是否死亡---开始时候默认活着
    boolean isDie = false;
    //定时器，用于启动小蛇
    Timer timer;
    //定义食物位置
    int foodX;
    int foodY;
    //定义分数
    int score;

    //定义初始化方法---在构造方法中初始化并不太好，所以拿到外面来方便直接调用；
    public void reset() {
        //初始化蛇的长度
        length = 4;
        //初始化蛇的位置
        snakeX[0] = 200;
        snakeY[0] = 275;
        snakeX[1] = 175;
        snakeY[1] = 275;
        snakeX[2] = 150;
        snakeY[2] = 275;
        snakeX[3] = 125;
        snakeY[3] = 275;
        //初始化食物位置
        foodX = 400;
        foodY = 500;
        //初始化蛇头位置
        direction = "R";
    }
```

> 5，接下来定义面板构造方法--承接上面代码，构造方法中有监听器，同时有判断的逻辑

```java
    public GamePanel() {
        reset();
//程序运行之后要将焦点挪到游戏界面
        this.setFocusable(true);
//加入按键监听--
          /*public interface KeyListener extends EventListener {
            KeyListener 是一个接口，实现接口需要全部实现*/


       /*this.addKeyListener(new KeyListener() {
            @Override
            public void keyTyped(KeyEvent e) {

            }

            @Override
            public void keyPressed(KeyEvent e) {

            }

            @Override
            public void keyReleased(KeyEvent e) {

            }
        });*/
        /*----public abstract class KeyAdapter implements KeyListener {
        KeyAdapter实现了KeyListener接口，同时又是一个抽象类，可以根据自己需要进行覆写*/
        this.addKeyListener(new KeyAdapter() {
            @Override
            public void keyPressed(KeyEvent e) {
                super.keyPressed(e);
                //通过监听按键来控制游戏--每一个按键对应一个ASCII码，通过此来判断用户按了哪个键
                int input = e.getKeyCode();
                //监听空格以开始游戏
                if (input == KeyEvent.VK_SPACE) {
//                    同时要判断小蛇是否死亡
                    if (isDie) {//如果死了，那么恢复初始状态
                        reset();
                        //重新赋予小蛇生命
                        isDie = false;
                    } else {//没死的话，开始--暂停进行切换
                        isStart = !isStart;
                        repaint();//重绘
                    }
                }
                //这个监听完毕之后开始监听上下左右箭头---当然可以监听wasd,

                if (input == KeyEvent.VK_UP) {
                    direction = "U";
                }
                if (input == KeyEvent.VK_W) {
                    direction = "U";
                }
                if (input == KeyEvent.VK_DOWN) {
                    direction = "D";
                }
                if (input == KeyEvent.VK_S) {
                    direction = "D";
                }
                if (input == KeyEvent.VK_RIGHT) {
                    direction = "R";
                }
                if (input == KeyEvent.VK_D) {
                    direction = "R";
                }
                if (input == KeyEvent.VK_LEFT) {
                    direction = "L";
                }
                if (input == KeyEvent.VK_A) {
                    direction = "L";
                }

            }
//按键释放--没有相应操作--所以不用动他
            @Override
            public void keyReleased(KeyEvent e) {
                super.keyReleased(e);
            }
        });
       
```

> 6，写完上述代码，小蛇还没有动，这时候需要启动定时器

```java //启用定时器--每200ms监测一次面板一下
        timer = new Timer(200, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                if (isStart == true && isDie == false) {//游戏开始状态
                    //蛇动----蛇尾开始动，然后开始蛇头--俗称guyong'
                    for (int i = length - 1; i > 0; i--) {
                        //后一节依次往前挪一位
                        snakeX[i] = snakeX[i - 1];
                        snakeY[i] = snakeY[i - 1];
                    }
                    
                    //头部比较复杂一下，因为要上下左右动
                    if ("R".equals(direction)) {
                        snakeX[0] += 25;
                    }
                    if ("L".equals(direction)) {
                        snakeX[0] -= 25;
                    }
                    if ("U".equals(direction)) {
                        snakeY[0] -= 25;
                    }
                    if ("D".equals(direction)) {
                        snakeY[0] += 25;
                    }
                    //防止越界后     回不来了
                    if (snakeX[0] > 750) {
                        snakeX[0] = 25;
                    }
                    if (snakeX[0] < 25) {
                        snakeX[0] = 750;
                    }
                    if (snakeY[0] < 100) {
                        snakeY[0] = 725;
                    }
                    if (snakeY[0] > 725) {
                        snakeY[0] = 100;
                    }
                    
                    //检测碰撞动作--吃食物
                    //这里一定要注意是25的倍数，不然会哦出现吃不到食物的尴尬
                    if (snakeX[0] == foodX && snakeY[0] == foodY) {
                        //吃到食物后蛇产古增加，积分增加
                        length++;
                        //吃到食物后，食物会重新产生--随机
                        foodX = ((int) (Math.random() * 30) + 1) * 25;
                        foodY = ((new Random().nextInt(26) + 4) * 25);
                        score += 10;
           /**蛇长度增加后要马上修改该节身子的位置--不然会在游戏左上角，当蛇每吃到一颗食物就会闪一下，可以把以下代码注释掉试一下，出现的原因是每次增加的时候没有立马给定增加的身体部分位置，所以默认为位置为 （0,0），加上一下两行代码就解决了；*/
                        snakeX[length - 1] = snakeX[length - 2];
                        snakeY[length - 1] = snakeY[length - 2];

                    }
                    //判定死亡的方法--蛇头跟任意一节身子碰撞就是死亡
                    for (int i = 1; i < length; i++) {
                        if (snakeX[0] == snakeX[i] && snakeY[0] == snakeY[i]) {
                            isDie = true;
                        }
                    }

                    repaint();//重绘制
                }
            }
        });
//        以上写完小蛇还没动--因为没有开启定时器
        timer.start();
    }

```

> 7，接下来要不断绘制图像界面才能使得人眼看到动的小蛇

```java
    @Override//覆写该方法
    public void paintComponent(Graphics g) {
        super.paintComponent(g);//这是一个画笔
        //设置游戏区面板颜色
        this.setBackground(new Color(195, 253, 4));
        // 加载图片--header--m默认位置，不需要动
        SnakeImages.headerImg.paintIcon(this, g, 0, 5);
        //设置画笔颜色
        g.setColor(new Color(193, 255, 183));
        //填充游戏界面，可以找个好看的图片
        //SnakeImages.backImg.paintIcon(this,g,8,70);//由于太丑，所以不加载背景图了
        g.fillRect(10, 70, 770, 685);
        //下面加载蛇头和身子
        if ("R".equals(direction)) {
            SnakeImages.rightImg.paintIcon(this, g, snakeX[0], snakeY[0]);
        }
        if ("L".equals(direction)) {
            SnakeImages.leftImg.paintIcon(this, g, snakeX[0], snakeY[0]);
        }
        if ("U".equals(direction)) {
            SnakeImages.upImg.paintIcon(this, g, snakeX[0], snakeY[0]);
        }
        if ("D".equals(direction)) {
            SnakeImages.downImg.paintIcon(this, g, snakeX[0], snakeY[0]);
        }
//        画完蛇头画蛇身
        for (int i = 1; i < length; i++) {
            SnakeImages.bodyImg.paintIcon(this, g, snakeX[i], snakeY[i]);

        }

        if (isStart == false) {
            //设置画笔颜色
            g.setColor(new Color(255, 173, 0));
            //画一个文字
            g.setFont(new Font("微软雅黑", Font.BOLD, 40));
            g.drawString("点击空格开始游戏", 250, 300);
        }
        //画食物
        SnakeImages.foodImg.paintIcon(this, g, foodX, foodY);
//画积分
        g.setColor(new Color(255, 0, 19));
        g.setFont(new Font("微软雅黑", Font.BOLD, 20));
        g.drawString("积分：" + score, 620, 40);

        //死亡状态：
        if (isDie) {
            g.setColor(new Color(255, 82, 68));
            g.setFont(new Font("微软雅黑", Font.BOLD, 20));
            g.drawString("小蛇死亡，游戏停止，按下空格重新开始游戏", 200, 330);
            score = 0;
        }
    }
}
```

以上代码写完，运行后发现少了点什么，-----在游戏启动界面没有添加还面板，接下来就是补充和完善代码了-

```java
package com.snake;

import javax.swing.*;
import java.awt.*;


public class StartGame {
    public static void main(String[] args) {
        //创建窗体
        JFrame jFrame = new JFrame();
        //设置窗口标题
        jFrame.setTitle("贪吃蛇游戏   designed by Gavin");
        //设置窗体大小，由于要考虑到不同的屏幕尺寸，所以首先要获得运行时电脑的屏幕参数
        int height = Toolkit.getDefaultToolkit().getScreenSize().height;
        int width = Toolkit.getDefaultToolkit().getScreenSize().width;
        jFrame.setBounds((width - 800) / 2, (height - 800) / 2, 800, 800);
        //设置窗体大小不可调节
        jFrame.setResizable(false);
        //设置窗体关闭游戏退出
        jFrame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        GamePanel gamePanel= new GamePanel();
        jFrame.add(gamePanel);
        //默认窗体不可见，设置成可见的
        jFrame.setVisible(true);
    }
}


```

运行结果的一些截图--
启动界面--

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021070513513218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

运行界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705135311415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

死亡界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705135401561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 如果你有大块时间完全可以在完善一下此代码，添加一些模式-- 
> 1，急速模式--调节启动器的时间间隔即可 
> 2，在窗口中再来一条蛇--与多线程结合
> 3，在丰富一些设置内容--比如选择模式等
>
> 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705135627995.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)重新游戏时记得积分清零哦。。。



> 总结---虽然现在swing编程很low了，但是该学习的地方还是有的---
>  1，要学会将外部对象导入到程序中并封装成相应的类
> 2，练习监听器的思想 
> 3，检测自己学过的基础知识--数组，循环以及逻辑思维；
>  4，重要的是游戏的开发思维---动效来自于不断的刷新；
>  5，代码中多次用到了匿名对象---如果该对象只用一次，可以这样用，如果多次那么就要去实例化他了

[贪吃蛇resource下载](https://gitee.com/varyu-program/CodeMar/blob/snake/snake/SnakeGame.zip)

![](..\pic\div\weichatcode.png)