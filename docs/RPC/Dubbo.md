

[![在这里插入图片描述](https://img-blog.csdnimg.cn/abdcc6f4ffbe4572be2b5028e2a55271.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)](https://dubbo.apache.org/zh/)
@[TOC](Dubbo)

## Dubbo简介

>Apache Dubbo 是一款高性能、轻量级的开源服务框架,Dubbo框架不仅仅是具备RPC访问功能，还包含服务治理功能。

**服务**是 Dubbo 中的**核心**概念，一个服务代表**一组 RPC 方法的集合**，服务是面向用户编程、服务发现机制等的基本单位。

**Dubbo 开发的基本流程**
用户**定义 RPC 服务**(接口)，通过约定的配置 方式将 RPC 声明为 Dubbo 服务，然后就可以基于服务 API 进行编程了。
对服务提供者来说是提供 RPC 服务的具体实现;
对服务消费者来说则是使用**特定数据发起服务调用** (通过注册中心,url绑定可调用方法的服务实例)。

## 发展历史

Dubbo最初是阿里内部使用的RPC框架,后来开源了,想要了解更多历史故事,还请自行BD
![在这里插入图片描述](https://img-blog.csdnimg.cn/136b653eec544c80b600632373f897d1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## 六大核心能力:
  1. **面向接口代理的高性能RPC调用;**
>提供高性能的基于代理的远程调用能力，服务以接口为粒度，为开发者屏蔽远程调用底层细节。
  2. **智能容错和负载均衡;**
>内置多种负载均衡策略，智能感知下游节点健康状况，显著减少调用延迟，提高系统吞吐量。
  3. **服务自动注册和发现;**
>支持多种注册中心服务，服务实例上下线实时感知。
  4. **高度可扩展能力;**
 >遵循微内核+插件的设计原则，所有核心能力如Protocol、Transport、Serialization被设计为扩展点，平等对待内置实现和第三方实现。
  5. **运行期流量调度;**
>内置条件、脚本等路由策略，通过配置不同的路由规则，轻松实现灰度发布，同机房优先等功能。
  6. **可视化的服务治理与运维;**
> 提供丰富服务治理、运维工具：随时查询服务元数据、服务健康状态及调用统计，实时下发路由策略、调整配置参数。

## dubbo下载与安装
[![在这里插入图片描述](https://img-blog.csdnimg.cn/8dcd8281db9347bb9b0ecbdaef40245f.png)](https://www.apache.org/dyn/closer.lua/dubbo/3.0.5/apache-dubbo-3.0.5-src.zip)


## 定义服务


## IDL定义服务~官方推荐
>Dubbo3 推荐使用 IDL 定义跨语言服务

服务示例文件---DemoService.proto
```c
syntax = "proto3";

option java_multiple_files = true;
option java_package = "org.apache.dubbo.demo";
option java_outer_classname = "DemoServiceProto";
option objc_class_prefix = "DEMOSRV";

package demoservice;

// The demo service definition.
service DemoService {#RPC服务名称
  rpc SayHello (HelloRequest) returns (HelloReply) {}#方法签名
}

// The request message containing the user's name.
message HelloRequest {#方法入参结构体
  string name = 1;
}

// The response message containing the greetings
message HelloReply {#方法出参结构体
  string message = 1;
}
```
 IDL 格式的服务**依赖 Protobuf 编译器**，用来生成可以被用户调用的客户端与服务端编程 API，Dubbo 在原生 Protobuf Compiler 的基础上提供了适配多种语言的特有插件，用于适配 Dubbo 框架特有的 API 与编程模型。

使用 **Dubbo3 IDL 定义的服务** **只允许一个入参与出参**，这种形式的服务签名🈶两个优势:

  1. 对多语言实现更友好; 
  2. 可以保证服务的向后兼容性;

>依赖于 Protobuf 序列化的兼容性，我们可以很容易的调整传输的数据结构如增、删字段等，完全不用担心接口的兼容性。

## 编译服务

**Java**
Java 语言生成的 stub 如下，核心是一个接口定义

```java
@javax.annotation.Generated(
value = "by Dubbo generator",
comments = "Source: DemoService.proto")
public interface DemoService {
    static final String JAVA_SERVICE_NAME = "org.apache.dubbo.demo.DemoService";
    static final String SERVICE_NAME = "demoservice.DemoService";

    org.apache.dubbo.demo.HelloReply sayHello(org.apache.dubbo.demo.HelloRequest request); CompletableFuture<org.apache.dubbo.demo.HelloReply> sayHelloAsync(org.apache.dubbo.demo.HelloRequest request);
}
```

## 配置和加载服务
**Java 版代码** 

提供端，实现服务
```java
public class DemoServiceImpl implements DemoService {
    private static final Logger logger = LoggerFactory.getLogger(DemoServiceImpl.class);

    @Override
    public HelloReply sayHello(HelloRequest request) {
        logger.info("Hello " + request.getName() + ", request from consumer: " + RpcContext.getContext().getRemoteAddress());
        return HelloReply.newBuilder()
                .setMessage("Hello " + request.getName() + ", response from provider: "
                        + RpcContext.getContext().getLocalAddress())
                .build();
    }

    @Override
    public CompletableFuture<HelloReply> sayHelloAsync(HelloRequest request) {
        return CompletableFuture.completedFuture(sayHello(request));
    }
}
```
**配置文件**
提供端，注册服务(以 Spring XML 为例)

```xml
<bean id="demoServiceImpl" class="org.apache.dubbo.demo.provider.DemoServiceImpl"/>
<dubbo:service serialization="protobuf" interface="org.apache.dubbo.demo.DemoService" ref="demoServiceImpl"/>
```

消费端，引用服务

```xml
<dubbo:reference scope="remote" id="demoService" check="false" interface="org.apache.dubbo.demo.DemoService"/>
```

消费端，使用服务 proxy

```java
public void callService() throws Exception {
    ...
    DemoService demoService = context.getBean("demoService", DemoService.class);
    HelloRequest request = HelloRequest.newBuilder().setName("Hello").build();
    HelloReply reply = demoService.sayHello(request);
    System.out.println("result: " + reply.getMessage());
}
```

先运行一下然后在学习原理
案例准备-->>
1,在D盘下新建文件夹,
2,git 克隆 官方提供的源码--->>
>git clone -b master https://github.com/apache/dubbo-samples.git

3,进入protbuf那个案例后,编译打包项目
>mvn clean package

运行provider
>[root@localhost target]# java -jar protobuf-provider-1.0-SNAPSHOT.jar 

dubbo启动成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/325b6b0a83fc4239b3d0aa3b2df4e587.png)

然后与运行consumer

>[root@localhost target]# java -jar protobuf-consumer-1.0-SNAPSHOT.jar 

运行结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/327e4e6c828b4b288e45eca1a67f523e.png)

## 框架与架构

## Dubbo架构说明
**Container**  容器,Dubbo基于spring实现的;
**Provider**    生产者,负责将地址和服务注册到注册中心   编写持久层和事务代码;
**Registry**     注册中心 存放provider提供的信息 遵循的协议  ip+端口  (类似于rmi://localhost:2181),接口信息+可调用的方法;
**Consumer**  消费者（RPC调用者，SOA调用服务的项目）开发中也是一个项目，调用服务中的方法。
**Monitor**     监控中心,用于监控Provider的压力情况,每隔几分钟Consumer和Proivider会把调用次数发给Monitor,由monitor进行统计(默认2分钟);

 - 1,spring项目启动后Provider会把相关信息注册到注册中心;
 - 2,Consumer从注册中心订阅消息,如果有匹配的Provider发布的信息,则注册中心通知Consumer;
 - 3,Consumer根据通知信息调用Provider中的方法;
 - 4,Consumer与Provider将调用次数异步发送给Monitor进行统计;

异步非阻塞线程性能高,同步阻塞线程必须等待响应结果才能继续执行,性能相对较低;

## Dubbo支持的协议
Dubbo3 提供了 Triple(Dubbo3)、Dubbo2 协议，这是 Dubbo 框架的原生协议。除此之外，Dubbo3 也对众多第三方协议进行了集成，并将它们纳入 Dubbo 的编程与服务治理体系， 包括 **gRPC**、Thrift、JsonRPC、Hessian2、REST 等;

 - 部分协议优缺点:

**Dubbo协议**
优点:Nio服用单一协议,并使用线程池并发处理请求,减少了握手次数,提高了并发效率,性能好;
缺点:对大文件上传不太友好;
**Rmi协议**
优点:JDK自带的;
缺点:偶尔会链接失败;
**Hessian协议**
优点:可与原生Hessian互操作，基于HTTP协议
缺点:要hessian.jar支持，http短连接的开销大;
**gRPC**
 优点:协议简单,学习成本低,有server push/ 多路复用 / 流量控制能力;可跨平台和跨语言(基于 Protobuf 的多语言跨平台);协议生态丰富;
 缺点:对服务治理的支持比较基础较高的学习成本和改造成本;迁移成本较大;
 **Triple协议**
Triple协议也是Dubbbo3推荐的协议;
优点:
完整**兼容grpc**;
具备**跨语言互通**的能力;
**请求模型更完善**;
**易扩展**,**穿透性**好;
**多种序列化支持**,升级成本低~需要简单的修改协议名便可以轻松升级到 Triple 协议;

>**为什么选择Triple协议?**

Triple协议兼容 gRPC ，并且支持HTTP2 ;

 - **性能上**,Triple 协议采取了 **metadata 和 payload 分离**的策略，这样就可以避免中间设备，如网关进行 payload 的解析和反序列化，从而**降低响应时间**。 
 - **路由支持上**，由于 metadata 支持用户添加**自定义 header** ，用户可以根据 header更方便的**划分集群或者进行路由**，这样发布的时候切流灰度或容灾都有了**更高的灵活性**。 
 - **安全性上**，支持双向TLS认证（mTLS）等**加密传输**能力。
   **易用性上**，Triple 除了支持原生 gRPC 所推荐的 Protobuf 序列化外，使用通用的方式支持了 Hessian / JSON等其他序列化，能让用户更方便的升级到 Triple 协议。对原有的 Dubbo 服务而言，修改或增加 Triple 协议只需要在声明服务的代码块添加一行协议配置即可，**改造成本几乎为 0**;

![在这里插入图片描述](https://img-blog.csdnimg.cn/815570519f6f4cdf9204550614d04fb6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## Dubbo2的协议内容
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d3a220c7fc745b18a38b863bb696c94.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_17,color_FFFFFF,t_70,g_se,x_16)
**Magic - Magic High & Magic Low** :识别dubbo协议位
**Req/Resp** :请求和响应标记位 1 是请求,0是响应
**2way**:仅当Req/Resp标记位为1(请求)时才有用,设置为1会得到服务器的返回值,为0则不会收到返回值;
**Event**:事件标记位,是事件则标记为1;
**SerializationID**:识别序列化类型~fastjson为6,其他也有对应的数字表示;
**Status**:状态标记位,仅当Req/Resp为0(响应)时有用,用于标记响应码,20,30,40等标记码
  1. 20 - OK
  2. 30 - CLIENT_TIMEOUT
  3. 31 - SERVER_TIMEOUT
  4. 40 - BAD_REQUEST
  5. 50 - BAD_RESPONSE
  6. 60 - SERVICE_NOT_FOUND
  7. 70 - SERVICE_ERROR
  8. 80 - SERVER_ERROR
  9. 90 - CLIENT_ERROR
  10. 100 - SERVER_THREADPOOL_EXHAUSTED_ERROR
  **RPC REQUEST ID**:标识唯一请求ID
  **DATA LENGTH**:数据长度(序列化之后的)
  **Variable length Part**:可变长度部分
  如果Req/Resp为1(请求),则该部分内容依次是:
   1 Dubbo version 
   2 Service name 
   3 Service version
   4 Method name 
   5 Method parameter types 
   6 Method arguments 
   7 Attachments

如果Req/Resp为0(响应),则该部分内容依次是:
返回值类型，标识从服务器端返回的值类型：RESPONSE_NULL_VALUE - 2，RESPONSE_VALUE - 1，RESPONSE_WITH_EXCEPTION - 0。
返回值,从服务器返回的实际值。

>dubbo2框架使用json序列化时，在每部分内容间额外增加了换行符作为分隔，要在Variable Part的每个part后额外增加换行符;
## Triple 协议内容:
RPC协议的内容包含三部分:

**数据交换格式**： 定义 RPC 的请求和响应对象在网络传输中的字节流内容，也叫作序列化方式
**协议结构**： 定义包含字段列表和各字段语义以及不同字段的排列方式
**协议通过定义规则、格式和语义来约定数据如何在网络间传输**。一次成功的 RPC 需要通信的两端都能够按照协议约定进行网络字节流的读写和对象转换。如果两端对使用的协议不能达成一致，就会出现鸡同鸭讲，无法满足远程通信的需求。

Triple协议是基于 grpc 协议进行进一步扩展,

```c
Service-Version → “tri-service-version” {Dubbo service version}#triple协议新增
Service-Group → “tri-service-group” {Dubbo service group}#triple协议新增
Tracing-ID → “tri-trace-traceid” {tracing id}#用于全链路追踪
Tracing-RPC-ID → “tri-trace-rpcid” {_span id _}#用于全链路追踪
Cluster-Info → “tri-unit-info” {cluster infomation}#集群信息,灵活治理服务
```
Triple协议相比传统的unary方式，多了目前提供的Streaming RPC的能力
>解决了大文件传输在分片传输的排序问题;

通过Triple协议的Streaming RPC方式，会在consumer跟provider之间建立多条用户态的长连接Stream。同一个TCP连接之上能同时存在多个Stream，其中每条Stream都有StreamId进行标识，对于一条Stream上的数据包会以顺序方式读写。
[RPC协议的内容:](https://dubbo.apache.org/zh/docs/concepts/rpc-protocol/)

  1. 数据交换格式~序列化 (实现unicastObject)~
  2. 协议结构~定义包含字段列表和各字段语义以及不同的排列方式~即协议制定的数据传输标准;
  3. 协议通过定义规则、格式和语义来约定数据如何在网络间传输。


## Dubbo支持的注册中心
 - Zookeeper

优点:支持分布式;
缺点:原生稳定性稍差,需要抓门的辅助软件来提高稳定性;

 - Multicast
  优点:去中心化,不需要单独安装软件
  缺点: Provider和Consumer和Registry不能跨机房(路由)

 - Redis
  优点:支持集群,性能好;
  缺点:服务器时间要同步,否则可能会出现集群失败;

 - Simple
    标准RPC服务,不支持集群

### 服务发现模型
>服务发现，即**消费端自动发现服务地址列表**的能力，是微服务框架需要具备的关键能力，借助于自动化的服务发现，微服务之间可以在**无需感知对端部署位置与 IP 地址的情况下实现通信**
>![在这里插入图片描述](https://img-blog.csdnimg.cn/16875391b4e846629e8189bf83b19ada.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>服务发现的一个核心组件是注册中心,Provider 注册地址到注册中心,Consumer从注册中心读取和订阅Provider地址列表,因此要启用服务发现,需要Dubbo增加注册中心配置:
>配置模板--->>>

```yml
# application.yml
dubbo:
 registry:
  address: zookeeper://127.0.0.1:2181
```
**Dubbo3应用级服务发现优势**

  1. 适配云原生微服务,是一个通用的模型;
  2. 性能和可伸缩性上都有所提升,支持超大规模集群,引入的应用级服务发现模型,从本质上解决了注册中心地址数据存储与推送压力,Consumer侧的地址计算压力也显著下降,集群规模变得可预测可评估(与RPC接口数量无关,只和实力部署的规模有关);


### Dubbo2服务发现模型

![在这里插入图片描述](https://img-blog.csdnimg.cn/baaacbfc94d24d349856e41255790533.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
在接入云原生基础设施后,基础设施融入了微服务概念的抽象，容器化微服务被编排、调度的过程即完成了**在基础设施层面的注册**。如下图所示，**基础设施既承担了注册中心的职责，又完成了服务注册的动作**，而 “RPC 接口”这部分信息，由于与具体的业务相关，不可能也不适合被基础设施托管。
![在这里插入图片描述](https://img-blog.csdnimg.cn/0e47341edf7c4f47a2a47fd78aaa7c9a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### Dubbo3服务发现模型
 Dubbo3 需要在原有服务发现流程中抽象出通用的、与业务逻辑无关的地址映射模型，并确保这部分模型足够合理，以支持将地址的注册行为和存储委托给下层基础设施 Dubbo3 特有的业务接口同步机制，是 Dubbo3 需要保留的优势，需要在 Dubbo3 中定义的新地址模型之上，通过框架内的自有机制予以解决。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/ef11b58e0faa4daf935a0bbcbb0124a1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
 Dubbo3 服务发现模型**更适合构建可伸缩的服务体系**;
 假设一个微服务应用定义了 100 个接口（Dubbo 中的服务）， 则需要往注册中心中注册 100 个服务，按照dubbo2的服务发现模型,如果这个应用被部署在了 100 台机器上，那这 100 个服务总共会产生 100 * 100 = 10000 个虚拟节点；
 而同样的应用， 对于 Dubbo3 来说，新的注册发现模型只需要 1 个服务（只和应用有关和接口无关）， 只注册和机器实例数相等的 1 * 100 = 100 个虚拟节点到注册中心。 Dubbo 所注册的地址数量下降到了原来的 1 / 100，对于注册中心、订阅方的存储压力都是一个极大的释放。
 更重要的是， **地址发现容量彻底与业务 RPC 定义解耦**开来，整个集群的容量评估对运维来说将变得更加透明：部署多少台机器就会有多大负载，不会像 Dubbo2 一样， 因为业务 RPC 重构就会影响到整个集群服务发现的稳定性。


## 服务流量管理
由Dubbo定义的路由规则,实现对流量的分布式控制

![在这里插入图片描述](https://img-blog.csdnimg.cn/13d78bbde2b547d381e5b8d7112236e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
路由对应 应用服务~一对多;反之多对一;
路由也可以不对应服务,此时会因为没有对应的服务处理而报错;
Dubbo将流量管理分成了VirtualService和DestinationRule两部分.当consumer接收到了一个请求,会根据VirtualService中定义的DubboRoute和DubboRouteDetail匹配到对应的DubboDestination中的subnet，最后根据DestinationRule中配置的subnet信息中的labels找到对应需要具体路由的Provider集群;

**VirtualService**主要处理**入站流量分流**的规则，支持服务级别和方法级别的分流。
**DubboRoute**主要解决**服务级别的分流**问题。同时，还提供的重试机制、超时、故障注入、镜像流量等能力。
**DubboRouteDetail**主要解决某个服务中**方法级别的分流**问题。支持方法名、方法参数、参数个数、参数类型、header等各种维度的分流能力。同时也支持方法级的重试机制、超时、故障注入、镜像流量等能力。
**DubboDestination**用来描述**路由流量的目标地址**，支持host、port、subnet等方式。
**DestinationRule**主要处理**目标地址规则**，可以通过hosts、subnet等方式关联到Provider集群。同时可以通过trafficPolicy来实现负载均衡。

>这种设计解决了流量分流和目标地址之间的耦合问题。不仅将配置规则进行了简化有效避免配置冗余的问题，还支持VirtualService和DestinationRule的任意组合，可以非常灵活的支持各种业务使用场景。


**手写代码案例---->>**
新建一个项目---DubboPro,
依赖--->>

```xml
   
    <dependencyManagement>

        <dependencies>

            <!--springboot启动依赖-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
                <version>2.6.3</version>
            </dependency>
            <!--        dubbo启动依赖-->
            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-spring-boot-starter</artifactId>
                <version>2.7.3</version>
            </dependency>

            <!--         recipes doc中列出Zookeeper中所有recipes(除了两阶段提交)。  。-->
            <dependency>
                <groupId>org.apache.curator</groupId>
                <artifactId>curator-recipes</artifactId>
                <version>4.2.0</version>
            </dependency>
            <!--        依赖传递 curator-framework  zookeeper-->
            <!--        简化zookeeper的使用-->
            <dependency>
                <groupId>org.apache.curator</groupId>
                <artifactId>curator-framework</artifactId>
                <version>4.2.0</version>
            </dependency>

            <dependency>
                <groupId>org.apache.zookeeper</groupId>
                <artifactId>zookeeper</artifactId>
                <version>3.6.3</version>
            </dependency>
<!--            consumer用-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.5.9</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

```
抽象出一个子类API,用于定义服务标准---
新建一个服务接口---MyDubboService

```java
package com.gaivn.service;
/**
 * @author Gavin
 */
public interface MyDubboService {
    /**
     *一个测试类
     * @return
     * @param Param
     */
    String  show(String Param);
}

```
建立子项目dubboProvider
依赖-->

```xml
    <dependencies>
<!--        依赖 service接口所在类-->
        <dependency>
            <groupId>com.gavin.dubboDemo</groupId>
            <artifactId>dubboProject</artifactId>
           
        </dependency>
<!--springboot启动依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
           
        </dependency>
<!--        dubbo启动依赖-->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
          
        </dependency>
<!--         recipes doc中列出Zookeeper中所有recipes(除了两阶段提交)。  。-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
          
        </dependency>
        <!--
&lt;!&ndash;        依赖传递 curator-framework  zookeeper&ndash;&gt;
&lt;!&ndash;        简化zookeeper的使用&ndash;&gt;
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>5.2.0</version>
        </dependency>

        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.6.3</version>
        </dependency>

-->
    </dependencies>
```
依赖传递--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/c334d64b17804d9ca875100fabb51623.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
配置文件-->

```yml
spring:
  application:
    name: dubbo-provider
dubbo:
  application:
    name: dubbo-provider
  registry:
    address: zookeeper://192.168.135.145:2181
  protocol: #协议类型
    port: 20884
     name: dubbo #配置协议名称
     #若不配置协议名，则又可能会报错->>org.apache.dubbo.config.ProtocolConfig#0“ contains illegal character, only digit

```
service实现类

```java
package com.gavin.service.impl;

import com.gaivn.service.MyDubboService;
import org.apache.dubbo.config.annotation.Service;

/**
 * @author Gavin
 */
@Service //证明这个dubbo服务~即远程调用的服务接口--服务提供者 从2.7.6以后该注解被废弃,变成了DubboService

public class MyDubboServiceImpl  implements MyDubboService {
    @Override
    public String show(String Param) {
        System.out.println("show Invoked");
        return Param+"你好";
    }
}


```

建立启动类

```java
package com;

import org.apache.dubbo.config.spring.context.annotation.EnableDubbo;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @author Gavin
 */
@SpringBootApplication
@EnableDubbo//该启动类由dubbo管理

public class ProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(ProviderApplication.class,args);
    }
}

```

新建子项目Consumer
依赖---->>

```xml
    <dependencies>
        <dependency>
            <groupId>com.codem</groupId>
            <artifactId>API</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <!--springboot启动依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <!-- web启动依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--        dubbo启动依赖-->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
        </dependency>

        <!--         recipes doc中列出Zookeeper中所有recipes(除了两阶段提交)。  。-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
        </dependency>
        <!--        依赖传递 curator-framework  zookeeper-->
        <!--        简化zookeeper的使用-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </dependency>
    </dependencies>
```
Consumer中的接口

```java
package com.gavin.service;

/**
 * @author Gavin
 */
public interface DubboConsumerService {
    /**
     * 一个测试类
     * @return
     */
    String show(String name);
}


```
接口实现类
```java
package com.gavin.service.Impl;

import com.gaivn.service.MyDubboService;
import com.gavin.service.DubboConsumerService;
import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Service;

/**
 * @author Gavin
 */
@Service//这里是spring中的service,dubbo3之前的版本也有一个service,要区别一下

public class DubboConsumerServiceImpl implements DubboConsumerService {
    @Reference
    private MyDubboService myDubboService;
    @Override
    public String show(String name) {
       return  myDubboService.show(name);
    }
}

```
控制层--->>

```java
package com.gavin.controller;

import com.gaivn.service.MyDubboService;
import com.gavin.service.DubboConsumerService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * @author Gavin
 */
@Controller
public class ConsumerController {
    @Autowired
    private DubboConsumerService dubboConsumerService;

    @RequestMapping("/show")
    @ResponseBody
    public String show(String Param) {
        String show = dubboConsumerService.show(Param);
        return Param + show;
    }
}

```

启动类--

```java
package com.gavin;

import org.apache.dubbo.config.spring.context.annotation.EnableDubbo;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @author Gavin
 */
@SpringBootApplication
@EnableDubbo
public class ConsumerApplication{
    public static void main(String[] args) {
        SpringApplication.run(ConsumerApplication.class,args);
    }
}

```
配置文件

```yml
spring:
  application:
    name: dubbo-consumer #手动配置app名
dubbo:
  application:
    name: dubbo-consumer
  registry:
    address: zookeeper://192.168.135.145:2181
```

运行-->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/d4b49bd274c44093835173617cba18eb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

查看zookeeper中的内容
![在这里插入图片描述](https://img-blog.csdnimg.cn/247ae8ea1c4746e08245fd010ff83f58.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## Dubbbo3新特性

>Dubbo3 与 Dubbo2 的服务发现配置是完全一致的，不需要改动什么内容。但就实现原理上而言，Dubbo3 引入了全新的服务发现模型 - 应用级服务发现， 在工作原理、数据格式上已完全不能兼容老版本服务发现。

>Dubbo3 **应用级服务发现**，以应用粒度组织地址数据
>Dubbo2 **接口级服务发现**，以接口粒度组织地址数据
>Dubbo3 格式的 Provider 地址不能被 Dubbo2 的 Consumer 识别到，反之 Dubbo2 的消费者也不能订阅到 Dubbo3 Provider。


[应用级地址发现迁移指南](https://dubbo.apache.org/zh/docs/migration/migration-service-discovery/)

