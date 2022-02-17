![](..\pic\div\chuan.gif)

在学习之前要知道啥是SSM,他是Spring ,SpringMVC跟Mybatis的简称;
按照字母顺序我们应该先学习Spring

Spring框架体系
![在这里插入图片描述](https://img-blog.csdnimg.cn/8f8971b91b904e339b339fcd3307efb3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 本次笔记内容主要是核心框架,AOP,ASPECT以及transaction

# Spring

## 什么是Spring?
网上和书上有很多定义,简单来说就是 
 IOC---->控制反转
 DI--->依赖注入
 AOP---->内核框架

首先如果不是maven方式配置环境的话,需要手动下载Spring

[请移步官网----->>](https://repo.spring.io/ui/native/libs-release-local/org/springframework/spring/5.2.9.RELEASE)

先看一下spring框架的目录结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/58ac831a0a8b414fbdda40557fc3950c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

重点在于lib目录下----->>![在这里插入图片描述](https://img-blog.csdnimg.cn/d951b101dcfe4e92a1ef8660223689f8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)其中由有个核心包----->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/d0075375633941f7bfe38381e4af9903.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
第三方依赖包----

[核心容器需要依赖一个JAR包----commons-logging-1.2.jar](https://dlcdn.apache.org//commons/logging/binaries/commons-logging-1.2-bin.zip)

基本环境需要的jar包都已经介绍完毕!


几个重要概念----->>

### 控制反转---IOC
什么叫控制反转?
在传统的面向对象编程中,获取对象的基本方式  new 一个,这时是由我们主动创建的,而在Spring中对象的生命周期由Spring框架提供的IOC容器来管理即我们可以直接从IOC容器中获取一个对象;


我们在表示小明有一本书的时候,我们常常在 小明这个类中去new 一个Book,在Book类中去new一个小明,这样对象之间的练习就很紧密,如果引入了人IOC,小明和书之间失去了直接联系,当小明需要这本书的时候由IOC容器创建一个书对象,这时候就不需要我们手动去创建书对象了;
一张图表示----->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/1666dcd8ab8c4bdab452f4089b731e31.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
### 依赖注入---DI
跟控制反转是同一件事情,只不过是描述的对象不一样,这次主人公是书
就是主动与 被动的区别-----有点神奇!!!

#### IOC与DI给开发带来的好处---
1,可维护性比较好,因为减少了代码的耦合性,使得修改代码时尽可能的减少对整体程序的影响;
2,每个团队可以专注于自己要实现的业务逻辑,需要别的团队的代码时可以通过IOC/DI来实现;
3,增强了代码的复用性---即我们可以把常用的代码抽取成一个类/方法以供各个开发组使用;
4,项目可快速部署---具有热插拔的特性;



#### IOC/DI的实现
Spring 主要依靠4个核心jar包和一个第三方jar包来实现;


IOC的简单实现----->>>>
准备工作------>
1,新建一个maven工程
![在这里插入图片描述](https://img-blog.csdnimg.cn/cda1188269fc4aedbabd2cc0f7c52f55.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

2,引入依赖---- 
到maven仓库中引入依赖
commons-logging
spring-context
spring-context-support
spring-expression
spring-core
![在这里插入图片描述](https://img-blog.csdnimg.cn/507c889a8a5f44d8a7ced70f7cc23a3b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

3,编写测试代码

目路结构---->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/6020f9060df54f82be23046963a18f1e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
代码实现---->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/a7a272576e9c48cc8e0d367a01321738.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## Spring中Bean详解

### bean的配置
bean的配置方式有两种方式,一个是通过xml来配置,一个是通过注解方式来配置,工作中常用注解方式完成配置;
![在这里插入图片描述](https://img-blog.csdnimg.cn/f76f12bc8ae24f76a9dc42e2b9d194b5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)Bean中可以配置的子元素/属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d137feb95774307ab41f1a1d013d8d7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 如果在Bean中未指定id和name，那么Spring会将class值当作id使用。

### bean的作用域 
Spring中常用的作用域是  singleton 和prototype

![在这里插入图片描述](https://img-blog.csdnimg.cn/667b4ae6cc874bac97683de92e047573.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
指定作用域----

![在这里插入图片描述](https://img-blog.csdnimg.cn/f3c8762169944f8c8b9331f8f81d41a4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/c6964548c5034263a0e824de555d8fe8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 如果不设置scope="singleton"，其输出结果也是一个实例，因为Spring容器默认的作用域就是singleton。

>
> singleton作用域对于无会话状态的Bean（如Dao组件、Service组件）来说是最理想的选择。
>

>
> 对需要保持会话状态的Bean应用使用prototype作用域。


## bean的装配方式
bean的装配方式有基于xml方式,注解方式以及自动装配
### 基于xml方式的装配
Spring提供了两种基于XML的装配方式：设值注入（Setter Injection）和构造注入（ConstructorInjection）,

#### 依赖注入

##### Setter方式注入
![在这里插入图片描述](https://img-blog.csdnimg.cn/7c9cfcfe152f454196373f8b960a4c94.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)例中的se**()方法完成属性赋值，从而实现依赖注入。其name属性表示Bean实例中的相应属性名，ref属性用于指定其属性值;

在Spring配置文件中需要使用<bean>元素的子元素<property>来为每个属性注入值；

注意---->设值注入的方式---

**1,Bean类必须提供一个默认的无参构造方法。
2,Bean类必须为需要注入的属性提供对应的setter()方法。**


![在这里插入图片描述](https://img-blog.csdnimg.cn/355c7cff2c47445cabe33df934cf4b2e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


##### 构造方法注入
![在这里插入图片描述](https://img-blog.csdnimg.cn/3bc2ea2f56c7427e99d3d879bdfc5c62.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/70c65067da9c416db0ce7a10f77628a7.png)

#### P命名空间与C命名空间

> 通过P和C命名空间简化装配

```xml
  xmlns:p="http://www.springframework.org/schema/p"
xmlns:aop="http://www.springframework.org/schema/aop"
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/029bc27a030c4fc19ff3f31f03abd47a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




### 基于注解的方式装配
当xml中装配的越来越多,会显得非常的乱,不容易维护;
此时可以用注解的方式来完成装配
注解的详解----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/0546b5b5a37e4655ae62c83ce1b6aadd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
再实际开发中问常常会习惯于将repository,servive,与controller注解的作用域进行限制,使其只对某个包下(或者更小的方位内有效;
)
 use-default-filters="false"
    默认值为true 代表使用默认的扫描过滤器
    默认的扫描过滤器会识别并包含 @Component @Controller @Service @Repository 四个注解
    不使用默认的filter,使用我们自己的filter

![在这里插入图片描述](https://img-blog.csdnimg.cn/6dafb45f3f49477e8f5f0ec7194d94d8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



### 依赖注入的实际应用---->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/39514dcba64d4b4b83472d7b875e3ddb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果出现-----

```java
“java.lang.NoClassDefFoundError:org/springframework/aop/TargetSource”错误。
```
则是因为Spring-aop包没有导入

注解完成自动装配---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/658bfc699b8c4ab7b06f6a0dc65e93bf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/24a1647ea3774f8f951290bc97ef76c9.png)

## SpringAop

> Spring的AOP模块是Spring框架体系结构中十分重要的内容，提供了面向切面编程的实现。

### 什么是SpringAop
SpringAop是Spring中面向切面的一种编程方式,是对面向对象编程的一种补充;

#### Aop设计的初衷---->>>
这里纯属意淫,如有不当之处还望各路大神指正!

> 在传统的面向对象过程中,有很多地方用到的代码重复的地方太多,虽然可以用继承的方式来提高代码复用性,但继承的局限性是得代码的复用性得不到很好的放大;
>

> 那可不可以将相同类型的代码抽取出来,在编译或者运行的时候将这些代码应用到需要的地方-----SpringAop

**SpringAop框架有两个,一个是SpringAOP(纯java实现,不需要专门的编译过程和类加载器,在运行期间通过代理方式向目标类注入代码)一个是ApectJ是一个基于Java语言的AOP框架,提供了一个专门的编译器，在编译时提供横向代码的植入。**

> 新版本的Spring框架建议使用AspectJ来开发AOP。



### SpringAop中的一些术语
**Aspect**
切面 ----即java中的类,通常是用于横向插入系统中功能实现的类

**Joinpoint**
即我们要在哪里插入时做的一个标记


**Pointcut**
指类或者方法名


**Advice**
通知增强处理,指在定义好的切入点处要执行的程序代码

**Target Object**
被通知的对象

**Proxy**
将通知应用到目标对象后,被动态的创建的对象

**Weaving**
将切面代码插入目标对象上，从而生成代理对象的过程。


### AOP的实现方式
实现方式跟注入方式类似,都可以通过XML注入或者通过注解方式注入


常用的xml配置
可以当作模板来用,不过一般习惯用注解的方式

```xml
<!--    定义一个bean-->
    <bean id="myAspect"  class="com.gavin.myAspect.MyAspect"/>
<!--    将bean转为切面bean-->
<!--    aop配置-->
    <aop:config>
<!--        配置切面-->
<aop:aspect id="aspect" ref="myAspect">
<!--    定义切入点-->
    <aop:pointcut id="myPointCut" expression="execution(* com.gavin.myAspect.*.*(..))"/>
<!--    在切入点之前运行的代码,一般用于检查验证之类的-->
    <aop:before method="myBefore" pointcut-ref="myPointCut"/>
<!--    运行时要插入的功能代码-->
<aop:after-returning method="myAfterReturning" returning="returnVal" pointcut-ref="myPointCut"/>
<!-- 环绕通知-->
   <aop:around method="myAround" pointcut-ref="myPointCut" />
<!--    最终通知-->
    <aop:after-throwing method="myAfterThrowing" pointcut-ref="myPointCut" throwing="e"/>

<aop:after method="myAfter" pointcut-ref="myPointCut"/>
</aop:aspect>
    </aop:config>
</beans>
```

#### 基于xml的声明式AspectJ

注意,在配置时可能会遇到一点问题比如在导入JoinPoint时候爆红无法导入-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/9f809b6b5a8a4a6ca6adaa76ccf81268.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 后来才发现是作用域的问题,scope作用域标签中改为complie或者直接去掉.保持默认----complie


下面是具体的AOP实现

```java
public class MyAspect {
 public void myBefore (JoinPoint joinPoint){
  System.out.print("前置通知---权限检查,");
  System.out.print("目标类--"+joinPoint.getTarget());
  System.out.println(",被植入的增强处理的目标方法为--"+joinPoint.getSignature().getName());
  }
 public void myAfterReturning(JoinPoint joinPoint)  {
  System.out.println("后置通知,模拟日志记录");
  System.out.println(",被植入的增强处理的目标方法为--"+joinPoint.getSignature().getName());
 }
 public Object myAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
  System.out.println("环绕开始");
//  invoke current method
  Object proceed = proceedingJoinPoint.proceed();
  System.out.println("环绕结束");
  return proceed;
 }
 public void myAfterThrowing(JoinPoint joinPoint, Throwable e)  {
  System.out.println("出现异常:"+e.getMessage());
 }
 public void myAfter(JoinPoint joinPoint)  {
  System.out.println("最终通知:结束后释放资源");
 }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

<bean id="userDao" class="com.gavin.Dao.UserDaoImp"/>

<!--    定义一个bean-->
    <bean id="myAspect"  class="com.gavin.myAspect.MyAspect"/>
<!--    将bean转为切面bean-->
<!--    aop配置-->
    <aop:config>
<!--        配置切面-->
<aop:aspect id="aspect" ref="myAspect">
<!--    定义切入点-->
    <aop:pointcut  expression="execution(* com.gavin.Dao.*.*(..))" id="myPointCut"/>
<!--    在切入点之前运行的代码,一般用于检查验证之类的-->
    <aop:before method="myBefore" pointcut-ref="myPointCut"/>
<!--    运行时要插入的功能代码-->
<aop:after-returning method="myAfterReturning"  pointcut-ref="myPointCut"  returning="joinPoint"/>
<!-- 环绕通知-->
   <aop:around method="myAround" pointcut-ref="myPointCut" />
<!--异常通知-->
    <aop:after-throwing method="myAfterThrowing" pointcut-ref="myPointCut" throwing="e"/>
    <!--    最终通知-->
<aop:after method="myAfter" pointcut-ref="myPointCut"/>
</aop:aspect>
    </aop:config>
</beans>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/fd2480719c7d437bb2ec064b088022fa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
开始配置时出现`java.lang.ClassCastException: class jdk.proxy2.$Proxy9 cannot be cast to class com.gavin.Dao.UserDaoImp (jdk.proxy2.$Proxy9 is in module jdk.proxy2 of loader 'app'; com.gavin.Dao.UserDaoImp is in unnamed module of loader 'app')
`


原因在于 proxy代理模式的原理,返回的是一个接口,所以 要用接口来接,而不是实现类来接返回值;

> 使用<aop:after-returning>配置的后置通知和使用<aop:after>配置的最终通知虽然都是在目标方法执行之后执行，但它们是有区别的。后置通知只有在目标方法成功执行后才会被植入，而最终通知不论目标方法如何结束（包括成功执行和异常中止两种情况），它都会被植入。另外，如果程序没有异常，异常通知将不会执行。
>
>
> PROCCEDINGJOINPOINT用于控制在环绕通知中哪些在切入点之前执行,哪些在切入点之后执行

如果方法很多,那么基于xml配置的aspect的优势也会荡然无存,所以为了方便开发,使用注解来减少代码配置的繁琐是个不错的选择;

#### 基于注解的声明式AspectJ
首先熟悉AspectJ的注解
![在这里插入图片描述](https://img-blog.csdnimg.cn/2b79e1000d064061846b70e133fabe93.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
基于注解的开发
![在这里插入图片描述](https://img-blog.csdnimg.cn/a32792f631d84ff4a90b8be294f1dfb6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 相对来说，使用注解的方式更加简单、方便，所以在实际开发中推荐使用注解的方式进行AOP开发。


![在这里插入图片描述](https://img-blog.csdnimg.cn/a75b00fe7cc745dbba51b970b722b5d2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
> 如果在同一个连接点有多个通知需要执行，那么在同一切面中，目标方法之前的前置通知和环绕通知的执行顺序是未知的，目标方法之后的后置通知和环绕通知的执行顺序也是未知的。



如果遇到下面的错误类型很可能是切入点配置出现了问题;

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userDao' defined in class path resource [applicationContext.xml]: Initialization of bean failed; nested exception is java.lang.IllegalArgumentException: warning no match for this type name: com.gavin.Dao [Xlint:invalidAbsoluteTypeName]

```
全注解开发Aop主要是用aspect来实现
![在这里插入图片描述](https://img-blog.csdnimg.cn/6585a61781eb42f39ab67ed6811a4af5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


## SpringJdbc

> Spring的JDBC模块负责数据库资源管理和错误处理，大大简化了开发人员对数据库的操作，使得开发人员可以从烦琐的数据库操作中解脱出来，从而将更多的精力投入编写业务逻辑中。

### SPringJdbc核心
JdbcTemplate是SpringJdbc的核心,
继承自抽象类JdbcAccessor，同时实现了JdbcOperations接口。（1）JdbcOperations接口定义了在JdbcTemplate类中可以使用的操作集合，包括添加、修改、查询和删除等操作。（2）JdbcTemplate类的直接父类是JdbcAccessor，该类为子类提供了一些访问数据库时使用的公共属性

![在这里插入图片描述](https://img-blog.csdnimg.cn/35e7cc0dc8ac49b488777377cbd8f45d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 在Spring源码中可以看到数据库连接的操做的影子,所以使用Spring框架开发能够简化开发人员面对数据库是重复的一些操做,同时能够降低代码的耦合度,便于开发;

![在这里插入图片描述](https://img-blog.csdnimg.cn/3aea7c44bed2452f8c2f231bf55e5d3d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### SpringJdbc的配置

#### 基于Xml方式的配置
模板文件---->>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--    配置数据源用于连接数据库-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://127.0.0.1:3306/gavin?"/>
<property name="username" value="gavin"/>
    <property name="password" value="955945"/>
</bean>
<!--配置jdbc连接模板-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
<!--        配置连接的数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
<!--配置注入类-->

    <bean id="user" class="注入实现类">
        <property name="jdbcTemplate" ref="jdbcTemplate"/>
    </bean>
........省略号........

</beans>
```

在SpringJdbc中封装了很多对数据库操做的方法

### SpringJDBC中的常用方法

还是基本操做
1,新建一个项目,
2导入Spring核心包---4个
core  , context   , context-support  ,expression    
外部依赖包  common-loging
3,导入数据库驱动jar包
4,导入SpringJdbc相关的包
spring-jdbc , spring-tx
5配置springjdbc模板文件
然后开始写代码
**SpringJDBC创建一个表**
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa9d8bbdeb1448d5923da4ec2948940f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)代码就很多简单了,是不是比传统JDBC实现简单的多了

配置文件详解
![在这里插入图片描述](https://img-blog.csdnimg.cn/1b104f500a1b48aa98aff9c775053670.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**SpringJDBC更新表中数据**
包括插入,删除,更新操做都用update来操做

配置文件中多了注入类的Bean
![在这里插入图片描述](https://img-blog.csdnimg.cn/b38ec2732f874b38a1d3f400482c3072.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c6917c992ac64c6aab7d12730cfa80f3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**SpringJDBC查询表中数据**

先看API中常用方法

![在这里插入图片描述](https://img-blog.csdnimg.cn/46ac253e4332453f9ac2af9a91b3d715.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)选几个常用的方法------>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/2c56ccbb47f44f568d6dc788ee8082c7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
实际应用---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/1ef43e2fb4174b3dbb8171f725d542f1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)放几条执行的结果----嘻嘻
![在这里插入图片描述](https://img-blog.csdnimg.cn/533c0d128e584c1d84cc01fce9188242.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**真是大大简化了jdbc的代码,不用开发人员去管理每个数据库接口的声明周期;**


## Spring的事务声明
声明式事务管理：基于XML方式的声明式事务和基于Annotation方式的声明式事务。

Spring中事务生命的核心Jar包
Spring-tx
在maven中直接引入Transaction依赖就可以了;

在包中有三个用于事务管理的接口
![在这里插入图片描述](https://img-blog.csdnimg.cn/82c53cacf81a45ebb2ba731010edd5f7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**PlatformTransactionManager**

> PlatformTransactionManager接口是Spring提供的平台事务管理器，主要用于管理事务。

TransactionStatus getTransaction(TransactionDefinition definition)：用于获取事务状态信息。该方法会根据TransactionDefinition参数返回一个TransactionStatus对象。TransactionStatus对象表示一个事务，被关联在当前执行的线程上。
　void commit(TransactionStatus status)：用于提交事务。　void rollback(TransactionStatus status)：用于回滚事务


针对不同的持久层框架,需要使用事务管理接口不同的实现类来实现事务管理;

**DataSourceTransactionManager：用于配置JDBC数据源的事务管理器。**

**HibernateTransactionManager：用于配置Hibernate的事务管理器。**


**JtaTransactionManager：用于配置全局事务管理器。**

关于获取事物描述的一些方法---->>>

> string getName()：获取事务对象名称。
> int getlsolationLeve()：获取事务的隔离级别。　 
> int getPropagationBehavior()：获取事务的传播行为。 　
> int setTimeout()：获取事务的超时时间。
> boolean isReadOnly()：获取事务是否只读。


事务的管理主要是针对于数据的插入,更新
和删除,必须要进行事务管理,如果没有指定事务传播行为,Spring默认为Required;

![在这里插入图片描述](https://img-blog.csdnimg.cn/788d3f39094345628f2dd592f988271f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**TransactionStatus**

> 用于描述事务的状态

![在这里插入图片描述](https://img-blog.csdnimg.cn/e5ee40cc8cc44f49b9dd3add47ebd442.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### Spring中事物的管理方式

#### 基于xml实现声明式事务管理
新建一个maven项目,
引入核心依赖
SpringJdbc
数据管理由于要用到数据库,所以要引入数据库驱动;
xml方式实现事务管理要写通知来声明事务,最后用AOP自动生成代理;
引入 aop,aspectj

![在这里插入图片描述](https://img-blog.csdnimg.cn/259b8b12250f4bc6b4298586e095fdd8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
xml中的基本配置
数据库连接部分配置是一样的方式,
然后是变化的部分
tx-advice 来编写增强通知

最后编写aop
![在这里插入图片描述](https://img-blog.csdnimg.cn/f42683fc932b4d8ba47f3b5ab768050a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)在没有异常的情况下执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/a7f5fc0559df4c36aaa0a50e7d2007c3.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/0f3f44c68d1043d28f9dc76cc7d0772c.png)
改回原始值-1000,并创建一个异常,再次测试
报异常,数据库中的值没有变化

![在这里插入图片描述](https://img-blog.csdnimg.cn/309d9df8565d41ca9c5b1b453e9c846f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



#### 基于Annotation实现声明式事务管理
![在这里插入图片描述](https://img-blog.csdnimg.cn/afeb8bbeb56e4a1096a1887f833a709f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



> 如果使用注解式开发，就需要在配置文件中开启注解处理器，指定扫描哪些包下的注解。
>

> 在实际开发中，事务的配置信息通常是在Spring的配置文件中完成的，而在业务层类上只需使用@Transactional注解即可，不需要配置@Transactional注解的属性。


至此Spring常用框架已经学习完毕; 21.11.25

## 补充内容----->

### 关于spring中引入集合的一些操做;

![在这里插入图片描述](https://img-blog.csdnimg.cn/60bdda61cfa44a119e8814de1c91ac6f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

除此之外还也已定义一个公共的集合空间,使得该集合能被多个bean引用

![在这里插入图片描述](https://img-blog.csdnimg.cn/21ce4e6d66664ea4a39b14343171631c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/b32f438746f94aafac1e6d657fa546e7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/94d4a5b9b37d4679b7f41462cf97cff7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)学完Spring基本框架之后,我们会发现Spring对Bean的一个管理,Bean是怎样的一个生命周期呢?


## Bean的生命周期---->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/e438dde17d8643f6a545bac17b042519.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> BeanPostProcessor接口作用：

如果我们想在Spring容器中完成bean实例化、配置以及其他初始化方法前后要添加一些自己逻辑处理。我们需要**定义一个或多个BeanPostProcessor接口实现类，然后注册到Spring IoC容器中。**


1、接口中的两个方法都要将传入的bean返回，而不能返回null，如果返回的是null那么我们通过getBean方法将得不到目标。
2、ApplicationContext会自动检测在配置文件中实现了BeanPostProcessor接口的所有bean，并把它们注册为后置处理器，然后在容器创建bean的适当时候调用它，因此部署一个后置处理器同部署其他的bean并没有什么区别。**而使用BeanFactory实现的时候，bean 后置处理器必须通过代码显式地去注册**，在IoC容器继承体系中的ConfigurableBeanFactory接口中定义了注册方法


前置通知和后置通知一定会执行的,跟是否我们初始化Bean没有关联;


使用纯注解开发---->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/70e7a4d150c94f979e4a296f1d9fcaf4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



有时候使用纯注解开发会难以理解,因为要有一个脉络,不然很容易出错,所以xml配合注解实现会更好!


[案例开发1](https://gitee.com/varyu-program/jdbcbatch.git)

[案例开发2](https://gitee.com/varyu-program/spring-zone)

Spring5中还用到了最新的日志log4j2,跟log4j的不同之处在于log4j2是使用xml来配置的;

顺带说一下log4j2

> spring5框架自带了通用的日志封装,也可以整合自己的日志  1)spring移除了
> LOG4jConfigListener,官方建议使用log4j2  2)spring5整合log4j2 导入log4j2依赖

![在这里插入图片描述](https://img-blog.csdnimg.cn/3b66bb42f3324bbfb0576b609344fff0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
依赖的传递性使得导入impl就使得其他依赖一同导进来了,非常方便,maven的方便体现出来了;

log4j2.xml文件配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="DEBUG">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{YYYY-MM-dd HH:mm:ss} [%t] %-5p %c{1}:%L - %msg%n" />
        </Console>
    </Appenders>
    <Loggers>
        <Root level="debug">
            <AppenderRef ref="Console" />
        </Root>
    </Loggers>
</Configuration>
```
# Spring中的代理模式

早接触java设计模式时就了解过很多设计模式,记得我接触到的设计模式----->>>
单例模式
观察者设计模式
适配器模式
工厂设计模式
代理设计模式

本文主要写一些对于代理模式的理解

还是发一下以前写的代理模式

现在有一种情况.
例如：张扬属于老好人.也不差钱.马跃向张扬借了10000块.规定一年后还.一年之后.当张某再次向 马某讨债的时候.张某在无奈之下找到 范某.范某经营一家讨债公司.基本上手法：刀子。手枪
范某为了成功的把钱讨回来.准备好了笑道.绳索.钢筋.钢锯
马某害怕了.之后还钱
债讨回来了.范某要销毁罪证。
张某 -----范某------讨债     


讨债工作由范某完成.但是真正的债主是张某.凡剖只是代理张某去完成讨债工作；所以在java中.我们将这种设计模式称为代理设计模式；

传统的纯java代码
```java
interface Give {
	public void giveMoney();}
class RealGive implements Give {
	public void giveMoney() {
		System.out.println("还钱");	}}
class ProxyGive implements Give { // 讨债公司
	private Give give = null;
	public ProxyGive(Give give) {
		this.give = give;	}
	public void Before() {
		System.out.println("准备工具");	}
		public void giveMoney() {
				this.Before();
		this.give.giveMoney();// 代表真正讨债者完成讨债
		this.After();
	}
	public void After() {
		System.out.println("销毁罪证");
	}}
public class Nullable {
	public static void main(String args[]) {
		Give give = new ProxyGive(new RealGive());
		give.giveMoney();
	}
}
```
与时俱进一下,spring,换一个流程,张三要去吃饭,

```java
public interface Court {
    void doCourt();
}

```

```java

@Repository("people")
class People implements Court {
    @Value("张三")
    private String name;

    public People() {
    }

    public People(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public void doCourt() {
        System.out.println(this.name + ":我是无罪的");
    }
}
```

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Service;

@Service("law")
public class Law implements Court{
    @Autowired
    @Qualifier("people")
    private People people;
    @Override
    public void doCourt() {
        System.out.println("被告人"+people.getName()+"有不在场的证据");
        people.doCourt();
    }
}

```

```java
import com.gavin.Proxy.Law;
import org.junit.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
@ComponentScan(basePackages = "com.gavin")
public class testone {
    @Test
    public void test(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(testone.class);
        Law law = ac.getBean("law", Law.class);
        law.doCourt();
    }
}
```


> 原理基本是一样的,代理方和被代理方都是实现同一个接口,通过代理对象访问目标对象，这样可以在目标对象基础上增强额外的功能，如添加权限，访问控制和审计等功能。
>


> 但是这种方式属于静态代理,这就说明我们的一个静态代理类只能代理一个类，并且还要事先知道我们要代理哪个类才能写代理类，如果我们有其他类还想使用代理那就必须再写一个代理类。然而在实际开发中我们是可能是有非常多的类是需要被代理的，并且事先我们可能并不知道我们要代理哪个类。所以如果继续使用静态代理反而会增加许多的工作量，并且效率低下，代码复用率也不好。



总结

1在**不修改原有代码的 或者没有办法修改原有代码的情况下**  增强对象功能  使用代理对象 代替原来的对象去完成功能
进而达到**拓展功能**的目的
2JDK Proxy 动态代理面向接口的动态代理  一定要**有接口和实现类**的存在 代理对象增强的是实现类 在实现接口的方法重写的方法   
   生成的代理对象只能**转换成 接口**的不能转换成 被代理类
   代理对象只能**增强接口中定义的方法  实现类中其他和接口无关的方法是无法增强**的
   **代理对象只能读取到接口中方法上的注解 不能读取到实现类方法上的注解**



## JDK中的动态代理----PROXY

动态代理两种实现方式-----
实现接口的
继承父类的
 本质都是去覆写一些方法来完成代理;
再JDK中通过proxy来实现面向接口的代理;
还有一个第三方的 clib来实现面向父类的代理

代码实现------>>

准备,接口一个

```java
public interface Dinner {
    void eat();
    void drink();
}
```

准备两个实现类

```java

public class Person  implements Dinner{

    private String name;
    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public void eat(String foodName) {
        System.out.println(name+"在吃"+foodName);
    }

    @Override
    public void drink() {
        System.out.println(name+"在喝啤酒");
    }
}

```

```java
public class Student  implements Dinner{
    private String name;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public void eat(String foodName) {
        System.out.println(name+"在吃"+foodName);
    }
    @Override
    public void drink() {
        System.out.println(name+"在喝雪碧");
    }
}

```

准备动态代理的实现

首先要了解Proxy类的API,
不妨来一起看一下

继承实现关系
public class Proxy    extends Object  implements Serializable

类描述:
Proxy提供**静态方法来创建像实例一样的对象 接口**，但允许自定义方法调用。 **为某个接口创建代理实例**

构造方法---->

protected 	Proxy​(InvocationHandler h) 	

构建一个新的 Proxy来自子类的实例 （通常是动态代理类）具有指定值 为其调用处理程序。 

类中的方法---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/00e40a53e8e94f638ec62d29e228738d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
下面就开始实践----->>

```java

import org.junit.Test;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyDemo {
    @Test
    public void test(){
   Dinner dinner= new Person("李二狗");
   //get a proxy Instance
//        args
       /* ClassLoader loader, ---classloader
        Class<?>[] interfaces,  --- interface
        InvocationHandler h   ----invoke method
        */
        //get classloader
        ClassLoader classLoader=dinner.getClass().getClassLoader();
        Class<?>[] interfaces = dinner.getClass().getInterfaces();
        InvocationHandler invocationHandler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                Object returnVal;
               if(method.getName().equals("eat")){
                   System.out.println("饭钱洗手");
                   returnVal = method.invoke(dinner, args);
                   System.out.println("饭后刷碗");
               }else{
                   returnVal= method.invoke(dinner,args);
               }

                return returnVal;
            }
        };
       Dinner dinner1 = (Dinner)Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);
   dinner1.eat("米饭");
   dinner1.drink();
    }
}
```


代码解析----->>>
首先是两个是实现类,不做过多赘述,重点在于代理类

![在这里插入图片描述](https://img-blog.csdnimg.cn/e6c3689880584f1584b71bac625063a3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 第三方CJLIB实现动态代理

在spring中引入了人第三方动态代理,所以直接导入spring核心包就可以了

cjlib是通过子类覆写父类方法来达到方法增强的目的;

举个例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f319fd26cfd43659e13748919c3aba4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 通过配置cjlib实现动态代理

![在这里插入图片描述](https://img-blog.csdnimg.cn/d7fdc0eebefe4eb5897d2163601a1f20.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)







# 纯注解模式下使用SpringJdbc事务管理

最近发现some网友真是越来越过分了,在问答区抛个习题张口就要代码,这届网友真的是我带过最差的一届(当然我也属于这一届);

> 俗话说旁观者清,但是身为一个在农村长大的孩纸,我总是那么的不合群,算是出淤泥而不染的一个!!!


纯注解开发SpringJDBC,老师说先做到会用,然后再慢慢....慢慢....
最后你就会了!

话不多说,先整一个老生常谈的案例------银行转账
首先准备一个数据库

```sql
use gavin;
CREATE TABLE `bank` (
  `u_id` int NOT NULL,
  `u_name` varchar(8) DEFAULT NULL,
  `u_balance` double DEFAULT NULL,
  PRIMARY KEY (`u_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci


insert into `bank` (`u_id`, `u_name`, `u_balance`) values('1','小花','1000');
insert into `bank` (`u_id`, `u_name`, `u_balance`) values('2','小刚','1000');

```
然后创建一个maven工程,并导入依赖文件;


![在这里插入图片描述](https://img-blog.csdnimg.cn/4fc56c5183df44c2ad847fcba4517c51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)看起来跟多,如果精简一下的话还可以这样---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/e26682f34bcd42e69e8e35a75e24a8be.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)因为依赖的传递,有的依赖会自动将其依赖的jar导入;具体可以看IDEA左边的maven窗口;
啊,好像少一个德鲁伊连接池

目录结构--->![在这里插入图片描述](https://img-blog.csdnimg.cn/407b06ce14674d90b403c404637dce51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



[代码详细信息](https://gitee.com/varyu-program/spring-zone.git)

# Druid连接池



今天有机会接触到阿里的德鲁伊,德鲁伊的本质其实就是一个数据库连接池,在工作中不需要在自己写一个数据库连接池,直接拿阿里的连接池用它不香吗?

> Druid是由阿里巴巴推出的数据库连接池。它结合了C3P0、DBCP、PROXOOL等数据库连接池的优点。之所以从众多数据库连接池中脱颖而出，还有一个重要的原因就是它包含控制台,很方便的帮助我们实现对于sql执行的监控。
> 先放一段手写连接池

```java
package com.gavin.dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.LinkedList;

public class ConnectionPool {
   final static String DRIVER="com.mysql.cj.jdbc.Driver";
    final static String URL = "jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&cachePrepStmts=true&reWriteBatchedStatements=true&useServerPrepStmts=true";
    final static String USER = "gavin";
    final static String PASSWORD = "955945";
    private static LinkedList<Connection> pool;
    private static int iniSize = 5;
    private static int maxSize = 1;

    static {
        try {
            Class.forName(DRIVER);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //初始化链接池
        pool = new LinkedList<Connection>();
        //创建5个连接对象
        for (int i = 0; i < iniSize; i++) {
            //初始化
            Connection connection = initConnection();
            if (null != connection) {
                pool.add(connection);
                System.out.println("连接初始化"+connection.hashCode()+"放入连接池中");
            }
        }

    }

    //初始化连接---私有的
    private static Connection initConnection() {
        try {
            return DriverManager.getConnection(URL, USER, PASSWORD);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    public static Connection getConnection() {
        Connection connection = null;
        if (pool.size() > 0) {
            connection = pool.removeFirst();//移除集合中的一个元素
            System.out.println(connection.hashCode()+"连接被取走");

        } else {
            connection = initConnection();
            System.out.println("连接池空,创建新连接"+connection.hashCode()+"放入连接池中");

        }
        return connection;
    }

    //归还连接
    public static void returnConnection(Connection connection) {
        if (null != connection) {

            try {
                if (!connection.isClosed()) {
                    try {
                        connection.setAutoCommit(true);
                    } catch (SQLException e) {
                        e.printStackTrace();
                    }

                    if (pool.size() < maxSize) {
                        pool.addLast(connection);
                    } else {
                        try {
                            connection.close();
                        } catch (SQLException e) {
                            e.printStackTrace();
                        }
                    }
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }

        } else {
            System.out.println( "链接不符合归还要求,归还失败");
        }
    }
}
```
看起来能用,但是还是在企业实践之后的 德鲁伊更好,接下来就来初识德鲁伊数据库连接池;

首先新建一个Spring--maven 项目,

在maven仓库中找到德鲁伊连接池
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e43f79c584348e2b6dc90d5e303c0f2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
引入spring依赖以及druid依赖,配置好相关文件

接下来测试一下连接池能否链接数据库


![在这里插入图片描述](https://img-blog.csdnimg.cn/0f32d9e536d34fcfbc8bb0e55ef305f0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
开启测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/1363792ebd7f4da88768db3024f240f6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以看到德鲁伊对数据库连接的监控能里,其实在德鲁伊里还有很多属性可以装配以实现对数据库连接的更加详细的监控


![在这里插入图片描述](https://img-blog.csdnimg.cn/cd4608cd6a1243cebc0302074d1698fc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
具体代码如下

maven依赖---

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>SpringDruid</groupId>
    <artifactId>DruidJdbc</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.8</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.27</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context-support -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.18.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>compile</scope>
        </dependency>

    </dependencies>

</project>
```
applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--    配置数据源用于连接数据库-->
<!--    alibaba德鲁伊连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">

        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="gavin"/>
        <property name="username" value="gavin"/>
        <property name="password" value="955945"/>
        <property name="initialSize" value="1"/>
    </bean>
    <!--配置jdbc连接模板-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!--        配置连接的数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--配置注入类-->

    <!--<bean id="user" class="注入实现类">
        <property name="jdbcTemplate" ref="jdbcTemplate"/>
    </bean>
-->

</beans>
```


输出一下dataSource以及连接和连接数量

![在这里插入图片描述](https://img-blog.csdnimg.cn/c5640fa22ae9416d8c27ab41b897272e.png)测试代码--->

```java
package com.gavin.test;
import com.alibaba.druid.pool.DruidDataSource;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import java.sql.SQLException;
public class test {
@Test
    public void test() throws SQLException {
    ApplicationContext ac=new ClassPathXmlApplicationContext("applicationContext.xml");
    DruidDataSource dataSource = ac.getBean("dataSource", DruidDataSource.class);

    System.out.println(dataSource);
    System.out.println(dataSource.getConnection());
    System.out.println(dataSource.getConnectCount());
}

}

```

额,报错了,
![在这里插入图片描述](https://img-blog.csdnimg.cn/c4078a7ec51847f0b985e2636ee83a81.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
数据库连接驱动写错了
修改后
![在这里插入图片描述](https://img-blog.csdnimg.cn/7de591fe16764a45983ab4a1169868f7.png)
运行结果----![在这里插入图片描述](https://img-blog.csdnimg.cn/158e71e6d2d141799cf778288d005aba.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

但是企业对于一些编程是有要求的,例如,不建议将数据库连接参数直接放在bean配置文件中,而是单独拿出来,因为者是要初始化时候用的;

在resource文件夹中新建一个配置文件
jdbc.properties
![在这里插入图片描述](https://img-blog.csdnimg.cn/a8e876ef78b141e29dc86e261df60bc2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
额,报错了,
![在这里插入图片描述](https://img-blog.csdnimg.cn/21061105897d42f586d929f1623a4395.png)




为什么呢? 

 debug一下,看看是哪里出错了,
![在这里插入图片描述](https://img-blog.csdnimg.cn/2e8218b858f44511a47dfd005605d35e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)查了一下才知道,spring在读取配置文件的时候,由于电脑本地的名为username=Gavin,所以再去读取jdbc配置文件的内容时不会再修改username对应的value,所以发生了错误,
办法很简单,就是修改jdbc配置文件中的内容

![在这里插入图片描述](https://img-blog.csdnimg.cn/fe371281e9764ff29d5ffa629cf1b1b2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2690bf363ea04e7595ef887c8c4813cf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/bec0e3f84fe84fb2b82979f2137ac1f9.png)
[Java语言中最好的数据库连接池](https://zhuanlan.zhihu.com/p/157607448)



# Spring中@Value注解为属性赋值后控制台中文乱码



先看遇到的问题----控制台乱码;
![在这里插入图片描述](https://img-blog.csdnimg.cn/9657c5e2f22d4055a4a0d76458888b65.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
出现乱码时第一反应是去网上搜一搜解决方法参照网上方法操做
![在这里插入图片描述](https://img-blog.csdnimg.cn/94e928b07e454cddbeee224a618b5bc1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)都勾选了还时没解决,
后来我才发现,在非全注解模式下的配置文件------>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/52a7a1d9d5a54b1f96eaac5506b3b445.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这里有一个设置配置文件编码的,于是想着,注解应该也有能配置编码格式的属性吧!

![在这里插入图片描述](https://img-blog.csdnimg.cn/f42daec86b904a64a60b95b8575580e7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)修改为UTF-8之后,乱码问题就解决了!


![在这里插入图片描述](https://img-blog.csdnimg.cn/c27df5209203485b8716f35dc2e3feab.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



# Spring学习回顾

## Bean的装配方式

***Bean的装配方式有三种，***

1，通过xml方式装配，此种方式比较麻烦，但也是入门时候需要学习的内容；
2，通过annotation注解方式装配，此种方式是目前最常用的，要学习xml方式装配才能更好理解此种方式；
3，通过自动方式装配；

先从刚入门开始，在学习装配时要先掌握每种xml标签的作用---
***1，通过xml方式装配--***
*①通过设值方式装配--案例演示*

```java
package com.test.one.charpterday1;
import java.util.Set;
public class Teacher implements Person {
	private Student student;//setter注入，
	public void setStudent(Student student) {
		this.student=student;	}
//设置Teacher属性
	private String TeacherName;
	private String TeachSubject;
	private Set<String>TeacherHobby;
	public Teacher() {	//无参构造方法，这里演示是为了更清楚看到该构造方法，当类中无其他构造方法时会默认有一个无参构造
	}
	//SETTER方法省略，eclipse有自动生成getter和setter的方法
	public String toString() {
		return "我是-"+this.TeacherName+",教授-"+this.TeachSubject+","
	          +"我的爱好是-"+this.TeacherHobby;
	}	
	@Override
	public void Say() {
		this.student.Say();
		System.out.println("I am a goog teacher!!!");	}}
```

```xml
<!-- 往teacher类中注入学生类 -->
 <bean id="teacher" class="com.test.one.charpterday1.Teacher" scope="singleton">
 <property name="student" ref="student"/><!--ref表示引用的bean属性名-->
<property name="TeacherName" value="刘能"/><!--setter注入时要用 <property>标签-->
 <property name= "TeachSubject" value ="语文"/><!-- name表示属性名，value表示属性值-->

 <property name="TeacherHobby"><!--<set>标签表示java.util包中的集合，用于存放集合数据，<list>、<map>也用于存放集合-->
 <set>
 <value>"编程"</value>
 <value>"乒乓球"</value>
 <value>"健身"</value>
 </set>
 </property>
 </bean>
```

******②通过构造方式注入******

```java
public class Student implements Person {
private String StuName;//学生姓名
private int StuAge;//学生年龄
private char StuGender;//学生性别
private List <String>StuHobby;//学生爱好
public Student(String name,int age,char gender,List<String>list) {
	this.StuName=name;
	this.StuAge=age;
	this.StuGender=gender;
	this.StuHobby=list;
}
```

```xml
<bean id="student1" class="com.test.one.charpterday1.Student" scope="prototype">
 <constructor-arg index="0" value="小红" /><!--构造注入时要用 <<constructor-arg>标签-->

 <constructor-arg index="1"  value="16" /><!-- index表示属性序号对应构造方法中的参数顺序，value表示属性值-->
 <constructor-arg index="2" value="女" />
 <constructor-arg index="3">
 <list>
 <value>"编程"</value>
 <value>"乒乓球"</value>
 <value>"健身"</value>
 </list>
 </constructor-arg>
 </bean>
```

下图是运行结果结果--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615075650105.png)
完整代码请查看链接---
链接：https://pan.baidu.com/s/1PigyaG60mAwJ8ckkfQnmKw 
提取码：jzja 
复制这段内容后打开百度网盘手机App，操作更方便哦

## annotation注解装配

> 上一篇文章提到过bean的装配，不明白的可以查看csdn链接--https://blog.csdn.net/weixin_54061333/article/details/117917508

下面是annotation装配的方式--
annotation分为以下几种 :

1. @Component 注解描述Sping中的bean，可用于任何层次。
2. @Repository 注解数据访问层
3. @Service 注解业务层
4. @Controller注解控制层
5. @Autowired 用于对Bean的属性变量，属性的setter方法及构造进行标注，配合对应的注解完成装配工作，默认按照bean类型进行装配。
6. @ Resource 有两个重要属性 name=“bean实例名称” type="bean实例类型"，如果不指定name或者type默认按name进行装配；
7. @Qualifier 与@Autowired配合使用，会讲默认按bean类型装配修改为按照bean实例名称装配。

下面在代码中进一步说明--

> 代码中有Student类，Teacher类以及Worker类分别实现Person接口,
> 假定 Teacher为管理类，	Worker为服务类，Student为数据访问类

```sql
package charpter0615;
public interface Person {
public void say();
}
```

```java
package com.test.chapter0615;
import javax.annotation.Resource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import charpter0615.Person;

@Controller("teacher")//老师是控制层吧
public class Teacher implements Person{
	private String TeacherName;
	private String TeacherSubject;
		public Teacher() {
		}
	
public void setTeacherName(String teacherName) {
		TeacherName = teacherName;
	}
	public void setTeacherSubject(String teacherSubject) {
		TeacherSubject = teacherSubject;
	}
@Resource(name="student")	
private Student student;
@Autowired//自动注入student
public void setStudent(Student student) {
	this.student=student;
}
@Resource(name="worker")
@Autowired//自动注入worker
private Worker worker;

public void setWorker(Worker worker) {
	this.worker=worker;
}
	@Override
	public void say() {
		this.student.say();
		System.out.println("同学们好！！我是你们的老师--"+this.TeacherName+"从今天起教你们--"+this.TeacherSubject);
		this.worker.say();
	}
}
```

```java
package com.test.chapter0615;
import org.springframework.stereotype.Repository;
import charpter0615.Person;
@Repository("student")//使用@Repository注解将Student类识别为bean
public class Student implements Person {
private String studentName;
private String studentGrade;
private int studentAge;
	public Student(String studentName ,String studentGrade,int studentAge) {
		this.studentName=studentName;
		this.studentAge=studentAge;
		this.studentGrade=studentGrade;
	}
		@Override
	public void say() {
		// TODO Auto-generated method stub
		System.out.println("老师好！！我是"+this.studentName+"--"+this.studentGrade+"班的学生"+"今年"+this.studentAge+"岁");
	}
}
```

```java
package com.test.chapter0615;
import org.springframework.stereotype.Service;
import charpter0615.Person;
@Service("worker")//使用@Service注解将Worker类识别为bean
public class Worker implements Person{
	@Override
	public void say() {
		// TODO Auto-generated method stub
		System.out.println("Work said : It's my pleasure to service for you!");
	}
}
```

配置xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.3.xsd">
        <context:annotation-config/>
        <bean id="student" class="com.test.chapter0615.Student" autowire="byName">
        <constructor-arg index="0" value="李二狗"/>
         <constructor-arg index="1" value="三年二班"/>
          <constructor-arg index="2" value="10"/>
        </bean>
        <bean id="worker" class="com.test.chapter0615.Worker"/>
         <bean id="teacher" class="com.test.chapter0615.Teacher" autowire="byName">
         <property name="student" ref="student"></property>
        <property name="worker" ref="worker"></property>
        <property name="TeacherName" value="张三"></property>
        <property name="TeacherSubject" value="数学"></property>
        </bean>
</beans>
        
```

创建测试类--

```java
package com.test.chapter0615;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import charpter0615.Person;
public class Testtwo {
	public static void main(String[] args) {
		
ApplicationContext app = new ClassPathXmlApplicationContext("applicationcontext.xml");
	Person per =(Person )app.getBean("teacher");
	per.say();
}
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615144027735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)通过annotation注解相比于单纯的xml方式能简化一部分代码，比如注入student和work的部分
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615145437621.png)有了注解@Autowired会自动帮你注入，通常配置文件需要结合annotation和xml共同配置；

还有一种扫描式注入，

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615145645151.png)需要指定扫描的位置就可以了，其余的配置跟上面差不许多；
以上就是annotation注解的详细内容，如有不当指出还请指出；

> 有没有发现上面用到了自动装配@autowired，下面说一下自动装配

***@Autowired自动装配***
自动装配@Autowired 有四个值：

1. default，由配置bean开头时<beans后指定 格式如下--

```xml
<beans default-autowire=“byName” >
```

1. byName--按照属性名自动装配；
2. byType按照属性类型自动装配；
3. constructor按照构造参数的数据类型进行byType模式的自动装配
4. no --在默然情况下不进行自动装配，必须按照<property     ref进行装配

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615152023709.png)自动装配@Autowired 主要用于注入的时候

共同学习共同进步，见证成长；



## Bean的作用域

1. ***Singleton--单例***   即每次请求只会返回同一个实例---即虽然实例名称不同但是实例指向同一个地址； 这张主要用于功能性组件即不需要用户参与修改的地方可以参考java单例设计便于理解；
2. ***Prototype--原型*** 即每次请求都会新建一个实例，每个实例地址都不一样； 需要保持会话状态的bean需要使用prototype；
3. ***Request***   同一次http请求返回同一个对象，否则返回不同的对象；
4. ***Session***    同一次http session请求中返回同一个对象，否则返回不同的对象；
5. ***Globalsession*** 在全局http session返回同一个对象，尽在使用portlet上下文时候有效；
6. ***Application*** 为每一个servletContext创建一个实例，仅在web相关的applicationcontext中有效；
7. ***Websocket*** 为每一个Websocket创建一个实例，仅在web相关的applicationcontext中有效；

后五种都跟http服务有关。

本次学习内容：

1、Singleton--单例  
2、Prototype--原型

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
 <bean id="student" class="com.test.one.charpterday1.Student" scope="prototype"/>
 
 <!-- 往teacher类中注入学生类 -->
 <bean id="teacher" class="com.test.one.charpterday1.Teacher" scope="singleton">
 <property name="student" ref="student"/>
 </bean>
 </beans>
```

```java
package com.test.one.charpterdaytest;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.test.one.charpterday1.Person;
import com.test.one.charpterday1.Student;
public class DayTest01 {

	public static void main(String[] args) {
ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationcontext.xml");
Person teacher=(Person)applicationContext.getBean("teacher");
Person teacher2=(Person)applicationContext.getBean("teacher");
Person student=(Person)applicationContext.getBean("student");
Person student1=(Person)applicationContext.getBean("student");
System.out.println(teacher);
System.out.println(teacher2);
boolean x=(teacher==teacher2);
System.out.println("作用域为singleton的两个teacher实例地址是否相等？---"+x);
System.out.println(student);
System.out.println(student1);
boolean y=(student==student1);
System.out.println("作用域为prototype的两个student实例地址是否相等？---"+y);
	}

}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615074541885.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)每日总结，加速学习！（如有不当之处还抢评论区指出，坐作者会第一时间修改。。。）
共同学习共同进步！！



## Spring--JDBC入门（一）

学习SpringJDBC的主要功能作用----数据库资源的管理和错误处理；

***SpringJDBC核心包：***

> 除了Spring的基础核心包（core、beans、context、
> expression、commons（扩展核心包））还有有数据库相关的包--（数据库驱动包，jdbc以及tx包）

Spring通过JdbcTemplate来解析Jdbc，继承了JdbcAccessor(抽象类),实现了JdbcOperation接口；

*JdbcOperation接口--*

> 定义了增删查改等围绕数据库的操作方法

*JdbcAccessor(抽象类)*

> 为子类提供了一些共访问数据库的公共属性；
> DataSource--获取数据源即连接数据库的作用具体实现还可以引入对数据库连接的缓冲池和分布事物的支持

*SqlexceptionTranslator*

> 负责对SqlException的转义工作

SpringJDBC配置文件的主要核心内容--

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
      <!-- 配置数据源  连接数据库 -->
      <bean id ="dataSource" class="org.SpringFrameWork.jdbc.dataSource.DriverManagerDataSource">
     <!-- 配置连接数据库的参数  驱动，url，用户名，密码 -->
      <property name="driverClassName" value="org.mysql.jdbc.Driver"/>
       <property name="url" value="jdbc:mysql://localhost3306/company"/>
       <property name="username" value="huixinjiao"/>
       <property name="password" value="955945"/>
       </bean>
       <!-- 配置可以操作数据库的JdbcTemplate模板 -->
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate" autowire="byName"/>
     <!-- 也可以将自动装配去掉，改为手动注入
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate" >
      <property name="dataSource" value="dataSource"/>
     </bean>  
      -->
      <!-- 配置注入类 -->
      </beans>
```

下面就是写java代码了，
先写一个接口，用于对数据库的一些操作方法

```java
package com.chapter0617.one;
import com.chapter0617.two.User;
public interface ManagerOperation {
public void CreateUser();//创建数据表
public int addUser(User user);//添加用户
public int deleteUser(User user);//删除用户
public int updateUser(User user);//更新用户
}
```

定义用户数据--

```java
package com.chapter0617.two;

import org.springframework.stereotype.Repository;

@Repository("user")
public class User {
	private Integer UserId;
	private String UserName;

	private String UserPassword;

	public Integer getUserId() {
		return UserId;
	}

	public void setUserId(Integer userId) {
		UserId = userId;
	}

	public String getUserName() {
		return UserName;
	}

	public void setUserName(String userName) {
		UserName = userName;
	}

	public String getUserPassword() {
		return UserPassword;
	}

	@Override
	public String toString() {
		return "User [UserId=" + UserId + ", UserName=" + UserName + ", UserPassword=" + UserPassword + "]";
	}

	public void setUserPassword(String userPassword) {
		UserPassword = userPassword;
	}
}

```

接口实现类--

```java
package com.chapter0617.two;
import javax.annotation.Resource;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Controller;
import com.chapter0617.one.ManagerOperation;
@Controller(value="userManager")
public class UserManager implements ManagerOperation {
	@Resource(name="jdbcTemplate")//注入jdbcTemplate以操作数据库
	private JdbcTemplate jdbcTemplate;
	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}
	@Override
	public void CreateUser() {
		String sql[] = new String[5];
		sql[0] = "create table userinfo(user_id int not null primary key,user_name varchar(64),user_password varchar(128))";
		sql[1] = "insert into userinfo (user_id, user_name, user_password) values(1001,'jack','123456')";
		sql[2] = "insert into userinfo (user_id, user_name, user_password) values(1002,'mary','456789')";
		sql[3] = "insert into userinfo (user_id, user_name, user_password) values(1003,'tom','410032')";
		sql[4] = "insert into userinfo (user_id, user_name, user_password) values(1004,'gavin','124545')";
		jdbcTemplate.execute(sql[0]);
		jdbcTemplate.execute(sql[1]);
		jdbcTemplate.execute(sql[2]);
		jdbcTemplate.execute(sql[3]);
		jdbcTemplate.execute(sql[4]);
		System.out.println("成功创建userinfo表并插入四条数据");
	}
	@Override
	public int addUser(User user) {//添加用户方法实现
		String sql = "insert into userinfo (user_id,user_name) values(?,?)";
		Object[] obj = new Object[] { user.getUserId(), user.getUserName()
		};
		int num = jdbcTemplate.update(sql, obj);//利用JdbcTemplate类的update()方法来对数据库数据进增删查改；
		if (num >= 0) {
			System.out.println("成功插入" + num + "条数据");
		}
		return num;
	}
	@Override
	public int deleteUser(User user) {//删除用户方法实现
		String sql = "delete table userinfo where user_id=?";
		Object obj[] = new Object[] { user.getUserId() };
		int num = jdbcTemplate.update(sql, obj);
		if (num >= 0) {
			System.out.println("成功删除" + num + "条数据");
		}
		return num;
	}
	@Override
	public int updateUser(User user) {//更新用户方法实现
		String sql = "update userinfo set user_name=? where user_id=?";
		Object obj[] = new Object[] { user.getUserName(), user.getUserId() };
		int num = jdbcTemplate.update(sql, obj);
		if (num >= 0) {
			System.out.println("成功更新" + num + "条数据");
		}
		return num;
	}
	}
```

还是放上xml配置文件源码吧---

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
      <!-- 配置数据源  连接数据库 -->
      <bean id ="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
     <!-- 配置连接数据库的参数  驱动，url，用户名，密码 -->
      <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
       <property name="url" value="jdbc:mysql://localhost:3306/company?serverTimezone=UTC"/>
       <property name="username" value="huixinjiao"/>
       <property name="password" value="955945"/>
       </bean>
       <!-- 配置可以操作数据库的JdbcTemplate模板 -->
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate" autowire="byName"/>
     <!-- 也可以将自动装配去掉，改为手动注入
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate" >
      <property name="dataSource" ref="dataSource"/>
     </bean>  
      -->
      <!-- 配置注入类 -->
      <bean id="userManager" class="com.chapter0617.two.UserManager" autowire="byName"/>
      </beans>
      
```

> 这里数据库驱动地址跟以前有所变化，之前是com.mysql.jdbc.Driver，现在改为了com.mysql.cj.jdbc.Driver
> 还有关于时区的设置--有时候没有设置时区，会出现-You must configure either the server or JDBC driver (via the serverTimezone configuration property)，只需要在数据库url后面添加?serverTimezone=UTC，有的还会遇到SSL加密的情况，需要设置及为false；

测试类---

```java
package com.chapter0617.one;



import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.chapter0617.two.User;

public class Testone {

	public static void main(String[] args) {
		ApplicationContext app = new ClassPathXmlApplicationContext("aplicationcontext.xml");
		ManagerOperation manager=(ManagerOperation)app.getBean("userManager");
	User user0=new User();
		//manager.CreateUser();
		user0.setUserId(1222);
		user0.setUserName("SteaveJobs");
		user0.setUserPassword("741654");
		manager.addUser(user0);
		User user1=new User();
		user1.setUserId(1001);
		manager.deleteUser(user1);
		User user2=new User();
		user2.setUserName("Gavin");
		user2.setUserId(1004);
		manager.updateUser(user2);
	}

}

```

```bash
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
6月 17, 2021 10:52:29 上午 org.springframework.context.support.ClassPathXmlApplicationContext prepareRefresh
信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@6fdb1f78: startup date [Thu Jun 17 10:52:29 CST 2021]; root of context hierarchy
6月 17, 2021 10:52:29 上午 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
信息: Loading XML bean definitions from class path resource [aplicationcontext.xml]
6月 17, 2021 10:52:30 上午 org.springframework.jdbc.datasource.DriverManagerDataSource setDriverClassName
信息: Loaded JDBC driver: com.mysql.cj.jdbc.Driver
成功创建userinfo表并插入四条数据
成功插入1条数据
成功删除1条数据
成功更新1条数据

```

以上就是SpringJDBC数据库连接实操；
共同学习共同进步，见证成长！！

------

## Spring--JDBC 入门（二）

------

先来一段碎碎念--这不像是程序员应该有的作风吧.........

> 昨晚不是618吗，熬了一个晚上去抢新的笔记本电脑，毕竟有羊毛可以薅一把，好端端的为什么要抢呢？---可能因为“穹”吧! 
> 事情的起因是这样的，昨晚在七点左右在写程序--实际上是学习阶段，打开eclipse需要1.5-2min，我的个老天（当然这是一部分原因），在加上一小段代码的执行需要30-45s（这也是一部分原因），然后我又打开了其他程序，卡了它卡了。。。。（这也是一部分有原因）最主要的原因是我的笔记本电脑确实该换了（强行解释最为致命）---还是看图说话吧----

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618100452534.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618100543746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)我还加装了一块4G内存条，组成了双通道，勉强用到了今天，是不是该换了。

***说回正题***

不啰嗦，看东西---

```java
package chapter0618.two;
//自定义类，用于定义存储和读取数据
public class Book {
private Integer book_id;
private String book_name;
private String book_type;
private float book_price;
private int book_acount;
public Integer getBook_id() {
	return book_id;
}
public void setBook_id(Integer book_id) {
	this.book_id = book_id;
}
public String getBook_name() {
	return book_name;
}
public void setBook_name(String book_name) {
	this.book_name = book_name;
}
public String getBook_type() {
	return book_type;
}
public void setBook_type(String book_type) {
	this.book_type = book_type;
}
public float getBook_price() {
	return book_price;
}
public void setBook_price(float book_price) {
	this.book_price = book_price;
}
public int getBook_acount() {
	return book_acount;
}
public void setBook_acount(int book_acount) {
	this.book_acount = book_acount;
}
@Override
public String toString() {
	return "Book [book_id=" + book_id + ", book_name=" + book_name + ", book_type=" + book_type + ", book_price="
			+ book_price + ", book_acount=" + book_acount + "]";
}
}

```

```java
package chapter0618.one;
import java.util.List;
import chapter0618.two.Book;
public interface ManagerBook {//定义书籍管理系统
	public void CreateBookStore();//安装--创建系统
public int InsertBook(Book book);//引进图书
public List<Book> findAllBook();//列出所有书籍
public Book findABook(String bookname);//查图书
public int deleteBook(int id);//删除书籍
public int updateBook(Book book);//更新书籍信息
}

```

```java
package chapter0618.two;
import java.util.List;
import javax.annotation.Resource;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import chapter0618.one.ManagerBook;
public class BookManager implements ManagerBook {//管理员实现对数据库数据的管理操作
	@Resource(name = "jdbcTemplate") // 注入配置文件--setter注入
	private JdbcTemplate jdbcTemplate;//负责去执行sql语句--增删查改
	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}
	@Resource(name = "book") // 注入实例--setter注入
	private Book book;
	public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}
	public void setBook(Book book) {
		this.book = book;
	}
	@Override
	public void CreateBookStore() {
		String sql ="create table bookshop (book_id int not null primary key,book_type varchar(128),book_name varchar(128),book_price float,book_account int)";
		jdbcTemplate.execute(sql);
		System.out.println("已在工商局备案---》》》》》恭喜你开店成功");
	}
	@Override
	public int InsertBook(Book book) {
		String sql = "insert into  bookshop values(?,?,?,?,?)";
		Object[] obj = new Object[] { book.getBook_id(), book.getBook_type(), book.getBook_name(), book.getBook_price(),
				book.getBook_acount() };
		int num = jdbcTemplate.update(sql, obj);
		if (num > 0) {
			System.out.println("成功引进" + num + "套书籍");
		}
		return num;
	}
	@Override
	public List<Book> findAllBook() {
		String sql = "select * from bookshop";
		RowMapper<Book> rowMapper = new BeanPropertyRowMapper<Book>(Book.class);
		return this.jdbcTemplate.query(sql, rowMapper);// 返回一个List类型的结果，元素为用户自定义的Book类
	}
	@Override
	public Book findABook(String bookname) {
		String sql = "select * from bookshop where book_name=?";
		// RowMapper接口 RowMapper接口的实现类--》》BeanPropertyRowMapper，并将返回结果映射到用户自定义类中
		RowMapper<Book> rowMapper = new BeanPropertyRowMapper<Book>(Book.class);
		return this.jdbcTemplate.queryForObject(sql, rowMapper, bookname);// 这里返回的是一个Object类型的单行记录，后续可能要转型
	}
	@Override
	public int deleteBook(int id) {
		String sql = "delete from bookshop where book_id=?";
		int num = jdbcTemplate.update(sql, id);
		if (num > 0) {
			System.out.println("删除" + num + "条数据");
		}
		;
		return num;
	}
	@Override
	public int updateBook(Book book) {
		String sql = "update bookshop set book_aacount=? where book_id=?";
		Object[] obj = new Object[] { book.getBook_acount(), book.getBook_id() };
		int num = jdbcTemplate.update(sql, obj);
		if (num > 0) {
			System.out.println("更新" + num + "条数据");
		}
		;
		return num;
	}

}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
      <!-- 配置数据源  连接数据库 -->
      <bean id ="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource" autowire="byName">
     <!-- 配置连接数据库的参数  驱动，url，用户名，密码 -->
      <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
       <property name="url" value="jdbc:mysql://localhost:3306/bookstore?serverTimezone=UTC"/>
       <property name="username" value="root"/>
       <property name="password" value="955945"/>
       </bean>
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate" autowire="byName"/>
      <bean id="book" class="chapter0618.two.Book"/>
      <bean id="bookManager" class="chapter0618.two.BookManager" >
      <property name="jdbcTemplate" ref="jdbcTemplate"/>
      </bean>
   
</beans>
```

测试类--

```java
package chapter0618.two;

import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import chapter0618.one.ManagerBook;

public class Testonr {

	public static void main(String[] args) {
		ApplicationContext app = new ClassPathXmlApplicationContext("NewFile.xml");
		ManagerBook bookmanager = (ManagerBook) app.getBean("bookManager");
		bookmanager.CreateBookStore();
		Book book = new Book();
		book.setBook_id(1001);
		book.setBook_name("资本论");
		book.setBook_type("社会科学");
		book.setBook_price(128.2f);
		book.setBook_acount(100);
		bookmanager.InsertBook(book);
		
		Book book1 = new Book();
		book1.setBook_id(1002);
		book1.setBook_name("母猪的");
		book1.setBook_type("劳动社会科学");
		book1.setBook_price(108.2f);
		book1.setBook_acount(130);
		bookmanager.InsertBook(book1);
		Object obj = bookmanager.findABook("资本论");// 返回的是Object类型的一行数据
		System.out.println("查询的数据是--"+obj);
		List <Book>list=bookmanager.findAllBook();//返回的是List类型的，所以用list来接收
		for (Book book2 : list) {
			System.out.println(book2);
		}
	}

}

```

```bash
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
6月 18, 2021 10:13:17 上午 org.springframework.context.support.ClassPathXmlApplicationContext prepareRefresh
信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@6fdb1f78: startup date [Fri Jun 18 10:13:17 CST 2021]; root of context hierarchy
6月 18, 2021 10:13:18 上午 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
信息: Loading XML bean definitions from class path resource [NewFile.xml]
6月 18, 2021 10:13:18 上午 org.springframework.jdbc.datasource.DriverManagerDataSource setDriverClassName
信息: Loaded JDBC driver: com.mysql.cj.jdbc.Driver
已在工商局备案---》》》》》恭喜你开店成功
成功引进1套书籍
成功引进1套书籍
查询的数据是--Book [book_id=1001, book_name=资本论, book_type=社会科学, book_price=128.2, book_acount=0]
Book [book_id=1001, book_name=资本论, book_type=社会科学, book_price=128.2, book_acount=0]
Book [book_id=1002, book_name=母猪的, book_type=劳动社会科学, book_price=108.2, book_acount=0]

```

> 从执行结果我们可以发现执行的顺序--org.springframework.context.support.--找到xml配置文件，然后开始解析
> org.springframework.beans.factory--工厂类找到对应的bean，在执行代码的时候注入对应的bean；
> 以上就是本次操作过程；
> 共同学习不断进步，见证成长！！！

## Spring--JDBC事务管理(xml)

> Spring事务管理简化了传统事务管理流程，在一定程度上减少了开发者的工作量，因为如果不用Spring的事务管理，程序员就要编写程序去处理这件事，需要考虑到怎样与数据库连接，驱动的适配等一系列问题，下一个程序员也不太容易接手；

先来看如果没有事务管理的情况，代码如下--

> 大体思路是商家有个活动，好友之间可以互相赠送积分给对方--老生常谈的事情了；

```java
package charpter0618.one;
public interface GameRule {//赠送积分的接口
public void zsJF(String giveuser,String receiveuser,Integer JF);
}
```

```java
package charpter0618.one.next;
public class User {//定义用户信息，以使得数据跟数据库上的表类型一致，
private Integer user_id;
private String user_name;
private Integer user_jifen;
public Integer getUser_id() {
	return user_id;
}
public void setUser_id(Integer user_id) {
	this.user_id = user_id;
}
public String getUser_name() {
	return user_name;
}
public void setUser_name(String user_name) {
	this.user_name = user_name;
}
public Integer getUser_jifen() {
	return user_jifen;
}
public void setUser_jifen(Integer user_jifen) {
	this.user_jifen = user_jifen;
}
@Override
public String toString() {
	return "User [user_id=" + user_id + ", user_name=" + user_name + ", user_jifen=" + user_jifen + "]";
}
}

```

```java
package charpter0618.one.next;

import javax.annotation.Resource;

import org.springframework.jdbc.core.JdbcTemplate;

import charpter0618.one.GameRule;

public class PlayGame implements GameRule {
	@Resource(name="jdbcTemplate")
private JdbcTemplate jdbcTemplate;
	
	
public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}
	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}
@Resource(name="user")
	private User user;
	public void setUser(User user) {
		this.user = user;
	}
	public User getUser() {
		return user;
	}
	@Override   //赠送积分的方法
	public void zsJF(String giveuser,String receiveuser,Integer zsJF) {
		String sql="update userdata set user_jifen=user_jifen+? where user_name=?";
		this.jdbcTemplate.update(sql, zsJF, receiveuser);//赠送积分以后另一个要同步减少相应积分
		//假设不做事务处理，演示结果--
		String pra="update userdata set user_jifen=user_jifen-? where user_name=?";
		this.jdbcTemplate.update(pra, zsJF,giveuser );//赠送积分以后另一个要同步减少相应积分	}
}
```

配置文件如下--没做事务处理--

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
      <!-- 配置数据源  连接数据库 -->
      <bean id ="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource" autowire="byName">
     <!-- 配置连接数据库的参数  驱动，url，用户名，密码 -->
      <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
       <property name="url" value="jdbc:mysql://localhost:3306/company?serverTimezone=UTC"/>
       <property name="username" value="root"/>
       <property name="password" value="955945"/>
       </bean>
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate" autowire="byName"/>
      <bean id="user" class="charpter0618.one.next.User"/>
      <bean id="gameRule" class="charpter0618.one.next.PlayGame" >       
      <property name="jdbcTemplate" ref="jdbcTemplate"/>
      </bean>
</beans>
```

这种情况下---如果下都执行成功了还好，万一赠出者的代码执行失败了，就会导致在数据库中积分接受者的积分增加，而赠出者的积分没有减少（默认初始积分都为1000）

```java
package charpter0618.one;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class Test {
public static void main(String[] args) {
	ApplicationContext app = new ClassPathXmlApplicationContext("NewFile.xml");
	GameRule game= (GameRule)app.getBean("gameRule");
	game.zsJF("jack", "mary", 50);
}
}

```

执行成功后的结果如下--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618170804347.png)
如果在执行过程中遇到了意外情况，
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061817094948.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)为了简单起见，作如下修改---并再次执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618171049203.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)`信息: Loaded JDBC driver: com.mysql.cj.jdbc.Driver
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at charpter0618.one.next.PlayGame.zsJF(PlayGame.java:33)
	at charpter0618.one.Test.main(Test.java:10)`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618171206825.png)
积分赠送成功但是，但是赠送者积分并没有因此而减少，这是不符合商家要求的，因此要做如下修改--
① 基于xml配置文件的事务声明---修改配置文件-

```java
package charpter0618.one.next;

import javax.annotation.Resource;

import org.springframework.jdbc.core.JdbcTemplate;

import charpter0618.one.GameRule;

public class PlayGame implements GameRule {
	@Resource(name = "jdbcTemplate")
	private JdbcTemplate jdbcTemplate;

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

	@Resource(name = "user")
	private User user;

	public void setUser(User user) {
		this.user = user;
	}

	public User getUser() {
		return user;
	}

	@Override // 赠送积分的方法
	public void zsJF(String giveuser, String receiveuser, Integer zsJF) {
		String sql = "update userdata set user_jifen=user_jifen+? where user_name=?";
		this.jdbcTemplate.update(sql, zsJF, receiveuser);// 赠送积分以后另一个要同步减少相应积分
		// 假设不做事务处理，演示结果--
		int x = 1 / 0;
		String pra = "update userdata set user_jifen=user_jifen-? where user_name=?";
		this.jdbcTemplate.update(pra, zsJF, giveuser);// 赠送积分以后另一个要同步减少相应积分
	}

}

```

修改之后会如遇到错误会回滚数据

```go
信息: Loaded JDBC driver: com.mysql.cj.jdbc.Driver
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.springframework.cglib.core.ReflectUtils$1 (file:/C:/EC_Workspqce/charpter0618/src/main/webapp/WEB-INF/lib/spring-core-4.3.6.RELEASE.jar) to method java.lang.ClassLoader.defineClass(java.lang.String,byte[],int,int,java.security.ProtectionDomain)
WARNING: Please consider reporting this to the maintainers of org.springframework.cglib.core.ReflectUtils$1
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at charpter0618.one.next.PlayGame.zsJF(PlayGame.java:33)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:64)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:564)
	at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:333)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.invokeJoinpoint(ReflectiveMethodInvocation.java:190)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:157)
	at org.springframework.transaction.interceptor.TransactionInterceptor$1.proceedWithInvocation(TransactionInterceptor.java:99)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:282)
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:96)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.aop.interceptor.ExposeInvocationInterceptor.invoke(ExposeInvocationInterceptor.java:92)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:213)
	at com.sun.proxy.$Proxy6.zsJF(Unknown Source)
	at charpter0618.one.Test.main(Test.java:10)
```

修改表中数据为下图所示，再次执行---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618180450807.png)
表中数据如下--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210618180604781.png)
会发现数据并没有被修改，这说明事务开启成功---今天就先学习到这里
共同学习共同进步，见证成长！！！



## Spring--JDBC事务管理(注解)

不啰嗦直接看东西

```java
package charpter0619.one;
//接口，定义涨价和降价的动物
public interface AnimalManger {
public void Transfer(String animalout,String animalin,float price );
}

```

> 和之前的代码差不多，这次简化了数据库的增删查操作，直接进行修改--

```java
package charpter0619.one.next;

public class Animal {
private Integer animal_ID;
private String animal_type;
private String animal_name;
private float animal_price;
private Integer animal_num;
public Integer getAnimal_ID() {
	return animal_ID;
}
public void setAnimal_ID(Integer animal_ID) {
	this.animal_ID = animal_ID;
}
public String getAnimal_type() {
	return animal_type;
}
public void setAnimal_type(String animal_type) {
	this.animal_type = animal_type;
}
public String getAnimal_name() {
	return animal_name;
}
public void setAnimal_name(String animal_name) {
	this.animal_name = animal_name;
}
public float getAnimal_price() {
	return animal_price;
}
public void setAnimal_price(float animal_price) {
	this.animal_price = animal_price;
}
public Integer getAnimal_num() {
	return animal_num;
}
public void setAnimal_num(Integer animal_num) {
	this.animal_num = animal_num;
}
@Override
public String toString() {
	return "Animal [animal_ID=" + animal_ID + ", animal_type=" + animal_type + ", animal_name=" + animal_name
			+ ", animal_price=" + animal_price + ", animal_num=" + animal_num + "]";
}

}

```

```java
package charpter0619.one.next;

import javax.annotation.Resource;

import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.transaction.annotation.Isolation;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

import charpter0619.one.AnimalManger;

@Controller("manageAimal") // 控制层
public class ManageAnaimal implements AnimalManger {
	@Resource(name = "jdbcTemplate") // 注入
	private JdbcTemplate jdbcTemplate;

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

	@Resource(name = "animal") // 注入
	private Animal animal;

	public void setAnimal(Animal animal) {
		this.animal = animal;
	}
@Transactional//在这里进行声明事务管理--表示支队该方法有效，如果有其他方法也涉及到事务，则不对他们生效，相关参数已在配置文件中出现，所以这里不重复定义
	@Override
	public void Transfer(String animalout, String animalin, float price) {// 动物涨价和降价
		String sql = "update animalshop set animal_price=animal_price+? where animal_name=?";
		this.jdbcTemplate.update(sql, price, animalin);
		System.out.println(animalin + "--涨价成功");
		String parm = "update animalshop set animal_price=animal_price-? where animal_name=?";
		this.jdbcTemplate.update(parm, price, animalout);
		System.out.println(animalout + "--降价成功");

	}
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
           http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx-4.3.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context-4.3.xsd
       http://www.springframework.org/schema/aop
      http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
     ">
	<!-- 配置数据源 -->
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url"
			value="jdbc:mysql://localhost:3306/company?serverTimezone=UTC" />
		<property name="username" value="root" />
		<property name="password" value="955945" />
	</bean>
	<!-- 配置jdbcTemplate -->
	<bean id="jdbcTemplate"
		class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="dataSource" />
	</bean>

	<bean id="animal" class="charpter0619.one.next.Animal" />
	<bean id="managerAnimal"
		class="charpter0619.one.next.ManageAnaimal">
		<property name="jdbcTemplate" ref="jdbcTemplate" />
		<property name="animal" ref="animal" />
	</bean>
	<!-- 配置事务管理 -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager"
		autowire="byName" />
	<tx:advice id="txAdvice"
		transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="*" propagation="REQUIRED"
				isolation="DEFAULT" read-only="false" />
		</tx:attributes>

	</tx:advice>
	
	<!--和xml配置相比，annotation只是缺少了AOP切面，少了切面，切入点的相应配置，但是要注册annotation驱动，同时如果执行方法在别的包的话，还有加上扫描-->
	<!-- 注册事务管理器驱动 -->
	
	<tx:annotation-driven transaction-manager="transactionManager"  />
	
</beans>
```

```java
package charpter0619.one;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {
		ApplicationContext app = new ClassPathXmlApplicationContext("NewFile.xml");
		AnimalManger animal =(AnimalManger)app.getBean("managerAnimal");
		animal.Transfer("小刚","小花", 100.5f);
	}

}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210619142107353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)总结，如果aop配置量少的话，可以用xml进行配置，如果多的话，还是建议annotation；



## [福利贴]网站上的音频免费下载

本教程仅供参考--

> 本教程仅供参考，在法律允许的范围内可以适当利用一下，但是别得寸进尺哦！

由于英语授课需要课本内音频--（七年级上册，好不容找到了网站）怎么办，于是在一众老师的要求下，我越狱了。。。。
本篇仅供学习交流使用。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210617163440926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)找到对应的音频播放处，点击键盘上的F12进入网页调试模式，你会看到很多内容-0-0-
点击播放音频，

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210617163613503.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210617163739198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)接下来就会看到缓存好的音频
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210617163916246.png)
双击音频进入浏览器播放页面，右键会有另存为音频--保存到响应路径即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210617163943481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
本教程仅限于交流学习的资源下载，如涉及版权的内容请自觉绕行。



# Spring问题实录

## cannot be opened because

> 本文旨在和小白一起成长，很不幸目前没有钱买idea，用了一段时间idea奈何到期了，试了各种方法破解--未果，只好重新转战eclipse。

絮絮叨叨一会--
初学者刚接触到spring，学习难免会遇到问题，本文将学习过程中遇到的问题且已经解决的问题进行整理，一是给自自己整理，另一个是供小白们（我也是）一起学习，共同成长。
***正题***
直接上文件目录，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210614214237550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

以上是文件分布目录，待会要用到。
1，UserDao内容--

```java
package com.first.test;

public interface UserDao {
public void login();
}

```

2，UserDaoImp实现了UserDao

```java
package com.myfirst.test;

import com.first.test.UserDao;

public class UserDaoImp implements UserDao {

	@Override
	public void login() {
		// TODO Auto-generated method stub
		System.out.println("说有这么一个人");
	}
}

```

3，Testone内容

```java
package com.first.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;
public class Testone {
public static void main(String []args) {
	//ApplicationContext app=new FileSystemXmlApplicationContext("src/applicationcontext.xml");
	ApplicationContext app=new ClassPathXmlApplicationContext("applicationcontext.xml");//配置文件根目录是java包的存放文件目录
	UserDao userdao=(UserDao)app.getBean("userdao");
	userdao.login();
}
}

```

4，applicationcontext内容

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
<bean id="userdao" class="com.myfirst.test.UserDaoImp"/>
       </beans>
```

> applicationcontext放置的位置不同也会用到不同的解析方式去读取他的位置
> ApplicationContext接口的常用实现类有以下3个。
> •  　FileSystemXmlApplicationContext：从文件系统中的XML文件加载上下文中定义的信息。
> •  　ClassPathXmlApplicationContext：从类路径中的XML文件加载上下文中定义的信息，把上下文定义的文件当成类路径资源。
> •  　XmlWebApplicationContext：从Web系统中的XML文件加载上下文中定义的信息。

使用ClassPathXmlApplicationContext时applicationContext.xml文件位于项目的src下Java文件包存放的位置目录底下，如①所示。

使用FileSystemXmlApplicationContext时applicationContext.xml文件位于项目的任何位置，只要路径设置正确如②所示。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210613175000355.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)以上就是本次学习内容。

## setter方式注入失败的经历

> 实现的功能：
> 一个Person类，Student和Teacher是两个实现类，在老师类里面注入学类
> 代码如下

```java
package com.test.one.charpterday1;
public interface Person {
public void Say();
}
package com.test.one.charpterday1;

public class Student implements Person {
	@Override
	public void Say() {
		// TODO Auto-generated method stub
		System.out.println("祝老师端午节安康");
	}

}
```

```java
package com.test.one.charpterday1;

public class Teacher implements Person {
	private Student student;
	public void setStudent(Student student) {
		this.student=student;
	}
	@Override
	public void Say() {
		// TODO Auto-generated method stub
		this.student.Say();
		System.out.println("I am a goog teacher!!!");
	}

}
```

```java
package com.test.one.charpterdaytest;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.test.one.charpterday1.Person;
import com.test.one.charpterday1.Student;
public class DayTest01 {

	public static void main(String[] args) {
//		// TODO Auto-generated method stub
ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationcontext.xml");
Person teacher=(Person)applicationContext.getBean("teacher");
teacher.Say();
	}

}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
 <bean id="student" class="com.test.one.charpterday1.Student"/>
 
 <!-- 往teacher类中注入学生类 -->
 <bean id="teacher" class="com.test.one.charpterday1.Teacher"/>
 <property name="student" ref="student"/> 
 </beans>
<!--运行后出现错误提示--lineNumber: 10; columnNumber: 42; cvc-complex-type.2.4.a: 发现了以元素 '{"http://www.springframework.org/schema/beans":property}' 开头的无效内容。
配置文件错误，原因是<bean 配置错误-->
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615073659320.png)

> 正确的因为注入的Student实例要在Teacher的bean里面---如下所示--
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615073726962.png)
> 以上就是本次错误经历！





## @Resource注解爆红

絮絮叨叨的小白：

> 小白入门难免会遇到问题，今天在学习spring入门的时候就遇到了问题
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615100958560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)在用resource注解的时候遇到了问题

问题描述：提示没有resource，查找了半天原因

1. 既然没有又不能自己创建，尝试了一下手动倒入annotation包

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615104802644.png)结果失败，问题依然没有解决。

1. 在CSDN中求助大神吧--
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615104927145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)感谢大神们的及时回复，作为小白的我感激不尽--
   学习了---stereotype里面的注解是用来ioc容器自动装配的
             javax扩展包annotation是配合spring框架告诉它我这里是引用（源数据）
2. 百度吧---搜到了各种各样的解决方式，有配置maven'的，有倒入包的，问题依然没有解决，知道当我尝试倒入tomcat服务，才成功了；
   操作如下，右键 Referenced Libaries -----build path ---config build path
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615110234155.png)
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615110258725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210615110308905.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061511032022.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
   reource注解---需要tomcat里面的依赖支持。
   以上问题就解决了。

共同学习共同进步，记录成长！！



Spring项目resource:

- [数据库批量操作](https://gitee.com/varyu-program/jdbcbatch)
- [零xml模式开发案例](https://gitee.com/varyu-program/spring-zone/tree/master)





<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/#为什么会有这个网站)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/mybatis/Mybatis.md)

</center>

![](..\pic\div\plant.gif)