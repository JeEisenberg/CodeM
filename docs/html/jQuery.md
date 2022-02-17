![](..\pic\logo2.png)

# jQuery

# jQuery简介

**什么是jQuery?**

目前最流行的JavaScript函数库之一，对JavaScript进行了封装。并不是一门新语言。将常用的、复杂的操作进行函数化封装，直接调用，大大降低了使用JavaScript的难度，改变了使用JavaScript的习惯。jQuery能做的JavaScript也能做，但使用jQuery能大幅提高开发效率,由美国人John Resig在2006年推出，目前最新版本是v3.5.1。宗旨：Write less，do more（写更少代码，做更多事情）。官方网址http://jquery.com/

为什么使用jQuery ?

使用原生JavaScript编码存在问题

1选择器功能弱  

2 DOM操作繁琐之极

3浏览器兼容性不好

4动画功能弱

jQuery的优点

1强大的选择器

2出色的DOM封装

3出色的浏览器兼容性

4强大的动画功能

5体积小，压缩后只有100KB左右

6可靠的事件处理机制

7使用隐式迭代简化编程

8丰富的插件支持

# jQuery的版本

目前jQuery有三个大版本


q  1.x：兼容ie678,使用最为广泛的，官方只做BUG维护，功能不再新增。因此一般项目来说，使用1.x版本就可以了，最终版本：1.12.4 


q  2.x：不兼容ie678，很少有人使用，官方只做BUG维护，功能不再新增。如果不考虑兼容低版本的浏览器可以使用2.x，

q  3.x：不兼容ie678，只支持最新的浏览器。除非特殊要求，一般不会使用3.x版本的，很多老的jQuery插件不支持这个版本。目前该版本是官方主要更新维护的版本。

**jQuery代码演示**

```html
<!DOCTYPE html>
<html>
    <head>
         <meta charset="utf-8">
         <title></title>
         <!--引入jQuery文件-->
         <script  src="js/jquery.min.js"></script>
         <script>
             $(function(){
                  $("tr:odd").css("backgroundColor","yellowgreen")
                  $("tr:even").css("backgroundColor","lightyellow")
                  $("tr:first").css("backgroundColor","greenyellow")
             })
         </script>
    </head>
    <body>
         <table class="datalist"   summary="list   of members in EE Studay" id="oTable">
             <tr>
                  <th scope="col">Name</th>
                  <th scope="col">Class</th>
                  <th scope="col">Birthday</th>
                  <th scope="col">Constellation</th>
                  <th scope="col">Mobile</th>
             </tr>
             <tr>
                  <td>hello</td>
                  <td>world</td>
                  <td>hello</td>
                  <td>你好</td>
                  <td>world</td>
             </tr>
             <tr>
                  <td>play</td>
                  <td>joke</td>
                  <td>happy</td>
                  <td>Spring</td>
                  <td>date</td>
             </tr>
             <tr>
                  <td>girlfriend</td>
                  <td>boy</td>
                  <td>love</td>
                  <td>dog</td>
                  <td>ritle</td>
             </tr>
             <tr>
                  <td>game</td>
                  <td>but</td>
                  <td>PROBLEM</td>
                  <td>Shanghai</td>
                  <td>example</td>
             </tr>
         </table>
    </body>
</html>
```

jQuery提供了丰富的选择器功能，这个是jQuery相比JavaScript的一大优势。我们先来看一下jQuery API。可以看到提供了众多的选择器，可以非常方便简单的获取要选择的内容。

 

问题：JavaScript是如何直接获取要选择的内容的


q  getElementById( ) ：返回一个节点对象


q  getElementsByName( )：返回多个（节点数组）


q  getElementsByTagName( ) ：返回多个（节点数组）


jQuery的选择器功能要强大的多

 

# 基本选择器

​        	

```
标签选择器 $("a")  

ID选择器 ("#id")     ("p#id")

类选择器 (".class")    ("h2.class")

通配选择器 $("*")

并集选择器$("elem1,elem2,elem3")

后代选择器$(ul li)    

父子选择器 $(ul>li)  

后面第一个兄弟元素 prev + next

后面所有的兄弟元素 prev ~ next

这些选择器其实我们并不陌生，因为在讲解CSS样式中都有类似的选择器，并且其含义也是一样的。不同的在CSS中是对选择的内容定义CSS样式，在jQuery中用来选择内容并后续进行更多的下步操作。

```


