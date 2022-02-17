![](..\pic\div\bird.gif)



> JAVEE阶段需要学习的核心技能
>
> Servlet、JSP、JSTL/EL、JavaBean、MVC模式、过滤器Filter、监听器listener、Ajax  分页 
>
> 学习Java EE需要一个Web容器,目前就免费的服务器来讲,Tomcat最合适了;

# tomcat的安装

tomcat由apache开源组织使用java开发的一款web容器,在使用之前需要安装JDK及配置JAVA_HOME.Tomcat是绿色软解，解压就可使用。如果之前已经安装了其他tomcat并且还配置了CATALINA_HOME 不要忘记修改CATALINA_HOME指向我们现在使用的这个tomcat

[tomcat官网](https://tomcat.apache.org/)

tomcat启动

运行startup.bat文件。

![](..\pic\tomcat.png)

一定要配置JAVA_HOME   C:\Program Files\Java\jdk**(这个根据自己的JD版本去实际操作)
部分电脑需要配置CATALINA_HOME   D:/***/***/apache-tomcat-9.0.41
软件路径应避免中文,空格和特殊符号,可以使用_
Tomcat关闭

  运行shutdown.bat文件或者直接关闭掉启动窗口。

访问Tomcat

访问Tomcat的URL格式：http://ip:port

访问本机Tomcat的URL格式：http://localhost:8080

# tomcat的部署方式
tomcat部署的两种方式实践-------
![](..\pic\mysql_pic\27.png)

## 部署方式1
原理----将项目文件放到Tomcat服务器的webapps文件夹下,然后就可以访问了

![在这里插入图片描述](https://img-blog.csdnimg.cn/68814daa36b4454c9932d3def5ceea40.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)webapps中的文件夹详解----

![在这里插入图片描述](https://img-blog.csdnimg.cn/65d224a512bc4e07a68a41da789fc0e5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




访问方式----浏览器地址栏输入

> http://127.0.0.1:8080/ProjectMail/indexmail.html

![在这里插入图片描述](https://img-blog.csdnimg.cn/1b38f4fef7904d4297441482cc13f157.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



**访问项目中的其他资源------**

![在这里插入图片描述](https://img-blog.csdnimg.cn/9efe2bd579ca4c62bdee2ccd9908f1f4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**小结**


> **想访问文件夹下所有内容会提示错误----必须指定具体资源名称才可以访问**

![在这里插入图片描述](https://img-blog.csdnimg.cn/df6d95068a504b4386f9a0b7478fc8b2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## 部署方式2
![在这里插入图片描述](https://img-blog.csdnimg.cn/fcb7d149612340e48029c5695efca2ad.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这个时候如果访问该项目,Tomcat找不到该项目(因为默认是在webapps中找的)怎么办呢?就需要告诉tomcat服务器我的项目放到哪里了
再tomcat文件夹下找到配置文件夹(conf)再找到Catalina,
再对应IP地址的文件夹下建立配置文件------

![在这里插入图片描述](https://img-blog.csdnimg.cn/02e3a7f6ca794a93bd8e1bf514e83bfd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/00d74281b7a14b3684b51d33fa601d90.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
配置文件内容-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/e938b88e1dad425f9c2a82f22410778c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
重启tomcat,再次访问
![在这里插入图片描述](https://img-blog.csdnimg.cn/e26b05ac1c254e5a8bcf20abca7f995b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


可以正常访问,说明配置成功;

反其道行之,修改一下配置文件的文名看看会出现什么错误-------->>>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/e0941a426f214766a66d5ce5338fa36e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)出现404错误,


但是这个时候也可以访问------

修改访问地址为

http://localhost:8080/ProjectMai/indexmail.html

![在这里插入图片描述](https://img-blog.csdnimg.cn/e90735c0ecc941c58f62a1990c72b88f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)例如修改要访问的项目名----404未找到
![在这里插入图片描述](https://img-blog.csdnimg.cn/b3586d2579004a6b8fc61a2e95c2b13e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**小结**

配置文件名最好是跟项目名一致;

# tomcat的配置

## 端口号设置

首先tomcat是一项服务,那么就需要修改配置文件------server.xml

![在这里插入图片描述](https://img-blog.csdnimg.cn/a14b3e2fb97847deafa9b50339771d62.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)修改为9090端口:接着开启tomcat服务---



![在这里插入图片描述](https://img-blog.csdnimg.cn/c7269e032ad24cd59f9424c1a1ca369f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)浏览器访问8080端口,提示错误
![在这里插入图片描述](https://img-blog.csdnimg.cn/66702edcebe84d36abf13d9e0eba8ea5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)访问9090端口则正常;


![在这里插入图片描述](https://img-blog.csdnimg.cn/f86df5d850da42bd88525c9c6101684d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)接下来修改端口为80,你会发现浏览器隐藏了端口号
而且win10H2版本中并没有去访问Tomcat网页而是指向了微软的一个网页----
![在这里插入图片描述](https://img-blog.csdnimg.cn/25e547002dfd46718f6cb621cfd3ecc0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这是为什么呢?
**首先,80端口是http协议默认的端口号,在http1.1协议中如果不写端口号则默认是80端口;**
**其次为什么会指向微软的网站呢?**
原来是这样的------
打开服务,找到WorldWideWeb服务,关闭该服务;![在这里插入图片描述](https://img-blog.csdnimg.cn/31578f0d3fab4de484288cf2eabe44fa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)接下来再次重启tomcat---发现可以监听到80端口了
![在这里插入图片描述](https://img-blog.csdnimg.cn/464df1e507d14512b9f845dd8ace6d9a.png)再试----浏览器中输入localhost:80     依旧是微软加的网站,
这又是什么原因呢?
因为IIS服务没有来得及做出修改,
搜索iis,打开
![在这里插入图片描述](https://img-blog.csdnimg.cn/ce303c6f6db7470d85283d59b9df2280.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)右键停止服务;

![在这里插入图片描述](https://img-blog.csdnimg.cn/8163f61c23524c68b29e9b121c69d865.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
再次尝试-----问题解决
![在这里插入图片描述](https://img-blog.csdnimg.cn/15726fa4c42d4db997e1bc2e707e62fe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



**> Tomcat配置文件介绍**

Tomcat 的配置文件由4个xml组成，分别是 context.xml、web.xml、server.xml、tomcat-users.xml。每个文件都有自己的功能与配置方法。

context.xml

Context.xml 是 Tomcat **公用的环境配置**。 Tomcat 服务器会定时去扫描这个文件。一旦发现文件被修改（**时间戳改变了**），就会自动重新加载这个文件，而**不需要重启服务器** 。

web.xml

Web应用程序描述文件，都是关于是**Web应用程序的配置文件**。所有Web应用的 web.xml 文件的父文件。

server.xml

是 tomcat 服务器的核心配置文件，server.xml的每一个元素都对应了 tomcat中的一个组件，通过对xml中元素的配置，实现对 tomcat中的**各个组件和端口的配置**。

tomcat-users.xml

配置访问Tomcat的**用户以及角色的配置文件**。

## 并发数设置

```xml
<Connector port="8080" 
protocol="HTTP/1.1"    
minSpareThreads="100" <!--初始化创建的线程数-->
 maxSpareThreads="500"   <!--线程并发数,一旦超过这个值,tomcat就会关掉不再需要的socket线程-->
 maxThreads="1000" <!--最大并发线程数-->
 acceptCount="700" <!--指定所有可以使用的处理请求都被使用时(即最大线程并发数)---可以放到队列的请求数为700,一旦超过这个数,tomcat不予处理
 connectionTimeout="20000" 
 redirectPort="8443"   />
```



## manger账户

> Tomcat Manager是Tomcat自带的、用于**对Tomcat自身以及部署在Tomcat上的应用进行管理的web应用**。
>

> **默认情况下，Tomcat Manager是处于禁用状态的**。准确的说，Tomcat Manager需要以用户角色进行登录并授权才能使用相应的功能，不过Tomcat并没有配置任何默认的用户，因此我们需要先进行用户配置后才能使用Tomcat Manager。


tomcat的webapps文件夹中有manger和host-manager用于对项目性能进行查看,以便对项目进行调优;
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed0a2d30ff3f4afa9d273124cb396e39.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)manager目录下文件-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/7e1933209fa645a497077c8dd006697d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)查看mannager中的index.jsp

![在这里插入图片描述](https://img-blog.csdnimg.cn/08d58bb694314965874f6d5b2736d324.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**配置Tomcat Manager的访问用户**

安装好Tomcat之后,Tomcat Manager中**没有默认用户**，我们需要在tomcat-users.xml文件配置。Tomcat Manager的用户配置需要配置两个部分：角色配置、用户名及密码配置。

**配置账户信息------->>>**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f44e12076d1e403fa35424316c8c8473.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)账户权限分类----

> manager-gui角色：
> 允许访问HTML GUI和状态页面(即URL路径为/manager/html/*)
> manager-script角色：
> 允许访问文本界面和状态页面(即URL路径为/manager/text/*)
> manager-jmx角色：
> 允许访问JMX代理和状态页面(即URL路径为/manager/jmxproxy/*)
> manager- status角色：
> 仅允许访问状态页面(即URL路径为/manager/status/*)

**重要配置信息----->>>>manager**

注意::::
![在这里插入图片描述](https://img-blog.csdnimg.cn/611f993908d04ecd9058166a129bcc7e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_2,color_FFFFFF,t_70,g_se,x_16)
请注意，对于Tomcat 7以后，使用管理器应用程序所需的角色已从单一管理员角色更改为以下四个角色。您将需要分配您希望访问的功能所需的角色。
manager-gui - 允许访问HTML GUI和状态页面
manager-script - 允许访问文本界面和状态页面
manager-jmx - 允许访问JMX代理和状态页面
管理员状态 - 只允许访问状态页面
HTML接口受到CSRF保护，但文本和JMX接口不受保护。为了保持CSRF保护：
具有manager-gui角色的用户不应被授予manager-script或manager-jmx角色。

**manager-gui权限要与manager-script或manager-jmx 分开点定义**

![在这里插入图片描述](https://img-blog.csdnimg.cn/faaaaa5ef2ef4288a25bf5a2c4774426.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)配置后登录manager/index.jsp是这个样子的
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f36d61173f7491c88238fd53ab86e18.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


## host-manger账户
继续配置tomcat-users.xml 文件,打开
在上一次修改的基础上添加以下内容

![在这里插入图片描述](https://img-blog.csdnimg.cn/dad74980ec154837b3745bfb8a4844f0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**重要配置信息----->>>>host-manager**

![在这里插入图片描述](https://img-blog.csdnimg.cn/adab88ae31c54defaa8fd7e039e81f10.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
所以并不是随便整合的,即
admin-gui权限要与admin-script分开定义
![在这里插入图片描述](https://img-blog.csdnimg.cn/308a92d864d74ee798e592da60c9310c.png)
所以至少要定义两个账户才能满足安全性的要求

登录host-manager/index.jsp;是这个样子的

![在这里插入图片描述](https://img-blog.csdnimg.cn/39b4c03580ac41e3a2ae2526c1b5b533.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# 账户权限分类----验证



![](..\pic\mysql_pic\29.png)

# JavaWeb项目的建立

![在这里插入图片描述](https://img-blog.csdnimg.cn/2350690212d2428dbb08757c2ad09048.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/69fcbf30875c431f82369c4fd7f0a871.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a5018da6ae014b5e903928d7276fa398.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/fce215f76a9046cd8a4210d1ae1dd811.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ed7f619d37fb4d9382bd9df3fef5e8ba.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c8f3d548eb034873bc185de1be08c0bf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/38e0ae5efccc478c850a61151946e39e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f7a1cfc728ca410588f16e5014c6fdf6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## 项目工作原理

![](..\pic\mysql_pic\31.png)



## 响应头设置

![](..\pic\mysql_pic\32.png)

![33](..\pic\mysql_pic\33.png)



## HttpServlet的继承/实现关系
![](..\pic\mysql_pic\34.png)

有点乱,但应该能看得懂吧.

![](..\pic\mysql_pic\35.png)

### service方法实现

HttpServlet类中有两个service方法,先看第一个------>

```java
 protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //获取请求的方式----
        String method = req.getMethod();
        long lastModified;//定义最后一次修改的序列号
        //判断请求方式是什么类型,get/post.....

        if (method.equals("GET")) { //是get
            //获得最后一次修改的序列号
            lastModified = this.getLastModified(req);//这个要请求的资源的序列号
            //第一次请求会返回   服务器中最后一次修改的序列号     为-1L,
            if (lastModified == -1L) {
                //处理请求
                this.doGet(req, resp);
            } else {//否则的话就会判断序列号是否为-1L
                long ifModifiedSince;//
                try {
                    //即返回内容有修改才会将资源返回,如果没修改,则不返回资源而是用的浏览器缓存,只返回304响应,可以在浏览器上实验一下
                    ifModifiedSince = req.getDateHeader("If-Modified-Since");
                 /*   If-Modified-Since 是一个条件式请求首部，
                    服务器只在所请求的资源在给定的日期时间之后对内容进行过修改的情况下才会将资源返回，状态码为 200  。
                    如果请求的资源从那时起未经修改，那么返回一个不带有消息主体的  304  响应，
                    而在 Last-Modified 首部中会带有上次修改时间。
                    不同于  If-Unmodified-Since, If-Modified-Since 只可以用在 GET 或 HEAD 请求中。*/
                } catch (IllegalArgumentException var9) {//有异常处理异常
                    ifModifiedSince = -1L;
                }
           /*     Last-Modified  是一个响应首部，其中包含源头服务器认定的资源做出修改的日期及时间。
                它通常被用作一个验证器来判断接收到的或者存储的资源是否彼此一致。
                由于精确度比  ETag 要低，所以这是一个备用机制。
                包含有  If-Modified-Since 或 If-Unmodified-Since 首部的条件请求会使用这个字段。

            ETag HTTP响应头是资源的特定版本的标识符。这可以让缓存更高效，并节省带宽，因为如果内容没有改变，Web服务器不需要发送完整的响应。
                而如果内容发生了变化，使用ETag有助于防止资源的同时更新相互覆盖（“空中碰撞”）。
                如果给定URL中的资源更改，则一定要生成新的Etag值。 因此Etags类似于指纹，
                也可能被某些服务器用于跟踪。 比较etags能快速确定此资源是否变化，但也可能被跟踪服务器永久存留。*/

           //将获得的最后一次修改时间与服务器中修改序列号相比较,小于则返回新的请求资源并设置修改
                //如果在获得If-Modified-Since时产生了异常,也会返回新的请求资源
                if (ifModifiedSince < lastModified / 1000L * 1000L) {//如果之前请求的资源(iflastModified )   在服务器中(lastModified)做出了修改,那么处理请求返回新的资源
        this.maybeSetLastModified(resp, lastModified);
        this.doGet(req, resp);//如果是get方法,则会调用doGet()方法,那么doGet方法会做一些什么呢?--                    
                } else {//否则只返回状态码----304
                    resp.setStatus(304);
                }
            }
            //处理其他请求方式
        } else if (method.equals("HEAD")) {
            lastModified = this.getLastModified(req);
            this.maybeSetLastModified(resp, lastModified);
            this.doHead(req, resp);
        } else if (method.equals("POST")) {
            this.doPost(req, resp);
        } else if (method.equals("PUT")) {
            this.doPut(req, resp);
        } else if (method.equals("DELETE")) {
            this.doDelete(req, resp);
        } else if (method.equals("OPTIONS")) {
            this.doOptions(req, resp);
        } else if (method.equals("TRACE")) {
            this.doTrace(req, resp);
        } else {//如果没有以上的请求方式,则返回501错误提示----不支持这种请求方式即没有此类型的方法被实现
            String errMsg = lStrings.getString("http.method_not_implemented");//获取国际上通用的提示解析信息
            Object[] errArgs = new Object[]{method};
            errMsg = MessageFormat.format(errMsg, errArgs);
            resp.sendError(501, errMsg);
        }

    }
```
再看第二个----->>>>

```java
 public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        HttpServletRequest request;
        HttpServletResponse response;
        try {
// 将请求和响应强制包装成HttpServletRequest和HttpServletResponse对象
            request = (HttpServletRequest)req;
            response = (HttpServletResponse)res;
        } catch (ClassCastException var6) {
            throw new ServletException(lStrings.getString("http.non_http"));
        }
//然后调用上面的service方法
        this.service(request, response);
    }
```

> **为什么会有两个呢?** 
> 因为第一个是protect 方法,外部不能直接调用,只能在同包内访问,而第二个是public,这样由同类中的 public      service调用;



为了便于理解ifModifiedSince  和lastModified,画了一张图
![在这里插入图片描述](https://img-blog.csdnimg.cn/0ebb554a70f14e9488e12bd8eb4106a9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
浏览器中的状态![在这里插入图片描述](https://img-blog.csdnimg.cn/59cf2da80fb8453a9632bcea58dbc53e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### doGet方法

```java

    private void sendMethodNotAllowed(HttpServletRequest req, HttpServletResponse resp, String msg) throws IOException {
      //获得协议类型
        String protocol = req.getProtocol();
        //如果协议的长度不为0且协议不是以0.9结尾且协议不是以1.0结尾
        //即如果协议为空或者为http/0.9或者http/1.0返回4开头的响应----->>>这三种基本都不用了,现在以http/1.1为主要要应用,也有http/2,0
        //他们之间的区别----可以查看下面连接
        //https://www.cnblogs.com/wupeixuan/p/8642100.html
        if (protocol.length() != 0 && !protocol.endsWith("0.9") && !protocol.endsWith("1.0")) {
            resp.sendError(405, msg);//发送响应----405对于请求所标识的资源，不允许使用请求行中所指定的方法
        } else {
            resp.sendError(400, msg);//无法找到该网页”HTTP
        }

    }
```
整体的运行逻辑------
![在这里插入图片描述](https://img-blog.csdnimg.cn/e5c2ccd6cd3e4bfa99fa9dac9f06ea4e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

###  doPost方法

```java
  protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      //获取国际上通用的提示解析信息 
        String msg = lStrings.getString("http.method_post_not_supported");
        this.sendMethodNotAllowed(req, resp, msg);
    }
    
```

```java
  private void sendMethodNotAllowed(HttpServletRequest req, HttpServletResponse resp, String msg) throws IOException {
  //跟get方式类似
        String protocol = req.getProtocol();//获得协议类型
        if (protocol.length() != 0 && !protocol.endsWith("0.9") && !protocol.endsWith("1.0")) {
            resp.sendError(405, msg);//返回响应405信息
        } else {
            resp.sendError(400, msg);//返回响应400信息
        }

    }
```
其他的请求方式

HEAD、DELETE、OPTIONS、TRACE

不太常用,也不做分析了;

可以发现源码中的doGet和doPost方法只负责发送一些4**信息,其他的活啥也不干;


> 在我们自定义的Servlet中,如果想区分请求方式,不同的请求方式使用不同的代码处理,那么我么重写 doGet  doPost 即可
> 如果我们没有必要区分请求方式的差异,那么我们直接重写service方法即可 要么重写doGet  doPost 要么重写 service,必须二选一,而且必须进行重写;



## Servlet的生命周期

```java

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Test  extends HttpServlet {
    public Test(){
        System.out.println("父类的构造方法执行了---构造了一个servlet对象");
    }

    @Override
    public void init() throws ServletException {
        System.out.println("初始化----init执行了");
    }

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("service 执行了");
    }

    @Override
    public void destroy() {
        System.out.println("资源释放了");
    }
}

```
控制台打印结果----
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ab91612b68f45178b6a0f3bdac8c836.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)第一次运行项目时会实例化一个servlet对象,执行初始化,
当请求test.do时,service()方法执行了;
当第二次请求时,servlet对象并没有再次构造一个,初始化方法也没有执行,而只有service方法执行了;
原因------>

>   **当客户端浏览器第一次请求Servlet时，容器会实例化这个Servlet，  然后调用一次init方法，并在新的线程中执行service方法处理请求。  service方法执行完毕后容器不会销毁这个Servlet而是做缓存处理，
>   当客户端浏览器再次请求这个Servlet时，容器会从缓存中直接找到这   个Servlet对象，并再一次在新的线程中执行Service方法**。



那什么时候调用destroy()方法呢?

> **当容器在销毁Servlet之前对调用一次destory方法。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/46ddd7c1ff014bdcb4d03879bce192b8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
        <servlet>
        <servlet-name>myServlet</servlet-name>
        <servlet-class>com.gavin.lcy.MyServlet</servlet-class>
        <load-on-startup>6</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>myServlet</servlet-name>
        <url-pattern>/myServlet.do</url-pattern>
    </servlet-mapping>


    <servlet>
        <servlet-name>myServlet2</servlet-name>
        <servlet-class>com.gavin.lcy.MyServlet2</servlet-class>
        <load-on-startup>7</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>myServlet2</servlet-name>
        <url-pattern>/myServlet2.do</url-pattern>
    </servlet-mapping>


//配置初始化参数
<context-param>
    <param-name>username</param-name>
    <param-value>Gavin</param-value>
</context-param>
<context-param>
    <param-name>password</param-name>
    <param-value>123456</param-value>
</context-param>

</web-app>
```

```java
  //获取初始化信息
  System.out.println(servletContext.getInitParameter("username"));
System.out.println(servletContext.getInitParameter("password"));
       //获取全部的初始化信息的名字
        Enumeration<String> initParameterNames = servletContext.getInitParameterNames();
        Iterator<String> stringIterator = initParameterNames.asIterator();
    while(stringIterator.hasNext()){
        System.out.println(stringIterator.next());
    }
```
添加初始化信息,必须先添加然后别的才能获得到,否则去除的值为 null
```java
List<String> data = new ArrayList<>();
        Collections.addAll(data,"张三","李四","王五");
        servletContext.setAttribute("list",data);
        servletContext.setAttribute("gender","girl");
```



```java
  <servlet>
        <servlet-name>myServlet</servlet-name>
        <servlet-class>com.gavin.lcy.MyServlet</servlet-class>
        <!--myServlet的初始化参数-->
        <init-param>
            <param-name>id</param-name>
            <param-value>10001</param-value>
        </init-param>
        <load-on-startup>6</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>myServlet</servlet-name>
        <url-pattern>/myServlet.do</url-pattern>
    </servlet-mapping>
```
## URL的匹配规则

### 精确匹配
精确匹配是指<url-pattern>中配置的值必须与url完全精确匹配。
![在这里插入图片描述](https://img-blog.csdnimg.cn/9b8589b9a6e5492e99dce9cdcfa2b53a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16) 
### 扩展名匹配
在<url-pattern>允许使用统配符“*”作为匹配规则，“*”表示匹配任意字符。在扩展名匹配中只要扩展名相同都会被匹配和路径无关。注意，在使用扩展名匹配时在<url-pattern>***中不能使用“/”，否则容器启动就会抛出异常。***

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/51bfa7d02bed4d81baacb18f3fe40860.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


### 路径匹配


根据请求路径进行匹配，在请求中只要包含该路径都匹配。“*”表示任意路径以及子路径。注意这里是指该项目下以love为路径开头进行访问;

![在这里插入图片描述](https://img-blog.csdnimg.cn/e020da5e3b51448bb45c18ad0e0dd161.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)比如访问 like/love 就不可以

![在这里插入图片描述](https://img-blog.csdnimg.cn/0815c2965a0b4ad695a902ae4bfb349d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 任意匹配


匹配“/”。匹配所有但不包含JSP页面

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/aaedc210573847e9b7d04e68b6dc0842.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



### 匹配所有

 

![<url-pattern>/*</url-pattern>](https://img-blog.csdnimg.cn/4088b0450c8a4cbea1c058ebc73686ad.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

但是这就会有一个问题,当一个路径符合多个匹配规则时,那么该怎么办呢?


### 路径匹配优先顺序

**当一个url与多个Servlet的匹配规则可以匹配时，则按照 “ 精确路径 > 最长路径 >扩展名”这样的优先级匹配到对应的Servlet**

**案例分析**

Servlet1映射到 /abc/*

Servlet2映射到 /*

Servlet3映射到 /abc

Servlet4映射到 *.do

当请求URL为“/abc/a.html”，“/abc/*”和“/*”都匹配，Servlet引擎将调用Servlet1。

当请求URL为“/abc”时，“/abc/*”和“/abc”都匹配，Servlet引擎将调用Servlet3。

当请求URL为“/abc/a.do”时，“/abc/*”和“*.do”都匹配，Servlet引擎将调用Servlet1。

当请求URL为“/a.do”时，“/*”和“*.do”都匹配，Servlet引擎将调用Servlet2。

当请求URL为“/xxx/yyy/a.do”时，“/*”和“*.do”都匹配，Servlet引擎将调用Servlet2。

URL映射方式

在web.xml文件中支持将多个URL映射到一个Servlet中，但是相同的URL不能同时映射到两个Servlet中。

### 多映射URL的实现方式

![在这里插入图片描述](https://img-blog.csdnimg.cn/cbcfbd11492540ecbb417ab9f296c010.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5b404622d46840b285cb517a3a44de8c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)







## 基于注解式开发Servlet

```java
import javax.servlet.ServletConfig;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Enumeration;
import java.util.concurrent.TimeUnit;
//注解模式开发servlet
@WebServlet(urlPatterns={"/myServlet3.do","/Gavin.do"},loadOnStartup = 7,initParams = {@WebInitParam(name="userName",value="Gavin"),@WebInitParam(name="otherName",value="GavinY")})
public class MyServlet3 extends HttpServlet {
    @Override
    public void init() throws ServletException {
        System.out.println("MyServlet3初始化成功");
    }

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletConfig servletConfig = this.getServletConfig();
        Enumeration<String> initParameterNames = servletConfig.getInitParameterNames();

        //得到项目名
        System.out.println(servletConfig.getServletName());
        while (initParameterNames.hasMoreElements()){
            System.out.println(initParameterNames.nextElement());

        }
    }
}

```


在Servlet3.0以及之后的版本中支持注解式开发Servlet。对于Servlet的配置**不在依赖于web.xml配置文件而是使用@WebServlet将一个继承于javax.servlet.http.HttpServlet的类定义为Servlet组件。**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c5fba7e0f5764e05b3c1da293a85a8f0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/d44918a1c7aa4b30823edb224eda1d2e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



  

## 请求转发
(｡･∀･)ﾉﾞ嗨,也不说什么定义了,直接上大白话-----

> **就是当客户端向一个服务器A请求一个资源,但是这个服务器A上没有该资源,那么服务器A向有该资源的服务器B请求,服务器A得到资源后再将此资源转发给客户端;**

### forward转发

> **功能描述----浏览器向MyServlet4发出请求,但是MyServlet4里没有想要的资源,于是MyServlet4将请求转发至MyServlet5**

代码1---->>>

```java
@WebServlet(urlPatterns = "/MyServlet4.do",loadOnStartup = 7)
public class MyServlet4 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("MyServlet4响应---");
        String fruit = req.getParameter("fruit");
        System.out.println("fruit--"+fruit);
        //将请求转发给另一个对象

        //首先获得一个请求转发器
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("MyServlet5.do");
//    由请求转发器做出转发动作
       // forward模式 转发
requestDispatcher.forward(req,resp);
    }
}
```

```java
@WebServlet(urlPatterns = "/MyServlet5.do",loadOnStartup = 8)
public class MyServlet5 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        System.out.println("MyServlet5.do--收到了请求");
        PrintWriter writer = resp.getWriter();
        String []apple={"红富士","嘎啦","金帅","乔纳金","昂林","红香蕉","北斗"};
        for (int i = 0; i < apple.length; i++) {
            writer.write(apple[i]+" ");
        }

    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4a37baf77dce4a7ea9d5957107ceb4c6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![](..\pic\mysql_pic\46.jpg)
简单描述一下阶段一和阶段二

阶段一是服务器A 收到了请求,发现自己处理不了,于是将 该请求以及响应对象传给了服务器B,由服务器B对请求做出响应,注意 服务器B中的请求对象与服务器A中的请求对象是同一个,回应对象亦然(因为服务器A在forward 转发时将两个作为参数传给了服务器B)

> **那么问题就来了,我在服务器A中预先响应一些信息,那么客户端能接收到在服务器A中的响应吗?**

试一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/7a9cf413875f459ca4c15462ec6d7bfa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)什么原因你呢???
原因就是在我们请求转发时,会将转发的resp对象中的信息完全清空,然后交由受托服务器去处理;
因此,在forward转发模式下,请求应该完全交给目标资源去处理,我们在源组件中,不要作出任何的响应处理;
**在forward方法调用之前先响应----**
1,设置了解码方式,那么在客户端也接收不到该信息;
2,没设置解码方式,那么会影响到 真正响应服务的的响应
![在这里插入图片描述](https://img-blog.csdnimg.cn/f080baabcbaf4602820bcd8204520448.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


**在forward方法调用之后,做出响应**
1,设置解码和不设置对真正的响应没有什么影响;因为在这之前已经将请求和响应对象转发给真正响应的服务器了;
感兴趣的可以自己验证;


**小结----**

>  /*
>       * 1请求转发是一种服务器的行为,是对浏览器屏蔽
>       * 2浏览器的地址栏不会发生变化
>       * 3请求的参数是可以从源组件传递到目标组件的
>       * 4请求对象和响应对象没有重新创建,而是传递给了目标组件
>       * 5请求转发可以帮助我们完成页面的跳转
>       * 6请求转发可以转发至WEB-INF里
>       * 7请求转发只能转发给当前项目的内部资源,不能转发至外部资源
>       * 8常用forward

#### forword()处理特点

1由于forword()方法**先清空用于存放响应正文的缓冲区**，因此源Servlet生成的响应结果不会被发送到客户端，只有目标资源生成的响应结果才会被发送到客户端。

2如果源Servlet在进行**请求转发之前，已经提交了如下方法（flushBuffer(),close()方法），那么forward()方法抛出IllegalStateException。**为了避免该异常，不应该在源Servlet中提交响应结果。
如果用了flushBuffer()或者close()方法会出现什么情况呢?

就是要转发到的服务器不会做出任何响应,即接受不到转发过来的请求;

![在这里插入图片描述](https://img-blog.csdnimg.cn/e087412bf9b548bfa0f10148d45d1363.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### include转发

```java

@WebServlet(urlPatterns = "/MyServlet6.do",loadOnStartup = 9)
public class MyServlet6 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//include模式下 做出响应的时源组件,所以在原组件中也要设置解码格式
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
resp.getWriter().write("你想要什么种类的苹果?");

       //向MyServlet7转发
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("MyServlet7.do");
        requestDispatcher.include(req,resp);
        resp.getWriter().write("我们的苹果很好吃");
        System.out.println("我们的苹果很好吃");
    }
}

```

```java

@WebServlet(urlPatterns = "/MyServlet7.do",loadOnStartup = 10)
public class MyServlet7 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        String []apple={"红富士","嘎啦","金帅","乔纳金","昂林","红香蕉","北斗"};
        for (int i = 0; i < apple.length; i++) {
            writer.write(apple[i]+" ");
        }
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d90367fe3334e9f8e3c5f1a044c3c70.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
分析一下可以发现,include转发方式 真正响应的是  源组件,
既然是源组件,那么就可以在源组件中做出响应,该响应可以在转发之前也可以在转发之后;

转发之前的响应内容放在转发之前,之后则放在转发之后;

即服务器A处理不了的请求转发给另一个服务器B,由服务器B将处理好的请求返回给该服务器A,服务器A在此回应的基础上再做一些回应内容的添加;
原理如下图----
![在这里插入图片描述](https://img-blog.csdnimg.cn/eb60aff846e44a0086945239e1549b71.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

#### include()处理特点

include与forward转发相比，包含有以下特点：

1源Servlet与被包含的目标资源的输出数据都会被添加到响应结果中。

2在目标资源中对响应状态码或者响应头所做的修改都会被忽略。

1,请求转发是服务器的动作,对浏览器屏蔽的.所以浏览器看不到地址栏有什么变化;
2,请求的参数是可以从源组件中传递到目标组件中的
3,请求对象和响应对象并没有重新创建而是传递给了目标组件
4,请求转发可以帮助我们完成页面跳转;
![在这里插入图片描述](https://img-blog.csdnimg.cn/bdc81135c8ff43c9a8fd2529c6643004.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)5,请求转发可以转发到Web-inf下的资源
即请求转发可以请求到当前项目里的内容而不可以申请外部资源

### flushBuffer()和close()

![在这里插入图片描述](https://img-blog.csdnimg.cn/40e8ca3d532c49c8ada205f8681e2fc1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/c6c2a742d7f24d49b95f809e640b33a1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
> 总结
> 服务器响应的时候会有一个缓存,如果没有调用resp.flushBuffer()或者write.flush(),那么会等着所有响应内容都打包好了再一块返回客户端;
> 如果使用了,则会处理完一块数据发送一块数据给客户端**





### redirect()

```java

//注解模式开发servlet
@WebServlet(urlPatterns={"/myServlet8.do"})
public class MyServlet8 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("myServlet9收到了请求");
        String fruit = req.getParameter("fruit");
        System.out.println("fruit--"+fruit);
       //响应重定向
        resp.sendRedirect("myServlet9.do");

    }
}

```

```java
//注解模式开发servlet
@WebServlet(urlPatterns={"/myServlet9.do"})
public class MyServlet9 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       // req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        PrintWriter writer = resp.getWriter();
        String []apple={"红富士","嘎啦","金帅","乔纳金","昂林","红香蕉","北斗"};
        for (int i = 0; i < apple.length; i++) {
            writer.write(apple[i]+" ");
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/55be336bdceb41c6b844e7ebd68f30e9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16) 那响应重定向的工作原理是怎样的呢?

![在这里插入图片描述](https://img-blog.csdnimg.cn/0f651022e0cb4349adb929879695e4aa.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)**简言之就是浏览器请求时处理不了请求,回应了一句你去哪个服务器看看吧;**

小结:
1,响应重定向---时浏览器行为,时服务器给出处理后让重新指向另一个服务器地址;
2,重定向时,请求对象和响应对象都会重新建立,所以第一次请求时的参数是不会携带的;
3,重定向依旧可以跳转到html/jsp等网页资源
4,由于重定向时浏览器行为,所以不能访问WEB-INF中的资源的'
5.重定向可以帮助跳转到外部资源的;

![在这里插入图片描述](https://img-blog.csdnimg.cn/afb0a9597c894ada875c1b33a335c9a6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以看到302 重定向的信息;

### 请求转发路径问题

web文件夹下目录结构-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/263e27308947477096073ce70f32ef70.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)现在想要在a页面中跳转到aa .html以及跳转到b.html
那超链接中的地址该怎么写?
两种方式---
第一种是相对路径

就是相对于a.html   其他文件的位置;
由于 aa.html跟a.html在同一个文件夹下 所以跳转到aa.html的相对路径可以这么写---aa.html



不以斜线开头表示----直接跳转到本文件夹下的某一个内容;
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
this is page a
<br/>
<!--相对路径-->
<!--不以斜线开头-->
<a href="aa.html" target="_blank">跳转到aa</a>
<a href="../../rest/b/b.html" target="_blank">跳转到b</a>

</body>
</html>
```
而如果要跳转到b.html,由于b.html跟a.html不在同一个文件夹下,但是他在a.html的父级文件夹 test的同辈-----rest中,所以   要网上倒腾两代(本代和父代直到爷爷代) ../../

即下图所示![在这里插入图片描述](https://img-blog.csdnimg.cn/7fb62b01707049f19b58fb85153528d0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
this is page a
<br/>
<!--相对路径-->
<!--不以斜线开头-->
<a href="aa.html" target="_blank">跳转到aa</a>
<a href="../../rest/b/b.html" target="_blank">跳转到b</a>
<!--绝对路径-->
<!--以斜线开头-->
<a href="/GavinP/test/a/aa.html" target="_blank">跳转到aa</a>
<a href="/GavinP/rest/b/b.html" target="_blank">跳转到b</a>
C:\Users\Gavin\IdeaProjects\ThreadDemo\GavinP\web\test\a\aa.html
</body>
</html>
```
**总结----
相对路径,不以/开头,就是相对路径  ..代表向上一层
    绝对路径,以/开头   在页面上 /代表从项目的部署目录开始找  从webapps中开始找
    页面的绝对路径要有项目名,除非我们的项目没有设置项目名**

还有一一种写法是基于base标签实现相对路径;

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--<base>标签的作用是补充相对路径开头的路径内容-->
    <!--作为相对路径的基准-->
    <base href ="http://127.0.1:8080/GavinP/">
</head>
<body>
this is page a
<br/>
<!--相对路径-->
<!--不以斜线开头-->
<a href="aa.html" target="_blank">跳转到aa</a>
<a href="../../rest/b/b.html" target="_blank">跳转到b</a>
<br/>
<br/>
<!--绝对路径-->
<!--以斜线开头-->
<a href="GavinP/test/a/aa.html" target="_blank">跳转到aa</a>
<a href="GavinP/rest/b/b.html" target="_blank">跳转到b</a>
<br/>
<br/>
<a href="test/a/aa.html" target="_blank">跳转到aa</a>
<a href="rest/b/b.html" target="_blank">跳转到b</a>

</body>
</html>
```

其中<base>标签要放在head标签之中,否则不起作用;

```java
package com.gavin.lcy;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
//@WebServlet(urlPatterns = "/RequestQueenDemo.do")
//urlPatterns 模式下
@WebServlet(urlPatterns = "/my/c/cc/RequestQueenDemo.do")
public class RequestQueenDemo extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //绝对路径
        //RequestDispatcher requestDispatcher = req.getRequestDispatcher("/test/a/a.html");
        //相对路径
       // RequestDispatcher requestDispatcher = req.getRequestDispatcher("test/a/a.html");
        //相对路径
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("../../../test/a/a.html");

        requestDispatcher.forward(req,resp);
    }
}

```

```java
package com.gavin.lcy;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

//@WebServlet(urlPatterns = "/RequestQueenDemo.do")
//urlPatterns 模式下---影响相对路径
//@WebServlet(urlPatterns = "/my/c/cc/RequestQueenDemod.do")
@WebServlet(urlPatterns = "/RequestQueenDemod.do")

public class RequestQueenDemod extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // resp.sendRedirect("../../../test/a/a.html");
        //绝对路径---带项目名
        //获取i项目名
        ServletContext servletContext = this.getServletContext();
        String contextPath = servletContext.getContextPath();
        System.out.println(contextPath);
        resp.sendRedirect(contextPath+"/index.html");
    }
}

```


##  Cookie与Session

### Cookie


 * @desc Cookie对象与HttpSession对象的作用是
 * 维护客户端浏览器与服务端的会话状态的两个对象。
 * 由于HTTP协议是一个无状态的协议，
 * 所以服务端并不会记录当前客户端浏览器的访问状态，
 * 但是在有些时候我们是需要服务端能够记录客户端浏览器的访问状态的，
 * 如获取当前客户端浏览器的访问服务端的次数时就需要会话状态的维持。
 * 在Servlet中提供了Cookie对象与HttpSession对象用于维护
 * 客户端与服务端的会话状态的维持。
 * 二者不同的是Cookie是通过客户端浏览器实现会话的维持，
 * 而HttpSession是通过服务端来实现会话状态的维持。
    */


#### Cookie的java实现
代码1
```java

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/CookieDemo.do",loadOnStartup = 10)
public class CookieDemo  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  //  创建一个cookie
        Cookie cookie= new Cookie("userName","Gavin");
        //在响应时添加Cookie
       resp.addCookie(cookie);
       //第一次访问时获取Cookie为null,再次访问时就可以获取到的Cookie
        String cookie2 = req.getHeader("Cookie");
       System.out.println(cookie2);
    }
}
```

### 状态Cookie与持久化Cookie
代码1中的Cookie是状态Cookie,在重新打开浏览器访问项目中的其他资源时并不会携带该Cookie,

![](..\pic\mysql_pic\46.png)

 Ø  Cookie使用字符串存储数据

Ø  Cookie使用Key与Value结构存储数据

Ø  单个Cookie存储数据大小限制在4097个字节

Ø  Cookie存储的数据中不支持中文，Servlet4.0中支持

Ø  Cookie是与域名绑定所以不支持跨一级域名访问

Ø  Cookie对象保存在客户端浏览器内存上或系统磁盘中

Ø  Cookie分为持久化Cookie(保存在磁盘上)与状态Cookie(保存在内存上)

Ø  浏览器在保存同一域名所返回Cookie的数量是有限的。不同浏览器支持的数量不同，

Chrome浏览器为50个

Ø  浏览器每次请求时都会把与当前访问的域名相关的Cookie在请求中提交到服务端。

3.2.2 Cookie对象的创建

Cookie cookie = new Cookie("key","value")
通过new关键字创建Cookie对象

response.addCookie(cookie)

通过HttpServletResponse对象将Cookie写回给客户端浏览器。

3.2.3 Cookie中数据的获取

通过HttpServletRequest对象获取Cookie，返回Cookie数组。

Cookie[] cookies = request.getCookies()

3.2.4 Cookie不支持中文解决方案

在Servlet4.0版本之前的Cookie中是不支持中文存储的，如果存储的数据中含有中文，代码会直接出现异常。我们可以通过对含有中文的数据重新进行编码来解决该问题。在Servlet4.0中的Cookie是支持中文存储的。

 

java.lang.IllegalArgumentException:   Control character in cookie value or attribute.


可以使用对中文进行转码处理

URLEncoder.encode("content","code")

将内容按照指定的编码方式做URL编码处理。

URLDecoder.decode("content","code")

将内容按照指定的编码方式做URL解码处理。

3.2.5 Cookie持久化和状态Cookie

状态Cookie：浏览器会缓存Cookie对象。浏览器关闭后Cookie对象销毁。

持久化Cookie：浏览器会对Cookie做持久化处理，基于文件形式保存在系统的指定目录中。在Windows10系统中为了安全问题不会显示Cookie中的内容。

当Cookie对象创建后默认为状态Cookie。可以使用Cookie对象下的cookie.setMaxAge(60)方法设置失效时间，单位为秒。一旦设置了失效时间，那么该Cookie为持久化Cookie，浏览器会将Cookie对象持久化到磁盘中。当失效时间到达后文件删除。

###  Cookie跨域问题

域名分类：

域名分为顶级域、顶级域名(一级域名)、二级域名。


​                       

域名等级的区别：

一级域名比二级域名更高级，二级域名是依附于一级域名之下的附属分区域名，即二级域名是一级域名的细化分级。例如：baidu.com 为一级域名，news.baidu.com为二级域名。

Cookie不支持一级域名的跨域，支持二级域名的跨域。
![在这里插入图片描述](https://img-blog.csdnimg.cn/6bd535cbd6cf4f7fa8088e920f03bfa7.png)


![在这里插入图片描述](https://img-blog.csdnimg.cn/78e913d6a4b34e1aa965a077394e73df.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/958e810f61d04643a393bdcaac9a47d7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a1e880692d0140388c0fa818ac486eb3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
Cookie对象总结

Cookie对于存储内容是基于明文的方式存储的，所以安全性很低。不要在Cookie中存放敏感数据。在数据存储时，虽然在Servlet4.0中Cookie支持中文，但是建议对Cookie中存放的内容做编码处理，也可提高安全性。



### HttpSession

![在这里插入图片描述](https://img-blog.csdnimg.cn/499e2145bccf4dcab8472d0bce0579ee.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)有几种情况,再次访问时不会携带JESSISIONID:
浏览器端----
1,再访问之前已经关闭浏览器,打开浏览器再次访问,不会携带之前的JESSIONid,服务器会视为第一次访问;
原理是-----由于创建的Httpsession对象会将生成的jessisionID以cookie的形式发送给给浏览器,此时的Cookie为状态Cookie,存放于内存之中,关闭浏览器再次打开就会丢失原来的Cookie信息;
2,手动清除Cookie
3,达到最大不活动时间(一般为30min)----即两次访问时间之间的间隔超过指定时间服务器也会销毁原来Jessionid,
4,服务器重启也会丢失原来的JEssionID;
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ed5aa1414a040d384955b68ac1a69cb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
5,手动设置HttpSession不可用

```java
@WebServlet(urlPatterns = "/HttpSessionLearn.do")
public class HttpSessionLearn  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        //HttpSession     4.0已经支持中文了
        session.setAttribute("银行卡号","12345678910");
session.invalidate();//HttpSession不可用
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e3a53580d2544ad78bf3b31312673d22.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这里输出NULL,说明手动设置JESSIONID不可用生效
![在这里插入图片描述](https://img-blog.csdnimg.cn/85b1dd3ab95f4873954fe21076f54416.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> HttpSession的销毁方式

HttpSession的销毁方式有两种：

Ø  通过web.xml文件指定超时时间(最大不活动时间)

Ø  通过HttpSession对象中的invalidate()方法销毁当前HttpSession对象

   我们可以在web.xml文件中指定HttpSession的超时时间，当到达指定的超时时间后，容器就会销毁该HttpSession对象，单位为分钟。该时间对整个web项目中的所有HttpSession对象有效。时间的计算方式是根据最后一次请求时间作为起始时间。如果有哪个客户端浏览器对应的HttpSession的失效时间已到，那么与该客户端浏览器对应的HttpSession对象就会被销毁。其他客户端浏览器对应的HttpSession对象会继续保存不会被销毁。


```xml
<session-config>

    <session-timeout>30</session-timeout>

  </session-config>
```

#### HttpSession的java实现

```java
@WebServlet(urlPatterns ="/HttpSessionDemo.do" )
public class HttpSessionDemo extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        /*获得Httpsession对象
                由于是接口, 所以要通过req获取
        从req中获取session的cookie
                如果获取失败, 则会认为上次会话已经结束,
                再这里开启一个新的会话, 创建一个新的httpsession并返回给浏览器

                如果获取成功,则根据sessionID再服务器中找到对应的httpsession对象
                找到了则返回找到的httpsession
                如果没找到,则创建新的httpsession并将sessionId以cookie的形式发送到resp对象中相响应给浏览器

                */
/*httpsession一般用于保存用户的 登录信息
        用户的权限
                用户的其他信息*/
        HttpSession session = req.getSession();
session.setAttribute("userName","Gavin");
session.setAttribute("userPwd","123456");
session.setAttribute("userLevel","1");

    }
}

```

浏览器访问---- 确实是同一个jessionid
![在这里插入图片描述](https://img-blog.csdnimg.cn/80a2dd4ac01a4a2188ad5370aed0f98d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/558b2f9adde34f5596d0d7540e8df1b9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

细节-----
第一次访问时,请求行里没有Cookie  JESSISION的信息,响应行里有
![在这里插入图片描述](https://img-blog.csdnimg.cn/8d46048870a140378ebf90f19207cc6b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
再次访问
![在这里插入图片描述](https://img-blog.csdnimg.cn/d158d56a033f423198bb7c3d973dabdb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/df280dba75194587ac324501736eeff7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## Cookie与HttpSession对象总结

**1,Cookie是存放在浏览器器中的用于存放重要的的信息显得很不安全;
2,HttpSession是存放在服务器上的,一般是比较安全的;
3,Cookie的生命周期比较短,在没有特殊设置的情况下退出浏览器即会消除;
4,httpsession的生命周期是不确定的,第一次访问创建到下一次访问时间如果超时则会清除;服务器重启也会清除;
5,Cookie存放的信息很少,一般浏览器也会限制Cookie存放的数量;
  单个cookie保存的数据不能超过4K，很多浏览器都限制一个域名保存cookie的数量。而HttpSession没有容量以及数量的限制。
6我们可以在HttpSession对象中存储数据，但是由于HttpSession对象的生命周期不固定，所以不建议存放业务数据。一般情况下我们只是存放用户登录信息。**



## 用Cookie记录用户访问次数

```java
package com.gavin.lcy;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(urlPatterns = "/CookieDemo2.do")
public class CookieDemo2 extends HttpServlet {
    static int  i;
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");

        PrintWriter writer = resp.getWriter();
        Cookie[] cookies = req.getCookies();
        boolean flag=true;
        if(null!=cookies){//说明之前有过访问
            for (Cookie cookie : cookies) {
                if("count".equals(cookie.getName())){
                    String cookieValue = cookie.getValue();
                    i = Integer.parseInt(cookieValue)+1;
                    //新建一个cookie以替换掉原来的;
                    //后来设置的Cookie会替换掉之前的
                    Cookie c= new Cookie("count",String.valueOf(i));
                    //将cookie添加到响应中
                    resp.addCookie(c);
                    writer.write("欢迎第"+i+"次访问");
                    flag=false;
                }

            }
        }
        if(flag){
            Cookie cookie= new Cookie("count","1");
            resp.addCookie(cookie);
            writer.write("欢迎第"+1+"次访问");
        }

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/81d5fa7b410c457c83fb2e97d911ff60.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)代码验证---

![在这里插入图片描述](https://img-blog.csdnimg.cn/92c22ab19cd2456faa5e7e26152cd12f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



> 刚学javaWeb时对一些信息的获取有一些不懂,请求行获取的信息和请求域获取的信息有什么不一样的?除了方法不一样........但是搞清楚了原理,一切迎刃而解;如果你也有疑惑,那就看看这篇文章吧!



## Servlet中三大域对象

请求行与请求域

就是浏览器地址栏处的信息-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/67c7bde3e66144d9a806785e881c3b87.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)当请求信息时有时候我们会填写一些信息,然后到服务器中去请求;
举个很常见的例子---->>>百度搜索关于javaweb的一些内容

![在这里插入图片描述](https://img-blog.csdnimg.cn/54281d394b454780bbfc1f45f5f4fe67.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)红色方框处是我们的请求内容,其他内容是百度服务器自己添加的一些关于请求信息的内容;

那我们怎样获得请求信息呢?

来一个简单的网页----

```
<!DOCTYPE html>
<html >
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form method="get" action="MyServlet2.do">
    账&emsp;户:<input type="text" name="user" required><br>
   密&emsp;码:<input type="password" name="pwd" required><br>
    <input type="submit"value="登录">
</form>
</body>
</html>
```
在浏览器中输入账户,密码
![在这里插入图片描述](https://img-blog.csdnimg.cn/7cf85bb8593c4ccfb7f6797d13d039c9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```

//这里是绝对路径
@WebServlet(urlPatterns = "/MyServlet2.do")
public class MyServlet1 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//获得请求行的信息
        String user = req.getParameter("user");
        String pwd = req.getParameter("pwd");
        System.out.println(user);
        System.out.println(pwd);
    }
}
```



点击登录,则会在控制台上打印账户和密码
![在这里插入图片描述](https://img-blog.csdnimg.cn/1e5ca46ba9534fa38823f8069478c12d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
关于请求行中的信息,是以键值对的形式存储的,获得请求行中的信息----
**req.getParameter("user");**

图解请求行信息----

![在这里插入图片描述](https://img-blog.csdnimg.cn/4571e55afa064be88c0b0f55d5cea57e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 请求域
对于请求行,它是通过HttpServletRequest req 来获取请求行中的信息;
关于请求域,其实它也是一个对象,

> **什么是域对象?** 
> 那些能放数据并存储传递数据作为数据存放区域的对象 简单来说就是能够存储数据,获取数据,传递数据的对象


Servlet中有三大域----------对象------------------作用范围

Request域                HTTPServletRequest          一次请求/请求转发
Session域                 HTTPSession                       一次会话(跨请求)
Application域          ServletContext                    任意一次请求和会话(跨会话)

请求域中的信息怎么获取呢

```

@WebServlet(urlPatterns = "/MyServlet10.do")
public class MyServlet10  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      //向Request域中添加数据
        List<String> g= new ArrayList<>();
        Collections.addAll(g,"a","b","c");
        req.setAttribute("list",g);
        req.setAttribute("java","工程师");
        req.setAttribute("Python","人工智能");
        //当键值对的name相同时,会取后面一个
        req.setAttribute("java","全栈工程师");
        //请求转发到MyServlet11,以上信息才能继续存活
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("MyServlet11.do");
        requestDispatcher.forward(req,resp);
    }
}

```
这里采用请求转发的形式,请求转发到MyServlet11,以上信息才能继续存活
```

@WebServlet(urlPatterns = "/MyServlet11.do")
public class MyServlet11 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       /* Enumeration<String> names = req.getAttributeNames();
        while (names.hasMoreElements()) {
            String s = names.nextElement();
            System.out.println(s + "--" + req.getAttribute(s));
        }*/
        System.out.println(req.getAttribute("list"));
        System.out.println(req.getAttribute("java"));
        System.out.println(req.getAttribute("Python"));

    }
}
```
而如果是响应重定向,则会得到null;
![在这里插入图片描述](https://img-blog.csdnimg.cn/91b66dcaefbd4bb4bfc8f9e9fce4c039.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
什么原因呢?
原因就在于如果是请求转发,则会将请求对像和响应对象通通转发给目标服务器,而如果是响应重定向,则会再目标服务器上新建一个请求对象;

请求域中的方法------->>>>

setAttribute(name,value); 设置修改数据
getAttribute(name);获得数据的方法
removeAttribute(name);移除数据的方法


三大域对象的作用范围是不同的----
![在这里插入图片描述](https://img-blog.csdnimg.cn/8fdada47fe0b4039a155c8d3a6d06f65.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### Session域

**有效范围**

**单次会话内**有效,可以跨多个请求

**生命周期**

**创建 会话的产生,第一次发生请求-----会话的开始**

**使用 本次会话之内,**浏览器和服务器之间发生多次请求和响应有效

**销毁 会话结束**,如:浏览器失去JSESSIONID、到达最大不活动时间、手动清除

```

@WebServlet(urlPatterns = "/MyServlet10.do")
public class MyServlet10  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      //向Request域中添加数据
        List<String> g= new ArrayList<>();
        Collections.addAll(g,"a","b","c");
        HttpSession session = req.getSession();
        session.setAttribute("list",g);
        session.setAttribute("java","工程师");
        session.setAttribute("Python","人工智能");
        //当键值对的name相同时,会取后面一个
        session.setAttribute("java","全栈工程师");
        //请求转发到MyServlet11,以上信息才能继续存活
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("MyServlet11.do");
        requestDispatcher.forward(req,resp);
    }
}

```

```
@WebServlet(urlPatterns = "/MyServlet11.do")
public class MyServlet11 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            HttpSession session = req.getSession();
        System.out.println(session.getAttribute("list"));
        System.out.println(session.getAttribute("java"));
        System.out.println(session.getAttribute("Python"));

    }
}

```

request域在一次请求到一次响应----生命周期结束,当然中间还可以加上请求转发;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a74de22f3d27487f962d982da18acb46.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/889dda9258394c37ad6af9372e46f8d4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### Application域

```

@WebServlet(urlPatterns = "/MyServlet12.do")
public class MyServlet12 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      //向Request域中添加数据
        List<String> g= new ArrayList<>();
        Collections.addAll(g,"a","b","c");
        ServletContext application = this.getServletContext();
        application.setAttribute("list",g);
        application.setAttribute("java","工程师");
        application.setAttribute("Python","人工智能");
        //当键值对的name相同时,会取后面一个
        application.setAttribute("java","全栈工程师");

    }
}

```

```

@WebServlet(urlPatterns = "/MyServlet13.do")
public class MyServlet13 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        System.out.println(servletContext.getAttribute("list"));
        System.out.println(servletContext.getAttribute("java"));
        System.out.println(servletContext.getAttribute("Python"));

    }
}

```
怎么体现Application域作用范围更大呢?

1---可以关掉浏览器然后重新访问;
2,--换一个浏览器访问;
![在这里插入图片描述](https://img-blog.csdnimg.cn/582b2e65cecb4c9ab6af5430d8077f29.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/d9b781d3e09a4711aaf404195546ba92.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/4301279708af476aadcdb74875754891.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> application的生命周期随着服务的启动而开始,随着服务起的关闭/重启而结束;
> 由于生命周期太长,不适合放一些用户数据,避免application域中的信息太多而过于臃肿,
> application域中适合放一些关于服务器的配置信息等域服务其相关的数据;
>
> 


Application域

有效范围

当前web服务内,跨请求,跨会话

生命周期

创建 项目启动

使用 项目运行任何时间有效

销毁 项目关闭





@[TOC](jsp中的动态网页实现[)
# servlet实现动态页面
> Servlet同样也可以向浏览器动态响应HTML,但是需要大量的字符串拼接处理,在JAVA代码上大量拼接HTML字符串是非常繁琐耗时的一件事,它涉及到HTML本身的字符串处理,还涉及到css样式代码和文件,以及js脚本代码和文件,HTML中的各种外部引入路径等等,处理起来相当的麻烦

```js
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/10/19
  Time: 8:53
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
    <style>
      table{border: 1px solid green;width: 50%;margin: 0px auto;}
      table td{border: 1px solid blue;}
    </style>
  </head>
  <body>
<%
int h=Integer.parseInt(request.getParameter("height"));
int r= Integer.parseInt(request.getParameter("row"));
StringBuilder sbd= new StringBuilder();
sbd.append("<table>");
  for (int i = 0; i < h; i++) {
    sbd.append("<tr>");
    for (int j = 0; j < r; j++) {
      sbd.append("<td>");
      sbd.append(String.valueOf(i));
      sbd.append(String.valueOf(j));
      sbd.append("</td>");
    }
    sbd.append("</tr>");
  }

  sbd.append("</table>");
out.print(sbd);
%>
  </body>
</html>

```
# jsp实现动态网页

简单理解就是在html的基础上扩展 了,在jsp中可以插入java代码了;
java代码插入的形式----


<%  java代码 %>


```js
<%@ page import="java.util.Random" %><%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/10/19
  Time: 9:30
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>test</title>
</head>
<body>
<%
    Random random= new Random();
int num= random.nextInt(100);
int grade=num/10;
switch(grade){
    case 10:
        out.print("A+");
        break;
    case 9:
        out.print("A");
        break;
    case 8:
        out.print("B+");
        break;
    case 7:
        out.print("B");
        break;
    case 6:
        out.print("C");
        break;
    default:
        out.print("C-");
}
%>
<%out.print(num);%>


</body>
</html>

```

## 简写形式----->>

```js
<%@ page import="java.io.PrintWriter" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--
    ctrl +shift + /
    JSP中通过<%%>来穿插JAVA代码
    <%=变量/值%>将变量/值打印到页面上的标签显示的位置
--%>
    <%
        int score =(int)(Math.random()*101);
    %>
    分数:
    <%--
        <%
            //PrintWriter out = response.getWriter();
            out.print(score);
        %>
    --%>
    <%=score%>
    <br/>
    等级:
    <%
        int grade =score/10;
        switch (grade){
            case 10:
            case 9:
    %>
               <%="A"%>
    <%
                break;
            case 8:
    %>
                <%="B"%>
    <%
                break;
            case 7:
    %>
                <%="C"%>
    <%
                break;
            case 6:
    %>
               <%="D"%>
    <%
                break;
            default:
    %>
                <%="E"%>
    <%
        }
    %>
</body>
</html>

```

## jsp的运行原理是什么?

###  JSP的运行原理

JSP看似是HTML代码,看似是页面,但是事实上是**一种后台技术**,当我们第一发送请求一个JSP资源时,**JSP加载引擎会帮助我们将一个.JSP文件转换成一个.java文件**,相当于自动的给我们生成了一个Servlet并将页面上HTML代码编入到这个Servlet中,然后运行这个Servlet,将数据响应给浏览器.JSP的本质其实就是一个Servlet,.JSP中的HTML代码相当于是我们向**浏览器响应的HTML内容的模板**
以下是Jsp经过转换后生成的java文件;
![在这里插入图片描述](https://img-blog.csdnimg.cn/8d362ba969524dbf8e5ae70c807d2e1c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
打开看一下里面有些啥---->>>

```java
package org.apache.jsp;//包名
//test.jsp转换之后生成的java类名  test_jsp
//继承了HttpJspBase,实现了JspSourceDependent和JspSourceImports
public final class test_jsp extends org.apache.jasper.runtime.HttpJspBase
    implements org.apache.jasper.runtime.JspSourceDependent,
                 org.apache.jasper.runtime.JspSourceImports {
```
来看一下HttpJspBase里都有些什么

```java
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.jsp.HttpJspPage;

import org.apache.jasper.Constants;
import org.apache.jasper.compiler.Localizer;
// HttpJspBase是一个抽象类,继承了HttpServlet 实现了HttpJspPage
public abstract class HttpJspBase extends HttpServlet implements HttpJspPage {
```
所以JSP可以看作是一个servlet,这也就不难理解在JSP网页中可以直接使用request和response对象了;

### 简单分析一下转译过程----->>>


![在这里插入图片描述](https://img-blog.csdnimg.cn/8c8303d8d5b94cd6a4a53ea3eff7a346.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

### jsp的整个运行过程---->>>


web.xml中
![在这里插入图片描述](https://img-blog.csdnimg.cn/1716a441dea1435386f425656e4b7328.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)在tomcat初始化时就会加载jsp引擎,通过下面这个配置来完成jsp转换为java代码的
请求JSP是都会被JSP加载引擎所匹配,
![在这里插入图片描述](https://img-blog.csdnimg.cn/f6f2a396516449719020fb26aebbc245.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## JSP执行过程

JSP的执行过程大致可以分为两个时期:**转译时期**和**请求时期**

### 转译时期（Translation Time）：

JSP网页转译成Servlet,生成.java文件,然后进行编译生成.class字节码文件

正好在写代码时漏了一个;,于是访问时 报错5000
![在这里插入图片描述](https://img-blog.csdnimg.cn/f580b37332ad42fab31f1095755e5193.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


### 请求时期（Request Time）：

运行.class字节码文件,处理请求。

具体过程

1、客户端发出Request请求

2、JSP Container 将JSP转译成Servlet的源代码.java文件

3、将产生的Servlet源代码经过编译后.生成字节码.class文件

4、将.class字节码文件加载进入内存并执行,其实就是在运行一个Servlet

5、通过Response对象将数据响应给浏览器


> 当JSP网页在执行时，JSP Container 会做检查工作，如果发现JSP网页有更新修改时，JSP Container 才会再次编译JSP成 Servlet; 如果JSP没有更新时，就直接执行前面所产生的Servlet.**,也就是说,当我们在JSP上修改了代码时,不需要频繁的更新和重启项目,直接访问就可以完成更新



## JSP的性能问题

有人都会认为JSP的执行性能会和Servlet相差很多，其实执行性能上的差别只在第一次的执行。因为JSP在执行第一次后，会被编译成Servlet的类文件，即.class，当再重复调用执行时，就直接执行第一次所产生的Servlet,而不再重新把JSP编译成Servelt。除了第一次的编译会花较久的时间之外，之后JSP和同等功能的Servlet的执行速度就几乎相同了。

JSP慢的原因不仅仅是第一次请求需要进行转译和编译,而是因为JSP作为一种动态资源,本质上就是Servlet,它是需要运行代码才会生成资源,和HTML本身资源已经存在,直接返回,有着本质上的差异,另外,JSP转译之后,内部通过大量IO流形式发送页面内容,IO流本身是一种重量级操作,是比较消耗资源的


## jsp中的变量声明问题
jsp实际也就相当于一个servlet,servlet中最好不要声明全局变量,以防高并发时对数据的修改而产生不可描述的错误;


### 在jsp中声明 变量

![在这里插入图片描述](https://img-blog.csdnimg.cn/5aa19855bc1d43a7a68e40bf0b34e5d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)两种方式有什么不同呢?
看一下转译后的java文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/0f03acbc33c64094955864f94cb54ddf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/a13a43c46e914013b421000dc188e15b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 在jsp中注释

jsp中有html的结构 所以可有  css注释,js注释html注释
同时由于jsp中可以穿插Java代码,所以还可以有java注释
访问javatest.jsp,查看网页编码----
![在这里插入图片描述](https://img-blog.csdnimg.cn/718e43a281c3449a9659b60d823a744d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)会发现js,css,html注释都被发送给浏览器了,实际上是不应该被看到的;
什么原因呢?

![在这里插入图片描述](https://img-blog.csdnimg.cn/3a1c086f0220418faecb7ade849316a7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)查看java文件可以看到,js,css,html,java这四种注释都会进入java文件,然后前三种被java文件发送给浏览器了, 
jsp注释没有进入到java文件;

  1. 1JSP注释    仅仅存在于JSP页面     不会被编入java代码  不会响应给浏览器  
  2. html注释    不仅仅存在于JSP页面 编入java代码        会响应给浏览器  
  3. java注释   不仅仅存在于JSP页面 编入java代码    不会响应给浏览器   
  4. css js注释 不仅仅存在于JSP      编入java代码        会响应给浏览器 
      **推荐在JSP    页面使用JSP注释  尤其是在注释 html代码的时候**
      ![在这里插入图片描述](https://img-blog.csdnimg.cn/f8305f6bfe1b40e58919e81a19eab909.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


## jsp中的指令标签

指令标签是JSP页面上的一种特殊标签,JSP指令可以用来设置整个JSP页面相关的属性，如网页的编码方式,脚本语言,导包等等。

指令标签的语法

<%@ directive   attribute="value" %>

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/fad5efa6ce664bc9960d05980ce72e90.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
page标签---->>>>
常用的page标签--->>>

```js

<%--全局设置--%>
<%-- page contentType 相当于   response.setContentType("text/html;charset=UTF-8")--%>
<%@ page contentType="text/html;charset=UTF-8"  %>
<%--默认是java语言--%>
<%@page language="java" %>
<%--引入其他类--%>
<%@ page import="java.math.BigInteger"%>

<%--指名class文件编译编码格式,默认为content-type里一样--%>
<%@ page pageEncoding="UTF-8"%>
<%--指定错误页  当jsp中出现错误,则会跳往此页面--%>
<%@page errorPage="errortest.jsp" %>
<html>
<head>
    <title>Title</title>
    <script>
       /* 这是js注释*/
    </script>
    <style type="text/css">

        /*这是css注释*/
    </style>
</head>
<%--这是jsp注释--%>
<body>
<!--这是html注释-->
<%
    int i=100;
    int result =i/0;
    /*这是java注释*/%>
</body>
</html>
```

```js
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
   <%-- 这里设置为true表明这是一个错误页,在本页面中就可以--%>
    <%@page isErrorPage="true" %>
</head>
<body>
this is an error page!<br>
<%=exception.getMessage()%>
</body>
</html>

```
页面并没有发生跳转而是直接响应了错误页的信息.
![在这里插入图片描述](https://img-blog.csdnimg.cn/41fedb8608be4cf9b74a85a1dc6bb23a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 配置错误页----404

方式一:

```
<%@ page contentType="text/html;charset=UTF-8"  %>

<%--指名class文件编译编码格式,默认为content-type里一样--%>
<%@ page pageEncoding="UTF-8"%>
<%--指定错误页  当jsp中出现错误,则会跳往此页面--%>
<%@page errorPage="errortest.jsp" %>
<html>
<head>
    <title>Title</title>
    <script>
       /* 这是js注释*/
    </script>
    <style type="text/css">

        /*这是css注释*/
    </style>
</head>
<%--这是jsp注释--%>
<body>
<!--这是html注释-->

<%
    int i=100;
    /*手动造一个异常*/
    int result =i/0;

    /*这是java注释*/%>
</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a63fe53dcb464652bd307b9ed985a654.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
方式二
在web.xml中配置信息当发生对应的错误码时会跳到相应的页面上去
![在这里插入图片描述](https://img-blog.csdnimg.cn/1e6053e713934c85ae7424e330119117.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0953a5f6950a41f4afa1d1dc5de082a2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
***两种方式的区别,***
在页面文件中配置,只是当前页面出现了问题会跳转到对相应页面,
在web.xml中配置则是针对于全局项目中的错误都会跳转到该错误页中;


两种方式的优先级是页面配置中的优先级高于全局;
![在这里插入图片描述](https://img-blog.csdnimg.cn/addc1ad1734e4e42b53c0698f58f40a6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## jsp中的九大内置对象

```js

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--
jsp中有9大对象
--%>
<%
    /*四大域对象*/
    request.setAttribute("user", "gavin");
    session.setAttribute("pwd", 12356);
    application.setAttribute("userId", "1001");
    pageContext.setAttribute("userPhone", 10086);

    /*响应*/
    response.sendRedirect("https://www.baidu.com");
    /*输出流对象*/
    out.print("IO流");
%>
<%--当<%@page isErrorPage="true" %>设置为true时才可使用exception对象--%>
<%@page isErrorPage="true" %>

<%--异常对象--%>
<%exception.getCause();%>
<%--page对象---即当前jsp对象--%>
<% page.getClass();

/*config对象*/
config.getServletName();


%>

</body>
</html>

```
那这九大对象在那里体现呢?
打开转译之后的java代码---可以看到在jspservice方法中用到了这九大对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/5ffaa913a3594fd6bbeecadbe326640e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```java

@WebServlet(urlPatterns = "/PutDemo2.do")
public class PutDemo2 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //向servlet三大域中放信息
        Person per= new Person("张三",22,"男");
        Person per1= new Person("李四",23,"男");
        Person per2= new Person("王五",24,"男");
        Person per3= new Person("赵六",25,"男");

        req.setAttribute("request域",per);

        HttpSession session = req.getSession();
        session.setAttribute("session域",per1);

        ServletContext application = req.getServletContext();
        application.setAttribute("application域",per2);

        RequestDispatcher requestDispatcher = req.getRequestDispatcher("ReadDemo.jsp");
        requestDispatcher.forward(req,resp);

    }
}
```
jsp文件
```js
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <%@page import="PutDemo.Person" %>
</head>
<body>
<%=request.getAttribute("request域")%>
<br>
<%=session.getAttribute("session域")%>
<br>
<%=application.getAttribute("application域")%>
<%=page.getClass()%>
<%pageContext.setAttribute("user",new Person("钱八",19,"男"));%>
<br>
<%=pageContext.getAttribute("user")%>
</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/62c86c9721924920994215a9b3765178.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# JSTL标签库的使用

## include标签

JSP可以通过**include指令来包含其他文件**。被包含的文件可以是**JSP文件、HTML文件或文本文件**。包含的文件就好像是该JSP文件的一部分，会被同时编译执行。除了include指令标签可以实现引入以外,使用 **jsp:include 也可以实现引入**

![在这里插入图片描述](https://img-blog.csdnimg.cn/3579c35fc6c442a9aaafbe66563d2069.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
静态引入----

![在这里插入图片描述](https://img-blog.csdnimg.cn/3f5089a421f94f69b95e3769b8c4dbf0.png)

> **静态引入: @include   被引入的网页和当前页生成代码后形成了一个java文件**

![在这里插入图片描述](https://img-blog.csdnimg.cn/58cbbb6170f240dc8d3f72bf8ba054c6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/8169a2d7cbc646fdb2554de6d0bb5fee.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **动态引入: jsp:include 被引入的JSP页面会生成独立的java代码**
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/65ffe551c8674cc297022a7ea9d6d35a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5d6b8075a744600a7e48e157ce8095c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## taglib 标签

作用是引入jstl标签库,用于简化代码量
JSTL（Java server pages standarded tag library，即JSP标准标签库）是由JCP（Java community Proces）所制定的标准规范，它主要提供给Java Web开发人员一个标准通用的标签库，并由Apache的Jakarta小组来维护。

### 使用JSTL的好处:

开发人员可以利用JSTL和EL来开发Web程序，取代传统直接在页面上嵌入Java程序的做法，以提高程序的阅读性、维护性和方便性。在jstl中, 提供了多套标签库, 用于方便在jsp中完成或简化相关操作.

### JSTL标签库的组成部分

>核心标签库: core, 简称c

>格式化标签库: format, 简称fmt

>函数标签库: function, 简称fn


### JSTL的使用前提

1需要在项目中导入jstl-1.2.jar **,jstl在后台由java代码编写,** jsp页面中通过标签进行使用, 使用标签时, 会自动调用后台的java方法,

2标签和方法之间的映射关系在对应的tld文件中描述. 需要在页面中通过**taglib指令引入对应的标签库, uri可以在对应的tld文件中找到**

 书写格式----

<%@   taglib uri="标签库的定位" prefix="前缀(简称)" %>

## c:set c:remove c:out

 代码展示--->>>
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%--使用核心标签库--%><%--compat 包--%>
<%@taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
<%
    request.setAttribute("msg","requestMessage");
%>
c:set
scope指定数据的域 page request session application
value 实际数据
</hr>
<br/>
<c:set scope="page" var ="msg" value="pageMessage"></c:set><br/>
<c:set scope="request" var ="msg" value="requestMessage"></c:set><br/>
<c:set scope="session" var ="msg" value="sessionMessage"></c:set><br/>
<c:set scope="application" var ="msg" value="applicationMessage"></c:set><br/>
<hr/>
四大域中的信息<br/>
page: ${pageScope.msg}<br/>
request:${requestScope.msg}<br/>
session:${sessionScope.msg}<br/>
application:${applicationScope.msg}<br/>
<br/>

<c:set scope="page" var ="msg" value="pageMessage"></c:set><br/>
<c:set scope="request" var ="msg" value="requestMessage"></c:set><br/>
<c:set scope="session" var ="msg" value="sessionMessage"></c:set><br/>
<c:set scope="application" var ="msg" value="applicationMessage"></c:set><br/>
<hr/>

<%--移除指定域中的值--%>
<c:remove var="msg" scope="page"></c:remove><br/>
<c:remove var="msg" scope="request"></c:remove><br/>
<c:remove var="msg" scope="session"></c:remove><br/>
<c:remove var="msg" scope="application"></c:remove><br/>

移除后四大域中的信息
page: ${pageScope.msg}<br/>
request:${requestScope.msg}<br/>
session:${sessionScope.msg}<br/>
application:${applicationScope.msg}<br/>

通过c:out标签获取域中的值
取出值是还是得依靠EL表达式,如果表达式为null,则就用默认值展示
<c:set scope="page" var ="msg" value="pageMessage"></c:set><br/>
<c:out value="${pageScope.msg}" default="page msg not found"/>
<c:out value="${requestScope.msg}" default="request msg not found"/>
<c:out value="${sessionScope.msg}" default="session msg not found"/>
<c:out value="${applicationScope.msg}" default="applaication msg not found"/>
</body>
</html>

```

## c:if c:choose

```js
<%@ page import="java.util.Random" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
<html>
<head>
    <title>Title</title>
</head>
<hr>

<% int num = new Random().nextInt(101);

    pageContext.setAttribute("num", num);
%>

<%--<c:if test="用于放判断条件(EL表达式)" var="数据名" scope="指定域"></c:if>--%>

<%--<c:if 结构中每一个条件都要判断--%>

${num}:</br>
等级--
<c:if test="${num ge 90}" scope="page" var="f1">A</c:if>
<c:if test="${num ge 80 &&num lt 90} " scope="page" var="f2">B</c:if>
<c:if test="${num ge 70 && num lt 80}">C</c:if>
<c:if test="${num lt 70}">D</c:if>

${f1}</br>
${pageScope.f2}</br>
<c:if test="${num ge 60}" scope="page" var="flag">及格</c:if>
<c:if test="${!pageScope.flag}">不及格</c:if>
</hr></br>


<%--<c:choose>结构当判断第一个成立时,以后的就不会判断,以此类推--%>

<c:choose>
    <c:when test="${num ge 90}">A</c:when>
    <c:when test="${num ge 80}">B</c:when>
    <c:when test="${num ge 70}">C</c:when>
    <c:when test="${num ge 60}">D</c:when>
    <%-- <c:when test="${num lt 60}">E</c:when>--%>
    <%--其他情况--%>
    <c:otherwise>E--</c:otherwise>
</c:choose>
</body>
</html>

```

## c:foreach打印九九乘法表

```js
打印九九乘法表<br>
<c:forEach var="i"  begin="1" end="9" step="1">
    <c:forEach var="j"  begin="1" end="${i}" step="1">
        <c:set var="x" value="${i*j}" scope="page"></c:set>
        ${j} * ${i}=<c:if test="${x lt 10}" var="ji" scope="page"> 0${i*j}</c:if>
        <c:if test="${!ji}">${i*j}</c:if>
&emsp;
    </c:forEach>
</br>

</c:forEach>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/72a9499bdabe4010b9b508afd17f7fe8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```js
<%@ page contentType="text/html;charset=UTF-8" %>
<%@taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
<html>
<head>
    <title>欢迎使用图书管理系统</title>
    <style type="text/css">
        table {
            border: 4px solid black;
            width:70%;
            margin: 0px auto;
            mso-cellspacing: 0px;
        }
        td,th {
            border: 2px solid black;
        }
    </style>
</head>
<body>
<div>
    <table cellpadding="0px" cellspacing="0px">
     <tr>
            <th>BookId</th>
            <th>BookName</th>
            <th>BookPublish</th>
            <th>BookPrice</th>
            <th>BookKind</th>
     </tr>
        <%--<c:forEach 中items 表示要遍历的结合或者数组---要从域中取出 var 表示 每次迭代的变量名--%>
        <c:forEach items="${BookInfo}" var="book">
        <tr>
<%--这里跟添加到list中的属性字段一致--%>
             <td>${book.bookId} </td>
             <td>${book.bookName}</td>
             <td>${book.bookPublish}</td>
             <td>${book.bookPrice}</td>
             <td>${book.bookKind}</td>
        </tr>
</c:forEach>
    </table>
</div>
</body>
</html>

```
# EL表达式的应用

```java
package com.gavin.libary;
import com.gavin.libary.BookPojo.UserPojo;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
@WebServlet("/LibraryTest.do")
public class LibraryTest  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         req.setAttribute("userInfo", new UserPojo("userName","pwd"));
        HttpSession session = req.getSession();
session.setAttribute("user",new UserPojo("Gavin","123456"));
        ServletContext application = req.getServletContext();
application.setAttribute("Mary","1234567");
//将请求转发到一个jsp;
        req.getRequestDispatcher("Mytest.jsp").forward(req,resp);
    }
}

```

## javaweb与数据库

```java
<%@ page import="com.gavin.libary.BookPojo.UserPojo" %><%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/10/21
  Time: 10:38
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%
 pageContext.setAttribute("name",new UserPojo("王五","123456"));
%>
<%--获得用户名--%>
<%--requestScope.userInfo相当于从request域中取出userPojo对象,
然后通过get方法得到userName,userName要和类中属性名一致--%>
request域:${requestScope.userInfo.userName}<br>
<hr>
session域:${sessionScope.user.userName}<br>
<hr>
application域:${applicationScope.Mary}<br>
<hr>
page域:${pageScope.name}
</body>
</html>
```


```
package com.gavin.libary.BookPojoImp;

import com.gavin.libary.BookPojo.BookPojo;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class BookImp implements SearchBook {
    final static String NAME = "gavin";
    final static String PASSWORD = "955945";
    final static String MyDRIVER = "com.mysql.cj.jdbc.Driver";
    final static String URL = "jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&cachePrepStmts=true&reWriteBatchedStatements=true&useServerPrepStmts=true";
    final static String SQL = "select * from bookstore;";
    static List<BookPojo> list = new ArrayList<>();
    Connection connection =null;
    PreparedStatement preparedStatement=null;
    ResultSet resultSet =null;
    @Override
    public List<BookPojo> findAll()  {
        try {
            Class.forName(MyDRIVER);
            connection=   DriverManager.getConnection(URL, NAME, PASSWORD);
            preparedStatement= connection.prepareStatement(SQL);
            resultSet=   preparedStatement.executeQuery();
            if (null != resultSet) {
                while (resultSet.next()) {
                 Integer bookId = resultSet.getInt("BookId");
                    String bookName = resultSet.getString("BookName");
                    String bookPublish = resultSet.getString("BookPublish");
                  Double bookPrice = resultSet.getDouble("BookPrice");
                    String bookKind = resultSet.getString("BookKind");
                    BookPojo bookPojo = new BookPojo(bookId, bookName, bookPublish, bookPrice, bookKind);
                    list.add(bookPojo);
                }

            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                if(null!=resultSet)
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if(null!=preparedStatement)
                    preparedStatement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if(null!=connection)
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return list;
    }
}

```



```

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@page import="com.gavin.libary.BookPojo.BookPojo" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>

<html>
<head>
    <title>欢迎使用图书管理系统</title>
    <style type="text/css">
        table {
            border: 4px solid black;
            width:70%;

            margin: 0px auto;
            mso-cellspacing: 0px;
        }

        td {
            border: 2px solid black;

        }

        th {
            border: 2px solid black;

        }

    </style>
</head>
<body>

<div>
    <table cellpadding="0px" cellspacing="0px">

     <tr>
            <th>BookId</th>
            <th>BookName</th>
            <th>BookPublish</th>
            <th>BookPrice</th>
            <th>BookKind</th>
        </tr>
        <%
            List<BookPojo> bookInfos = (ArrayList) request.getAttribute("BookInfo");
            for (BookPojo bookPojo : bookInfos) {
                pageContext.setAttribute("book", bookPojo);
        %>

        <tr>

<%--这里跟添加到list中的属性字段一致--%>

             <td>${book.bookId} </td>
             <td>${book.bookName}</td>
             <td>${book.bookPublish}</td>
             <td>${book.bookPrice}</td>
             <td>${book.bookKind}</td>
        </tr>
        <%
            }
        %>
    </table>
</div>
</body>
</html>
```

## EL表达式获取请求参数

```java
@WebServlet("/Test.do")
public class Test extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(req.getParameter("uName"));
        System.out.println(Arrays.toString(req.getParameterValues("word")));
        req.getRequestDispatcher("test.jsp").forward(req,resp);
    }
}
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${param.uName}
<hr>
${paramValues.word[0]}
<hr>
${paramValues.word[1]}
</body>
</html>

```

## EL表达式的运算符
在EL表达式中, 支持运算符的使用

算数运算符: + - * / %

比较运算符: 

==  eq equals

" >"     gt greater then

<     lt   lower then

">= " ge  greater then or equals

<=  le   lower then or equals

!=   ne   not equals

逻辑运算符: || or    && and 

三目运算符: ${条件 ?表达式1 : 表达式2}

判空运算符: empty


## 算数运算符 +-*/

```
算数运算符 +-*/
<hr/>
${10+10}
${10+"10"}
<%--如果一个字符串不能转换为数字则出现异常--%>
<%--${"10a"+10}--%>
<%--两个字符串相加,如果都不能转换成数字,也会报异常--%>
<%--${"10q"+"20q"}--%>
<%--两个字符串相加,如果都能转换成数字,则是求和--%>
${"10"+"10"}
<%--除以0得到的结果为infinity而不是出现异常--%>
${10/0}
</body>
</html>

```

## 关系运算符和比较运算符

```
关系运算符和比较运算符
<hr><br/>
"10" eq 10:${"10" eq 10}<br/>  转换为数字再比较

10==10: ${10 eq 10}<br/>
8>9:${8 gt 9}<br/>
10<19:${10 lt 19}<br/>

1000>=999:${1000 ge 999}<br/>

888<=777:${888 le 777}<br/>
"你好"!="Hello":${"你好" ne "Hello"}<br/>
```




## 条件运算符/三目运算符

```


条件运算符/三目运算符
<hr>
true?1:2-------${true?1:2}<br/>
10 eq 9? 10:9-------- ${10 eq 9? 10:9}<br/>

<%
    pageContext.setAttribute("AAA",null);
    pageContext.setAttribute("BBB","null");
    pageContext.setAttribute("CCC","");
    int array[]=new int[0];
    List list= new LinkedList();
    pageContext.setAttribute("DDD",list);
    pageContext.setAttribute("EEE",array);

%>

AAA   value为null: ${empty AAA}<br/> null判断为空
BBB   value为字符串"null":${empty BBB}<br/>
CCC  value为空字符串: ${empty CCC}<br/>字符串长度为0 判断为空
DDD  value为空集合: ${empty DDD}<br/>集合长度为0判断为空
EEE  value为空数组: ${empty EEE}<br/>数组长度为0认为不是空

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/8c01352d9ebd42bdb324f576a42529d3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


# JavaWeb中的过滤器Filter

> Filter是Servlet技术中最实用的技术，通过Filter过滤技术，对web服务器上所有的web资源进行管理：例如Jsp,
> Servlet, 静态图片文件或静态 html 文件等进行拦截/放行，从而能够实现一些特殊的功能。
比如实现URL级别的权限访问控制、过滤敏感词汇、压缩响应信息等一些高级功能 处理编码。

它主要用于**对用户请求进行预处理**，也可以**对HttpServletResponse进行后处理**。

## 如何使用过滤器
使用Filter的完整流程：

  1. Filter对用户请求进行预处理;
  2. 将请求交给Servlet进行处理并生成响应;
  3. 最后Filter再对服务器响应进行后处理。

具体描述----->>>>

> 1,在HttpServletRequest到达 Servlet 之前，拦截客户的HttpServletRequest。根据需要检查HttpServletRequest，也可以修改HttpServletRequest 头和数据。
>

> 2,在HttpServletResponse到达客户端之前，拦截HttpServletResponse根据需要检查HttpServletResponse，也可以修改HttpServletResponse头和数据。
>


> 3Filter接口中有一个doFilter方法，当开发人员编写好Filter，并配置对哪个web资源进行拦截后，Web服务器每次在调用web资源的service方法之前，都会先调用一下filter的doFilter方法**，doFilter方法中有一个filterChain对象,用于继续传递给下一个filter,**在传递之前我们可以定义过滤请求的功能,在传递之后,我们可以定义过滤响应的功能


简单的框架实现---->>>
首先定义一个servlet

```java
public class MyServlet1 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        resp.getWriter().println("MyServlet1--invoke");
    }
}

```
然后定义一个过滤器----->>>

自定义过滤器要实现servlet包下的Filter接口,接口中有有三个方法-----

初始化方法----init(FilterConfig filterConfig)

过滤方法---- doFilter(ServletRequest **servletRequest,** ServletResponse **servletResponse**, FilterChain **filterChain**) 

销毁过滤器方法----destroy()

其中和新方法是  doFilter方法来对请求和访问进行过滤和处理的;

```java
import javax.servlet.*;
import java.io.IOException;

public class MyFilter implements Filter {
//初始化方法
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("初始化完成");
    }
//过滤方法
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("请求已过滤");
    }
//销毁方法
    @Override
    public void destroy() {

    }
}
```

准备好这些后,还要指定过滤器访问哪个servlet时使用该过滤器;

配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>MyServlet1</servlet-name>
        <servlet-class>com.gavin.myfiflter.MyServlet1</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>MyServlet1</servlet-name>
        <url-pattern>/MyServlet1.do</url-pattern>
    </servlet-mapping>
    <filter>
        <filter-name>MyFilter</filter-name>
        <filter-class>com.gavin.fiflter.MyFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>MyFilter</filter-name>
        <!--对哪个资源请求和响应进行过滤-->
        <url-pattern>/MyServlet1.do</url-pattern>

    </filter-mapping>
</web-app>
```

框架搭建完毕后,运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/f574a7642eb540a8aa2ecbd3bc1b78f5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)但是浏览器中闭关没有输出任何信息,是因为我们只是定义了过滤,对过滤之后没有进行任何处理,所以相当于给全部过滤掉了,那这个时候需要添加一些代码对请求/响应进行过滤处理,这时候服务器才能对请求做出真正的响应;
![在这里插入图片描述](https://img-blog.csdnimg.cn/77a929e875a64f269c4febcc240c8d86.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 完善过滤器代码

```java
import javax.servlet.*;
import java.io.IOException;

public class MyFilter implements Filter {
//初始化方法
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("初始化完成");
    }
//过滤方法
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("请求已过滤");
        //放行请求,继续让过滤链中的其他过滤器过滤
        filterChain.doFilter(servletRequest,servletResponse);
        System.out.println("对响应进行过滤");
        servletResponse.getWriter().print("过滤之后添加的响应内容");

    }
//销毁方法
    @Override
    public void destroy() {
        System.out.println("已销毁");
    }
}

```
再次访问之后-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/470f8a7c4a7043d7989412d9b09c08bd.png)
同时一个而过滤器可以过滤多个servlet对象

![在这里插入图片描述](https://img-blog.csdnimg.cn/77c179c883c44305a8f6eafd3964e461.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 同servlet对象一样,Filter对象的创建也是交给web服务器完成的,在web服务器创建和使用及最后销毁filter时,会调用filter对应的方法

构造方法:

**实例化一个Filter对象的方法**

初始化方法:

public void init(FilterConfig filterConfig);

和我们编写的Servlet程序一样，Filter的创建和销毁由WEB服务器负责。 web 应用程序启动时，web 服务器将创建Filter 的实例对象，并调用其init方法，读取web.xml配置，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作（filter对象只会创建一次，init方法也只会执行一次）。

拦截请求方法

public void doFilter

这个方法完成实际的过滤操作。当客户请求访问与过滤器关联的URL的时候，Servlet过滤器将先执行doFilter方法。FilterChain参数用于访问后续过滤器。

销毁方法:

public void destroy();

Filter对象创建后会驻留在内存，当web应用移除或服务器停止时才销毁。在Web容器卸载 Filter 对象之前被调用。该方法在Filter的生命周期中仅执行一次。在这个方法中，可以释放过滤器使用的资源。


简要描述:
1   WEB 容器启动时,会对Filter进行构造并初始化 一次
2   每次请求目标资源时,都会执行doFilter的方法
3   WEB容器关闭是,会销毁Filter对象



## 过滤器链的使用

搭建架构-----
准备两个过滤器,一个servlet 
![](..\pic\mysql_pic\50.png)

过滤器链原理分析---->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/4329b75520c041709f06080e02e6a88d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
web服务器根据Filter在web.xml文件中的注册顺序，决定先调用哪个Filter，当第一个Filter的doFilter方法被调用时，web服务器会创建一个代表Filter链的FilterChain对象传递给该方法。在doFilter方法中，开发人员如果调用了FilterChain对象的doFilter方法，则web服务器会检查FilterChain对象中是否还有filter，如果有，则调用第2个filter，如果没有，则调用目标资源。

使用过滤器链的好处是我们可以将不同的过滤功能分散到多个过滤器中,分工明确,避免一个过滤器做太多的业务处理,降低了代码的耦合度,这体现了单一职责的设计原则,应用了责任链的代码设计模式.

**决定过滤器的执行顺序是由filter-mapping标签决定**

## 过滤器参数初始化

过滤器的配置跟servlet类似,可以在web.xml中配置一些初始化参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/5aaeb5f0d47c43eeb411220b362df6e0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)那这些参数该如何取出来呢?
我们回到Filter的生命周期--
初始化的时候我们可以覆写一下 init()方法
public void init(FilterConfig filterConfig) throws ServletException

在这个方法中有一个参数FilterConfig filterConfig),通过这个参数来取出数据!
![在这里插入图片描述](https://img-blog.csdnimg.cn/8127aea3694a4e29885e366662f5abc4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/93587e11771c40c399b42c4de0a6ba30.png)通过配置文件比较麻烦的话,我们还可以通过注解模式来开发过滤器

## 注解模式开发过滤器

```java
/*@WebServlet(name= "MyServlet3")*/
@WebFilter(urlPatterns = "/MyServlet3.do" ,initParams ={@WebInitParam(name="UserInfo",value = "张三"),@WebInitParam(name="Charset",value = "UTF-8") })
public class NewFilterDemo implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println(filterConfig.getFilterName());
        System.out.println(filterConfig.getInitParameter("Charset"));
        System.out.println(filterConfig.getInitParameter("UserInfo"));

        filterConfig.getServletContext().setAttribute("userN","Gavin");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
filterChain.doFilter(servletRequest,servletResponse);

    }

    @Override
    public void destroy() {

    }
}

```
注解两种方式---->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/221820ed56ad4d2fada43ee56ebcd9da.png)
注解方式下过滤器的执行顺序------
在配置文件模式下配置的过滤器执行顺序跟过滤器的放置顺序有关系,那么在注解模式下,过滤器的执行顺序是怎样的呢?

做一个测试----->>>>
准备两个过滤器-----

Filter0_Demo
和Filter1_Demo,都对MyServlet3.do进行过滤-----结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/103ac2613a76485791629caa0b167834.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)下面修改一下Filter0_Demo为Filter8_Demo
![在这里插入图片描述](https://img-blog.csdnimg.cn/89843aa4e1424d0d925c7a093182acb0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
再次添加一个过滤器----Filter6_Demo
![在这里插入图片描述](https://img-blog.csdnimg.cn/9a879eefaa794f43bf248f782a09d3a4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以看到确实是按照名字进行排序的,所以在命名过滤器时要注意名字的命名规范,一般原则是Filter数字_过滤器描述



## 过滤器处理post乱码
先看一下 form表单  通过get的方式请求,服务器获得的数据

```html
<%--
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <form action="PostDemo.do" method="get">
    用户名:<input type="text" name="user" placeholder="请输入用户名">

    密&emsp;码:<input type="password" name="pwd" placeholder="请输入密码">
    <input type="submit" value="提交">
  </form>
  </body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a177fd72b9314ae4a82970d328b52c3f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)后台得到的数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/dbf7d95313224a3a8158c11ca0c2d3c3.png)换成post方式得到的数据---->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/a3954b2b9d8d49cab90f6c9aad1f7aab.png)怎么办?在servlet中设置解码方式----

