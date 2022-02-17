# tomcat异常实录

## 1,重新安装Tomcat出现提示A service with the given...

重新安装Tomcat出现提示A service with the given Service Name is already installed on this machine,

![在这里插入图片描述](https://img-blog.csdnimg.cn/0f259f04e0cb41b4be163909b42796e5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)解决方案----

如果仍然提示,异常那就需要修改以下注册表---
**第一步------->>>>>**
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a53b00ee28945c1ba625eb272f510c1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**查找注册表中关于tomcat的内容------>>>>**![在这里插入图片描述](https://img-blog.csdnimg.cn/ccd22b9719c647efa8d3bd197db56043.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)找到之后

![在这里插入图片描述](https://img-blog.csdnimg.cn/17215144b84b4820a168d323edba9cfb.png)删除,

**以管理员运行cmd,然后进入tomcat原来的安装目录下的bin**
![在这里插入图片描述](https://img-blog.csdnimg.cn/fff8bd338b164d4e8553bff402f41268.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> D:
>
> D:\Program
> Files(x86)\TomCat\apache-tomcat-9.0.53\bin(这是上一次tomcat安装的目录,找到下面的bin)
>
> sc delete Tomcat9 (数字是你安装的tomcat的版本,我的是9)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1e4fcb49a36f4c2ab7cd6905b9c3d2d5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

出现下面截图内容就表示移除tomcat服务成功了,
![在这里插入图片描述](https://img-blog.csdnimg.cn/6438730ca6e1457fa1bed34c84cd421c.png)应该可以正常安装tomcat了;



## 2,关于cmd界面下无法启动tomcat服务

**关于cmd界面下无法启动tomcat服务**
像java一样也需要配置tomcat环境
具体操作是------
再系统变量中添加   
变量名-----CATALINA_HOME
变量值(tomcat存放目录)-----D:\Program Files(x86)\TomCat\apache-tomcat-9.0.53
![在这里插入图片描述](https://img-blog.csdnimg.cn/d5485e2629f94aefb362cbfae86aa45a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

然后再path中添加
%CATALINA_HOME%\bin;
![在这里插入图片描述](https://img-blog.csdnimg.cn/4496c71294f24e76a6273b0f90923134.png)重新打开cmd运行startup.bat---开启服务

![在这里插入图片描述](https://img-blog.csdnimg.cn/651f87d3a62b424bba486c3f9ee4d763.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

shutdown.bat----关闭服务![在这里插入图片描述](https://img-blog.csdnimg.cn/bce9ab623a7f4d9c80e119faf07519db.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 3,打开Tomcat服务cmd界面乱码问题

看着是乱码,不影响tomcat的使用,但是看着不舒服那怎么办,
修改安装目录下的配置文件----

![在这里插入图片描述](https://img-blog.csdnimg.cn/75ece07d1cd849068162ec9f05ccac9b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_19,color_FFFFFF,t_70,g_se,x_16)

> 将UTF-8改为GBK

![在这里插入图片描述](https://img-blog.csdnimg.cn/7cb947a78f5f44fd923e0033c8bdf070.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



改完之后重启cmd,再次运行----显示中文了;

![在这里插入图片描述](https://img-blog.csdnimg.cn/4a3b80f4670147ec95c2001c7bdbcc54.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)





## 4,关于tomcat配置path的问题

注意,path的配置不要再额外添加内容了

比如------以下这两种情况都是错误的配置![在这里插入图片描述](https://img-blog.csdnimg.cn/b9dc713ae2a145b298e113c7de4fb29d.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/2868438673684b718908c7f376fb4492.png)

## 5,关于tomcat安装后设置manager但是无法访问的问题

问题描述---在重装系统后,重新配置tomcat环境时遇到了几个小插曲-----

第一个是-----下面这个

```
类型 异常报告

消息 java.lang.IllegalStateException: 无输出目录

描述 服务器遇到一个意外的情况，阻止它完成请求。

例外情况

org.apache.jasper.JasperException: java.lang.IllegalStateException: 无输出目录
	org.apache.jasper.servlet.JspServletWrapper.handleJspException(JspServletWrapper.java:605)
	org.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:436)
	org.apache.jasper.servlet.JspServlet.serviceJspFile(JspServlet.java:385)
	org.apache.jasper.servlet.JspServlet.service(JspServlet.java:329)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:741)
	org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)
根本原因。

java.lang.IllegalStateException: 无输出目录
	org.apache.jasper.JspCompilationContext.createOutputDir(JspCompilationContext.java:697)
	org.apache.jasper.JspCompilationContext.getOutputDir(JspCompilationContext.java:204)
	org.apache.jasper.JspCompilationContext.getClassFileName(JspCompilationContext.java:545)
	org.apache.jasper.compiler.Compiler.isOutDated(Compiler.java:468)
	org.apache.jasper.compiler.Compiler.isOutDated(Compiler.java:434)
	org.apache.jasper.JspCompilationContext.compile(JspCompilationContext.java:598)
	org.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:400)
	org.apache.jasper.servlet.JspServlet.serviceJspFile(JspServlet.java:385)
	org.apache.jasper.servlet.JspServlet.service(JspServlet.java:329)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:741)
	org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)
):注意 主要问题的全部 stack 信息可以在 server logs 里查看

```

同时在控制台上出现------>>

```
java.util.logging.ErrorManager: 4 java.io.FileNotFoundException:C.....

```

无输出目录,找不到log文件,



解决方案---->>>
右键tomcat安装文件,修改一下访问权限,将当前用户权限改为完全控制,然后重新启动tomcat就好了;

![在这里插入图片描述](https://img-blog.csdnimg.cn/d19d4a1401f142389ebecc02badb39ed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## 6,项目部署访问问题HTTP状态 404

**(项目变动后会手动重启tomcat服务器操作,排除这个的原因)**

将一个项目放在Tomcat服务器上,
![在这里插入图片描述](https://img-blog.csdnimg.cn/77e0e0f0058543b3a43804eb39c645d1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)访问路径也对,但是始终提示404未找到
![在这里插入图片描述](https://img-blog.csdnimg.cn/5f28fe4f09c84bd4aaeb04de9b00679e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**第一次尝试------>>**

检查url,确实没问题,检查conf中的(Catalina\localhost下的)配置文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/6e8bb30f90f14b1a866da32ae73d4280.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)好像也对呀,这是另一个项目的,跟这个无关;

**第二次尝试------>>**

将 WEB-INF文件夹整体移除项目之外就可以访问,所以问题可能出在WEB-INF上
![在这里插入图片描述](https://img-blog.csdnimg.cn/a67f7e63676649099823ec1f372bef50.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/adeba2b8adbb4341836445bb4adbbcb3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)问题出在哪里呢?
搞清楚一点------->>WEB-INF文件夹的问题那就要搞明白WEB-INF目录文件夹下方的内容都是些什么,有什么作用

WEB-INF是Java的WEB应用的安全目录，**客户端无法访问**，**只能通过服务端访问**，从而实现了代码的安全.

> **所以当有WEB-INF(文件夹为空)跟项目文件在同一个文件夹下,默认是项目资源无法访问;**

那该怎么访问呢?

将WEB-INF放回项目中
![在这里插入图片描述](https://img-blog.csdnimg.cn/18417d7af7e040c8aaecf9e240a653c0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

在WEB-INF中需要配置WEB项目配置文件--------->>>>>web.xml

![在这里插入图片描述](https://img-blog.csdnimg.cn/bd8958c33e4e4b66b2d8f58064ba878b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)在web.xml中配置一些信息-----先简单配置一些-------就是一个框架具体配置信息为空,但没有框架整一个空的web.xml可不行;
![在这里插入图片描述](https://img-blog.csdnimg.cn/bda6926df93047918abb67bc2d66f8ee.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**接着重启tomcat访问**

![在这里插入图片描述](https://img-blog.csdnimg.cn/b88ee6b4440942e3b136bc63c691ec2e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)正常访问;



在WEB-INF中主要是系统运行的配置信息和环境,文件夹下主要有

1. classes文件夹---->>>用于存放项目的源代码---servlet和非servlet的class文件
2. config文件夹------>>>用于存放java的配置文件
3. lib文件夹------->>>>用于存放jar包
4. web.xml------>>>用于配置WEB项目的信息描述了 servlet 和其他的应用组件配置及命名规则

![在这里插入图片描述](https://img-blog.csdnimg.cn/19d597568a0d42a98af7710106045c74.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



> **有问题不怕,重要的时沉下心来解决;**

## 7,引入静态图片失效

![在这里插入图片描述](https://img-blog.csdnimg.cn/7d78d489cbc3435d99501920eeb5535c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_18,color_FFFFFF,t_70,g_se,x_16)
什么原因呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/5bd3340b19a140b5823d8cda8ddc5533.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这里更新资源需要一定时间,所以要等一会才能行;



引入的CSS文件失效

说实在,没什么大问题,我只是当时没有写后面两个属性;![在这里插入图片描述](https://img-blog.csdnimg.cn/28a128ccd7c84403ac616f14b3893ad2.png)

引入的JS文件失效

![在这里插入图片描述](https://img-blog.csdnimg.cn/755945f2fff348a4aeac5736b6407961.png)一样的问题,格式问题;js双标签结束
link单标签结束





## 8,乱码问题合集

一个简单的小servlet,
描述:随机产生一个整数,如果为偶数,向浏览器发送"尔曹身于名俱灭",奇数则发送"不废江河万古枯"
简单的写完之后------>>>>
出现问题的代码

![在这里插入图片描述](https://img-blog.csdnimg.cn/cc277cb824ef44d6b239660dc9f4e91c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)出现的结果----

![在这里插入图片描述](https://img-blog.csdnimg.cn/38b3135eda6a4f289fc5c203459c3596.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这里可以看到如果不修改发送的数据格式的话,默认是iso-8859-1,而浏览器默认是UTF-8
![在这里插入图片描述](https://img-blog.csdnimg.cn/8d850048486c4870997298e989bb427c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)出现了乱码说明两者字符集不同;

**解决方案之前的尝试-----(错误的尝试-----仅供参考)**



修改方案一    给信息设置一个样式的时候指定字符为UTF-8但是问题并没有解决
![在这里插入图片描述](https://img-blog.csdnimg.cn/5f387b1c556445f085a75236cc47cf51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**正式的解决方案**

正式的解决方案有两种方案------------->>>>
一种是修改浏览器默认编码格式
第二种是修改发送数据的编码格式

很显然要写一个程序能兼容各种浏览器,而不是让浏览器兼容你,第二种方式是必然!

修改代码如下----

![在这里插入图片描述](https://img-blog.csdnimg.cn/31f68502e37243feb898de0846ebc833.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)结果展示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/6aa44ea9f8004d32a266c9d5de9e80b0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



导语:乱码问题产生的原因主要是字符编码的问题;
发送方要发送一串字符，首先必须用字符集给它编码变成0和1即二进制进行传输，接收方需要用同一个字符集进行解码（decode）方才能知道发送方发送的内容。如果双方所用的字符集不一致就会产生乱码。

### 8.1控制台乱码

![在这里插入图片描述](https://img-blog.csdnimg.cn/5cce1dd5cabc4338b00bd00c16427fc0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)设置Tomcat中 conf下logging.properties中所有的UTF-8编码为GBK即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/ea20e9bd8c094c9a9e14379675444685.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **原理----> 原因是windows中默认中文字符集为GBK,但是tomcat中日志输出为UTF-8,GBK对UTF-8显然是字符集不匹配;**

### 8.2post请求乱码

产生原因----->>>图解
![在这里插入图片描述](https://img-blog.csdnimg.cn/def9f28da79f42b497c9c7279be6847f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
解决方式--->
通过HttpServletRequest设置请求编码

 /*处理post请求乱码*/
  req.setCharacterEncoding("UTF-8");
![在这里插入图片描述](https://img-blog.csdnimg.cn/467e672dbed24f6f89a19fc448405eaa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)那我可不可以指定字符编码为GBK呢?

![在这里插入图片描述](https://img-blog.csdnimg.cn/75a1625bb2db4d408a56b476f3c988ea.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)原因在与这是项目本身运行环境字符集编码为UTF-8,
所以也会产生乱码(但至少不是?之类的符号了)

### 8.3get请求乱码

在之前版本的tomcat中get方式提交的数据可能会遇到修改server.xml中uri编码格式的情况,但是这在tomcat9已经解决了该问题
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd5539f887fd4f3fb83b962953c88fa4.png)   <Connector   port="8080" protocol="HTTP/1.1"

```
             connectionTimeout="20000"

             redirectPort="8443" URIEncoding="utf-8"  />

```

另一种是需要手动进行编码解码,注意:

  req.setCharacterEncoding("UTF-8"); 这种方式只对post提交方式有效;
  这一点在tomcat9之前的版本可以体现,因为版本较旧,就不在演示了



### 8.4响应乱码

浏览器将数据发送给tomcat服务器,服务器解析,当解码的字符集匹配但是发送的时候指定的编码集与数据类型不一致也会产生乱码问题,比如发送图片(ASCII),响应时指定字符集为文本(GBK),
![在这里插入图片描述](https://img-blog.csdnimg.cn/3e855365544540bbb4d0510841398b06.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

通过HttpServletResponse设置响应编码
手动编码解码-----
  //以UTF-8编码处理数据
  resp.setCharacterEncoding("UTF-8");
  解析为文本,是html文件
   resp.setContentType("text/html");![在这里插入图片描述](https://img-blog.csdnimg.cn/faaf04bdb9704253bf637b16d60b535d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)或者设置tomat连接器的编码格式---
  //设置响应头,以便浏览器知道以何种编码解析数据
  resp.setContentType("text/html;charset=UTF-8");



总结------->>>>
当没有指定解码方式和响应的数据类型(包括响应的数据编码格式)--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/97d6ac81f8c945d7ab85f19bb6795896.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)指定解码方式和响应的数据类型(包括响应的数据编码格式)----->>>
手动编码解码
![在这里插入图片描述](https://img-blog.csdnimg.cn/82e1e5e896b0494fb7025e9f8a289dd9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)设定URL的编码格式
![在这里插入图片描述](https://img-blog.csdnimg.cn/0cbc18156db641ab99d8e6b72e96409e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



这里补充一点-------
还有一种比较费劲的方式-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/76a13042f0a3459cab8144ba197c3a5a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这种方式也会得到正常解析,那要浏览器干嘛?如果发送的是图片或者其他的内容岂不很麻烦?![在这里插入图片描述](https://img-blog.csdnimg.cn/ef1357c0c4d64948a0789b86f3003b2f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## 9.JSTL中遇到的错误情景与解决方案

**使用的JSTL版本为1.2.5版本**

描述:

类型 异常报告

消息 /testdemo.jsp (行.: [16], 列: [0]) 根据标记文件中的TLD或attribute指令，attribute[value]不接受任何表达式

描述 服务器遇到一个意外的情况，阻止它完成请求。

例外情况

org.apache.jasper.JasperException: /testdemo.jsp (行.: [16], 列: [0]) 根据标记文件中的TLD或attribute指令，attribute[value]不接受任何表达式

![在这里插入图片描述](https://img-blog.csdnimg.cn/9f099d7e895f4c1b8a1e75bb02f47a67.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这是什么原因呢?

找了半天,一开始是认为自己倒的包不对,又下载了以便,发现还是那样

原来是jsp中的代码
出现了些许小毛病---
![在这里插入图片描述](https://img-blog.csdnimg.cn/958c1739ee964401a8b999fb4e8eb387.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)方框出应该导入core_rt包,即将uri更换为 下面这个就好了!

> **uri="http://java.sun.com/jstl/core_rt"**
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/b0109ed1cd1147b69a87601523b552d4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 10,"String.toLowerCase(java.util.Locale)" because "enc" is null

问题描述:
Cannot invoke "String.toLowerCase(java.util.Locale)" because "enc" is null

在配置编码字符集是出现了这样一个错误---->>

```
HTTP状态 500 - 内部服务器错误
类型 异常报告

消息 Cannot invoke "String.toLowerCase(java.util.Locale)" because "enc" is null

描述 服务器遇到一个意外的情况，阻止它完成请求。

例外情况

java.lang.NullPointerException: Cannot invoke "String.toLowerCase(java.util.Locale)" because "enc" is null
	org.apache.tomcat.util.buf.B2CConverter.getCharset(B2CConverter.java:58)
	org.apache.catalina.connector.Request.setCharacterEncoding(Request.java:1696)
	org.apache.catalina.connector.RequestFacade.setCharacterEncoding(RequestFacade.java:329)
	PostDemo.Filter.Filter0_PostDemo.doFilter(Filter0_PostDemo.java:16)
):注意 主要问题的全部 stack 信息可以在 server logs 里查看

```

看了具体的原因,   空指向异常,Cannot invoke "String.toLowerCase(java.util.Locale)" because "enc" is null

在代码的第16行
![在这里插入图片描述](https://img-blog.csdnimg.cn/ad5c72f2d9fa4537a7ba2edd1519daaf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)说明没有找到charset,检查了一下,发现IDEA没有报错,那是那里出问题了呢?
于是将注解配置模式改为xml配置,哎,正常了!
在换回注解模式,我才发现原来我注解模式下参数配置的有一些问题-----
原来的注解---->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/d79c580668284b9b8c07354024a5b8ce.png)我参数配置在过滤器外了,当然找不到该参数,于是修改之后,结果就正常了!
![在这里插入图片描述](https://img-blog.csdnimg.cn/79e9daca1a0441189aec904af228cf5f.png)

![](D:/WebRepository/pic/div/chuan.gif)