```html
 <!DOCTYPE html> 

<html> 
    <head> 
        <meta charset="utf-8"> 
        <title>基本选择器</title> 
         <style type="text/css">
            .myClass{ 
                background-color:  aqua; 
            } 
        </style> 
        <script type="text/javascript" src="js/jquery.min.js"></script> 
        <script type="text/javascript"> 
        	// 必须自己会使用的选择器  id选择器  ("#id") 类选择器 ('.class属性值')  标签选择器 $("标签名")
      $(function(){ 
            //标签选择器 $("a")   
            //$("h3").addClass("myClass"); 
            //$("p").addClass("myClass"); 
            //ID选择器 $("#id")     $("p#id") 
            //$("#h31").addClass("myClass"); 
            //$("h3#h31").addClass("myClass"); 
            //类选择器 $(".class")    $("h2.class") 
            //$(".red1").addClass("myClass"); 
            //通配选择器 $("*") 
            //$("*").addClass("myClass"); 
            //并集选择器$("elem1,elem2,elem3") 
            //$("#h31,span,div").addClass("myClass"); 
            //后代选择器$(ul li)   
            //$("p span").addClass("myClass");   
            //父子选择器 $(ul>li)   
            //$("p>span").addClass("myClass"); 
            //后面第一个兄弟元素 prev + next 
            //$("h3+p").addClass("myClass"); 
            //后面所有的兄弟元素 prev ~ next 
            $("h3~p").addClass("myClass"); 
        });               
    </script> 
</head> 
<body> 
   <h3 id="h31">JSP</h3> 
   <p>  
           JSP全名为Java Server Pages，中文名叫java服务器页面，
                   其根本是一个简化的<span>Servlet</span>设计， 它[1]  是
                   由Sun Microsystems公司倡导、许多公司参与一起建立的一种
                   动态网页技术标准。JSP技术有点类似ASP技术，它是在传统的网
                   页<em><span>HTML</span></em>（标准通用标记语言的子集）
                   文件(*.htm,*.html)   中插入Java程序段(Scriptlet)和JSP
                   标记(tag)，从而形成JSP文件，后缀名为(*.jsp)。 用JSP开发
                   的Web应用是跨平台的，既能在Linux下运行，也能在其他操作系
                   统上运行。 
   </p>  
   
   <h3 id="h32"   class="red1">Servlet</h3> 
   
   <p> 
   	    Servlet（Server Applet）是Java Servlet的简称，是为小服
   	    务程序或服务连接器，用Java编写的服务器端程序，主要功能在于
   	    交互式地浏览和修改数据，生成动态Web内容。 
   </p> 
   
   <p class="red1"> 
      	狭义的Servlet是指Java语言实现的一个接口，广义的Servlet
      	是指任何实现了这个Servlet接口的类，一般情况下，人们将
      	Servlet理解为后者。Servlet运行于支持Java的应用服务器中。
      	从原理上讲，Servlet可以响应任何类型的请求，但绝大多数情
      	况下Servlet只用来扩展基于HTTP协议的Web服务器。 
   </p> 
   
  <div> 
    <p>div   p</p>   
  </div> 
  
  <span>span</span> 
  
   <p class="red1"> 
   		狭义的Servlet是指Java语言实现的一个接口，广义的Servlet
   		是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet
   		理解为后者。Servlet运行于支持Java的应用服务器中。从原理上讲，
   		Servlet可以响应任何类型的请求，但绝大多数情况下Servlet只用
   		来扩展基于HTTP协议的Web服务器。 
   </p>
   
</body> 
</html>
```
# 属性选择器

```
[attribute] 匹配包含给定属性的元素

[attribute1][attribute2]  复合属性选择器，需要同时满足多个属性

[attribute=value] 匹配给定的属性是某个特定值的元素

[attribute!=value] 匹配所有属性不等于特定值的元素

[attribute^=value] 匹配给定的属性是以某些值开始的元素

[attribute$=value] 匹配给定的属性是以某些值结尾的元素

[attribute*=value] 匹配给定的属性是以包含某些值的元素

```

 

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">

		<title>属性选择器</title>

		<style type="text/css">
			.myClass {
				background-color: aqua;
			}
		</style>

		<script src="js/jquery.js" type="text/javascript" charset="utf-8"></script>

		<script type="text/javascript">
			$(function() {

				//[attribute] 

				//$("a").addClass("myClass"); 

				//$("a[href]").addClass("myClass"); 

				//attribute1 

				//$("ahref").addClass("myClass"); 

				//[attribute=value]   

				//$("a[href='film-2.html']").addClass("myClass"); 

				//[attribute!=value]   

				//$("ahref").addClass("myClass"); 

				//[attribute^=value]   

				//$("a[href^='http']").addClass("myClass"); 

				//[attribute$=value   

				//("a[href='htm']").addClass("myClass"); 

				//[attribute*=value]   

				$("a[href*='mashibing']").addClass("myClass");

			});
		</script>
	</head>
	<body>
		<ul id="msb">
			<li>
				青花瓷
			</li>
			<li>
				小朋友,你是否有很多问号
			</li>
			<li>
				羞答答的玫瑰静悄悄的开
			</li>
			<li>
				月半小夜曲
			</li>
			<li>
				单恋一枝花
				<ul id="film">
					<li>
						乱世佳人
					</li>
					<li>
						阿郎的故事
					</li>
					<li id="film3">
						阿甘正传
					</li>
					<li>
						鲁冰花
					</li>
					<li>
						太行山上
					</li>
					<li>无问西东</li>
				</ul>
			</li>
		</ul>
	</body>