![在这里插入图片描述](https://img-blog.csdnimg.cn/ca477825608b48669894c6af405267e2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)但是如果servlet很多的话挨个写?........
这时候过滤器就可以起作用了>
可以使得买次请求时都通过过滤器设置编码格式为UTF-8

![在这里插入图片描述](https://img-blog.csdnimg.cn/6d7eead2446b4ac49f562575d038763e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)最后实现的结果跟在每个Servlet种单独设置是一样的----->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/237ea73b976342dca64132a2dfefa9c5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
还有一个间接的方式----在定义过滤器时可以将charset 放到 初始化时的参数中,通过读取参数来完成charset的设置;


具体操作如下:
![在这里插入图片描述](https://img-blog.csdnimg.cn/257b518338e04404b77b14e2de7f70ec.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d108b72c40db4a5390eaeb80c49f3d45.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
方式二是通过注解的方式完成----->

![在这里插入图片描述](https://img-blog.csdnimg.cn/5126758bdcfa450196549b481bf3ec84.png)




# JavaWeb中的监听器



> 学习javaweb时我们会用到很多种域对象,
> request域,session域,以及application域这三种时servlet中的三大域对象;
> 每一个域中都可以存放一些信息,但是是有针对性的,并不是随便放置的,这个暂且不提;

今天主要学习的是监听器------->
什么是监听器?

类似于前端的事件绑定,java中的监听器用于监听web应用中某些对象、信息的创建、销毁、增加，修改，删除等动作的发生，然后作出相应的响应处理。当范围对象的状态发生变化的时候，服务器自动调用监听器对象中的方法。常用**于统计在线人数和在线用户，系统加载时进行信息初始化，统计网站的访问量**等等。


## 监听器的分类

**按监听的对象划分**

a.ServletContext对象监听器

b.HttpSession对象监听器

c.ServletRequest对象监听器

**按监听的事件划分**

a.对象自身的创建和销毁的监听器

b.对象中属性的创建和消除的监听器

c.session中的某个对象的状态变化的监听器

java中一共给我们提供了八个监听器接口,分别用于监听三个域对象,每个监听器都有专门监听的事件


### Request域监听器

监听器并不是一个有实体方法的类,而是一个接口,接口中的方法需要我们去实现;

创建一个Request域监听器----->
实现requestInitialized和requestDestroyed方法

```java
import javax.servlet.ServletRequest;
import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;

public class MyListener_demo2  implements ServletRequestListener {
  
//监听request对象创建
    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        ServletRequest servletRequest = sre.getServletRequest();//可以得到请求对象;
        System.out.println(servletRequest.hashCode());
        System.out.println(servletRequest);
//        通过请求对象做出一些处理
        System.out.println(servletRequest.getAttribute("user"));//取出域中的信息
        
    }
    //监听request对象销毁
    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        ServletRequest servletRequest = sre.getServletRequest();
        System.out.println(servletRequest.hashCode()+"请求对象销毁了");
        System.out.println(servletRequest+"请求对象销毁了");
    }
}
```

创建好之后还需要将监听器部署到项目中;

配置监听器---

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

  <!--  //配置监听器-->
   <listener>
        <listener-class>MyListener.MyListener_demo2</listener-class>
    </listener>
    
</web-app>
```

创建servlet

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
@WebServlet("/MyServlet_demo01.do")
public class MyServlet_demo01  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servlet1----invoked");

        req.setAttribute("user","张三");
        System.out.println("请求对象1"+req.hashCode());
    }
}

```

部署项目后运行

![在这里插入图片描述](https://img-blog.csdnimg.cn/6fd637fb9d2e44e5afacd945b0ca49d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

运行后发现该监听器只能监听request对象的创建和销毁,要想监听request域中内容得增删改,那么该如何操作呢?


创建一个Request域属性监听器----->

```java

public class MyListener_demo2  implements ServletRequestAttributeListener {


    //监听request中属性的添加
    @Override
    public void attributeAdded(ServletRequestAttributeEvent srae) {
        String name = srae.getName();
        Object value = srae.getValue();
        //获得request对象;
        ServletRequest servletRequest = srae.getServletRequest();
        System.out.println("请求行中的user="+servletRequest.getParameter("user"));
        System.out.println("请求域中添加了"+name+"="+value);
    }
    //监听request中属性的移除
    @Override
    public void attributeRemoved(ServletRequestAttributeEvent srae) {
        String name = srae.getName();
        Object value = srae.getValue();

        System.out.println("请求域中删除了"+name+"="+value);
    }
    //监听request中属性的修改
    @Override
    public void attributeReplaced(ServletRequestAttributeEvent srae) {
        String name = srae.getName();
        Object value = srae.getValue();

        System.out.println("请求域中修改了"+name+"="+value);
    }
}

```

创建servlet

```java
@WebServlet("/MyServlet_demo03.do")
public class MyServlet_demo03 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

      //添加Attribute
       req.setAttribute("user","张三");
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //修改Attribute
       req.setAttribute("user","李四");
//添加新的
        req.setAttribute("password","123456");
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
//        移除
        req.removeAttribute("password");
    }
}

```
运行------>
![在这里插入图片描述](https://img-blog.csdnimg.cn/d141a3140fec49bba21819e5a234349d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
### session域监听器

跟request监听器搭建类似,实现接口然后调用方法等;
监听sessio的创建与销毁---HttpSessionListener
监听session中的属性增删改---HttpSessionAttributeListener
```java

import javax.servlet.http.*;

public class MySessionListener_demo01  implements HttpSessionListener, HttpSessionAttributeListener {
    //监听session对象的创建

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        HttpSession session = se.getSession();
        System.out.println("哈希值为--"+session.hashCode()+"session对象创建了");

    }
    //监听session对象的销毁
    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        HttpSession session = se.getSession();
        System.out.println("哈希值为--"+session.hashCode()+"session对象销毁了");
    }

//监听session中的属性添加
    @Override
    public void attributeAdded(HttpSessionBindingEvent se) {
        HttpSession session = se.getSession();
        System.out.println("哈希值为--"+session.hashCode()+"对象中增加了属性");
        System.out.println(se.getName()+"="+se.getValue());
    }
    //监听session中的属性移除
    @Override
    public void attributeRemoved(HttpSessionBindingEvent se) {
        HttpSession session = se.getSession();
        System.out.println("哈希值为--"+session.hashCode()+"对象中移除了属性");
        System.out.println(se.getName()+"="+se.getValue());
    }
    //监听session中的属性修改
    @Override
    public void attributeReplaced(HttpSessionBindingEvent se) {
        HttpSession session = se.getSession();
        System.out.println("哈希值为--"+session.hashCode()+"对象中修改了属性");
        System.out.println(se.getName()+"="+se.getValue());
    }
}

```


可以绑定监听器与某一个session相关联
HttpSessionBindingListener


创建一个监听器----->>>实现
HttpSessionBindingListener
HttpSessionActivationListener
```java

import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSessionActivationListener;
import javax.servlet.http.HttpSessionBindingEvent;
import javax.servlet.http.HttpSessionBindingListener;
import javax.servlet.http.HttpSessionEvent;
@WebListener
public class MySessionBindListener_demo01 implements HttpSessionBindingListener, HttpSessionActivationListener {

//绑定事件
    @Override
    public void valueBound(HttpSessionBindingEvent event) {
        System.out.println(event.getName());
        System.out.println(event.getValue());
        System.out.println("该监听器与"+event.getSession().hashCode()+"绑定了");

    }
//解绑事件
    @Override
    public void valueUnbound(HttpSessionBindingEvent event) {

        System.out.println(event.getName());
        System.out.println(event.getValue());
        System.out.println("该监听器与"+event.getSession().hashCode()+"解绑了");

    }

//session钝化
    @Override
    public void sessionWillPassivate(HttpSessionEvent se) {
        System.out.println(se.getSession().hashCode()+"session钝化");
    }
    //session活化
    @Override
    public void sessionDidActivate(HttpSessionEvent se) {
        System.out.println(se.getSession().hashCode()+"session活化");
    }
}

```

#### session活化与session钝化

session是有自己生命周期的,当我们创建一个session时,在浏览器保存一个cookie,下次浏览器访问时就可以跳过一些环节,但是当session 失效时后者session被清除时(服务器重启,那么浏览器中的cookie对象也就会失效,服务器回重新生成一个session),如果服务器不想这样的化,就需要在服务器重启时  钝化session------简单的说就是将session对象序列化后写入到磁盘,---这就时session的钝化,当服务器重启时读取这个session那么服务器就会检查到浏览器发过来的cookie,这叫session的活化;


### application域监听器
监听器
```java
package MyListener;
import javax.servlet.ServletContextAttributeEvent;
import javax.servlet.ServletContextAttributeListener;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class MyApplicationListener_demo1 implements ServletContextListener, ServletContextAttributeListener {

    @Override
    public void attributeAdded(ServletContextAttributeEvent scae) {
        System.out.println("被添加的信息---");
        System.out.println(scae.getName()+"---"+scae.getValue());
    }

    @Override
    public void attributeRemoved(ServletContextAttributeEvent scae) {
        System.out.println("被移除的信息---");
        System.out.println(scae.getName()+"---"+scae.getValue());
    }

    @Override
    public void attributeReplaced(ServletContextAttributeEvent scae) {
        System.out.println("被修改的信息---");
        System.out.println(scae.getName()+"---"+scae.getValue());
    }

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println(sce.getServletContext().hashCode()+"被实例化了");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println(sce.getServletContext().hashCode()+"被销毁了");
    }
}

```
servlet

```java
@WebServlet("/MyServlet_demo06.do")
public class MyServlet_demo06 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("servlet6----invoked");
        ServletContext servletContext = req.getServletContext();
        servletContext.setAttribute("username","王二麻子");
        servletContext.setAttribute("password","WangEr");
        servletContext.setAttribute("username","李代张冠");
        servletContext.removeAttribute("username");
    }
}
```


## 用request监听器记录请求日志

```java
package MyListenerdemo;
import org.apache.log4j.Logger;
import javax.servlet.ServletRequest;
import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;
import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpServletRequest;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.PrintWriter;
import java.text.SimpleDateFormat;
import java.util.Date;
@WebListener
public class MyListenerDemo01  implements ServletRequestListener {
    @Override
    public void requestDestroyed(ServletRequestEvent sre) {

    }

    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        Logger logger = Logger.getLogger(MyListenerDemo01.class);
        ServletRequest servletRequest = sre.getServletRequest();
        HttpServletRequest req=(HttpServletRequest)servletRequest;
        String remoteHost = req.getRemoteHost();
        System.out.println(remoteHost);
        String requestURI = req.getRequestURI();
        String requestUrl = req.getRequestURL().append(requestURI).toString();
        System.out.println(requestUrl);

        SimpleDateFormat sdf= new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String date = sdf.format(new Date());
        System.out.println(date);

//用打印流记录,但是会遇到阻塞等,比较繁琐;
        try {
            PrintWriter pw= new PrintWriter(new FileOutputStream("d:/gavin.txt"),true);
            pw.println(remoteHost+" "+requestUrl+" "+date);
            pw.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }finally {
            logger.info(remoteHost+" "+requestUrl+" "+date);
        }
    }
}

```

## 用session监听器记录在线人数


```java
package MyListenerdemo;

import javax.servlet.ServletContext;
import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;
@WebListener
public class MySessionListener_01 implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent se) {

//        拿到session
        HttpSession session = se.getSession();
        ServletContext application = session.getServletContext();
        //查看session中是否有值
        Object attribute =  application.getAttribute("count");
//        如果count为null,说明是第一次访问
        if(attribute==null){

            application.setAttribute("count",1);
        }else{//不是第一次访问
            Integer count=(Integer)attribute;
            application.setAttribute("count",++count);
        }
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        //        拿到session
        HttpSession session = se.getSession();
        ServletContext application = session.getServletContext();

        Integer  count=(Integer)application.getAttribute("count");
        application.setAttribute("count",--count);

    }
}

```

## session的钝化与活化监听器实现

这种方式用于用户七天之内免登录的路况!

框架搭建!!
准备一个jsp页面------->>>

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
当前在线人数:${applicationScope.count}
  <form method="post" action="myServlet01.do">
    账号:<input type="text" name="username" placeholder="请输入账号">

    密码:<input type="text" name="userpwd" placeholder="请输入密码">
    <input type="submit" value="登录">
  </form>
  </body>
</html>

```
准备一个user实体类,

```java
package MyPojo;

import java.io.Serializable;
import java.util.ArrayList;

public class User implements Serializable {
    private static final long serialVersionUID = 88591L;
    private String username;
    private String userpwd;

    public User() {
    }

    public User(String username, String userpwd) {
        this.username = username;
        this.userpwd = userpwd;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getUserpwd() {
        return userpwd;
    }

    public void setUserpwd(String userpwd) {
        this.userpwd = userpwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", userpwd='" + userpwd + '\'' +
                '}';
    }
}

```

准备两个servlet,一个用于检测是否登录成功,另一个用于检测设置七天免登录是否成功

```java
import MyPojo.User;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

@WebServlet(urlPatterns = "/myServlet01.do")
public class myServlet01 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String userpwd = req.getParameter("userpwd");
        if ("gavin".equals(username)&&"123".equals(userpwd)){
            HttpSession session = req.getSession();
            User user= new User(username,userpwd);
            session.setAttribute("user",user);
        }else{
            resp.sendRedirect("index.jsp");
        }

    }
}

```

```java
package MyServletdemo;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(urlPatterns = "/myServlet02.do")
public class myServlet02  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
//        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=UTF-8");

        HttpSession session = req.getSession();
        Object attribute = session.getAttribute("user");
        PrintWriter writer = resp.getWriter();
        String Mes;
        if(null!=attribute){
            Mes="您已登录过!";
           
        }else{
            Mes="你还未登录!";
        }
        writer.write(Mes);
    }
}

```
在没有添加配置文件的时候---重启服务器后用户就需要重新登录,

添加配置文件后用户仍然处于登陆状态!

添加配置文件如下---->>
在web文件夹下添加  META-INF文件夹,然后创建Context.xml配置文件;

```xml
<?xml version="1.0" encoding="UTF-8"?>

<Context>

    <Manager className="org.apache.catalina.session.PersistentManager">

        <Store className="org.apache.catalina.session.FileStore" directory="d:/session"/>

    </Manager>
    </Context>
```
之后你就会发现-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/bab449c0e70247b895789f5ba366f44e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
原理就是服务器会在重启关闭后将session持久化到文件中,重启之后读取该文件,恢复session对象;


之前提到过session的钝化与活化,这里有一个监听器来监测session的钝化与活化-----


既然session是序列化后写入到文件中,那么 该监听器就需要实现监听器接口和实现序列化接口

```java

import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionActivationListener;
import javax.servlet.http.HttpSessionEvent;
import java.io.Serializable;

@WebListener
public class MySessionListener_o2 implements HttpSessionActivationListener , Serializable {

    @Override
    public void sessionWillPassivate(HttpSessionEvent se) {
        HttpSession session = se.getSession();
        System.out.println("检测到session"+session.hashCode()+"钝化了");

    }

    @Override
    public void sessionDidActivate(HttpSessionEvent se) {
        HttpSession session = se.getSession();
        System.out.println("检测到session"+session.hashCode()+"活化了");
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/94b3cbbc2bf342a89dd456b4bf6abb79.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/a9b6b9ae91454b11898483d87608cb57.png)

#  

# JavaWeb分页实现

后端代码的实现---->>>

目录结构的实现---->>

![](https://img-blog.csdnimg.cn/01345b29df0a43fb829836e59a18f607.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
代码


servlet,主要用请求转发,(请求行中携带一些关于查询的一些细信息)
```java
package com.gavin.controller;

import com.gavin.dao.FilmImp.FilmDaoImp;
import com.gavin.pojo.Film;
import com.gavin.pojo.PageBean;
import service.FilmService;
import service.imp.FilmServiceImp;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

@WebServlet(urlPatterns = "/ShowFilmController.do")
public class ShowFilmController extends HttpServlet {
    private FilmDaoImp filmImp = new FilmDaoImp();
private FilmService filmService= new FilmServiceImp();

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//         接收数据
        String currentPage1 = req.getParameter("currentPage");
        String pageSize1 = req.getParameter("pageSize");
        //转换成需要的类型
        //页码数
        int currentPage = Integer.parseInt(currentPage1);
        //页大小
        int pageSize = Integer.parseInt(pageSize1);
//        业务处理---查找数据
        PageBean<Film> pageBean = filmService.findByPage(currentPage, pageSize);

//将数据放入请求域
        req.setAttribute("pageBean", pageBean);


//        响应数据,页面跳转
        req.getRequestDispatcher("showFilm.jsp").forward(req, resp);


    }

}

```
接口----


```java
import java.util.List;

public interface FilmDao {
 /*   List<Film> findAll() ;
*/
    List<Film> findByPage(int currentPage, int pageSize);
Integer findTotalSize();


}
```
查询方法抽取
```java
package com.gavin.dao.FilmImp;


import com.gavin.pojo.Film;

import java.lang.reflect.Field;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

/**
 * @Author: Ma HaiYang
 * @Description: MircoMessage:Mark_7001
 */
public abstract class BaseDao {
    public int baseUpdate(String sql,Object ... args){
        Connection connection = null;
        PreparedStatement preparedStatement=null;
        ResultSet resultSet=null;
                int rows=0;
        try{
            connection = ConnectionPool.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            //设置参数
            for (int i = 0; i <args.length ; i++) {
                preparedStatement.setObject(i+1, args[i]);
            }
            //执行CURD
            rows =preparedStatement.executeUpdate();// 这里不需要再传入SQL语句
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            if(null != preparedStatement){
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            ConnectionPool.returnConnection(connection);
        }
        return rows;
    }


    public List baseQuery(Class clazz, String sql, Object ... args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        List list=new ArrayList<>() ;
        try{
            connection = ConnectionPool.getConnection();
            connection.setAutoCommit(false);
            preparedStatement = connection.prepareStatement(sql);//这里预编译SQL语句"select * from film limit 0,10;"
            //设置参数
            for (int i = 0; i <args.length ; i++) {
                preparedStatement.setObject(i+1, args[i]);
            }
            //System.out.println(sql);
            //执行CURD
            resultSet = preparedStatement.executeQuery();// 这里不需要再传入SQL语句----为什么resultset 为null返回size=0
            connection.commit();
            // 根据字节码获取所有 的属性
            Field[] fields = clazz.getDeclaredFields();
            for (Field field : fields) {
                field.setAccessible(true);// 设置属性可以 访问
            }

            while(resultSet.next()){
                // 通过反射创建对象
                Object obj = clazz.newInstance();//默认在通过反射调用对象的空参构造方法
                for (Field field : fields) {// 临时用Field设置属性
                    String fieldName = field.getName();// empno  ename job .... ...
                    Object data = resultSet.getObject(fieldName);
                    field.set(obj,data);
                }
                list.add(obj);

            }

        }catch (Exception e){
            e.printStackTrace();
        }

            FilmDaoImp.closeLink(resultSet,preparedStatement,connection);
            return list;

    }


    public Integer baseQueryInt(String sql,Object ... args) {
        Connection connection = null;
        PreparedStatement preparedStatement=null;
        ResultSet resultSet=null;
       Integer count=0;
        try{
            connection = ConnectionPool.getConnection();
            preparedStatement = connection.prepareStatement(sql);//这里已经传入SQL语句
            //设置参数
            for (int i = 0; i <args.length ; i++) {
                preparedStatement.setObject(i+1, args[i]);
            }
            //执行CURD
            resultSet = preparedStatement.executeQuery();// 这里不需要再传入SQL语句
            // 根据字节码获取所有 的属性
            while(resultSet.next()){
                // 通过反射创建对象
                count = resultSet.getInt(1);
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            FilmDaoImp.closeLink(resultSet,preparedStatement,connection);
            return count;
        }

    }
}


```
连接池

```java
package com.gavin.dao.FilmImp;

import org.apache.log4j.Logger;
import util.UtilTool;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.LinkedList;


public class ConnectionPool {
    //准备连接用的
    private static String DRIVER ;
   private static String URL ;
    private static String USER;
    private static String PASSWORD ;
    //准备一个池子用于盛放连接
    private static LinkedList<Connection> pool;
    //初始化大小
    private static int initSize ;
    //限制池子的最大大小
    private static int maxSize ;
    //加载该连接池类信息时就初始化了pool,pool中存放了5个连接实例
    private static Logger logger=null;

    static {
       logger= Logger.getLogger(ConnectionPool.class);
     UtilTool util= new UtilTool("/jdbc.properties");
       DRIVER = util.getProperties("DRIVER");
       URL = util.getProperties("URL");
        USER = util.getProperties("USER");
       PASSWORD= util.getProperties("PASSWORD");
       initSize=Integer.parseInt(util.getProperties("iniSize"));
        maxSize=Integer.parseInt(util.getProperties("maxSize"));
        //加载驱动
        try {

            Class.forName(DRIVER);
        } catch (ClassNotFoundException e) {
           logger.fatal("驱动加载失败",e);

        }
        //开辟空间用于初始化连接池
       pool = new LinkedList<Connection>();
       //往池子中添加5个连接实例
       for (int i = 0; i < initSize; i++) {
       Connection connection = initConnection();
        pool.addLast(connection);
            }
        }
        //初始化一个connection;
public static Connection initConnection(){
Connection connection = null;
    try {
        connection=DriverManager.getConnection(URL, USER, PASSWORD);
    } catch (SQLException e) {
        logger.error("初始化连接失败",e);
    }
return connection;

}
    //用于获得连接的方法
    public static Connection getConnection() {
        //怎么获得呢?
        //当连接池中有连接的时候,直接从里面取用
        Connection connection;
        if (pool.size() != 0) {
            connection = pool.removeFirst();
            logger.info("连接池中--"+connection.hashCode()+"被取走了");


        } else {//当连接池中没有的时候,创建一个
             connection = initConnection();
            logger.info("连接池已空--"+connection.hashCode()+"被创建了");


        }
        return connection;
    }

    //用户还回去的方法
    public static void returnConnection(Connection connection){

//什么时候往回还,要求池子内少于五个的时候我才接收
        //检测
        if(null!=connection) {//不为空

                try {//检测
                    if (!connection.isClosed()) {//连接未关闭的话
                        //检测
                        //如果池子中少于10个则可以添加进去--始终保证最大化缓冲池
                        if (pool.size() <= maxSize) {
                            //向池中增加该连接
                            connection.setAutoCommit(true);
                            pool.addLast(connection);//
                            logger.info("归还的连接"+connection.hashCode()+"符合要求,归还成功");

                        }else{//如果归还后池子中连接数大于10,则将该链接关闭
                            logger.info("池子中已满"+connection.hashCode()+"--将被关闭");

                            connection.close();
                            logger.info("池子中已满"+connection.hashCode()+"--被关闭");
                        }
                    }else{//连接关闭
                        logger.warn("归还的连接不能使用,归还失败");

                    }
                } catch (SQLException e) {
                    logger.warn("连接关闭失败");
                }

        }else{
            logger.warn("连接不能使用,归还失败");

        }

    }
    //测试连接池
    /*public static void main(String[] args) {
        Connection connection = getConnection();
        Connection connection1 = getConnection();
        try {
            connection1.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        getConnection();
        getConnection();
        getConnection();
        getConnection();
        returnConnection(connection);
        returnConnection(null);
        returnConnection(connection1);
    }*/
}

```
实现方法
```java
package com.gavin.dao.FilmImp;


import com.gavin.dao.FilmDao;
import com.gavin.pojo.Film;

import java.sql.*;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class FilmDaoImp extends BaseDao implements FilmDao {
    static List<Film> list = new ArrayList<>();
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    ResultSet resultSet = null;

   /* @Override
    public List<Film> findAll() {
        try {
            String SQL = "select * from film;";
            connection = ConnectionPool.getConnection();
            preparedStatement = connection.prepareStatement(SQL);
            resultSet = preparedStatement.executeQuery();
            if (null != resultSet) {
                while (resultSet.next()) {
                    Integer film_id = resultSet.getInt("film_id");
                    String title = resultSet.getString("title");
                    String description = resultSet.getString("description");
                    Date release_year = resultSet.getDate("release_year");
                    Integer rental_duration = resultSet.getInt("rental_duration");
                    Double rental_rate = resultSet.getDouble("rental_rate");
                    Integer length = resultSet.getInt("length");
                    Double replacement_cost = resultSet.getDouble("replacement_cost");
                    String rating = resultSet.getString("rating");
                    String special_feature = resultSet.getString("special_feature");

                    Film film = new Film(film_id, title, description, release_year, rental_duration, rental_rate, length, replacement_cost, rating, special_feature);
                    list.add(film);
                }

            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            closeLink(resultSet, preparedStatement, connection);
        }
        return list;
    }
*/
    /**
     * @param currentPage  当前页
     * @param pageSize     页大小
     * @return
     */
    @Override
    public List findByPage(int currentPage, int pageSize) {
        String sql = "select * from film limit ?,?;";
       //从哪条记录开始
        //页大小
        List list = baseQuery(Film.class, sql,(currentPage - 1) * pageSize,pageSize);
        return list;
    }

    @Override
    public Integer findTotalSize() {
        String sql = "select count(?) from film ";
        Integer TotalSize = baseQueryInt(sql, 1);


        return TotalSize;
    }

    public static void closeLink(ResultSet resultSet, PreparedStatement preparedStatement, Connection connection) {

        try {
            if (null != resultSet)
                resultSet.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (null != preparedStatement)
                preparedStatement.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        if (null != connection)
            ConnectionPool.returnConnection(connection);
    }
}

```
实体类封装
```java
package com.gavin.pojo;

import java.io.Serializable;
import java.util.Date;

public class Film implements Serializable {

//    private static final long serialVersionUID = 95594597L;
    //属性
    private   Long film_id;
    private String title;
    private String description;
    private Date release_year;
    private  Long rental_duration ;
    private  Double rental_rate ;
    private Long length;
    private  Double replacement_cost;
    private String rating;
    private String special_features;

    public Film() {
    }
//构造


    public Film(Long film_id, String title, String description, Date release_year, Long rental_duration, Double rental_rate, Long length, Double replacement_cost, String rating, String special_features) {
        this.film_id = film_id;
        this.title = title;
        this.description = description;
        this.release_year = release_year;
        this.rental_duration = rental_duration;
        this.rental_rate = rental_rate;
        this.length = length;
        this.replacement_cost = replacement_cost;
        this.rating = rating;
        this.special_features = special_features;
    }

    public Long getFilm_id() {
        return film_id;
    }

    public void setFilm_id(Long film_id) {
        this.film_id = film_id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public Date getRelease_year() {
        return release_year;
    }

    public void setRelease_year(Date release_year) {
        this.release_year = release_year;
    }

    public Long getRental_duration() {
        return rental_duration;
    }

    public void setRental_duration(Long rental_duration) {
        this.rental_duration = rental_duration;
    }

    public Double getRental_rate() {
        return rental_rate;
    }

    public void setRental_rate(Double rental_rate) {
        this.rental_rate = rental_rate;
    }

    public Long getLength() {
        return length;
    }

    public void setLength(Long length) {
        this.length = length;
    }

    public Double getReplacement_cost() {
        return replacement_cost;
    }

    public void setReplacement_cost(Double replacement_cost) {
        this.replacement_cost = replacement_cost;
    }

    public String getRating() {
        return rating;
    }

    public void setRating(String rating) {
        this.rating = rating;
    }

    public String getSpecial_features() {
        return special_features;
    }

    public void setSpecial_features(String special_features) {
        this.special_features = special_features;
    }

    @Override
    public String toString() {
        return "Film{" +
                "film_id=" + film_id +
                ", title='" + title + '\'' +
                ", description='" + description + '\'' +
                ", release_year=" + release_year +
                ", rental_duration=" + rental_duration +
                ", rental_rate=" + rental_rate +
                ", length=" + length +
                ", replacement_cost=" + replacement_cost +
                ", rating='" + rating + '\'' +
                ", special_feature='" + special_features + '\'' +
                '}';
    }
}

```

封装分页数据
```java
package com.gavin.pojo;

import java.io.Serializable;
import java.util.List;

public class PageBean<T>  implements Serializable {
private List<T> data;
private Integer totalSize;//总记录
private Integer pageSize;//页大小
private Integer totalPage;//总页数
private Integer currentPage;//当前页

    public PageBean() {
    }

    public PageBean(List<T> data, Integer totalSize, Integer pageSize, Integer totalPage, Integer currentPage) {
        this.data = data;
        this.totalSize = totalSize;
        this.pageSize = pageSize;
        this.totalPage = totalPage;
        this.currentPage = currentPage;
    }

    public List<T> getData() {
        return data;
    }

    public void setData(List<T> data) {
        this.data = data;
    }

    public Integer getTotalSize() {
        return totalSize;
    }

    public void setTotalSize(Integer totalSize) {
        this.totalSize = totalSize;
    }

    public Integer getPageSize() {
        return pageSize;
    }

    public void setPageSize(Integer pageSize) {
        this.pageSize = pageSize;
    }

    public Integer getTotalPage() {
        return totalPage;
    }

    public void setTotalPage(Integer totalPage) {
        this.totalPage = totalPage;
    }

    public Integer getCurrentPage() {
        return currentPage;
    }

    public void setCurrentPage(Integer currentPage) {
        this.currentPage = currentPage;
    }
}

```
接口
```java
import com.gavin.pojo.Film;
import com.gavin.pojo.PageBean;

public interface FilmService {
    PageBean<Film> findByPage(int currentPage, int pageSize);


}
```
接口实现
```java
package service.imp;
import com.gavin.dao.FilmImp.FilmDaoImp;
import com.gavin.dao.FilmDao;
import com.gavin.pojo.Film;
import com.gavin.pojo.PageBean;
import service.FilmService;

import java.util.List;

public class FilmServiceImp implements FilmService {

    //private FilmService filmDao = new FilmServiceImp();


    //    查找数据并将分页数据封装
    @Override
    public PageBean<Film> findByPage(int currentPage, int pageSize) {
//        怎么查找?
//        通过filmDao接口中的方法
         FilmDao filmDao = new FilmDaoImp();
        ////查询 该页所有数据,比如第一页,10条数据
       List<Film> data = filmDao.findByPage(currentPage, pageSize);
        Integer totalSize = filmDao.findTotalSize();
        //计算总页数
        Integer totalPage = totalSize % pageSize == 0 ? totalSize : totalSize / pageSize + 1;
        //当前页
        //页大小
        PageBean<Film> pageBean = new PageBean<>(data, totalSize, pageSize, totalPage, currentPage);


        return pageBean;
    }
}

```


记录一下在过程中遇到的一点小问题

![在这里插入图片描述](https://img-blog.csdnimg.cn/3b0b3868890b4a51bb82e8b44c604b4e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

错误日志-----![在这里插入图片描述](https://img-blog.csdnimg.cn/ead1dd0c24ec44c4acf24d7bb8e7d348.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
原因时在封装对象进入List集合时由于用的时Object,对于整型会自动转为Long,而是不时Integer

修改实体类中的相应数据类型为Long就可以了;(由于考虑到有些数据可能为null,所以最好要用包装类;

# JavaWeb同步异步交互

以前觉得异步跟同步是一个很高深的学问,后来才发现就是一门学问,深不深的看人而已;

## 同步交互
就是用户在浏览器上所有动作都操作完毕后再将信息发送给服务器端,然后服务器端做出响应返回给浏览器数据;
大体逻辑是这样的------>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/d277bdcdc3994739b344645bc5cd620b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 同步交互的优点
> 可以保留浏览器后退按钮的正常功能。在动态更新页面的情况下，用户可以回到前一个页面状态，浏览器能记下历史记录中的静态页面,用户通常都希望单击后退按钮时，就能够取消他们的前一次操作，同步交互可以实现这个需求.

>同步交互的缺点
>1同步交互的不足之处，会给用户一种不连贯的体验，当服务器处理请求时，用户只能等待状态，页面中的显示内容只能是空白。
>2因为已经跳转到新的页面,原本在页面上的信息无法保存,好多信息需要重新填写


## 异步交互
指发送一个请求,不需要等待返回,随时可以再发送下一个请求，即不需要等待。

大体逻辑是这样的---->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/07a1796d81294c939ca0a679035668cf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 优点
> 1前端用户操作和后台服务器运算可以同时进行,可以充分利用用户操作的间隔时间完成运算
> 2页面没有跳转,响应回来的数据直接就在原页面上,页面原有信息得以保留


> 缺点
> 可能破坏浏览器后退按钮的正常行为。在动态更新页面的情况下，用户无法回到前一个页面状态，这是因为浏览器仅能记录的始终是当前一个的静态页面。用户通常都希望单击后退按钮，就能够取消他们的前一次操作，但是在AJAX这样异步的程序，却无法这样做。




**什么是AJAX?**

 AJAX 即

“Asynchronous Javascript And XML”（异步 JavaScript和 XML），是指一种创建交互式、快速动态网页应用的网页开发技术，无需重新加载整个网页的情况下，能够更新部分网页的技术。通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

**AJAX关键技术**

使用CSS构建用户界面样式,负责页面排版和美工

使用DOM进行动态显示和交互,对页面进行局部修改

使用XMLHttpRequest异步获取数据

使用JavaScript将所有的元素绑定在一起



简单的写了一个异步交互的小案例--->

我们很常见的一个案例-----用户注册时会提示用户名可用后者不可用;

先看效果图--->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/3414475bd9b7434ca1603dbac6aa2674.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/1d5881ad1f9b446996a8572f6ae58ec0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
部分代码---->>

先理清思路,然后就好写了;
从前端开始倒推,然后开始完善我们需要的一些代码!!

前端------>>>jsp

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>注册页面</title>
    <script src="static/js/jquery.js"></script>
    <script>
        var xhr;

        function checkName() {
            var str = $("#uName").val();/*获取input中输入的文字*/
            /*当str为null或者空格字符串的时候*/
            if (str == null || str == "") {
                $("#checkInfo").html("<font color='red'> 用户名不允许为空</font>");
                /* 如果不符合条件则不执行下面的内容;*/
                return;
            }
            //如果不为空则显示字符   ''
            $("#checkInfo").text('');
//接下来发送异步请求
            xhr = new XMLHttpRequest();
//怎么发送呢?指定发送方式,发送地址,是否异步---true
            xhr.open("GET", "CheckName.do?userName="+$("#uName").val(),true)
            // 设置回调函数,用于接好服务器端发送过来的信息
            xhr.onreadystatechange = showReturnInfo;
//正式发送
            xhr.send(null);
        }
        function showReturnInfo() {
            /* 如果进行到第四阶段了,xhr的状态为200*/
            if (xhr.readyState == 4 && xhr.status == 200) {
                //获得服务器返回的数据
                var returnInfo = xhr.responseText;
                console.log(returnInfo);
               if (returnInfo == '用户名可用') {
                    $("#checkInfo").html("<font color='green'>用户名可用</font>");
                } else {
                    $("#checkInfo").html("<font color='red'>用户名不可用</font>");
                }
            }
        }
    </script>
</head>

<body>


<form method="get" action="MyServlet01.do">
    账号: <input type="text" name="userName" id="uName" onblur="checkName()"><span id="checkInfo"></span><br>
    密码:<input type="password" name="userPwd" id="uPwd"><br>
    <input type="submit" value="注册">
</form>

</body>
</html>

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/1f9c6a926c0b4b53ac4f6231fc8d8018.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


针对前端3处,
准备一个接口和实现类专门用以从数据库查询数据,并返回信息;
```java

public class CheckUserImp extends BaseDao implements Check {

    @Override
    public List CheckUser(String uName, String uPwd) {
        String sql = "select * from userinfo where userName = ? and userPwd = ? ; ";
        List list = baseQuery(User.class, sql, uName, uPwd);
       return list;
    }

    @Override
    public String RemindInfo(String uName) {
        String sql = "select * from userinfo where  userName = ? ;";
        List list = baseQuery(User.class, sql,uName);
        if (null != list&&list.size()!=0) {
            return "用户名不可用";
        }
            return "用户名可用";

    }

}

```
封装的实体类
![在这里插入图片描述](https://img-blog.csdnimg.cn/c36c9566dc96466f8534782f97586bb8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
准备servlet,用以检查用户名并向前端发送信息-----(异步)
```java
import AJAXDao.Check;
import AJAXDao.CheckImp.CheckUserImp;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
@WebServlet("/CheckName.do")
public class CheckName  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");

        String userName = req.getParameter("userName");
        Check check = new CheckUserImp();
        String remindInfo = check.RemindInfo(userName);
        resp.setCharacterEncoding("UTF-8");
     resp.setContentType("text/html;charset=UTF-8");
        resp.getWriter().write(remindInfo);
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/299ef054ae0d41de9be04683ef8e05ec.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
[源码放在这里了!](https://download.csdn.net/download/weixin_54061333/37091306)


JSON(JavaScriptObject Notation, JS 对象简谱) 是一种轻量级的数据交换格式。它基于ECMAScript (欧洲计算机协会制定的js规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。**简洁和清晰的层次结构使得JSON** 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。它有如下优点

1 **轻量级**,实现数据转换

2 Json 格式既能**考虑到前端对象的特点** 同时也能**兼顾后台对象信息**的特点

3 Json 格式可以被**前端直接识别并解析成对象**

4 jQuery形式实现AJAX**默认前后端传递数据的格式就是JSON**


## json在前端中的应用


```javascript
<script>
    // 用json创建对象
    var person = {"name": "张三", "age": 20}
console.log(person);
    console.log(person.name +"--"+person.age);
    var persons =[{"name":"李四","age":45,"gender":"女"},{'name':"王五","age":22}];
    console.log(persons);
    for (var i = 0; i < persons.length; i++) {
        var per=persons[i];
        console.log(per.name+"----"+per.age);
    }
/*字符串信息*/
var  personStr='{"name":"gavin","age":18,"gender":"男"}';
    /*将上面的信息转换成对象*/
var cha=JSON.parse(personStr);


console.log(cha);
console.log(cha.name+"---"+cha.age+"---"+cha.gender);
    
```


json的应用---->>

准备一个servlet用于向前端发送信息----
```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import pojo.Personn;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Date;
import java.util.List;

@WebServlet("/Myservlet01.do")
public class Myservlet01 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         req.setCharacterEncoding("UTF-8");
         resp.setCharacterEncoding("UTF-8");


        Personn per= new Personn("张三",21,"男",new Date());
        Personn per1= new Personn("李四",18,"男",new Date());
        Personn per2= new Personn("王五",22,"男",new Date());
        Personn per3= new Personn("赵六",26,"男",new Date());
        Personn per4= new Personn("陆七",19,"男",new Date());
        List<Personn> list= new ArrayList<>();
        Collections.addAll(list,per,per1,per2,per3,per4);
        //设置一些格式
     // GsonBuilder 用于指定字符串内某些数据类型的格式
        GsonBuilder gsonBuilder=new GsonBuilder().setDateFormat("yyyy-MM-dd");
        //指定好之后创建gson对象
        Gson gson=gsonBuilder.create();
//        String toJson = gson.toJson(per);

        String toJson = gson.toJson(list);//此时i是一个Json对象集合
        System.out.println(toJson);
        resp.getWriter().write(toJson);

    }
}

```

准备前端页面-->>

```javascript
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/11/5
  Time: 10:44
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<script src="js/jquery.js"></script>
<script>
    var xhr;

    function checkN() {
        var str = $("#u_Name").val();
        if (str == null || str == "") {

            $("#remindInfo").text("用户名不允许为空");
            return;
        }
       $("#remindInfo").text('');
        xhr = new XMLHttpRequest();
        xhr.open("GET", "Myservlet01.do?userName=" + str, true);
        xhr.send(null);
        xhr.onreadystatechange = showReturnInfo;
    }

    function showReturnInfo() {
        if (xhr.status == 200 && xhr.readyState == 4) {
//根据需要看怎样处理
            var info = xhr.responseText;//这里得到一个字符串--是json格式的对象集合
            var Inform = JSON.parse(info);/*将字符串转为对象集合*/

            for (var i = 0; i < Inform.length; i++) {
                var per=Inform[i];
                console.log(per.u_name);
                console.log(per.u_age);
                console.log(per.u_gender);
                console.log(per.u_birthday);
            }
        }
    }
</script>
<form method="get" action="">

    <input type="text" name="userName" id="u_Name" placeholder="请输入账号" onblur="checkN()"><span
        id="remindInfo"></span><br>
    <input type="password" name="userPwd" id="u_Pwd"><br>
    <input type="submit" value="注册">

</form>
</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/b1e3fa60eadf4bec92b981faee99e640.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
AJAX结合jquery简化异步交互代码---->>

```javascript

<!DOCTYPE html>
<html lang="en">
<meta charset="UTF-8"/>
<html>
<head>
    <title>$Title$</title>
</head>
<body>
<script src="js/jquery.js"></script>
<script>
    // // 用json创建对象
    // var person = {"name": "张三", "age": 20}
    // console.log(person);
    // console.log(person.name + "--" + person.age);
    // var persons = [{"name": "李四", "age": 45, "gender": "女"}, {'name': "王五", "age": 22}];
    // console.log(persons);
    // for (var i = 0; i < persons.length; i++) {
    //     var per = persons[i];
    //     console.log(per.name + "----" + per.age);
    // }
    //
    // var personStr = '{"name":"gavin","age":18,"gender":"男"}';
    // /*将上面的信息转换成对象*/
    // var cha = JSON.parse(personStr);
    //
    //
    // console.log(cha);
    // console.log(cha.name + "---" + cha.age + "---" + cha.gender);
    var xhr;

    function CheckName() {
        var u_name = $("#u_Name").val();
        if (u_name == null || u_name == "") {
            $("#checkInfo").text("用户名不允许为空");
            return;
        }
        $("#checkInfo").text("");
//通过jquery封装好的ajax()来发送异步请求
        /* $.ajax(属性名:属性值,方法名:方法)*/
        $.ajax(
            {
                type: "GET",//请求方式
                url: "CheckDemoImp.do?",//后台服务路径
                data: "userName=" + $("#u_Name").val(),//提交的数据
                success: function (info) {//成功时候响应
                   $("#checkInfo").text(info);
                },
                error: function () {//出现错误时候响应
                }
            }
        );
        // xhr = new XMLHttpRequest();
        // xhr.open("GET", "CheckDemoImp.do?userName=" + u_name, true);
        // xhr.send(null);
        // xhr.onreadystatechange = remindInfo;
    }

    // function remindInfo() {
    //     if (xhr.status == 200 && xhr.readyState == 4) {
    //         var Info = xhr.responseText;
    //         $("#checkInfo").text(Info);
    //     }
    //}


</script>

<form method="get" action="myServlet.do">

    <input type="text" id="u_Name" placeholder="用户名" name="userName" onblur="CheckName()"> <span
        id="checkInfo"></span><br>
    <input type="password" id="u_Pwd" placeholder="密码" name="userPwd"><br>
    <input type="submit" value="注册">
</form>

</body>
</html>
```
jquery中封装了ajax函数,通过主函数去直接调用就可以了,主义的就是参数的写法,这个跟ajax的open方法参数基本一样的理解----
{
type:"请求方式",
url:"将要发送到的服务端servlet",
data:"要发送的数据",
success: function(参数){
放一些响应成功后的信息;
},
error:function(参数){
产生异常时响应什么内容;
}
}

每个属性之间有用 逗号来区分;


## ajax发送数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/785b166edc3c4896a3e84fbf29d467f0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
后端发送数据,格式为JSON,前端解析json

准备一个jsp或者html,

```javascript
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>注册页面</title>
</head>
<body>
<script src="js/jquery.js"></script>
<script>
    function Info() {
        $.ajax(
            {
                type: "GET",
                url: "AjaxTest.do?",
                //data:{"userName":"小明","password":"123456"},可以在属性名处加引号也可以不加
                data: {userName: "小明", password: "123456"},//json对象的形式
                //data:"{\"userName\":\"小明\",\"password\":\"123456\"}",//json字符串形式
               dataType: "json",//以json来解析后端发过来的数据
                success: function (info) {
                  //如果后端发送过来的是字符串  ---info
                 alert(info);//是一个json对象
                   console.log(info);
                   console.log(info.userName);
                   console.log(info.password);

                 /*   var per = JSON.parse(info);
                    console.log(per.userName);
                    console.log(per.password);*/
                },
                error: function () {
                }
            }
        )
    }
</script>
<%--异步请求--%>
<input type="button" value="点我呀!!" onclick="Info()">

</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc002bb866ab40dcbaa8353e6a1bd816.png)




后端代码,准备一个servlet

```java

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/AjaxTest.do")
public class AjaxTest  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       req.setCharacterEncoding("UTF-8");
       resp.setCharacterEncoding("UTF-8");
       resp.setContentType("text/html;charset=UTF-8");
        String userName = req.getParameter("userName");
        String password = req.getParameter("password");

        if("小明".equals(userName)&&"123456".equals(password)){
           String str="{\"userName\":\"小明\",\"password\":\"123456\"}";//json     对象的形式

            resp.getWriter().write(str);
        }
    }
}

```
前端解析---->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/f610f7040f7f4295a14beb3c95cee464.png)
如果去掉dataType属性----->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/e4bb41cdcf434a258f9eaf71670acc85.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
怎么办呢?
手动转换为json格式的对象

![在这里插入图片描述](https://img-blog.csdnimg.cn/9733a9d82eb4470c908d3d2d9271bb14.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c67cb6fc24ae44ad94b7e7d92178ecbf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
@WebServlet("/AjaxTest.do")
public class AjaxTest  extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       req.setCharacterEncoding("UTF-8");
       resp.setCharacterEncoding("UTF-8");
       resp.setContentType("text/html;charset=UTF-8");
        String userName = req.getParameter("userName");
        String password = req.getParameter("password");
List<Personn> list = new ArrayList<>();
        Personn per= new Personn("张三",21,"男",new Date());
        Personn per1= new Personn("李四",18,"男",new Date());
        Personn per2= new Personn("王五",22,"男",new Date());
        Personn per3= new Personn("赵六",26,"男",new Date());
        Personn per4= new Personn("陆七",19,"男",new Date());
        Collections.addAll(list,per,per1,per2,per3,per4);
        if("小明".equals(userName)&&"123456".equals(password)){
           //String str="{\"userName\":\"小明\",\"password\":\"123456\"}";//json对象的形式
           //String str="{userName:\"小明\",password:\"123456\"}";//json对象的形式
           //将集合发送到前端,由于有日期需要处理的格式
            GsonBuilder gsonBuilder=new GsonBuilder();//不处理日期格式
           // GsonBuilder gsonBuilder=new GsonBuilder().setDateFormat("yyyy-MM-dd");
            //GsonBuilder gsonBuilder1 = gsonBuilder.setDateFormat("yyyy-MM-dd");
            Gson gson = gsonBuilder.create();
            String json = gson.toJson(list);
            resp.getWriter().write(json);

        }
    }
}
```

向前端发送集合后解析
![在这里插入图片描述](https://img-blog.csdnimg.cn/c77ccd33b9dc4e39855878877909431f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed3c6369a35a4d46951dffd917d77ad2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**jQuery.ajax()属性详解**

**1.url:**
要求为String类型的参数，（默认为当前页地址）发送请求的地址。

**2.type:**

要求为String类型的参数，请求方式（post或get）默认为get。注意其他http请求方法，例如put和delete也可以使用，但仅部分浏览器支持。

3.timeout:

要求为Number类型的参数，设置请求超时时间（毫秒）。此设置将覆盖$.ajaxSetup()方法的全局设置。

4.async:

要求为Boolean类型的参数，默认设置为true，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为false。注意，同步请求将锁住浏览器，用户其他操作必须等待请求完成才可以执行。

5.cache:

要求为Boolean类型的参数，默认为true（当dataType为script时，默认为false），设置为false将不会从浏览器缓存中加载请求信息。

**6.data:**

要求为Object或String类型的参数，发送到服务器的数据。如果已经不是字符串，将自动转换为字符串格式。get请求中将附加在url后。防止这种自动转换，可以查看　　processData选项。对象必须为key/value格式，例如{foo1:"bar1",foo2:"bar2"}转换为&foo1=bar1&foo2=bar2。如果是数组，JQuery将自动为不同值对应同一个名称。例如{foo:["bar1","bar2"]}转换为&foo=bar1&foo=bar2。

**7.dataType:**

要求为String类型的参数，预期服务器返回的数据类型。如果不指定，JQuery将自动根据http包mime信息返回responseXML或responseText，并作为回调函数参数传递。可用的类型如下：

xml：返回XML文档，可用JQuery处理。

html：返回纯文本HTML信息；包含的script标签会在插入DOM时执行。

script：返回纯文本JavaScript代码。不会自动缓存结果。除非设置了cache参数。注意在远程请求时（不在同一个域下），所有post请求都将转为get请求。

**json：返回JSON数据。**

**jsonp：JSONP格式。**使用JSONP形式调用函数时，例如myurl?callback=?，JQuery将自动替换后一个“?”为正确的函数名，以执行回调函数。

text：返回纯文本字符串。

**8.beforeSend：**

要求为Function类型的参数，发送请求前可以修改XMLHttpRequest对象的函数，例如添加自定义HTTP头。**在beforeSend中如果返回false可以取消本次ajax请求。**

 

XMLHttpRequest对象是惟一的参数。
  function(XMLHttpRequest){
       this; //调用本次ajax请求时传递的options参数
  }


9.complete：

要求为Function类型的参数，请求完成后调用的回调函数（请求成功或失败时均调用）。参数：XMLHttpRequest对象和一个描述成功请求类型的字符串。

 

function(XMLHttpRequest, textStatus){
  this; //调用本次ajax请求时传递的options参数


}


10.success：

要求为Function类型的参数，请求成功后调用的回调函数，有两个参数。
(1)由服务器返回，并根据dataType参数进行处理后的数据。
(2)描述状态的字符串。

 

function(data, textStatus){
  //data可能是xmlDoc、jsonObj、html、text等等
  this; //调用本次ajax请求时传递的options参数


}

 常用的属性:

```javascript
  $.ajax(
            {
                type: "GET",
                url: "AjaxTest.do?",
                async: true,//以异步的i形式发送,默认也是
                //data:{"userName":"小明","password":"123456"},可以在属性名处加引号也可以不加
                data: {userName: "小明", password: "123456"},//json对象的形式
                //data:"{\"userName\":\"小明\",\"password\":\"123456\"}",//json字符串形式
                // dataType: "json",//以json来解析后端发过来的数据
                success: function (info) {
                    // 发过来的是集合
                    alert(info);
                    var t = JSON.parse(info);
                    alert(t);//是一个json对象
                    for (var i = 0; i < t.length; i++) {
                        var per = t[i];
                        console.log(per);
                    }
                    $.each(t, function (i, e) {//i为下标,e为元素
                        console.log(e);
                    })
                },
                error: function () {
                },
                beforeSend: function (xhr) {
                    //在请求之前做一些处理
                    //return false;//取消ajax请求
                },
                complete: function () {
                    alert('请求完成,成不成功都会执行');
                },
                cache: true,//启用缓存
                //内容编码类型默认为"application/x-www-form-urlencoded"。该默认值适合大多数应用场合。
                contentType: "application/x-www-form-urlencoded",//
                processData: true,//是否自动转换信息
                dataFilter: function (data, type) {
                    /*
                    * 数据过滤方法
                    * 给Ajax返回的原始数据进行预处理的函数。
                    * 提供data和type两个参数。
                    * data是Ajax返回的原始数据，
                    * type是调用jQuery.ajax时提供的dataType参数。
                    * 函数返回的值将由jQuery进一步处理
                    * */
                }
            }
        )
```
还有简化的一些方法可以使用,在jquery中你会发现一些神奇的API,大大简化我们的代码量,

一起瞅瞅,jQuery中封装好的API
第一种是经典形式,前面已经用到过,不在赘述;

![在这里插入图片描述](https://img-blog.csdnimg.cn/4b49dc7da1c441c2af6caaaae77bdfc3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### load(url, [data], [callback])

![在这里插入图片描述](https://img-blog.csdnimg.cn/6b390c56df0c42efa6b8c277e5dce374.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)准备一个servlet
![在这里插入图片描述](https://img-blog.csdnimg.cn/acb1dd8a1d9c454fb56a0c31987ff77e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)准备一个jsp或者html,用load方法去实现异步交互


![在这里插入图片描述](https://img-blog.csdnimg.cn/6abc1c900fc3421f92aa20a2e223ba35.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)大大减少了代码量;


除此之外load方法还可以加载一些其他元素
比如下面的代码

```javascript
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/11/6
  Time: 10:13
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<script src="js/jquery.js"></script>
<style type="text/css">
    #d1 {
        border: 1px solid red;
        width: 200px;
        height:300px;
    }
</style>
<script>
    function showInput() {
       // $("#d1").load("ajaxload.do?", "userInput=" + $("#uInput").val(), function () {
     //alert("响应结束");
       // })
       //  $("#d1").load( "staticIndex.html #fm")
        //$("#d1").load("staticIndex.html")//可以是整个页面,也可以是页面中的元素
        $("#d1").load("index.jsp")//可以是jsp
    }
</script>
<div id="d1"></div>
<input type="text" name="userInput" id="uInput" placeholder="请输入内容" onblur="showInput()">
</body>
</html>

```
应用-------注册页面可以用load实现.

![在这里插入图片描述](https://img-blog.csdnimg.cn/43e9f4b7384045f8bb595371a2b2a044.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### get(url, [data], [callback], [type])

通过远程 HTTP GET 请求载入信息。

这是一个简单的 GET 请求功能以取代复杂 $.ajax 。请求成功时可调用回调函数。如果需要在出错时执行函数，请使用 $.ajax。

> url,[data,[callback]]String,Map/String,CallbackV1.0url:待装入 HTML 网页网址。
> data:发送至服务器的 key/value 数据。在jQuery 1.3中也可以接受一个字符串了。
> callback:载入成功时回调函数。

```javascript
 $.get(
            "ajaxload.do?",
            "userInput=" + $("#uInput").val(),
            function(info){
               $("#d1").html(info)
            }
        )
```

### post(url, [data], [callback], [type])
通过远程 HTTP POST 请求载入信息。

这是一个简单的 POST 请求功能以取代复杂 $.ajax 。请求成功时可调用回调函数。如果需要在出错时执行函数，请使用 $.ajax。

```javascript
 $.post(
            "ajaxload.do?",
            "userInput=" + $("#uInput").val(),
            function(info){
                $("#d1").html(info)
            }
        )
```
如果传过来的是对象或者其他引用

### getJSON(url, [data], [callback])
通过 HTTP GET 请求载入 JSON 数据。

在 jQuery 1.2 中，您可以通过使用JSONP形式的回调函数来加载其他网域的JSON数据，如 "myurl?callback=?"。jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。 注意：此行以后的代码将在这个回调函数执行前执行。

```javascript
 $.getJSON(
            "ajaxload.do?",
            "userInput=" + $("#uInput").val(),
            function (info) {
                //遍历集合
                $.each(info,function (i, e) {
                    console.log(e)
                })
            }
        )
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e50b09cc60c342be9189bcf7f6f3ec03.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

常用的jquery异步请求方法就这几个,

$.ajax()---经典方法
$.post()
$.get()
$.getJson()----这种方法默认是get方法请求,然后转换成json



## ajax跨域请求
跨域---是指页面请求的资源不在同一个服务器上,怎么模拟呢???

首先打开Hbuilder,新建一个项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/6e4d9daa3a9347acb52c02b66013d74c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

index页面内容---->>>直接从idea中拷贝

```html
<!DOCTYPE html>
<html>

	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>

	<body>

<script src="js/jquery.js"></script>
<script>
    var xhr;

    function CheckUserName() {
        if ($("#uname1").val() == null || $("#uname1").val() == '') {

            $("#Info").html("<font color='red'>用户名不能为空</font>");
            return;
        }
        $("#Info").text('');
        $.get(
            "checkNameServlet.do?",
            userName = +$("#uname1").val(),
            function (info) {
                if (info == "用户名可用") {
                    $("#Info").html("<font color='green'>用户名可用</font>");
                } else {
                    $("#Info").html("<font color='red'>用户名已被占用</font>");
                }
            }
        )
    }

</script>

<form method="post" action="MyServlet01.do">
    <input type="text" id="uname1" name="userName" placeholder="账户" onblur="CheckUserName()"><span id="Info"></span>
    <br>
    <input type="password" name="userPwd" placeholder="密码">
    <br>
    <input type="submit" value="提交">


</form>
	</body>

</html>
```

效果并没有达到,

![在这里插入图片描述](https://img-blog.csdnimg.cn/6b979c1a42944f6b94e79626bacbbb51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
查看一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/e004a9165dba4048b46cf01334cbbf87.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
发现Hbuilder在请求servlet.do是直接在本项目下请求的,现在是模拟跨域请求,即我们的html上的url是相对路径,改为绝对路径再次请求-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/29961c9f3123495ba2573825ffe216c4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/32bc957d1115420485620c94d49b36fe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)仍然没有达到效果,再看一下原因------>>被拦截了
![在这里插入图片描述](https://img-blog.csdnimg.cn/6d2a7c3e464446a88faca17f7c1a7edd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 如何实现AJAX跨域



为什么会出现上面的问题?

 那就要了解什么是跨域?

出于浏览器的**同源策略限制**。同源策略（Sameoriginpolicy）是一种约定，它是浏览器最核心也最基本的**安全功能**，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。同源策略会阻止一个域的javascript脚本和另外一个域的内容进行交互。所谓**同源**（即指在同一个域）就是两个页面具有**相同的协议**（protocol），**主机（**host）和**端口号（port)**

我们分析一下,
因为要跨域请求一个servlet,那是这个请求是在哪里产生的呢?

jquery中封装的函数,那可不可以跨域请求一个jquery文件来实现异步请求呢?


首先打开IDEA中的项目,
![在这里插入图片描述](https://img-blog.csdnimg.cn/47bdb6d7c3624b3a9594e812aad36de5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)运行项目,然后在浏览器中打开该jquery文件,
复制浏览器中的地址到 Hbuilder中的index.html文件中

![在这里插入图片描述](https://img-blog.csdnimg.cn/89962044bbfe446d98d2044f1546f80a.png)再次运行,发现仍然不能完成异步请求;

我们来测试下注掉异步请求的部分代码,然后再尝试一下,发现验证用户名不为空部分是好使的,说明可以跨域请求js文件,但是直接异步请求跨域的servlet却不可以;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a3c5638435904c73834582963fbd24b6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)接下来我们在post的方法中加入一个属性----->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/73bce55409cb4a62a5d4bad08c80cf02.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
那么这样可不可以成功的异步请求到servlet呢?

![在这里插入图片描述](https://img-blog.csdnimg.cn/c51d7848c8034716802ea587b6e8800b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如何证明呢?我们在servlet中输出一下

![在这里插入图片描述](https://img-blog.csdnimg.cn/875274f2f91e4bc88d7ed7bea437cccb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
在浏览器中输入文字,servlet成功接收到了信息,说明的确实现了跨域请求,但是返回的数据浏览器中报错  --->**Uncaught ReferenceError: 用户名可用 is not defined**  说明是返回的数据解析式出现了问题;
![在这里插入图片描述](https://img-blog.csdnimg.cn/12d0981b37264427a676ab3a0f4320d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

为什么加了属性 jsonp就可以实现跨域异步请求呢?

![在这里插入图片描述](https://img-blog.csdnimg.cn/791fef9d72d746dbaede607e791eb57a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
原理是这样子的,通过增加type属性 "jsonp"会创建一个script,里面的url指向这个servlet,即相当于这个样子的----->>

```html
<script src=  "http://localhost:8080/AJAXproject_war_exploded/checkNameServlet.do?"></script>
```

然后返回的数据会被浏览器当作js代码来运行,

为便于测试,修改servlet中返回的数据------

![在这里插入图片描述](https://img-blog.csdnimg.cn/5d8475d9a69c45ff9179bb12c38b561c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
重新部署项目,

![在这里插入图片描述](https://img-blog.csdnimg.cn/42c4290e728547b6a273dc4c78487cbc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这说明后台servlet返回的数据确实是被浏览器当做js代码来运行的;

既然是这样,那就好办了,

告诉浏览器去调用某个js方法

![在这里插入图片描述](https://img-blog.csdnimg.cn/c0debcf4fafd44ce846cdc4d462774eb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/ad6b9696c7ea42b7b79f78bfa16187e3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这样不就可以实现我们想要的效果了么,


重新部署项目,

![在这里插入图片描述](https://img-blog.csdnimg.cn/76b192b0e21a4bcb921470d2c17eb6b4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
但是我们一般是后台代码写好后不会怎么轻易修改,因为修改后需要重新部署或者重启服务器,那么怎么解决呢?


在前端请求时的参数中加入回调时的js方法,这样后端获取该方法名然后拼接到返回数据中就可以了;

![在这里插入图片描述](https://img-blog.csdnimg.cn/48381d3b5e4c47888b0ad11630913144.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
仍然是好使的;,但是这种方法是使得原来post/get中的function失效了;![在这里插入图片描述](https://img-blog.csdnimg.cn/4bda12483c534ba68cc3536f854e31f7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)因为post/get方法参数类型可选的比较少,
url(必选), [data], [callback], [type]),并没有jsonp可选,


如果想要使得方法中的function不失效,可以选择用更加详细的$.ajax()来实现,(.ajax()可以实现全局的属性设置,用于处理比较复杂一些的异步请求)

如果用ajax方法实现------>>
```javascript
$.ajax({
					type: "POST",
					url: "http://localhost:8080/AJAXproject_war_exploded/checkNameServlet.do?",
					data: "userName=" + $("#uname1").val(),
					dataType: "jsonp",
					jsonp:"method",//命名success属性下的方法名
					success: function(info) {
						if(info == "用户名可用") {
							$("#Info").html("<font color='green'>用户名可用</font>");
						} else {
							$("#Info").html("<font color='red'>用户名已被占用</font>");
						}
					}
				})
```

jsonp属性的作用:
在一个**jsonp请求中重写回调函数的名字**。**这个值用来替代在"callback=?"这种GET或POST请求中URL参数里的"callback"部分**，比如{jsonp:'onJsonPLoad'}会导致将"onJsonPLoad=?"传给服务器。





后端代码----->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/1adea1f649d2458280df4c9ac5ea4a23.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这种相当于在data种拼接了一个"&method=系统生成的名字",
打印的回调函数名----->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/090407fd28824b5d949c691e394b1257.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


> 总结,在异步请求时,post或者get方法实现时  增加一个属性  type,     为  "jsonp",但是post/get中的function(){}就失效了
>
>
> 如果是ajax方法实现,需要增加  dataType="jsonp",想要使得方法内部的function(){} 有效并得以执行,还需要加一个参数   jsonp:"方法名"


针对上面的post/get方法在实现跨域时,function()会失效,可以通过jquery中封装好的getjson方法------->>>

### getJSON(url, [data], [callback])
通过 HTTP GET 请求载入 JSON 数据。

在 jQuery 1.2 中，您可以通过使用JSONP形式的回调函数来**加载其他网域**的JSON数据，如 "myurl?callback=?"。jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。 注意：此行以后的代码将在这个回调函数执行前执行。

参数
url,[data],[callback]

url:发送请求地址。

data:待发送 Key/value 参数。

callback:载入成功时回调函数。




```javascript
	$.getJSON(
					"http://localhost:8080/AJAXproject_war_exploded/checkNameServlet.do?callback=?",
					"userName=" + $("#uname1").val(),
					function(info) {
						if(info == "用户名可用") {
							$("#Info").html("<font color='green'>用户名可用</font>");
						} else {
							$("#Info").html("<font color='red'>用户名已被占用</font>");
						}
					}

				)
```
url上加上---任意名 =?,

![在这里插入图片描述](https://img-blog.csdnimg.cn/348e350c1e9b44ee8a1fe25e04dee11f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# 三级联动的实现

直接开干就行了；
![在这里插入图片描述](https://img-blog.csdnimg.cn/a20bcbfd619b497a8f975076529fb7d8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



MVC模式开搞-------》》》
先看一下整体框架结构：
![在这里插入图片描述](https://img-blog.csdnimg.cn/bdd4632bf8cb4417b9a48b6d96eb328b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)前端发送请求(异步)给servlet，servlet调用service去获取数据库的数据，然后发送到前端

准备各个层的代码------核心代码

servlet代码-----》》》
```java
import cityPojo.Area;
import cityService.CityService;
import cityService.Imp.CityServiceImp;
import com.google.gson.Gson;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

@WebServlet("/CityController.do")
public class CityController extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
//给定初始值,以防未传参数时的异常;
        Integer parentid=0;
        try {//如果转换失败,做一些处理
         parentid=Integer.parseInt(req.getParameter("parentid"));
        } catch (NumberFormatException e) {
            e.printStackTrace();
        }

        CityService cityService= new CityServiceImp();
//        获得查询到的数据---list集合
        List<Area> byParentId = cityService.findByParentId(parentid);
//发送到前端,要用json对象的形式
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        Gson gson= new Gson();//获得gson对象，将获得的list集合转为字符串
        String cityInfo = gson.toJson(byParentId);
        //发送到前端
        resp.getWriter().write(cityInfo);
    }
}

```

前端代码-----》》分解---

需求是页面加载完毕就要实现省份的加载，选择完省份的时候就要加载完城市，

用jquery来实现----》》

```html
 <script>
        // 页面加载完毕就要加载省份
        $(function () {
            $.ajax({
                    type: "GET",//get请求方式
                    url: "CityController.do?",//请求的servlet
                    data: "parentid=0",//参数
                    dataType: "json",//返回的数据类型,这样就可以直接用了,不用转换了
                    success: function (info) {//后端返回来的信息
                        $.each(info, function (i, e) {
                            // 加载完毕省份之后要知道用户选择了那个省,那怎么实现呢?
                            $("#province").append('<option value="' + e.areaid + '">' + e.areaname + '</option>');
                        })
                    }

                }
            )

        })
        function showCity(selectId){
            $.ajax({
                    type: "GET",//get请求方式
                    url: "CityController.do?",//请求的servlet
                    data: "parentid="+selectId,//参数
                    dataType: "json",//返回的数据类型,这样就可以直接用了,不用转换了
                    success: function (info) {//后端返回来的信息
                        $.each(info, function (i, e) {
                            // 加载完毕省份之后要知道用户选择了那个省,那怎么实现呢?
                            $("#city").append('<option value="' + e.areaid + '">' + e.areaname + '</option>');
                        })
                    }

                }
            )

            //alert(selectId);
        }

    </script>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a932bd57aa8544f9adfabcb606b267d5.png)

但是写完会有一个问题-----》》
![在这里插入图片描述](https://img-blog.csdnimg.cn/81144739b3734ecfb47512fa8af6dffe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)原因是上一次的加载的信息没有清除，清除后就好了“
![在这里插入图片描述](https://img-blog.csdnimg.cn/f8f29a6a72ba42aaae2f0d802f51ea38.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


同样去处理区的信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/de06fecc2b434e50a2a2c8851b5a51b6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/208db4c5d10e496ea094186ae985c2df.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)写完之后的效果----

![在这里插入图片描述](https://img-blog.csdnimg.cn/8f35a8f54a424363a1f66bb3ab03d6e3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

但是重复代码太多，可以简化一下-----》》
提取方法 为showArea(selectid,area)
完整的前端代码
```html

<html>
<head>
    <title>三级联动</title>
    <script src="js/jquery.js"></script>
    <script>
        // 页面加载完毕就要加载省份
        $(function () {
           /* $.ajax({
                    type: "GET",//get请求方式
                    url: "CityController.do?",//请求的servlet
                    data: "parentid=0",//参数
                    dataType: "json",//返回的数据类型,这样就可以直接用了,不用转换了
                    success: function (info) {//后端返回来的信息
                        $.each(info, function (i, e) {
                            // 加载完毕省份之后要知道用户选择了那个省,那怎么实现呢?
                            $("#province").append('<option value="' + e.areaid + '">' + e.areaname + '</option>');
                        })
                    }

                } )*/
           showArea(0,'#province')

        })
        function showArea(selectId,area){
            $.ajax({
                    type: "GET",//get请求方式
                    url: "CityController.do?",//请求的servlet
                    data: "parentid="+selectId,//参数
                    dataType: "json",//返回的数据类型,这样就可以直接用了,不用转换了
                    success: function (info) {//后端返回来的信息
                        $(area).html("<option>--请选择--</option>");
                        $.each(info, function (i, e) {
                            // 加载完毕省份之后要知道用户选择了那个省,那怎么实现呢?onchange()
                            $(area).append('<option value="' + e.areaid + '">' + e.areaname + '</option>');
                        })
                    }

                }
            )
        }
     /*   function showCity(selectId){
            $.ajax({
                    type: "GET",//get请求方式
                    url: "CityController.do?",//请求的servlet
                    data: "parentid="+selectId,//参数
                    dataType: "json",//返回的数据类型,这样就可以直接用了,不用转换了
                    success: function (info) {//后端返回来的信息
                        $("#city").html("<option>--请选择--</option>");
                        $.each(info, function (i, e) {
                            // 加载完毕省份之后要知道用户选择了那个省,那怎么实现呢?onchange()
                            $("#city").append('<option value="' + e.areaid + '">' + e.areaname + '</option>');
                        })
                    }

                }
            )

            //alert(selectId);
        }
*/
     /*   function showRegion(selectId){
            $.ajax({
                    type: "GET",//get请求方式
                    url: "CityController.do?",//请求的servlet
                    data: "parentid="+selectId,//参数
                    dataType: "json",//返回的数据类型,这样就可以直接用了,不用转换了
                    success: function (info) {//后端返回来的信息
                        $("#region").html("<option>--请选择--</option>");
                        $.each(info, function (i, e) {
                            // 加载完毕省份之后要知道用户选择了那个省,那怎么实现呢?onchange()
                            $("#region").append('<option value="' + e.areaid + '">' + e.areaname + '</option>');
                        })
                    }

                }
            )

            //alert(selectId);
        }*/
    </script>
</head>
<body>
<%--<select id="province" onchange="showCity(this.value)">
    <option>--请选择--</option>

</select>
<select id="city" onchange="showRegion(this.value)">
    <option>--请选择--</option>
</select>
<select id="region">
    <option>--请选择--</option>
</select>--%>

<select id="province" onchange="showArea(this.value,'#city')">
<option>--请选择--</option>
</select>
<select id="city" onchange="showArea(this.value,'#region')">
    <option>--请选择--</option>
</select>
<select id="region">
    <option>--请选择--</option>
</select>
</body>
</html>
```

后端代码就是简单的查询数据库，然后传到前端，不一一展示了；

Mybatis项目源码:

- [三级联动resource下载](https://github.com/JeEisenberg/JeEisenberger)
- [tomcat异常实录](/tomcat/tomcatexception.md#tomcat异常实录)

<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/#为什么会有这个网站)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/spring/SpringMvc.md#springmvc)

</center>

![](..\pic\div\weichatcode.png)