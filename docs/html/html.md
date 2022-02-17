![](..\pic\div\shark.gif)

# html语言

标记---即标签的意思
简单的格式---
用记事本新建一个txt文档,在文档中加入以下内容,然后修改后缀名为 html或者htm,之后用浏览器打开

```html
<html>
<head>
helloworld
</head>

<body>
你好世界
</body>

</html>
```
被html所包围有头和身子部分,然后用浏览器打开,一个简单的html页面就诞生了;

但是不能完全  用记事本或者notepad来编写,需要借助一切便捷工具来辅助开发----从dreamweaver,到hbuilder,等等一些辅助工具的开发极大的便利了程序员的开发效率;[hbuider的下载](https://www.dcloud.io/hbuilderx.html)
但是我还是习惯与vscode的使用毕竟 vscode功能更加强大一些;

简单的说一下html中标签的格式
有单标签和双标签
标签有很多,入门先掌握  基本的html格式

```html
<html>
<!----><!--这是注释的格式-->
<head>
<title>这是网页标题</title>
<meta charset="utf-8"/><!--这是指定文件编码格式-->
<meta http-equiv="content-type"content="text/html; charset=UTF-8"/><!--
</head>
<body>
这是身子部分内容
</body>
</html>
```
这是网页基本要素,可以说每一个网页里面都会看有

**下面的内容是head标签中可以包含的(只是部分内容)**
```html
<html>
<!--这是注释-->
<head>

<title>百度一下,你就知道</title>
<meta charset="utf-8"/><!--设置页面编码方式 html5版-->
<!--<meta http-equiv="content-type"content="text/html; charset=UTF-8"html4版-->
<!--
	作者：@qq.com
	时间：2021-09-15
	描述：随便写的
-->
<!--设置页面自动刷新并可以指定跳转到的网页-->
<!--<meta http-equiv="refresh" content="3;"-->
<!--<meta http-equiv="refresh"content="1;https://cn.bing.com/?scope=web&FORM=ANNTH1"-->
<!--页面作者-->
<meta name="author"content="Gavin;123456@qq.com"
	<!--设置页面搜索关键字和信息-->
<meta name="keywords"content="JAVA" />
<meta name="description"content="被缚的普罗米修斯"  />
<!--link 标签表示引入外部资源-->
<!--<link rel="shortcut icon" href="https://baidu.com/favicon.ico" type="image/x-icon" />-->
<!--引入外部网页标题图标-->
<link rel="shortcut icon" href="https://cn.bing.com/sa/simg/favicon-2x.ico" type="image/x-icon" />
</head>
<body>

这是身子部分内容
</body>
</html>
```

再来一个----

```html
<!DOCTYPE HTML>
<html>
	<head>
		<title>
			百度一下,你就不知道
		</title>
		<meta name="refresh" content=""/>
		<meta charset="UTF-8"/>
	<link rel="shortcut icon" href="https://cn.bing.com/sa/simg/favicon-2x.ico" type="image/x-icon" />
	<meta name="gavin" content="#0062CC" />
	</head>
	<body>
		<title>
			这是body标题
		</title>
			<!--引入多媒体文件--><!--图片大小设置一个即可按比例缩放-->
	<img src="img/赤壁赋.jpg" width="300px" title="赤壁赋"/>
	<img src="img/1234.jpg" width="300px" title="风景如画"/>
	<img src="img/赤壁赋.jpg" width="300px" title="赤壁赋"/>
	<img src="img/1234.jpg" width="300px" title="风景如画"/>
	<img src="img/赤壁赋.jpg" width="300px" title="赤壁赋"/>
		
		<h1><font color="#007AFF" size="7" face=楷体>赤壁赋&copy;</font></h1>
		<h2>苏轼</h2>
		<p> &emsp;&emsp;<font color=#2AC845 size=3,face=仿宋>七月既望，苏子与客泛舟，游于赤壁之下。清风徐来，水波不兴。举酒属客，诵明月之诗，歌窈窕之章。少焉，月出于东山之上，徘徊于斗牛之间。白露横江，水光接天。纵一苇之所如，凌万顷之茫然。浩浩乎如冯虚御风，而不知其所止；飘飘乎如遗世独立，羽化而登仙。<b><i><u>(冯 通：凭)</u></i></b></font></p>

　　<p>&emsp;&emsp;<font color=darkorange size="4" face=微软雅黑>于是饮酒乐甚，扣舷而歌之。歌曰：“桂棹兮兰桨，击空明兮溯流光。渺渺兮予怀，望美人兮天一方。”客有吹洞箫者，倚歌而和之。其声呜呜然，如怨如慕，如泣如诉；余音袅袅，不绝如缕。舞幽壑之潜蛟，泣孤舟之嫠妇。</font></p>

　　<p>&emsp;&emsp;<font color=deeppink size=3 face=新宋体 >正襟危坐，而问客曰：“何为其然也？”客曰：“‘月明星稀，乌鹊南飞。’此非曹孟德之诗乎？西望夏口，东望武昌，山川相缪，郁乎苍苍，此非孟德之困于周郎者乎？方其破荆州，下江陵，顺流而东也，舳舻千里，旌旗蔽空，酾酒临江，横槊赋诗，固一世之雄也，而今安在哉？况吾与子渔樵于江渚之上，侣鱼虾而友麋鹿，驾一叶之扁舟，举匏樽以相属。寄蜉蝣于天地，渺沧海之一粟。哀吾生之须臾，羡长江之无穷。挟飞仙以遨游，抱明月而长终。知不可乎骤得，托遗响于悲风。”</p> </font>

　　<p>&emsp;&emsp;<font color=lightsalmon size=4 face=微软雅黑>客亦知夫水与月乎？逝者如斯，而未尝往也；盈虚者如彼，而卒莫消长也。盖将自其变者而观之，则天地曾不能以一瞬；自其不变者而观之，则物与我皆无尽也，而又何羡乎！且夫天地之间，物各有主，苟非吾之所有，虽一毫而莫取。惟江上之清风，与山间之明月，耳得之而为声，目遇之而成色，取之无禁，用之不竭。是造物者之无尽藏也，而吾与子之所共适。”<u><i><b>(共适 一作：共食)</b></i></u></font>
</p>
　　<p>&emsp;&emsp;<font color=#2AC845 size=3,face=仿宋>客喜而笑，洗盏更酌。肴核既尽，杯盘狼籍。相与枕藉乎舟中，不知东方之既白。</font></p>
	
				
	</body>
	
</html>
```
引入多媒体文件----

```html
<!--引入多媒体文件--><!--图片大小设置一个即可按比例缩放-->
	<img src="img/赤壁赋.jpg" width="750px" title="赤壁赋" alt="图片加载失败"/>
	<br />
		
	<!--引入音频-->
	<embed src="music/再见亦是泪.mp3"></embed>
	<!--引入视频-->
	<embed src="video/下雨了.mp4"></embed>
```
超文本标签

```html
	
		<a href="hello.html" target="_blank">这是一个超链接1</a>
		<a href="">这是一个超链接2</a><!--跳转到当前页-->
		<!--在父类页面打开-->
		<a href="https://www.baidu.com" target="_parent">这是一个超链接3在父类页面打开</a><!--跳转到外部连接-->
	<!--在自身页面打开-->
	<a href="https://www.baidu.com" target="_self">这是一个超链接4</a><!--跳转到外部连接-->
	<!--在空白页打开-->
	<a href="https://www.baidu.com" target="_blank">这是一个超链接5</a><!--跳转到外部连接-->
	<a href="http://www.baidu.com"><img src="img/1234.jpg"width=300px />  </a>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>失落,并不一定难过--精品散文赏析      </title>
	<meta name="Gavin" content="加油!!!"/>
	<link rel="shortcut icon" href="https://cn.bing.com/sa/simg/favicon-2x.ico" type="image/x-icon" />
	</head>
	<body>
		<!--	<embed src="music/从你的全世界路过.mp3"></embed>-->
		
	<a href="#1F">赤壁赋</a>
	<a href="#2F">归去来兮辞</a>
	<a href="#3F">失落,并不一定难过</a>

	<a name="1F"></a>
		<h1  align="center"><font color="#F0AD4E" face="微软雅黑" size="7">赤壁赋 </font> </h1>
		<h4  align="center"><a href="https://so.gushiwen.cn/authorv_3b99a16ff2dd.aspx" target="_self">苏轼</a> <a href="https://so.gushiwen.cn/shiwens/default.aspx?cstr=%e5%ae%8b%e4%bb%a3">〔宋代〕</a></h4>
<p align="left"> &emsp;&emsp;壬戌之秋，七月既望，苏子与客泛舟，游于赤壁之下。清风徐来，水波不兴。举酒属客，诵明月之诗，歌窈窕之章。少焉，月出于东山之上，徘徊于斗牛之间。白露横江，水光接天。纵一苇之所如，
	凌万顷之茫然。浩浩乎如冯虚御风，而不知其所止；飘飘乎如遗世独立，羽化而登仙。 　
<p align="left"> &emsp;&emsp;于是饮酒乐甚，扣舷而歌之。歌曰：“桂棹兮兰桨，击空明兮溯流光。渺渺兮予怀，望美人兮天一方。”客有吹洞箫者，倚歌而和之。其声呜呜然，如怨如慕，如泣如诉；余音袅袅，不绝如缕。舞幽壑
	之潜蛟，泣孤舟之嫠妇。</p>
<p align="left"> &emsp;&emsp;苏子愀然，正襟危坐，而问客曰：“何为其然也？”客曰：“‘月明星稀，乌鹊南飞。’此非曹孟德之诗乎？西望夏口，东望武昌，山川相缪，郁乎苍苍，此非孟德之困于周郎者乎？方其破荆州，下江陵，
	顺流而东也，舳舻千里，旌旗蔽空，酾酒临江，横槊赋诗，固一世之雄也，而今安在哉？况吾与子渔樵于江渚之上，侣鱼虾而友麋鹿，驾一叶之扁舟，举匏樽以相属。寄蜉蝣于天地，渺沧海之一粟。哀吾生之须臾，羡长江之无穷。
	挟飞仙以遨游，抱明月而长终。知不可乎骤得，托遗响于悲风。” 　</p>
<p align="left"> &emsp;&emsp;苏子曰：“客亦知夫水与月乎？逝者如斯，而未尝往也；盈虚者如彼，而卒莫消长也。盖将自其变者而观之，则天地曾不能以一瞬；自其不变者而观之，则物与我皆无尽也，而又何羡乎！且夫天地之间，
	物各有主，苟非吾之所有，虽一毫而莫取。惟江上之清风，与山间之明月，耳得之而为声，目遇之而成色，取之无禁，用之不竭。是造物者之无尽藏也，而吾与子之所共适。” 　　
<p align="left"> &emsp;&emsp;客喜而笑，洗盏更酌。肴核既尽，杯盘狼籍。相与枕藉乎舟中，不知东方之既白。</p>
<hr width=100% align="center"/>	
		<a name="2F"></a>
		<h1  name="2F" align="center"><font color=lawngreen face="微软雅黑"size="7">归去来兮辞</font></h1>
<h4 align="center"><a href="https://so.gushiwen.cn/authorv_07d17f8539d7.aspx" target="_self">陶渊明</a> <a href="https://so.gushiwen.cn/shiwens/default.aspx?cstr=%e9%ad%8f%e6%99%8b">〔魏晋〕</a></h4>

<p align="left"> &emsp;&emsp;余家贫，耕植不足以自给。幼稚盈室，瓶无储粟，生生所资，未见其术。亲故多劝余为长吏，脱然有怀，求之靡途。会有四方之事，诸侯以惠爱为德，家叔以余贫苦，遂见用于小邑。于时风波未静，心惮远役，彭泽去家百里，公田之利，足以为酒。故便求之。及少日，眷然有归欤之情。何则？质性自然，非矫厉所得。饥冻虽切，违己交病。尝从人事，皆口腹自役。于是怅然慷慨，深愧平生之志。犹望一稔，当敛裳宵逝。寻程氏妹丧于武昌，情在骏奔，自免去职。仲秋至冬，在官八十余日。因事顺心，命篇曰《归去来兮》。乙巳岁十一月也。

<p align="left"> &emsp;&emsp;归去来兮，田园将芜胡不归？既自以心为形役，奚惆怅而独悲？悟已往之不谏，知来者之可追。实迷途其未远，觉今是而昨非。舟遥遥以轻飏，风飘飘而吹衣。问征夫以前路，恨晨光之熹微。</p>

<p align="left"> &emsp;&emsp;乃瞻衡宇，载欣载奔。僮仆欢迎，稚子候门。三径就荒，松菊犹存。携幼入室，有酒盈樽。引壶觞以自酌，眄庭柯以怡颜。倚南窗以寄傲，审容膝之易安。园日涉以成趣，门虽设而常关。策扶老以流憩，时矫首而遐观。云无心以出岫，鸟倦飞而知还。景翳翳以将入，抚孤松而盘桓。

<p align="left"> &emsp;&emsp;归去来兮，请息交以绝游。世与我而相违，复驾言兮焉求？悦亲戚之情话，乐琴书以消忧。农人告余以春及，将有事于西畴。或命巾车，或棹孤舟。既窈窕以寻壑，亦崎岖而经丘。木欣欣以向荣，泉涓涓而始流。善万物之得时，感吾生之行休。

<p align="left"> &emsp;&emsp;已矣乎！寓形宇内复几时？曷不委心任去留？胡为乎遑遑欲何之？富贵非吾愿，帝乡不可期。怀良辰以孤往，或植杖而耘耔。登东皋以舒啸，临清流而赋诗。聊乘化以归尽，乐夫天命复奚疑！</p>
	<hr width=100% align="center"/>	
	
	<a name="3F"></a>
			<h1 name="3F" align="center"><font size="7" face="微软雅黑"color=darkorange >失落，并不一定难过</font></h1>
			<!--<embed src="music/再见亦是泪.mp3"></embed>-->
			<h4 align="center">作者: 于公谨啊<b><a href="http://www.meiwen.net.cn/article/170434.html" target="_blank">来源: 美文摘抄网</a></b>发表于2018-07-04被阅读4077次</h4>
<p align="left">&emsp;&emsp;走过了许许多多的路，总是难掩心头的孤独；曾经面临着十字路口的犹豫，曾经面临着选择的踌躇；曾经看到美丽的风景，因为我并没有保持着清醒，所以就这样恍然入梦，就这样开始了朦胧；
然后就狠狠地摔倒在地上，留下想那些难以言喻的忧伤，还有那些难以言喻的衷肠。那些岁月的河流，从来就不可能会因为我的忧愁，就不再流淌，或者是不再心头留下惆怅。这让我发出着感慨，让我展现胸怀，让我留下着孤独和徘徊。</p>


<p align="left">&emsp;&emsp;那些岁月，挂满了我的心血，本来就不想有任何的松懈，但是，却还是留下了日子里面的圆缺，还有那些日子里面的凛冽。经历了多少的风风雨雨，却还是有着自己的歌曲；那些脚下的旋律，总是
在不经意间就如细雨，留下着丝丝缕缕，缠绕在心头，让心留下了许许多多的疑惑。孤独的身影，就这样表现的不平静；那些凸显著岁月的安宁，却留下了心中的风情。而那些曾经的失落，总是和时光交错而过，却留下并不一定是难过。</p>

<p align="left">&emsp;&emsp;岁月里面的迷离，总是会留下着许许多多的涟漪。而那些本来早已经蛰伏的记忆，却因为不经意的失意，就会重新开始涌动着涟漪。记忆里面的痕迹，弯弯曲曲并没有多少偏离，却让心不断地流动
着曾经的神秘。那些日子里面的痛，因为自己的情，而感觉到了撕心裂肺的疼。用时间的手，轻轻抚摸着心底的那一份忧愁；任何所有的伤口，就开始结痂，虽然还是有着挣扎，却有时候会觉得这是记忆里面花，也是岁月留下的面纱。
那些裸露在外面的失落，悠动着时间的轮廓；而抬头却看得了那些希望在前方闪烁，心中就会没有了任何的难过。</P>

<p align="left">&emsp;&emsp;记忆的山峰，是人生的旅程，在不断起起伏伏中，向前延伸着，也像是心情在忐忑。那些曲曲折折，留下时光里面的选择。岁月的草，在时光的风里闪动着骄傲；那些记忆的树，在路边展现着孤独。
原来看上去是高不可攀的山峦，心中有着幽怨，有着畏惧，有着自己的思绪；现在回头看看的时候，脚下的路，就是曾经征服的山，在不断蜿蜒。只是下山的时候，想要做出的停留，却会不断跌倒，让
自己没有了骄傲，却听到了时光里面的嘲笑。那些经历的失落，就像是树木在不断落错，并没有一定的规律，却有着自己的秩序。</p>

<p align="left">&emsp;&emsp;那些希望一直都像是山涧里面的河流，依附着天空的白云悠悠，在不断淅淅沥沥地动着，欢乐着，发出着动人的声音，显得清纯。阳光并没有多少温润，却可以让心中有着迷人的韵，在不断跳动，
不断涌动。那些过去的时光，留下了多少迷茫？那些懵懵懂懂的岁月，有着多少圆缺？而记忆的深浅，在不断向前绵延，不断留恋，不断地依恋。想要抽刀断水，想要迷醉，或者是想要沉睡，只是那些失落，却在不断闪烁，也不断开始交错。</p>

<p align="left">&emsp;&emsp;并不需要多少眷顾，那些路，总是会不断邂逅，不断留在了心头。邂逅的是我，是寂寞，是沉默？是岁月邂逅了我？还是我邂逅了岁月？是失落邂逅了我，还是我邂逅了失落？而那些失落，却让
我并不难过，因为那些失落，让我变得执着，也变得成熟，也坚定了我的征途。		<hr width=100% align="center"/>	</p>
		
	<!--<u1 type="square">--><!--u1 列表是无序的-->
		<u1 style="list-style:url(img/favicon.ico);">
		<li>A</li>
		<li>B</li>
		<li>C</li>
		
	</u1>
	
		<!-- 有序列表--O1--type 设置有序的类型,start表示从哪里开始> 
                <ol type="I" start="1">
                        <li>JAVASE</li>
                        <li>ORACLE</li>
                        <li>MYSQL</li>
                        <li>HTML</li>
                        <li>CSS</li>
                        <li>JS</li>
                </ol>			
		</body>
</html>

```
表格标签


```html
<!--表格边框线间距cellsapce,border表格边的粗细-->
		<table border="1px" background="img/1234.jpg" cellspacing="0px">
			<!--表格-->
			<tr bgcolor="#EC971F">
				<!--行-->
				<th>id</th>
				<!--列-->
				<!--th特殊单元格-->
				<th>name</th>
				<th>gender</th>
				<th>age</th>
			</tr>

			<tr bgcolor="#F0AD4E">
				<td> 1000</td>
				<td align="center">张三</td>
				<td>男</td>
				<td rowspan="3">19</td>
				<!--列合并-->

			</tr>
			<tr bgcolor="#F0AD4E">
				<td> 1000</td>
				<td align="center">张三</td>
				<td>男</td>

			</tr>
			<tr bgcolor="#F0AD4E">

				<td align="center" colspan="2">张三</td>
				<!--colspan行合并-->
				<td>男</td>

			</tr>

		</table>
```
内嵌框架---->>>

```html
<html>
	<head>
		<title>这是展示页面</title>
		<meta charset="UTF-8"/>
	</head>
	<body>
		<ul>
				<li>
					<a href='img/1234.jpg' target="图片展示"> 1234</a>
				</li>
				<li>
					<a href='img/favicon.ico' target="图片展示"> favicon</a>
				</li>
	            <li>
					<a href='img/赤壁赋.jpg' target="图片展示"> 赤壁</a>
				</li>
				<li>
					<a href='img/爱你中国.png' target="图片展示"> 爱你</a>
				</li>

			</ul>
		<iframe with="800px" height="600PX" src="myhtml.html#1F"></iframe>
	</body>
</html>
```


一些文本框---->

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<form action="qqqq" method="get">
		<input type="text" name="user" placeholder="请输入账号"/><!--提交按钮--><br />
		
		<input type="submit" value="提交"/><br />
		
		
		<input type="search" name="search" value="你好" readonly="readonly"/>
		<input type="submit" value="sousuo"/><br />
		
		<input type="search" name="search" value="你好" disabled="disabled"/>
		<input type="submit" value="sousuo"/><br />
		</form>
		
		
		<!--
        	作者：@qq.com
        	时间：2021-09-18
        	描述：通过name来进行控制到一个分组里,不同的值提交后用value来区分
        	checked="checked"---默认选中
        -->
		<form>
			<input type="password" name="pwd" /><br />
			<!--name名一样就是单选-->
			<input  type="radio" name="man" value=1 checked="checked"/>男    <input  type="radio"name="man"  value=0/>女
			<input type="submit" value="tijiao"/><br />
			<!--<input  type="radio"  name ="man"/>男    <input  type="radio"  name ="woman"/>女
			<input type="submit" value="tijiao"/><br />-->
			
		</form>
		
		<form>
			你学习的语言
			<input type="checkbox" name="like" id="1" value="java" checked="checked"/>java
			<input type="checkbox" name="like" id="2" value="C++" />C++
			<input type="checkbox" name="like" id="3" value="C++" checked="checked"/>Python
			<input type="checkbox" name="like" id="4" value="C+" />C+
			<input type="submit"  />
			
			
			
		</form>
		<form>
			<!--
            	作者：.com
            	时间：2021-09-18
            	描述：input
            -->
			<input type="email" name="邮箱" id="email" placeholder="请输入邮箱地址" autofocus="autofocus" required="required"/>邮箱
			
			<input type="url" name="网址" id="url" placeholder="请输入网址" required="required"/>网址
				
			<input type="color" name="颜色" id="color" value="颜色" />颜色
			<input type="number" name="数字" id="number" value="0" step="5" max="100" min="90" required="required"/>数字
			1<input type="range" name="范围" id="range" value="0" min="0" max="10" step="2" required="required"/>10
			请选择日期:<input type="date" name="日期" id="date" value="日期" required="required"/>
			请选择时间:<input type="datetime-local" name="时间" id="时间" value="日期" required="required"/>
			请选择月份:<input type="month" name="月份" id="月份" value="月份" required="required"/>
				<!--
                	作者：@qq.com
                	时间：2021-09-18
                	描述：mutiple--多选 placehoder 默认提示 autofocus :自动获取焦点               	
                -->								
				请选择城市:
				<select name="city" multiple="multiple" required="required">
				
				<option value="0"> ---请选择---</option>
				<option value="1" selected="selected">北京</option>
				<option value="2">上海</option>
				<option value="3">广州</option>
				<option value="4">烟台</option>
				<option value="5">青岛</option>
				
			</select>
			<input type="submit" name="email" id="email" value="登录" />
			</form>
	</body>
</html>

```
***css样式表------>***

```html
<html>
	<head>
		<title>百度一下,你就知道</title>
		<link rel="shortcut icon" href="https://www.baidu.com/favicon.ico" type="image/x-icon" />
		<meta charset="UTF-8"/>
		<!--内敛样式-->
		<h1 style="color: orangered;">这是一个标题</h1>
		<!--
        	作者：1187342818@qq.com
        	时间：2021-09-18
        	描述：内部样式---head标签中加入一个style标签,再里面定位到需要修饰的元素,然后在{}里加入要修饰的元素
        -->
		<style type="text/css">
		
	</head>
	
	<body>
		
		<h2 >这是一个标题</h2>			
	</body>
</html>
```

```html
<html>
	<head>
		<title>百度一下,你就知道</title>
		<link rel="shortcut icon" href="https://www.baidu.com/favicon.ico" type="image/x-icon" />
		<meta charset="UTF-8"/>
		<!--引入css样式表-->
		<link type="text/css"  rel="stylesheet"   href="css/new_file.css" />
		<!--内敛样式-->
		<h1 >这是一个标题</h1>
		<!--
        	作者：1187342818@qq.com
        	时间：2021-09-18
        	描述：内部样式---head标签中加入一个style标签,再里面定位到需要修饰的元素,然后在{}里加入要修饰的元素
        -->
	</head>
	
	<body>
		
		<h2 >这是一个标题</h2>			
	</body>
</html>
```

```css

	h1 {
		color: red;
		font: "bookman old style";
	}
	
	h2 {
		color: green;
		font: "楷体";
	}
```
小练习---
元素选择器---->

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>基本选择器</title>
		
		<style type="text/css">
			/* 基本选择器:元素选择器*/
			h1{
				color: mediumvioletred;
			}
			h2{
				
				color:pink;
				font-family: "agency fb";
				
			}
			i{
				color: cornflowerblue;
			}
			
			/*
			 * 基本选择器中的
			 * 类选择器----
			 * 不同类型的标签使用相同的类型
			 */
			.myclass{
				color:darkslateblue;
				font: "微软雅黑";
			}
			/*
			 * id选择器---主要适用于获取唯一一个元素
			 * 
			 */
			#myid{
				
				color: plum;
				font: "bodoni mt condensed";
				
			}
			优先级>id>class>元素选择器
		</style>
		
	</head>
	<body>
		<h1>这是一个标题</h1>
		<h2>这是<i>一个倾斜</i>标题2</h2>
		<h2 id="myid">这是测试标签体</h2>
		<h2 class="myclass">测试用标签</h3>
		<h4 class= "myclass">测试用标题</h4>
		
	</body>
</html>

```
关系选择器--->

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>div与span</title>
		<style type="text/css">
			/*<!--关系选择器:后代选择器-->*/
			div h1{
				
				Color:red;
				border: 1PX black solid; 
				
			}
			/*<!--关系选择器:子代选择器-->*/
			div>h1{
				
				Color:red;
				border: 1PX black solid; 
				
			}
			span h1{
					Color:green;
				border: 1PX red solid; 
			}			
		</style>
	</head>
	<body>
		<div>
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>
		<span id="1">
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>	
		</span>
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>
		<h1>GavinGavin</h1>
		</div>
	</body>
</html>

```


## BOM

BOM是Browser Object Model的简写，即浏览器对象模型。

BOM有一系列对象组成，是访问、控制、修改浏览器的属性的方法

BOM没有统一的标准(每种客户端都可以自定标准)。

BOM的顶层是window对象

> 如下图中的红色框部分,这部分是由BOM管,可以看到上面有许多可以控制浏览器行为的一些按钮;前进后退刷新,收藏,历史等
>
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/b34057dfd77a49f589660ea85d80bf3b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
>
> **BOM编程就是把整个浏览器抽象成一个对象(window),这个对象中有很多的属性和方法,访问这些属性或者调用这些方法就可以控制浏览器作出…行为**


自从入门了Java,于是有了一种思想------>万物皆对象,把这种思想运用到浏览器中就有了

**BOM编程中的常用方法**

**Window 对象---浏览器中打开的窗口。**

如果文档包含框架（iframe标签），浏览器会为 HTML 文档创建一个 window 对象，并为每个框架创建一个额外的 window 对象。

注意： 没有应用于 window 对象的公开标准，不过所有浏览器都支持该对象。

**关于window对象中的方法练习---**

```html
<script type="text/javascript">
			function fun(){
				// 这三个时windows对象的弹窗方式---三种
				alert('这是一个弹窗');
				var i=confirm("确定要放入回收站么");//带返回值--true/false
			var j=	prompt("请输入一个数:",'例如:1000');//带返回值,第二个参数为提示
			console.log(i);
			console.log(j);
			}
			fun();
		</script>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/413d7d204a4742e79e5fa086c25f1cba.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**计时器-----》》》》》**


setInterval（）---------》》》》》每隔一段时间执行一次
```html
<script type="text/javascript">
		function start() {
			window.setInterval(function(){

				var t = new Date();
				var h = t.getHours();
				var m = t.getMinutes();
				var s = t.getSeconds();
				var str = h + "时" + m + "分" + s + "秒";
				console.log(str);
			}, 1000)
			}
		</script>
```
//每隔一段时间执行一次
![在这里插入图片描述](https://img-blog.csdnimg.cn/10bc3bac82dc4b7d86d361d14040ccb1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


setTimeout()---------------->>>在时间耗尽后执行一次
```javascript
	// 在时间耗尽执行一次
			function startout() {
				window.setTimeout(

					function() {
						var t = new Date();
						var h = t.getHours();
						var m = t.getMinutes();
						var s = t.getSeconds();
						var str = h + "时" + m + "分" + s + "秒";
						alert(str);
					},
					10000 //10秒之后,弹窗

				)
			}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/1e75bf0a510347a9a3b3f02cff51f63c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**将时间显示在网页上----**

```javascript
var intervarId;
			//每隔一段时间执行一次
			function start() {
				intervarId = window.setInterval(function() { //setInterval是有返回值的

					var t = new Date();
					var h = t.getHours();
					var m = t.getMinutes();
					var s = t.getSeconds();
					var str = h + "时" + m + "分" + s + "秒";
					
					//document.write(str);//方式不可取,因为会打印好多而不是在原来位置上继续打印
					var ti=document.getElementById("timearea");
					ti.value=str;
					console.log(str);
				}, 1000)
			}

```
body中的内容------>>>
```html
<input type="text" name="" id="timearea" value="" />
```
这样就会将时间显示在文本框中
![在这里插入图片描述](https://img-blog.csdnimg.cn/a202d606d76c41a3abe7cac6e1521c62.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)但是这样有一个bug,就是当我们连续点击两次 计时开始,再次点击结束计时,发现计时的时间根本停不下来;
原理-------->>>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/fea0c11d82af4104824bd8c1f72accb1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)怎么做才能让他停下来呢?

我们希望 停止计时跟计时开始绑定在一起;



解决方式1,停止计时生成的id能过够计时被停止计时捕捉到;


```html
	<script type="text/javascript">
		
			var intervarIds= new Array();
			//每隔一段时间执行一次
			function start() {
				intervarId = window.setInterval(function() { //setInterval是有返回值的

					var t = new Date();
					var h = t.getHours();
					var m = t.getMinutes();
					var s = t.getSeconds();
					var str = h + "时" + m + "分" + s + "秒";
					
					//document.write(str);//方式不可取,因为会打印好多而不是在原来位置上继续打印
					var ti=document.getElementById("timearea");
					ti.value=str;
					
				}, 1000);
				// push() 方法（在数组结尾处）向数组添加一个新的元素：
				intervarIds.push(intervarId);//将生成的id装进数组
			}
			// 停止计时---要将要停止的setInterval()方法的返回值----传入到这个参数里
			function stop() {
				while(intervarIds.length>0){
				//取出数组元素,肯定有一个能停止计时
				window.clearInterval(intervarIds.shift());
				console.log("计时结束");
				}
			}
				</script>
```
**完整代码--------------->>>>>**
```html
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>

		<script type="text/javascript">
			var intervarIds = new Array();
			var intervarId;

			function startTime() {

				intervarId = window.setInterval(function() {

					var t = new Date();
					var H = t.getHours();
					var M = t.getMinutes();
					var S = t.getSeconds();

					var str = H + ":" + M + ":" + S;

					var i = document.getElementById("timearea");
					i.value = str;
				}, 1000);
				intervarIds.push(intervarId);
			}

			function endTime() {
				while(intervarIds.length > 0) {

					window.clearInterval(intervarIds.shift());

				}

			}

			var timeOuts = new Array();
			var timeOut;

			function startT() {
				alert("计时开始");
				timeOut = window.setTimeout(function() {

					var t = new Date();
					var H = t.getHours();
					var M = t.getMinutes();
					var S = t.getSeconds();

					var str = H + ":" + M + ":" + S;

					var i = document.getElementById("timearea1");
					i.value = str;

				}, 10000);

				timeOuts.push(timeOut);
			}

			function endT() {
				while(timeOuts.length > 0) {
					window.clearTimeout(timeOuts.shift());

				}

			}
		</script>

	</head>

	<body>

		<input type="button" value="计时开始" class="A" onclick="startTime()" />

		<input type="text" id="timearea" value="" />

		<input type="button" value="计时结束" class="B" onclick="endTime()" />

		<input type="button" value="倒计时开始" onclick="startT()" />

		<input type="text" id="timearea1" value="" />

		<input type="button" value="结束计时" onclick="" />
	</body>

</html>
```
**窗口关闭和打开----------------->>>>>>>>**

```html
<script type="text/javascript">
			function oppen(){
				window.open("https://mp.csdn.net/mp_blog/manage/article?spm=1000.2115.3001.5448");
			}
			function closeed(){
				window.close();
			}
		</script>
		
```
**浏览器地址栏操作-------->>>>>>>>>**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>历史记录</title>
		
		
		<script type="text/javascript">
			function forwarded(){
				window.history.forward();
			}
			function backwarded(){
				history.back();
			}
			function goon(){
				history.go(2);//正整数 向前跳转 负整数向后跳转
			}
		</script>
	</head>
	<body>
		<a href="wenti.html">这是页面1</a>
		<input type="button" value="上一页" onclick="forwarded()"/>
		<a href="https://www.baidu.com">baidu</a>
		<input type="button" value="下一页" onclick="backwarded()"/>
		<input type="button" value="gogogo" onclick="goon()"/>
	</body>
</html>

```
测试页面
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
   
</head>
<body>
<a href="https://wwww.baidu.com"> 跳转百度搜索</a>

</body>
</html>
 
```

```javascript
	function fun(){
					console.log("屏幕宽--"+screen.width);
					console.log("屏幕高--"+screen.height);
					console.log("用户代理--"+navigator.userAgent);
					console.log("app名--"+navigator.appName);
					console.log(navigator.appVersion);
					console.log("屏幕X轴DPI--"+window.screen.systemXDPI);
					console.log("屏幕Y轴DPI--"+window.screen.systemYDPI);
				}
```

## DOM

DOM是Document Object Model的简写，即文档对象模型。

DOM用于XHTML、XML文档的应用程序接口(API)。

DOM提供一种结构化的文档描述方式，从而使HTML内容使用结构化的方式显示。

DOM由一系列对象组成，是访问、检索、修改XHTML文档内容与结构的标准方法。


DOM的顶层是document对象

如下图中的部分,超链接,图片等
![在这里插入图片描述](https://img-blog.csdnimg.cn/64661beafb5e4d4eaeb4c3701c70f9ac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **DOM编程就是把浏览器当前页面对应的文档抽象成一个对象(document),这个对象中有很多关于操作文档的一些属性和方法,访问这些属性和方法的时候,我们就可以通过代码动态控制页面上显示的内容**


几乎每天都在用的软件----浏览器是由BOM和DOM共同协作来完成与人的交互;
![在这里插入图片描述](https://img-blog.csdnimg.cn/849b047f78fe44ca84174df26410c335.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)DOM的结构是一棵树============
![在这里插入图片描述](https://img-blog.csdnimg.cn/58e5e3fc3f364539b36099506f27a010.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)用一个简单的html文件来分析一下html中document的存储结构------->>>>

DOM节点分类node

结点对象:Node,document对象中的每一个分支点都是一个node对象,它有三个子类

元素节点 Element   如:<a ></a>

属性节点 Attribute  如:href=

文本节点 Text      如:11

![](..\pic\mysql_pic\63.png)

知道存储结构那么就可以根据节点找到document中的各种元素,进而找到元素中的内容进行修改或者其他操作;

获取节点内容的方式----------->>>>
document------>>>元素-------->>>元素中的值

## OSI与TCP/IP



![](..\pic\mysql_pic\64.png)



OIS没有给出实际解决方案,只是理论上的一种模型;后来TCP/IP协议被广泛使用;

TCP/IP要解决的五大问题----->>
1,数据量大,网络底层设备以一次无法吞吐巨大的数据量------数据的拆分,通过复用路径来提高效率
2,增加协议头来包装数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/a3598720a39b43f7906ff4eeaeca4cb1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)3,相邻设备之间的连接--即局域网
4,路由寻址,找到最适合传输的路线
5,数据重组以及还原
![在这里插入图片描述](https://img-blog.csdnimg.cn/31c19536fbf742f5ba99b348275fc44c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
TCP协议
连接时的三次握手

![在这里插入图片描述](https://img-blog.csdnimg.cn/448c20ef474e432eb43916df43273861.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)断开时的四次挥手
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6105a90d8794335944c87d418352de2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)按照TCP协议在拿到应用层数据后,在传输数据前 要做三件事情-----
1,报文拆分
2,增加tcp头
3,数据重组

发送数据的时候会顺序发送,但是到达接收方时时无序的;
但是如果TCP段特别多的时候,怎么进行排序?
采用时间窗口的形式,在某一个时间段进行排序,如果不在这个时间段,那么就要求客户端重新发送;

TCP头:标志位  NS,ECN,CWR这是属于Tcp协议扩展,URG是一个紧急标志位,当设置紧急标志位为1时,会优先处理该条数据;

ACK响应标志位,
PSH 传送数据,
RST重置标志位
FIN断开标志位
SYN同步标志位
![在这里插入图片描述](https://img-blog.csdnimg.cn/76bfbfc4beb0428387492ab14f053872.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)IP协议
![在这里插入图片描述](https://img-blog.csdnimg.cn/2c0b9c61860d4b899fa44850574c63a8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)32位,大约40亿个地址
源IP,即要发送数据的IP地址,目的Ip即接收方Ip
TypeService   服务类型,主要 用于选择   低延迟/低丢包率/
高吞吐率
IHL 协议头长度
 Total Length  报文长度---即封包的长度
 Identfication  报文ID,由发送方分配,由于发送时位有序的但是接收时时乱序的;
 Fragment offset  描述是否要分包,及如何拆分
 Time to Live 封包存活时间
 Protocol   描述上层的协议
 CheckSum 检验封包的正确性;

**TCP/IP工作原理**

![在这里插入图片描述](https://img-blog.csdnimg.cn/208baca004e74a26a9c5db121871e82d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

借助工具分析一下TCP/IP协议中都包含的内容----->>>>>
![在这里插入图片描述](..\pic\mysql_pic\75.png)

**手动模拟一个http协议的请求**

```java
package testAera;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * 模拟http传输协议
 */
public class MyHttp {
    public static void main(String[] args) throws IOException {
        //准备一个服务端
        ServerSocket socket = new ServerSocket(8888);
        //然后准备一个环境,一直接受请求
        while (true) {
            //blocking
            Socket accept = socket.accept();
            System.out.println("一个socket被创建了");
            //接收输入过来的流
            InputStream inputStream = accept.getInputStream();
            //通过缓存流来读
            BufferedReader br= new BufferedReader(new InputStreamReader(inputStream));
            //读取的信息通过StringBuilder拼接
            StringBuilder request= new StringBuilder();
            String line="";
            while(!(line=br.readLine()).isBlank()){
                request.append(line+'\n');
            }
            //输出拼接的信息
            System.out.println(request.toString());
            //请求之后返回一些信息给客户端吧   要使得客户端与服务端连接起来  传入输出流
            BufferedWriter bw= new BufferedWriter(new OutputStreamWriter(accept.getOutputStream()));
            bw.write("http/1.1 ok\n\n欢迎来到http的世界");
bw.flush();//刷新一下以使得信息能显示出来
        }
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2b0415efaeb4476ba3830bee64672a18.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)注意,如果是在linux环境下,可以在控制台直接输入curl+IP地址,但是入果是在windows环境下需要配置一下curl才能使用;
![在这里插入图片描述](https://img-blog.csdnimg.cn/0a369bf4ed17456c9e6fd0f434caa856.png)

## curl

curl是利用URL语法在命令行方式下工作的开源文件传输工具。它被广泛应用在Unix、多种Linux发行版中，并且有DOS和Win32、Win64下的移植版本。
[官网下载----curl](https://curl.se/download.html)
跟配置java_home一样配置好curl然后就可以使用了;

# JavaScript

JavaScript是一门脚本语言,运行在浏览器中的(需要由浏览器支持----即浏览器带有其解释器)
![在这里插入图片描述](https://img-blog.csdnimg.cn/efe4643a28ed4bb2a3d0d74b30b36bb2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

特点:


  1. 简单,规模小不需要编译运行速度快;
  2. 是一门基于对象的语言;
  3. 有事件进行驱动;
  4. 定义变量也很简单 var a,b,c 没有数据类型的限制;

**代码操练**

JavaScript 是 web 开发者必学的三种语言之一：

HTML 定义网页的内容
CSS 规定网页的布局
JavaScript 对网页行为进行编程

先简单试一下:
一般html js 与css共同来开发网页使得网页具有了交互性;

## js的引入方式

**内联-式引入**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>这是网页标题</title>
		<!--
		内嵌式引入方式
		1,head标签中,用一对script标签,嵌入js代码
		type 属性可以省略不写
		-->
		<script type="text/javascript">
		
		
		function func(){
			/*弹窗提示*/
			alert("你好")
		}
		</script>
		
	</head>
	<body>
		<input type="button"   value="点我呀"  onclick="func()"/>
	</body>
</html>

```
**外部引入**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript" src="js/jstype.js">
			
		</script>
		<input type="button"  value="点我呀!!!!" onclick="fun1()" />
	</body>
</html>

```
js文件
```javascript
		function fun1(){
		/*弹窗提示*/
		alert("你好,又见面了!!!")
}
```
**常见错误原因---->**

一些小错误产生的原因------
用了javascript标签设置交互效果,但是没有效果出现
可能的原因-----在设置交互时没有写括号;

![在这里插入图片描述](https://img-blog.csdnimg.cn/fc57e24d9a504046bb2e3c555967ec49.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可能的原因----> 函数中缺少必要的{}

![在这里插入图片描述](https://img-blog.csdnimg.cn/868d54a912a04618820d4ea1a60adac7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可能的原因------>js文件引入的位置不对![在这里插入图片描述](https://img-blog.csdnimg.cn/89a6a1aba8404b9f87bf9a33b830c6ca.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可能内敛和外链混用了
比如下面这个代码----->
![在这里插入图片描述](https://img-blog.csdnimg.cn/f6f71b2cbd0e45f8af0f67a338bc295c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)在调试的时候也会提示我们,相应在哪里错了;![在这里插入图片描述](https://img-blog.csdnimg.cn/198211e53aa3413fb91a3c81d816f654.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


**内敛引入和外链引入的优先级**

![在这里插入图片描述](https://img-blog.csdnimg.cn/c24f738e238440539b354bd6dadd28ad.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

  1. **外链方式的复用度比内敛方式要好,
  2. 降低了代码维护难度
  3. 一个页面可以引入多个js文件
    这个css样式表是一样的逻辑;



**js的ECMA部分**

## js的数据类型
js中数据类型
**数值类型(number)**----将整形和浮点型通通视为数值类型
**字符串类型**----用双引号或者单引号括起来表示
**布尔类型**-----true/false
**空(null)**-----表示没有值,不存在的引用,但并不等于空字符串或者0
**未定义(undefined)值**----表示虽然定义了,但是没有赋值
**Object类型**-----复合数据类型包括对象和数组

```javascript
		<script type="text/jscript">
		/* 
		js中是弱类型脚本语言
		1,所有的变量的数据类型用var 声明
		2,先声明后赋值也是可以的---这中方式在声明时并不知道数据类型,而是在赋值的时候才知道
		3,js中变量可以被反复声明,后声明的会将前面的覆盖	
		4,js中可以不用分号作为结尾,每一行代码都是一个独立的语句
		5,再给变量赋值可以改变变量的数据类型;
		6,基本数据类型之外时符合数据类型----OBJECT
		7,js中标识符与命名规则与java中保持一致即可,避免用$起名
		*/
	   
	  /* <!--整数和小数都叫number类型--> */
		var a=10.9999999999999;
		a=10000000;
		alert (a);
		var i;
		i=12;
		alert(i);
				var aa;
		aa="3.24";
		alert (aa);		
		/* 字符串类型 */
		var b;
		b="你好";
		
		alert(b);
	/*空值类型 */
		var c=null;
		alert(c);
		/* typeof返回数据的类型*/
		alert(typeof b);
		alert(typeof a+ "你好") ;
		/* 布尔类型 */
		var q=true;
		alert(typeof q);
		var p =1>2;
		alert(p);
		/* 数组 */
		var t=[1,2,3,4,5,6];
		alert(t);
		alert(typeof t);
		//其他引用数据类型
		var y=new Date();
		alert(y);
		alert(typeof y);
```

**数组**

数组的声明跟java中不一样,var q =[0,1,2,3]
方式一直接指定数组

```javascript
var q=[0,1,2,3];
			for (var i = 0; i < q.length; i++) {
				alert(q[i]);
			}
```

方式二,先声明后给数组赋值

```javascript
	var array= new Array();
			array=[1,2,3,4,5,6,7,8,9];
			array[0]=8;
			console.log(array);
```

方式三---创建给定长度的数组

```javascript
var array= new Array();
	array=[1,2,3,4,5,6,7,8,9];
			array[0]=8;
			alert(array[0]);
			console.log(array);//控制台打印数组
			//alert(array);
			// 创建定长数组
			var arr= new Array(5);
			console.log(arr);
```

在控制台查看信息---
![在这里插入图片描述](https://img-blog.csdnimg.cn/e0baa258a8c446af9e3480a449c2481b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)数组中存放的元素可以是任意类型的;

例如:

![在这里插入图片描述](https://img-blog.csdnimg.cn/04046fbf1f0d4d77906eb539ffd484f8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**获取数组元素与数组的扩容----**
类似于Java,可以用下表来获取数组元素,可以根据循环来遍历数组;
数组的扩容----两种方式
**第一种是修改索引长度来修改数组的长度
第二种方式是修改下标来修改数组的长度**

```javascript
var arr =[1,2,3];
			
			document.write("数组的元素--{")
			for (var i = 0; i < arr.length; i++) {
					
				if(i==arr.length-1){
					document.write(arr[arr.length-1]+"}")
				}else{
					document.write(arr[i]+",");
				}							
			}
			
			document.write("数组长度--"+arr.length);
			//通过长度修改数组的长度;
			arr.length=5;
				document.write("扩容后的数组的元素--{")
			arr[4]="helloworld";
			for (var i = 0; i < arr.length; i++) {
					
				if(i==arr.length-1){
					document.write(arr[arr.length-1]+"}")
				}else{
					document.write(arr[i]+",");
				}							
			}
			//通过索引可以需改数组长度
			arr[10]=99;
			alert(arr.length);
```

```javascript
/* foreach循环 
			遍历的i是索引而不是元素
						*/
			
			for(var i in arr){
				alert(arr[i]);
			}
```

**数组中常用方法**

```javascript
		 var array=[1,"hello",2,"world",3,"js",4,"java"];
			document.writeln("有返回");
			document.write(array.indexOf("hello"));//1
			document.writeln("没有返回");
	document.write(array.indexOf(8));//-1
			
			//数组的合并
			var array1=[5,"hellow",6,"word",7,"jscript",8,"java#"];
		var family =array.concat(array,array1);
		console.log(family);
		//alert(family);
		//数组中元素拼合,
		var c=array.join("---");//可以规定连接符
		console.log(c);
		document.write(typeof c);
		//数组元素的添加和删除
		var a=array.pop();//删除最后一个元素,返回最后一个元素的值,数组数组长度减少
		document.write("pop后数组长度--"+array.length)
		alert(a);//java
		var b=array.push("今天天气不错呀!!")//添加元素,数组长度增加
		document.write("push后数组长度--"+array.length)
		alert(b);//8  返回添加进去的元素的下标,可以看到js中数组就类似于java中的集合
		//数组中的元素移位
		
		var aq=array1.shift();//5--- 返回被移除的第一个元素,其余元素向低位移动,驻足长度对应减少
		console.log(array1);
		alert(array1.length);//7
		alert(aq);//5
		//像数组中第一个位置添加元素,其余元素向高位移动
		 var v=array1.unshift("hellokity")
		document.write(v);//8--返回新数组的长度
		//总结一下,移除时返回的时元素,增加时返回数组的长度
		//修改数组中元素
		var array3=[1,2,3];
		//通过下标来修改
		array3[2]="hello";
		//添加元素
		/* 从前面添加用push();
		从后面添加用下标
		 */
		array3.push("你好");//返回数组的长度
		array3[array3.length]="helloworld";
		
		alert(array3);//1,2,hello,你好,helloworld 
		
	//移除元素---delete
	var t=	delete array3[0];//true移除元素位置为 undefined
		console.log(t);
		
		 
		//添加和删除元素同时进行
		var array5=[1,1,1,1,1,1,1,1,1,1];//10个元素
		
		var bian=array5.splice(2,2,"2","2","2");//在第二个位置添加元素,删除两个,返回的值时删除的元素
		alert(bian);
		
		alert(array5);
		
		
		
		//可以通过调整splice的参数来达到删除元素的目的
		var array6=[1,1,1,1,1,1,1,1,1,1];//10个元素
		var y=array6.splice(2,3);
		alert(y);
		alert(array6);
		//合并数组--返回一个新数组
		var uu=array6.concat(array,array5);
		console.log(typeof uu);
		console.log(uu);
		
		//也可以通过concat方法向数组中添加元素,返回一个 新数组
		var arr1 = ["Emma", "Isabella"];
		var myChildren = arr1.concat("Jacob", "Michael", "Ethan"); 
		alert(myChildren);
				
		//数组的分割
		var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
		var citrus = fruits.slice(3); //从下标为3的开始切割,返沪i一个新数组
		var citrus = fruits.slice(1, 3); //从下标为1开始取到下标为3截至返回新数组;(范围[)     )
```

**对象**

```javascript
var cars = ["Porsche", "Volvo", "BMW"];         // 数组
var x = {firstName:"Bill", lastName:"Gates"};    // 对象 
```

## js的运算符

**大部分编程语言的基本运算符号时差不多的,在js中一些运算符号对比java会有一些差别**
![在这里插入图片描述](https://img-blog.csdnimg.cn/cd67ec1312b14654be57bb74e322172e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/6249b35d5d5643b49c7ddcbf8e1349fe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)基本运算符跟java差不多;

```javascript
/* 	// 加减乘除
			alert("1+1="+(1+1));
			alert((10-2)+"=10-2");
			alert(10*10+"=10*10");
			alert(100/10+"=100/10");
			//除不尽的会进行四舍五入操作
			alert("10/3="+10/3);
			//取模与递加递减操作
			var i;
			i=1;
			alert("i=1,输出i++后的值"+(i++));
			var j=1;
			alert("j=1,输出++j后的值"+(++j));
			var k=1;
			alert("输出k--的值"+(k--));
			var m=1;
			alert("输出--m的值"+(--m));
			// 赋值运算
			var a=8;
			b=a;
		alert("a=8,b=a求b的值"+b); */
		//除以0的结果
		//alert(10/0);//结果时是Infinity,而不是报错
		//alert(10%0);//NaN---意思是not a number
		/* alert(12.6/0);//infinity
		alert(12.6%8);//4.6
		alert(12.6%3.4);//2.4
		
		var c,d;
		c=1;d=2; */
		//c+=d;
		//alert("c=1;d=2,c+=d之后求c的值"+c);//3
		//c-=d;
		//alert("c=1;d=2,c-=d之后求c的值"+c);//-1
	   //	c*=d;
		//alert("c=1;d=2,c*=d之后求c的值"+c);//2 
		//c/=d;
		//alert("c=1;d=2,c/=d之后求c的值"+c);//0.5
		
		/* 关于+号即是运算符也是连结符号 */
		//如果两端有至少一端是字符串则是连接符,如果是数字则是运算符
		
		//alert("true+1的值"+true+1);
		//布尔类型做运算时会将true视为1,false视为0
		//alert(true+1);//2
		//alert(100-false);//100
		
		
		
		//关于== 号
		//alert(1==1);//true
		
		//alert("你好"=="你好");//true
		//alert(1=="你好");//false
		
		alert (1==true);
		alert(false==0);
		//会进行强制类型转换
		/* 先比较类型,如果类型一致,再比较内容,
		如果类型不一致,会强制转换为number再比较内容 */
		//alert(1=="1");//true
		/* === 号 */
		/* 数据类型不同 直接返回false如果类型相同 才会比较内容 */
		alert(1===1);//true
		alert(1==="你好");//false
		alert(1==="1");//false
		alert (1===true);//false
		alert(false===0);//false
```

##  js的流程控制
基本的流程控制跟java中的差不多

  1. 顺序结构  略
  2. 分支结构  if switch
  3. 循环结构 while,do while, for循环 ,foreach循环

**if 分支**

```javascript
//输出该月的季节
		var i=10;
			
		if(i==12||i==1||i==2){
			alert("冬季")
		}else if(i>=3&&i<=5){
			alert("春天");
		}else if(i>=6&&i<=8){
			alert("夏天");
		}else {
			alert("秋天");
		}
```
**switch分支**

 ```javascript
var j=12;
			switch(j){
				case 12:
				case 1:
				case 2:
				alert("大约在冬季");
				break;
				case 3:
				case 4:
				case 5:
				alert("好像在盛夏");
				break;
				case 6:
				case 7:
				case 8:
				alert("金黄的秋天");
				break;
				default:
				alert("凛冽的冬天");
				break;
			}
 ```

**while循环**

```javascript
var r=0;
			while(r<3){
				alert(r);
				r++;
			}
```
**do while循环**

```javascript
	//求0-100的和
			var t=0;
			var result=0;
				
			do {
				result+=t;
				t++;
				
			}while(t<101)
			alert(result);
```
**for循环**

```javascript
var q=[0,1,2,3];
			for (var i = 0; i < q.length; i++) {
				alert(q[i]);
			}
```

## js中的函数

**函数的格式**

function 函数名(参数列表){
方法体
}
或者
var  函数名 =function (参数列表){
方法体
}

或者
var 方法名 =new Function('js代码');
```javascript

			function methodA(){
				document.writeln("你好")
			}
		</script>
		<button  style="width: 5.25rem;height: 2.25rem;"  value="提交"  onclick="methodA()"></button>
		
		<script type="text/javascript">
				
			var methodB= function(){
				alert("helloworld");
			}
			
			var methodC = new Function('alert("这是第三种方式")');
			methodC();
		</script>
		<input type="button" style="width: 2.625rem;height: 0.625rem;"  ondblclick="methodB()" />
	
	
```
**带参数的函数-----**

```javascript
var sum =0;
			function fun1 (a,b,c){
				 sum=a+b+c;
				document.write(sum)
			}
			//调用函数
			fun1(10,20,70);
			
			
			
			var avg =function(a,b,c,d){
				return (a+b+c+d)/4;
			}
			document.write(avg(2,3,4,5));
```
关于参数的个数问题----
可以比定义参数个数少也可以多

```javascript
	
			var avg =function(a,b,c,d){
				return (a+b+c+d)/4;
			}
			document.write(avg(2,3,4,5,6));
			
```
函数的参数也可以是一个函数

```javascript
<script type="text/javascript">
			var sum = 0;

			function fun1(a, b, c) {
				sum = a + b + c;
				alert(sum);
			}
			//调用函数
			fun1(10, 20, 70);



			var avg = function(a, b, c, d) {
				return (a + b + c + d) / 4;
			}
			document.write(avg(2, 3, 4, 5, ));

			fun1(1, avg(1, 2, 3, 4), 3);
```
一下这张方式会看起来比较难以理解,只需要知道这种写法就可以了;
```javascript

		function funa(i, j) {
				return i + j;
			}
			function funb(a) {
				return a(10, 20);
			}
			var sum = funb(funa)
			alert(sum)
				
			function func(b){
				return b(20,20);
			}
			var asd=func(funa);
			alert(asd);		
			
			function methodA (a,b,c,d){
				return a-b+c-d;
			}
			function methodB(h){
				return h(1,2,3,4)
			}
			var t=methodB(methodA);
			alert(t);
			
```

> 这种方式-----可以理解为(以methodA和methodB为例)
> methodA正常定义,methodB传入参数h,h包含四个数,然后嚷着四个参数干嘛呢?
> 想让他们做加减运算,可以把methodA方法名传入(不要带括号,因为带括号的话理解为你还要往里面传参数,不带的话就相当于把方法methodB的参数传入到MethodA中),以达到目的;


## js中的对象

**字符串对象----String**

```javascript
	var str="helloworld"
			var c=str.charAt(8);
			alert(c);
			var str1="你好世界!"
			var str3=str.concat(str1);
			document.write(str3);
			var str5=str.concat(str,str1,str3);
			alert(str5
			document.write(str.repeat(3));
			var i =str.substr(1,6);//从1截取6个
			
			var q=str.substring(1,6)//从开始到6结束
		
		console.log(i);
		console.log(q);
		
		//js中将字符产解析为JS代码运行
		
		var strstr = ' var k ="你好!!!"';		
		//alert(strstr);//' var k ="你好!!!"'		
				//alert(k);//k未定义
eval(strstr);//解析字符串为代码
alert(k);
```

**自定义对象**

```javascript
			person = new Object();
			person.firstname="Gavin";
			person.lastname="Doe";
			person.age=18;
			person.eyecolor="blue";
			alert(person);
			console.log(person);//按照字母顺序排序
			document.write(person);
			
			person1= new Object();
			//使用对象构造器
			person1={firstname:"Gavin",lastname:"Doe",age:19,eyecolor:"blue"};
			alert(person1);
			
			
			
			//使用对象构造器----方法作为载体
			function girl(firstname,lastname,age,eyecolor){
				this.firstname=firstname;
				this.lastname=lastname;
				this.age=age;
				this.eyecolor=eyecolor;
			}
			// 有了构造器就可以按照构造器构造对象了
			var girlfriend=new girl("John","Tom",22,"blue");
			alert(girlfriend);
			console.log(girlfriend);
			
			// 除此之外还可以在构造中添加方法
			function boy(firstname,lastname,age,eyecolor){
				this.firstname=firstname;
				this.lastname=lastname;
				this.age=age;
				this.eyecolor=eyecolor;
				this.changename=changename;//这个不能省哦
				function changename(name){
					this.lastname=name;
				}
			
			}
			
			myfriend=new boy("Sally","Rally",48,"green");
			myfriend.changename("Tom")
			document.write(myfriend.lastname);
			console.log(boy);
			
			/* JavaScript 的对象是可变的
			
			对象是可变的，它们是通过引用来传递的。
			
			以下实例的 person 对象不会创建副本： */
			
			var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}
			 
			var x = person;
			x.age = 10;           //  x.age 和 person.age 都会改变跟java中一样；
		
		    alert(person.age);
		
		/* new 和不 new的区别：
		
		    如果 new 了函数内的 this 会指向当前这个 person 并且就算函数内部不 return 也会返回一个对象。
		    如果不 new 的话函数内的 this 指向的是 window。 */
		
		function person(firstname,lastname,age,eyecolor)
		{
		    this.firstname=firstname;
		    this.lastname=lastname;
		    this.age=age;
		    this.eyecolor=eyecolor;
		    return [this.firstname,this.lastname,this.age,this.eyecolor,this] 
		}
		
		var myFather=new person("John","Doe",50,"blue");
		var myMother=person("Sally","Rally",48,"green");
		console.log(myFather) // this 输出一个 person 对象
		console.log(myMother) // this 输出 window 对象
		
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/1fd87938c26d4bccbdd96fea395a2087.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**Number对象**

```javascript
	// document.write(Number.MAX_SAFE_INTEGER);
			// document.write(Number.MIN_SAFE_INTEGER);
			// document.write("\t")
			// document.write(Number.MAX_VALUE);
			//document.write(Number.MIN_VALUE);
			
			var t=100;
			Number.MAX_INTEGER=t;
			document.write(Number.MAX_INTEGER);
			
			//字符串解析为数字
			alert(Number.parseInt("10.234")+70);
			alert(Number.parseFloat("10")+90);
			alert(Number.isInteger(12));//判断是否为整型
			
			
			var i=10%0;//非数字
			var j=10/3;//无限循环
			document.write(Number.isFinite(i));//false
			document.write(Number.isFinite(j));//true
			document.write(Number.isNaN(i));//true
			document.write(Number.isNaN(j));//false
			
```

**日期对象**

```javascript
		var d1= new Date();
			var d2= new Date("October 13,1975 11:13:09");
		var d3=new Date(79,5,24,11,33,0);
		console.log(d1);
		console.log(d2);
		console.log(d3);
		
		
		
		var x=new Date();
		x.setFullYear(2100,0,14);
		var today = new Date();
		
		if (x>today)
		{
		    alert("今天是2100年1月14日之前");
		}
		else
		{
		    alert("今天是2100年1月14日之后");
		}
		
		
		Date.prototype.format = function (fmt) {
		  var o = {
		    "M+": this.getMonth() + 1,                   //月份
		    "d+": this.getDate(),                        //日
		    "h+": this.getHours(),                       //小时
		    "m+": this.getMinutes(),                     //分
		    "s+": this.getSeconds(),                     //秒
		    "q+": Math.floor((this.getMonth() + 3) / 3), //季度
		    "S": this.getMilliseconds()                  //毫秒
		  };
		
		  //  获取年份 
		  // ①
		  if (/(y+)/i.test(fmt)) {
		    fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
		  }
		
		  for (var k in o) {
		    // ②
		    if (new RegExp("(" + k + ")", "i").test(fmt)) {
		      fmt = fmt.replace(
		        RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
		    }
		  }
		  return fmt;
		}
		var fmt=d2.format("yyyy-MM-dd hh:mm:ss")
	alert(fmt);
	
		var now = new Date();
		var nowStr = now.format("YYYY-MM-DD"); // 2021-01-11
		var dd=new Date();
		var i=dd.getDate();//返回月份的第几天
		var ii=dd.getDay();//返回一周的第几天
		var iii=dd.getFullYear();//返会完整年份
		var iiii=dd.getTimezoneOffset();//返回
	var iiiii =dd.getYear();//返回与1900年之间的年份差
	console.log(i);
	console.log(ii);
	console.log(iii);
	console.log(iiii);	
	console.log(iiiii);
	
	var t = new Date();
	var tt = new Date("1990,10,19")
	if(t>tt){
			alert(t);
	}else{
			alert(tt);
	}

```
**自定义对象的三种方式**

```javascript
	<script type="text/javascript" >	
		//创建对像=------方式一通过Object来定义
		var per= new Object();
		per.name="扎根三";
		per.age=19;
		per.gender="男";
			
		per.perinfo=function(address){
			console.log("姓名--"+this.name+"--性别--"+this.gender+"--年龄--"+this.age+"岁--"+address+"人")
		}
			console.log(per.name+"的个人信息--");
		per.perinfo("新疆");
			
			
		</script>
		<script type="text/javascript">
		//准备一个构造方法
			function Person(name,age,gender){
				this.name=name;
				this.age=age;
				this.gender=gender;
				
				this.like=function(food){
					console.log(this.name+"--"+this.gender+"--"+this.age+"--"+"喜欢吃"+food)
				}
			}
			var perp=new Person("零四十",19,"女");
			console.log(perp.name+"的个人信息----");
			perp.like('饺子');
		</script>
		
		<script type="text/javascript">
		//json
			var per={name:"张牙舞爪",
			age:19,gender:"男",
			eat:function(food){
				document.write(this.age+"岁的"+this.gender+"孩子"+this.name+"正在吃"+food);
			}
			};
			console.log(per);
			per.eat("地瓜");
		</script>
```


# 原型链结构

![在这里插入图片描述](..\pic\mysql_pic\76.png)

# 常用事件

**鼠标事件**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>事件绑定与触发</title>


	</head>
	<body onload="startTime()">

		<!-- JS事件驱动型
		当单机按钮时,触发某些JS代码 
		一个事件可以同时绑定多个事件
		一个页面元素可以同时绑定躲着事件-->
		<script src="js/danji.js" type="text/javascript" charset="utf-8"></script>
		<input type="button" value="单击按钮!!!" onclick="clicked()" />
		<input type="button" value="双击按钮!!!" ondblclick="answer()" />
		<input type="button" value="单击可执行多个方法的按钮" onclick="clicked(),answer()" />
		<input type="button" value="悬停在我之上" onmouseover="over()" />

		<!-- // <embed src="music/再见亦是泪.mp3"></embed> -->

		<!-- 区别是一个是修改id匹配的内容,一个是修改当前对象的内容 -->
		<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是? </button>
		<!-- <p id='demo'></p> -->
		<!-- 修改当前对象内容 -->
		<button onclick="this.innerHTML=Date()">现在是</button>
		<div id ="demo">
想要知道事件请单击"现在的时间是?" 按钮
		</div>
		<!-- 鼠标按下----无论左键右键 -->
		<button onmousedown="this.innerHTML=Date()">点我试试看</button>
		<!-- 鼠标指上去 -->                     <!--    鼠标离开 -->
		<button onmouseenter="this.innerHTML=Date()" , onmouseleave="this.innerHTML=Date()">鼠标在我身上</button>

<button onmouseenter="getElementById('word').innerHTML=Date()" , onmouseout="getElementById('txt').innerHTML=Date()">鼠标离开我</button>
<button onmouseover="this.innerHTML=Date()"  onmouseout="this.innerHTML=Date()">鼠标来与去</button>

<button onmousewheel="this.innerHTML=Date()" > 鼠标</button>

<div id="word">
	这是第一块
</div>
<div id="txt">
	这是下一块
</div>



<div onkeydown="answer()"></div>
	</body>
</html>

```
**鼠标事件练习----**

```html

<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>鼠标事件练习</title>
		<style type="text/css">
			.d1{
				width: 12.5rem;
				height: 12.5rem;
				background-color: gold;
			}
		</style>
	</head>
	
	<body>
		<script src="js/danji.js" type="text/javascript" charset="utf-8"></script>
		<div class="d1" 
		onmouseenter="enter()"
		 onmouseout="out()"
		 onmouseup="up()"
		onmousedown="down()"
		 onmousemove="move()" 
		 onmouseover="over()"
		  onmousewheel="wheel">		
		</div>
	</body>
</html>

```



**按键事件**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>按键事件</title>
	</head>
	<body>
		<script type="text/javascript">
				
			function fun1(){
				console.log("按键按下");
			}
			function fun2(){
				console.log("按键抬起");
			}
			function fun3(){
				console.log("按键按下又抬起");
			}
		</script>				
		<input type="text" placeholder="请输入账号"  onkeydown="fun1()" onkeyup="fun2()" onkeypress="fun3()" />
	</body>
</html>

```
# 元素获取与修改

通过style.css样式名和通过设置class属性两种方式实现

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		
		
		<script type="text/javascript">
				
			function func(){
				var elements=document.getElementById(3);
				console.log(elements);
				//获取标签内内容并修改
				elements.innerText="塑料袋哦被清空";
			}
			// 通过类名来获取内容
			function fun2(){
				var elements=document.getElementsByClassName("A");
				console.log(elements);
				for (var i = 0; i < elements.length; i++) {
					console.log(elements[i]);
				}
			}
			//通过标签获取内容
			
			function fun3(){
				var elements=document.getElementsByTagName("input");
				console.log(elements);
				for (var i = 0; i < elements.length; i++) {
					console.log(elements[i]);
					elements[0].value="花生";
						elements[1].value="韭菜";
						elements[2].value="橘子";
							elements[6].value="桂圆";
				}
			}
		</script>
	</head>
	<body>

		<div id="1">这是一个塑料袋</div>

		<div id="2">
			塑料袋里有什么
		</div>
		<div id="3">
			塑料袋里有白菜,萝卜,花生;
		</div>
			<input type="text" value="huasheng" class="A"/>
		<input type="text" value="西瓜" class="A"/>
		<input type="text" value="jizu" class="A"/>
		<input type="text" value="西瓜"  class="B"/>
		<input type="text" value="荔枝" class="B" />
		<input type="text" value="茭瓜"  class="B"/>
		<input type="text" value="柜员"  class="B"/>
		
		
		<input type="button"  value="id获取内容" onclick="func()" />
		<input type="button"  value="class获取内容" onclick="fun2()" />
		<input type="button"  value="name获取内容" onclick="fun3()" />
	</body>
</html>

```
innerHtml 操作双标签中间的HTML

innerText  操作双标签中间的 Text

value      操作表单标签值

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.AA{
				width: 300px;
				height: 100px;
				background-color: palegoldenrod;
				font-size: 20px;
				
				
			}
		</style>
		<script type="text/javascript">
			function m() {
			var elements=	document.getElementById("div4");
				console.log(elements.innerHTML);
console.log(elements.innerText);
elements.setAttribute("class","AA");
elements.innerHTML="<h1>我和我的祖国</h1>"

			}
			function ma() {
						var elements=	document.getElementById("BQ");
							console.log(elements.innerHTML);
			console.log(elements.innerText);
			elements.setAttribute("class","AA");
			elements.value="一颗♥在跳动"
			
						}
		</script>



	</head>
	<body>
		<div id="div4">
			a
			<span id="span1">文字</span>
			b
		</div>

		<input type="text" value="我和我的祖国" id="BQ" class="textarea"/>

		<input type="button" value="获取内容"  onclick="m()" />
		<input type="button" value="修改内容" onclick="ma()" />



	</body>
</html>

```
jQuery提供了丰富的选择器功能，这个是jQuery相比JavaScript的一大优势。我们先来看一下jQuery API。可以看到提供了众多的选择器，可以非常方便简单的获取要选择的内容。

 

问题：JavaScript是如何直接获取要选择的内容的


q  getElementById( ) ：返回一个节点对象


q  getElementsByName( )：返回多个（节点数组）


q  getElementsByTagName( ) ：返回多个（节点数组）


jQuery的选择器功能要强大的多

 


基本选择器

标签选择器 $("a")  

ID选择器     $("#id")

类选择器 $(".class")    $("h2.class")

通配选择器 $("*")

并集选择器$("elem1,elem2,elem3")

后代选择器$(ul li)    

父子选择器 $(ul>li)  

后面第一个兄弟元素 prev + next

后面所有的兄弟元素 prev ~ next

这些选择器其实我们并不陌生，因为在讲解CSS样式中都有类似的选择器，并且其含义也是一样的。不同的在CSS中是对选择的内容定义CSS样式，在jQuery中用来选择内容并后续进行更多的下步操作。

 

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
        	// 必须自己会使用的选择器  id选择器  $("#id") 类选择器 $('.class属性值')  标签选择器 $("标签名")
        	
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
属性选择器

[attribute] 匹配包含给定属性的元素

[attribute1][attribute2] 复合属性选择器，需要同时满足多个属性

[attribute=value] 匹配给定的属性是某个特定值的元素

[attribute!=value] 匹配所有属性不等于特定值的元素

[attribute^=value] 匹配给定的属性是以某些值开始的元素

[attribute$=value] 匹配给定的属性是以某些值结尾的元素

[attribute*=value] 匹配给定的属性是以包含某些值的元素

 

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
         <script src="js/jquery.js"   type="text/javascript"   charset="utf-8"></script>
         <script type="text/javascript">
             $(function() {
                  //[attribute] 
                  //$("a").addClass("myClass"); 
                  //$("a[href]").addClass("myClass"); 
                  //[attribute1][attribute2] 
                  //$("a[href][title]").addClass("myClass"); 
                  //[attribute=value]   
                  //$("a[href='film-2.html']").addClass("myClass"); 
                  //[attribute!=value]   
                  //$("a[href][href!='film-2.html']").addClass("myClass"); 
                  //[attribute^=value]   
                  //$("a[href^='http']").addClass("myClass"); 
                  //[attribute$=value   
                  //$("a[href$='htm']").addClass("myClass"); 
                  //[attribute*=value]   
                  $("a[href*='mashibing']").addClass("myClass");
             });
         </script>
    </head>
    <body>
         <ul id="msb">
             <li>
                  <a href="http://www.mashibing.com/job.html">青花瓷</a>
             </li>
             <li>
                  <a href="http://www.mashibing.com/dorm">小朋友,你是否有很多问号</a>
             </li>
             <li>
                  <a href="http://www.mashibing.com/mobile">羞答答的玫瑰静悄悄的开</a>
             </li>
             <li>
                  <a href="http://www.mashibing.com/flag">月半小夜曲</a>
             </li>
             <li>
                  <a href="http://www.msb.com/film">单恋一枝花</a>
                  <ul id="film">
                      <li>
                          <a href="film-1.html">乱世佳人</a>
                      </li>
                      <li>
                          <a href="film-2.html"   title="阿郎的故事">阿郎的故事</a>
                      </li>
                      <li id="film3">
                          <a href="film-3.html">阿甘正传</a>
                      </li>
                      <li>
                          <a href="http://www.mashibing.com/film/film-4.htm"   title="鲁冰花">鲁冰花</a>
                      </li>
                      <li>
                          <a name="film-5.htm"   title="太行山上">太行山上</a>
                      </li>
                      <li>无问西东</li>
                  </ul>
             </li>
         </ul>
    </body>
</html>
```

 位置选择器

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
表单选择器

关于表单项的选择器

:text   :password  :radio  :checkbox  :hidden  :file  :submit

:input  匹配所有 input, textarea, select 和 button 元素

 

关于表单项状态的选择器

:selected  :checked  :enabled  :disabled  :hidden :visible

 

注意$("input")和$(":input")的区别

$("input")：标签选择器，只匹配input标签,$(":input")： 匹配所有 input, textarea, select 和 button 元素





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
                      <td colspan="2">
                      	<input type="submit" />
                      	<input type="reset" />
                      	
                      </td>
                      
                  </tr>
             </table>
         </form>
    </body>
</html>
```



<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/#为什么会有这个网站)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](https://blog.csdn.net/weixin_54061333?spm=1010.2135.3001.5421)

</center>

[烟花代码---html5版]( http://varyu-program.gitee.io/happy-new-year)

[![](..\pic\div\weichatcode.png)](https://mp.weixin.qq.com/s?__biz=MzkwODMyNTg0Nw==&mid=2247483665&idx=1&sn=8da1d2ea052e3c2e127e3c5281e1762c&chksm=c0cae7a9f7bd6ebfc2b89b9479ad12b6ea82e061ad51bfba6d0d220fc1ca2c30c9a826f47595#rd)