</html>
```

# 位置选择器

针对整个页面而言的位置选择器

:first 获取第一个元素

:last 获取最后一个元素

:odd 匹配所有索引值为奇数的元素，从0 开始计数 

:even匹配所有索引值为偶数的元素，从0 开始计数

:eq(n) 匹配一个给定索引值的元素

:gt(n) 匹配所有大于给定索引值的元素

:lt(n) 匹配所有小于给定索引值的元素

 

针对上级标签而言的位置选择器

:first-child  匹配第一个子元素

:last-child匹配最后一个子元素 

:only-child如果某个元素是父元素中唯一的子元素，将会被匹配

:nth-child(n)  :nth-child(odd|even) :nth-child(xn+y) 匹配其父元素下的第N个子或奇偶元素

注意：nth-child()选择器编号是从1开始，而其他选择器从0开始

```html
<!DOCTYPE html> 
<html> 
      <head> 
        <meta charset="utf-8"> 
        <title>位置选择器</title> 
       <style type="text/css"> 
                 div{ 
                      border: 1px solid  red; 
                } 
                .myClass{ 
                      background-color:  aqua; 
                } 
        </style> 
        <script src="js/jquery.min.js"   ></script>
        <script type="text/javascript"> 
            $(function(){ 
                //位置针对整个页面 
                //:first     :last   :odd   :even   
                //$("p:first").addClass("myClass"); 
                //$("p:last").addClass("myClass"); 
                //$("p:odd").addClass("myClass");//索引从0开始 奇数的索引 1 3 5 第偶数的元素
                //$("p:even").addClass("myClass"); //
                //:eq(n)     :gt(n)   :lt(n) 
                //$("p:eq(4)").addClass("myClass");   //equals 
                //$("p:lt(4)").addClass("myClass");//less   than  
                //$("p:gt(4)").addClass("myClass");//greater   than 
                //位置针对上级标签 
                //:first-child    :last-child   :only-child 
                //$("p:first-child").addClass("myClass"); 
               //$("p:last-child").addClass("myClass"); 
                //$("p:only-child").addClass("myClass"); 
                //:nth-child(n)   :nth-child(odd|even) :nth-child(xn+y)
                  //索引从0开始 只有此处从1开始
                //$("p:nth-child(2)").addClass("myClass");
                //$("p:nth-child(odd)").addClass("myClass"); 
                //$("p:nth-child(even)").addClass("myClass"); 
                //$("p:nth-child(3n+1)").addClass("myClass");//n=0,1,2,3 
            });
        </script> 
    </head>   
    <body> 
        <div> 
            <p>1.   JavaSE</p>
           <p>2.   Oracle</p>
        </div> 

       <div> 
            <p>3.   HTML/CSS/JS</p>
            <p>4.   jQuery/EasyUI</p>
            <p>5.   JSP/Servlet/Ajax</p>
        </div> 
        <div> 
            <p>6.   SSM</p>  
            <p>7.   SpringBoot/SpringCloud/SpringData</p>
            <p>8.   Maven/Linux/p> 
            <p>9.   Redis/Solr/Nginx</p> 
           <p>10.   SpringBoot/SpringCloud/SpringData</p> 

           <p>11.   SpringBoot/SpringCloud/SpringData</p> 
            <p>12.   SpringBoot/SpringCloud/SpringData</p> 
        </div> 
        <div> 
            <p>13. 就业指导</p> 
        </div> 
    </body>  
</html> 
```

# 表单选择器

关于表单项的选择器

:text   :password  :radio  :checkbox  :hidden  :file  :submit

:input  匹配所有 input, textarea, select 和 button 元素

关于表单项状态的选择器

:selected  :checked  :enabled  :disabled  :hidden :visible

 

```
注意("input")和(":input")的区别

("input")：标签选择器，只匹配input标签,(":input")： 匹配所有 input, textarea, select 和 button 元素
                   
```

```html
         <!DOCTYPE html>

