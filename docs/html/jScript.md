

# JS简介

> Javascript是一种由Netscape(网景)的LiveScript发展而来的原型化继承的面向对象的动态类型的区分大小写的客户端脚本语言，主要目的是为了解决服务器端语言，比如Perl，遗留的速度问题，为客户提供更流畅的浏览效果。JavaScript的组成包含ECMAScript、DOM、BOM。JS是一种运行于浏览器端上的小脚本语句,可以实现网页如文本内容动,数据动态变化和动画特效等



#  JS特点

JS是运行在浏览器上的一种脚本语言
**1.脚本语言** 

脚本语言是一种简单的程序，规模小,不需要编译,运行快,是由一些ASCII字符构成，可以使用任何一种文本编辑器编写。脚本语言是指在web浏览器内有解释器解释执行的编程语言，每次运行程序的时候，解释器会把程序代码翻译成可执行的格式。一些程序语言（如C、C++、Java等）都必须经过编译，将源代码编译成二进制的可执行文件之后才能运行，而脚本语言不需要事先编译，只要有一个与其相适应的解释器就可以执行。

**2.基于对象的语言**  

面向对象有三大特点（封装，继承，多态）缺一不可。通常"基于对象"是使用对象，但是无法利用现有的对象模板产生新的对象类型，也就是说"基于对象"没有继承的特点。没有了继承的概念也就无从谈论"多态"

**3.事件驱动：**

在网页中执行了某种操作的动作，被称为"事件"(Event)，比如按下鼠标、移动窗口、选择菜单等都可以视为事件。当事件发生后，可能会引起相应的事件响应。

**4.简单性**

变量类型是采用弱类型，并未使用严格的数据类型。var a,b,c;  a=123;  b="abc"; a=b; 

**5.安全性**

JavaScript不能访问本地的硬盘，不能将数据存入到服务器上，不能对网络文档进行修改和删除，只能通过浏览器实现信息浏览或动态交互

**6.跨平台性**

JavaScript依赖于浏览器本身，与操作平台无关， 只要计算机安装了支持JavaScript的浏览器（装有JavaScript解释器），JavaScript程序就可以正确执行。

缺点

各种浏览器支持JavaScript的程度是不一样的，支持和不完全支持JavaScript的 浏览器在浏览同一个带有JavaScript脚本的网页时，效果会有一定的差距，有时甚至会显示不出来。

 

# JS 与java

区别1：公司不同，前身不同

JavaScript是Netscape公司的产品，是为了扩展Netscape Navigator功能而开发的一种可以嵌入Web页面中的基于对象和事件驱动的解释性语言，它的前身是Live Script；Java是SUN公司推出的新一代面向对象的程序设计语言，特别适合于Internet应用程序开发； Java的前身是Oak语言。

 

区别2：基于对象和面向对象

JavaScript是脚本语言，是一种基于对象的语言。本身提供了非常丰富的内部对象供设计人员使用，但不支持继承和多态。Java是面向对象的，是一种真正的面向对象的语言，支持封装、继承和多态。

 

区别3：变量类型强弱不同

Java采用强类型变量检查，即所有变量在编译之前必须声明为某一指定类型。如: int  x=1234;JavaScript中是弱类型变量。统一采用var声明，可赋各种数据类型值。


区别4: 运行的位置不同

Java运行与服务器端的,大型编程语言, JS运行于客户端(浏览器)一种小规模脚本语言

 

HTML和CSS和JS这之间的关系

HTML和CSS和JS都是前端的主要技术,三者各有分工.HTML可以用于制作网页的主体结构,CSS用于给网页做美化,JS用于在网页上添加动态效果



# JS引入

**内嵌式引入**

```html
<!DOCTYPE html>

<html><head>
		<meta charset="utf-8" />
		<title></title>
		<!--内嵌式引入方式
           1在head标签中,用一对script标签,嵌入JS代码
           2type属性可以省略不写
                -->
		<script type="text/javascript">
		/*定义一个函数(方法)*/
		function fun1() {
				/*弹窗提示一点信息 */
				alert("你好")
			}
		</script>
	</head>
	<body>
		<input type="button" value="点我呀" onclick="fun1()" />
	</body>
<html>
```

缺点:
1,只能在当前一个网页中使用,代码复用度低,可维护性低

2 JS代码和HTML代码混合在一个文件中,可阅读性差

**链接式引入**

跟Css类似

将JS代码放入外部JS文件中,通过script标签引入

```html
<!DOCTYPE html>

<html>

	<head>

		<meta charset="UTF-8">

		<title></title>

		<!--链接式 引入外部JS文件

                        提高的代码复用度  

                        降低了代码维护的难度

                        1 一个页面可以同时引入多个不同的JS文件

                        2 script标签一点用于引入外部JS文件,就不能在中间定义内嵌式代码

                        3 一个页面上可以用多个script标签  位置也不是非得放到head标签中不可

                        4src属性可以指向一个网络路径,就是第三种引入方式

                -->

		<script type="text/javascript" src="js/myjs.js"></script>

		<script type="text/javascript" src="js/myjs2.js"></script>

		<!--<script type="text/javascript" src="URL网络路径"></script>

        </head>

        <body>

                <input type="button" value="点我呀" onclick="fun1()" />

                <input type="button" value="点我呀2" onclick="fun2()" />

                <input type="button" value="点我呀3" onclick="fun3()" />

                <script >

                        function fun3(){

                                alert("总能见到你")

                        }

                </script>

        </body>

</html>
```

优点:

代码复用度高,更易于维护代码

注意事项:

1在一个页面上可以同时引入多个JS文件

2每个JS文件的引入都要使用一个独立的script标签

3内嵌式和链接式的引入不能使用同一标签



# JS数据类型

1数值型：

number整数和浮点数统称为数值。例如85或3.1415926等。

2字符串型：

String由0个,1个或多个字符组成的序列。在JavaScript中，用双引号或单引号括起来表示，如"您好"、'学习JavaScript' 等。

3逻辑（布尔）型：

boolean用true或false来表示。

4空（null）值：

表示没有值，用于定义空的或不存在的引用。要注意，空值不等同于空字符串""或0。

5未定义（undefined）值：

它也是一个保留字。表示变量虽然已经声明，但却没有赋值。

6除了以上五种基本的数据类型之外，JavaScript还支持复合数据类型Object，复合数据类型包括对象和数组两种。 

# JS运算符

![](..\html\js.png)

JS中,数字类型都是number,除法的结果中如果没有小数位,直接就是一个整数,如有小数位,才是浮点数

JS中如果出现除零,那么结果是 infinity,而不是报错



```html
 <script>
                        /* 
                         * 能除尽,则默认结果就是一个整数,不能除尽,结果默认就是浮点数
                         * 除零不会出现异常,而是出现 Infinity
                         * 和0取余数,出现NaN   not a number 不是一个数字
                         * */
                        alert(10/3);
                        alert(10/0);
                        alert(10%3);
                        alert(10%0);
                        
                </script>
```

JS取余数运算对于浮点数仍然有效,如果和0取余数,结果是NaN(not a number)

加号同时也是连接运算符,看两端的变量类型,如果都是number那么就是算数中的加法 如果有字符串,那么就是连接符号,如果是布尔类型和number相加,那么会将true转化为1 将false 转化为0

```html
 <script>
                        /*
                         * +号中 如果一段是字符串,就变成了文字拼接
                         * 数字和 boolean类型相加  true会转变成1  false会转变成0  再做数学运算
                         * */
                        var i=1;
                        alert(i+1);
                        alert(1+"1");
                        alert(1+true);
                </script>
```

