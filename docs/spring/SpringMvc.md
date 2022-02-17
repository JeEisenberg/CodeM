![](..\pic\logo2.png)

# SpringMvc

如果你还不会spring请移步以下链接

[Spring 基础入门](https://blog.csdn.net/weixin_54061333/article/details/121455205) 

如果你还不会mybatis,请移步,请移步以下链接

[Mybatis基础入门](https://blog.csdn.net/weixin_54061333/article/details/121450470)

假如你是大神,请悄悄看完,就当是复习了,如果文章中恰巧有什么跟你的认知有冲突的地方,还请指教！

## SpringMvc的优势:
Spring MVC是Spring提供的一个轻量级Web框架，它实现了我们在tomcat时的WebMVC设计模式。

Spring MVC在使用和性能等方面要比较优秀
他灵活性强,易于与其他框架集成,同时xml配置文件的修改不需要重新编译应用和层序;

所以spring的基本框架在为微服务中很 流行;

要注意一点在环境搭建的时候


# Spring MVC应用
在开始之前我们假定你已经有了Spring与mybatis基础,并且能理解tomcat的工作模式;

在具体的代码实现之前我们要先了解一下springmvc框架的执行流程,就像我们学习tomcat一样;

我们之前学习过MVC模式,
MVC模式大体上是这个样子的------>>>




## MVC模式
一个简单的表格是这么创建的：
| 层               | 含义                                                         |
| ---------------- | ------------------------------------------------------------ |
| M 即model层      | Dao层的封装,像我们的数据库,我们将数据库中的表封装为一个实体类,然后通过Dao层去调用Service层(当然如果项目很小的话可以不用这个service层); |
| V  即view层      | 用于前端展示的想什么html,jsp,php等                           |
| C 即Controller层 | 即servlet封装,用于业务逻辑的处理                             |

SpringMvc模式即通过Spring将MVC及进行整合---


| 层   | 含义         |
| ---- | ------------ |
| M层  | mybatis      |
| V层  | html,jsp,php |
| C层  | SpringMVC    |

SpringMVC是spring为展现层提供的基于MVC设计理念的优秀WEB框架,是目前最主流的MVC框架之一
SpringMVC通过一套注解,可以让普通的JAVA类成为contrllor控制器,无需继承Servlet,实现了控制层和Servlet之间的解耦
SpringMVC支持Rest风格的URL写法,可以增强url的安全性
SpringMVC采用了松耦合,可热插的主键结构,比其他的框架更具扩展性和灵活性





## 创建项目

### 项目的目录结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/155737a93f6f4b3fb495f7ef6e0294f9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
注意,由于创建时找的一个maven-webapp股价创建的,发现当前的web.xml配置有一些过时,
过时的web.xml配置------------>>>
web--2.0
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4"
         xmlns="http://java.sun.com/xml/ns/j2ee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee
         http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

```
如果用比较新的jar包来运行,可能会有一些不兼容的问题

这里建议修改为新的配置------>>>
web--4.0

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
</web-app>
```

### 基础JAR包引入

这一步凭借maven可以快完成jar包依赖的导入;
回顾一下spring的核心jar包----->>

core ,beans,context ,expression

然后引入sprinmvc核心jar包
 spring-web和spring-webmvc

如果要打印日志还需要 log的jar包
```xml
        <!--        SPRING    -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-expression</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>


        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>

        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>

        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.14.1</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>compile</scope>
        </dependency>


        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
            <scope>provided</scope>
        </dependency>


        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>
```

编写一个控制层---->>

在这里回顾一下传统的控制层该怎么写----->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/82d639e6fc6040faa660bf4b1fd6def9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然后请求转发到welcome.jsp中,如果之前需要做处理还需要配置过滤器等等
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd07ef054aea44b0a48d8b5e339b3649.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


在springmvc模式下该怎么写呢???


实现Springmvci相关jar包中的Controller接口---->>>
import org.springframework.web.servlet.mvc.Controller;

```java
public class ControllerTest implements Controller {

    @Override
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {

      ModelAndView modelAndView= new ModelAndView();
      modelAndView.addObject("msg","Welcome to Yantai!");
      modelAndView.setViewName("welcome.jsp");
        return modelAndView;
    }
}

```


可以看到在控制层中将model层和view层结合到一块,

控制层出处理请求,并向指定地址返回一个modelAndView对象,这样请求就会被转到发到welcome.jsp;

你可能会发现Sringmvc模式下与servlet的一些小区别----
控制层怎么定位到?
本质上都要定位到控制层,只不过这次是通过spring与web来完成控制层的定位


在resource下新建一个配置文件---->>>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

<!--   配置控制层实例-->
   <bean name="/controllerTest" class="com.gavin.Controller.ControllerTest"/>
<!--   处理url-->
   <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<!--控制适配器-->
   <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
<!--视图层处理-->
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"/>

</beans>
```
然后在web.xml中配置如下内容----->>>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
<!--        配置前端控制器-->
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!--       过滤器初始化参数  控制器位置-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-config.xml</param-value>
        </init-param>
<!--        启动时加载-->
        <load-on-startup>1</load-on-startup>
    </servlet>
<!--    配置映射-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
<!--        过滤路径   拦截所有url并交由DispatcherServlet处理-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```



运行--->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/0db1b36f40674268b904f887e29ec87c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 在老版本的Spring中，配置文件内必须要配置处理器映射器、处理器适配器和视图解析器。但在Spring 4.0以后，如果不配置处理器映射器、处理器适配器和视图解析器，就会使用Spring内部默认的配置来完成相应的工作;
>



SpringMvc的工作流程----->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/9a8232f6aada40089d522e4ed0f13c17.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
在springmvc的流程中我们不需要关心DispatcherServlet、HandlerMapping、HandlerAdapter和ViewResolver对象是怎么工作的,我们只需要配置好前端控制器完成业务逻辑就行了;

注:实际上在spingmvc配置文件中只需要配置
控制层的bean就可以以了;
**spingmvc配置文件**
![在这里插入图片描述](https://img-blog.csdnimg.cn/28ae480e14cc49e2945ba885e3f9bbde.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/91f7e81b4c8d4ffb866c141f9c90c444.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


在spring2.5之前的版本中只能通过配置xml文件来完成控制层的定位,但是在这之后可以通过注解的方式来实现定位;

通过注解的方式完成控制层的定位


## 注解实现控制层

### @Controller注解

```java
import org.springframework.stereotype.Controller;

@Controller
public class ControllerTest  {


}

```

这样通过注解的方式找到控制器后,还需要知道控制器内部对每个请求是如何 处理的;

### @RequestMapping

```java
@Controller
public class ControllerTest {
    //   @RequestMapping 当标注在一个方法上时，该方法将成为一个请求处理方法，
//    它会在程序接收到对应的URL请求时被调用。
    @RequestMapping(value = "/controllerTest")
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "你好Plus");
        modelAndView.setViewName("welcome.jsp");
        return modelAndView;
    }
}

```



xml与注解对比-----<>

首先看一下逻辑----->>
基本逻辑一样


![在这里插入图片描述](https://img-blog.csdnimg.cn/43807cfdf7214aba82b027bf7fbcd610.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/0044dfacd3fb4b8291098b215c49d0d2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

除了 @RequestMapping 标注在Controller的方法上,还可以标注在类上,


这表示该类下所有的方法都为请求映射方法,该类中的所有方法都将映射为相对于类级别的请求,即所有的请求都被映射到value属性所指定的的路径下;
### 定义多个处理方法


```java
@Controller
@RequestMapping(value ="/controll")
public class ControllerTest {
    //   @RequestMapping 当标注在一个方法上时，该方法将成为一个请求处理方法，
//    它会在程序接收到对应的URL请求时被调用。
    @RequestMapping(value="/controllerTest1.do")
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "你好Plus");
        modelAndView.setViewName("../welcome.jsp");
        return modelAndView;
    }

    @RequestMapping(value="/controllerTest2.do")
    public ModelAndView handleRequest1(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        String username = httpServletRequest.getParameter("username");

        if ("gavin".equals(username)) {
            PrintWriter writer = httpServletResponse.getWriter();
            writer.println("你好,欢迎来到这里");
        }
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "你好,欢迎来到这里");
        modelAndView.setViewName("../welcome.jsp");
        return modelAndView;
    }


}
```

部署后请求------>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/8a9b7a7c9ee54df8b3780107d502dcb3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以看到请求路径上多了一层controll   
    `http://localhost:8080/myweb1_war_exploded/controll/controllerTest2.do`


@RequestMapping注解除了基本的--默认为
value属性还可以指的别的属性
name---为地址取一个别名
method---请求方式
params ---指定请求时的参数,只有包含该参数才可以通过处理;
headers---指定请求时包含的header
consumes -----指定请求的提交内容的类型
produces---指定返回的内容类型

## 组合注解

requsetmapping可以只指定请求的方式,当然在注解时可以直接用请求方式注解来实现一样的功能;



```java
@Controller
public class ControllerTest {
    //   @RequestMapping 当标注在一个方法上时，该方法将成为一个请求处理方法，
//    它会在程序接收到对应的URL请求时被调用。
    @GetMapping(value = "/controllerTest")
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "你好Plus");
        modelAndView.setViewName("welcome.jsp");
        return modelAndView;
    }
}
```
除了getMapping还有其他请求方式----
　    @GetMapping：匹配GET方式的请求。　
　    @PostMapping：匹配POST方式的请求。
　　@PutMapping：匹配PUT方式的请求。　
　　@DeleteMapping：匹配DELETE方式的请求。　  
　　  @PatchMapping：匹配PATCH方式的请求

设置含有指定的请求参数时才可以生效的方法----->>>

```java

@Controller
public class ControllerTest {
    //   @RequestMapping 当标注在一个方法上时，该方法将成为一个请求处理方法，
//    它会在程序接收到对应的URL请求时被调用。
    @GetMapping(value = "/controllerTest" ,params = "name=gavin")
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "你好Plus");
        modelAndView.setViewName("welcome.jsp");
        return modelAndView;
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b33447529f24df6a6660eb90185fdbd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


> 注意org.springframework.ui.Model类型不是一个Servlet API类型，而是一个包含Map对象的Spring MVC类型。如果方法中添加了Model参数，那么每次调用该请求处理方法时，Spring MVC都会创建Model对象，并将其作为参数传递给方法。
>
>
> 请求处理方法还可以返回其他类型的数据。
> Spring MVC所支持的常见方法返回类型如下。
> ModelAndView　
> Model　
> Map　
> View　
> String
> void
> HttpEntity<?>或ResponseEntity<?>　
> Callable<?>　
> DeferredResult<?>


常见的返回类型有ModelAndView、String和void。
ModelAndView类型中可以添加Model数据，并指定视图；
String类型的返回值可以跳转视图，但不能携带数据；
而void类型主要在异步请求时使用，它只返回数据，而不会跳转视图。



当我们没有参数要传递时,可以用以下方法更加迅速---->>>>
定义一个返回值为String的方法
```java
@Controller
public class ControllerTest {
    //   @RequestMapping 当标注在一个方法上时，该方法将成为一个请求处理方法，
//    它会在程序接收到对应的URL请求时被调用。
    @GetMapping(value = "/controllerTest.do")
    public String handleRequest()  {
     return "welcome.jsp";
    }
}
```
这时候如果要传递一些信息可以通过Model
的参数类型来传递

```java
@Controller
public class ControllerTest {
    //   @RequestMapping 当标注在一个方法上时，该方法将成为一个请求处理方法，
//    它会在程序接收到对应的URL请求时被调用。
    @GetMapping(value = "/controllerTest.do")
    public String handleRequest(Model model)  {
        model.addAttribute("msg","世界那么大");
     return "welcome.jsp";

    }
}
```
**如果方法中添加了Model参数，那么每次调用该请求处理方法时，Spring MVC都会创建Model对象，并将其作为参数传递给方法。**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2fa14692248a46e3beee49774f190830.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
整合案例----->>>

### forward 与 redirect

请求转发与响应重定向
```java
@Controller
public class ControllerTest {
    //   @RequestMapping 当标注在一个方法上时，该方法将成为一个请求处理方法，
//    它会在程序接收到对应的URL请求时被调用。
    @GetMapping(value = "/controllerTest.do")
    public String handleRequest(HttpServletRequest req,HttpServletResponse resp,Model model)  {
        System.out.println(req);
        String name = req.getParameter("name");
        String pwd = req.getParameter("pwd");
        User user = new User();
        user.setName(name);
        user.setPwd(pwd);
        model.addAttribute("msg",user);
     return "forward:welcome.jsp";
//return "redirect:welcome.jsp";
    }
}
```

## 视图解析器
在没使用注解的时候我们在xml中配置了一个视图解析器,

![在这里插入图片描述](https://img-blog.csdnimg.cn/819bf7b4b9304fa0ae7f11e51cf81ebb.png)
视图解析器有什么作用呢?
我们可以在定义多个方法时----即类上加requestMapping(getMapping/postMapping都可以)时 设置访问前缀与后缀

```java

    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="../"/>
        <property name="suffix" value=".jsp"/>


    </bean>
```
案例应用------>>


```java

@Controller
@RequestMapping(value ="/controll")
public class ControllerTest {
    //   @RequestMapping 当标注在一个方法上时，该方法将成为一个请求处理方法，
//    它会在程序接收到对应的URL请求时被调用。
    @RequestMapping(value="/controllerTest1.do")
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "你好Plus");
        modelAndView.setViewName("welcome");
        return modelAndView;
    }

    @RequestMapping(value="/controllerTest2.do")
    public ModelAndView handleRequest1(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        String username = httpServletRequest.getParameter("username");

        if ("gavin".equals(username)) {
            PrintWriter writer = httpServletResponse.getWriter();
            writer.println("你好,欢迎来到这里");
        }
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "你好,欢迎来到这里");
        modelAndView.setViewName("../welcome.jsp");//这个是为了对比
        return modelAndView;
    }


}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/9ef6d670a72d4f20a416f75bf238c243.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这样设置后，方法中所定义的view路径将可以简化,在访问时视图解析器会自动地增加前缀和后缀。


## Springmvc中的数据绑定

在处理请求时我们会遇到各种各样的请求,springmvc框架是如何绑定并获取请求的呢?


试想一下,当我们编写springmvc程序时我们知道输入的什么参数类型能得到什么样子的反馈,但是若换做他人,该怎么 处理呢?

> 在执行程序时，Spring MVC根据客户端请求参数的不同将请求消息中的信息以一定的方式转换并绑定到控制器类的方法参数中

### 简单数据绑定
简单数据绑定包括绑定**默认数据类型**、**绑定简单数据类型**、**绑定POJO类型**、**绑定包装POJO**等。

常用的默认参数类型

HttpServletRequest：获取请求信息。　
HttpServletResponse：处理响应信息。　
HttpSession：得到session中存储的对象。　
Model/ModelMap：作用是将Model数据填充到request域。

**绑定默认数据类型**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f6584b7c01804f008fb0241b6ee12689.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**绑定简单数据类型**

基本数据类型及其包装类

![在这里插入图片描述](https://img-blog.csdnimg.cn/c399ccd4fc12460b85dde79f9fd33efa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
有时候定义请求的参数与前端参数不一样,这时候可以通过@RequestParam来绑定一个别名

```java
@Controller
@Transactional(propagation = Propagation.REQUIRED, isolation = Isolation.DEFAULT)
public class ControllerDemo1 {
    /**
     * @return 
     */
    @RequestMapping(value ="/controller1.do" ,method= RequestMethod.GET)
    public Object searchBookById(Integer bid, @RequestParam(value = "book_id") Integer id, Model model) {
model.addAttribute("msg",id);
model.addAttribute("message",bid);
//        参数处理
        return "book.jsp";

    }
}

```
book.jsp
```html
<%@ page contentType="text/html;charset=UTF-8" language="java"  pageEncoding="UTF-8" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${bookInfo}</br>

你好</br>

${msg}


${message}
</body>
</html>

```
可以看到没有绑定的是得不到请求参数的;------>>
将请求中book_id参数的值1赋给方法中的id形参。这样通过输出语句就可以输出id形参中的值。
![ook](https://img-blog.csdnimg.cn/fe57316cf7f049359d552a6e261f0d5a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

 **绑定pojo**

当我们查询多个参数时,可以将这些参数封装到一个pojo类中,这样就减少了一些操作;

例如

下面这个案例----注册页面

```java


    @Autowired
    private SqlSessionFactory sqlSessionFactory;

    @Autowired
    @Qualifier(value = "sqlSessionFactory")
    public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
        this.sqlSessionFactory = sqlSessionFactory;
    }

    @RequestMapping(value = "/register.do")
    public String register() {


        return "register.jsp";
    }

    @RequestMapping("/RegisterUser.do")
    public Object regUser(User user) {//参数为 user类对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.addUser(user);
        ModelAndView modelAndView=new ModelAndView();
        modelAndView.addObject("user",user.getName());
        modelAndView.setViewName("welcome.jsp");
        return modelAndView;
    }
}
```
注册页面---->>


```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<meta charset="UTF-8">
<head>
    <title>Title</title>
</head>
<body>
<form action="${pageContext.request.contextPath}/RegisterUser.do" method="post">
    账号:<input type="text" name="name" placeholder="请输入用户名"/>
    密码:<input type="password" name="pwd" placeholder="请输入账号密码"/>
    <input type="submit" value="注册"/>
</form>
</body>
</html>
```

跳转到的页面---->>

```html
<%@ page contentType="text/html;charset=UTF-8" language="java"  pageEncoding="UTF-8" %>
<html>
<meta charset="UTF-8">
<head>
    <title>Title</title>
</head>
<body>

你好${user}

</body>
</html>

```


注册之后的页面---->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/180fd1633dcd4d48a471f0e500172a58.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
还记得怎么处理这个的吗??

几种方式----
1,在回应的时候处理,
2,在前端加一个过滤器,进行转码处理;

所以此时适用于第二种

在web配置文件中添加过滤器

自己写的话,实现filter接口.......
如果真的有一些要自定义过滤的话.....
在框架中早就帮我们做好了


```xml
<filter>
    <filter-name>CharacterEncoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
再次测试---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/29439c4d98054205a0b58a925b6dcfb6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

[另请参考web中的过滤器------](https://blog.csdn.net/weixin_54061333/article/details/120947211)

**包装类Pojo**
所谓包装类pojo就时一个类中包含了另一个类的信息,类似于mybatis中一对一或者一对多那样;

来一个一对一案例-----...

[源码](https://download.csdn.net/download/weixin_54061333/66175158)见链接----


## 复杂数据绑定
**数组**

举个不太恰当的例子----加入要注销员工-----即删除多个员工

映射文件---

```xml
    <delete id="delEmpByempno" >
        delete from gavin.emp where EMPNO in
        <foreach collection="array" index="index"
                 item="item" open="(" separator="," close=")">
          #{item}
        </foreach>
    </delete>

```

```java


    @RequestMapping("/toDel.do")
    public String toDel(){
        return "delemp.jsp";
    }
    @RequestMapping("/delEmp.do")
    @Transactional(propagation = Propagation.REQUIRED ,isolation = Isolation.DEFAULT)
    public Object delEmp(@RequestParam("ids") Integer []ids,Model model){
        for (Integer id : ids) {
            System.out.println(id);
        }
        SqlSession sqlSession = sqlSessionFactory.openSession();
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
        int i = mapper.delEmpByempno(ids);
model.addAttribute("delInfo",i);
        return "delInfo.jsp";
    }
```
前端页面
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8" %>
<html>
<meta charset="UTF-8"/>
<head>
    <title>Title</title>
</head>
<body>
<form method="get" action="${pageContext.request.contextPath}/delEmp.do">

    <input type="checkbox" name="ids" value="7369">SMITH<br>
    <input type="checkbox" name="ids" value="7499">ALEEN<br>
    <input type="checkbox" name="ids" value="7521">WARD<br>
    <input type="submit" value="提交">

</form>

</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/61ad5670e6f44ae2a1a5d46e13da6384.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**集合**

> 前端请求传递过来的数据可能会批量包含各种类型的数据，如Integer、String等。这种情况使用数组绑定是无法实现的。针对这种情况，可以使用集合数据绑定


举个例子,假如我们要批量修改用户信息
好像字段太多,还是换部门表进行修改吧
加入要修改或者添加多个部门信息


准备方法---
![在这里插入图片描述](https://img-blog.csdnimg.cn/a3bf25b44f784681a1bfae079de198ec.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
但是发现前端无法绑定集合,那则么办呢?
只能去一个折中的方法

新建一个类用于包装一下集合
像这样

```java
import com.gavin.pojo.Dept;
import lombok.AllArgsConstructor;
import lombok.Data;

import java.util.List;
@Data
@AllArgsConstructor
public class addDeptInfo {
    private List<Dept> deptList;
}

```
映射接口就改为这样,以便用于接收集合
![在这里插入图片描述](https://img-blog.csdnimg.cn/532c8a88071a45a29cd35bf91d99f235.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
配置映射文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gavin.mapper.addDeptInfoMapper">
</mapper>
```

```xml
    <insert id="addDept" parameterType="addDeptInfo">
        insert into gavin.dept (deptno, dname, loc) values
        <foreach collection="deptList" item="dept" separator=",">

            (#{dept.deptno} ,#{dept.dname},#{dept.loc})

        </foreach>

    </insert>
```

写处理方法---

```java
    @RequestMapping("/toAdd.do")
    public String toAdd(){
        return "addDeptInfo.jsp";
    }

    @Transactional(propagation = Propagation.REQUIRED,isolation = Isolation.DEFAULT)
    @RequestMapping("/addDept.do")
    public Object addDept( addDeptInfo deptList,Model model){

        List<Dept> deptList1 = deptList.getDeptList();
//检查获得的参数
        for (Dept dept:deptList1
             ) {
            System.out.println(dept);
        }
        SqlSession sqlSession = sqlSessionFactory.openSession();
        DeptMapper mapper = sqlSession.getMapper(DeptMapper.class);
        int i = mapper.addDept(deptList);
        model.addAttribute("num",i);
        return "addSuccessInfo.jsp";
    }
```
测试代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/b78d6e0ac2df4f01b802b6963fe58f77.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

图解
![](C:\Users\Gavin\Pictures\Camera Roll\69.png)



## 静态资源放行
案例演示---->>
在请求资源时,

```java
    @RequestMapping("/toreq.do")
    public String toRequest(){
        return "toReq.jsp";
    }
```
 前端资源


```html
<%@ page contentType="text/html;charset=UTF-8" %>
<%@taglib prefix="fmt" uri="http://java.sun.com/jstl/fmt_rt" %>
<%@taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
<html>
<head>
    <title>欢迎</title>

</head>
<body>

<link rel="stylesheet" type="text/css" href="css/devc.css"/>
<h1>蒹葭</h1>

<dev id="devinfo" >
    <p id="devp" style="text-align: justify-all">
        蒹葭苍苍，白露为霜。所谓伊人，在水一方。溯洄从之，道阻且长。溯游从之，宛在水中央。<br>
        蒹葭萋萋，白露未晞。所谓伊人，在水之湄。溯洄从之，道阻且跻。溯游从之，宛在水中坻。<br>
        蒹葭采采，白露未已。所谓伊人，在水之涘。溯洄从之，道阻且右。溯游从之，宛在水中沚。<br>
    </p>

</dev>
<img src="img/jianjiacangcang.jpeg">

</body>
</html>
```
访问结果--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/1d2e5061f5ab4d8dbd5fdce1e6685d74.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


由于前端需要css文件和图片,但是被disparture拦截了,
![在这里插入图片描述](https://img-blog.csdnimg.cn/7d96a8994956497daac907872777476d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


什么原理呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/f379e447a5294368af45786df25c4892.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
怎么解决呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/ffc0fb8ec7ed42f2b1b567057ed398e2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
除此之外,还可以简化为将静态资源放在一个文件夹下,这样就不用写那么多放行的资源了;

例如----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/bcfd42c11d864234bdd72d49d47f9a74.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## JSON数据交互和RESTful支持
Spring MVC中的数据绑定需要对传递数据的格式和类型进行转换

### JSON数据交互


JSON与XML非常相似，都是用于存储数据的，但JSON相对于XML来说，解析速度更快，占用空间更小。

json是一种纯文本,比xml读写的速度更快
**JSON 语法规则**
JSON 语法是 JavaScript 对象表示语法的子集。

数据在键值对中对中
每个数据间用逗号分隔
用大括号 {} 保存对象
中括号 [] 保存数组，数组可以包含多个对象

key:value

json 值可以是 
数字,字符串,逻辑值,数组([]),对象({}),null
**数字:**
{"age":30}
对象:
{"name":"hello","url":"world"}

**数组**
数组可包含多个对象：
       {
        "jdbc":[
            {"driver":"com.mysql.cj.jdbc.Driver"},
            {"url":"jdbc:mysql://localhost:3306?gavin"},
            {"username":"gavin"},
            {"password":"123456"}
        ]
    }
这个表示一个数组对象,名为site,包含4个对象;



**逻辑值**
{"flag":true}
**null**
{"hello":null}


关键字必须为String类型的,值可以是任意数据类型;
JSON 使用 JavaScript 语法，所以无需额外的软件就能处理 JavaScript 中的 JSON。
json使用javaScript语法

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<script>
    var jdbc=

        [
            {"driver":"com.mysql.cj.jdbc.Driver"},
            {"url":"jdbc:mysql://localhost:3306?gavin"},
            {"username":"gavin"},
            {"password":"123456"}
        ];

 // 访问 JavaScript 对象数组中的第一项（索引从 0 开始）
function wwwww(){
    console.log(jdbc[0].driver);
}
</script>

<input type="button" onclick="wwwww()">

</body>
</html>
```
这样修改和查看

```html
console.log(jdbc[0].driver);
    jdbc[2].username="lim";
    console.log(jdbc[2].username);
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/eea57c8644fb428f95f2c10975d98868.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
json与xml表相同内容时候的写法对比

```xml
<jdbc>
    <driver>com.mysql.cj.jdbc.Driver
    </driver>
</jdbc>
    <jdbc>
        <url>com.mysql.cj.jdbc.Driver
        </url>
    </jdbc>
    <jdbc>
        <username>com.mysql.cj.jdbc.Driver
        </username>
    </jdbc>
    <jdbc>
        <password>com.mysql.cj.jdbc.Driver
        </password>
    </jdbc>
```

xml需要xml解析器来解析,json可以使用标准的javaScript来解析
    

> JSON.parse(): 将一个 JSON 字符串转换为 JavaScript 对象。 JSON.stringify():  JavaScript 值转换为 JSON 字符串。


案例开发--->>

java代码---->>

```java
@RequestMapping("/testJson.do")//这里映射加上
   @ResponseBody//先测试一下如果是简单的类型
    public User testJson(@RequestBody User user){

        System.out.println(user);
        return user;
    }
```
前端jsp
```html
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/12/21
  Time: 11:16
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8" %>

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transition//EN"
"http://www.w3.org/TR/html4/loose.dtd">

<html>

<head>

    <title>json数据交互</title>

</head>

<body>

<form>
    账号:<input type="text" name="name" id="name" placeholder="请输入账号"/><br>

    密码:<input type="password" name="pwd" id="pwd" placeholder="请输入密码"/><br>
    <input type="button" value="测试" onclick="testJson()"/><br>
</form>
<script type="text/javascript"
        src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js">

</script>
<script type="text/javascript">
    function testJson() {
        console.log("你好" + name + pwd)
        $.ajax({
                url: "${pageContext.request.contextPath}/testJson.do",
                type: "POST",
                contentType: "application/json ;charset=UTF-8",
                //  data: [{"name": $("#name").val()},{"pwd":$("#pwd").val()}],
                data: JSON.stringify({name: $("#name").val(), pwd: $("#pwd").val()}),
                //   data:{"name":name},
                dataType: "json",
                success: function (data) {
                    alert("Success")
                    alert(data.name + data.pwd)
                },
                err: function () {
                    alert("发生了错误");
                }
            }
        );
    }
</script>

</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4291c853d0f84cd486231ade1e66d928.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**Spring MVC除了支持JSON数据交互外，还支持RESTful风格的编程**


Spring MVC除了支持JSON数据交互外，还支持RESTful风格的编程
什么是restful风格?
简单来说就是将请求参数变为请求路径的一种,比如我们在查询


> http://localhost:8080/myweb2_war_exploded/searchByDeptNo.do?deptno=10


restful风格的路径是这么写的

> http://localhost:8080/myweb2_war_exploded/DeptNo.do/deptno=10



RESTful风格中的URL将请求参数id=1变成了请求路径的一部分，并且URL中的searchByDeptNo.do也变成了DeptNo.do（RESTful风格中的URL不存在动词形式的路径


话不多说直接上------------------------------>>>>

将原来的查询方法直接拿过来改一下------------->>

```java
    @RequestMapping(value ="/DeptNo.do/{deptno}",method = RequestMethod.GET)
    @ResponseBody
    public Object searchByDeptno1(@PathVariable("deptno") Integer deptno) {
        if (deptno != null) {
            SqlSession sqlSession = sqlSessionFactory.openSession();
            DeptMapper mapper = sqlSession.getMapper(DeptMapper.class);
            Dept dept = mapper.selectEmpBydeptno(deptno);
            sqlSession.close();
            return dept;
        }
        return null;
    }
```
前端页面--->>

```html
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/12/21
  Time: 11:16
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8" %>

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transition//EN"
"http://www.w3.org/TR/html4/loose.dtd">

<html>

<head>

    <title>json数据交互</title>

</head>

<body>

<form>
    部门号:<input type="text" name="deptno" id="deptno" placeholder="请输入部门号"/><br>

    <input type="button" value="查询" onclick="testJson()"/><br>
</form>
<script type="text/javascript"
        src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js">

</script>
<script type="text/javascript">

    function testJson() {
        var deptno= $("#deptno").val()
        console.log(deptno);
        $.ajax({
                url: "${pageContext.request.contextPath}/DeptNo.do/"+deptno,
                type: "get",
                contentType: "application/json ;charset=UTF-8",
                dataType: "json",
                success: function (data) {
                    alert("Success");
                  alert("查询的部门信息为:"+data.deptno+":"+data.dname+":"+data.loc+data.empOfDept );
                },
                err: function () {
                    alert("发生了错误");
                }
            }
        );
    }
</script>

</body>
</html>

```
结果如下:
![在这里插入图片描述](https://img-blog.csdnimg.cn/8ea92326038c4988bb304ca69ad47609.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/11a4785c63514385925a4626ccc44d00.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

restful对不同请求方式的识别



```java


import org.springframework.web.bind.annotation.*;

@RestController
public class testController {
    @GetMapping(value = "/toInfo.do/{id}")
    public String toGET(@PathVariable("id") Integer id){
        System.out.println("get"+id);
        return "GetSuccess";
    }
    @PostMapping("/toInfo.do/{id}")
    public String toPost(@PathVariable("id") Integer id){
        System.out.println("post"+id);
        return "PostSuccess";
    }
    @PutMapping("/toInfo.do/{id}")
    public String toPut(@PathVariable("id") Integer id){
        System.out.println("put"+id);
        return "PutSuccess";
    }
    @DeleteMapping("/toInfo.do/{id}")
    public String toDel(@PathVariable("id") Integer id){
        System.out.println("del"+id);
        return "DelSuccess";
    }
}


```

前端

```html
--%>
<%@ page contentType="text/html;charset=UTF-8" %>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>


<body>
<form method="get" action="toInfo.do/10" >
    id号<input type="text" name="id" placeholder="请输入账号"/><br>
    <input type="submit" value="toget" />

</form>
<form method="post" action="toInfo.do/10" >
    id号<input type="text" name="id" placeholder="请输入账号"/><br>
    <input type="submit" value="topost" />

</form>
<form method="post" action="toInfo.do/10" >
    id号<input type="text" name="id"   placeholder="请输入账号"/><br>
    <input type="hidden" name="_method" value="PUT"/>
    <input type="submit" value="toput" />

</form>
<form method="get" action="toInfo.do/10" >
    id号<input type="text" name="id"    placeholder="请输入账号"/><br>
    <input type="hidden" name="_method" value="DELETE"/>
    <input type="submit" value="todel" />

</form>

</body>
</html>

```


总结一下运行逻辑
![在这里插入图片描述](https://img-blog.csdnimg.cn/bceb5f30381a4805b8ddae9de84a10fe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

我们知道前端提交时没有put和del的显式方式,那么表示相应的请求方式呢?restful风格又是怎么识别这些请求方式的呢?

springmvc中提供了一个过滤器专门来处理请求
```xml
<filter>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
HiddenHttpMethodFilter中的方法----->>
```java

	private static final List<String> ALLOWED_METHODS =
			Collections.unmodifiableList(Arrays.asList(HttpMethod.PUT.name(),
					HttpMethod.DELETE.name(), HttpMethod.PATCH.name()));

	/** Default method parameter: {@code _method}. */
	public static final String DEFAULT_METHOD_PARAM = "_method";

	private String methodParam = DEFAULT_METHOD_PARAM;


	/**
	 * Set the parameter name to look for HTTP methods.
	 * @see #DEFAULT_METHOD_PARAM
	 */
	public void setMethodParam(String methodParam) {
		Assert.hasText(methodParam, "'methodParam' must not be empty");
		this.methodParam = methodParam;
	}

	@Override
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException {

		HttpServletRequest requestToUse = request;
//首先判断是否是post,然后提交时当中包含"_method" 然后根据value之来识别相应的请求方式
		if ("POST".equals(request.getMethod()) && request.getAttribute(WebUtils.ERROR_EXCEPTION_ATTRIBUTE) == null) {
			String paramValue = request.getParameter(this.methodParam);
			if (StringUtils.hasLength(paramValue)) {
				String method = paramValue.toUpperCase(Locale.ENGLISH);
				if (ALLOWED_METHODS.contains(method)) {
					requestToUse = new HttpMethodRequestWrapper(request, method);
				}
			}
		}

		filterChain.doFilter(requestToUse, response);
	}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/4b4d967f34a041de9216a1bc791b23c6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然后开就可以识别put与delete两种请求方式了;



[源码打包下载----->>>](https://github.com/JeEisenberg/springmvc.git)

## springmvc中的拦截器
主要用于拦截用户请求,并对请求做相应的处理

拦截器的实现方式-----类似于javaweb中的过滤器,实现接口或者继承接口实现类;

```
　一种是通过实现 HandlerInterceptor接口或者继承HandlerInterceptor接口的实现类（如HandlerInterceptorAdapter）来定义。
　另一种是通过实现 WebRequestInterceptor接口或继承WebRequestInterceptor接口的实现类来定义。
```
###  过滤器的定义

```java
package com.gavin.InterCeptor;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AllInterceptor implements HandlerInterceptor {

        /**
         * @param request  请求
         * @param response 回应
         * @param handler  handler选择要执行的处理程序，用于类型和/或实例计算
         * @return 返回false, 则不会向下继续执行 默认是不做拦截处理的
         * @throws Exception
         * @desc 该方法会在控制器方法前执行，其返回值表示是否中断后续操作
         */


        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            return false;
        }


        /**
         * @param request      请求
         * @param response     回应
         * @param handler      handler选择要执行的处理程序，用于类型和/或实例计算
         * @param modelAndView 处理程序返回的modelAndView
         * @throws Exception
         * @desc 该方法会在控制器方法调用之后，且解析视图之前执行。
         * 可以通过此方法对请求域中的模型和视图做出进一步的修改
         */
        @Override
        public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

        }

        /**
         *    *@desc 该方法在整个请求完成，即视图渲染结束之后执行。
         *    可以通过此方法实现一些资源清理、记录日志信息等工作。
         * @param response 当前HTTP响应
         * @param handler 启动异步的处理程序执行，用于类型和/或实例检查
         * @param请求当前HTTP请求
         * @exception
         */
        @Override
        public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

        }
}

```
看一下Api会发现,接口中的访问为default即可以去实现也可以不去实现;

下面模拟一下过滤器中方法执行顺序
首先主备一个controler层的方法

```java
    @RequestMapping("/testInterceptor.do")
    public String testInterceptor(){
        System.out.println("hello world");
        return "WEB-INF/1234.jsp";
    }

```
准备过滤器
在配置文件添加过滤器------springmvc.xml中
```xml
        <mvc:interceptor>
            <mvc:mapping path="/toAdd.do"/>
           <bean class="com.gavin.InterCeptor.DeptInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```
执行后看结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/dea4d14b6f8044a6808043e3df7599b1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



由于没有对请求做任何处理,所以可以看到原生的结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/875f801154704abe8c602d186887cfce.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

拦截器配置模板--->>

```xml  
  <!--模板过滤器 -->
 <!--   <mvc: interceptor>中的子元素必须按照上述代码的配置顺序进行编写，
    即<mvc: mapping…/>→<mvc: exclude-mapping…/>→<bean…/>的顺序，否则文件会报错。
-->
    <mvc:interceptors>
<!--        配置拦截器  全局拦截器 拦截所有的请求-->
        <bean class="com.gavin.InterCeptor.AllInterceptor"/>
        <mvc:interceptor>
<!--            配置作用范围-->
            <mvc:mapping path="/dataOne/**"/>
<!--            配置例外情况 如果配置了,那么path不能为空-->
            <mvc:exclude-mapping path="/interceptorDemo/**"/>
<!--            对匹配路径的请求才拦截-->
            <bean class="com.gavin.InterCeptor.EmpInterceptor"/>
        </mvc:interceptor>

        <mvc:interceptor>
            <mvc:mapping path="/toAdd.do"/>
           <bean class="com.gavin.InterCeptor.DeptInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>


```

### 多个拦截器
多个拦截起的情况下是怎样一种执行顺寻呢?
配置两个拦截器---在上述一个拦截器的基础上

第一种情况----
两个拦截器对同一个请求路径进行拦截----

![在这里插入图片描述](https://img-blog.csdnimg.cn/9d91d63293a4411e919333298f290416.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)执行结果--->
![在这里插入图片描述](https://img-blog.csdnimg.cn/7c586bef1f114248a558777d37cf378c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
颠倒拦截器顺序后--->

![在这里插入图片描述](https://img-blog.csdnimg.cn/3d8720af39214f2a8bccc23084fafe24.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/278947fdc78840338885c5f9c1c89e25.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以看到执行时的顺序---->>
![](C:\Users\Gavin\Pictures\Camera Roll\68.png)



当然这种拦截根本用不到,谁会用两个拦截器拦截同一个请求呢...闲的,只是做个实验

第二种情况---
实际用到的情况是一个全局拦截器,和一个局部拦截器

![在这里插入图片描述](https://img-blog.csdnimg.cn/4edc6122ae294a09b8f031b8d76387ec.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e8a924d351848d8bb5d2f704eda3811.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
哈哈,跟定义第一种情况时候顺序一样

下面做一个小案例就是验证用户登录
这个跟之前javaweb中的过滤器一个道理



验证用户登录案例---->>

本来想写两种方式的,但是发现两种方式从内来讲用的是一个原理,

先写一个吧------>>

```java

import com.gavin.pojo.User;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class UserInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        System.out.println("preHandle--invoked");

//      检查登陆地址是否符合要求,不能直接去请求别的资源
        //要放行的请求资源
        String requestURI = request.getRequestURI();
//        放行一
        int i = requestURI.indexOf("/toLogin.do");
        if(i>=0){
            return true;
        }
//        放行二
        int i1 = requestURI.indexOf("/toLoginIn.do");
        if(i1>=0){
            return true;
        }
//还有一种情况 已经登陆了 放行三
        HttpSession session = request.getSession();
        User user =(User) session.getAttribute("user");
        if(null!=user){
            return true;
        }
//        剩下的直接拦截然后   登录页
        request.setAttribute("Info","请先登录!");
       response.sendRedirect("index.jsp");

        //request.getRequestDispatcher("index.jsp").forward(request,response);
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle--invoked");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion--invoked");
    }
}


```

```java
import com.gavin.mapper.UserMapper;
import com.gavin.pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@Controller
public class userController {

    @Qualifier
    private SqlSessionFactory sqlSessionFactory;

    @Autowired
    public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
        this.sqlSessionFactory = sqlSessionFactory;
    }

    SqlSession sqlSession = null;


    //    请求  跳转到登陆页面
    @RequestMapping("/toLogin.do")
    public Object toLogin() {
        return "index.jsp";
    }

    HttpSession session = null;

    //登录检查
    @RequestMapping(value = "/toLoginIn.do", method = RequestMethod.GET)
    public String login(HttpServletRequest req) {
//        如果user不为null,说明已经登陆过
        session = req.getSession();
        User user = (User)session.getAttribute("user");
        if(user!=null) {
            return "forward:/WEB-INF/welcome.jsp";
        }
//user 为null 则判断登录是否符合条件
        String name = req.getParameter("name");
        String pwd = req.getParameter("pwd");
    /*    System.out.println(name + pwd);*/

        sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
  /*      System.out.println(mapper);*/

        Integer integer = mapper.searchUser(name, pwd);
     /*   System.out.println(integer);*/

//        用户存在
        if (null != integer) {
           User userInfo = new User();
            userInfo.setName(name);
            userInfo.setPwd(pwd);
            this.session = req.getSession();
            this.session.setAttribute("user", userInfo);
            return "forward:/WEB-INF/welcome.jsp";

        }

        this.session.setAttribute("Info", "用户名或密码错误,请重新输入");
        sqlSession.close();
        return "redirect:index.jsp";
    }

    @RequestMapping("/logout.do")
    public String logout() {
        session.invalidate();
        return "index.jsp";
    }
```


前端页面

```html
<%@ page contentType="text/html;charset=UTF-8" %>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript"
            src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js">

    </script>

</head>


<body>
<form method="get" action="toLoginIn.do" >

    账号:<input type="text" name="name" id="username" placeholder="请输入账号"/> <dev>${Info}</dev><br>
    密码:<input type="password" name="pwd" id="userpwd" placeholder="请输入密码"/><dev>${Info}</dev><br>
    <input type="submit" value="提交" />

</form>
</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/459a7fc56a694233a1fae676839719bf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d6b766a580b4439299a2215dd09ff4de.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## Springmvc中的异常处理

在springweb中遇到异常我们可以通过web.xml配置异常时跳转到的页面

```html
 <error-page>
        <exception-type>java.lang.Exception</exception-type>
        <location>/static/info/failedPage.jsp</location>
    </error-page>
```
准备一个前端页面---->>

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8" %>

<html lang="en">
<meta charset="UTF-8"/>
<head>
    <meta charset="utf-8">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <style>
        html,
        body {
            height: 100%;
        }

        body {
            color: #444;
            font: 12px/1.5 'Helvetica Neue', Arial, Helvetica, sans-serif;
            background: #f2f2f2 url("${pageContext.request.contextPath}/static/info/img/blueprint.png") repeat 0 0;
        }

        div.da-wrapper {
            width: 100%;
            height: auto;
            min-height: 100%;
            position: relative;
            min-width: 320px
        }

        div.da-wrapper .da-container {
            width: 96%;
            margin: auto
        }

        div.da-content {
            clear: both;
            padding-bottom: 58px
        }

        @media only screen and (max-width:480px) {
            div.da-content {
                margin-top: auto
            }
        }

        div.da-error-wrapper {
            width: 320px;
            padding: 30px 0;
            margin: auto;
            position: relative
        }

        div.da-error-wrapper .da-error-heading {
            color: #e15656;
            text-align: center;
            font-size: 24px;
            font-family: Georgia, "Times New Roman", Times, serif
        }

        @-webkit-keyframes error-swing {
            0% {
                -webkit-transform: rotate(1deg)
            }

            100% {
                -webkit-transform: rotate(-2deg)
            }
        }

        @-moz-keyframes error-swing {
            0% {
                -moz-transform: rotate(1deg)
            }

            100% {
                -moz-transform: rotate(-2deg)
            }
        }

        @keyframes error-swing {
            0% {
                transform: rotate(1deg)
            }

            100% {
                transform: rotate(-2deg)
            }
        }

        div.da-error-wrapper .da-error-code {
            width: 285px;
            height: 170px;
            padding: 127px 16px 0 16px;
            position: relative;
            margin: auto auto 20px;
            z-index: 5;
            line-height: 1;
            font-size: 32px;
            text-align: center;
            background: url("${pageContext.request.contextPath}/static/info/img/error-hanger.png") no-repeat center center;
            -webkit-transform-origin: center top;
            -moz-transform-origin: center top;
            transform-origin: center top;
            -webkit-animation: error-swing infinite 2s ease-in-out alternate;
            -moz-animation: error-swing infinite 2s ease-in-out alternate;
            animation: error-swing infinite 2s ease-in-out alternate
        }

        div.da-error-wrapper .da-error-code .tip {
            padding-top: 2px;
            color: #e15656;
        }

        div.da-error-wrapper .da-error-code .tip2 {
            padding-top: 15px;
        }

        div.da-error-wrapper .da-error-code .tip3 {
            padding-top: 20px;
            font-size: 16px;
            color: #e15656;
        }

        div.da-error-wrapper .da-error-pin {
            width: 38px;
            height: 38px;
            display: block;
            margin: auto auto -27px;
            z-index: 10;
            position: relative;
            background: url("${pageContext.request.contextPath}/static/info/img/error-pin.png") no-repeat center center
        }

        p {
            margin: 0;
            padding: 0;
        }
    </style>
    <title>错误页面</title>
</head>

<body>
<div class="da-wrapper">
    <div class="da-content">
        <div class="da-container clearfix">
            <div class="da-error-wrapper">
                <div class="da-error-pin"></div>
                <div class="da-error-code">
                    <p class="tip">STATUS:403</p>
                    <p class="tip2">服务器开小差了</p>
                    <p class="tip3">You don't have permission to access the URL on this server.</p>
                </div>
                <h1 class="da-error-heading">Sorry, 请稍后再试 !!!</h1>
            </div>
        </div>
    </div>
</div>

</body>
</html>

```
准备后端页面

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ExceptionHandler;

@Controller("/test.do")
public class ExceptController {
 @RequestMapping("/test.do")
    public String test1(){
       return "/WEB-INF/ServiceData/success.jsp";
    }
    
}

```

访问test.do

![在这里插入图片描述](https://img-blog.csdnimg.cn/cced378e7fd54f6db1f729ec5246806e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

接着新建有几个带有错误的请求

```

    @RequestMapping("/test2.do")
    public String test2() {
        System.out.println(10 / 0);
        return "/WEB-INF/serviceData/success.jsp";
    }
   @RequestMapping("/test3.do")
    public String test3() {
       String str=null;
       System.out.println(str.length());
       return "/WEB-INF/serviceData/success.jsp";
    }

```
依次访问
![在这里插入图片描述](https://img-blog.csdnimg.cn/9073d80e95c1483f96d91abf7b69d1ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/1a907f6c9dab4dcfb752556d4e8d4182.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa264fcdd15742989de4c3a73a388a38.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_19,color_FFFFFF,t_70,g_se,x_16)

### 异常处理ExceptionHandler
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6fd20028b6b42669c19a9c34b544e76.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

下面注释掉web.xml中的error-page,定义一个异常处理的servlet

```java
    @ExceptionHandler(value = {NullPointerException.class ,ArithmeticException.class})
    public String test4() {

        return "/static/info/failedPage.jsp";
    }

```
再次请求-->
![在这里插入图片描述](https://img-blog.csdnimg.cn/926ddb448d7840729e4134093fec087f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/060b4192ab614783ab01df90105aa908.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以看到响应的代码并不一样.web.xml中的异常页面在发生异常时并没有及你才能够处理而是直接跳转到一个指定页面,

在加入了springmvc异常处理之后,是对异常进行处理,相当与一次正常响应;
SpringMVC中的异常----->>>
系统中异常包括两类：预期异常(检查型异常)和运行时异常 RuntimeException，前者通过捕获异常从而获取异常信息， 后者主要通过规范代码开发、测试通过手段减少运行时异常的发生。
系统的 dao、service、controller 出现都通过 throws Exception 向上抛出，最后由 springmvc 前端控制器交由异常处理器进行异常处理;
在开发中应尽可能在后端去处理一下异常,以便做出很好的应对;

error-page是tomcat去处理的,可作用于全局,也可指定异常
SpringMVC中的异常是由dispature去处理的;


使用@ExceptionHandler注解处理异常,只能处理当前Controller中的异常---即局部异常
如果想要扫描其他controller中的异常,这该怎么办呢?
下面就要用到ControllerAdvice了
## @ControllerAdvice
![在这里插入图片描述](https://img-blog.csdnimg.cn/a7b508bb76c54fd68b57b86b82134952.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


@ControllerAdvice配合  @ExceptionHandler完成异常的处理
```java
package com.gavin.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RequestMapping;

@ControllerAdvice
public class ExceptController1 {
    @ExceptionHandler(value = {NullPointerException.class ,ArithmeticException.class})
    public String test4() {
        return "/static/info/failedPage.jsp";
    }


}

```

```java
package com.gavin.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class ExceptController {
    //@ExceptionHandler
    @RequestMapping("/test.do")
    public String test1() {
        return "forward:/WEB-INF/serviceData/success.jsp";
    }

    @RequestMapping("/test2.do")
    public String test2() {
        System.out.println(10 / 0);
        return "/WEB-INF/serviceData/success.jsp";
    }
   @RequestMapping("/test3.do")
    public String test3() {
       String str=null;
       System.out.println(str.length());
       return "/WEB-INF/serviceData/success.jsp";
    }

/*注销掉,以免影响结论
    @ExceptionHandler(value = {NullPointerException.class ,ArithmeticException.class})
    public String test4() {

        return "/static/info/failedPage.jsp";
    }
*/


}

```
再次测试.......还有准备工作没做呢,要告诉他那些servlet是他管辖的

配置包扫描----使得在这个包下的异常都归其处理
```xml
<context:component-scan base-package="com.gavin.controller"/>
```
可在xml配置文件中进行配置,或者在ControllerAdvice注解上配置


![在这里插入图片描述](https://img-blog.csdnimg.cn/1d6a0e4d6bb044119b2dca989bee5101.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
将异常的类放在在com.gavin.controller包之外在测试一下


```java
package com.gavin.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RequestMapping;

@ControllerAdvice(basePackages = "com.gavin.controller")
public class ExceptController1 {

    @ExceptionHandler(value = {NullPointerException.class ,ArithmeticException.class})
    public String test4() {
           return "/static/info/failedPage.jsp";
    }
}
```
由于异常处理时扫描只扫这个包下的,所以...会报500错误;
注:web.xml中的error-page已开启
![在这里插入图片描述](https://img-blog.csdnimg.cn/0f113bd9bc6a43a9a6d4b6169addd314.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到在包之外的异常是管不到的;所以web.xml中配置的异常页起作用了;



当局部跟全局异常处理都存在时,执行的优先级是怎样的呢?
局部异常
![在这里插入图片描述](https://img-blog.csdnimg.cn/7fe6e88aca9b4b92bb3025b86dbd87df.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

全局异常
![在这里插入图片描述](https://img-blog.csdnimg.cn/595ef7aa7ce0421485c6b073ba7215dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)先走局部,
![在这里插入图片描述](https://img-blog.csdnimg.cn/916dc1f0d6ac4858a88b74e02ae22fec.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
局部无法处理,就会走全局,全局没法处理就可能会走error-page
![在这里插入图片描述](https://img-blog.csdnimg.cn/760ee8e2fa3e4088af387696a63cebba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




## 自定义异常处理
SimpleMappingExceptionResolver

conroller层

```
package com.gavin.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class ExceptController {
    //@ExceptionHandler
    @RequestMapping("/test.do")
    public String test1() {
        System.out.println(1233);
        return "forward:/WEB-INF/serviceData/success.jsp";
    }

    @RequestMapping("/test2.do")
    public String test2() {
        System.out.println(10 / 0);
        return "/WEB-INF/serviceData/success.jsp";
    }

    @RequestMapping("/test3.do")
    public String test3() {
        String str = null;
        System.out.println(str.length());
        return "/WEB-INF/serviceData/success.jsp";
    }


    //@ExceptionHandler(value = {ArithmeticException.class})
    public String test4() {

        return "/WEB-INF/serviceData/success.jsp";
    }


}

```

注销局部异常处理与全局异常处理
![在这里插入图片描述](https://img-blog.csdnimg.cn/3350ca35e4c146a99a3ecce0fd8047c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7d6e38d8851e4febb18572e2d58e6645.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 自定义映射简单异常处理类

```java

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.handler.SimpleMappingExceptionResolver;

import java.util.Properties;

@Configuration//配置类---如果选择这种,可以将xml中的部分去掉
public class simpleExceptionController {
@Bean     //这里得到一个简单异常映射处理类
    public SimpleMappingExceptionResolver  getSimpleMappingExceptionResolver(){
        SimpleMappingExceptionResolver simpleMappingExceptionResolver= new SimpleMappingExceptionResolver();
  Properties properties= new Properties();
  properties.put("java.lang.NullPointerException","/static/info/failedPage.jsp");
  properties.put("java.lang.ArithmeticException","/static/info/failedPage.jsp");
  simpleMappingExceptionResolver.setExceptionMappings(properties);
  return simpleMappingExceptionResolver;
    }
}

```
或者在spring配置文件中添加如下内容,以实现根据异常不同来跳转到不同的页面;
```xml
 <bean id="simpleExceptionController" class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <property name="exceptionMappings">
            <props>
                <prop key="java.lang.ArithmeticException">redirect:/static/info/failedPage.jsp</prop>
                <prop key="java.lang.NullPointerException">forward:/WEB-INF/serviceData/success.jsp</prop>
            </props>
        </property>
    </bean>
```
请求--->
test2.do
![在这里插入图片描述](https://img-blog.csdnimg.cn/5c264f49c0b244289533425f4afd7506.png)
test3.do
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c62aa89b8774c589f1d7078bc1ee71f.png)
当有多个异常,每个异常需要跳转到不同的页面,这个时候可以用自定义简单映射异常类来配置映射完成(定义配置类或者在xml中配置)

#### 自定义异常处理HandlerExceptionResolver
在简单异常处理上可以根据需要来定义处理方案;

实现HandlerExceptionResolver
```java
package com.gavin.controller;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
@Configuration
public class ExceptionHandlerController implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
      ModelAndView modelAndView=new ModelAndView();
      if (ex instanceof NullPointerException ){
          modelAndView.setViewName("/static/info/failedPage.jsp");
      }
      if (ex instanceof ArithmeticException){
          modelAndView.setViewName("forward:/WEB-INF/serviceData/success.jsp");
      }

        return modelAndView;
    }
}

```
根据异常不同跳转到不同的页面

![在这里插入图片描述](https://img-blog.csdnimg.cn/1e9a5f18df794aa19bab7daf8a95f29c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d4cd798df8624d9bb19bed91b1f400d6.png)


可以看到
1,简单映射异常处理类--------得到一个的简单映射异常实例,这个实例也是实现了HandlerExceptionResolver,然后通过setExceptionMappings();方法来添加异常映射关系.
![在这里插入图片描述](https://img-blog.csdnimg.cn/e462545f17da40449247b50980d6d36c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ff14a99afe8d44b09ad849e6554e7703.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

2,异常处理类---实现HandlerExceptionResolver


如果是配置类,那么就需要配置一下扫描的包;

![](C:\Users\Gavin\Pictures\Camera Roll\67.png)



# springmvc中请求参数的耦合

这里所说的是请求时的参数耦合
在javaweb我们常常用request对象来获得请求中的参数
如果request获得的参数名称不匹配,那么获得的参数就为null

比如下面的代码---->>
```java
package com.gavin.test;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@RestController
public class testController {
    @RequestMapping("/tolog.do")
    public String toLogin(HttpServletRequest request, HttpServletResponse resp) {
        String username = request.getParameter("username");
        String password = request.getParameter("password");
//        连接数据库查询
        System.out.println(username + "--" + password);
        return "success";
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/5ec56ba0be8a42c796a87783d3b5716e.png)
假如请求时参数不匹配,即我们的请求参数只能是username和password
![在这里插入图片描述](https://img-blog.csdnimg.cn/11a000f5408e49939231484bf7204ad8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

这时候如果改变就会得不到想要的参数;


所以需要一种方式来实现低耦合,不用request对象获得参数并能获得请求时的参数

```java
    @RequestMapping("/tologg.do")
    public String toLogin(String username, String password) {
     
//        连接数据库查询
        System.out.println(username + "--" + password);
        return "success";
    }
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/e2e2e4ca03ed4e41be0a8fc50fdc460d.png)

这里也是要求请求时的参数要和方法中的参数名一致,如果不一致---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/c7a8e199bf8945f49662568abf7fb706.png)

那怎么实现解耦合?


我们可以指定请求时的参数别名

```java
    @RequestMapping("/tologgg.do")
    public String toLog(@RequestParam("name") String username,@RequestParam("pwd") String password) {
//        连接数据库查询
        System.out.println(username + "--" + password);
        return "success";
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c7bab402839f463dbe6ffb6efa7c2cd1.png)

使用这种方式还可以自动实现类型的转换
![在这里插入图片描述](https://img-blog.csdnimg.cn/7a5cd5b623744dfcbb2e2d3d219c3a06.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
有时候接收前端的参数太多像这样

![在这里插入图片描述](https://img-blog.csdnimg.cn/5f767b4a6e18406c88079daaa27c7589.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)后端接收
![在这里插入图片描述](https://img-blog.csdnimg.cn/65168ad0d11c439ab26d57a77a90cd20.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这时候就要将参数封装为一个类来实现

## POJO类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class TPerson {
    String name;
    Integer age;
    String gender;
    String[] hobby;
    String birth;
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/dc550e70d2ea48a49397545c2ae25fef.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 这样如果参数发生了变化,我们需要修改TPerson类中的属性而不是方法


springmvc底层是通过getset方法来实现参数的解析---即反射的方式,所以类中必须要有无参构造方法;

![在这里插入图片描述](https://img-blog.csdnimg.cn/791c22fff016430aa13364a9155c1840.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这里会涉及到一个问题就是对于一些基本类型的转换是可以的,如果是日期类型的,那会提示转换失败
其实原因很简单,关于日期的转换格式不知道,所以,无法进行解析

## 日期类型转换

![在这里插入图片描述](https://img-blog.csdnimg.cn/7a410c1d7ded488ab6c98a54a8caf96d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

那如果放在实体类中

![在这里插入图片描述](https://img-blog.csdnimg.cn/a3e3f1fdcea24527b08bcfe5b6d10256.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
我们看到既然注解放在参数上时时可以识别日期,那同样我们将注解放到TPerson类中的属性上
![在这里插入图片描述](https://img-blog.csdnimg.cn/3cd0c1a25bbb475396cea21c686d9649.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

除了上述的注解转换方式,spring还提供了转换接口
![在这里插入图片描述](https://img-blog.csdnimg.cn/be3489535dd0444e81e167867a4cda09.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
import lombok.SneakyThrows;
import org.springframework.core.convert.converter.Converter;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

public class StringToDate implements Converter<String, Date> {
    SimpleDateFormat dateFormat= new SimpleDateFormat("yyyy-MM-dd");
    @SneakyThrows//偷偷默默的抛出异常的注解
    @Override
    public Date convert(String source) {
        Date date = dateFormat.parse(source);
        return date;
    }
}

```

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class TPerson {
    String name;
    Integer age;
    String gender;
    String[] hobby;
//    @DateTimeFormat(pattern = "yyyy-MM-dd")
    Date birth;
    @Override
    public String toString() {
        return "TPerson{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                ", hobby=" + Arrays.toString(hobby) +
                ", birth='" + birth + '\'' +
                '}';
    }
}

```
请求后-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/79ecfacb6b9e40c38d4220867d52be11.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
害,没有配置转换类,
在配置文件中配置转换类
先看一下要使用的接口




![在这里插入图片描述](https://img-blog.csdnimg.cn/a993f68ec11546bbaabd93713ea6460e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
再次请求后得到结果,
![在这里插入图片描述](https://img-blog.csdnimg.cn/99a5c6900cea4de2a8b89a4d57cb5517.png)



可以看到明明通过一个注解的方式解决,却要费尽心思用一个转换类加配置
类解决,显然不是一个方便的方式;




## List参数的解耦

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<form method="post" action="dodo.do">
    名称:<input name="fruitList[0].fruitName"></br>
    种类:<input name="fruitList[0].fruitType"></br>

    名称:<input name="fruitList[1].fruitName"></br>
    种类:<input name="fruitList[1].fruitType"></br>
    <input type="submit" value="提交">
</form>

</body>
</html>

```
后端--->

```java
package com.gavin.pojo;

import java.io.Serializable;
import java.util.Date;
import java.util.List;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * t_user
 * @author 
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class TUser implements Serializable {
    private Integer userId;

    private String userName;

    private String loginName;

    private String password;

    private Integer roleId;

    private String tel;

    private Date registerTime;

    private String status;

    /**
     * 一个用户对应一个角色
     */
    private TRole tRole;
    private static final long serialVersionUID = 1L;


    private List<Fruit> fruitList;
}
```

```java
    @RequestMapping("/dodo.do")
    public String dododo(TUser tUser){

        System.out.println(tUser.getFruitList());
        return "success";
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e523a3ad336a4c48be554e438876fd99.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 这里相当于遍历集合中的元素,按照下标的形式进行装配

## map类型参数

跟list一样,

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<form method="post" action="dodo.do">
    名称:<input name="fruitCount['fruit'].fruitName"></br>
    种类:<input name="fruitCount['fruit'].fruitType"></br>

    名称:<input name="fruitCount['1'].fruitName"></br>
    种类:<input name="fruitCount['1'].fruitType"></br>
    <input type="submit" value="提交">
</form>

</body>
</html>

```
后端

```java
    @RequestMapping("/dodo.do")
    public String dododo(TUser tUser){

        System.out.println(tUser.getFruitCount());
        return "success";
    }
```

只要满足键值对关系就可以了


## restful风格的参数
说到这个restful风格,说起来也和常见,比如
链接一下baidu.com
![在这里插入图片描述](https://img-blog.csdnimg.cn/e42b30301b924a33989dd9ec4508d6ad.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这个就是一种restful风格的解析

在springmvc中怎么用着种风格呢?
我记得之前在某一篇帖子种写了这种方式, [看链接](https://blog.csdn.net/weixin_54061333/article/details/121824842)

## 请求转发与响应重定向
[基础知识请看这个链接](https://blog.csdn.net/weixin_54061333/article/details/120779605)
后端--->

```java
    @RequestMapping("testForward.do")
    public String testForward() {
        System.out.println("testForward");

        return "1234.jsp";
    }
    @RequestMapping("testForward1.do")
    public String testForward1() {
        System.out.println("testForward1");
        return "/1234.jsp";
    }
    @RequestMapping("testRedirect.do")
    public String testRedirect() {
        System.out.println("testForward1");
        return "redirect:1234.jsp";
    }
```

```html
<form method="get" action="testForward.do">

    <input type="submit" value="testForward"/>

</form>

<form method="get" action="testForward1.do" >

    <input type="submit" value="testForward1" />

</form>
<form method="get" action="testRedirect.do" >

    <input type="submit" value="testRedirect" />

</form>

```

通过view来完成请求转发与重定向

```java
    @RequestMapping("/viewForward1.do")
    public View viewForward1(HttpServletRequest request){
        View view=null;
        System.out.println("viewForward1");
//        请求转发
//     view= new InternalResourceView("/1234.jsp");
//     重定向
     view=new RedirectView(request.getContextPath()+"/1234.jsp");
      return view;
    }
}

```

```html
<form method="get" action="viewForward1.do" >

    <input type="submit" value="viewForward1" />

</form>

```
通过ModelAndView 来完成请求转发与重定向;
```java
    @RequestMapping("/viewForward.do")
    public Object viewForward(HttpServletRequest request) {
        System.out.println("viewForward");
        ModelAndView model = new ModelAndView();
//      model.setViewName("1234.jsp");
//        model.setViewName("redirect:1234.jsp");

       // model.setView(new InternalResourceView("/1234.jsp"));
       model.setView(new RedirectView(request.getContextPath()+"/1234.jsp"));
        return model;
    }


```

小结:
可以通过  返回值来直接完成请求转发或者重定向,
可以通过View接口/ModelAndView接口来实现

实际上一般是通过Model model/ModelAndView来在请求转发时添加一些数据;可以看到ModelAndView 可以通过setview方法传入view参数实现请求转发与响应重定向;


## Json数据的发送与接收

```java
   @RequestMapping("/searchUser.do")
    @ResponseBody
    public User searchUser() {
     User user= new User(88,"gavin","123");
        return user;
    }

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc2caadd1511420388ab6d4c7c8788be.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


json数据的传递来龙去脉

为便于理解,做以下逐级递进
```java
//普通格式
    @RequestMapping("/searchUser.do")
    @ResponseBody
    public User searchUser(@RequestParam("name") String name,@RequestParam("pwd") String pwd) {
        System.out.println(name+pwd);
     User user= new User(88,"gavin","pwd");
        return user;
    }
   // pojo对象格式
    @RequestMapping("/searchUserd.do")
    @ResponseBody
    public User searchUserd(User users) {
        System.out.println(users);
     User user= new User(88,"gavin","pwd");
        return user;
    }

```
前端测试---->>
```html
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/12/20
  Time: 19:46
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" %>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>


<body>
<form method="get" action="toInfo.do/10">
    id号<input type="text" name="id" placeholder="请输入账号"/><br>
    <input type="submit" value="toget"/>

</form>
<form method="post" action="toInfo.do/10">
    id号<input type="text" name="id" placeholder="请输入账号"/><br>
    <input type="submit" value="topost"/>

</form>
<form method="post" action="toInfo.do/10">
    id号<input type="text" name="id" placeholder="请输入账号"/><br>
    <input type="hidden" name="_method" value="PUT"/>
    <input type="submit" value="toput"/>

</form>
<form method="get" action="toInfo.do/10">
    id号<input type="text" name="id" placeholder="请输入账号"/><br>
    <input type="hidden" name="_method" value="DELETE"/>
    <input type="submit" value="todel"/>

</form>


<form method="get" action="testVoid.do">

    <input type="submit" value="testVoid"/>

</form>
<form method="get" action="testForward.do">

    <input type="submit" value="testForward"/>

</form>

<form method="get" action="testForward1.do">

    <input type="submit" value="testForward1"/>

</form>
<form method="get" action="testRedirect.do">

    <input type="submit" value="testRedirect"/>

</form>

<form method="get" action="viewForward.do">

    <input type="submit" value="viewForward"/>

</form>

<form method="get" action="viewForward1.do">

    <input type="submit" value="viewForward1"/>

</form>

<form method="get" action="ModelAndViewForward1.do">

    <input type="submit" value="ModelAndViewForward1"/>

</form>

<form>
    姓名:<input name="name" id="name" type="text">
    密码:<input name="pwd" id="pwd" type="text">
    <input type="button" value="searchUser" onclick="search()"/>

</form>
<script src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js" type="text/javascript">

</script>
<script type="text/javascript">
    function search() {
     var str=$("#name").val();
       var str1=  $("#pwd").val();
        $.ajax({
            url: "${pageContext.request.contextPath}/searchUserd.do?name="+str+"&pwd="+str1,
            type:"get",
            // data: JSON.stringify({'name':'123','pwd':'123'}),
            // dataType: "json",
            contentType: "application/json;charset=UTF-8",
            success: function (data) {
                alert(data.name + data.pwd);
            }
        });
    }
</script>
</body>
</html>

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/355d6ce2e2a4467aacf6c5bcd545788f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/87b6d9b6c6b945bc8c5c462287e38ac9.png)

```java
@RestController
//@Controller
public class testController {  
    @RequestMapping("/searchUser.do")
    @ResponseBody
    public User searchUser(@RequestParam("name") String name,@RequestParam("pwd") String pwd) {
        System.out.println(name+pwd);
     User user= new User(88,"gavin","pwd");
        return user;
    }
    @RequestMapping("/searchUserDemo.do")
    public User searchUser() {
     User user= new User(88,"gavin","pwd");
        return user;
    }
    @RequestMapping("/searchUserd.do")
    @ResponseBody
    public User searchUserd(User users) {
        System.out.println(users);
     User user= new User(88,"gavin","pwd");
        return users;
    }

}

```

> 如果返回的数据都是json格式的.可以直接在类上加注解 @RestController来简化每个方法上的@RequesponseBody注解


**逐渐将数据替换为json格式**


```html

<form>
    id:<input name="id" id="id" type="text"/>
    姓名:<input name="name" id="name" type="text"/>
    密码:<input name="pwd" id="pwd" type="text"/>
    <input type="button" value="searchUser" onclick="search()"/>

</form>
<script src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js" type="text/javascript">

</script>
<script type="text/javascript">
    function search() {
        var str0=$("#id").val();
     var str=$("#name").val();
       var str1=$("#pwd").val();
        $.ajax({
            url: "${pageContext.request.contextPath}/searchUserd.do",
            type:"post",
            data: JSON.stringify({id:$("#id").val(),name:$("#name").val(),"pwd":$("#pwd").val()}),
            dataType: "json",
            contentType: "application/json;charset=UTF-8",
            success: function (data) {
                alert(data.id+data.name + data.pwd);
            }
        });
    }
</script>
</body>
</html>

```

后端--->>

```java
    @RequestMapping("/searchUserd.do")
    @ResponseBody
    public User searchUserd(@RequestBody User users) {
        System.out.println(users);
     User user= new User(88,"gavin","pwd");
        return users;
    }

```


![在这里插入图片描述](https://img-blog.csdnimg.cn/609c1ad5be204319b535f3a8efb87c26.png)

> @RequestBody主要用来接收前端传递给后端的json字符串中的数据的(请求体中的数据的)；而最常用的使用请求体传参的无疑是POST请求了，所以使用@RequestBody接收数据时，一般都用POST方式进行提交。在后端的同一个接收方法里，@RequestBody与@RequestParam()可以同时使用，@RequestBody最多只能有一个，而@RequestParam()可以有多个。


RequestBody 与RequestParam的区别

可以这么理解,RequestBody是接收前端json字符串的,{key:value}
RequestParam是接收前端参数的  key=value


## DateTimeFormat与 JsonFormat的区别
DateTimeFormat----参数格式

JsonFormat---响应数据格式
先看案例--->>
一个实体类
```java
package com.gavin.pojo;

import com.fasterxml.jackson.annotation.JsonFormat;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.stereotype.Repository;

import java.util.Date;
@Data
@AllArgsConstructor
@NoArgsConstructor
@Repository(value = "player")
public class Player {
    private  String name;
    private int age;
    @DateTimeFormat(pattern = "yyyy-MM-dd")//声明一个字段或方法参数应该被格式化为日期或时间。
    //处理响应json 数据的处理, pattern ：指定响应时间日期的格式 , Timezone：指定响应的时区，否则会有8个小时的时间差
    @JsonFormat(pattern = "yyyy-MM-dd" ,timezone = "UTC-8")
    private Date birth;
    private static final long serialVersionUID = 1L;
}

```


controller层
```java
package com.gavin.dateFormat;

import com.gavin.pojo.Player;
import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Repository;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class DateFormatController {
    @RequestMapping("/toLogin.do")
    @ResponseBody
    public Player showInfo(@RequestBody Player player){

        System.out.println("success");
        return player;
    }
}

```
前端

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>注册</title>
    <script src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js"></script>
</head>
<body>


<script type="text/javascript">
    function happy() {
        $.ajax({
            url:"${pageContext.request.contextPath}/toLogin.do",
            type:"post",
            data:JSON.stringify({name:"张胜男",age:18,birth:"1998-10-19"}),
            dataType:"json",
            contentType:"application/json;charset=UTF-8",
            success:function(data){
                $("#name").val(data.name);
                $("#age").val(data.age);
                $("#birth").val(data.birth);
            }
        });
    }
</script>
<form >

    <button type="button" onclick="happy()">注册</button>
    <input name="name" id="name" type="text"/>
    <input name="age" id="age" type="text"/>
    <input name="birth" id="birth" type="text"/>
</form>
</body>
</html>

```
前端点击按钮,然后异步显示数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/e7ba800ad02e4921bcadd9c224411114.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
要想知道区别,别然讲一万遍不如自己时间一遍;
![在这里插入图片描述](https://img-blog.csdnimg.cn/902550b0ae9945c9971d3e5c429131b2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
请求结果--->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/d365fa5f4e724942a027b1ac473c7b89.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
具体的原因是这样的---->
![在这里插入图片描述](https://img-blog.csdnimg.cn/522311c00d11454cb486450f1d613d3d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 当前端传过来的数据是一堆字符串时,这个时候可以用JSON.stringify(str)来将字符串转换为json对象;
>
> required：是否必须有请求体。默认值是:true。当取值为 true 时,get 请求方式会报错。如果取值 为 false，get
> 请求得到是null。



# 文件的上传与下载
前言:


用了那么多年的电脑,访问了那么多的网站,注册过那么多的账号,发表过那么多帖子,逛了那么多论坛......你有想过一个问题吗?你在上传文件的时候都上传到哪里去了?
百度识图---上传图片
![在这里插入图片描述](https://img-blog.csdnimg.cn/e9fe2b9939c949b0a7575260dfd2a6c2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


QQ空间--上传视频
![在这里插入图片描述](https://img-blog.csdnimg.cn/a0d1d5d9cf1649e29d2ad052807eeedb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
查看源码找了一下,差不多就是这个,视频被传到了这个地址上,当然这是一个servlet处理地址;
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ddcdfb53dd14760bbb447ba5356cfed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

处理结束后访问的网络地址-----这是封面图地址

```bash
http://m.qpic.cn/psc?/3237c5ef-b287-433a-aef8-8d089c2a198e/ruAMsa53pVQWN7FLK88i5tQ*nXXR08.8qgqrdzObu56d8SGR0BNfewynzKJUdIOApeH5oU4OPGqup.PQ9VPVsY7oHFmCa33uwE48CdNOfpk!/mnull&bo=FAc4BAAAAAABBw8!&rf=photolist&t=5
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/70486bb20581425e88b121640e946a12.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
快7000次请求---这是因为传的是一个视频,上传的文件很大,没有任何一个请求参数可以携带这么大的参数,想想如果我们上传10g的电影,那么该参数就会非常大,服务器可以下接收不到这么大的数据,所以只能及逆行拆分,拆分的过程中服务提供商会对数据进行一定的处理;

> 比如刚刚我只是传了一个30M(本地),上传后经过TX压缩成了17M,站应用空间直接减半;

在请求的过程中数据以二进制数据进行传输,如果数据过大会进行拆分,所以我们在传数据的时候会看到进度条的原理就是因为这个;

那这个过程是怎样实现的呢? 说一个简单点的---上传图片


## 图片的上传

这里模拟一个注册页面来实现图片的上传,前端简化页面如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/c1d81e1352fb48ba8dbb7202e5f3d88b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



基本思路是这样的 点击上传,然后显示图片缩略图,最后提交将剩余信息一起提交,所以上传时触发的是异步请求;

**第一步----前端页面**
要知道图片上传到哪里了,就要搞明白上传图片时发生了什么,


```html
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" %>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript"
            src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js">
    </script>

</head>


<body>
<form method="post" action="addPlayer.do">

    账号:<input type="text" name="name" id="name" placeholder="请输入账号"/>
    <dev>${Info}</dev><!--检查信息-->
    <br>
    密码:<input type="password" name="pwd" id="pwd" placeholder="请输入密码"/>
    <dev>${Info}</dev>
    <br>
    昵称:<input type="text" name="nickname" placeholder="请输入昵称"><br>
    头像:<input type="file" id="upFile">
    <a href="javascript:void(0)" id="uploadFile" onclick="uploadPic()">立即上传</a>
    <br>
<div id="divimg" style="width: 300px;height: 200px">
    <img src="${pageContext.request.contextPath}/static/img/jianjiacangcang.jpeg"/>
</div>
    <input type="submit" value="注册"/>
</form>
<script type="text/javascript">
    function uploadPic() {
        console.log($("#upFile")[0]);

    }
</script>

</body>
</html>

```
前端调试可以看到,传输时是以json格式进行的
![在这里插入图片描述](https://img-blog.csdnimg.cn/988a01f517b2407b9a077451196a0ae9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
由于本次只是上传了一个文件,如果是多个文件还要区分一下,所以取第一个 $("#upFile")[0];
上传时的地址----"C:\\fakepath\\zjy.jpg"
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a739cba19734c748f274eaf7b55ce31.png)
好了知道了文件源,下面要知道将文件发送给到哪里,
那么我们上传时的文件在哪里呢?
在该前端对象中找到files属性,
![在这里插入图片描述](https://img-blog.csdnimg.cn/a12ede896d0b474bbf250e803718fb93.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
由前端调试器可以看到我们传数据的时候其实是被封装为一个file数组,我们上传的数据就在这个数组里,那我们在后台保存数据的数据就要从这个数组里面去取了,由于只有一个文件,这就不去遍历了,直接取出来就好了
 $("#upFile")[0].files[0];

![在这里插入图片描述](https://img-blog.csdnimg.cn/e95317f27b3646eab5814cbea441faf6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果上传一个别的格式的,这时候file的type显示为application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,我们可以通过这个来限制上传的文件格式;
![在这里插入图片描述](https://img-blog.csdnimg.cn/ec28de8aa9fd4cff81cdaee0cfce027c.png)
别的不说,我们现在只追求最简单的一个功能实现,我们在后端只需要将这个对象解析出来就可以保存到相应位置即可;

不过,在解析之前我们先看一下该对象包含哪些属性---

```html
{
  "name":
  "zjy.jpg",
  "webkitRelativePath":"",
  "lastModified":"1640314679842",
  "size": 167763,
  "type":"image/jpeg" 
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6a9604d74e7d4fac853eb82be124d0b2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
根据属性做一些筛选;
```html
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/12/20
  Time: 19:46
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" %>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript"
            src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js">
    </script>

</head>


<body>
<form method="post" action="addPlayer.do">

    账号:<input type="text" name="name" id="name" placeholder="请输入账号"/>
    <dev>${Info}</dev><!--检查信息-->
    <br>
    密码:<input type="password" name="pwd" id="pwd" placeholder="请输入密码"/>
    <dev>${Info}</dev>
    <br>
    昵称:<input type="text" name="nickname" placeholder="请输入昵称"><br>
    头像:<input type="file" id="upFile">
    <a href="javascript:void(0)" id="uploadFile" onclick="uploadPic()">立即上传</a>
    <br>
    <div id="divimg" style="width: 300px;height: 200px">
        <img src="${pageContext.request.contextPath}/static/img/jianjiacangcang.jpeg"/>
    </div>
    <input type="submit" value="注册"/>
</form>
<script type="text/javascript">
    function uploadPic() {
           var photoFile = $("#upFile")[0].files[0];

        if (photoFile == undefined) {
         alert("请上传文件!");
            return;
        }
        var photoType = $("#upFile")[0].files[0].type;
        var photoSize = $("#upFile")[0].files[0].size;
        if ("image/jpeg" != photoType) {
            alert("请上传图片!!!");
            return;
        }
        if (photoSize > 160000) {
            alert("上传图片大小不得高于160K!!!");
            return;
        }

        var photo = null;
    }
</script>

</body>
</html>

```
筛选之后要进行提交了,把数据提交给谁,提交到哪里是接下来要做的事;

将文件包装到FormData对象中

```bash
        var formData = new FormData();
            formData.append("headerPic", photoFile);
        $.ajax({
                url: "${pageContext.request.contextPath}/fileUpload.do",
                data: formData,
                type: "post",
                processData: false,
                contentType: false,
                dataType: "json",
                success: function (data) {
                    console.log(data);
                }
            }
        )
    }
```
后端接收数据要用MultipertFile 对象

```bash

@Controller
public class FileController {

    @RequestMapping("/fileUpload.do")
    @ResponseBody
    public String upDataPic(MultipartFile headerPic){//用MultipartFile接收数据
        return  "success";
    }
}

```

但是spring并不能将json对象那个直接转换为 MutipartFile对象
,因此我们需要做一下配置----->>文件上传组件
 这个id值必须为这个名字multipartResolver,通过该id找到该对象;
```bash

<!--    配置文件上传组件-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>
```
在配置jar包时遇到了以下小麻烦,具体过程查看以下链接,说不定有你遇到的问题解决思路哦!
[链接地址在这里Error creating bean with name ‘multipartResolver](https://blog.csdn.net/weixin_54061333/article/details/122263540)

![在这里插入图片描述](https://img-blog.csdnimg.cn/d51da55688ef4315b4fa79fa12aa6474.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/c467f1f3a29c49cfb61cfc6e8486b9cc.png)

下面就是要做将文件展示在前端页面上,但是这会面临一个问题,服务器随意访问本地磁盘的文件是不太现实的,也不安全,我们需要将上传的文件放到静态资源项目静态资源文件夹下
右键项目静态文件夹,copy path的到静态资源的地址,
然后修改前端jsp和后端上传路径

![在这里插入图片描述](https://img-blog.csdnimg.cn/4b4e5ff0ab6345e198132f0aef0ea129.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20f83155dc0e435c8838b75b4676ea1b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/bdd8174779eb410a9e8a21a03185b284.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
测试---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/f7413c395c4346029b5d8c9ac4d2d63f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

之后要做的就是美化了----设置图片大小尺寸为固定的大小,就可以了
![在这里插入图片描述](https://img-blog.csdnimg.cn/528f37ed24bb4ec6988fbb174b78320f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

最后做一下整合,单独建立一个文件夹用于存放上传的图片


后端---->>

```bash


import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.io.IOException;

@Controller
public class FileController {

    @RequestMapping("/fileUpload.do")
    @ResponseBody
    public String upDataPic(MultipartFile headerPic, HttpServletRequest request) throws IOException {//用MultipartFile接收数据
        //通过request对象获得绝对路径
        ServletContext servletContext = request.getServletContext();
        String realPath = servletContext.getRealPath("/uploadPic");
        //        指定文件存储目录
//File dir=new File("C:"+File.separator+"imgs");
        File dir = new File(realPath);
        if (!dir.exists()) {
            dir.mkdirs();
        }
//获取文件名
        String originalFilename = headerPic.getOriginalFilename();
//        文件存储位置
        File file = new File(dir, originalFilename);
//        文件保存
        headerPic.transferTo(file);
        return "success";
    }
}


```
前端--->>

```bash
<%@ page contentType="text/html;charset=UTF-8" %>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript"           src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js">
    </script>
</head>
<body>
<form method="post" action="addPlayer.do">

    账号:<input type="text" name="name" id="name" placeholder="请输入账号"/>
    <dev>${Info}</dev><!--检查信息-->
    <br>
    密码:<input type="password" name="pwd" id="pwd" placeholder="请输入密码"/>
    <dev>${Info}</dev>
    <br>
    昵称:<input type="text" name="nickname" placeholder="请输入昵称"><br>
    头像:<input type="file" id="upFile">
    <a href="javascript:void(0)" id="uploadFile" onclick="uploadPic()">立即上传</a>
    <br>
    <div id="divimg" style="width: 150px;height: 200px">
        <img  id="img" src="${pageContext.request.contextPath}/uploadPic/zzy.jpg"/>
    </div>
    <input type="submit" value="注册"/>
</form>
<script type="text/javascript">
    function uploadPic() {
        console.log($("#upFile"));
        console.log($("#upFile")[0].files[0]);
        console.log($("#upFile")[0].files[0].type);
        var photoFile = $("#upFile")[0].files[0];
        if (photoFile == undefined) {                     alert("请上传文件!");
            return;
        }
        var photoType = $("#upFile")[0].files[0].type;
        var photoSize = $("#upFile")[0].files[0].size;
        var photoName= $("#upFile")[0].files[0].name;

        if ("image/jpeg" != photoType && "image/png" !=photoType) {
            alert("请上传图片!!!");
            return;
        }
           if (photoSize > 1600000) {
            alert("上传图片大小不得高于1600K!!!");
            return;
        }
        // $("#img").src="D:\\Program Data\\idea2019Data\\SompleDemo\\myweb2\\target\\myweb2-1.0-SNAPSHOT\\static\\img"+$('#upFile')[0].files[0].name;
        var formData = new FormData();
           formData.append("headerPic", photoFile);
        $.ajax({
                url: "${pageContext.request.contextPath}/fileUpload.do",
                data: formData,
                type: "post",
                processData: false,
                contentType: false,
                // dataType: "json",
                success: function (data) {
                    console.log(data);
                   window.location.reload();
                }
            }
        )
    }
</script>
</body>
</html>
```



还没结束呢,有很多问题需要解决----比如上传的文件名是一样的,但是文件内容不一样,这样只会存在同名文件中的一个是被替换掉呢还是被覆盖掉?

**答案是被覆盖掉**

那怎么避免这种情况呢?
前端用户上传我们无法控制,只能上传之后统统改名,这样就不会出现这样重名被覆盖掉的情况;

后端处理----->
使得上传的数据具有唯一性,那么可以通过UUID来实现,
![在这里插入图片描述](https://img-blog.csdnimg.cn/fcfd56cad777439dbcd62c7374a7ffa7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
UUID是一个128bit的数值,是基于当前时间、计数器（counter）和硬件标识（通常为无线网卡的MAC地址）等数据计算生成的一个128bit的数;

jdk中已经为我们提供了相应的类去实现这个功能;
[UUID的详细API出门左转](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/UUID.html)

简单测试一下

```bash

public class TestJson {

    public static void main(String[] args){      System.out.println(objectMapper.writeValueAsString(user));
        testUUID();
    }
    public static void testUUID(){
        UUID  uuid=  UUID.randomUUID();
        String s = uuid.toString();
        System.out.println("UUID:"+s);
        String STR="ZZY";
        String STR2="ZZY";

        byte[] bytes = STR.getBytes();
        byte[] bytess = STR2.getBytes();
        UUID uuid1 = UUID.nameUUIDFromBytes(bytes);
        UUID uuid2= UUID.nameUUIDFromBytes(bytess);
        System.out.println(uuid1);
        System.out.println(uuid2);
    }
}

```

```bash
UUID:993517f1-4467-4d69-b487-a82f4cd5f934
e34775e9-cc05-33b5-8fd9-e075741a4fe7
e34775e9-cc05-33b5-8fd9-e075741a4fe7

```
可以看到如果调根据名字生成的UUID还是会出现重复,所以只能选择随机生成UUID

后端是这样处理的--->
1,生成一个UUID,
2,获得文件的扩展名
3,将生成的uuid+扩展名拼成一个新文件名
4,多次发送同名文件


```bash

@Controller
public class FileController {

    @RequestMapping("/fileUpload.do")
    @ResponseBody
    public String upDataPic(MultipartFile headerPic, HttpServletRequest request) throws IOException {
             ServletContext servletContext = request.getServletContext();
        String realPath = servletContext.getRealPath("/uploadPic");
 
//File dir=new File("C:"+File.separator+"imgs");
        File dir = new File(realPath);
        if (!dir.exists()) {
            dir.mkdirs();
        }

        String originalFilename = headerPic.getOriginalFilename();

        String uuid= UUID.randomUUID().toString();

        String extendsName = originalFilename.substring(originalFilename.lastIndexOf("."));

        String newFileName=uuid.concat(extendsName);      
        File file = new File(dir, newFileName);
        headerPic.transferTo(file);
        return "success";
    }
}

```
上传成功!
![在这里插入图片描述](https://img-blog.csdnimg.cn/eec1f0af97434ab6b275a19ab060e746.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 后端控制文件类型
其实在前端我们也可以控制文件类型,通过获得文件的 type属性来实现提醒,后端则是通过获得扩展名来实现文件的上传控制;

那我们来看前端实现了什么功能?

![在这里插入图片描述](https://img-blog.csdnimg.cn/c537d3a74214486589e8864682eb55c3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
检查文件类型,并返回信息
这里以一个map对象返回
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c29f71f05c0489ba1469a7a7b33f5ce.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
到这里你会发现之前在前端做判断的话,前端代码很多,容易暴露一些业务逻辑,所以实际工作中将这些判断逻辑放在后端去处理,前端放很少的内容,前端一般是用于展示而非逻辑判断
精简后的前端

```bash
<%@ page contentType="text/html;charset=UTF-8" %>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript"            src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js">
    </script>
</head>
<body>
<form method="post" action="addPlayer.do">
    账号:<input type="text" name="name" id="name" placeholder="请输入账号"/>
    <dev>${Info}</dev><!--检查信息-->
    <br>
    密码:<input type="password" name="pwd" id="pwd" placeholder="请输入密码"/>
    <dev>${Info}</dev>
    <br>
    昵称:<input type="text" name="nickname" placeholder="请输入昵称"><br>
    头像:<input type="file" id="upFile">
    <a href="javascript:void(0)" id="uploadFile" onclick="uploadPic()">立即上传</a>
    <br>
    <div id="divimg" style="width: 150px;height: 200px">
        <img  id="img" src="${pageContext.request.contextPath}/uploadPic/zzy.jpg"/>
    </div>
    <input type="submit" value="注册"/>
</form>

<script type="text/javascript">
    function uploadPic() {
           var photoFile = $("#upFile")[0].files[0];
        var formData = new FormData();
           formData.append("headerPic", photoFile);
        $.ajax({
                url: "${pageContext.request.contextPath}/fileUpload.do",
                data: formData,
                type: "post",
                processData: false,
                contentType: false,
                // dataType: "json",
                success: function (data) {
                    console.log(data);
                    alert(data.msg);
                   window.location.reload();
                }
            }
        )
    }
</script>
</body>
</html>
```
后端----->>

```bash

@Controller
public class FileController {
    @RequestMapping("/fileUpload.do")
    @ResponseBody
    public Map<String, String> upDataPic(MultipartFile headerPic, HttpServletRequest request) throws IOException {
        Map<String, String> map = new HashMap<>();
        //通过request对象获得绝对路径
        ServletContext servletContext = request.getServletContext();
        String realPath = servletContext.getRealPath("/uploadPic");
        // 指定文件存储目录
//File dir=new File("C:"+File.separator+"imgs");
        File dir = new File(realPath);
        if (!dir.exists()) {
            dir.mkdirs();
        }

        if (headerPic==null){
            map.put("msg","请上传图片!");
            return map;
        }
//获取文件名
        String originalFilename = headerPic.getOriginalFilename();
//        控制文件大小
        if (headerPic.getSize()>1024*1024*1){
            map.put("msg","文件大小不超过1M");
            return map;
        }
//        避免名字冲突,用UUID替换文件名,但是扩展名不变
        String uuid = UUID.randomUUID().toString();
//        获取拓展名
        String extendsName = originalFilename.substring(originalFilename.lastIndexOf("."));
        if (!(".jpg".equals(extendsName) || ".png".equals(extendsName))) {
            map.put("msg", "文件类型不符合要求");
            return map;
        }
//拼接成新名字
        String newFileName = uuid.concat(extendsName);
        //        文件存储位置
        File file = new File(dir, newFileName);
//        文件保存
        headerPic.transferTo(file);
        map.put("msg","文件上传成功");
//        返回新生成的文件名
        map.put("newFileName",newFileName);
        map.put("fileType",headerPic.getContentType());
        return map;
    }
}

```
除了在java代码中限制i文件大小,还可以通过配置上传时最大的文件大小来实现这个目的

```xml
    <!--    配置文件上传组件-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="1048576"></property>
    </bean>
```
这个配置文件一般是配合后端java代码来更好地约束文件的大小;
为什么不建议单独使用这个配置文件来实现文件大小的约束呢?
首先单独使用这种方式,**前端用户无法收到提示信息**,其次如果文件大小不符合要求,请求时会出现 500错误
![在这里插入图片描述](https://img-blog.csdnimg.cn/330049b6e92a46a39aa86c460d5f6ced.png)
同时后端代码会出现异常

```c
在路径为/myweb2_war_exploded的上下文中，Servlet[myDeparture]的Servlet.service（）引发了具有根本原因的异常Request processing failed; nested exception is org.springframework.web.multipart.MaxUploadSizeExceededException: Maximum upload size of 1048576 bytes exceeded; nested exception is org.apache.commons.fileupload.FileUploadBase$SizeLimitExceededException: the request was rejected because its size (1169635) exceeds the configured maximum (1048576)
	org.apache.commons.fileupload.FileUploadBase$SizeLimitExceededException: the request was rejected because its size (1169635) exceeds the configured maximum (1048576)
```

可以看到QQ在上传文件时会有一个缩略图显示,它是怎么实现的呢?

## 文件的显示

这个实现起来就就比较简单了,加一个img标签,添加src,不过这个是动态实现的;
后端不需要做修改,只需要将前端收到的文件名塞到img标签下的src属性上

前端代码---->>改动部分
![在这里插入图片描述](https://img-blog.csdnimg.cn/254b5cf4fcd24e3a82716317363ea624.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/aaac27d0705847cdba2179cbb8ff465e.png)

## 进度条的实现
一直很好奇进度条的实现,代码怎么实现?
用到的工具---------->>
准备材料:
1,准备一个文件
2,通过FormData发送对象----ajax中传输二进制文件的一个de接口;
3,通过jquery中的XmlHttpRequest.upload来监听文件上传进度.,这个方法会返回一个upload对象,

前端页面
准备一个进度条
其实就是两个嵌套的div来实现的
外层div是

```html
<!--    给进度条君添加一些样式-->
<style>
        .progress {
            width: 400px;
            height: 10px;
            border-radius: 10px;
            margin: 10px 0px;
            overflow: hidden;
        }

        .progress > div {
            width: 0px;
            height: 100px;
            background-color: yellowgreen;
            transition: all .3s ease;
        }
    </style>
 <div class="progress">
        <div>
        </div>
    </div>
    
```

然后就要通过异步ajax来实现进度条

```
  function uploadPic() {
        var photoFile = $("#upFile")[0].files[0];
        var formData = new FormData();
            formData.append("headerPic", photoFile);
        $.ajax({
                url: "${pageContext.request.contextPath}/fileUpload.do",
                data: formData,
                type: "post",
                processData: false,
                contentType: false,
                // dataType: "json",
                success: function (data) {
                    console.log(data);
                    alert(data.msg);
                    $("#img").attr("src", "uploadPic/" + data.newFileName);
                    //window.location.reload();
                },
                xhr: function () {
                    var xhr = new XMLHttpRequest();
                    //使用XMLHttpRequest.upload监听上传过程，注册progress事件，打印回调函数中的event事件
                    xhr.upload.addEventListener('progress', function (e) {
                        console.log(e);
                        //loaded代表上传了多少
                        //total代表总数为多少
                        var progressRate = (e.loaded / e.total) * 100 + '%';
                        $("#divNum").text(progressRate);//显示百分比
                        //通过设置进度条的宽度达到效果
                        $('.progress > div').css('width', progressRate);
                    })
                    return xhr;//每次监听都返回一个xhr
                }
            }
        );

    }

```
简易效果如下--->
![在这里插入图片描述](https://img-blog.csdnimg.cn/d1ae81c3845a40958a01ee24b38031cd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 图片的下载

那时候对云盘还没有什么概念,只知道邮箱里有一个中转站,算是早期网盘的雏形;
![在这里插入图片描述](https://img-blog.csdnimg.cn/fdac2f6230f44770894815aa3e1525fa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
大约在2014年的时候网盘才浮现在我的视野里,到现在用过好多云盘,360云盘,百度云盘,新浪微盘,阿里云盘,腾讯微盘,现在还活着的.....

说远了,关于文件的下载,跟文件的上传是反着来的,可以理解位服务器上传文件到用户计算机;

前端案例---->>
前端展示要下载的内容,用户点击下载就可以下载到本地;
```bashhtml
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2022/1/8
  Time: 14:56
  To change this template use File | Settings | File Templates.
--%>

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <style>
        #playerTable {
            width: 50%;
            border: 3px solid cadetblue;
            margin: 0px auto;
            text-align: center;
        }

        #playerTable th, td {
            border: 1px solid gray;
        }

        #playerTable img {
            width: 100px;
            height: 100px;
        }
    </style>
    <script type="text/javascript" src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js"></script>
    <script>
        // 进入这个页面一上来就加载所有信息
        $(function () {
            $.ajax({
                type: "post",
                url: "getAllUser",/*获得所有信息*/
                success: function (users) {
                    $.each(users, function (i, e) {
                        $("#playerTable").append('<tr>\n' +
                            '        <td>' + e.id + '</td>\n' +
                            '        <td>' + e.name + '</td>\n' +
                            '        <td>' + e.pwd + '</td>\n' +
                            '        <td>' + e.nickname + '</td>\n' +
                            '        <td>\n' +
                            '            <img src="http://192.168.1.2:8090/loadPic/' + e.picname + '" alt="" src>\n' +
                            '        </td>\n' +
                            '        <td>\n' +/*点击下载跳转链接,使得文件发送到用户端*/
                            '            <a href="fileDownload.do?picname='+e.picname+'&filetype='+e.filetype+'">下载</a>\n' +
                            '        </td>\n' +
                            '    </tr>')
                    })
                }
            })
        })
    </script>
</head>
<body>
<table id="playerTable" cellspacing="0xp" cellpadding="0px">
    <tr>
        <th>编号</th>
        <th>账号</th>
        <th>密码</th>
        <th>昵称</th>
        <th>头像</th>
        <th>下载</th>
    </tr>

</table>
</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ca692f88fcd4d308fe266f9c2a20d8f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

在这之前准备的数据库的一些其他改动就省略了;
后端页面

```bash
   /**
     * 文件下载
     *
     * @param picname  文件名
     * @param fileType 文件类型--前端
     * @param resp     响应对象
     * @throws IOException
     */

    @RequestMapping("/fileDownload.do")
    public void fileDownLoad(String picname, String fileType, HttpServletResponse resp) throws IOException {
        //设置响应头
//        这里如果传过来id则还要到数据库查,不如直接穿过来文件名
//    解析文件名----数据库里没有直接存文件格式,所以要在这里解析文件格式,但是这列格式并非前端识别的格式,所以还是修改之前的在用户表上添加前端文件格式

//    保存数据到磁盘上,不在浏览器上直接解析
        resp.setHeader("Content-Disposition", "attachment;filename=" + picname);
        resp.setContentType(fileType);
//        获取一个文件输入流
        InputStream inputStream = new URL(pic_location+picname).openStream();
//        浏览器获得输出文件
        ServletOutputStream outputStream = resp.getOutputStream();
        IOUtils.copy(inputStream,outputStream);
    }
    @RequestMapping("/showUser.do")
    public String showUser(){

        return "/WEB-INF/serviceData/loginPage/picDown.jsp";
    }
```

整个完整的文上传与下载界面---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/5b3a137f3ac448388df7d0697c36fb99.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

- [文件上传下载resource](https://github.com/JeEisenberg/fileupload)
- [员工管理系统部分resource](https://github.com/JeEisenberg/springmvc)
- [图书管理系统resource](https://github.com/JeEisenberg/LibraryProject)

<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/tomcat/tomcat.md#三级联动的实现)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/spring/SpringBoot.md#springBoot)

</center>

![](..\pic\div\plant.gif)