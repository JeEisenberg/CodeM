![](..\pic\logo2.png)

#   SpringBoot

咱们不聊源码与底层原理,也不去分析spring底层各种设计 模式,咱就从  SpringBoot的使用上入手;

[spring官方启动器](https://docs.spring.io/spring-boot/docs/2.6.3/reference/htmlsingle/#using.build-systems.starters)

**Springmvc与  SpringBoot简单对比**

在没有接触到springmvc时,要写与数据库交互的代码,发现很多代码都是重复的,耦合度也很大,在接触过springmvc时,大大简化了代码量,最重要的是事务管理交由spring处理,降低了下耦合度;
在接触到  SpringBoot之后,发现还可以在springmvc的基础上进行简化,由于springmvc中的配置文件太多;

Spring为企业级Java开发提供了一种相对简单的方法，通过依赖注入和面向切面编程，用简单 的Java对象（Plain Old Java Object，POJO）实现了EJB的功能。

springmvc在配置依赖的时候如果依赖版本之间不兼容会发生不可描述的事情----个人经历过;
再来说一下
**  SpringBoot优点:**
①　使用Spring Boot可以创建独立的Spring应用程序
②　在Spring Boot中直接嵌入了Tomcat、Jetty、Undertow等Web  容器，在使用  SpringBoot做Web开发时不需要部署WAR文件
③　通过提供自己的启动器(Starter)依赖，简化项目构建配置
④　尽量的自动配置Spring和第三方库
⑤　绝对没有代码生成，也不需要XML配置文件

> 注:所有 官方 启动器都遵循类似的命名模式； spring-boot-starter-*
>
> 第三方启动器通常以项目名称开头。 例如，一个名为 `thirdpartyproject`通常会被命名为 `thirdpartyproject-spring-boot-starter`.  

## 搭建  SpringBoot----方式1



**新建一个空白项目**

![在这里插入图片描述](https://img-blog.csdnimg.cn/c49f69046be944a0995bd2f051f9b653.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**配置一下maven**

按照自己的设置走,以方便控制与检查
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a7a1d274e0f4601ac2fa61dcaebfbea.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
新建maven项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/4d152d5505d54fefb549dac679804cc0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**配置  SpringBoot父类依赖**

![在这里插入图片描述](https://img-blog.csdnimg.cn/f1625c6df98b4eca9ffd39fa304b70ef.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 该依赖里又配置了一些其他信息,猜测是配置好这个依赖之后运行或者编译的时候会自动去下载一些其他所需要的依赖;

![在这里插入图片描述](https://img-blog.csdnimg.cn/48911069b70c431e9befc83eb4601962.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
除此之外要想启动项目还需要导入一个启动器

**导入启动器依赖**

Spring Boot 的启动器实际上就是一个依赖。这个依赖中包含了整个这个技术的相关jar包,  SpringBoot又很多类型的启动器,比如最基本的启动器spring-boot-starter,还有spring-boot-starter-web


![在这里插入图片描述](https://img-blog.csdnimg.cn/f86d618f99034ba8a470403e8783c0b9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)导入这个启动器之后就会根据依赖传递导入其他的一些依赖![在这里插入图片描述](https://img-blog.csdnimg.cn/ec50f6f132bf4fa79275d7919d844b92.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d845b9e7eccf428cb290bbf71f451c36.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
导入spring-boot-starter-web启动器
![在这里插入图片描述](https://img-blog.csdnimg.cn/ea85ab5d8e7744849afc71b4d88c99c9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
依赖图解---->
![在这里插入图片描述](https://img-blog.csdnimg.cn/afb0f537dd7347e381049af7712d7879.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

导入一个依赖然后我们使用的springmvc依赖基本都导入进来了;
接下来测试一下-----以spring-boot-starter-web启动依赖为根基
写一个controler

```java
package com.gavin.controller;

import com.gavin.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**test
 * @author Gavin
 */
@Controller
public class FileController1 {
@RequestMapping("/showInfo.do")
@ResponseBody
    public String showName(@RequestBody(required = false) User user){

        return "hello-  SpringBoot";
    }
}

```
再写一个启动类

```java

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.  SpringBootApplication;

/**启动类
 * @author Gavin
 */
@  SpringBootApplication//注解标记为  SpringBoot启动类
public class FileController1Application {
//    定义主方法
    public static void main(String[] args) {

        SpringApplication.run(参数还没写呢!);
    }
}

```
该启动类的参数---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/e9b319ed41f94b02a9201ca024366b6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**启动类**

>  启动类 要求：
>  1.启动类的位置，必须包含在一个包里，并且该包是其他文件的同包或者父包。
>  2.在类的上方添加注解 @  SpringBootApplication
>  3.Spring Boot 项目自动扫描 启动类所在的包以及 启动类 所在包的子包。


```java
package com.gavin;

import com.gavin.controller.FileController1;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.  SpringBootApplication;

/**启动类
 * @author Gavin
 */
@  SpringBootApplication//注解标记为  SpringBoot启动类
public class FileController1Application {
//    定义主方法
    public static void main(String[] args) {
//param 启动类字节码,用来确定默认扫描的包,为了能够扫描到所有的注解,所以要将该类放在项目最顶层,另一个参数将主方法的args传过来即可
        SpringApplication.run(FileController1Application.class,args);
    }
}

```
接下来运行一下启动类

**运行实例分析**

![在这里插入图片描述](https://img-blog.csdnimg.cn/15b716873453408e89f1ebfab69fbc5a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然后按照之前的springmvc模式,应该有一个访问路径才对,比如 ***war_exploded,
于是习惯性的找一下,看看路径是什么.
![在这里插入图片描述](https://img-blog.csdnimg.cn/a54ba19651954817994bb3e5decbc584.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
发现并没有,那访问tomcat主页试一试,
也报错了,是不是启动失败了呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/ffd8a0baaef34e40994240ed869e75bd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然而并没有,直接访问servelet就可以得到返回结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/e4e1e52a19de4a24ad97ce7af8a7099c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
对比一下之前费劲心思搞得一堆配置文件与依赖的导入,  SpringBoot还真是简洁有效
基本上看不到什么配置文件与依赖导入;

> Spring Boot本质是Spring Framework，作用是Spring 整合各种其他技术，让其他技术使用更加方便;


总结   SpringBoot搭建方式1------>>>继承父类
继承父类pom

```xml
   <groupId>com.gavin.  SpringBoot</groupId>
    <artifactId>  SpringBootlearn1</artifactId>
    <version>1.0-SNAPSHOT</version>
<parent><!--继承父项目方式-->
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.2</version>
</parent>
<dependencies>
<!--    spring启动器-->

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.6.2</version>
    </dependency>

</dependencies>
```
## 搭建  SpringBoot----方式2
直接导入  SpringBoot依赖
同样新建一个空白的模块
导入依赖

```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.6.2</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <!--启动器依赖--><groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.6.2</version>
        </dependency>
    </dependencies>
```
在  SpringBoot中用了继承的方式实现了jar包管理,那么就不能在继承别的项目了,这样的话就不利于程序后期的扩展与维护了,所以  SpringBoot提供了以来导入的方式

创建控制器

```java
package com.gavin.controller;

import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**test
 * @author Gavin
 */@RestController
public class FileController01 {

    @PostMapping("/show.do")
    public String showInfo(){

        return "hello World";
    }
}

```
启动器--->>

```java
package com.gavin;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.  SpringBootApplication;

/**just start
 * @author Gavin
 */

@  SpringBootApplication//标记启动类
public class FileController01Application {
    public static void main(String[] args) {
        SpringApplication.run(FileController01Application.class,args);
    }
}

```
这种方式要自己配置一下编码格式,\ utf-8,不然会出现乱码等问题;
那么默认的请求方式是什么呢?-----get![在这里插入图片描述](https://img-blog.csdnimg.cn/c7dcb2935eb7450088c4bd319f060fb6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
第三种方式就是依靠IDE来实现  SpringBoot的搭建

## 搭建  SpringBoot----方式3

依靠Idea来实现  SpringBoot的搭建

![在这里插入图片描述](https://img-blog.csdnimg.cn/7e9c31d6bcac4d558e4c87fb958b3db8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/c21e7ac91f294207a1723f41aaab9dea.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

接下来即使开发controller了,具体跟上面一样，不在赘述了；

##   SpringBoot创建 jar/war
创建一个可以在生产中运行的完全独立的可执行 jar 文件



要创建一个可执行的 jar，我们需要添加 spring-boot-maven-plugin对我们的 pom.xml. 为此，请在下面插入以下行 dependencies部分：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```
保存您的 pom.xml并运行 mvn package从命令行,
打包好之后, 执行该jar(默认是打成jar包,这里包含了tomcat),如果要打成war则需要在依赖中将tomcat依赖排除,然后在收到导入tomcat依赖,并设置工作范围为 provided
```
$ jar tvf target/myproject-0.0.1-SNAPSHOT.jar
```


##   SpringBoot的运行逻辑

在一开始我们分析了  SpringBoot相应的启动器依赖之后，由于依赖传递性，相关依赖也会被导入；

下面我们细看一下第一种  SpringBoot创建方式---》
继承父类的方式，既然继承了父类，那么子类就会用到父类的一些配置，等等
![在这里插入图片描述](https://img-blog.csdnimg.cn/56e19846287b43d28f4b748f24b9c540.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> spring基本依赖导入之后,如果想要加入一些其他功能,那么只需要导入相关启动器即可

例如导入spring-boot-starter-web依赖，那么查看该依赖，发现相关jar依赖都被导入进来了；

![在这里插入图片描述](https://img-blog.csdnimg.cn/6c3abac108924f7196d5eb7353ccfa4c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
相关一类通过启动器来导入后大大方便了，在启动  SpringBoot时我们还需要通过扫描注解来实现
这里用到的时spring中零注解的实现方式------通过注解来实现
假如启动类中没有注解
![在这里插入图片描述](https://img-blog.csdnimg.cn/55789e383a5f4713a01baf8cf1ba5c44.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

报错----》》启动失败

```

org.springframework.context.ApplicationContextException: Unable to start web server; nested exception is org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.

Caused by: org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.getWebServerFactory(ServletWebServerApplicationContext.java:210) ~[spring-boot-2.6.2.jar:2.6.2]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.createWebServer(ServletWebServerApplicationContext.java:180) ~[spring-boot-2.6.2.jar:2.6.2]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:160) ~[spring-boot-2.6.2.jar:2.6.2]
	... 8 common frames omitted

```

再看启动类中的main方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/5f6ac85258954372b78d3b7248fbdf66.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
通过 SpringApplication.run(  SpringBoot03Application.class, args);
可以看出，该方法获得了字节码信息，就像[spring无注解开发](https://blog.csdn.net/weixin_54061333/article/details/121579602)时的
原理一样，首先通过获得字节码信息扫描配置类，然后在装配相关bean实现功能；
![在这里插入图片描述](https://img-blog.csdnimg.cn/e7fc90f61cf546229497537cfb81c682.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
当我们使用了@  SpringBootApplication注解就等同于使用了 @  SpringBootConfiguration
 @EnableAutoConfiguration
 @ComponentScan
 这三个注解;

除了包扫描还需要扫描配置文件，
![在这里插入图片描述](https://img-blog.csdnimg.cn/5f30f2b1bd134a64b2d945143fe8aa58.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这里有一个重要的注解----import

这个注解有什么用呢?
简单在  SpringBoot基础上添油加醋一些

前面说import是导入一些类的字节码信息用于注解扫描,要扫描注解,那么就要通过反射来获得该注解于是乎就要找到该类,加载实体类的字节码信息;
![在这里插入图片描述](https://img-blog.csdnimg.cn/7085c334b77348188f8bb9d52e4032da.png)
准备一个配置类,
![在这里插入图片描述](https://img-blog.csdnimg.cn/eebbf2bff6f444318dd345db58b4f25e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
  SpringBoot的配置类---->>

在spring启动类上加载该信息----->>

```java
package com.gavin;

import com.gavin.pojo.fruit;
import com.gavin.pojo.vegetables;
import com.gavin.service.myconfigration;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.  SpringBootApplication;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Import;

/**just start
 * @author Gavin
 */

@  SpringBootApplication//标记启动类

public class FileController01Application {
    public static void main(String[] args) {
        SpringApplication.run(FileController01Application.class,args);
        AnnotationConfigApplicationContext applicationContext2 = new AnnotationConfigApplicationContext(myconfigration.class);
        String[] beanNames = applicationContext2.getBeanDefinitionNames();
        for(int i=0;i<beanNames.length;i++){
            System.out.println("加载了=="+beanNames[i]);
        }

    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/9038efaefa3f49e3b51ea4c813bd907e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 对应的import字节码信息都已经加入到spring的容器中了

![在这里插入图片描述](https://img-blog.csdnimg.cn/a26d7f43eb8e44698438659cb7b49f14.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
例如--->>>

```
package com.gavin.service;

import org.springframework.context.annotation.ImportSelector;
import org.springframework.core.type.AnnotationMetadata;

/**
 * @author Gavin
 */

public class ConfigurationImportSelectorDemo implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        String str[] = {"com.gavin.pojo.fruit"};//要引入的类全限定名

        return str;
    }
}
```
配置文件
```java
package com.gavin.service;

import com.gavin.pojo.fruit;
import com.gavin.pojo.vegetables;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

/**
 * @author Gavin
 */
@Import({ConfigurationImportSelectorDemo.class})
@Configuration
public class myconfigration {


}


```
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e0c097b6c7942319865c6ba605b4f97.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

另一种加载方式---->>实现ImportBeanDefinitionRegistrar

```java
package com.gavin.service;

import org.springframework.beans.factory.support.BeanDefinitionRegistry;
import org.springframework.beans.factory.support.BeanNameGenerator;
import org.springframework.beans.factory.support.RootBeanDefinition;
import org.springframework.context.annotation.ImportBeanDefinitionRegistrar;
import org.springframework.context.annotation.ImportSelector;
import org.springframework.core.type.AnnotationMetadata;
public class ConfigurationImportSelectorDemo implements ImportBeanDefinitionRegistrar {
    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry, BeanNameGenerator importBeanNameGenerator) {
        RootBeanDefinition rootBeanDefinition = new RootBeanDefinition(com.gavin.pojo.fruit.class);
//        注册一个类 由于参数需要RootBeanDefinition实例,所以....
        registry.registerBeanDefinition("fruit",rootBeanDefinition);
    }
}

```
配置类---->>

```java
package com.gavin.service;

import com.gavin.pojo.fruit;
import com.gavin.pojo.vegetables;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

/**
 * @author Gavin
 */
@Import({ConfigurationImportSelectorDemo.class})
@Configuration
public class myconfigration {


}


```
结果---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/e5f7b4753b7544238ada247222ccd90f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
所以通过加载AutoConfigurationImportSelector.class字节码信息就可以顺带加载类里的一些实例,在需要的时候进行注入;需要与数据库连接则加载beanfactory,打印日志---则加载log等;


![就](https://img-blog.csdnimg.cn/90e266b0aecf412693560c60806065fb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



简单来讲  SpringBoot像是整合了springmvc,同时又通过注解来实现0xml的配置来完成;几个核心的注解---
>@import---加载字节码
>@  SpringBootConfiguration---标注  SpringBoot启动类
>@EnableAutoConfiguration---自动扫描配置
>@ComponentScan---包扫描
>![在这里插入图片描述](https://img-blog.csdnimg.cn/3eaedccd7e404f8bae7c9c34ede4c378.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



  SpringBoot虽然是0xm配置,但是这是因为他在底层已经将我们需要的配置好了,那么

扫描的配置文件在哪里呢?
还是要找到import注解作为突破口,加载类,然后在 AutoConfigurationImportSelector类里有这样一些方法

```java
protected AnnotationAttributes getAttributes(AnnotationMetadata metadata) {
        String name = this.getAnnotationClass().getName();
        AnnotationAttributes attributes = AnnotationAttributes.fromMap(metadata.getAnnotationAttributes(name, true));
        Assert.notNull(attributes, () -> {
            return "No auto-configuration attributes found. Is " + metadata.getClassName() + " annotated with " + ClassUtils.getShortName(name) + "?";
        });
        return attributes;
    }
     protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
        List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
        Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
        return configurations;
    }
  protected void handleInvalidExcludes(List<String> invalidExcludes) {
        StringBuilder message = new StringBuilder();
        Iterator var3 = invalidExcludes.iterator();

        while(var3.hasNext()) {
            String exclude = (String)var3.next();
            message.append("\t- ").append(exclude).append(String.format("%n"));
        }

        throw new IllegalStateException(String.format("The following classes could not be excluded because they are not auto-configuration classes:%n%s", message));
    }

```
当配置发生异常时会有一些提示,
No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.
这说明在扫描配置文件时会扫描 META-INF下的spring.factories,
这个包在 spring-boot-autoconfigure包下(测试类 就略过吧)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8a85dc28227e4fa7aa5eaa216d1b66b5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这个spring.factories,都有什么呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/08ba172e25b842c8b73d5be24b28706c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/db85148340a64be2b069d135a80c4976.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

看起来好多,那先一下关于jdbc以及web的一些配置

![在这里插入图片描述](https://img-blog.csdnimg.cn/fa7363da3a9743d992535fe996065de5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
其他配置信息所在文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/8e02dacb0e3e42bca9311f130eacfc85.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
  SpringBoot的项目结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/9838281bc0544730a41aff5a9ee457e9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
static文件夹下的内容可以直接访问,不需要在配置静态资源放行了

![在这里插入图片描述](https://img-blog.csdnimg.cn/d02f030de2aa4a8387049a893e60fce0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/0b459bc2cb4241419c23b4d893933695.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
public里的内容也可以直接访问;
![在这里插入图片描述](https://img-blog.csdnimg.cn/147e7830c9704aeeb2d615a64afdaee6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/845492c04ac041948a1e7866413a530e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
小结----static是一般是用于存放静态资源的,public用于存放登录页,注册页等公共访问资源的;

**静态资源默认存放位置**

默认情况下，Spring Boot 从类路径中名为`/static``/public``/resources``/META-INF/resources` 的目录或从 的根目录提供静态内容。它使用 `ServletContext``ResourceHttpRequestHandler``WebMvcConfigurer``addResourceHandlers`from Spring MVC，以便您可以通过添加自己的行为并重写该方法来修改该行为。

在独立的 Web 应用程序中，容器中的默认 servlet 也会被ServletContext启用并充当回退，从 if Spring 决定不处理它的根目录提供内容。大多数情况下，这种情况不会发生（除非您修改默认的 MVC 配置），因为 Spring 始终可以通过 DispatcherServlet处理请求。

静态资源默认存放的路径为static content from a directory called /static (or /public or /resources or /META-INF/resources)

默认  SpringBoot将从如下位置按如下优先级(从高到低)加载jar包对应前端静态资源：

1.jar包同级static目录
2.jar包同级public目录
3.jar包同级resource目录
4.jar包/META-INF/resources
在调试模式下，  SpringBoot将从class目录中按如下优先级(从高到低)加载对应前端静态资源

1.class目录下static目录
2.class目录下public目录
3.class目录下resource目录
4.class目录下/META-INF/resources

resources 目录下,static/public目录是我们的静态资源目录,直接访问该目录下的资源的映射路径不需要携带/public或者/static,直接访问即可

![1644032733912](..\pic\static)

如果controller的映射路径和静态资源路径冲突,会发生什么事情呢?

```java
package com.gavin.controller;

import com.gavin.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class Mycontroller {


    @ResponseBody
    @RequestMapping("/hello")
    public String hello() {
        return " hello world";
    }
    @ResponseBody
    @RequestMapping("/1.png")
    public String hell() {
        return " hello world";
    }
}

```

请求进来,先去看Controller中有没有对应的资源,如果有则,执行controller资源,如果没有,就交给静态资源处理器,静态资源处理器也没有找到,则返回404

![1644033973189](..\pic\address)

当发生这种情况时,我们可以通过配置  SpringBootweb静态资源访问前缀来区别,

默认情况下，资源映射在 上，但您可以使用该属性对其进行调整。例如，可以按如下方式重新调整所有资源：

```yml
spring: mvc: static-path-pattern: "/resources/**" 
```

例如---->>>

```yml
spring:
  mvc:
    format:
      date: dd/MM/yyyy
    static-path-pattern: /static/**
```

![1644034237192](..\pic\static1)

**指定静态资源位置**

使用该属性自定义静态资源位置（将默认值替换为目录位置列表）。根 Servlet 上下文路径 也会自动添加为位置。`spring.web.resources.static-locations``"/"` 

我么还可手动指定要扫描的静态资源目录

比如---->>

```yml
mvc:
  format:
    date: dd/MM/yyyy
  static-path-pattern: /res/**  #指明访问的是静态资源
web:
  resources:
    static-locations: [classpath:/res/]  #指明静态资源存放位置
```

```java
package com.gavin.controller;

import com.gavin.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class Mycontroller {


    @ResponseBody
    @RequestMapping("/hello")
    public String hello() {
        return " hello world";
    }
    @ResponseBody
    @RequestMapping("/1.png")
    public String hell() {
        return " hello world";
    }
}

```

![1644047044144](..\pic\test11)

![1644047104285](..\pic\mulu)

实际上按照  SpringBoot默认的就好,一般不手动配置静态资源存放位置,保持static/public/templates就好

[  SpringBoot静态内容api](https://docs.spring.io/spring-boot/docs/2.4.13/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content)

**WebJars**

>   SpringBoot还支持静态资源webjars 的处理方式,就是将静态资源打成jar导入也可以实现静态资源的导入和访问。任何具有 in 路径的资源都从 jar 文件提供，如果它们以 Webjars 格式打包。`/webjars/**` 

> 如果应用程序打包为 jar，请不要使用该目录。尽管此目录是通用标准，但它**仅适用于** war 打包，并且如果您生成 jar，大多数构建工具都会默默忽略`src/main/webapp` 文件夹;

WebJars是客户端Web库（例如jQuery&Bootstrap）打包成JAR（Java Archive）文件。

- 显式且轻松地管理基于 JVM 的 Web 应用程序中的客户端依赖关系
- 使用基于 JVM 的构建工具（例如 Maven、Gradle、sbt 等）下载客户端依赖项
- 了解您正在使用的客户端依赖项
- 可传递依赖关系自动解析，并可选择通过 RequireJS 加载

**WebJars有四种形式**

```
1,NPM WebJars
- Contents mirror NPM package
- GroupId: org.webjars.npm
- ArtifactId: NPM Package or URL-based Name

2,Bower GitHub WebJars
Contents mirror Bower package
GroupId: org.webjars.bowergithub.[GITHUB_ORG]
ArtifactId: GitHub Repo Name

3,Classic WebJars
Custom Built and Manually Deployed
GroupId: org.webjars
ArtifactId: Varies

4,Bower Original WebJars
Deprecated Use Bower GitHub WebJars instead
GroupId: org.webjars.bower
ArtifactId: Bower Package or URL-based Name

```

![1644048945709](..\pic\picture)

![]()

导入依赖

```xml
 <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.6.0</version>
        </dependency>

       <!-- <dependency>
            <groupId>org.webjars.bower</groupId>
            <artifactId>jquery</artifactId>
            <version>3.6.0</version>
        </dependency>
        <dependency>
            <groupId>org.webjars.npm</groupId>
            <artifactId>jquery</artifactId>
            <version>3.6.0</version>
        </dependency>
        <dependency>
            <groupId>org.webjars.bowergithub.jquery</groupId>
            <artifactId>jquery-dist</artifactId>
            <version>3.6.0</version>
        </dependency>
-->
```

导入依赖之后的目录结构

![1644049088803](..\pic\webjars)

导入之后可以直接访问吗?

![1644049335397](..\pic\1644049335397.png)

但是这种方式在打包时并不会体现在项目目录结构上,而是在导入的jar中存放,所以我们也可以根据这种形式来到导入静态资源;





以上就是  SpringBoot入门知识了;
果然越简单的东西是越复杂的;



##   SpringBoot的版本升级/迁移

升级到新功能版本时，某些属性可能已重命名或删除。 Spring Boot 提供了一种在启动时分析应用程序环境和打印诊断信息的方法，还可以在运行时为您临时迁移属性。 要启用该功能，请将以下依赖项添加到您的项目中：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-properties-migrator</artifactId>
    <scope>runtime</scope>
</dependency>
```

	后期添加到环境中的属性，例如使用时 @PropertySource, 不予考虑。
	完成迁移后，请确保从项目的依赖项中删除此模块。 


[spring官方启动器](https://docs.spring.io/spring-boot/docs/2.6.3/reference/htmlsingle/#using.build-systems.starters)
前面已经提到了  SpringBoot的运行原理,包扫描以及配置扫描
一些基本配置就生效了,比如我们引入一个spring-boot-starter-web,启动时就会默认开启8080端口,我么也没有做其他配置,如果我们想要自定义配置那该怎么做呢?

#   SpringBoot自定义配置
想要自动一配置就要知道  SpringBoot启动时会扫描那些配置文件---->>
扫描哪些呢?.xml ?.peoperties?还是其他的?
别着急,看看  SpringBoot源码中是怎么做的---->

先看第一种创建  SpringBoot的方式----继承父类
在这父类依赖中
![在这里插入图片描述](https://img-blog.csdnimg.cn/35f9fdd1242c49b283055ffb4baaf62e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 编译器版本

默认jdk1.8如果需要别的jdk版本,则按需配置字符编码默认为UTF-8
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a94f8fef65640dc9e8514a5b751a57f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

如果想要自定义配置,可以在pom.xml自己配置别的jdk版本与编码方式;

## 配置文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/f999e90377244b8ca33a587df1b93022.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
第一个resource表示资源文件只扫描这个一application*.yml或者application*.yaml或者application*.properties文件;

第二个resource表示扫描时排除了上面三个文件剩下的;
如果要自定义配置文件/或文件类型,那么在pom文件中配置resource来替换掉这个配置,这将会覆盖spring-boot-starter-parent中的代码，即spring-boot-starter-parent中的代码将失效。

> 但是既然官方都默认这么干了,那还是按照官方走吧;
> 由此我们也可以看到,如果要自动扫描,就要循序官方的配置文件命名格式
> 配置文件的命名格式
> **以application开头的yam/yaml/properties文件;**
> 实际工作中以yml作为配置文件用的是最多的;
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/433211d511bd49768ea15f75be51e156.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
> 如果非要使用xml来配置的化.也不是不可以,(但  SpringBoot为我们提供的方便---自动扫描,何乐而不为呢,自己配置的还要再去手动指认要扫描的配置文件名;)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/71ecaab056da4438aa66d2168a25f16b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


这里还是以官方推荐的配置文件进行展开-------------------->>>>

配置文件的配置格式是什么样的呢?------最好的学习方式就是去官网;
找到  SpringBoot部分,映入眼帘的是,关于  SpringBoot的介绍,系统要求,以及构建工具等;
![在这里插入图片描述](https://img-blog.csdnimg.cn/ade09790d7ac4fb2805ebf6e73307839.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)每个  SpringBoot支持的JDK版本不同---->>

> Spring Boot 2.6.2 需要Java 8，并且兼容 Java 17（包括 Java 17）。Spring Framework 5.3.14或更高版本也是必需的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/ff8bc030885a4863a4ece5d5fdfa53dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
如果手动添加的数据库配置则  SpringBoot中的默认配置会退位让贤给手动配置的数据库内容;
![在这里插入图片描述](https://img-blog.csdnimg.cn/fb90b7c45e7d4b7cb28419e69d1f9779.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c9bbdb1651094c31a13587af52814a6b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/345ee420573242b3862bfa081aa01a96.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/895230adf91648d3afe6a221ace47cb1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
内容还是比较多的,先打地基-----配置文件格式----->>

## 配置文件格式
配置文件的类型有很多,不过大部分还是以默认的为基准,真正自己自定义的配置比较少;

> 数据库配置,
> 事务配置 
> web端口号和访问路径配置 
> 缓存配置

![在这里插入图片描述](https://img-blog.csdnimg.cn/424d734d90d4454a8637508f55793021.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

以web中的配置为例-------->>
  SpringBoot在没有配置上下文路径是默认为空,即直接访问servlet就可以了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/e36bb64c169843afb8c5d43ce434be8b.png)
但实际运用中要配置上下问路径的;
![在这里插入图片描述](https://img-blog.csdnimg.cn/5eb4f28fc9034b97a8c84d38c432edf1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
下面到了自己指定上下文路径的时候了

![在这里插入图片描述](https://img-blog.csdnimg.cn/6671b33399f743419d433aa6a5a21316.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
报错了---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/716e015cdcf647f38d737733accf50ea.png)
落了一个    /
注意事项---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/ecb6d7f0b916471f8ebd172f10731e75.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
再次运行--->
![在这里插入图片描述](https://img-blog.csdnimg.cn/348164abcc67401ea1ba2b3c590480e9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

前面提到了 配置文件的命名那个规则,以application开头的yam文件,yaml,或者properties文件
三种文件的配置格式---->>
properties配置---->>键值对的形式

![在这里插入图片描述](https://img-blog.csdnimg.cn/ac6c4c36de42453692339df48bc53045.png)
yml的配置形式--->>>层级关系
![在这里插入图片描述](https://img-blog.csdnimg.cn/72870d95480e47bf98ef64db8e575b98.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
yaml配置形式---->>层级关系
![在这里插入图片描述](https://img-blog.csdnimg.cn/746c06833cc54091923a76abf67d3bfa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这里的每一个. 都代表一个层级 
properties配置文件转换成yml/yaml之后,使用缩进代表层级关系
基本格式要求
①　大小写敏感
②　使用缩进代表层级关系
③　相同的部分只出现一次
④　注意配置参数与值之间的空格

自定义的配置信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/40261ba239e546acbb2736df7c706c31.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d881bc375054f688470b13a9ebc4636.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


既然有三种配置形式,那么优先级关系是怎样的呢??
当建立了三种配置文件,idea给出的图标提示如下---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/1f460214599c462b87ce49230a85fc56.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
分别在三个配置文件中添加内容,port端口号不同

![在这里插入图片描述](https://img-blog.csdnimg.cn/17040fa2d5444e12a17bf8f928ab418b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

properties的优先级最高;其次是yml,最后是yaml
如果properties中没有的配置内容,会再次扫描yml,以此类推

![在这里插入图片描述](https://img-blog.csdnimg.cn/cae7e0b9ddb0466390ee102630a080fd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
访问端口与路径
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d4f99481cfd4a5ab0eb07b5b24c70e6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
springweb与springjdbc配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/95265149d5e84237b045e04c2375ccb9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
但是就这样来讲,这些配置写法不太好做分类;
比较乱,
yml配置文件
```yml
server:
  servlet:
    encoding:
      charset: UTF-8
    context-path: /gavin
  port: 8050
```
yaml配置文件
```yaml
server:
  servlet:
    context-path: /gavin_war_exployded
  port: 8070
```
properties配置文件

```java
server.port=8080
server.servlet.context-path=/spring_boot_war_exploded
server.servlet.encoding.charset=UTF-8
server.error.path=/error.html
spring.datasource.url=jdbc:mysql://localhost:3306/gavin?timeZone=Asia/shanghai
spring.datasource.username=gavin
spring.datasource.password=955945
spring.datasource.driver-class-name=
spring.transaction.default-timeout=10
spring.jdbc.template.query-timeout=5
spring.datasource.tomcat.default-transaction-isolation=
spring.transaction.rollback-on-commit-failure=
```



### 配置文件存放的位置
1,网上说放在根目录下,为啥我试了不好使呢?

![在这里插入图片描述](https://img-blog.csdnimg.cn/67588f6404c94f649c97d31c8ecda328.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
2,放在根目录下的config文件夹下也不好使
![在这里插入图片描述](https://img-blog.csdnimg.cn/0d60bcb35f374fbab0669774ef6f9f7c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
3.放在classpath目录下,这个好使
![在这里插入图片描述](https://img-blog.csdnimg.cn/986f8032b25f40729bfe851254bcf488.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
4,放在classpath下的config文件夹下
![在这里插入图片描述](https://img-blog.csdnimg.cn/90eb350897c742f3abc9eb20efff45b7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/65e8871e11854ec1b06b007d2a416de8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
套娃也不行
![在这里插入图片描述](https://img-blog.csdnimg.cn/eeaa3806a86d4a9a90742077f2feba08.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可能是版本的问题吧,先不管他了;
#### 存放位置读取的优先级

![在这里插入图片描述](https://img-blog.csdnimg.cn/45518e2ce45b4a75bbea0ef199f943ac.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
8070胜利,即内层的config文件夹下被先读取;

> 为什么不是覆盖?
> 如果同一个目录下，有application.yml也有application.properties，默认先读取application.properties。
> 如果同一个配置属性，在多个配置文件都配置了，默认使用第1个读取到的，后面读取的不覆盖前面读取到的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/8307a84f7d9440dc99b811a8a451621d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

我习惯把配置文件统一放在classpath目录下;

## 导入其他配置类

 这 @Import注解可用于导入额外的配置类。 或者可以使用 @ComponentScan自动拾取所有 Spring 组件，包括 @Configuration类。

## 禁用特定的自动配置类
如果您发现正在应用您不想要的特定自动配置类，您可以使用的 exclude 属性 @  SpringBootApplication禁用它们

如果该类不在类路径中，则可以使用 `excludeName`注释的属性并指定完全限定名称 





## bootstrap配置文件

Spring Boot 中有两种上下文对象，一种是 bootstrap, 另外一种是 application(ServletContext), bootstrap 是应用程序的父上下文，也就是说 bootstrap 加载优先于 applicaton。bootstrap 主要用于从额外的资源来加载配置信息，还可以在本地外部配置文件中解密属性。这两个上下文共用一个环境，它是任何Spring应用程序的外部属性的来源。bootstrap 里面的属性会优先加载，它们默认也不能被本地相同配置覆盖。

### bootstrap配置文件特征

①boostrap 由父 ApplicationContext 加载，比 applicaton 优先加载。
②boostrap 里面的属性不能被覆盖。

bootstrap与 application 的应用场景
application 配置文件主要用于 Spring Boot 项目的自动化配置。
bootstrap 配置文件有以下几个应用场景。
①使用 SpringCloudConfig 配置中心时，这时需要在 bootstrap 配置文件中添加连接到配置中心的配置属性来加载外部配置中心的配置信息。
②一些固定的不能被覆盖的属性。
③一些加密/解密的场景。

##   SpringBootWeb 配置

在没有配置web项目之前的目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/2b2aa92df8854e478f49b3b70728b1c1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

建一个webapp目录发现idea只是把它当作一个普通目录来识别的,要想识别为项目资源目录,需要这样配置一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/c798e78c6e2a42168906873bc6e39339.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



[  SpringBoot对远程开发的支持](https://docs.spring.io/spring-boot/docs/2.6.3/reference/htmlsingle/#using.devtools.remote-applications)

#   SpringBoot与各组件之间的整合

## 方式一----继承

![在这里插入图片描述](https://img-blog.csdnimg.cn/a507f02a1a03495f879f0862a1f4f5a3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
通过idea创建  SpringBoot项目,可以根据需要去选择要配置的一些组件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.gavin.  SpringBootmybatis</groupId>
    <artifactId>  SpringBootbatis</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>  SpringBootbatis</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>11</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.1</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```
这种方式是通过继承父类来实现的;

## 方式二---引入独立依赖
手动引入mybatis依赖,web依赖即可
为什么我不建议初学者直接使用第一种方式?
1,虽然第一种方式方便快捷,但是毕竟是初学者,还是要搞懂原理;
2,另外,第一种方式是继承父类的,不便于后期功能扩展;

下面分析一下第二种方式,
首先搭建  SpringBoot项目,无论是  SpringBootweb还是  SpringBootmybatis,甚至  SpringBootjdbc,最基本的是  SpringBoot这个基石;

**第一步:建立一个maven项目**
![在这里插入图片描述](https://img-blog.csdnimg.cn/b96463f63a2b42cdbd1b2915b6435e57.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

第二步:引入基本的  SpringBoot依赖
![在这里插入图片描述](https://img-blog.csdnimg.cn/045ef21f80644f54ba62d7968e8bfe8a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然后在引入需要的启动器---->

web启动器/mybatis启动器/jdbc启动器(按需导入,这里一次性导入了.)

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.6.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
            <version>2.6.2</version>
        </dependency>

    </dependencies>

```
在这里我们可以看到启动器的命名规则上有一些区别------->>
一般来讲,官方的启动器命名规则是spring-boot-starter-** 在前,而第三方的是 **-spring-boot-starter;

# Spring案例整合

##   SpringBoot与mybatis整合案例

依赖分析---->可以看到相关的jdbc由于依赖关联而被自动导入;
并没有导入mysql的驱动
![在这里插入图片描述](https://img-blog.csdnimg.cn/4cbefcd81c334bbab77496af75b4cf29.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

之后按照需求导入

准备pojo类

```java
package com.gavin.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.io.Serializable;

/**
 * user
 * @author 
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User implements Serializable {
    private Integer id;

    private String name;

    private String pwd;

    private String nickname;

    private String picname;

    private String filetype;

    private static final long serialVersionUID = 1L;

}
```

运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/ee91c7d084944c31af31b2abcb096a43.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


注意的点:Mapper注解告诉容器该类是mybatis的接口
![在这里插入图片描述](https://img-blog.csdnimg.cn/67bcac1cc00c47abafc6eb86ef3ba2c0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
如果不写这个还可以在启动类上添加包扫描来实现  

@MapperScan,该注解下有很多参数,
最基本的value, basePackage,以及basePackageClasses等;
![在这里插入图片描述](https://img-blog.csdnimg.cn/f337fcab1c644603be9d847eb78ba9df.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



总结------>>
1,导入根基依赖spring-boot
2,按需导入其他依赖
3,检查其他依赖,着重检查可变依赖-----比如数据库依赖
4,配置依赖参数
5,编写相关类


##   SpringBoot整合日志记录

在  SpringBoot中不需要在单独导入log4j等日志记录依赖,因为在  SpringBoot中有专门的用于日志的记录组件----logback

logback读取配置文件的步骤----->
1,在classpath下查找文件 logback-test.xml
2,如果文件不存在,则查找logback.xml


logback配置文件--- 模板

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径-->
    <property name="LOG_HOME" value="${catalina.base}/logs/"/>

    <!-- 控制台输出 -->
    <appender name="Stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!-- 日志输出格式 -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
            </pattern>
        </layout>
    </appender>


    <!-- 按照每天生成日志文件 -->
    <appender name="RollingFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
            <FileNamePattern>/server.%d{yyyy-MM-dd}.log</FileNamePattern>
            <MaxHistory>30</MaxHistory>
        </rollingPolicy>
        <layout class="ch.qos.logback.classic.PatternLayout">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
            </pattern>
        </layout>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>10MB</MaxFileSize>
        </triggeringPolicy>
    </appender>

    <!-- 异常日志文件 -->
    <appender name="error_file"  class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
            <FileNamePattern>${LOG_HOME}/error/error.log.%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--日志文件保留天数-->
            <MaxHistory>30</MaxHistory>
        </rollingPolicy>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>500MB</MaxFileSize>
        </triggeringPolicy>
        <!-- 只打印错误日志 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>error</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>
    <!--连接数据库配置-->
    <appender name="db_classic_mysql_pool" class="ch.qos.logback.classic.db.DBAppender">
        <connectionSource class="ch.qos.logback.core.db.DataSourceConnectionSource">
            <dataSource class="org.apache.commons.dbcp.BasicDataSource">
                <driverClassName>com.mysql.cj.jdbc.Driver</driverClassName>
                <url>jdbc:mysql://127.0.0.1:3306/gavin?serverTimezone=Asia/Shanghai</url>
                <username>gavin</username>
                <password>955945</password>
            </dataSource>
        </connectionSource>
    </appender>
    <!--myibatis log configure-->
    <logger name="com.apache.ibatis" level="warn"/>
    <logger name="java.sql.Connection" level="warn" />
    <logger name="java.sql.Statement" level="warn"/>
    <logger name="java.sql.PreparedStatement" level="warn"/>


    <!-- 日志输出级别 -->
    <root level="debug">
        <appender-ref ref="Stdout"/>
        <appender-ref ref="RollingFile"/>
        <appender-ref ref="error_file"/>
        <appender-ref ref="db_classic_mysql_pool"/>
    </root>
    <logger name="com.gavin.mapper" level="DEBUG"></logger>
 </configuration>
```
注意---->>日志记录过程需要   还需要以下几个依赖

![在这里插入图片描述](https://img-blog.csdnimg.cn/45704e71ba1248c9b0ab9b9a74763106.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```xml
    <dependency>
        <groupId>commons-dbcp</groupId>
        <artifactId>commons-dbcp</artifactId>
        <version>1.4</version>
    </dependency>
    <!-- logback -->
    <dependency>
        <groupId>net.logstash.logback</groupId>
        <artifactId>logstash-logback-encoder</artifactId>
        <version>6.3</version>
    </dependency>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
        <version>1.2.3</version>
    </dependency>

```
少了一些依赖就会报一些错误,比如 *****    null   或者初始化的时候找不到...或者初始化失败;
最好的方式是把每一个依赖都去掉看看会报什么错;这里不在演示了;

别着急,如果配置了异步写入数据库,那么数据往哪里写?数据写入的字段又是些什么?

 这个在依赖的jar包里有答案---
![在这里插入图片描述](https://img-blog.csdnimg.cn/22fdfc2d40a94a7e883e1ce6ce59996d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
按照给出的sql语句创建sql表,然后执行  SpringBoot

![在这里插入图片描述](https://img-blog.csdnimg.cn/3eb50415f675404d9f70f1de1cc9c1dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/68875bc7c74b428ea5141919ed80b15c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

##   SpringBoot分页实现
回顾一下[springweb中的分页](https://blog.csdn.net/weixin_54061333/article/details/121107075)实现

在springweb中实现分页还是比较繁琐的,前端需要向后端发送页数以及页大小等信息然后后端需要向数据库查询,在去做处理;
在  SpringBoot中正常的查询业务只需要加上一行代码即可实现分页所需;

pagehelpers 的实现原理----->>

其在方法内使用了静态ThreadLocal参数,分页的参数和线程是绑定的,

ThreadLocal设置分页参数(pageindex,pagesize),

查询时获取参数
通过拦截器在sql中添加sql语句分业务数据参数,

实现分页查询

最后清除查询参数

案例演示---->>
准备数据库表格----->>goods表


```sql
DROP TABLE IF EXISTS `goods`;
CREATE TABLE `goods`  (
  `id` int NOT NULL AUTO_INCREMENT,
  `goodsName` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `price` float NULL DEFAULT NULL,
  `goodsDesc` varchar(200) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 12 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of goods
-- ----------------------------
INSERT INTO `goods` VALUES (3, '宫保鸡丁', 21, '宫保鸡丁哦');
INSERT INTO `goods` VALUES (5, '青椒肉丝', 22, '青椒肉丝不好吃');
INSERT INTO `goods` VALUES (8, '地三鲜', 15, '酸甜口');
INSERT INTO `goods` VALUES (9, '鱼香肉丝', 9, '四川风味');
INSERT INTO `goods` VALUES (10, '回锅肉', 12, '辣味');
INSERT INTO `goods` VALUES (11, '热狗肠', 32, '纯瘦肉版');
INSERT INTO `goods` VALUES (12, '烧花鸭', 56, '河南黄鸭');
INSERT INTO `goods` VALUES (13, '焖鸭掌', 35, '芦花鸭');
INSERT INTO `goods` VALUES (14, '肥肠鱼', 46, '香辣');
INSERT INTO `goods` VALUES (15, '松花小肚', 88, '猪肚');
INSERT INTO `goods` VALUES (16, '炸花生米', 12, '大花生');
INSERT INTO `goods` VALUES (17, '清蒸鲈鱼', 34, '海鲈鱼');

SET FOREIGN_KEY_CHECKS = 1;

```
添加pagehelper依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.4.1</version>
</dependency>

```


编辑查询方法    查询方法不在这里赘述，直接看结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/b2a62c13d73b4defa7f5562b5d4bb8ac.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
接下来就是实现分页数据

添加分页查询相关的方法


```java
@Service
public class GoodsServiceImp implements GoodsService {
    @Autowired
    private GoodsMapper goodsMapper;

    @Override
    public List<Goods> showGoods() {
        return goodsMapper.findGoods();
    }

    @Override
    public List<Goods> findByPage(Integer pageNum, Integer pageSize) {
//将分页数据传入
        PageHelper.startPage(pageNum,pageSize);
//这样查询数据时会按照页进行查询
        return goodsMapper.findGoods();
    }
}

```

首先先将pagehelper部分注释掉,
接下来对比一下

![在这里插入图片描述](https://img-blog.csdnimg.cn/e3cdca4d8fe749fe8d2f668f5e102e5f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

在没有添加pagehelper时,执行的sql语句---->>

```
 [http-nio-8080-exec-1] INFO  o.a.c.core.ContainerBase.[Tomcat].[localhost].[/] - Initializing Spring DispatcherServlet 'dispatcherServlet'
[http-nio-8080-exec-1] INFO  org.springframework.web.servlet.DispatcherServlet - Initializing Servlet 'dispatcherServlet'
[http-nio-8080-exec-1] INFO  org.springframework.web.servlet.DispatcherServlet - Completed initialization in 1 ms
[http-nio-8080-exec-1] INFO  com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Starting...
 [http-nio-8080-exec-1] INFO  com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Start completed.
[http-nio-8080-exec-1] DEBUG com.gavin.mapper.GoodsMapper.findGoods - ==>  Preparing: select id, goodsName, price, goodsDesc from goods
[http-nio-8080-exec-1] DEBUG com.gavin.mapper.GoodsMapper.findGoods - ==> Parameters: 
 [http-nio-8080-exec-1] DEBUG com.gavin.mapper.GoodsMapper.findGoods - <==      Total: 12

```
1,实例化DispatcherServlet
> Initializing Spring DispatcherServlet 'dispatcherServlet'
> 2,数据库连接池初始化
> com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Start completed.
> 3,查询数据库
> Preparing: select id, goodsName, price, goodsDesc from goods

在有pagehelper是我们再看
![在这里插入图片描述](https://img-blog.csdnimg.cn/521b1e453c0944f48101f684a83ae1a6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

查询非第一页时
![在这里插入图片描述](https://img-blog.csdnimg.cn/ca2a16b6a43743f9b70db0add5a70420.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
前端还可以接收到哪些信息呢?

PageHelper.startPage()方法是带有返回值的,

```java
 Page<Goods> goodsPage = PageHelper.startPage(pageNum, pageSize);
```


返回值都包含哪些信息呢?找到page类去看一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/a71b943602d640cd96595deb748ca8ed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
包含很多信息,我们都可以通过get方法来获取

```java
  System.out.println(goodsPage);
        System.out.println("---------------------");
        System.out.println("当前页--" + goodsPage.getPageNum());
        System.out.println("总页--" + goodsPage.getPages());
        System.out.println("页大小--" + goodsPage.getPageSize());
        System.out.println("总记录数--" + goodsPage.getTotal());
        System.out.println("当前页数据--" + goodsPage.getResult());
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f1d392bcfcee4a7fac21f74244888458.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
但是这些数据在查询完毕后就会被清除掉,要想保存下来,那么就需要将这些信息给封装起来

```java

        PageInfo<Goods> pageInfo= new PageInfo<>(goodsPage);
        System.out.println("当前页"+pageInfo.getPageNum());
        System.out.println("总页数"+pageInfo.getPages());
        System.out.println("页大小"+pageInfo.getSize());
        System.out.println("总记录数"+pageInfo.getTotal());
        System.out.println("当前页数据"+pageInfo.getList());
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/f6a71477897349d087f1036ca8e6fea4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

所以如果想要持久化保存分页数据以便前端能够用到，那么可以将返回值改为pageInfo

依赖分析---->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/dda730c7c1df4246bc42b7f388aed26b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
##   SpringBoot与Druid整合
其实很简单,就是把Druid启动器添加到依赖然后将原来jdbc连接池的部分替换为druid即可;


为什么要使用Druid连接池?在这篇文章中已经详细阐述了
[阿里的德鲁伊怎么玩?](https://blog.csdn.net/weixin_54061333/article/details/121541293)

引入druid启动器
```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.8</version>
</dependency>
```

看一下该依赖的依赖传递关系
![在这里插入图片描述](https://img-blog.csdnimg.cn/123ceac61dd542058fa21fe0f3a160a8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
接下来是配Druidpool,

```yml
spring:
  datasource:
    # type表示不在使用spring内部的连接池,而是使用阿里的Druid连接池
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 填写你数据库的url、登录名、密码和数据库名
    url: jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
    username: gavin
    password: 123456
  druid:
    # 连接池的配置信息
    # 初始化大小，最小，最大
    initial-size: 5
    min-idle: 5
    maxActive: 20
    # 配置获取连接等待超时的时间
    maxWait: 60000

```
启动项目

![在这里插入图片描述](https://img-blog.csdnimg.cn/9b03ce67fd0f43d989df2d1c294ce897.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
访问一下druid控制台
奇怪的事情又发生了,发生了404

![在这里插入图片描述](https://img-blog.csdnimg.cn/29dfe616f28149519695aabffa6bc1df.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

404-找不到,这是什么原因?
经排查是依赖的版本的问题,druid的版本太高了,
发现1.10版本可以正常访问,不太清楚什么原因

![在这里插入图片描述](https://img-blog.csdnimg.cn/71c9d202e7ce421da739f8263fc71503.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
druid连接池可以做一些列参数设定;
![在这里插入图片描述](https://img-blog.csdnimg.cn/9286bf96ad1d4305ba7886f69b6afdff.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
当设定192.168.135.145(虚拟机IP地址)可以访问druid控制台时.

![在这里插入图片描述](https://img-blog.csdnimg.cn/5d155e3e91654c61a05d3f2938c7ef63.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

当禁用192.168.135.145(虚拟机IP地址)访问时--->
发现并没有生效,这是为什呢?
![](https://img-blog.csdnimg.cn/3442dd8bcd704a41a1c4c770f1ec4ba4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
排查后发现少了一个参数enable
![在这里插入图片描述](https://img-blog.csdnimg.cn/d32747e6cc174758ba67c53fcf5f9204.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

如果不配置这个,则默认的为false,即谁都可以访问,
所以要配置enable为true使关于ip配置生效;

如果没有另一方式是通过配置类来实现 ,原理是通过StatViewServlet去处理IP地址;

```java
package com.gavin.controller;

import com.alibaba.druid.support.http.StatViewServlet;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @author Gavin
 */
@Configuration
public class DruidConfiguration {
    @Bean
    public ServletRegistrationBean<StatViewServlet> statViewServlet() {
        ServletRegistrationBean<StatViewServlet> srb = new ServletRegistrationBean
                (new StatViewServlet(), "/druid/*");
       //IP白名单(没有配置或者为空，则允许所有访问)
        srb.addInitParameter("allow", "127.0.0.1");
        //IP黑名单(黑白均有时，deny优先于allow) :
        //如果满足deny的即提示：Sorry, you are not permitted to view this page.
        srb.addInitParameter("deny", "192.168.135.145");
        //账号参数名必须为loginUsername
        srb.addInitParameter("loginUsername", "admin");
        //密码参数名必须为loginPassword
        srb.addInitParameter("loginPassword", "123456");
        //是否能够重置数据
        srb.addInitParameter("resetEnable", "false");
        return srb;
    }
}

```
然后将配置文件中的关于权限的部分注释掉,这样通过后端来限制并作出相应反馈处理;
![在这里插入图片描述](https://img-blog.csdnimg.cn/ae0a3ecd318c4d4184c7c5deb61e7707.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

##   SpringBoot整合jsp-----[旧技术]
引入jsp依赖

```
  <!--JSP依赖-->
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <scope>provided</scope>
        </dependency>
```

编写controller,并访问,duanduanduang

![在这里插入图片描述](https://img-blog.csdnimg.cn/6f559ffcc2e74dcdbb197f030cb3f129.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
啥情况?

oh,我这个工程是一个聚合工程,因此需要设置一下使得jsp生效;
![在这里插入图片描述](https://img-blog.csdnimg.cn/31392f3581c243728c6b3ea5884797be.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

设置之后
![在这里插入图片描述](https://img-blog.csdnimg.cn/51337d3978744dd9a8866d7c6b63ba2a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
jsp就到这里吧;

##   SpringBoot项目打包
>  SpringBoot项目可以是jar类型的maven项目，也可以是一个war类型的maven项目，取决于我们要不要整合jsp使用。但是不管是哪种项目类型，已经不是我们传统意义上的项目结构了
>在本地使用  SpringBoot的启动器即可访问我们开发的项目。如果我们将项目功能开发完成后，需要使用  SpringBoot的打包功能来将项目进行打包。


>  SpringBoot项目打包在linux服务器中运行:
>   ①jar类型项目会打成jar包:
>   jar类型项目使用  SpringBoot打包插件打包时，会在打成的jar中内置一个tomcat的jar。所以我们可以使用jdk直接运行该jar项目可，jar项目中有一个功能，将功能代码放到其内置的tomcat中运行。我们直接使用浏览器访问即可。
>   ②war类型项目会打成war包:
>   在打包时需要将内置的tomcat插件排除，配置servlet的依赖。将war正常的放到tomcat服务器中运行即可。

打包插件

```xml
 <build>
        <plugins>
           <plugin><!--  打包插件-->
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
    </build>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/8fe73a6bbd204e548a085faf1fa25c30.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/72916ae8bbf3403cbd0b723b27f8ebd4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**项目打成jar包运行**

.jar.original文件里的内容
![在这里插入图片描述](https://img-blog.csdnimg.cn/0701fe60599c44b6bd5e2807ee65006b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
打包好之后就可以直接在JDK环境中运行了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/70839195173846cd96cb5b69ffc0ef3d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

其实在打包的时候可以指定打包的类型----jar/war
默认为jar,打包的jar可以直接运行,如果打包成war可以直接部署在服务器上;(打包时已经继承了tomcat)

![在这里插入图片描述](https://img-blog.csdnimg.cn/d699133144ff4e8f9c5788424e81ad3c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**项目导出war包运行;**

打成war包由于是要将war包放在服务器上去运行,而服务器上是有自己的tomcat的,所以在打包war时要将spring boot自带的tomcat排除,(如果代码中需要req与resp,那么可以再次单独导入tomcat依赖,并将作用范围指定为编译时生效---provided);

```xml
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <!--    排除自带的tomcat-->
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <!--打包的时候可以不用包进去，别的设施会提供。事实上该依赖理论上可以参与编译，测试，运行等周期。
                相当于compile，但是打包阶段做了exclude操作-->
            <scope>provided</scope>
        </dependency>
```
打包好的项目目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/e19b170769ef4095ad0527b51b8d5091.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
war.original文件中的内容
![在这里插入图片描述](https://img-blog.csdnimg.cn/3c5c7a1cab0d4a029a6e6d48ab756df4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
接着将war包放到本地tomcat服务器webapp中

![在这里插入图片描述](https://img-blog.csdnimg.cn/7f649cc114774cc6b2009145d6ce7a41.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_17,color_FFFFFF,t_70,g_se,x_16)
然后运行本地tomcat,启动之后会自动扫描war项目.并将该war解压;

![在这里插入图片描述](https://img-blog.csdnimg.cn/1d0aac97b64b419cb584deab37428f36.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

这样可以访问我们的项目吗?----404找不到,什么原因?项目路径的问题,
![在这里插入图片描述](https://img-blog.csdnimg.cn/003f6ef8129d407690a960c054ac9034.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
我们来看tomcat解压后的项目上下文路径,
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c27ab7f8b3e4c43a2f5441864093579.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
所以访问时的路径如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/91478ba2ed0c464f904e01a75fdb47b3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
打成war项目的上下文路径由实际路径决定(wa包名)而不是按照我们指定的上下文路径,同时如果我们在项目中指定端口,那么这时候也会失效,而是按照服务器的配置去运行;
>如果我们使用的是tomcat7则需要将javax.el-api-3.0.0.jar包放到tomcat下        的lib目录中。

## Spring与FreeMarker整合

**Freemarker**

FreeMarker 是一款 模板引擎： 即一种基于模板和要改变的数据， 并用来生成输出文本(HTML网页，电子邮件，配置文件，源代码等)的通用工具。 它不是面向最终用户的，而是一个Java类库，是一款程序员可以嵌入他们所开发产品的组件。


freemaker在前后端交互中起一个什么样的作用呢?

首先来看freemarker所处的位置,是前端和后端之间,后端发来的数据经过模板生成了静态页面,然后发送到前端解析器去解析
![在这里插入图片描述](https://img-blog.csdnimg.cn/40caf3984b0a455e90f77693ecec174f.png)

查看依赖关系,发现了springcontext依赖,这也验证了freemarker在上下文关系中的作用
![在这里插入图片描述](https://img-blog.csdnimg.cn/1ca0c1b589c8419f8e185df906d1e363.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
编写一个freemarker模板---->>
就是一个静态的模板,模板内容按照html格式写,之不顾哦文件类型(扩展名为ftlh)

```html
<!DOCTYPE html>

<html lang="en">
<meta charset="UTF-8">
<head>
    <title>Title</title>
</head>
<body>
this is a free-Marker-style-page</br>

姓名:${name}</br>
年龄:${age}
</body>
</html>

```
简单的后端代码--->>>

```java
@Controller
public class FreeMarkerController {
    @RequestMapping("/test01")
    public String test01(Map<String, Object> map) {
        map.put("name", "张三");
        map.put("age", 18);
        return "index";
    }

}

```

访问一下---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/46e420fbca3f41e88449eafbf35ac78a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

验证-----如果ftlh是一个像jsp那样的动态页面.那么修改ttlh中内容会在重新编译后才生效,看一下源代码,是一个静态页面的内容;

![在这里插入图片描述](https://img-blog.csdnimg.cn/8fcfe5eb59b1415287c62b0900362074.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> freemarker并不关心数据的来源，只是根据模板的内容，将数据模型在模板中显示并输出文件;

所以一个简单的FreeMarker案例就是这样

![在这里插入图片描述](https://img-blog.csdnimg.cn/12f8d81811dc49bbbf57b1bc3cc9ebaa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

>  模板和静态HTML是相同的，只是它会包含一些 FreeMarker 将它们变成动态内容的指令：

通常模板文件存放在服务器上,当有人来访问这个页面， FreeMarker将会介入执行，然后动态转换模板，用后端得到的数据内容替换模板中 ${} 的部分， 之后将结果作为一个静态资源发送到访问者的Web浏览器中。


![在这里插入图片描述](https://img-blog.csdnimg.cn/0b1ee5744839417db02a5d9693e936e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

在FreeMarker中又两种数据模型
第一种hash表形式

```
(root)
  |
  +- animals
  |   |
  |   +- mouse
  |   |   |   
  |   |   +- size = "small"
  |   |   |   
  |   |   +- price = 50
  |   |
  |   +- elephant
  |   |   |   
  |   |   +- size = "large"
  |   |   |   
  |   |   +- price = 5000
  |   |
  |   +- python
  |       |   
  |       +- size = "medium"
  |       |   
  |       +- price = 4999
  |
  +- message = "It is a test"
  |
  +- misc
      |
      +- foo = "Something"
```
访问形式----->>animals.mouse.price。


令一种形式---->>
就时将每个属性的名换成序号

```
(root)
  |
  +- animals
  |   |
  |   +- (1st)
  |   |   |
  |   |   +- name = "mouse"
  |   |   |
  |   |   +- size = "small"
  |   |   |
  |   |   +- price = 50
  |   |
  |   +- (2nd)
  |   |   |
  |   |   +- name = "elephant"
  |   |   |
  |   |   +- size = "large"
  |   |   |
  |   |   +- price = 5000
  |   |
  |   +- (3rd)
  |       |
  |       +- name = "python"
  |       |
  |       +- size = "medium"
  |       |
  |       +- price = 4999
  |
  +- misc
      |
      +- fruits
          |
          +- (1st) = "orange"
          |
          +- (2nd) = "banana"
```

访问形式 animal[0].price

上述存储单值的变量 (size, price, message 和 foo) 称为 scalars (标量)

[FreeMarker基础知识](../freemarker/freemarker.md#FreeMarker)

## Spring与Thymeleaf整合

**Thymeleaf简介**

> Thymeleaf是一个现代的服务器端 Java 模板引擎，适用于 Web 和独立环境。

>Thymeleaf 的主要目标是为您的开发工作流程带来优雅的自然模板— HTML 可以在浏览器中正确显示，也可以作为静态原型工作，从而在开发团队中实现更强的协作。

>Thymeleaf 具有 Spring Framework 模块、大量与您最喜爱的工具集成，以及插入您自己的功能的能力，是现代 HTML5 JVM Web 开发的理想选择 — 尽管它可以做的还有很多。

[更多描述请转官网](https://www.thymeleaf.org/)
---------------------摘自官网描述--------------------

**Thymeleaf入门**

入门就是先整一个Thymeleaf程序

参照freemarker的整合,这里将freemarker的启动器换成Thymeleaf
依赖的变动就这么一点;

```xml
     <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
            <version>2.6.2</version>
        </dependency>


```
这里选择最新版本的依赖,如果出现一些其他问题,后面再去解决;

**freemarker与Thymeleaf的对比**

![在这里插入图片描述](https://img-blog.csdnimg.cn/9e696471e98d465d97c5dc99fc034b7f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**关于ThymeLeaf的配置**

[详细官网配置请见spring官网](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties.web)

![在这里插入图片描述](https://img-blog.csdnimg.cn/569cf4b916e24b13a282b04103a3e08f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
源码是这样写的:

```java
public class GTVGApplication {
          ...
    private final TemplateEngine templateEngine;
    ...
      
    public GTVGApplication(final ServletContext servletContext) {
        super();
        ServletContextTemplateResolver templateResolver = 
                new ServletContextTemplateResolver(servletContext);
        
        // HTML is the default mode, but we set it anyway for better understanding of code
        templateResolver.setTemplateMode(TemplateMode.HTML);
        // This will convert "home" to "/WEB-INF/templates/home.html"
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        // Template cache TTL=1h. If not set, entries would be cached until expelled
        templateResolver.setCacheTTLMs(Long.valueOf(3600000L));
        
        // Cache is set to true by default. Set to false if you want templates to
        // be automatically updated when modified.
        templateResolver.setCacheable(true);
        
        this.templateEngine = new TemplateEngine();
        this.templateEngine.setTemplateResolver(templateResolver);
        
        ...

    }

}
```
以上默认配置表示  thymeleaf使用UTF-8编码,
访问时前缀template,后缀为html,
即直接转发或重定向到这个文件即可;

直接访问这个文件不可以么?

![在这里插入图片描述](https://img-blog.csdnimg.cn/c7934a45060f407a9397cbc1947e3ecc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
还真不可以,这个跟freemarker一致,


> Thymeleaf在Spring Boot项目中放入到resources/templates中。这个文件夹中的内容是无法通过浏览器URL直接访问的（和WEB-INF效果一样），所有Thymeleaf页面必须先走控制器。

**Thymeleaf运行原理**

![在这里插入图片描述](https://img-blog.csdnimg.cn/6bf64607f6e348ce96144ddb537132c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
模板解析器

```java
//模板解析器是实现来自 Thymeleaf API 的接口的对象，全限定名称为：org.thymeleaf.templateresolver.ITemplateResolver
public interface ITemplateResolver {
    ...
  
    /*
     * Templates are resolved by their name (or content) and also (optionally) their 
     * owner template in case we are trying to resolve a fragment for another template.
     * Will return null if template cannot be handled by this template resolver.
     */
    public TemplateResolution resolveTemplate(
            final IEngineConfiguration configuration,
            final String ownerTemplate, final String template,
            final Map<String, Object> templateResolutionAttributes);
}
```
为了处理数据,并适应国际化的需求,模板引擎在获得request,response,还需要获得请求来自的地区;

```java
public class HomeController implements IGTVGController {

    public void process(
            final HttpServletRequest request, final HttpServletResponse response,
            final ServletContext servletContext, final ITemplateEngine templateEngine)
            throws Exception {
        
        WebContext ctx = 
                new WebContext(request, response, servletContext, request.getLocale());
        
        templateEngine.process("home", ctx, response.getWriter());
        
    }

}
```

> 准备好上下文对象后，现在我们可以告诉模板引擎使用上下文处理模板（按其名称），并将其传递给响应编写器，以便可以将响应写入它;

**ThymeLeaf展示数据**

**友情提醒**----在展示数据之前还需要引入一个命名空间

```
<html xmlns:th="http://www.thymeleaf.org">
```
因为我们在表单中使用的这些非标准属性是 HTML5 规范所不允许的。


1,Thymeleaf中表达式必须依赖标签而不能单独使用
2,标准变量表达式一般在开始标签中,以 th开头
3,语法为: <tag th:***="${key}"   ></tag>
4,表达式中可以通过${}取出域中的值并放入标签的指定位置
5,${}在这里不能单独使用,必须在 **th:后面的双引号**里使用

**小案例----->>**

比如后端---->>
```java
@Controller
public class ShowController {
    @RequestMapping("/goodsinfo")
    public String showInfo(Map<String,Object>map){
        map.put("goodsname","小鸡炖蘑菇");
        return "sample";
    }
}
```
前端:

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

这道菜是<p>"${goodsname}"</p>
<input type="text" th:value=${goodsname}>
</body>
</html>
```
html文件中th并没有把规定标签,所以要引入th空间;
注:这里时idea自带的提示,官方建议是引入
 xmlns:th="http://www.thymeleaf.org,

![在这里插入图片描述](https://img-blog.csdnimg.cn/d40da27b3fec4b359bfb78ee3956557d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

html源代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/4976db0e0fe04a1d9120c7340e8add12.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
@Controller
public class ShowController {
    @RequestMapping("/goodsinfo")
    public String showInfo(Map<String,Object>map){
        map.put("goodsname","小鸡炖蘑菇");
        map.put("username","wet");
        map.put("colorful","background-color: yellowgreen");
        return "sample";
    }
}

```
后端传过来的数据

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml" xmlns:color="http://www.w3.org/1999/xhtml"
      xmlns:style="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style type="text/css">

    </style>
</head>
<body>

这道菜是<p>"${goodsname}"</p><br>
<input type="text" th:value=${goodsname}><br>
<span style:background-color:red th:text="${goodsname}"></span>
<span th:style="${colorful}" th:text="${goodsname}"></span>
<button th:text="${goodsname}"></button>
<input type="text" name="name" th:value="${username}">

</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/000ba9b1a41648e286c40110fe933498.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
简单总结一下---->
Thymeleaf将后端的数据 用th:*来展现,且只能在前标签中才能生效;
基本书写格式----->>th:标签中属性名="${}"


如果为了更方便区分可以在属性前加 data-th-标签中属性名="$()"
这两种形式是完全等效的,自己习惯哪一种就用哪一种;
![在这里插入图片描述](https://img-blog.csdnimg.cn/f9cbbeb6d6c94124bf228b42c520e86b.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/db85a008db4a410aa47a23be98579daa.png)
以上内容均为白话形式,如有不当之处,还请参阅
[Thymeleaf官方文档](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#the-template-engine)

[Thymeleaf基础知识](../thymeleaf/ThymeLeaf.md.md#Thymeleaf)



##   SpringBoot对异常的友好处理

我们在写代码时有时候会遇到一些异常,如果直接返回异常的代码,对用户来讲并不是很友好,这事后我们你可以在templates文件夹下建立一个专门用于存放反馈异常信息的页面


异常返回页面的命名规则---是404错误,就将该页面命名为404,则发生404错误时会跳到这个页面;同理500
目录结构---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/afb3a603957b4a6fbadf4e8ebb3647ab.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
这时候在controller中手动添加一个异常  int i=1/0;
![在这里插入图片描述](https://img-blog.csdnimg.cn/344a662fe8454009a1bd56c14f7b73b9.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/6ad181b3205a4cf1a91e8eb10e54f73e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


但是又可能会遇到别的4**或者5**的错误,这样的话,我们可以将 页面命名为4xx,5xx,即可

还可以定义一个统一的页面---error.html

![在这里插入图片描述](https://img-blog.csdnimg.cn/54152f659fc14c73b7dc3fa160d23d59.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

异常跳转的优先级---->>

当template/error文件夹下有相应的错误反馈页面时会优先走这里,如果没有则会走template下的error.html;

按照有异常就要处理的原则,我们可通过注解的方式来完成异常的处理;

```java
 @ExceptionHandler(value = {ArithmeticException.class,NullPointerException.class})
    public String errorHandler(){

        return "forward:/templates/error";
    }
```
这是局部异常处理,如果要做到全局异常处理

定义一个全局异常处理--->>
此处优先级低于局部异常处理器


```java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(value = {ArithmeticException.class,NullPointerException.class})
    public String ExceptionHandler(){

        return "forward:/templates/error";
    }
}

```
自定义异常处理

```java
import java.util.Properties;
/**
 * @author Gavin
 */
    @Configuration
public class GlobalExceptionHandler {
    @Bean
    public SimpleMappingExceptionResolver getSimpleMappingExceptionResolver(){

        SimpleMappingExceptionResolver exceptionResolver= new SimpleMappingExceptionResolver();
        Properties properties= new Properties();
        properties.put("java.lang.NullPointException","../static/error.html");
        properties.put("java.lang.ArithmeticException","../static/error.html");
        exceptionResolver.setExceptionMappings(properties);
        return exceptionResolver;
    }
}

```
[还有一种是通过配置xml了来实现](http://varyu-program.gitee.io/code-m/#/spring/SpringMvc?id=%E8%87%AA%E5%AE%9A%E4%B9%89%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86),这里看链接吧;

```java

/**
 * @author Gavin
 */
    @Configuration
public class GlobalExceptionHandler implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
       ModelAndView modelAndView= new ModelAndView();
       if(ex instanceof ArithmeticException ){
           modelAndView.setViewName("../static/error.html");
       }
       if(ex instanceof NullPointerException){
           modelAndView.setViewName("../static/error.html");
       }
        return modelAndView;
    }
}
```
>还是  SpringBoot中自带的异常处理比较方便;


##   SpringBoot中的测试单元
![在这里插入图片描述](https://img-blog.csdnimg.cn/22cbf3494cb2425b8b4959310b410d30.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
test单元的运行逻辑----->>当运行测试单元时会加载spring的运行环境;
```java
package com.gavin.zzy;

import com.gavin.pojo.Goods;
import com.gavin.service.GoodsService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.  SpringBootTest;

import java.util.List;

@  SpringBootTest
class ZzyApplicationTests {
@Autowired
    private GoodsService goodsService;
    @Test
    void contextLoads() {
        List<Goods> goods = goodsService.showGoods();
        goods.forEach(System.out::println);
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/26308f9fe65345258846da924dae3357.png)
在测试的时候一般测试类的目录结构跟项目的目录结构保持一致;
若不这样可能由于版本的问题会出现一些错误;

> 1. 测试类不能叫做Test,会和注解同名
> 2. 测试方法必须是public
> 3. 测试方法返回值必须是void
> 4. 测试方法必须没有参数



##   SpringBoot对Bean的管理
Spring Boot 由于没有XML文件，所有的Bean管理都放入在一个配置类中实现。
配置类就是类上具有@Configuration的类。这个类就相当于之前的applicationContext.xml


1 新建一个配置类
com.codemar.config.MyConfig ， 在  SpringBoot中最好是单独建一个配置类文件夹,方便维护(虽然可以直接放在resources文件夹下);
注意：配置类要有@Configuration,方法要有@Bean

```java
@Configuration
public class MyConfig {
    //访问权限修饰符没有强制要求，一般是protected
    //返回值就是注入到Spring容器中实例类型。
    // 方法名没有强制要求,相当于<bean >中id属性。
    @Bean
    protected User getUser(){
        User user = new User();
        user.setId(1L);
        user.setName("张三");
        return user;
    }
    //自定义bean名称
    @Bean("user2")
    protected  User getUser2(){
        User user = new User();
        user.setId(2L);
        user.setName("李四");
        return user;
    }
}
```

如果Spring容器中存在同类型的Bean通过Bean的名称获取到Bean对象。或结合@Qualifier使用

```java
 @  SpringBootTest
public class TestGetBean {
    @Autowired
    @Qualifier("user2")
    private User user;
    @Test
    public void testGetUser(){
        System.out.println(user);
    }
}
```

在配置类的方法中通过方法参数让Spring容器把对象注入。
 //自定义bean名称

```java
@Bean("user1")
public  User getUser(){
    User user = new User();
    user.setId(2L);
    user.setName("李四");
    return user;
}
@Bean
//可以直接从方法参数中取到。
public People getPeople(User user1){
    People p = new People();
    p.setUser(user1);
    return p;
}
```
##   SpringBoot拦截器
首先配置一个拦截器

```java
package com.gavin.intercepter;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author Gavin
 */
@Component//bean注入
public class MyIntercepter implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截器执行了");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}

```
拦截器准备完毕后,拦截器不像其他的bean需要的时候注入即可,拦截器要的是全局,所以要配置一个配置类使得拦截器能够全局生效;

```java
package com.gavin.intercepter;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration//拦截器配置类
public class INTERCEPTERCONFIG implements WebMvcConfigurer {

@Autowired  //自动装配

 private MyIntercepter intercepter;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //注入拦截器,添加拦截路径与放行路径
        InterceptorRegistration interceptorRegistration = registry.addInterceptor(intercepter).addPathPatterns("/**") .excludePathPatterns("/login");
    }
}

```
> 配置拦截器 注意：类上有注解@Configuration。此类相当于SpringMVC配置文件。 addPathPattern():
> 拦截哪些URL。 /** 拦截全部 excludePathPatterns():
> 不拦截哪些URL。当和addPathPattern()冲突时,excludePathPatterns()生效。



##   SpringBoot中的注解
**@  SpringBootApplication**
查看源码可以发现该注解其实是一个符合注解,该注解等同于
@  SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.gavin")

三个注解,
![在这里插入图片描述](https://img-blog.csdnimg.cn/b48f7da9ff2f487097e4a9af810ef961.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
前面的文章已将对此做了解释,不在赘述了,只看结果;

![在这里插入图片描述](https://img-blog.csdnimg.cn/d2236cf5e91e42b2a4f48237af09a45f.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b728d764cfb94a848fbfed628af04d3b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**@Configration**

定义一个配置类,在配置类中注册一个bean
![在这里插入图片描述](https://img-blog.csdnimg.cn/911c840ce42d45a589fb4b2089522e42.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

在启动的时候就会将user的信息加载到上下文的域中,这个时候我们可以从上下文中取出这些信息;

```java
@  SpringBootApplication
public class ZzyApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(ZzyApplication.class, args);
        User getUser = context.getBean("getUser", User.class);
        System.out.println(getUser);
    }

}

```
这个注解跟前面文章提到的import注解还不太一样,,improt是通过无参构造来完成注入的;
比如说---->>

```java
package com.gavin.config;

import com.gavin.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

/**
 * @author Gavin
 */
@Configuration
@Import(value = User.class)
public class MyConfig {

    @Bean
    public User  getUser(){
        return new User("张三",23);
    }
}

```

```java
package com.gavin;
import com.gavin.config.MyConfig;
import com.gavin.pojo.User;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.  SpringBootConfiguration;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.  SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;

/**
 * @author Gavin
 */
@  SpringBootApplication
public class ZzyApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(ZzyApplication.class, args);
        User getUser = context.getBean("getUser", User.class);
        System.out.println(getUser);
        AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext(MyConfig.class);
        User getUser1 = annotationConfigApplicationContext.getBean("getUser", User.class);
        System.out.println(getUser1);
        String[] beanDefinitionNames = annotationConfigApplicationContext.getBeanDefinitionNames();
        for (String b :
                beanDefinitionNames) {
            System.out.println(b);
        }
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6da44fe99ac40ecbfe8cb57ce5f3ab2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到当我们注入bean时默认的注入bean的名为方法名------此时是按照类的type来装配的;

如果要按照id来装配,需要指定bean的id值;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a37c7312a573489e9c5236651e809232.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/6c2824be91d14977b36d2908a55709fd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

> 初始化加载的类----->>org.springframework.context.annotation.internalConfigurationAnnotationProcessor
> org.springframework.context.annotation.internalAutowiredAnnotationProcessor
> org.springframework.context.annotation.internalCommonAnnotationProcessor
> org.springframework.context.event.internalEventListenerProcessor
> org.springframework.context.event.internalEventListenerFactory

代码分析---->>

```java
   MyConfig bean = context.getBean(MyConfig.class);
        MyConfig bean2 = context.getBean(MyConfig.class);

//        spring中的单例设计   true
        System.out.println(bean==bean2);
        System.out.println("-----------------------");
        //从context中获取   true
        User user = bean.getUser();
        User user1 = bean.getUser();
        System.out.println(user==user1);
        System.out.println("-----------------------");
        // 自己new色结果为false,不做解释
            MyConfig M= new MyConfig();
        User user2 = M.getUser();
        User user3 = M.getUser();
System.out.println(user2==user3);
```
在@Configration注解中我们发现

```java
package org.springframework.context.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.core.annotation.AliasFor;
import org.springframework.stereotype.Component;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";

    boolean proxyBeanMethods() default true;
}

```
>  boolean proxyBeanMethods() default true;

代理Bean方法默认为true,所以 我们在容器中获取Myconfig对象并非是一个真实的对象,而是一个代理对象,所以多次从容器中取出的是同一个对象;

如果设置代理bean方法为false,其结果就会发生变化;
![在这里插入图片描述](https://img-blog.csdnimg.cn/5badc1e360004b6ab2258403df0574c3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



我们查看一下获得的对象真实名

```java
   MyConfig bean = context.getBean(MyConfig.class);
        String simpleName = bean.getClass().getSimpleName();
System.out.println(simpleName);
```

```java
MyConfig$$EnhancerBySpringCGLIB$$6c794284
```
[代理模式详解--->>](https://blog.csdn.net/weixin_54061333/article/details/121554945)

当我们设置`@Configuration(proxyBeanMethods = false)`
返回结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/f16b9c2a0d9a4e82a0dffffe74dddda4.png)

> MyConfig配置类本身也是一个spring容器中的bean
 * proxyBeanMethods=true 属性,给MyConfig对象产生一个代理对象
 * 通过代理对象控制反复调用MyConfig里面的方法返回的是容器中的一个单实例
 * 如果proxyBeanMethods=false 那么我们拿到的MyConfig对象就不是一个代理对象, 那么这个时候反复调用MyConfig中的方法返回的就是多实例
 > proxyBeanMethods=false 称之为Lite模式  特点启动快

 > proxyBeanMethods=true  称之为Full模式  特点依赖spring容器控制bean单例





**@ConfigureProperties**
在这个注解案例之前先回顾一下之前装配数据的方式---->>@Value

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
@Component
public class User implements Serializable {
@Value("周三干")
    private String uName;
@Value("18")
    private Integer uAge;
    private static final long serialVersionUID = -1L;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/065ccbfe7af44fa390b7600f2416f432.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
通过配置文件进行bean的注入
我们知道,  SpringBoot在扫描配置文件的时候会扫描yml,yaml以及properties配置文件,所以我们可以通过.properties的方式来完成bean中属性数据的注入----这就需要依靠@ConfigureProperties来完成,

```java
 
@Configuration
public class MyConfig {


```
application.properties文件
```xml
user.uName=zzy
user.uAge=29
```

```java
package com.gavin.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.io.Serializable;

/**
 * @author Gavin
 */
@AllArgsConstructor
@NoArgsConstructor
@Data

@ConfigurationProperties(prefix = "user")//匹配前缀,一般习惯类的小写
@Component//注册bean
public class User implements Serializable {

//@Value("周三干")

    private String uName;
//@Value("18")

    private Integer uAge;
    private static final long serialVersionUID = -1L;
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d5dfb7a883964440b8d5f71411ea88c7.png)
当然我们按照习惯会将注解添加到配置类上
则这时需要的注解有所改变---->>

**@ EnableConfigurationProperties** 
```java
package org.springframework.boot.context.properties;
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.context.annotation.Import;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import({EnableConfigurationPropertiesRegistrar.class})
public @interface EnableConfigurationProperties {
    String VALIDATOR_BEAN_NAME = "configurationPropertiesValidator";

    Class<?>[] value() default {};
}

```
案例

```java
@EnableConfigurationProperties(User.class)
public class MyConfig {

```

```java

@AllArgsConstructor
@NoArgsConstructor
@Data
@Component
@ConfigurationProperties(prefix = "user")

public class User implements Serializable {

//@Value("周三干")

    private String uName;
//@Value("18")

    private Integer uAge;
    private static final long serialVersionUID = -1L;
}

```
小结--->>>如果在实体类上注解--->>
@Component//注册bean
@ConfigureProperties(prefix="指定前缀"),
配置类上注解---->>>@Configuration
如果在配置类上,则实体类上
则只需要将配置类上注解换成@EnableConfigurationProperties(User.class)

##   SpringBoot中的条件注解

**@Conditional**
当触发特定条件时会装载该bean

该注解下有很多子注解,
![在这里插入图片描述](https://img-blog.csdnimg.cn/60c7df924f9f4cc79a08fb31b1d37e47.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7d368e48ccd1445fb9609da80b940202.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/14f31141b6694964a3bd0ce70cecd97a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**@ConditionalOnClass**
```java
package org.springframework.boot.autoconfigure.condition;
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.context.annotation.Conditional;

@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional({OnClassCondition.class})
public @interface ConditionalOnClass {
    Class<?>[] value() default {};

    String[] name() default {};
}

```
案例--->>>
```java
@ConditionalOnClass(name="com.mysql.cj.jdbc.Driver")
    @Bean(value = "myUser")
    public User  User1(){
        return new User("李四",23);
    }

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/26989ec5144440afaedcd82f95a92a3d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
当修改为
com.mysql.cj.jdbc.DriverDatasource时,

```java
Exception in thread "restartedMain" java.lang.reflect.InvocationTargetException
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:49)
Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'myUser' available
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeanDefinition(DefaultListableBeanFactory.java:872)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getMergedLocalBeanDefinition(AbstractBeanFactory.java:1344)
```
>在获取bean时会提示找不到该bean
>@ConditionalOnClass(name="aaa")注解在类aaa加载时才会装配该bean,其实这也体现在源码中,比如我们导入thmeleaf时,会自动导入相关配置依赖


![在这里插入图片描述](https://img-blog.csdnimg.cn/59f10c3e9f1c4c11bf68ceafd38047b5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
例如在没有引入freemarker时,
在  SpringBootautoconfig中的配置类信息中可以看到


![在这里插入图片描述](https://img-blog.csdnimg.cn/e24005fd1afd46a3b26c64042ae6097f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
这说明有一些配置类没有加载进来,如果这时候我们导入freemarker,再次查看

![在这里插入图片描述](https://img-blog.csdnimg.cn/782aa314f7c14bc9acbcb3b3912bd451.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
说明当没有导入freemarker时,有关于freemarker的配置信息不会加载

同理其他的一些依赖也是如此;
但是这也会引发一个问题,就是我肯不想
**@ConditionalOnProperty**配置类:
```java
package com.gavin.config;
import com.gavin.pojo.User;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Conditional;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

/**
 * @author Gavin
 */
@Configuration(proxyBeanMethods = false)
@Import(value = {User.class} )
public class MyConfig {

    @Bean(value = "myUser")
    @ConditionalOnProperty(name = "server.port" ,havingValue = "8080")
//当配置中有server.port且值为8080时会加载这个bean
    public User  getUser(){
        return new User("张三",23);
    }

}

```

```java
@  SpringBootApplication
public class ZzyApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(ZzyApplication.class, args);
        User getUser = context.getBean("myUser", User.class);
        System.out.println(getUser);

    }

}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7ef9a2a692b7476fa01c4c1526c4c4b8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

如果类似的注解在类上,那么这表示配置类中的所有bean都会被加载,

```java
@ConditionalOnProperty(name = "server.port" ,havingValue = "8080")
@Configuration(proxyBeanMethods = false)
@Import(value = {User.class} )
public class MyConfig {

```
那么就会产生一个问题----优先级的问题
类注解上有该注解,方法上也有注解;
发现当注解的信息名相同,但是值不同时,都会报错,即相同的配置只能有一个 ;

**@ConfigureProperties**
在这个注解案例之前先回顾一下之前装配数据的方式---->>

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
@Component
public class User implements Serializable {
@Value("周三干")
    private String uName;
@Value("18")
    private Integer uAge;
    private static final long serialVersionUID = -1L;
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/065ccbfe7af44fa390b7600f2416f432.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
通过配置文件进行bean的注入
我们知道,  SpringBoot在扫描配置文件的时候会扫描yml,yaml以及properties配置文件,所以我们可以通过.properties的方式来完成bean中属性数据的注入----这就需要依靠@ConfigureProperties来完成,

```java
 
@Configuration
public class MyConfig {


```
application.properties文件
```xml
user.uName=zzy
user.uAge=29
```

```java
package com.gavin.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.io.Serializable;

/**
 * @author Gavin
 */
@AllArgsConstructor
@NoArgsConstructor
@Data

@ConfigurationProperties(prefix = "user")//匹配前缀,一般习惯类的小写
@Component//注册bean
public class User implements Serializable {

//@Value("周三干")

    private String uName;
//@Value("18")

    private Integer uAge;
    private static final long serialVersionUID = -1L;
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d5dfb7a883964440b8d5f71411ea88c7.png)



## 在  SpringBoot中以xml方式引入bean(了解)
自己定义xml配置文件,在文件中配置bean,这需要在resources目录下准备一个xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.gavin.pojo.User">
        <property name="UName" value="李四五"/>
        <property name="UAge" value= '18'/>
    </bean>
</beans>
```

配置xml之后,还需要将扫描该文件,因为默认会扫描yml,yaml,properties文件,所以要通过注解来完成对xml文件的导入;

```java
@Configuration
@ImportResource(value = "classpath:beans.xml")
public class MyConfig {

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/05b1d4389c574928bbd82235de7e6f34.png)

##   SpringBoot中构造方法注入bean

可以添加 @ComponentScan没有任何参数或使用 @  SpringBootApplication隐式包含它的注释。 您的所有应用程序组件（ @Component, @Service, @Repository, @Controller, and others) 被自动注册为 Spring Beans。 

```java
import org.springframework.stereotype.Service;
@Service
public class MyAccountService implements AccountService {

    private final RiskAssessor riskAssessor;

    public MyAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
    }

    // ...

}


```


如果一个 bean 有多个构造函数，你需要标记你希望 Spring 使用的那个 

```java
@Autowired:

import java.io.PrintStream;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class MyAccountService implements AccountService {

    private final RiskAssessor riskAssessor;

    private final PrintStream out;

    @Autowired
    public MyAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
        this.out = System.out;
    }

    public MyAccountService(RiskAssessor riskAssessor, PrintStream out) {
        this.riskAssessor = riskAssessor;
        this.out = out;
    }

    // ...

}
```
##   SpringBoot的热插拔

**引入依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <version>2.6.3</version>
</dependency>

```
设置一下idea
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c927bf452f64601a9a09cb9aae4c822.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
设置registry
![在这里插入图片描述](https://img-blog.csdnimg.cn/296c7cf3929f44978897d7646e8aace0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/b073aa7e5f7447b3805273e3144a644a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


某些资源在更改时不一定需要触发重新启动。 例如，Thymeleaf 模板可以就地编辑。 默认情况下，更改资源 /META-INF/maven, /META-INF/resources, /resources, /static, /public， 或者 /templates不会触发重新启动，但会触发 实时重新加载 。 如果要自定义这些排除项，可以使用 spring.devtools.restart.exclude 。 例如，仅排除 /static和 /public您将设置以下属性：
特性
yaml

```yml
spring.devtools.restart.exclude=static/**,public/**
```



```yml
如果要保留这些默认值并 添加 其他排除项，请使用 spring.devtools.restart.additional-exclude而是 。 
```

当您对不在类路径上的文件进行更改时，您可能希望重新启动或重新加载您的应用程序。  为此，请使用 `spring.devtools.restart.additional-paths`属性来配置额外的路径来观察变化。  您可以使用 `spring.devtools.restart.exclude`属性来控制附加路径下的更改是触发完全重启还是 实时重新加载 

如果您不想使用重新启动功能，可以使用 `spring.devtools.restart.enabled` 。  在大多数情况下，您可以在您的 `application.properties`（这样做仍然会初始化重新启动类加载器，但它不会监视文件更改）。 

如果您需要 *完全* 禁用重新启动支持（例如，因为它不适用于特定库），您需要设置 `spring.devtools.restart.enabled` `System`  `false`打电话之前 `SpringApplication.run(…)`，如下例所示： 

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.  SpringBootApplication;
```

##   SpringBoot的延迟初始化
SpringApplication允许延迟初始化应用程序。 当启用延迟初始化时，bean 会在需要时创建，而不是在应用程序启动期间创建。 因此，启用延迟初始化可以减少应用程序启动所需的时间。 在 Web 应用程序中，启用延迟初始化将导致许多与 Web 相关的 bean 在收到 HTTP 请求之前不会被初始化。

延迟初始化的一个缺点是它会延迟应用程序问题的发现。 如果配置错误的 bean 被延迟初始化，则在启动期间将不再发生故障，并且只有在 bean 初始化时问题才会变得明显。 还必须注意确保 JVM 有足够的内存来容纳应用程序的所有 bean，而不仅仅是那些在启动期间初始化的 bean。 由于这些原因，默认情况下不启用延迟初始化，建议在启用延迟初始化之前对 JVM 的堆大小进行微调。 


可以使用以下方式以编程方式启用延迟初始化 lazyInitialization方法 SpringApplicationBuilder或者 setLazyInitialization方法 SpringApplication. 或者，可以使用 spring.main.lazy-initialization属性如下例所示：
特性
yaml

```yml
spring.main.lazy-initialization=true
```



	如果您想禁用某些 bean 的延迟初始化，同时对应用程序的其余部分使用延迟初始化，您可以使用显式将它们的延迟属性设置为 false @Lazy(false)注解。 



##   SpringBoot主页配置

方式1 配置静态资源欢迎页, index.html 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
这是竹叶

</body>
</html>
```

静态资源位置

![1644060697607](..\pic\1644060697607.png)

application.yml配置文件

```xml
server:
  port: 8080
  servlet:
    encoding:
      charset: UTF-8
    context-path: /gavin
spring:
  datasource:
    #    使用druid链接池
    type: com.alibaba.druid.pool.DruidDataSource
    #    数据库要素
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
    username: gavin
    password: 955945

    #    druid连接池配置
    druid:
      #      sql类型
      db-type: mysql
      initial-size: 5
      min-idle: 5
      max-wait: 20
      #       配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
      time-between-eviction-runs-millis: 300000
      validation-query: SELECT 1
      test-while-idle: true
      test-on-return: false
      #      打开PSCache，并且指定每个连接上PSCache的大小
      pool-prepared-statements: true
      max-pool-prepared-statement-per-connection-size: 20
      #      配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙

      filters: stat,wall,slf4j
      connect-properties: druid.stat.mergeSql\=true;druid.stat.slowSqlMillis\=5000
      #      配置DruidStatFilter
      web-stat-filter:
        enabled: true
        url-pattern: "/*"
        exclusions: "*.js,*.gif,*.jpg,*.bmp,*.png,*.css,*.ico,/druid/*"
      stat-view-servlet:
        #       IP白名单(没有配置或者为空，则允许所有访问)
        allow: 127.0.0.1,192.168.1.2
        #        IP黑名单 (存在共同时，deny优先于allow)
        deny: 192.168.135.145
        #        禁用HTML页面上的“Reset All”功能
        reset-enable: false
        login-username: admin
        login-password: 123456
        enabled: true
  mvc:
    format:
      date: dd/MM/yyyy
    #static-path-pattern: /res/** #静态资源前缀
  web:
    resources:
      static-locations: [classpath:/res/]

```



访问结果--->>>

![1644060598854](..\pic\zhuye)

 **注意此时不能配置静态资源前缀,否则访问时会找不到index.html**

```html
Whitelabel Error Page

This application has no explicit mapping for /error, so you are seeing this as a fallback.
Sat Feb 05 19:35:28 CST 2022
There was an unexpected error (type=Not Found, status=404).
No message available
```

方式2,我们还可通过controller来配置欢迎页

```java
   @RequestMapping("/")
    public String welcome() {
        return "forward:welcome.html";
    }
```

目录结构

![1644061329056](..\pic\1644061329056.png)



定义/与index都可以访问主页;

```java
  @RequestMapping(path={"/","/index"})
    public String welcome(){
        return "welcome";
    }
```



##  SpringBoot中的拦截器

这里模拟一个登录界面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link type="text/css" href="css/mycss.css">
    <script src="js/myjs.js"></script>
</head>
<body>
<img src="./weichatcode.png">
<form action="login">
    <input type="text" name="username" >
    <input type="submit">
</form>
</body>
</html>
```

controller层

```java
  @RequestMapping("/login")
    public String login(String username, HttpServletRequest request){
        //如果参数符合要求,那就往域中添加信息
        if(null != username && !"".equals(username)){
            request.getSession().setAttribute("username", username);
            return "main";
        }
       // 否则重定向到登陆界面
        return "redirect:/login.html";
    }
```

那么谁来验证是否登录过呢?

拦截器----->

```java
package com.gavin;

import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author Gavin
 */
public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object username = request.getSession().getAttribute("username");
        // 如果登录过,那么就放行
        if(null != username){
            return true;
        }
        // 如果没登陆过,那么就回到登录页,重定向
        response.sendRedirect("login.html");
        return false;
    }
}

```

有了拦截器也不会生效---因为没有在 SpringBoot中注册该拦截器

注册拦截器

```java
package com.gavin.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

public class InterceptorReg implements WebMvcConfigurer {
    //注册拦截器

    @Autowired
    private LoginInterceptor loginInterceptor;

    //配置拦截器的映射

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginInterceptor).addPathPatterns("/**").excludePathPatterns("/login","/login.html","/css/**","/js/**","/img/**","/font/**");
    }
}

```

## SpringBoot文件上传与下载

文件的上传与下载

在springmvc阶段要实现文件的上传下载,需要的依赖--->>


```xml
 <!--文件上传依赖-->
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.4</version>
    </dependency>
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.8.0</version>
    </dependency>
```

在springboot中其实自带了一下关于文件上传和下载的依赖

```java
    package com.gavin.fileupload;
       import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.context.ConfigurableApplicationContext;
    
    /**
     * @author Gavin
     */
    @SpringBootApplication
    public class FileuploadApplication {
    
        public static void main(String[] args) {
            ConfigurableApplicationContext context = SpringApplication.run(FileuploadApplication.class, args);
            String[] beanDefinitionNames = context.getBeanDefinitionNames();
            for (String name : beanDefinitionNames) {
                System.out.println(name);
            }
        }
    
    }
```


运行结果--->>>

```
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration
    multipartConfigElement
    multipartResolver
    spring.servlet.multipart-org.springframework.boot.autoconfigure.web.servlet.MultipartProperties
    org.springframework.boot.devtools.autoconfigure.LocalDevToolsAutoConfiguration$RestartConfiguration
    restartingClassPathChangedEventListener
    classPathFileSystemWatcher
    classPathRestartStrategy
    fileSystemWatcherFactory
```

所以我们就不需要在引入文件上传组件---multipartResolver
springboot实现文件的上传

1,准备一个服务器,用于存放上传的文件;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a41c52b2e4054961afee6bf406c039e6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
修改端口号和修改只读为false;

启动文件服务器
![在这里插入图片描述](https://img-blog.csdnimg.cn/e5b34f88c9dd4596a4ffb02e6acb5289.png)
跨服务器上传/下载的依赖

先搭建一个框架--->>

登录页-->

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>登录页面</title>
      <link rel="stylesheet" href="css/fileCss.css" type="text/css"/>
    </head>
    <body>
    <div class="main">
        <h2>欢迎访问学生管理系统</h2>
        <form action="login.do" method="post" class="form">
            账号: <input type="text", name="name"/></br>
            密码: <input type="password", name="pwd"/></br>
            <button type="submit">登录</button><br> <button><a href="registPage.html"><span style="color: white; ">注册</span> </a></button><br>
        </form>
    </div>
    </body>
    </html>

```

注册页代码

```html
    <!DOCTYPE html>
    <html lang="en">
    <meta charset="UTF-8">
    <head>
        <title>注册页</title>
        <link rel="stylesheet" href="css/login.css">
        <link rel="stylesheet" href="css/font-awesome.css">
        <script type="text/javascript"
                src="js/jquery-3.5.1.min.js">
        </script>
        <style>
            input::-webkit-input-placeholder {
                color: white;
            }
    
            input::-moz-placeholder {
                /* mozilla Firefox 19+ */
                color: white;
            }
    
            input:-moz-placeholder {
                /* mozilla Firefox 4 to 18 */
                color: white;
            }
    
            input:-ms-input-placeholder {
                /* Internet Explorer 10-11 */
                color: white;
            }
    
            /*给进度条君添加一些样式*/
    
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
    </head>
    
    <body>
    <form action="addUser.do" method="POST">
        <div id="login-box">
            <div class="form">
                <div class="item">
                    <i class="fa fa-user-circle" aria-hidden="true"></i>
                    <span style="color: white; font-size: large; ">账号:</span></font><input type="text" name="name" id="name"
                                                                                           placeholder="请输入账号"
                                                                                           onblur="checkUserName()"/>
                    <div id="checkNameMsg"></div><!--检查信息-->
                    <div id="checkMultiName"></div>
                    <br>
                </div>
    
                <div class="item">
                    <i class="fa fa-key" aria-hidden="true"></i>
                    <span style="color: white; font-size: large; ">密码:</span><input type="password" name="pwd" id="pwd"
                                                                                    placeholder="请输入密码"
                                                                                    onblur="checkPwd()"/>
                    <div id="checkPwdMsg"></div>
                    <br>
                </div>
    
                <div class="item">
                    <i class="fa fa-nick" aria-hidden="true"></i>
                    <span style="color: white; font-size: large; ">昵称:</span><input type="text" name="nickname"
                                                                                    id="nickname"
                                                                                    placeholder="请输入昵称"
                                                                                    onblur="checkNickName()"><br>
                    <div id="checkNickMsg"></div>
                </div>
    
                <div class="item">
                    <i class="fa fa-nick" aria-hidden="true"></i>
                    <span style="color: white; font-size: large; ">头像:</span><input type="file" id="upFile" value="选择文件"/>
                    <a href="javascript:void(0)" id="uploadFile" onclick="loadPic()">立即上传</a>
                    <br>
                    <div id="divimg" style="width: 100px;height: 150px">
                        <img id="img" style="width: 140px;height: 180px;" alt="您还未上传图片"/>
                    </div>
                    <br>
                    <div class="progress">
                        <div>
                        </div>
                    </div>
                    <div id="divNum"></div>
                </div>
    
                <button type="submit">注册</button>
            </div>
        </div>
    </form>
    
    <script type="text/javascript">
    
    
        function checkUserName() {
            var reg1 = /^[a-zA-Z]{5,10}$/;
            var userName = $("#name").val();
            if (!reg1.test(userName)) {
                $("#checkNameMsg").html("<span style='color: red; '>输入5-10个以字母开头的字串</span>");
                return false;
            }
            $("#checkNameMsg").html("");
            $.ajax({
                url: "checkUserName.do",
                type: "post",
                /*  dataType: "json",*/
                data: userName,
                contentType: "text/html;charset=UTF-8",
                success: function (data) {
                    if (data =="error:Repeated") {
                        $("#checkMultiName").html("<span style='color :red'>该账号已被注册</span>");
    
                        return false;
                    } else {
                        $("#checkMultiName").html("<span style='color :green'>该账号可用</span>");
                        return true;
                    }
                }
            });
            // $("#checkNameMsg").html = ("<font style='color :green'>OK</font>");
        }
    
        function checkPwd() {
            var reg1 = /^(\w){6,10}$/;
            var userPwd = $("#pwd").val();
    
            if (!reg1.test(userPwd)) {
                $("#checkPwdMsg").html("<font color ='red'>只能输入6-10个字母、数字、下划线</font>");
                return false;
            }
    
            $("#checkPwdMsg").html("<font style='color :green'>OK</font>");
        }
    
        function checkNickName() {
            if ($("#nickname").val().length == 0) {
                $("#checkNickMsg").html("<span style='color :red'>请输入昵称</span>");
                return false;
            }
            $("#checkNickMsg").html("<span style='color :green'>OK</span>");
        }
    
        function loadPic() {
    
            var photoFile = $("#upFile")[0].files[0];
    
            // 将文件传到这个对象中
            var formData = new FormData();
    
            formData.append("headerPicture", photoFile);
    
            $.ajax({
                    url: "/fileUpload2.do",
                    data: formData,//传送的数据
                    type: "post",
                    processData: false,//告诉浏览器发送的是一个对象   请求数据
                    contentType: false,//告诉浏览器 请求数据的类型 二进制类型
                    // dataType: "json",
                    success: function (data) {//接收返回来的数据并修改img标签里的内容
                        //console.log(data);
                       alert(data.msg);
                        $("#img").attr("src", data.newFileName);
                    },
                    xhr: function () {
                        var xhr = new XMLHttpRequest();
                        xhr.upload.addEventListener('progress', function (e) {
                            console.log(e);
                            var progressRate = (e.loaded / e.total) * 100 + '%';
                            if (photoFile != null) {
                                $("#divNum").text(progressRate);
                            }
                            if (photoFile != null) {
                                $('.progress > div').css('width', progressRate);
                            }
                        })
                        return xhr;
                    }
                }
            );
        }
    </script>
    
    
    </body>
    
    <footer>
        <div id="message-box">Hello, I'm CodeM!</div>
    </footer>
    
    </html>
```


主页代码

```html
    <!DOCTYPE html>
    <html lang="en">
    <html>
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <span data-th-text="${user.name}"></span>,welcome to here!!
    <button><a style="text-decoration-line: none"  href="showUser.do">查看所有用户</a></button>
    </body>
    </html>
```


注册成功页


```html
   <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>注册成功</title>
        <script type="text/javascript"
                src="js/jquery-3.5.1.min.js">
        </script>
    
    </head>
    <body>
    <span data-th-text="${msg}"></span>,两秒后返回登录页....,如果超时请  <a href="loginPage.html">手动返回</a>
    </body>
    </html>
```

下载页

 

```html
   <!DOCTYPE html>
    <html lang="en">
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
        <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
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

controller层

```java
    package com.gavin.controller;
    import com.gavin.mapper.UserMapper;
    import com.gavin.pojo.User;
    import com.sun.jersey.api.client.Client;
    import com.sun.jersey.api.client.WebResource;
    import org.apache.ibatis.annotations.Param;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.tomcat.util.http.fileupload.IOUtils;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.beans.factory.annotation.Qualifier;
    import org.springframework.core.io.ClassPathResource;
    import org.springframework.core.io.support.PropertiesLoaderUtils;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.*;
    import org.springframework.web.multipart.MultipartFile;
    import org.springframework.web.servlet.ModelAndView;
    import javax.servlet.ServletOutputStream;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import javax.servlet.http.HttpSession;
    import java.io.IOException;
    import java.io.InputStream;
    import java.net.URL;
    import java.util.*;
    /**
     * @author Gavin
     */
    @Controller
    public class FileController {
        //上传到的文件位置---非本地
    
        private String newFileName = null;
        private String fileType = null;
    
       private static ClassPathResource classPathResource = new ClassPathResource("/config/location.properties");
        Properties properties = PropertiesLoaderUtils.loadProperties(classPathResource);
    
        String pic_location = properties.getProperty("pic_location");
    
    
        @Qualifier(value = "sqlSessionFactory")
        private SqlSessionFactory sqlSessionFactory;
    
        public FileController() throws IOException {
        }
    
        @Autowired
        public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
            this.sqlSessionFactory = sqlSessionFactory;
        }
    
        /**
         * @return 返回查到的用户信息
         * @Desc 文件展示
         */
        @RequestMapping("/getAllUser")
        @ResponseBody
        public List<User> getAllUser() {
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            List<User> users = mapper.getAllUser();
    //        返回一个json对象
            return users;
        }
    
    
        /**
         * 首页
         */
        @RequestMapping("/")
        public String Welcome(/*HttpServletRequest request*/) {
    
            return "loginPage.html";
        }
    
        /**
         * @param user 前端传来的用户信息
         * @return 处理结果
         * @Desc 登录检查
         */
    
        @PostMapping("/login.do")
        public String loginSys(@Param("user") User user, HttpServletRequest request) {
         //  System.out.println(user);
    //        如果没登陆过
            //如果user不为空,则查询user是否存在
            if (null != user) {
                SqlSession sqlSession = sqlSessionFactory.openSession();
                UserMapper mapper = sqlSession.getMapper(UserMapper.class);
                Integer i = mapper.loginCheck(user);
                //如果存在则跳到main页面
                if (null != i) {
                    HttpSession session = request.getSession();
                    session.setAttribute("user", user);
                    return "../static/main.html";
                }
            }
            //如果不存在则重定向到
            // 登录页
            return "redirect:../static/loginPage.html";
    
        }
    
        /**
         * @param name 前端传来的用户名
         * @return 检验结果
         * @Desc 检查用户名唯一性
         */
        @RequestMapping("/checkUserName.do")
        @ResponseBody
        public String checkUser(@RequestBody String name) {
    
            SqlSession sqlSession = sqlSessionFactory.openSession();
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            Integer result = mapper.checkUser(name);
            sqlSession.close();
            if (null != result) {
                return "error:Repeated";
            }
            return "right:OK";
        }
    
    
        /**
         * @param user 前端传来的用户注册信息
         * @return 响应结果
         * @Desc 检查用户名唯一性
         */
        @PostMapping("/addUser.do")
        public Object addUser(@Param("user") User user) {
            ModelAndView model = new ModelAndView();
            System.out.println(user.toString());
            if (null != user) {
                user.setPicname(newFileName);
                user.setFiletype(fileType);
                System.out.println(user);
                SqlSession sqlSession = sqlSessionFactory.openSession();
                UserMapper mapper = sqlSession.getMapper(UserMapper.class);
                int i = mapper.registerUser(user);
                sqlSession.close();
                if (i == 1) {
                    model.addObject("msg", "注册成功");
                    model.setViewName("../static/success.html");
    
                } else {
                    //        如果用户注册失败(网络原因,其他原因,那么注册的图片还要删除,这里不做处理)
                    model.addObject("msg", "注册失败");
                    model.setViewName("../static/registPage.html");
                }
            }
            return model;
        }
    
    
        @RequestMapping("/fileUpload2.do")
        @ResponseBody
        public Map<String, String> upDataPicture(MultipartFile headerPicture, HttpServletRequest request) throws IOException {
            Map<String, String> map = new HashMap<>(1);
    //        如果文件不存在
            if (headerPicture == null) {
                map.put("msg", "请上传图片!");
                return map;
            }
    //有文件,则获取文件名
            String originalFilename = headerPicture.getOriginalFilename();
    //        控制文件的大小
            Integer size = 1024 * 1024 * 10;
            if (headerPicture.getSize() > size) {
                map.put("msg", "文件大小不超过10M");
                return map;
            }
    //        避免名字冲突,用UUID替换文件名,但是扩展名不变
            String uuid = UUID.randomUUID().toString();
    //        获取拓展名,只支持jpg与png
            String[] exName = {".jpg", ".png"};
            String extendsName = originalFilename.substring(originalFilename.lastIndexOf("."));
            if (!(exName[0].equals(extendsName) || exName[1].equals(extendsName))) {
                map.put("msg", "文件类型不符合要求");
                return map;
            }
    //拼接成新名字
            newFileName = uuid.concat(extendsName);
            System.out.println(newFileName);
    
    
    //创建 jersey 包中的client对象,用于跨服务器请求
            Client client = Client.create();
            System.out.println(client);
            WebResource resource = client.resource(pic_location + newFileName);
            String result = resource.put(String.class, headerPicture.getBytes());
            System.out.println(result);
            map.put("msg", "文件上传成功");
    //        返回新生成的文件名
            map.put("newFileName", pic_location + newFileName);
            fileType = headerPicture.getContentType();
            map.put("fileType", fileType);
            return map;
        }
        @RequestMapping("/showUser.do")
        public String showUser() {
    
            return "../static/picDown.html";
        }
    
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
            InputStream inputStream = new URL(pic_location + picname).openStream();
    //        浏览器获得输出文件
            ServletOutputStream outputStream = resp.getOutputStream();
            IOUtils.copy(inputStream, outputStream);
        }
    
    }
```



简单的测试一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/71f77cbaefcc4144ae6f9d84534b0eed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/4a68a72949864884953e83317ffe49c5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


登陆成功界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/afc52f04b2934967bbbbceda75f715dd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
注册成功界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/195877790c1d430eaf966eb40ee7af95.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

下载页面![在这里插入图片描述](https://img-blog.csdnimg.cn/cb1e750a46b049e58caf5f22cc1e7d99.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



更多源码信息请关注微信下方公众号,回复"上传下载"获取!

正在更新ing,最新动态请关注个人博客--[CodeMartain](https://blog.csdn.net/weixin_54061333?spm=1010.2135.3001.5421) 

![](..\pic\div\weichatcode.png)