<html>

    <head>

         <meta charset="utf-8">

         <title>表单选择器</title>

         <style type="text/css">

             .myClass {

                  background-color: aqua;

             }

         </style>

         <script src="js/jquery.js"   type="text/javascript"></script>

         <script type="text/javascript">

             $(function() {

                  //:text   :password    :radio  :checkbox  :hidden    :file  :submit 

                  //var arr =$("input"); // 标签名选择器

         //var arr = $("input[type=hidden]"); 
              //var arr = $("input:hidden");              
              //:input  匹配所有 input, textarea, select 和 button 元素 
              //var arr =   $("input,select,textarea,button");   
              //var arr = $(":input"); 
              //:selected    :checked  :enabled  :disabled   
              //var arr = $(":disabled"); 
              //var arr = $(":enabled"); 
              //var arr = $(":input:not(:disabled)"); 
              //var arr = $(":checked"); 
              //var arr = $(":selected"); 
              //:hidden :visible       
              //var arr = $("input:hidden") 
             /* var   arr = $(":input:visible")
              for(var i = 0; i < arr.length; i++) {
                  console.info(arr[i]);
              }*/
         });
     </script>
</head>
<body>
     <h3>注册用户</h3>
     <form action=""   method="get">
         <table border="1"   width="40%">
              <tr>
                  <td>用户名</td>
                  <td>
                  	  <input type="text"   name="username"   value="请输入姓名" />
                      <input type="hidden"   name="username"   id="username"   value="请输入姓名"    />
                  </td>
              </tr>
              <tr>
                  <td>密 码</td>
                  <td><input type="password"   name="pwd"   id="pwd" disabled="disabled"   /></td>
              </tr>
              <tr>
                  <td>确认密码</td>
                  <td><input type="color"   name="repwd"   id="repwd"   /></td>
              </tr>
              <tr>
                  <td>性别</td>
                  <td>
                       <input type="radio"   name="sex"   value="男" />男
                       <input type="radio"   name="sex"   checked="checked"   value="女" />女
                  </td>
              </tr>
              <tr>
                  <td>年龄</td>
                  <td><input type="text"   min="6"   max="30"   name="age"   id="age"   value="18"   /></td>
              </tr>
             <tr>
                 <td>电子邮箱</td>
                 <td><input type="text"   name="email"   id="email"   /></td>
             </tr>
            <tr>
                  <td>籍贯</td>
                 <td>
                     <select name="pro">
                     	<option value="1">京</option>
                     	<option value="2" selected="selected">津</option>
                     	<option value="3">冀</option>
                     </select>
                </td>
             </tr>
             <tr>
                 <td>自我介绍</td>
                 <td>
                    <textarea name="intro"></textarea>
                 </td>
             </tr>
              <tr>
                  <td colspan="2">                  	<input type="submit" />
                  	<input type="reset" />
                 	
                 </td>
                  
              </tr>
         </table>
     </form>
</body>
</html>
```
**迭代遍历**

jQuery给我们封装了一个快捷遍历元素的方法,接下来我们就使用一下jQuery中新的遍历方式

```java
 <!DOCTYPE html>
<html>
  <head>
        <meta charset="utf-8">
        <title></title>
       <script type="text/javascript" src="js/jquery.min.js" ></script>
        <script>
            $(function(){
                  var lis =('li')
                 console.info($lis)
                 for(var i = 0;i<$lis.length;i++){
                     /遍历出的每个元素是DOM对象/
                     console.info($lis[i].innerText)
                 }
                for(var i in $lis){
                     console.info($lis[i].innerText)
                  }
                 /遍历所有元素的方法/
                /*
                  each每拿出一个元素 都会执行一次内部的function
                  i 当前元素的所有
                e 当前元素 DOM对象
                  *
                  * */
                  $lis.each(function (i,e){
                    console.info(i+'>>>'+$(e).text())
                  })
                 .each(lis,function (i,e){
                     console.info(i+'>>>'+$(e).text())
               })
            })
        </script>
  </head>
   <body>
        <ul>
           <li>AI</li>

            <li>Python</li>
            <li>大数据</li>
            <li>JAVA</li>
           <li>前端</li>
        </ul>
    </body>
</html>

```

对于自学来讲,比较推荐的方式是通过看官方API进行学习,对于后端来讲,掌握jQuery选择器,其他的可以在使用的时候查看API,

这里给出思维导图,参考这个顺序进行学习;

![jQuery学习路线](..\html\jquery.png)

<table><th>

[jQuery菜鸟教程](https://www.runoob.com/jquery/jquery-tutorial.html)</th><th>

[jQueryW3教程](https://www.w3school.com.cn/jquery/index.asp)</th><th>

[jQuery文档](https://jquery.cuishifeng.cn/)</th><th>

[bootstrap4](https://www.runoob.com/bootstrap4/bootstrap4-tutorial.html)</th>

</table>

![](..\pic\div\shark.gif)