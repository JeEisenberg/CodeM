# MicroService
![在这里插入图片描述](https://img-blog.csdnimg.cn/b2eec27e2703443991e95b4ffae892f3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## 微服务简介
>把一个**大型的单个应用程序和服务** **拆分为**数个甚至数十个的支持**微服务**，它**可扩展单个组件**而不 是整个的应用程序堆栈.


![在这里插入图片描述](https://img-blog.csdnimg.cn/ee881f7496364186a65ff8b14b9d1a4a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

微服务（或微服务架构）是一种云原生架构方法，其中单个应用程序由许多松散耦合且可独立部署的较小组件或服务组成。这些服务通常

>    1, 有自己的**堆栈**，包括**数据库和数据模型**； 
   2, 通过REST API，**事件流和消息代理**的组合相互**通信**；   
> 3,它们是按业务能力组织的，分隔服务的线通常称为有界上下文。


一个经典的微服务框架
![在这里插入图片描述](https://img-blog.csdnimg.cn/4710fe490d8f43e3bdbadd9bb299c2aa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

##  微服务优点:
1, 可以更轻松地更新代码。
2, 团队可以为不同的组件使用不同的堆栈。
3, 组件可以彼此独立地进行缩放，从而减少了因必须缩放整个应用程序而产生的浪费和成本，因为单个功能可能面临过多的负载。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5b203636773b4d2eb6a4a339f714fb9e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

Spring中给我们提供了关于微服务的一些框架,之不够有一些不太适合企业的被大厂们进行再次开发,比如注册中心组件  

> Dubbo(阿里) : 目前开源于Apache ; 2012年推出;2014年停更;2015年恢复更新
> DubboX(当当基于Dubbo的更新) 
> JD-hydra(京东基于Dubbo的更新) 
> ServiceComb/CSE(华为2017)
> SpringCloud (Spring推出) 官网有自己的组件,但是部分没人用

![在这里插入图片描述](https://img-blog.csdnimg.cn/d0ac8629808f4d02a2b63dc3a2c445ad.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

 - **cloud升级**

![在这里插入图片描述](https://img-blog.csdnimg.cn/3345ecad67bf4189a3f506e4a759b02b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

 - **阿里微服务框架**

![图片来自于网络](https://img-blog.csdnimg.cn/b34e90756f8742fbb2adb5e0e9e40c38.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **spring微服务架构与阿里的微服务架构对比**

![在这里插入图片描述](https://img-blog.csdnimg.cn/3b0aaa9655d041968bf9a161c1bb90c6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
还是先学习spring的框架作为入门
### Spring Cloud Netfilx

 - **官网概述~**

Spring Cloud Netfilx 通过**自动配置**和**绑定到 Spring Environment** 和其他 **Spring 编程模型**习惯用法，为 Spring Boot 应用程序提供 Netflix OSS 集成。 通过一些简单的注释，您可以快速启用和配置应用程序中的常见模式，并使用经过实战考验的 Netflix 组件构建大型分布式系统。 提供的模式包括**服务发现 (Eureka)、断路器 (Hystrix)、智能路由 (Zuul) 和客户端负载平衡 (Ribbon)**。 



 - **Spring Cloud Netflix 功能**

  **服务发现**：可以注册 Eureka 实例，客户端可以使用 **Spring 管理的 bean** 发现实例;

**服务发现**：可以使用声明性 Java 配置创建嵌入式 Eureka 服务器

   **断路器**：可以使用简单的注释驱动方法装饰器构建 Hystrix 客户端

   **断路器**：具有声明性 Java 配置的嵌入式 Hystrix 仪表板

   **声明式 REST 客户端**：Feign 创建用 JAX-RS 或 Spring MVC 注释装饰的接口的动态实现

   **客户端负载均衡器**：功能区

   **外部配置**：从 Spring 环境到 Archaius 的桥梁（使用 Spring Boot 约定启用 Netflix 组件的本地配置）

   **路由器和过滤器**：自动注册 Zuul 过滤器，以及一种简单的约定优于配置的方法来创建反向代理

 - **Spring Cloud Netflix 的组件**

**1,Eureka**
 **服务注册和发现**，它提供了一个**服务注册中心、服务发现的客户端**，还有一个方便的查看所有**注 册的服务的界面**。 所有的服务使用Eureka的服务发现客户端来将自己注册到Eureka的服务器上。

**2,Ribbon**
**负载均衡**，一个请求发送给某一个服务的应用的时候，如果一个服务启动了多个实例，就会通过 Ribbon来通过一定的负载均衡策略来发送给某一个服务实例。


**3,Feign**
**服务客户端**，服务之间如果需要相互访问，可以使用RestTemplate，也可以使用Feign客户端访 问。它默认会使用Ribbon来实现负载均衡。现在终止维护,使用openFeign或者直接使用阿里的框架

**4,Hystrix**
**监控和熔断器** 我们只需要在**服务接口上添加Hystrix标签**，就可以实现对这个接口的监控和断路 器功能。
Hystrix Dashboard，**监控面板**，他提供了一个界面，可以**监控各个服务上的服务调用所消耗的时间**等。
Turbine，监控聚合，使用Hystrix监控，我们需要打开每一个服务实例的监控信息来查看。 

**5,Zuul**
网关,所有的客户端请求通过这个网关访问后台的服务。他可以使用一定的路由配置来判断某一个URL由哪个服务来处理。并从Eureka获取注册的服务来转发请求。

## 微服务入门
版本的选择
>SpringCloud版本和Springboot版本兼容 不同的springboot版本,配套支持SpringCloud的版本;如果使用Hoxton版本,Boot版本必须要2.2.x或者以上.

![在这里插入图片描述](https://img-blog.csdnimg.cn/aecb7c84d0904131bfb0a987f42e77a3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
这里给出一个参考~
```xml
<properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>2.6.6</spring-boot.version>
        <spring-cloud.version>2021.0.1</spring-cloud.version>
    </properties>
```
### 微服务项目框架
 - 新建项目**microService**

 - 新建父类模块~**dependencies**
      ~依赖限制版本,以使得开发时版本一致~
      ~pom方式;项目总依赖包;通用的依赖版本;总体父项目~

```xml
 <parent>
        <artifactId>wfwservice</artifactId>
        <groupId>com.gavin</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
<packaging>pom</packaging>
    <artifactId>dependency</artifactId>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
                <version>2.6.6</version>
            </dependency>

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.6.6</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.6.6</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.22</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
                <version>3.1.1</version>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.28</version>
                <scope>runtime</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-starter</artifactId>
                <version>1.2.8</version>
            </dependency>

        </dependencies>
    </dependencyManagement>


```


  **这里为什么要建立父类模块而不是直接用microService,**

>这是因为单继承的限制,这里使用的是一台电脑,所以将各个模块集中到一个总的框架中,Eureka模块需要单独拿出来作为一个注册中心,父类pom为springbootparent;

 - **Common模块**
>jar包方式;项目公共包;包含通用实体类,工具类,结果码等.


```java

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

/**
 * 公共的实体类,用于服务与服务之间传递数据用的
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class OrderInfo implements Serializable {


    private int oid;

    private String number;

    private static final long serialVersionUID = -1L;

}

```

 - **Eureka模块**
**搭建springboot项目**
>**服务发现是基于微服务架构的关键原则之一**
> Eureka 是 Netflix **服务发现服务器和客户端**。 可以将服务器**配置和部署为高度可用**的，每个服务器都将有关**已注册服务的状态复制给其他服务器**。 

依赖~

```xml
 <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.6</version>
        <relativePath/>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>eurekaservice</artifactId>


    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.6.6</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
            <version>3.1.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter</artifactId>
            <version>3.1.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>2.6.6</version>
        </dependency>
    </dependencies>

</project>

```
依赖分析

![在这里插入图片描述](https://img-blog.csdnimg.cn/a9162fb619144ebba166e615e5d482a8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

项目注册中心;可做集群 ; 默认端口 8761; 应用名称(spring.application.name) : ldx-eureka
端口规划 : 7000 ; 集群规划 7000,7001,7002;

配置文件~

```yml
server:
  port: 7000
spring:
  application:
    name: gavin_eureka
#因为指定了端口号,所以要改默认注册中心地址;
#eureka:
 # instance:
  #  hostname: gavin_eureka
  #client:
   # service-url:
    #  defaultZone: http://127.0.0.1:7000/eureka
    #register-with-eureka: false  #是否将该项目注册到注册中心
    #fetch-registry: false   #是否从注册中心抓取
```

将eureka配置注销掉之后---会有一些默认配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/d79b7fba8f2042ca80c030221bce04af.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
默认是8761端口
如果采取默认值~~启动eureka

因为tomcat默认启动端口为8080.而eureka默认在8761,所以报连接被拒绝异常~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ab87435b3374aec8e0aee30df01b875.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
将tomcat端口改为8761,能够正常连接

![在这里插入图片描述](https://img-blog.csdnimg.cn/102db40f867048b0b1bdfa813cc8b166.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
连接之后的界面~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/ecb142706f824fa2b26d2c429191a407.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

为什么要给定spring application name?~~**eureka也会给我们开一个实例**

![在这里插入图片描述](https://img-blog.csdnimg.cn/bd636d927aa84b6ababec91af19825dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20d6567315484bab9dbc1c9ecdd469c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 >其余的服务提供者都需要注册到eureka

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cd3792dfe1144e4810c9ce6beb15252.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)

 - **order模块**

~模块是要在eureka注册中心注册的~
~同时还需要连接数据库做curd的~   服务的提供者
依赖~

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>dependency</artifactId>
        <groupId>com.gavin</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>order</artifactId>

    <dependencies>
        <dependency>
            <groupId>com.gavin</groupId>
            <artifactId>commons</artifactId>
            <version>1.0-SNAPSHOT</version>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency> <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
        </dependency>

    </dependencies>

</project>

```

配置类
```yml
server:
  port: 8000
spring:
  application:
    name: gavin_order_client
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://172.23.141.26:3306/gavin
    username: gavin
    password: 955945
    druid:
      db-type: mysql
    type: com.alibaba.druid.pool.DruidDataSource
eureka:
  instance:
    hostname: gavin_order_client  #实例名
  client:
    service-url:
      defaultZone: http://127.0.0.1:7000/eureka  #eureka 地址
    register-with-eureka: true
    fetch-registry: true

```
启动类
```java
@SpringBootApplication
@EnableEurekaClient
public class ApplicationOrder {
    public static void main(String[] args) {
        SpringApplication.run(ApplicationOrder.class,args);
    }
}

```
之后看一下order在eurake中是否注册成功

~运行eurake 和 order模块~

注册
![在这里插入图片描述](https://img-blog.csdnimg.cn/81d91a645d324d20bb682ff4284b9e5b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
发现
![在这里插入图片描述](https://img-blog.csdnimg.cn/2f693ad058034e749adec063627301cd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **新建orderapi模块**
订单服务 ; 实际调用order ; 可做集群 ; 默认端口 8200 应用名称(spring.application.name) : orderapi
端口规划 : 8200 - 8399
>这里就参考dubbo框架


 - **新建goods模块**
依赖参考order

商品服务接口 ; 可做集群 ; 默认端口 8400 应用名称(spring.application.name) : ldx-goods
端口规划 : 8400 - 8599




 - **新建模块goodsapi**
商品服务 ; 实际调用ldx-goods; 可做集群 ; 默认端口 8600 应用名称(spring.application.name) : goodsapi
端口规划 : 8600 - 8799



这样框架基本搭建完毕,接下来完善代码~

### 微服务项目框架代码完善

 - **order模块代码完善**

明确~~order 作为服务的提供者
![在这里插入图片描述](https://img-blog.csdnimg.cn/d09b5d43fd584f15b4cbfa99da4feb81.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table(name = "orderinfo")
public class wfwOrderInfo {

    @Id
    @Column(name = "oid")
    @GeneratedValue(strategy=GenerationType.IDENTITY)
    private int oid;
    @Column(name = "number")
    private String number;

    private static final long serialVersionUID = -1L;
}


```
OrderRepository接口

>这里用jpa来与数据库连接操作,标准又规范
```java
//这里并不是远程调用,这里与数据库关联,所以要用自己的实体类,
//commons里的实体类是用作服务与服务之间传递数据用的,而他的 作用也就相当与一个传递数据的标准
import com.code.repository.info.OrderInfo;
import org.springframework.data.jpa.repository.JpaRepository;
public interface OrderRepository extends JpaRepository<OrderInfo,Integer> {
//这里是微服中的实体类
    int createOrder(OrderInfo orderInfo);
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/06482cc756654e3c931b88e122e1b0dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
OrderServiceImpl----------------->>接口定义省略
```java
@Service
//暴露的服务,让别人去调用
public class OrderServiceImpl implements OrderService {

    @Autowired
    private OrderRepository orderRepository;

    //创建订单 ,传进来的是commons中的实体类
    @Override
    public int createOrder(OrderInfo orderInfo) {
//而操作数据库用的是 order模块里的实体类,所以需要转化一下
       com.code.repository.info.OrderInfo orderInfo1 = new com.code.repository.info.OrderInfo();
        orderInfo1.setNumber(orderInfo.getNumber());
        int oid=-1;
        try{
            //接口调用失败,处理
            oid = orderRepository.save(orderInfo1).getOid();
        }catch(Exception e) {
            System.out.println(e);
            return -1;
        }
       return oid;
    }
}
```


阶段测试~~
测试类
```java
@SpringBootTest(classes = ApplicationOrder.class)
public class OrderApplicationTests {
  /* @Autowired
    private OrderService orderService;
*/
    @Test
    void Test01() {
    ...略....就是玩

    }

}

```
运行eureka 和测试类
![在这里插入图片描述](https://img-blog.csdnimg.cn/ea4631ec86a94594bc33831945306bb8.png)

>这里返回-1说明保存到数据库失败;

 - **完善orderapi模块代码**

明确orderapi为服务的调用者

我们的目的是使得服务的提供者与调用者不在一个服务器是,所以orderapi作为服务的调用者,代码如下


```java
import org.springframework.web.bind.annotation.RestController;

@RestController
public class OrderController {

    public String addOrder(String number){return "1";}

}

```
同时改模块在eureka注册中心中扮演了一个调用者身份----添加注解@EnableDiscoveryClient服务发现

```java
@EnableDiscoveryClient
@SpringBootApplication
public class ApplicationOrderService {
    public static void main(String[] args) {
        SpringApplication.run(ApplicationOrderService.class,args);
    }
}
```

服务调用
```java
@RestController
public class OrderInfoController {


//服务的调用者,怎样调用呢?

    public String addOrder(OrderInfo orderInfo){

        return "";
    }


}

```

 - **服务远程调用---openfeign**

引入依赖调用模块

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
    <version>3.1.1</version>
</dependency>
```


**远程调用的接口**
```java
package com.gavin.web;

import com.gavin.enity.OrderInfo;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.PostMapping;

/**
 * @Description: UP up UP
 * @author: Gavin
 * @date:2022/4/5 16:00
 */

/**@FeignClient(name,url)
 *name ="gavin-order-client"   调用的哪一个服务---服务提供者者的eureka-instance-hostname
 *
 * eureka:
 *   instance:
 *     hostname: gavin-order-client  #这是服务提供者中被调用的服务名
 
 *url ="loclhost:8000"服务提供者的地址,只需要写 ip:端口 即可
 * 
 *client:
 *  service-url:
 *   defaultZone: http://127.0.0.1:7000/eureka 
  */

@FeignClient(name ="gavin-order-client" , url = "localhost:8000")
public interface OrderFeignClient {

    /**
     *   @PostMapping("/order/add") 是被调用者中控制器的路径
     * @param orderInfo
     * @return
     */
    @PostMapping("/order/add")
 String add(OrderInfo orderInfo);
}


```

测试---
![在这里插入图片描述](https://img-blog.csdnimg.cn/62c0acdb7f6c4b2880c45fa7c30d7201.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd7d64d337df488fb14f8b9e7817f5c0.png)
这里参数传递还有待完善

>注意---由于是http远程调用,所以**实体类要实现序列化**以便数据传输
>

**那使用openfeign远程调用时的参数是怎样传递的呢?**


修改一下 远程调用接口

```java
    @RequestMapping("/addOrderInfoTest")
    public String addOrderInfo(@RequestParam String number){
        OrderInfo orderInfo= new OrderInfo();
//数据保存到orderinfo
        orderInfo.setNumber(number);
        orderInfo.setPrice(99.0f);
        System.out.println("第一步--orderinfo"+orderInfo);
//        远程调用
        OrderInfo orderInfo1 = orderFeignClient.addOrder(orderInfo);

        System.out.println(orderInfo1);
        return String.valueOf(orderInfo1.getOid());
    }


```
![在这里插入图片描述](https://img-blog.csdnimg.cn/33a75a06d3214443adf6d574bd3ac017.png)
可以看到参数在传递时发生的一些事情;

![在这里插入图片描述](https://img-blog.csdnimg.cn/d68d477015cf4317ac29212f75f9dcf0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>orderservice中的参数没有传递到 order,但是order中的参数可以传递到 orderservice

接下来就是要使得参数能够从orderservice传递到order


![在这里插入图片描述](https://img-blog.csdnimg.cn/22b9587d956d4cc99e641206c5288a30.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **负载均衡Ribbon**
随着集群的应用,负载均衡就显得很有必要了

![在这里插入图片描述](https://img-blog.csdnimg.cn/c140ef4049a5474ca84d59e3074d2fb6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
负载均衡的实现~

在远程调用时,openfeign已经包含了负载均衡的功能,所以可以直接去调用负载均衡的功能就可以了;

负载均衡的实现代码~~

**准备多个orderservice**

其实就是改变一下端口号---因为没有那么多服务器,虚拟机也不开了,一个为了避免每次启动改端口呀,可以建立多个springboot启动项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b2bbc8db84a4c57906b6bef2890d67e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

启动euteka,和order集群
![在这里插入图片描述](https://img-blog.csdnimg.cn/0094371e26674180b46605031721f2bc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

为了使用集群,把下面的url注掉
![在这里插入图片描述](https://img-blog.csdnimg.cn/fe66c38883d343bea031244f1c90e717.png)


这时候出现了一个问题---
![在这里插入图片描述](https://img-blog.csdnimg.cn/01033947e7054645ac03da8012e41acf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

未知host异常,
经过排查,原来是post解析未能正确解析地址
这时候修改

order配置文件如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/cbeb646860e34983b71c9c570d3757ed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

改完之后
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f24b1e363c74c9da2f45d33264a3a0f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
feign的负载均衡是由ribbon实现的,只不过feign包由依赖传递会自动导入ribbon

这里我们可以看到ribbon的策略主要有以下几种:

com.netflix.loadbalancer.RandomRule #配置规则 随机
com.netflix.loadbalancer.RoundRobinRule #配置规则 轮询
com.netflix.loadbalancer.RetryRule #配置规则 重试
com.netflix.loadbalancer.WeightedResponseTimeRule #配置规则 响应时间权重
com.netflix.loadbalancer.BestAvailableRule #配置规则 最空闲连接策略

 - **配置负载均衡模式~**

```yml
server:
  port: 8200
spring:
  application:
    name: gavin-eurake-server

eureka:
  instance:
    hostname: gavin-eurake-server  #实例名
  client:
    service-url:
      defaultZone: http://192.168.135.1:7000/eureka  #eureka 地址
    register-with-eureka: false
    fetch-registry: true
gavin-order-client:  # 在远程调用模块里配置~  服务提供者的负载均衡 模式
  ribbon: #负载均衡由ribbon实现

    #    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule #配置规则 随机
    #    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RoundRobinRule #配置规则 轮询
    #    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RetryRule #配置规则 重试
    #    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.WeightedResponseTimeRule #配置规则 响应时间权重
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.BestAvailableRule #配置规则 最空闲连接策略
    ConnectTimeout: 500 #请求连接超时时间
    ReadTimeout: 1000 #请求处理的超时时间
    OkToRetryOnAllOperations: true #对所有请求都进行重试
    MaxAutoRetriesNextServer: 2 #切换实例的重试次数
    MaxAutoRetries: 1 #对当前实例的重试次数

```

 - **熔断机制**
来看一下熔断机制
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c1e57c55672418488ee82a114277c55.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
啊,这里的熔断机制并不是股市的那个熔断,


而是------->>
 >在操作系统中, 某一个程序长时间占用CPU (俗称"卡死"), 就会触发操作系统的熔断机制.

![在这里插入图片描述](https://img-blog.csdnimg.cn/2a2f92dc951c437081ed5a7cb43ea36c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>熔断通过具有三种正常状态的有限状态机实现：**CLOSED，OPEN和HALF_OPEN以及两个特殊状态DISABLED和FORCED_OPEN**。当熔断器关闭时，所有的请求都会通过熔断器。如果失败率超过设定的阈值，熔断器就会从关闭状态转换到打开状态，这时所有的请求都会被拒绝。当经过一段时间后，熔断器会从打开状态转换到半开状态，这时仅有一定数量的请求会被放入，并重新计算失败率，如果失败率超过阈值，则变为打开状态，如果失败率低于阈值，则变为关闭状态。

**开启熔断机制**

首先明确熔断机制在哪里起作用,在服务调用的时候,如果没有响应超时或者没有得到回应,会触发熔断机制;

在消费端那里开启熔断机制----

依赖~

```xml
   <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
            <version>2.2.10.RELEASE</version>
        </dependency>

```
原理---当远程调用没有成功,那么就会在本消费端发生同名的方法调用老作为响应内容;

所以第一步 要有一个类来实现远程调用的接口,实现的主要目的是在发生熔断是能够执行同名的回调方法

```java
@Component
public class OrderFallback implements OrderFeignClient {

//发生了服务降级就会执行 这里的同名方法
    @Override
    public String add(OrderInfo orderInfo) {
        System.out.println("发生了熔断");
        return null;
    }

    @Override
    public OrderInfo addOrder(OrderInfo orderInfo) {
        System.out.println("发生了熔断---服务降级了");
        return null;
    }
}

```
>注意这里要修改一下controller层代码--实际上面的controller部分还少一些健壮机制,需要加上判断

```java
    @RequestMapping("/addOrderInfoTest")
    public String addOrderInfo( String number){
        OrderInfo orderInfo= new OrderInfo();
//数据保存到orderinfo
        orderInfo.setNumber(number);
        orderInfo.setPrice(99.0f);
        System.out.println("第一步--orderinfo{    "+orderInfo);

//        远程调用  后返回的 orderinfo 信息
        OrderInfo orderInfo1 = orderFeignClient.addOrder(orderInfo);
        System.out.println("远程调用---orderinfo{    "+orderInfo1);
//当远程调用失败时返回orderInfo1 为null,会报错
        return null==orderInfo1?"error":String.valueOf(orderInfo1.getOid());
    }

```


配置文件

```yml
server:
  port: 8200
spring:
  application:
    name: gavin-eurake-server
  cloud:
    circuitbreaker:
      hystrix:
        enabled: true  #spring-cloud 带的启动熔断配置

eureka:
  instance:
    hostname: gavin-eurake-server  #实例名
  client:
    service-url:
      defaultZone: http://192.168.135.1:7000/eureka  #eureka 地址
    register-with-eureka: false
    fetch-registry: true
gavin-order-client:
  ribbon:

    #NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule #配置规则 随机
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RoundRobinRule #配置规则 轮询
    #    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RetryRule #配置规则 重试
    #    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.WeightedResponseTimeRule #配置规则 响应时间权重
    # NFLoadBalancerRuleClassName: com.netflix.loadbalancer.BestAvailableRule #配置规则 最空闲连接策略
    ConnectTimeout: 500 #请求连接超时时间
    ReadTimeout: 1000 #请求处理的超时时间
    OkToRetryOnAllOperations: true #对所有请求都进行重试
    MaxAutoRetriesNextServer: 2 #切换实例的重试次数
    MaxAutoRetries: 1 #对当前实例的重试次数
feign:
  circuitbreaker:  #feign 带的熔断机制
    enabled: true
  metrics:
    enabled: true
hystrix:
  metrics:
    enabled: true
    polling-interval-ms: 1 #轮询间隔---即熔断周期 ,


```

接着在启动类中加上注解以开启熔断机制

```java
@EnableDiscoveryClient
@SpringBootApplication
@EnableFeignClients
@EnableHystrix //启用熔断器
public class ApplicationOrderService {

    public static void main(String[] args) {
        SpringApplication.run(ApplicationOrderService.class,args);
    }
}

```
最后模拟熔断时发生的一系列条件

开启测试
1,开启erueka
2,将服务提供者order处于debug模式,
3,开启消费者 orderservice


![在这里插入图片描述](https://img-blog.csdnimg.cn/8af29e90f9c445d8a2ae8d39b93afcea.png)

启动熔断关键配置---

配置文件上

```yml
feign:
  circuitbreaker:
    enabled: true
  metrics:
    enabled: true
```
回调类要实现远程调用接口并覆写接口中的方法

```java
@Component //交由spring管理
public class OrderFallback implements OrderFeignClient {
....发生熔断后的友好提示.....

}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/bf11f0d7bceb4d159d78e2428c4765be.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
注意---这里默认的熔断触发时间很短,

![在这里插入图片描述](https://img-blog.csdnimg.cn/91e5cc6d4bdc4314ba029b5b974c7e83.png)

```yml
hystrix:
  metrics:
    enabled: true #启用Hystrix指标轮询。默认为true。
    polling-interval-ms: 2000  #轮询间隔 后续轮询度量之间的间隔。默认为2000 ms。

```
当请求次数在一个轮询周期内,并不会再次进行远程调用,而是直接返回熔断后的处理结果 

![在这里插入图片描述](https://img-blog.csdnimg.cn/85a78df1fe0643228ab445623dacc79d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/eb54d9251914423fa0acfcfa1922a53e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **hystrix图表监视器**

新建模块hystrixPic

依赖~

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.7.RELEASE</version>
        <relativePath />
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>hytrixPic</artifactId>
    <properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
  
    </properties>

    <dependencies>
    <!--        已弃用，请使用spring-cloud-starter-netflix-hystrix-dashboard
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
            <version>2.2.10.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
            <version>2.2.10.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
            <version>2.6.6</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.6.6</version>
        </dependency>
     <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>2.6.6</version>
        </dependency>

    </dependencies>
</project>

```

配置文件

```yml
spring:
  application:
    name: hytrix-pic
server:
  port: 8890
hystrix:
  dashboard: #开启熔断监控面板
    proxy-stream-allow-list: "192.168.135.1"  #监控列表 用双引号不然报错
management:
  endpoints:
    web:
      exposure:
        exclude: "*"
```

启动类
```java
@SpringBootApplication
@EnableHystrixDashboard  //开启仪表板
public class ApplicationHytrixPic {
    public static void main(String[] args) {
        SpringApplication.run(ApplicationHytrixPic.class,args);
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/305918df76b9443b881e6cb1df6afce0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
在启动时会有警告哦---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/06d6d2bbf8874322b41cbceb52a6f07f.png)
显示没有 动态配置文件
那就补充一个---config.properties,文件内容暂为空
![在这里插入图片描述](https://img-blog.csdnimg.cn/0cf1c7eb0c2b41af91f17132854358c1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_15,color_FFFFFF,t_70,g_se,x_16)
仪表盘建立好之后还需要确定监控的目标

在消费类中添加依赖,相当于一个监控点

```yml
   <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
            <version>2.6.6</version>
        </dependency>
```
然后在orderservice添加配置类

```java
@Configuration
public class OrderActuator {
    /**
     * ServletRegistrationBean---一个注册容器,由spring管理
     * 在使用之前 需要设置指定
     * URL映射
     * name。如果没有指定servlet名称，将推断它。
     *
     * @return
     */
    @Bean
    public ServletRegistrationBean servletRegistrationBean() {
        HystrixMetricsStreamServlet hystrixMetricsStreamServlet = new HystrixMetricsStreamServlet();

        //create an instance
        ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(hystrixMetricsStreamServlet);
        
        //Set the name of this registration
        servletRegistrationBean.setName("servletRegistrationBean");
        //set priority startup
        servletRegistrationBean.setLoadOnStartup(1);
        
        //set url mapping  这里是监控的名字,监控哪个模块就要将该配置文件安放在那个模块里
        servletRegistrationBean.addUrlMappings("/turbine.stream");
        
        return servletRegistrationBean;
    }
}
```

依次开启相应模块
最后开启监控界面

在监控界面相应位置输入地址---->> 该地址是远程调用的ip:host/监控文件名

![在这里插入图片描述](https://img-blog.csdnimg.cn/b53e35dfa20f4950a331a11e815a9b53.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

监控后的界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/890c073d7d34440c880497a44adec7fd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
发送请求----->之后的结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/ae9f4d433895485da6c77b7dc4013986.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>注要想实现熔断可以设置为超时响应时间,或者干脆以debug模式运行orderservice

![在这里插入图片描述](https://img-blog.csdnimg.cn/10272b0a79f34870bbc61b54b498d4e8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
监控的运行逻辑~
![在这里插入图片描述](https://img-blog.csdnimg.cn/875b86e2939d49dc8ed2dab3c59e7321.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

