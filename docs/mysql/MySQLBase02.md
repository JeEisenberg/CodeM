![](..\pic\div\deer.gif)

**在学习编程语言的时候我们一开始就学习的基本入门知识-------数据类型**

> **在数据库中有很多表,每张表中可以有很多字段,字段可以指定不同的数据类型**

# 基本数据类型

# 整型

![在这里插入图片描述](https://img-blog.csdnimg.cn/3e6191c1719244a4a5a3b4429e8c2a8b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/fffe0173ba014d42a69012705d02d343.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 有符号整型数据类型

一句话先建表------
**以tinyint为例子;**
![在这里插入图片描述](https://img-blog.csdnimg.cn/90bc5b3a2e904fc28c8c802cb0ec117b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)默认情况下创建的整型数据是有符号的,如果超出范围会提示

> **ERROR 1264 (22003): Out of range value for column 'a' at row 1**

## 无符号整型数据类型

无符号整型类型,顾名思义就是没有负号,只比表示正的数据,则正数表示范围为有符号时的一倍;
一句话先建表------
**以tinyint为例子;**
![在这里插入图片描述](https://img-blog.csdnimg.cn/3fec3f1ba27b48b1a08d0d7b4bc71472.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **可以发现在有符号的时候255无法插入,在无符号的时候就可以;**

其他整型数据跟tinyint差不多,只不过是范围有些不同;

**在建立表的时候最好是选择适合的数据类型------比如年龄的时候用tinyint unsigned(无符号的范围很小的整型)以节约存储空间,提高查询效率;**

## 显示宽度与实际值的关系----

先看图,
![在这里插入图片描述](https://img-blog.csdnimg.cn/cf84f945d5a04675ab0b6fc99f3fe168.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 显示宽度和数据类型的取值范围是无关的。显示宽度只是指明MySQL最大可能显示的数字个数，数值的位数小于指定的宽度时会由空格填充；如果插入了大于显示宽度的值，只要该值不超过该类型整数的取值范围，数值依然可以插入，而且能够显示出来。

比如上图中的指定位宽为2位,但是插入128的时候也可以显示出来;



# 浮点类型数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/a807e2c87bcd4ec3b3a5ddc0f167e6a3.png)

## 不指定精度

先建表-----float double decimal都不指定精度
![在这里插入图片描述](https://img-blog.csdnimg.cn/b5544eb2f2aa46a0bd18cf8fd45af445.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

插入数据----
![在这里插入图片描述](https://img-blog.csdnimg.cn/a7c6d316339c4215af25ea41ab8fc954.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以发现---
1,float和double类型的在没有指定精度的时候默认按照实际的精度进行操作,系统内并没有进行四舍五入;
2,插入c字段数据类型----decimal,
来看一下decimal对数据的要求------

> **decimal不指定精度,则默认为(10,0)
> 指定精度的格式----decimal(m,n)   其中m表示数据的总的位数,n表示小数位置的位数**
>  所以 decimal(10,0)表示整数位为10位,小数位为0位的数,最大值为999999999,最小值为-999999999;
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/5076d722cf3a4364a6aee03bbbcd70aa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 指定精度

再来建表
![在这里插入图片描述](https://img-blog.csdnimg.cn/f7ded7c4c4784cd3b2dcac187194563c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
-----插入数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/191bbb6e6592411fad80ac5200f4f791.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**由于指定了位数和小数部分的位数,所以当小数部分的数据不符合位数要求时会进行四舍五入;**

修改a字段数据类型为 float(5,2)-----查看字段值你发现了什么?
![在这里插入图片描述](https://img-blog.csdnimg.cn/5e28521fe2b943b2b06bcab7452782f8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**浮点数相对于定点数的优点是在长度一定的情况下，浮点数能够表示更大的数据范围；它的缺点是会引起精度问题。**

注意-----

> **1,在mysql中定点数是以字符串形式存储的,所以对于存储精度要求高的时候建议使用定点数表示** 
> **2,尽量避免浮点数之间的比较;**

# 日期与时间类型

![在这里插入图片描述](https://img-blog.csdnimg.cn/02929d792e2f4a9e8f76190a4f4f0d5c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## YEAR

year的表示方式----**四位数字格式或者四位字符串格式**

![在这里插入图片描述](https://img-blog.csdnimg.cn/c9fd2cfec74048e68bbad061eb5ea3aa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)year的表示方式-------**一/两位字符串格式表示**

![在这里插入图片描述](https://img-blog.csdnimg.cn/5800609cbacc41b7b31cb5eade1015c3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)以两位字符串格式表示的year------具体要求是:

1,范围'00'-'99';
2,'00'-'69'表示2000-2069,   '70'-'99'表示1970-1999,
3,单个字符串'0'-'9'表示2000-2009-----其实就相当于'00'-'09'

year的表示方式-------**两位数字格式表示**

![在这里插入图片描述](https://img-blog.csdnimg.cn/3aa673a3cd4c411abf56b636e14ff7c0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**以两位数字格式表示的年,其中0或者00 表示0000,而不是2000**

但是实际操作中一般习惯直接用四位数字格式表示年份;

**使用日期函数插入Year数据**

![在这里插入图片描述](https://img-blog.csdnimg.cn/8d9b9bcdb1e44fc6a99dab599c0f2990.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## TIME

time的表示方法----**有间隔的字符串形式**
![在这里插入图片描述](https://img-blog.csdnimg.cn/ab4a5dabe5244ff3a4c60db7e4192734.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)使用字符串表示的标准格式-----
'D HH:MM:SS'   D表示天,HH表示小时MM表示分SS表示秒
还可以用非严格的表示方式----

> ‘HH:MM:SS’、‘HH:MM’、‘D HH:MM’、‘D HH’或‘SS’。这里的D表示日，可以取0~34之间的值。在插入数据库时，D被转换为小时保存，格式为“D*24+HH”。

time的表示方法----**无间隔的字符串形式和数字形式**

![在这里插入图片描述](https://img-blog.csdnimg.cn/7db4d24b602c4ed8822d3e0f255f2b05.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)一些注意点-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/1b777241d9be4a84b62d9dc7b03bd37d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**在插入 '1045',1045 和'10:45' 时,从结果可以看到区别-----这是因为mysql默认无间隔的时间数据从右边秒开始读的,所以将 '1045',1045  识别为 00:10:45;**

> **在使用‘D HH’格式时，小时一定要使用双位数值，如果是小于10的小时数，应在前面加0。**

**使用日期函数插入Time数据**

![在这里插入图片描述](https://img-blog.csdnimg.cn/94a3480a43c748d4a736960efb62a717.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## Date

date只表示日期,不包含具体时间点

date的表示方法----**字符串形式**

建立表----
![在这里插入图片描述](https://img-blog.csdnimg.cn/632bb77a3a2a4d46a4ddb3f9f3873491.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以发现,如果表中有数据,且该数据不符合当前要修改的数据要求,会出现错误提示----
**ERROR 1292 (22007): Incorrect date value: '0000' for column 'birth' at row 1**

> **注意--------尽量不要修改已有数据的字段的数据类型;**

插入数据-------
![在这里插入图片描述](https://img-blog.csdnimg.cn/3661ba9de9514cea9a722496d1fea8d8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)总结以上几种形式----

```sql
'YYYY-MM-DD'----间隔符不一定非得为     -(短杠)
'YYYYMMDD' ------ 可以不加间隔符
'YYMMDD'-------------- 年份可以只写两位
'YY,MM,DD'---------年份写两位时也可以加间隔符
```

年份写两位的情况下   仍然遵循的时Year的年份表示形式

date的表示方法----**数字表示形式**

![在这里插入图片描述](https://img-blog.csdnimg.cn/f878f5be34374acc8993b5ff5a2a9638.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**使用日期函数插入date数据**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20435cfec1e4450bb381289f78c1e649.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## DateTime

可以将datetime作为date和time的结合提来学习,用于保存完整的(年月日时分秒)时间信息;

datetime的表示方法----**字符串表示形式**
建表插入数据----
![在这里插入图片描述](https://img-blog.csdnimg.cn/48d0212c31ef4d1cbbc7ffc3d3f7f99d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
总结上述表示格式----

```sql
'YYYY-MM-DD HH:MM:SS'   ------不局限于此种间隔符
'YYYYMMDDHHMMSS'
'YYMMDDHHMMSS'
'YY-MM-DD HH:MM:SS'
```

datetime的表示方法----**数字表示形式**

![在这里插入图片描述](https://img-blog.csdnimg.cn/9a16030ca264495cb2ec6d134ad96e8a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)总结上述表示格式----

**YYYYMMDDHHMMSS
YYMMDDHHMMSS**

**使用日期函数插入datetime数据**

![在这里插入图片描述](https://img-blog.csdnimg.cn/4e04832917464e41bed0f915b4e9645f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## TIMESTAMP

timestamp-----时间戳显示时间格式与datatime相同,显示宽度在19个字符,日期格式为YYY-MM-DD HH:MM:SS;
**时间范围:
'1970-01-01 00:00:01' UTC到'2038-01-19 03:14:07' UTC,因此杀入时间戳要在合理范围内;**

timestamp的表示方法----**字符串表示形式**

建表-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2ea425d34d24c15a479f4e5548a3eb6.png)
插入数据----
![在这里插入图片描述](https://img-blog.csdnimg.cn/7f041cd8bc644280a6ad8852f785533c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
查看数据------
![在这里插入图片描述](https://img-blog.csdnimg.cn/67d849ef19b54d428c961242090396c0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**显示格式跟datetime是一样的;**

timestamp的表示方法----**数字表示形式**

![在这里插入图片描述](https://img-blog.csdnimg.cn/f6845675b81f42b49f046fe505fadcaa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**timestamp格式跟datetime是一样的,只不过时间范围比datetime要小**

**使用日期函数插入TIMESTAMP数据**

![在这里插入图片描述](https://img-blog.csdnimg.cn/772a8f1f666041fc88c34dea2ece84e6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**注意----**

> date不包含具体时刻 datetime和timestamp包含具体时间,因此在具体时间转化的时候
>
> date转为datetime和timestamp时时刻会转为00:00:00 反过来会丢弃时刻信息;

**TIMESTAMP与DATETIME的区别**

> 1,TIMESTAMP与DATETIME除了存储字节和支持的范围不同,
> 2DATETIME在存储日期数据时，按实际输入的格式存储，即输入什么就存储什么，与时区无关；而TIMESTAMP值的存储是以UTC（世界标准时间）格式保存的，存储时对当前时区进行转换，检索时再转换回当前时区。查询时，不同时区显示的时间值是不同的。



# 文本字符串类型

在mysql中由两种字符串类型
1,非二进制字符串形式-------主要存储文本数据
2,二进制字符串形式-----主要存储图片\视频等非二进制数据

**非二进制字符串形式:**
![在这里插入图片描述](https://img-blog.csdnimg.cn/ad98073c754e435ca456a9943b962027.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## CHAR与VARCHAR----保存字符串

CHAR(M)为固定长度字符串，在定义时指定字符串列长。当保存时在右侧填充空格，以达到指定的长度。M表示列长度，M的范围是0~255个字符;

VARCHAR(M)是长度可变的字符串，M表示最大列长度。M的范围是0~65535。VARCHAR的最大实际长度由最长的行的大小和使用的字符集确定，而其实际占用的空间为字符串的实际长度加1。在值保存的时候尾部的空格仍保留;

建表----

![在这里插入图片描述](https://img-blog.csdnimg.cn/447fac24f7254a238a45227cea76e1d8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

插入数据----

![在这里插入图片描述](https://img-blog.csdnimg.cn/5a3700e03b274236b46c08120caafafb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这里变长和定长的意思是---

> **比如----- 定义了char(4)**
> **则该字段每条记录最长为4个字符,如果不够4个字符,那么会用空格来凑齐4个字符,即存储的长度为固定的,大小也就固定;**

> **定义了varchar(4)
> 则该字段每条记录最长也为4个字符,如果不够字符也不会用空格来凑,只存储为实际长度+1   (多的一位位结束字符);**

所以插入数据时'ab  '  在字段格式为char(4)与varchar(4)存储的数据是有区别的;

## text 保存非二进制字符串

TEXT列保存非二进制字符串，如文章内容、评论等。当保存或查询TEXT列的值时，不删除尾部空格。

Text类型分为4种：TINYTEXT、TEXT、MEDIUMTEXT和LONGTEXT。不同的TEXT类型的存储空间和数据长度不同。
（1）TINYTEXT最大长度为255（28–1）字符的TEXT列。
（2）TEXT最大长度为65535（216–1）字符的TEXT列。
（3）MEDIUMTEXT最大长度为16777215（224–1）字符的TEXT列。
（4）LONGTEXT最大长度为4294967295（232–1）或4GB字符的TEXT列。

建表---插入数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/64052afc245b4beebabc6db53f0961d8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## eunm 保存字符串对象

enum是一个字符串对象,就类似于java中的枚举
格式---
字段名 enum('值1','值2',.....)   代表该字段只能从值1,值2....等指定的值中取值,而且一次只能取一个;
创建的成员中有空格时，其尾部的空格将自动被删除。ENUM值在内部用整数表示，并且每个枚举值均有一个索引值：列表值所允许的成员值从1开始编号，MySQL存储的就是这个索引编号。枚举最多可以有65535个元素。



建表------
![在这里插入图片描述](https://img-blog.csdnimg.cn/74dc8432c9cb43acbeae87181ce9ca72.png)插入数据------

![在这里插入图片描述](https://img-blog.csdnimg.cn/0704a97611644c949db5cccb6b7ad9ff.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这里可以发现,枚举是在mysql内部以索引的方式存储的,

为了更清楚地了解枚举在mysql内部的存储顺序,建立一张新表------
![在这里插入图片描述](https://img-blog.csdnimg.cn/b8cd714c09154ae0aaac4cca8b4bb869.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/cd352355bf3b4c56924e69144746efae.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)从结果可以发现,枚举在mysql内部是按照顺序进行存储的,定义的数据编号从0开始,所以插入数据时可以用标号来代替;
可以插入NULL值;

![在这里插入图片描述](https://img-blog.csdnimg.cn/fd352674abcf44c78c79f96e504e171d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **ENUM列总有一个默认值：如果将ENUM列声明为NULL，NULL值则为该列的一个有效值，并且默认值为NULL；如果ENUM列被声明为NOT NULL，其默认值为允许的值列表的第1个元素。**



## set 保存字符串对象

set跟enum类似,只不过是set可以选择多个值

> 如果插入SET字段中列值有重复，则MySQL自动删除重复的值；插入SET字段的值的顺序并不重要，MySQL会在存入数据库时按照定义的顺序显示；如果插入了不正确的值，默认情况下，MySQL将忽视这些值，并给出警告。

建表,插入值和查看----
![在这里插入图片描述](https://img-blog.csdnimg.cn/8bb11a7e9f6841d5a1f8570d5e9190ed.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
插入时是按照定义的字符串顺序显示的;

# 非文本字符串类型

![在这里插入图片描述](https://img-blog.csdnimg.cn/2880fe40375d4ed7abc89760855b2437.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## bit类型

建表----

![在这里插入图片描述](https://img-blog.csdnimg.cn/67ce820ad9a44ed8b10f661a886705b8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)bit(m), 目标是位宽.例如上图bit(5),插入 十进制32的时候-------
因为5位宽的时候bit 最大为11111即31,当插入32时会超出这个限制而报错;
![在这里插入图片描述](https://img-blog.csdnimg.cn/90b1c1fbe9ac4817b2cf9262739ed75c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**函数bin()是将数据转换为二进制数**

## binary类型和varbinary类型

两者之间的而区别跟char与varchar的区别一样

binary为固定长度的,不足的部分补充'\0'以达到指定长度
varbinary 为可变长度的,存储长度为实际长度+1;

建表----
![在这里插入图片描述](https://img-blog.csdnimg.cn/80c3a4d667584ff9b6a7526f91a74a02.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
观察可发现binary对不足位数的部分补'0'来完成定长;
**函数length()用来查看字段长度的;**

## blob类型

跟int类型一样也可以分为四种-------
每种可表示的范围/大小不一样;
![在这里插入图片描述](https://img-blog.csdnimg.cn/bed83ade44ad409b9bb289f73c7d1af5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# 基本运算

## 加减乘除

```sql
mysql> create table test(
    -> a int,
    -> b double,
    -> c decimal(4,1));
Query OK, 0 rows affected (0.07 sec)

mysql> insert into test values(1,2,3);
Query OK, 1 row affected (0.03 sec)

mysql> select * from test;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|    1 |    2 |  3.0 |
+------+------+------+
1 row in set (0.00 sec)


mysql> select a ,a+1,a-1,a*2,a/0,a%2,a+1.6,a*5.9,a/3 from test;
+------+------+------+------+------+------+-------+-------+--------+
| a    | a+1  | a-1  | a*2  | a/0  | a%2  | a+1.6 | a*5.9 | a/3    |
+------+------+------+------+------+------+-------+-------+--------+
|    1 |    2 |    0 |    2 | NULL |    1 |   2.6 |   5.9 | 0.3333 |
+------+------+------+------+------+------+-------+-------+--------+
1 row in set (0.00 sec)

mysql> select b,b/2,b%2,b+5.6,b-0.4,b*2.3,b/3 from test;
+------+------+------+-------+-------+-------+--------------------+
| b    | b/2  | b%2  | b+5.6 | b-0.4 | b*2.3 | b/3                |
+------+------+------+-------+-------+-------+--------------------+
|    2 |    1 |    0 |   7.6 |   1.6 |   4.6 | 0.6666666666666666 |
+------+------+------+-------+-------+-------+--------------------+
1 row in set (0.00 sec)


mysql> select c ,c+1.8,c*2.9,c/0,c%0,c-1.623,c+5.123 from test;
+------+-------+-------+------+------+---------+---------+
| c    | c+1.8 | c*2.9 | c/0  | c%0  | c-1.623 | c+5.123 |
+------+-------+-------+------+------+---------+---------+
|  3.0 |   4.8 |  8.70 | NULL | NULL |   1.377 |   8.123 |
+------+-------+-------+------+------+---------+---------+
1 row in set (0.00 sec)
```

> **1,在数据库中一个数除以0 /模0,得到的值为null;**
> **2,对于无法整除的数据,不同的数据类型/不同的精度要求保留的位数是不一样的,整型是保留小数点后4位,**
> **3,浮点型在没有指定精度的情况下是保留小数点后16位;**

## 比较运算

比较运算符如下表------
![在这里插入图片描述](https://img-blog.csdnimg.cn/c6ff8f7ac1674683ad7ad5d01e5204c8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```sql
mysql> select 1=1,2>1,2<1,4>=3,5<=5,NULL=2,2=NULL,NULL=NULL,NULL<=>NULL ,5!=6,3<=>3,8<>9;
+-----+-----+-----+------+------+--------+--------+-----------+-------------+------+-------+------+
| 1=1 | 2>1 | 2<1 | 4>=3 | 5<=5 | NULL=2 | 2=NULL | NULL=NULL | NULL<=>NULL | 5!=6 | 3<=>3 | 8<>9 |
+-----+-----+-----+------+------+--------+--------+-----------+-------------+------+-------+------+
|   1 |   1 |   0 |    1 |    1 |   NULL |   NULL |      NULL |           1 |    1 |     1 |    1 |
+-----+-----+-----+------+------+--------+--------+-----------+-------------+------+-------+------+
1 row in set (0.00 sec)
```

1,比较结果为true返回1,为false返回0
2,任何值跟NULL做等于比较,返回值为NULL,
3,NULL只有在跟NULL做安全等于的时候返回1;
**4,等号不能用于判断空值	null.**

> **比较符号----is null ,isnull 用于判断是否为空值,**
>  **比较符号-----is not null    用于判断是否为非空;**

```sql
mysql> insert into test(a,c) values (6,8);
Query OK, 1 row affected (0.02 sec)

mysql> select * from test;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|    1 |    2 |  3.0 |
|    6 | NULL |  8.0 |
+------+------+------+
2 rows in set (0.02 sec)

mysql> select * from test where b is null;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|    6 | NULL |  8.0 |
+------+------+------+
1 row in set (0.00 sec)

mysql> select * from test where b=null;
Empty set (0.00 sec)

mysql> select * from test where b is null or c is not null;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|    1 |    2 |  3.0 |
|    6 | NULL |  8.0 |
+------+------+------+
2 rows in set (0.00 sec)
```



5,对于有字符串的参与的比较-------

> **当数字跟字符串比较时,会将字符串转换为数字进行比较; 
> 当都为字符串时,按照字符串来进行比较;**

```sql
mysql> select '12'=12,12>'8','9'='9','7'!='8';
+---------+--------+---------+----------+
| '12'=12 | 12>'8' | '9'='9' | '7'!='8' |
+---------+--------+---------+----------+
|       1 |      1 |       1 |        1 |
+---------+--------+---------+----------+
1 row in set (0.00 sec)

```

  **值1  between  值2 and 值 3** 
  表达式的意思时----值1 是在值2和值3 之间吗?
  即数学上的区间      值1在[值2,值3]区间,true返回1,false返回0

```sql
mysql> select 1 between 2 and 3,6 between 5.1 and 7.2,8 between 7 and 8,7 between 7 and 8;
+-------------------+-----------------------+-------------------+-------------------+
| 1 between 2 and 3 | 6 between 5.1 and 7.2 | 8 between 7 and 8 | 7 between 7 and 8 |
+-------------------+-----------------------+-------------------+-------------------+
|                 0 |                     1 |                 1 |                 1 |
+-------------------+-----------------------+-------------------+-------------------+
1 row in set (0.00 sec)

mysql> select '1' between 2 and 3,6 between '5.1' and 7.2,'8' between '7' and '8','7' between '7' and 8;
+---------------------+-------------------------+-------------------------+-----------------------+
| '1' between 2 and 3 | 6 between '5.1' and 7.2 | '8' between '7' and '8' | '7' between '7' and 8 |
+---------------------+-------------------------+-------------------------+-----------------------+
|                   0 |                       1 |                       1 |                     1 |
+---------------------+-------------------------+-------------------------+-----------------------+
1 row in set (0.00 sec)
mysql> select 'd' between 'f' and 'c', 'e' between 'a' and 'f','d' between 'c' and 'f';
+-------------------------+-------------------------+-------------------------+
| 'd' between 'f' and 'c' | 'e' between 'a' and 'f' | 'd' between 'c' and 'f' |
+-------------------------+-------------------------+-------------------------+
|                       0 |                       1 |                       1 |
+-------------------------+-------------------------+-------------------------+
1 row in set (0.00 sec)
```

对于数字来说是按照数字大小进行比较的,对于字符串----字母来说也是按照字母顺序进行比较的,
两者都是从值2向后数的,
如果值3大于等于值2且区间范围内包含则返回1,否则返回0;
如果值3小于值2,则直接返回0;

```sql
mysql> select 3 between 3 and 2 ,3 between 2 and 3 ,'c' between 'a'and 'c','c' between 'c' and 'c','c' between 'c' and  'a' ,3 between 3 and 3;
+-------------------+-------------------+------------------------+-------------------------+--------------------------+-------------------+
| 3 between 3 and 2 | 3 between 2 and 3 | 'c' between 'a'and 'c' | 'c' between 'c' and 'c' | 'c' between 'c' and  'a' | 3 between 3 and 3 |
+-------------------+-------------------+------------------------+-------------------------+--------------------------+-------------------+
|                 0 |                 1 |                      1 |                       1 |                        0 |                 1 |
+-------------------+-------------------+------------------------+-------------------------+--------------------------+-------------------+
1 row in set (0.00 sec)
```

**least 与greatest**
least(值1,值2.....值n)  返回当中最小值

greatest(值1,值2.....值n)  返回当中最小值

```sql
mysql> select least(1,2,3,4,56,-12,0.2),greatest(1,2,3,4,65,-98,123.5),least(NULL,1,2,3,4,-23,89,65.2),greatest(NULL,1,2,3.4,67,-87.8);
+---------------------------+--------------------------------+---------------------------------+---------------------------------+
| least(1,2,3,4,56,-12,0.2) | greatest(1,2,3,4,65,-98,123.5) | least(NULL,1,2,3,4,-23,89,65.2) | greatest(NULL,1,2,3.4,67,-87.8) |
+---------------------------+--------------------------------+---------------------------------+---------------------------------+
|                     -12.0 |                          123.5 |                            NULL |                            NULL |
+---------------------------+--------------------------------+---------------------------------+---------------------------------+
1 row in set (0.00 sec)

mysql> select least('a','c','a','d'),greatest('a','c','a','d'),least('a','c','a','d',NULL),greatest('a','c','a','d',NULL);
+------------------------+---------------------------+-----------------------------+--------------------------------+
| least('a','c','a','d') | greatest('a','c','a','d') | least('a','c','a','d',NULL) | greatest('a','c','a','d',NULL) |
+------------------------+---------------------------+-----------------------------+--------------------------------+
| a                      | d                         | NULL                        | NULL                           |
+------------------------+---------------------------+-----------------------------+--------------------------------+
1 row in set (0.00 sec)


```

> **1,字符串是按照字母顺序进行比较的------底层ascii码
> 2,数字是按大小进行比较的
> 3,当值中有NULL时,返回NULL,而不是返回最大值或者最小值;
> 4,数字和字母比较是按照ASCII码表来进行大小比较的
> 5,对于数字和字母组合的比较没有实际意义不做讨论;**

常见ASCII码的大小规则：0~9<A~Z<a~z。

数字比字母要小。如 “7”<“F”；数字0比数字9要小，并按0到9顺序递增。如“3”<“8”。

字母A比字母Z要小，并按A到Z顺序递增。如“A”<“Z”；同个字母的大写字母比小写字母要小32。如“A”<“a”。

几个常见字母的ASCII码大小：“A”为65；“a”为97；“0”为 48。



**IN 与NOT IN**

IN运算符用来判断操作数是否为IN列表中的其中一个值：如果是，返回值为1；否则返回值为0。
NOT IN运算符用来判断表达式是否为IN列表中的其中一个值：如果不是，返回值为1；否则返回值为0

```sql
mysql> SELECT  's' in(1,2,3,4,'s'),NULL in('12','null',null,23,34,NULL),12 in(null,12,2,'a',NULL);
+---------------------+--------------------------------------+---------------------------+
| 's' in(1,2,3,4,'s') | NULL in('12','null',null,23,34,NULL) | 12 in(null,12,2,'a',NULL) |
+---------------------+--------------------------------------+---------------------------+
|                   1 |                                 NULL |                         1 |
+---------------------+--------------------------------------+---------------------------+
1 row in set, 1 warning (0.00 sec)

mysql> SELECT  's' in(1,2,3,4,'s'),NULL in('12','null',null,23,34,NULL),12 in(null,2,'a',NULL);
+---------------------+--------------------------------------+------------------------+
| 's' in(1,2,3,4,'s') | NULL in('12','null',null,23,34,NULL) | 12 in(null,2,'a',NULL) |
+---------------------+--------------------------------------+------------------------+
|                   1 |                                 NULL |                   NULL |
+---------------------+--------------------------------------+------------------------+
1 row in set, 2 warnings (0.00 sec)
```

> **当左边为NULL 右边无论有什么值,返回结果为NULL**
> **当左边值1不为NULL,右边的值(包含null)如果有值1,则返回1,否则返回null;**

默认情况下mysql不区分大小写----

```sql
mysql> select 'a'='A',binary 'a'='A';
+---------+----------------+
| 'a'='A' | binary 'a'='A' |
+---------+----------------+
|       1 |              0 |
+---------+----------------+
1 row in set (0.00 sec)
```



## like与正则表达式

**LIKE**

like的意思是像,就是长得像的东西来比较

其中会用到两个符号----
（1）‘%’，匹配任何数目的字符，甚至包括零字符。（2）‘_’，只能匹配一个字符。

```sql
mysql> select 'day' like 'day','day' like 'da_','today' like 't%' ,'today' like 't___','today' like 't____';
+------------------+------------------+-------------------+---------------------+----------------------+
| 'day' like 'day' | 'day' like 'da_' | 'today' like 't%' | 'today' like 't___' | 'today' like 't____' |
+------------------+------------------+-------------------+---------------------+----------------------+
|                1 |                1 |                 1 |                   0 |                    1 |
+------------------+------------------+-------------------+---------------------+----------------------+
```



**正则  regexp**
回想java中的正则-------

**REGEXP运算符在进行匹配时，常用的有下面几种通配符：
（1）‘^’匹配以该字符后面的字符开头的字符串。（2）‘$’匹配以该字符前面的字符结尾的字符串。

```sql
mysql> select 'helloword' regexp('^h'),'hello' regexp('o$');
+--------------------------+----------------------+
| 'helloword' regexp('^h') | 'hello' regexp('o$') |
+--------------------------+----------------------+
|                        1 |                    1 |
+--------------------------+----------------------+
1 row in set (0.00 sec)
```

（3）‘.’匹配任何一个单字符。
（4）“[...]”匹配在方括号内的任何字符。
例如，“[abc]”匹配“a”“b”或“c”。为了命名字符的范围，使用一个‘-’。“[a-z]”匹配任何字母，而“[0-9]”匹配任何数字。
（5）‘*’匹配零个或多个在它前面的字符。例如，“x*”匹配任何数量的‘x’字符，
“[0-9]*”匹配任何数量的数字，而“*”匹配任何数量的任何字符。**



```sql
+--------------------------+--------------------------------+------------------------------------+
| 'hello' regexp ('hell.') | 'hello' regexp ('^[f-k]ello$') | '10086Hello' regexp('[0-9]*.ello') |
+--------------------------+--------------------------------+------------------------------------+
|                        1 |                              1 |                                  1 |
+--------------------------+--------------------------------+------------------------------------+
1 row in set (0.00 sec)
```

> **正则表达式是一个可以进行复杂查询的强大工具。相对于LIKE字符串匹配，它可以使用更多的通配符类型，查询结果更加灵活。**

## 逻辑运算符

![在这里插入图片描述](https://img-blog.csdnimg.cn/bc297da276414af3970140065d014033.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**逻辑非    -----!或者NOT**

```sql
mysql> select  not 0 ,not 1,not 10,not 'a', !0,!'a', !1,not null,!null;
+-------+-------+--------+---------+----+------+----+----------+-------+
| not 0 | not 1 | not 10 | not 'a' | !0 | !'a' | !1 | not null | !null |
+-------+-------+--------+---------+----+------+----+----------+-------+
|     1 |     0 |      0 |       1 |  1 |    1 |  0 |     NULL |  NULL |
+-------+-------+--------+---------+----+------+----+----------+-------+
1 row in set, 6 warnings (0.00 sec)
```

> **当为数字时,非0 返回1,0则返回1; 
> 当为字符串时 返回1;
> 当为null时返回null;**

```sql
mysql> select !1+1,!(1+1);
+------+--------+
| !1+1 | !(1+1) |
+------+--------+
|    1 |      0 |
+------+--------+
1 row in set, 2 warnings (0.00 sec)
```

> ***运算优先级--- ! 大于 算数运算符***

**逻辑与    -----and或者&&**

```sql
mysql> select 1 and 0,'a' and 'b',null and 1,null and 0,0 and null, 1 and null, null and null,1 and 'a';
+---------+-------------+------------+------------+------------+------------+---------------+-----------+
| 1 and 0 | 'a' and 'b' | null and 1 | null and 0 | 0 and null | 1 and null | null and null | 1 and 'a' |
+---------+-------------+------------+------------+------------+------------+---------------+-----------+
|       0 |           0 |       NULL |          0 |          0 |       NULL |          NULL |         0 |
+---------+-------------+------------+------------+------------+------------+---------------+-----------+
1 row in set, 2 warnings (0.00 sec)
```

**当为数时----**
非零值并且不为NULL时，返回1；
当有0时(包含0 and null)，返回0；
其余情况返回值为NULL。
**当有字符串时----**
返回0;

```sql
mysql> select 0 and'a',null and 'a';
+----------+--------------+
| 0 and'a' | null and 'a' |
+----------+--------------+
|        0 |            0 |
+----------+--------------+
1 row in set, 1 warning (0.00 sec)

mysql> select 0 and'a',null and 'a','a' and 'a';
+----------+--------------+-------------+
| 0 and'a' | null and 'a' | 'a' and 'a' |
+----------+--------------+-------------+
|        0 |            0 |           0 |
+----------+--------------+-------------+
1 row in set, 2 warnings (0.00 sec)

mysql> select '0' and'a',null and 'a','a' and 'a';
+------------+--------------+-------------+
| '0' and'a' | null and 'a' | 'a' and 'a' |
+------------+--------------+-------------+
|          0 |            0 |           0 |
+------------+--------------+-------------+
1 row in set, 2 warnings (0.00 sec)
```

**逻辑或运算or**

```sql
mysql> select 1 or 1,1  or 0 ,0 or 0,'a' or 'a',1 or 'a' ,0 or 'a' ,'1' or 0,null or null,1 or null,'0' or null,'1' or null;
+--------+---------+--------+------------+----------+----------+----------+--------------+-----------+-------------+-------------+
| 1 or 1 | 1  or 0 | 0 or 0 | 'a' or 'a' | 1 or 'a' | 0 or 'a' | '1' or 0 | null or null | 1 or null | '0' or null | '1' or null |
+--------+---------+--------+------------+----------+----------+----------+--------------+-----------+-------------+-------------+
|      1 |       1 |      0 |          0 |        1 |        0 |        1 |         NULL |         1 |        NULL |           1 |
+--------+---------+--------+------------+----------+----------+----------+--------------+-----------+-------------+-------------+
1 row in set, 3 warnings (0.00 sec)

```

有1 的时候返回1

**逻辑异或运算符XOR**

```sql
mysql> select 1 xor 1,1 xor 0,2 xor 2,0 xor 0, 'a' xor 'a','a' xor 'b', 1 xor null, 0 xor null, null xor null,1 xor 'a',0 xor 'a';
+---------+---------+---------+---------+-------------+-------------+------------+------------+---------------+-----------+-----------+
| 1 xor 1 | 1 xor 0 | 2 xor 2 | 0 xor 0 | 'a' xor 'a' | 'a' xor 'b' | 1 xor null | 0 xor null | null xor null | 1 xor 'a' | 0 xor 'a' |
+---------+---------+---------+---------+-------------+-------------+------------+------------+---------------+-----------+-----------+
|       0 |       1 |       0 |       0 |           0 |           0 |       NULL |       NULL |          NULL |         1 |         0 |
+---------+---------+---------+---------+-------------+-------------+------------+------------+---------------+-----------+-----------+
1 row in set, 6 warnings (0.00 sec)
```

> **1,如果有null则返回 null; 
> 2,1 xor 0 返回1; 
> 3,其他情况返回0**

```sql
mysql> select  1 xor 2 ,!1 and 2,1 and !2;
+---------+----------+----------+
| 1 xor 2 | !1 and 2 | 1 and !2 |
+---------+----------+----------+
|       0 |        0 |        0 |
+---------+----------+----------+
1 row in set, 2 warnings (0.00 sec)
```

由于!的优先级时最高的,所以没加 括号,不过最好是加括号以便于阅读;

> **a XOR b的计算等同于(a AND (NOT b))或者((NOT a)AND b)。**

## 位运算

![在这里插入图片描述](https://img-blog.csdnimg.cn/fbca3aef487249aa9c662a22b7af90c2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
位操作跟Java中是一样的,

```sql
mysql> select 1|2,2 & 3,4 ^ 5,6 << 7, 7 >> 8,~2;
+-----+-------+-------+--------+--------+----------------------+
| 1|2 | 2 & 3 | 4 ^ 5 | 6 << 7 | 7 >> 8 | ~2                   |
+-----+-------+-------+--------+--------+----------------------+
|   3 |     2 |     1 |    768 |      0 | 18446744073709551613 |
+-----+-------+-------+--------+--------+----------------------+
1 row in set (0.00 sec)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/365a2441022f43fca45421e8c0953ff8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

关于字符串的逻辑比较-----
虽然可以比较,但是当数字跟字符串比较时,会出现警告;

```sql
mysql> select 1 and 'a';
+-----------+
| 1 and 'a' |
+-----------+
|         0 |
+-----------+
1 row in set, 1 warning (0.00 sec)

mysql> show warnings;
+---------+------+---------------------------------------+
| Level   | Code | Message                               |
+---------+------+---------------------------------------+
| Warning | 1292 | Truncated incorrect DOUBLE value: 'a' |
+---------+------+---------------------------------------+
1 row in set (0.00 sec)
```





# 数学函数

## 平方根与取余

```sql
mysql> select abs(-100)=abs(100),pi(),sqrt(9),sqrt(2),sqrt(-100),mod(12,11),mod(12.5,10);
+--------------------+----------+---------+--------------------+------------+------------+--------------+
| abs(-100)=abs(100) | pi()     | sqrt(9) | sqrt(2)            | sqrt(-100) | mod(12,11) | mod(12.5,10) |
+--------------------+----------+---------+--------------------+------------+------------+--------------+
|                  1 | 3.141593 |       3 | 1.4142135623730951 |       NULL |          1 |          2.5 |
+--------------------+----------+---------+--------------------+------------+------------+--------------+
1 row in set (0.00 sec)
```

1,负数没有平方根,所以返回值为null;
2,MOD(x,y)返回x被y除后的余数，MOD()对于带有小数部分的数值也起作用，它返回除法运算后的精确余数。
mod(12.5,10)  12.5除以10 商1余2.5;

## 最小整数与最大整数

```sql
mysql> select ceil(18.9999),ceil(-18.9999),ceiling(18.9999),ceiling(-18.9999);
+---------------+----------------+------------------+-------------------+
| ceil(18.9999) | ceil(-18.9999) | ceiling(18.9999) | ceiling(-18.9999) |
+---------------+----------------+------------------+-------------------+
|            19 |            -18 |               19 |               -18 |
+---------------+----------------+------------------+-------------------+
1 row in set (0.00 sec)
```

ceil(X)与ceiling(X)
两者都表示------返回不小于X的最小整数值，返回值转化为一个BIGINT。
注解----ceiling  为天花板的意思,可以简写为ceil

```sql
mysql> select floor(10.2),floor(-10.2);
+-------------+--------------+
| floor(10.2) | floor(-10.2) |
+-------------+--------------+
|          10 |          -11 |
+-------------+--------------+
1 row in set (0.00 sec)
```

FLOOR(x)返回不大于x的最大整数值，返回值转化为一个BIGINT。
floor 为地板的意思,不可简写;

## 随机函数---rand

```sql
mysql> select rand(),rand(),rand(12),rand(12),rand(13);
+--------------------+--------------------+---------------------+---------------------+---------------------+
| rand()             | rand()             | rand(12)            | rand(12)            | rand(13)            |
+--------------------+--------------------+---------------------+---------------------+---------------------+
| 0.9349382686754114 | 0.2838225227629976 | 0.15741774081943347 | 0.15741774081943347 | 0.40760085024647497 |
+--------------------+--------------------+---------------------+---------------------+---------------------+
1 row in set (0.00 sec)
```

rand()与rand(x)的异同点---
1,都是返回0-1之间的值
2,rand(x)中的参数x被视作种子,如果该种子相同,则返回的值是一样的;(如果重新启用数据库则rand(x)会返回跟第一不同的值,

```sql
mysql> select rand(12)=rand(12),rand(12);
+-------------------+---------------------+
| rand(12)=rand(12) | rand(12)            |
+-------------------+---------------------+
|                 1 | 0.15741774081943347 |
+-------------------+---------------------+
1 row in set (0.00 sec)
```

## 四舍五入函数----round

```sql
mysql> select  round(0.123),round(0.123,2),truncate(0.123,2);
+--------------+----------------+-------------------+
| round(0.123) | round(0.123,2) | truncate(0.123,2) |
+--------------+----------------+-------------------+
|            0 |           0.12 |              0.12 |
+--------------+----------------+-------------------+
1 row in set (0.00 sec)

mysql> select  round(-0.123),round(-0.123,2),truncate(-0.123,2);
+---------------+-----------------+--------------------+
| round(-0.123) | round(-0.123,2) | truncate(-0.123,2) |
+---------------+-----------------+--------------------+
|             0 |           -0.12 |              -0.12 |
+---------------+-----------------+--------------------+
1 row in set (0.00 sec)

mysql> select  round(0.123),round(0.123,-2),truncate(0.123,-2);
+--------------+-----------------+--------------------+
| round(0.123) | round(0.123,-2) | truncate(0.123,-2) |
+--------------+-----------------+--------------------+
|            0 |               0 |                  0 |
+--------------+-----------------+--------------------+
1 row in set (0.00 sec)

mysql> select round (23.38,-1),round (232.38,-2);
+------------------+-------------------+
| round (23.38,-1) | round (232.38,-2) |
+------------------+-------------------+
|               20 |               200 |
+------------------+-------------------+
1 row in set (0.00 sec)
```

ROUND(x,y)返回最接近于参数x的数，其值保留到小数点后面y位，若y为负值，则将保留x值到小数点左边y位。

例如---round(23.38,-1)  表示   保留小数点前1位(舍弃小数部分),即个位如果大于5,进1,小于5则舍弃直接保存位0,所以结果为20;

**y值为负数时，保留的小数点左边的相应位数直接保存为0，不进行四舍五入。**

TRUNCATE(x,y)返回被舍去至小数点后y位的数字x。若y的值为0，则结果不带有小数点或不带有小数部分。若y设为负数，则截去（归零）x小数点左起第y位开始后面所有低位的值。

## 符号函数

**判断一个数的正负------符号函数**

```sql
mysql> select sign(-10000),sign(12.70),sign(0);
+--------------+-------------+---------+
| sign(-10000) | sign(12.70) | sign(0) |
+--------------+-------------+---------+
|           -1 |           1 |       0 |
+--------------+-------------+---------+
1 row in set (0.00 sec)
```

## 幂运算函数

POW(x,y)、POWER(x,y)返回x的y次方的值

EXP(x)--返回e的x乘方后的值。

```sql
mysql> select POW(2,2),power(2,3),exp(2),pow(2,0),power(2,-1),exp(0);
+----------+------------+------------------+----------+-------------+--------+
| POW(2,2) | power(2,3) | exp(2)           | pow(2,0) | power(2,-1) | exp(0) |
+----------+------------+------------------+----------+-------------+--------+
|        4 |          8 | 7.38905609893065 |        1 |         0.5 |      1 |
+----------+------------+------------------+----------+-------------+--------+
1 row in set (0.00 sec)
```

## 对数函数

```sql
mysql> select log(100),log10(100),log(-10);
+-------------------+------------+----------+
| log(100)          | log10(100) | log(-10) |
+-------------------+------------+----------+
| 4.605170185988092 |          2 |     NULL |
+-------------------+------------+----------+
1 row in set, 1 warning (0.00 sec)

mysql> show warnings;
+---------+------+--------------------------------+
| Level   | Code | Message                        |
+---------+------+--------------------------------+
| Warning | 3020 | Invalid argument for logarithm |
+---------+------+--------------------------------+
1 row in set (0.00 sec)
```

## 角度和弧度函数

```sql
mysql> select degrees(pi()),degrees(pi()/2);
+---------------+-----------------+
| degrees(pi()) | degrees(pi()/2) |
+---------------+-----------------+
|           180 |              90 |
+---------------+-----------------+
1 row in set (0.00 sec)
mysql> select radians(180),radians(90);
+-------------------+--------------------+
| radians(180)      | radians(90)        |
+-------------------+--------------------+
| 3.141592653589793 | 1.5707963267948966 |
+-------------------+--------------------+
1 row in set (0.00 sec)

mysql> select pi()=radians(180);
+-------------------+
| pi()=radians(180) |
+-------------------+
|                 1 |
+-------------------+
1 row in set (0.00 sec)
```

## 三角函数

```sql
mysql> select sin(pi()/2),asin(1),cos(pi()),acos(1), tan(pi()/3),atan(1),cot(pi());
+-------------+--------------------+-----------+---------+--------------------+--------------------+-----------------------+
| sin(pi()/2) | asin(1)            | cos(pi()) | acos(1) | tan(pi()/3)        | atan(1)            | cot(pi())             |
+-------------+--------------------+-----------+---------+--------------------+--------------------+-----------------------+
|           1 | 1.5707963267948966 |        -1 |       0 | 1.7320508075688767 | 0.7853981633974483 | -8.165619676597685e15 |
+-------------+--------------------+-----------+---------+--------------------+--------------------+-----------------------+
1 row in set (0.00 sec)
```

跟数学上的一样,不在解释赘述;

# 字符串函数

## 字符串长度函数

```sql
mysql> select char_length('qwert'),length('qwert');
+----------------------+-----------------+
| char_length('qwert') | length('qwert') |
+----------------------+-----------------+
|                    5 |               5 |
+----------------------+-----------------+
1 row in set (0.00 sec)

mysql> select char_length('qwert'),length(12345);
+----------------------+---------------+
| char_length('qwert') | length(12345) |
+----------------------+---------------+
|                    5 |             5 |
+----------------------+---------------+
1 row in set (0.00 sec)

mysql> select char_length('qwert'),length(123459),length('张三');
+----------------------+----------------+----------------+
| char_length('qwert') | length(123459) | length('张三') |
+----------------------+----------------+----------------+
|                    5 |              6 |              4 |
+----------------------+----------------+----------------+
1 row in set (0.00 sec)


```

**一个字母/数组占一个字节,一个汉字占2个字节;**

## 合并字符串函数

**CONCAT(s1,s2,…)**

```sql
mysql> select concat('q',1,'wer',null),concat('q','werr',1,2,3);
+--------------------------+--------------------------+
| concat('q',1,'wer',null) | concat('q','werr',1,2,3) |
+--------------------------+--------------------------+
| NULL                     | qwerr123                 |
+--------------------------+--------------------------+
1 row in set (0.00 sec)
```

有null则返回null

![在这里插入图片描述](https://img-blog.csdnimg.cn/71b5ff80258a451db31767fff1828b3c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**CONCAT_WS(M,s1,s2,…)**
M---表示连接符

```sql
mysql> select concat_ws('-','q',1,'wer'),concat_ws('\'','q','werr',1,2,3);
+----------------------------+----------------------------------+
| concat_ws('-','q',1,'wer') | concat_ws('\'','q','werr',1,2,3) |
+----------------------------+----------------------------------+
| q-1-wer                    | q'werr'1'2'3                     |
+----------------------------+----------------------------------+
```

有NULL值则不添加(即为空)

## 替换字符串的函数

```sql
mysql> select insert('hello',2,3,'world');
+-----------------------------+
| insert('hello',2,3,'world') |
+-----------------------------+
| hworldo                     |
+-----------------------------+
1 row in set (0.00 sec)

mysql> select insert('hello',2,-3,'world');
+------------------------------+
| insert('hello',2,-3,'world') |
+------------------------------+
| hworld                       |
+------------------------------+
1 row in set (0.00 sec)

mysql> select insert('hello',2,-1,'world');
+------------------------------+
| insert('hello',2,-1,'world') |
+------------------------------+
| hworld                       |
+------------------------------+
1 row in set (0.00 sec)

mysql> select insert('hellohello',2,-1,'world');
+-----------------------------------+
| insert('hellohello',2,-1,'world') |
+-----------------------------------+
| hworld                            |
+-----------------------------------+
1 row in set (0.00 sec)

mysql> select insert('hello',2,30,'world');
+------------------------------+
| insert('hello',2,30,'world') |
+------------------------------+
| hworld                       |
+------------------------------+
1 row in set (0.00 sec)

mysql> select insert('hello',-2,3,'world');
+------------------------------+
| insert('hello',-2,3,'world') |
+------------------------------+
| hello                        |
+------------------------------+
1 row in set (0.00 sec)

mysql>
```

**insert (x,a,b,y)**

当a<0 的 时候直接返回该字符串

```sql
mysql> select insert('hello',-2,3,'world');
+------------------------------+
| insert('hello',-2,3,'world') |
+------------------------------+
| hello                        |
+------------------------------+
```

当a>0的时候且b大于0表示从该字符串第a的位置取b个字符串替换为字符串y,剩下的保留;

```sql
 insert('hello',2,3,'world') |
+-----------------------------+
| hworldo
```

当a>0的时候且b小于0表示从字符串第a的位置开始插入字符串y剩下的不保留;

```sql
mysql> select insert('hellohello',2,-1,'world');
+-----------------------------------+
| insert('hellohello',2,-1,'world') |
+-----------------------------------+
| hworld                            |
+-----------------------------------+
```

**insert(x,a,b,d) 参数的含义是将字符串x的第a到b位替换为d,不管a到b有多少字符;**

以上是通过插入方式来替换字符串,
**还有一种是替换的方式**
将字符串中不的某字符串替换为另一个字符串
如----将'xxxx.csdn.net',中的x 替换为 w

```sql
mysql> select replace('xxxx.csdn.net','x','w');
+----------------------------------+
| replace('xxxx.csdn.net','x','w') |
+----------------------------------+
| wwww.csdn.net                    |
+----------------------------------+
1 row in set (0.00 sec)

mysql> select replace('xxxx.csdn.net','x','ww');
+-----------------------------------+
| replace('xxxx.csdn.net','x','ww') |
+-----------------------------------+
| wwwwwwww.csdn.net                 |
+-----------------------------------+
1 row in set (0.00 sec)
```

## 大小写转换函数

```sql
mysql> select lower('HELLO'),Lcase('WORLD');
+----------------+----------------+
| lower('HELLO') | Lcase('WORLD') |
+----------------+----------------+
| hello          | world          |
+----------------+----------------+
1 row in set (0.00 sec)


mysql> select upper('hello'),Ucase('hello');
+----------------+----------------+
| upper('hello') | Ucase('hello') |
+----------------+----------------+
| HELLO          | HELLO          |
+----------------+----------------+
1 row in set (0.00 sec)
```

## 获取指定长度的字符串的函数

```sql
mysql> select left('hello',3),left('hello',-3),left('hello',10);
+-----------------+------------------+------------------+
| left('hello',3) | left('hello',-3) | left('hello',10) |
+-----------------+------------------+------------------+
| hel             |                  | hello            |
+-----------------+------------------+------------------+
1 row in set (0.00 sec)


mysql> select right('hello',3),right('hello',-3),right('hello',10);
+------------------+-------------------+-------------------+
| right('hello',3) | right('hello',-3) | right('hello',10) |
+------------------+-------------------+-------------------+
| llo              |                   | hello             |
+------------------+-------------------+-------------------+
1 row in set (0.00 sec)
```

**left(a,x)----取字符串左边的x位为组成新的字符串;
如果x小于0 ,则返回空字符串
如果x大于字符串长度,则返回当前字符串;**

**right(a,x)------取字符串右边的x位组成新的字符串;
如果x小于0,则返回空字符串
如果x大于字符串长度,则返回当前字符串;**

## 填充字符串的函数

### LPAD(s1,len,s2)

-----LPAD(s1,len,s2)返回字符串s1，其左边由字符串s2填补到len字符长度。假如s1的长度大于len，则返回值被缩短至len字符。

```sql
mysql> select lpad('hello',5,'word');
+------------------------+
| lpad('hello',5,'word') |
+------------------------+
| hello                  |
+------------------------+
1 row in set (0.00 sec)

mysql> select lpad('hello',9,'word');
+------------------------+
| lpad('hello',9,'word') |
+------------------------+
| wordhello              |
+------------------------+
1 row in set (0.00 sec)

mysql> select lpad('hello',8,'word');
+------------------------+
| lpad('hello',8,'word') |
+------------------------+
| worhello               |
+------------------------+
1 row in set (0.00 sec)

mysql> select lpad('hello',3,'word');
+------------------------+
| lpad('hello',3,'word') |
+------------------------+
| hel                    |
+------------------------+
1 row in set (0.00 sec)
```

**select lpad('hello',3,'word');**  ----表示字符串 hello 将由word字符串填充至长度位3,hello字符串长度位5,比3大,所以不用填充,而是直接截取至长度为3的自字符串长度并返回该字符串 'hel'
**lpad('hello',8,'word')**----表示字符串 hello 将由word字符串填充至长度位8,hello字符串长度位5,不足8,所以剩下的3位由字符串word填充,所以取word前三位就够了,最后返回 字符串 'worhello'

### RPAD(s1,len,s2)

RPAD(s1,len,s2)返回字符串sl，其右边被字符串s2填补至len字符长度。假如字符串s1的长度大于len，则返回值被缩短到len字符长度。

```sql
mysql> select rpad('hello',5,'world');
+-------------------------+
| rpad('hello',5,'world') |
+-------------------------+
| hello                   |
+-------------------------+
1 row in set (0.00 sec)

mysql> select rpad('hello',10,'world');
+--------------------------+
| rpad('hello',10,'world') |
+--------------------------+
| helloworld               |
+--------------------------+
1 row in set (0.00 sec)

mysql> select rpad('hello',1,'world');
+-------------------------+
| rpad('hello',1,'world') |
+-------------------------+
| h                       |
+-------------------------+
1 row in set (0.00 sec)
```

原理跟lpad一样,只不过是放在填充之后的字符串放在右边;

## 删除空格的函数

trim(x)--------删除字符串两边的空格
ltrim(x)-------------删除字符串左边的空格
rtrim(x)--------------删除字符串右边的空格

```sql
mysql> select length(trim('   hello   ')),length(ltrim('     hello     ')),length(rtrim('     hello     '));
+-----------------------------+----------------------------------+----------------------------------+
| length(trim('   hello   ')) | length(ltrim('     hello     ')) | length(rtrim('     hello     ')) |
+-----------------------------+----------------------------------+----------------------------------+
|                           5 |                               10 |                               10 |
+-----------------------------+----------------------------------+----------------------------------+
1 row in set (0.00 sec)
```

## 删除指定字符串的函数

TRIM(A FROM B);-----在字符串B中删除两边的字符串A,如果没有则返回原来字符串;

```sql
mysql> select trim('o' from 'ohelloworldO'),trim('o' from 'HELLOWORLDO');
+-------------------------------+------------------------------+
| trim('o' from 'ohelloworldO') | trim('o' from 'HELLOWORLDO') |
+-------------------------------+------------------------------+
| helloworldO                   | HELLOWORLDO                  |
+-------------------------------+------------------------------+
1 row in set (0.00 sec)
```

## 重复生成字符串的函数

 **repeat('hello',4)------**
 生成字符串hello 4次

```sql
mysql> select repeat('hello',4),repeat('hello',0),repeat('hello',-2);
+----------------------+-------------------+--------------------+
| repeat('hello',4)    | repeat('hello',0) | repeat('hello',-2) |
+----------------------+-------------------+--------------------+
| hellohellohellohello |                   |                    |
+----------------------+-------------------+--------------------+
1 row in set (0.00 sec)
```

## 空格函数和替换函数

**space(4)**
----生成4个空格

```sql
+---------------------------+------------------+
| concat('(', space(4),')') | length(space(4)) |
+---------------------------+------------------+
| (    )                    |                4 |
+---------------------------+------------------+
1 row in set (0.00 sec)

```

 **replace('aaa.baidu.com','a','w'),**
 将字符串'aaa.baidu.com',中的a替换为'w'

```sql
mysql> select replace('aaa.baidu.com','a','w');
+----------------------------------+
| replace('aaa.baidu.com','a','w') |
+----------------------------------+
| www.bwidu.com                    |
+----------------------------------+
1 row in set (0.00 sec)

```

## 比较字符串大小的函数----strcmp()

strcmp----即stringcompare的缩写

```sql
mysql> select strcmp('hello','HELLO'),STRCMP(null,'HELLO'),STRCMP('12A','12B');
+-------------------------+----------------------+---------------------+
| strcmp('hello','HELLO') | STRCMP(null,'HELLO') | STRCMP('12A','12B') |
+-------------------------+----------------------+---------------------+
|                       0 |                 NULL |                  -1 |
+-------------------------+----------------------+---------------------+
1 row in set (0.00 sec)
```

相同返回0------不区分大小写
有null返回null,
其他情况----
由第一个不同的字符进行比较,
大于返回1,小于返回-1

如-----
STRCMP('12A','12B')第一个不同的字符----A 与B,(ASCII码)A<B返回-1;

```sql
mysql> select strcmp('qweeewrewre','qweweewqeqwe');
+--------------------------------------+
| strcmp('qweeewrewre','qweweewqeqwe') |
+--------------------------------------+
|                                   -1 |
+--------------------------------------+
1 row in set (0.00 sec)

```

## 获取子串的函数SUBSTRING(s,n,len)和MID(s,n,len)

```sql
mysql> select substring('helloworld',3),substring('helloworld',3,5),substring('helloworld',3,-2),substring('helloworld',-3,2),substring('helloworld',-3,-2);
+---------------------------+-----------------------------+------------------------------+------------------------------+-------------------------------+
| substring('helloworld',3) | substring('helloworld',3,5) | substring('helloworld',3,-2) | substring('helloworld',-3,2) | substring('helloworld',-3,-2) |
+---------------------------+-----------------------------+------------------------------+------------------------------+-------------------------------+
| lloworld                  | llowo                       |                              | rl                           |                               |
+---------------------------+-----------------------------+------------------------------+------------------------------+-------------------------------+
1 row in set (0.00 sec)
```

substring('helloworld',3)----从第三位开始返回到结尾的字符串
 substring('helloworld',3,5)----返会从第3 位开始向后取5个字符串
 substring('helloworld',3,-2)-----从正数第三位开始向后取两个字符串----不符合逻辑,返回空
 substring('helloworld',-3,2)----返回从倒数第三位开始正数2个字符串

mid()和substring()效果一样;

```sql
select mid('helloworld',3),mid('helloworld',3,5),mid('helloworld',3,-2),mid('helloworld',-3,2),mid('helloworld',-3,-2);
+---------------------+-----------------------+------------------------+------------------------+-------------------------+
| mid('helloworld',3) | mid('helloworld',3,5) | mid('helloworld',3,-2) | mid('helloworld',-3,2) | mid('helloworld',-3,-2) |
+---------------------+-----------------------+------------------------+------------------------+-------------------------+
| lloworld            | llowo                 |                        | rl                     |                         |
+---------------------+-----------------------+------------------------+------------------------+-------------------------+
```

**> 注释-----之前将 substring('helloworld',3,5)----返会从第3 位开始向后取5个字符串       -------注解错误,抱歉;**

## 匹配子串开始位置的函数

**locate('hello' ,'helloworldhello')------**
返回hello字符串在'helloworldhello'字符串中的位置;

**position('hello' in 'helloworldhello')-----------**
返回hello字符串在'helloworldhello'字符串中的位置; ----注意格式

**instr('helloworld' ,'hello')**
在'helloworld'字符串中返回   字符串hello的位置----注意格式

```sql
mysql> select locate('hello' ,'helloworldhello'),position('hello' in 'helloworldhello'),instr('helloworld' ,'hello');
+------------------------------------+----------------------------------------+------------------------------+
| locate('hello' ,'helloworldhello') | position('hello' in 'helloworldhello') | instr('helloworld' ,'hello') |
+------------------------------------+----------------------------------------+------------------------------+
|                                  1 |                                      1 |                            1 |
+------------------------------------+----------------------------------------+------------------------------+
1 row in set (0.00 sec)
```

## 字符串逆序的函数---reverse()

```sql
mysql> select reverse('hello');
+------------------+
| reverse('hello') |
+------------------+
| olleh            |
+------------------+
1 row in set (0.00 sec)
```

## 返回指定位置的字符串---elt()

> ELT(N，字符串1，字符串2，字符串3，...，字符串N)：若N =
> 1，则返回值为字符串1；若N=2，则返回值为字符串2；以此类推；若N小于1或大于参数的数目，则返回值为NULL。

![在这里插入图片描述](https://img-blog.csdnimg.cn/1497728cf18343b9b1a93dd4539eb887.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 返回指定字符串位置的函数---field()

```sql
mysql> select field(2,'1','2','3'),field(2,'1','20','3'),field(2,'1','2','3',null);
+----------------------+-----------------------+---------------------------+
| field(2,'1','2','3') | field(2,'1','20','3') | field(2,'1','2','3',null) |
+----------------------+-----------------------+---------------------------+
|                    2 |                     0 |                         2 |
+----------------------+-----------------------+---------------------------+
1 row in set (0.00 sec)
```

## 返回子串位置的函数FIND_IN_SET(s1,s2)

FIND_IN_SET(s1,s2)----返回s1在s2中的位置,
如果s1为null,则返回null
如果s1为非null,但是s2中没有字串s1,则返回0;

```sql
mysql> select find_in_set('hello','你好,hello,world,hello') as ss,find_in_set('hello','你好,hello,world,hello,null') as s2;
+----+----+
| ss | s2 |
+----+----+
|  2 |  2 |
+----+----+
1 row in set (0.00 sec)

mysql> select find_in_set('hello','你好,hello,world,hello') as ss,find_in_set(null,'你好,hello,world,hello,null') as s2;
+----+------+
| ss | s2   |
+----+------+
|  2 | NULL |
+----+------+
1 row in set (0.00 sec)

mysql> select find_in_set('hello','你好,hello,world,hello') as ss,find_in_set('null','你好,hello,world,hello,null') as s2;
+----+----+
| ss | s2 |
+----+----+
|  2 |  5 |
+----+----+
1 row in set (0.00 sec)

```

## 选取字符串的函数MAKE_SET(x,s1,s2,…,sn)

```sql
mysql> select make_set(1|2,'q','w','e','r','t');
+-----------------------------------+
| make_set(1|2,'q','w','e','r','t') |
+-----------------------------------+
| q,w                               |
+-----------------------------------+
1 row in set (0.00 sec)
```

1|2的值为3,二进制 0011,从右到左第一个和第二个为1,所以make_set(1|2,'q','w','e','r','t') 返回字符q,w

## 数字格式化函数函数

**format(x,n)**
表示将数字x保留小数点后n位(四舍五入);

```sql
mysql> select format(12333.3455,3);
+----------------------+
| format(12333.3455,3) |
+----------------------+
| 12,333.346           |
+----------------------+
1 row in set (0.00 sec)

mysql> select format(pi(),5), format(10.898989,1);
+----------------+---------------------+
| format(pi(),5) | format(10.898989,1) |
+----------------+---------------------+
| 3.14159        | 10.9                |
+----------------+---------------------+
1 row in set (0.00 sec)

```

## 进制准换

**conv(原数,原数的进制数,要转换为的进制数)**

 conv(10,10,2)表示10进制的10转换为2进制的数  结果为1010;

```sql
mysql> select conv(10,10,2),conv(10,16,8),conv(88,10,16);
+---------------+---------------+----------------+
| conv(10,10,2) | conv(10,16,8) | conv(88,10,16) |
+---------------+---------------+----------------+
| 1010          | 20            | 58             |
+---------------+---------------+----------------+
1 row in set (0.00 sec)
```

进制说明：
●　二进制，采用0和1两个数字来表示的数。它以2为基数，逢二进一。
●　八进制，采用0、1、2、3、4、5、6、7八个数字，逢八进一，以数字0开头。
●　十进制，采用0~9，共10个数字表示，逢十进一。
●　十六进制，由0-9,A-F组成，以数字0x开头。

## IP地址与数字相互转换的函数

inet_aton('ip地址')---------将该ip地址转换为一个数字
inet_ntoa(数字)----------将该数字转换为IP

```sql
mysql> select inet_aton('192.168.1.1'),inet_ntoa(inet_aton('192.168.1.1'));
+--------------------------+-------------------------------------+
| inet_aton('192.168.1.1') | inet_ntoa(inet_aton('192.168.1.1')) |
+--------------------------+-------------------------------------+
|               3232235777 | 192.168.1.1                         |
+--------------------------+-------------------------------------+
1 row in set (0.00 sec)
```

## 加锁和解锁函数

```sql
mysql> select get_lock('locked','Helloworld') as '加锁',
    -> is_used_lock ('locekd') as '正在被使用?',
    -> is_free_lock ('locked') as '没有锁?',
    -> release_lock('locked') as '释放了锁?';
+------+-------------+---------+-----------+
| 加锁 | 正在被使用? | 没有锁? | 释放了锁? |
+------+-------------+---------+-----------+
|    1 |        NULL |       0 |         1 |
+------+-------------+---------+-----------+
1 row in set, 1 warning (0.00 sec)（connection ID）；否则，返回NULL。
```

get_lock('locked','Helloworld'),给字符串'Helloworld'加一个锁,名为locked,超时为0;
is_used_lock ('locekd') 名为'locked'的锁在被使用吗?(如果没有叫locked'的锁,也返回null)没有返回null,有的话则返回使用该字符串的用户id,
is_free_lock('locked') 名为locked的锁可以使用吗?---是返回1,不是返回0,没有叫locked的锁则返回null
release_lock('locked')返回值为1，说明解锁成功.返回0则表是不成功,没有叫locked的锁则返回null;

## 重复执行指定操作的函数

**BENCHMARK(count,expr)**
函数重复count次执行表达式expr

```sql
mysql> select md5('尔曹身与名俱灭,不废江河万古流');
+--------------------------------------+
| md5('尔曹身与名俱灭,不废江河万古流') |
+--------------------------------------+
| 4d0bbdfc9c117005699ca0eb91d27f26     |
+--------------------------------------+
1 row in set (0.00 sec)

mysql> select benchmark(50000000,md5('尔曹身与名俱灭,不废江河万古流'));
+----------------------------------------------------------+
| benchmark(50000000,md5('尔曹身与名俱灭,不废江河万古流')) |
+----------------------------------------------------------+
|                                                        0 |
+----------------------------------------------------------+
1 row in set (6.58 sec)
```

> **BENCHMARK报告的时间是客户端经过的时间，而不是在服务器端的CPU时间，每次执行后报告的时间并不一定是相同的。读者可以多次执行该语句，查看结果。**

## 字符编码转换

```sql
mysql> select convert('hello' using latin1);
+-------------------------------+
| convert('hello' using latin1) |
+-------------------------------+
| hello                         |
+-------------------------------+
1 row in set (0.00 sec)


mysql> select charset('hello'),charset(convert('hello' using latin2));
+------------------+----------------------------------------+
| charset('hello') | charset(convert('hello' using latin1)) |
+------------------+----------------------------------------+
| gbk              | latin1                                 |
+------------------+----------------------------------------+
1 row in set (0.00 sec)


mysql> select convert('你好' using gbk);
+---------------------------+
| convert('你好' using gbk) |
+---------------------------+
| 你好                      |
+---------------------------+
1 row in set (0.00 sec)
```

## 改变数据类型的函数

> CAST(x, AS type)和CONVERT(x,
> type)函数将一个类型的值转换为另一个类型的值，可转换的type有BINARY、CHAR(n)、DATE、TIME、DATETIME、DECIMAL、SIGNED、UNSIGNED。

```sql
mysql> select cast('你好' as char(1)),convert(100,binary);
+-------------------------+------------------------------------------+
| cast('你好' as char(1)) | convert(100,binary)                      |
+-------------------------+------------------------------------------+
| 你                      | 0x313030                                 |
+-------------------------+------------------------------------------+
1 row in set, 1 warning (0.00 sec)
```

<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/mysql/MySQLBase01.md#mysql数据库)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/mysql/MySQLBase03.md#数据的基本操作)

</center>

![](..\pic\div\plant.gif)