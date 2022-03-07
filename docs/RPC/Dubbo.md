

[![在这里插入图片描述](https://img-blog.csdnimg.cn/abdcc6f4ffbe4572be2b5028e2a55271.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)](https://dubbo.apache.org/zh/)

# Dubbo

## Dubbo简介

> Apache Dubbo 是一款高性能、轻量级的开源服务框架,Dubbo框架不仅仅是具备RPC访问功能，还包含服务治理功能。

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

> 提供高性能的基于代理的远程调用能力，服务以接口为粒度，为开发者屏蔽远程调用底层细节。
>
> 1. **智能容错和负载均衡;**
>    内置多种负载均衡策略，智能感知下游节点健康状况，显著减少调用延迟，提高系统吞吐量。
> 2. **服务自动注册和发现;**
>    支持多种注册中心服务，服务实例上下线实时感知。
> 3. **高度可扩展能力;**
>
> > 遵循微内核+插件的设计原则，所有核心能力如Protocol、Transport、Serialization被设计为扩展点，平等对待内置实现和第三方实现。
>
> 1. **运行期流量调度;**
>    内置条件、脚本等路由策略，通过配置不同的路由规则，轻松实现灰度发布，同机房优先等功能。
> 2. **可视化的服务治理与运维;**
>    提供丰富服务治理、运维工具：随时查询服务元数据、服务健康状态及调用统计，实时下发路由策略、调整配置参数。

## dubbo下载与安装

[![在这里插入图片描述](https://img-blog.csdnimg.cn/8dcd8281db9347bb9b0ecbdaef40245f.png)](https://www.apache.org/dyn/closer.lua/dubbo/3.0.5/apache-dubbo-3.0.5-src.zip)

## 定义服务

## IDL定义服务~官方推荐

> Dubbo3 推荐使用 IDL 定义跨语言服务

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

> 依赖于 Protobuf 序列化的兼容性，我们可以很容易的调整传输的数据结构如增、删字段等，完全不用担心接口的兼容性。

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

> git clone -b master https://github.com/apache/dubbo-samples.git

3,进入protbuf那个案例后,编译打包项目

> mvn clean package

运行provider

> [root@localhost target]# java -jar protobuf-provider-1.0-SNAPSHOT.jar 

dubbo启动成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/325b6b0a83fc4239b3d0aa3b2df4e587.png)

然后与运行consumer

> [root@localhost target]# java -jar protobuf-consumer-1.0-SNAPSHOT.jar 

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

> **为什么选择Triple协议?**

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

> dubbo2框架使用json序列化时，在每部分内容间额外增加了换行符作为分隔，要在Variable Part的每个part后额外增加换行符;

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

> 解决了大文件传输在分片传输的排序问题;

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

> 服务发现，即**消费端自动发现服务地址列表**的能力，是微服务框架需要具备的关键能力，借助于自动化的服务发现，微服务之间可以在**无需感知对端部署位置与 IP 地址的情况下实现通信**
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/16875391b4e846629e8189bf83b19ada.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
> 服务发现的一个核心组件是注册中心,Provider 注册地址到注册中心,Consumer从注册中心读取和订阅Provider地址列表,因此要启用服务发现,需要Dubbo增加注册中心配置:
> 配置模板--->>>

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

> 这种设计解决了流量分流和目标地址之间的耦合问题。不仅将配置规则进行了简化有效避免配置冗余的问题，还支持VirtualService和DestinationRule的任意组合，可以非常灵活的支持各种业务使用场景。

**手写dubbo2代码案例---->>**

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

### 代码执行流程:

1,开启zookeeper服务
![在这里插入图片描述](https://img-blog.csdnimg.cn/3dd587a848004fac9a9a133d43552778.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

2 com.ProviderApplication   
启动Spring容器;

3,DubboConfigBindingRegistrar 
获取dubbo配置文件,外部化配置优先级最高,然后是yaml,yml,最后是properties,其次还与配置文件位置有关;
同名配置会被优先级高的覆盖;
![在这里插入图片描述](https://img-blog.csdnimg.cn/fa313a790a824830b0c140d530c094cc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

4,trationDelegate$BeanPostProcessorChecker   代理检查 bean,由于刚刚注册所以并不符合被代理的条件;
![在这里插入图片描述](https://img-blog.csdnimg.cn/467dcc561e724909901c5c11345e8db3.png)

5,DubboConfigBindingBeanPostProcessor
dubbo配置前置处理器,
![在这里插入图片描述](https://img-blog.csdnimg.cn/e686c591d1bf4ca58ec7450869b46282.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

name : org.apache.dubbo.config.ApplicationConfig#0被 dubbo.application绑定,
![在这里插入图片描述](https://img-blog.csdnimg.cn/63ea4c9c7dc94387ba02beb148645fa3.png)

> 如果BeanPostProcessor接口的某个实现类被注册到某个容器，那么该容器的每个受管Bean在调用初始化方法之前，都会获得该接口实现类的一个回调。容器调用接口定义的方法时会将该受管Bean的实例和名字通过参数传入方法，进过处理后通过方法的返回值返回给容器。

6,zookeepe运行前的环境检查
![在这里插入图片描述](https://img-blog.csdnimg.cn/8660bf2102884f539177ab19b751642a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

CuratorFrameworkImpl 扩展框架开启zookeeper服务
![在这里插入图片描述](https://img-blog.csdnimg.cn/3cd0bfbe818b433195cd6b8b24ba6d29.png)
ClientCnxn是ZooKeeper客户端的核心工作类，负责维护客户端与服务端之间的网络连接并进行一系列网络通信
zookeeper开辟一个session域
![在这里插入图片描述](https://img-blog.csdnimg.cn/b43751bb1aa8475da088f10d0181846f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
其中由ConnectionStateManager  监控zookeeper链接的状态;
服务启动
![在这里插入图片描述](https://img-blog.csdnimg.cn/2bea8703f8bc41088fef55aec97cf636.png)

本地访问Zookpeer服务器很慢~甚至报错

```c
org.apache.zookeeper.KeeperException$ConnectionLossException: KeeperErrorCode = ConnectionLoss
	at org.apache.zookeeper.KeeperException.create(KeeperException.java:102) ~[zookeeper-3.6.3.jar:3.6.3]
	at org.apache.curator.framework.imps.CuratorFrameworkImpl.checkBackgroundRetry(CuratorFrameworkImpl.java:862) ~[curator-framework-4.2.0.jar:4.2.0]
	at org.apache.curator.framework.imps.CuratorFrameworkImpl.performBackgroundOperation(CuratorFrameworkImpl.java:990) ~[curator-framework-4.2.0.jar:4.2.0]
	at org.apache.curator.framework.imps.CuratorFrameworkImpl.backgroundOperationsLoop(CuratorFrameworkImpl.java:943) ~[curator-framework-4.2.0.jar:4.2.0]
```

> 原因是没有在host文件中配置zookeeper地址;

![在这里插入图片描述](https://img-blog.csdnimg.cn/c5c7f121d9734489b31b04b1ed8553ed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

上一篇文章介绍了Dubbo2代码的实现;本篇文章在上篇文章的基础上进行;

**Dubbo3实现代码**
目录结构--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/2d22ab23f50e44569bdcfca156d5cc88.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
父依赖--->

```xml
    <dependencyManagement>
<!--    避免使用 spring-boot-parent-->
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-dependencies -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.6.3</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
<!--        dubbo-bom-->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-bom</artifactId>
            <version>3.0.5</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>3.0.5</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>5.2.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
            <version>5.2.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.7.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-registry-zookeeper</artifactId>
            <version>3.0.5</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.8</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.28</version>
        </dependency>

    </dependencies>
</dependencyManagement>
```

Provider服务提供者(这里服务提供者与实现者放在一起了)
服务定义--->>

```java
/**
 * @author Gavin
 */
public interface EmpDubboService {
    /**
     *展示所有dept数据
     * @return
     * @param
     */
    List<Dept> show();
}

```

服务实现类--->>
EmpDubboServiceImpl

```java
package com.gavin.service.impl;
import com.gavin.mapper.DeptMapper;
import com.gavin.pojo.Dept;
import com.gavin.service.EmpDubboService;
import org.apache.dubbo.config.annotation.DubboService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import java.util.List;
@DubboService//要远程调用的服务
public class EmpDubboServiceImpl implements EmpDubboService {
@Autowired
    private DeptMapper deptMapper;
    @Override
    public List<Dept> show() {
        List<Dept> show = deptMapper.show();
        return show;
    }
}

```

mapper接口

```java
package com.gavin.mapper;
import com.gavin.pojo.Dept;
import org.apache.ibatis.annotations.Mapper;
import java.util.List;
/**
 * @author Gavin
 */
@Mapper
public interface DeptMapper {
   /**
    *  test
    * @return
    */
   List<Dept> show();
}
```

实体类与mapper.xml省略

> 其实这里可以使用mybatisplus来实现零xml,感兴趣可以自行实践;(但是其他的配置.....)

Consumer---服务消费者~远程调用服务

```java
package com.gavin.service.impl;
import com.gavin.pojo.Dept;
import com.gavin.service.DeptService;
import com.gavin.service.EmpDubboService;
import org.apache.dubbo.config.annotation.DubboReference;
import org.springframework.stereotype.Service;

import java.util.List;
/**
 * @author Gavin
 */
@Service
public class DeptServiceImpl implements DeptService {
    @DubboReference
    private EmpDubboService empDubboService;
    @Override
    public List<Dept> show() {
        List<Dept> show = empDubboService.show();
        return show;
    }
}

```

配置文件

```yml
spring:
  application:
    name: dubbo3-consumer
  datasource:
    # type表示不在使用spring内部的连接池,而是使用阿里的Druid连接池
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 填写你数据库的url、登录名、密码和数据库名
    url: jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
    username: gavin
    password: 955945
  druid:
    # 连接池的配置信息
    # 初始化大小，最小，最大
    initial-size: 5
    min-idle: 5
    maxActive: 20
    # 配置获取连接等待超时的时间
    maxWait: 60000
dubbo:
  application:
    name: dubbo3-consumer
  registry:
    address: zookeeper://192.168.135.145:2181
server:
  port: 8090
```

```yml:
spring:
  application:
    name: dubbo3-provider
  datasource:
    # type表示不在使用spring内部的连接池,而是使用阿里的Druid连接池
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 填写你数据库的url、登录名、密码和数据库名
    url: jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
    username: gavin
    password: 955945
  druid:
    # 连接池的配置信息
    # 初始化大小，最小，最大
    initial-size: 5
    min-idle: 5
    maxActive: 20
    # 配置获取连接等待超时的时间
    maxWait: 60000
dubbo:
  application:
    name: dubbo3-provider
  registry:
    address: zookeeper://192.168.135.145:2181
  protocol:
    port: 20884
    name: dubbo #配置协议名
```



运行结果--->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/19dbe5e9124349788e64d33c8f34fbdb.png)



## 部署架构

dubbo主要由三大中心组件协作注册中心 配置中心 元数据中心

- **注册中心**
  协调Consumer与Provider之间的地址注册和发现;
- **配置中心**
  存储Dubbo启动阶段的全局配置,保证配置在跨环境时的全局一致性
    负责服务治理规则(路由规则和动态配置)的存储与推送;
- **元数据中心**
  接收Provider上的服务接口元数据,为Admin等控制台提供运维能力(如服务测试,接口文档等)
    作为服务发现机制,提供额外的接口/方法级别的配置信息同步能力,可以作为注册中心的扩展;

### 微服务交互过程

**Dubbo 微服务组件与各个中心的交互过程。**
![在这里插入图片描述](https://img-blog.csdnimg.cn/7d702230a0204a2aa3d575a4621a64e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

> 三个中心并不是必要条件,可以根据业务情况按需开启一个或多个以达到简化部署的目的;
> 一般是注册中心开始Dubbbo服务的开启,配置中心和元数据中心则会在服务的开发过过程中按需要引入;

### 注册中心

Dubbo中会默认将**注册中心的实例同时作为配置中心和元数据中心**，这是Dubbo的默认行为，如果确实不需要配置中心或者元数据中心的能力，可在配置中关闭，在注册中心的配置中有两个配置分别为use-as-config-center和use-as-metadata-center，将配置置为false即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/49d67dd8cf8c450ab8e6025fe304cbd8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 元数据中心

**元数据中心在2.7.x版本开始支持**，随着应用级别的服务注册和服务发现在Dubbo中落地，元数据中心也变的越来越重要。在以下几种情况下会需要部署元数据中心：

部署元数据中心的几种情况---->>
1,对于一个原先采用**老版本Dubbo搭建的应用服务，在迁移到Dubbo 3时**

> 因为Dubbo 3 需要一个元数据中心来维护RPC服务与应用的映射关系（即接口与应用的映射关系），在采用了**应用级别的服务发现和服务注册**，在注册中心中将采用“应用 —— 实例列表”结构的数据组织形式，不再是以往的“接口 —— 实例列表”结构的数据组织形式，而以往用接口级别的服务注册和服务发现的应用服务在迁移到应用级别时，得不到接口与应用之间的对应关系，从而无法从注册中心得到实例列表信息，所以Dubbo为了兼容这种场景，在Provider端启动时，会往元数据中心存储接口与应用的映射关系。
>
> 2,为了让注册中心更加聚焦与地址的发现和推送能力，**减轻注册中心的负担**，这在上面提到过,元数据中心也可用来存储数据,元数据中心承载了所有的服务元数据、大量接口/方法级别配置信息等，无论是接口粒度还是应用粒度的服务发现和注册，元数据中心都起到了重要的作用。

![在这里插入图片描述](https://img-blog.csdnimg.cn/9e92712099e8435a985439f1c8c7e4d4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 配置中心

Provider与Consumer都可以从配置中心读取配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/1166df3da6db4c43a5be0685e04dedda.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
不配备元数据中心，意味着Consumer可以从Provider暴露的**MetadataService获取服务元数据**，从而实现RPC调用

三大中心已不再是Dubbo应用服务所必须的,但是在生产环境中,Dubbo需要应对高并发等带来的压力,因此Dubbo需要支持三大中心的高可用的方案----->
**多注册中心:**

> (provider)一个接口或者应用可以被注册到多个注册中心,同时consumer也可以从多个注册中心订阅相关服务,这样可以使得某一个注册中心不可用时能够切换到另一个注册中心以保证正常的服务提供与调用服务;

**多配置中心:**

> Dubbo支持多配置中心，来保证其中一个配置中心集群出现不可用时能够切换到另一个配置中心集群，保证能够正常从配置中心获取全局的配置、路由规则等信息

**多元数据中心**

> Dubbo 支持多元数据中心：用于应对容灾等情况导致某个元数据中心集群不可用，此时可以切换到另一个元数据中心集群，保证元数据中心能够正常提供有关服务元数据的管理能力。
>
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/c640c9778003474094a135a2c34df1e6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## 负载均衡

### 集群与伪集群

**集群**

> 在上述高可用架构下,需要涉及到集群的概念,即一个应用/服务可以部署多次,形成的整体就成为了集群,集群中的每一个个体都应该部署到不同而服务器上,这种方式就成为集群;

**伪集群**

> 如果部署在一个服务器上,则需要部署到不同的端口上以区分不同的个体,这种方式称为伪集群;

负载均衡是在集群的前提下,对每个节点访问次数/频率设置一个规则,并按照规则访问整个集群中的个体;

> Dubbo内置了四个负载均衡的策略,默认为Random

- Random策略

 随机访问,访问概率与权重有关;

- RoundRobin策略

轮询策略,访问概率与权重有关;

- LeastActive

活跃数相同的随机,不同的活跃数,活跃数高的优先访问;

- ConsistentHash

相同的参数(相同的hash码)的发送到同一个提供者

**负载均衡的设置**

在远程调用的(暴露的)应用/服务上添加负载均衡配置--**调用的服务采用的负载均衡**

![在这里插入图片描述](https://img-blog.csdnimg.cn/636cb833239d48e7bc0e55bd64a1bccd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

也可以在当前服务上开启负载均衡~~该类中的所有服务

![在这里插入图片描述](https://img-blog.csdnimg.cn/e1a291bf2dc346cca0bebd697f2c7144.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
全局负载均衡策略配置

![在这里插入图片描述](https://img-blog.csdnimg.cn/aa4df5101129432e843606b71b0d642c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



> 权重~服务器性能好的权重稍微高一些;
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/106d88b3d0944232ae379641506d55c7.png)

## Dubbo集群的开启

在前面文章dubbo2的基础上开启Dubbo集群~由于没有多台服务器,所以这里以伪集群进行操作;
将provider复制4份,(这里模拟的是zookeeper的leader,基数能够在挂掉时产生新的leader)

![在这里插入图片描述](https://img-blog.csdnimg.cn/f8651c9778684c708535df8542d8922b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/bf2a05953e5e417584f2e63b40fcf91e.png)
执行consumer
观察均衡负载的效果-->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/860c206fe7c94f309cbbf9f7fae77158.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

> 其他负载均衡策略可以自行实验;

## Dubbo3实现CURD

- 本项目用到的核心依赖~

> - springboot
> - mybais-plus
> - druid 
> - mysql 
> - thymeleaf

- 整体项目目录架构

![在这里插入图片描述](https://img-blog.csdnimg.cn/1a66b3adfefc4cc0a19dc14aac0f4f25.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

> 创建父工程,作用就是负责管理依赖

依赖-->>

```xml
    <properties>
        <java.version>1.8</java.version>
        <source.level>1.8</source.level>
        <target.level>1.8</target.level>
    </properties>
    <dependencyManagement>
        <!--    避免使用 spring-boot-parent-->
        <dependencies>

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.6.3</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--        dubbo-bom-->
            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-bom</artifactId>
                <version>3.0.5</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-spring-boot-starter</artifactId>
                <version>3.0.5</version>
            </dependency>
            <!--dubbo扩展-->
            <dependency>
                <groupId>org.apache.curator</groupId>
                <artifactId>curator-framework</artifactId>
                <version>5.2.0</version>
            </dependency>

            <dependency>
                <groupId>org.apache.curator</groupId>
                <artifactId>curator-recipes</artifactId>
                <version>5.2.0</version>
            </dependency>
            <!--zookeeper 依赖传递,可以不写-->
            <dependency>
                <groupId>org.apache.zookeeper</groupId>
                <artifactId>zookeeper</artifactId>
                <version>3.7.0</version>
            </dependency>
            <!--dubbo注册中心-->
            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-registry-zookeeper</artifactId>
                <version>3.0.5</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.22</version>
            </dependency>
            <!--mybatis-->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.2.2</version>
            </dependency>
            <!--德鲁伊连接池-->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-starter</artifactId>
                <version>1.2.8</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-jdbc</artifactId>
                <version>2.6.3</version>
            </dependency>

            <!--mysql-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.28</version>
            </dependency>
<!--            springDependencies 依赖里已经储备了.其他项目在引入依赖时会自动导入-->
            <!--前端展示技术-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
                <version>2.6.3</version>
            </dependency>

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.6.3</version>
            </dependency>
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>3.5.1</version>
            </dependency>


<!--            一个RESTFUL请求服务JAVA框架-->
            <dependency>
                <groupId>com.sun.jersey</groupId>
                <artifactId>jersey-client</artifactId>
                <version>1.19</version>
            </dependency>

            <dependency>
<!--                对多部分文件上传功能的支持。-->
                <groupId>commons-fileupload</groupId>
                <artifactId>commons-fileupload</artifactId>
                <version>1.4</version>
            </dependency>
<!--            文件上传/下载字节转换用到-->
            <dependency>
                <groupId>commons-io</groupId>
                <artifactId>commons-io</artifactId>
                <version>2.8.0</version>
            </dependency>
            <!--日志-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
                <version>2.6.2</version>
            </dependency>

            <!-- logback -->
<!--            logback 编码器、指定写入数据库的编码-->
            <dependency>
                <groupId>net.logstash.logback</groupId>
                <artifactId>logstash-logback-encoder</artifactId>
                <version>6.3</version>
            </dependency>

<!--日志框架 用于写入数据库-->
<!--            https://blog.csdn.net/weixin_54061333/article/details/122514913-->
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

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <version>2.6.3</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.6.3</version>
            </dependency>

        </dependencies>
    </dependencyManagement>

```

> 新建子类pojo~用于封装实体类

目录结构~
![在这里插入图片描述](https://img-blog.csdnimg.cn/2a0a6a4f45c040f1a6db6beccada3ee6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
依赖~
这里采用lombok依赖来简化封装

```xml
 <dependencies>
        <dependency>            <groupId>org.projectlombok</groupId>            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
```

封装的dept

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
/**
 * @author Gavin
 * @TableName(value = "dept",autoResultMap = true)
 * 或者去掉TableName这个注解以便去掉mybatisplus依赖
 */
public class Dept implements Serializable {

    private Integer deptno;

    private String dname;

    private String loc;

    private String deptpic;

    private static final long serialVersionUID = 1L;
}
```

> 新建子项目Mapper
> 该项目主要负责链接数据库curd

![在这里插入图片描述](https://img-blog.csdnimg.cn/8615d6e30b9640559f987d390a98e4a6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

这里用mybatisplus来简化sql代码

依赖~

```xml
<dependencies>
        <!--   mapper 依赖pojo 中的实体类  DeptMapper extends BaseMapper<Dept>-->
        <dependency>
            <artifactId>Pojo</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <!--这里要做的工作是链接数据库操作-->
        <!--德鲁伊连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.8</version>
        </dependency>

        <!--mysql 驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.28</version>
        </dependency>

        <!--数据库启动-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
            <version>2.6.3</version>
        </dependency>

<!--        Mapper继承BaseMapper时用-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
        </dependency>
    </dependencies>

```

DeptMapper

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.gavin.pojo.Dept;

public interface DeptMapper extends BaseMapper<Dept> {

}
```

需要配置一下mybatis-plus和数据库

```yml
server:
  port: 8099
  servlet:
    encoding:
      charset: UTF-8
#    context-path: /gavin
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
mybatis-plus:
  type-aliases-package: com.gavin.pojo
  mapper-locations: classpath:mybatis/*.xml #按照习惯来吧
  #限制文件上传的大小

```

> 接下来新建子项目ServerAPI项目

该项目主要负责指定服务标准~接口

![在这里插入图片描述](https://img-blog.csdnimg.cn/efdfa7f86f6e4d0b8c67a5e732c57f1e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
import com.baomidou.mybatisplus.extension.service.IService;
import com.gavin.pojo.Dept;

public interface DeptService extends IService<Dept> {
}

```

依赖~

```xml
    <!-- 依赖pojo 中的实体类 返回参数要用 由于依赖传递只需要依赖mapper-->
    <dependency>
        <artifactId>Mapper</artifactId>
        <groupId>com.codem</groupId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
```

> 新建子项目Provider

该项目主要用于提供服务,同时将服务发布到注册中心~zookeeper
开启dubbo框架~分布式微服务框架

![在这里插入图片描述](https://img-blog.csdnimg.cn/1bd9b3b8f2af47f1a7df1e51e5a03d59.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
依赖~

```xml
 <dependencies>
        <!--        依赖API  implements EmpService-->
        <!--依赖传递~mapper-->
        <dependency>            <artifactId>ServerAPI</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
        </dependency>

        <!--dubbo扩展-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
        </dependency>
        <!--zookeeper 依赖传递,可以不写-->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </dependency>
        <!--dubbo注册中心-->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-registry-zookeeper</artifactId>
        </dependency>
        <dependency>            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
        </dependency>
    </dependencies>

```

API的实现类

```java
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.gavin.mapper.DeptMapper;
import com.gavin.pojo.Dept;
import com.gavin.service.DeptService;
import org.apache.dubbo.config.annotation.DubboService;

import java.util.List;

/**
 * @author Gavin
 */
@DubboService//暴露该接口

public class DeptServiceImpl extends ServiceImpl<DeptMapper,Dept>implements DeptService {

}

```

新建spring启动器~因为dubbo需要spring容器

```java
import org.apache.dubbo.config.spring.context.annotation.EnableDubbo;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@EnableDubbo
@MapperScan(value = "com.gavin.mapper")//扫描mapper接口

public class ApplicationProvider {
    public static void main(String[] args) {
        SpringApplication.run(ApplicationProvider.class,args);
    }
}

```

> 新建子项目DeptConsumer

因为要调用暴露的服务,所以要依赖ServerAPI;

目录结构-->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/b1c729b28aaa49378277967f8c4f487d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

依赖~

```xml
 <dependencies>

        <dependency>
            <artifactId>ServerAPI</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>

        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
        </dependency>

        <!--dubbo扩展-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
        </dependency>
        <!--zookeeper 依赖传递,可以不写-->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </dependency>
        <!--dubbo注册中心-->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-registry-zookeeper</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>

    </dependencies>

```

像springweb一样,定义接口

```java
import com.gavin.pojo.Dept;
import java.util.List;
public interface DubboDeptService {
    public List<Dept> selectAll ();
}
```

接口实现类~

```java
import com.gavin.pojo.Dept;
import com.gavin.service.DeptService;
import com.gavin.service.DubboDeptService;
import org.apache.dubbo.config.annotation.DubboReference;
import org.springframework.stereotype.Service;

import java.util.List;
/**
 * @author Gavin
 */
@Service
public class DubboDeptServiceImpl implements DubboDeptService {
    @DubboReference//表示引用的ServiceAPI中的服务,可以指顶负载均衡
    private DeptService deptService;
    @Override
    public List<Dept> selectAll() {
        List<Dept> deptList = deptService.list();
        return deptList;
    }
}

```

controller

```java
@Controller
public class DubboDeptController {

    @Autowired
    private DubboDeptService dubboDeptService;

    @RequestMapping("/show")
    public String showDept(Model model){
        List<Dept> deptList = dubboDeptService.selectAll();

        model.addAttribute("list",deptList);

        return "index";
    }
    @ResponseBody
    @RequestMapping("/demo")
    public List<Dept> show(){

        return dubboDeptService.selectAll();

    }
}

```

启动类~

```java
@SpringBootApplication
@EnableDubbo
public class ApplicationDeptConsumer {
    public static void main(String[] args) {
        SpringApplication.run(ApplicationDeptConsumer.class,args);
    }
}

```

测试~
![在这里插入图片描述](https://img-blog.csdnimg.cn/1586f5574d734d26b5dfa974cb7b02e9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ffe1d37b8be64ddcad273376486af87c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
之后可以整合前面的文件的上传下载以及其他的一些技术去实现相应分布式开发的功能了;

[dubbo整合文件上传下载](https://gitee.com/varyu-program/CodeMar/tree/dubbo/)

> ~并不是通过rpc来实现,而是通过consumer层去实现



## Dubbbo3新特性

> Dubbo3 与 Dubbo2 的服务发现配置是完全一致的，不需要改动什么内容。但就实现原理上而言，Dubbo3 引入了全新的服务发现模型 - 应用级服务发现， 在工作原理、数据格式上已完全不能兼容老版本服务发现。

> Dubbo3 **应用级服务发现**，以应用粒度组织地址数据
> Dubbo2 **接口级服务发现**，以接口粒度组织地址数据
> Dubbo3 格式的 Provider 地址不能被 Dubbo2 的 Consumer 识别到，反之 Dubbo2 的消费者也不能订阅到 Dubbo3 Provider。

## 应用级地址发现迁移指南

<iframe   style="width: 648px; height: 502px;" src="https://dubbo.apache.org/zh/docs/migration/migration-service-discovery/ "></iframe></div>

[应用级地址发现迁移指南 | Apache Dubbo](https://dubbo.apache.org/zh/docs/migration/migration-service-discovery/) 

## Dubbo性能优化 & 基准测试

<iframe   style="width: 648px; height: 502px;" src="https://dubbo.apache.org/zh/docs/performance/benchmarking/"></iframe></div>



[性能优化 & 基准测试| Apache Dubbo](https://dubbo.apache.org/zh/docs/performance/benchmarking/)