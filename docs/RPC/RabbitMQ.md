# AMQP

## AMQP简介

AMQP是一个**高级消息队列协议**,使得遵从该规范的客户端中的进程能够传递异步消息的网络协议。

  * 什么是消息队列?
1. 其本质是一个转发器,作为消息的发布者与消息的接收者的桥梁;
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/3fc9d5d682f14207a3ef045712bcb45c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


**AMQP工作模型**
这里以Rabbit为例
所有服务器实现都应该遵从AMQP协议，否则就服务器之间无法实现异步消息传输;

在服务器中，三个主要功能模块连接成一个处理链以便完成预期的功能：
首先是两端publisher与consumer,可以理解为生产者消费者;

 - “**exchange**”接收发布应用程序发送的**消息**，并根据一定的规则将这些**消息路由到“消息队列”。**

>常用的交换器类型
>1. direct(发布与订阅 完全匹配)
>2. fanout(广播)
>3. topic(主题，规则匹配)

 - “**message queue**" **存储消息**，直到这些消息被消费者安全处理完为止。

> message是不具名的,他由消息头和消息体组成,消息体不透明,二消息头可由可选属性组成~
>
>  - **routing~key(路由键)**
>  - **priority(优先级)**~相对于其他消息的优先权
>  - **dilivery-mode(分发模式)**~可指出消息的可持久性存储等

 - “**Binding**”定义了exchange和message queue之间的关联，提供**路由规则**。

使用这个模型我们可以很容易的模拟出存储转发队列和主题订阅这些典型的消息中间件概念。

 - [x] 一个AMQP服务器类似于邮件服务器;
 - [x] exchange类似于消息传输代理（email里的概念）;
 - [x] message queue类似于邮箱。

>**Binding定义了每一个传输代理中的消息路由表**，发布者将消息发给特定的传输代理，然后传输代理将这些消息路由到邮箱中，消费者从这些邮箱中取出消息。
>一个绑定就是基于**路由键**将交换器和消息队列连接起来的路由规则;

![在这里插入图片描述](https://img-blog.csdnimg.cn/2318798c18b44203ae67400003fde545.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **Routing-key路由键**

是RabbitMQ决定消息该投递到哪个队列的规则。（也可以理解为队列的名称，路由键是key，队列是value）队列通过路由键绑定到交换器。消息发送到MQ服务器时，消息将拥有一个路由键，即便是空的，RabbitMQ也会将其和绑定使用的路由键进行匹配。如果相匹配，消息将会投递到该队列。如果不匹配，消息将会进入黑洞。


 - **Queue消息队列**。

用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者链接到这个队列将其取走。

消息队列的特点~
先进先出,
![在这里插入图片描述](https://img-blog.csdnimg.cn/c8556a22c93e47c78bf40c60541ad5ab.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_13,color_FFFFFF,t_70,g_se,x_16)

>实际部署中,publisher,exchange,之间是一对多关系,exchange与queue之间一对多关系,
>queue与consumer之间也是一对多关系;
>![在这里插入图片描述](https://img-blog.csdnimg.cn/6a78acb6f40248d5a310551b84ae875b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_15,color_FFFFFF,t_70,g_se,x_16)


# RabbitMQ入门
**RabbitMQ**

> RabbitMQ 是轻量级的，易于在本地和云中部署。它支持多种消息传递协议。RabbitMQ可以部署在分布式和联合配置中，以满足大规模、高可用性的需求。
> RabbitMQ是比较常用的AMQP框架,

RabbitMQ是由Erlang语言编写的基于**AMQP的消息中间件**。而消息中间件作为分布式系统重要组件之一，可以**解决应用耦合，异步消息，流量削峰等问题。**

RabbitMQ的运行原理~
![在这里插入图片描述](https://img-blog.csdnimg.cn/913cfd6f39c146998959d4dc3a52f1c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
这样就将A与B进行了解耦,便于后期的维护和功能扩展;

>RabbitMQ~消息系统允许**软件、应用相互连接和扩展**．这些应用可以相互链接起来组成一个更大的应用，或者将用户设备和数据进行连接．消息系统通过将消息的发送和接收分离来实**现应用程序的异步和解偶．**

**RabbitMQ能做什么?**

 - [x] 1,数据投递; 
 - [x] 2,非阻塞操作或推送通知; 
 - [x] 3,消息的发布／订阅，异步处理; 
 - [x] 4,工作队列


**适用的场景**

 - [x] 排队算法 
 - [x] 秒杀活动 
 - [x] 消息分发 
 - [x] 异步处理 
 - [x] 数据同步 
 - [x] 处理耗时任务 
 - [x] 流量削峰
## AMQP环境搭建
>RabbitMQ是使用Erlang语言编写的，所以需要先配置Erlang

在安装 RabbitMQ 之前，要**安装受支持的 Erlang/OTP 版本**。标准centos，Fedora，CentOS存储库提供的Erlang版本通常已过时，不能用于运行最新的RabbitMQ版本。

### ERLang的安装~

 **在 CentOS 上安装 Erlang**

 

 - 1,PackageCloud. Yum Repository安装


 >导入密钥~检查erlang的安装版本是否安全

```bash
## 主 RabbitMQ 签名密钥 
 rpm --import https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc 
 ##  Erlang 存储库 
 rpm --import https://packagecloud.io/rabbitmq/erlang/gpgkey 
 ## RabbitMQ 服务器存储库 
 rpm --import https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey 
```

 - 2,为 RabbitMQ 和 Modern Erlang 添加 Yum 存储库

>为了使用 Yum 存储库，必须有一个 .repo 文件,在 /etc/yum.repos.d/ 目录下先建一个 rabbitmq.repo 文件,文件内容如下--


```xml
[rabbitmq_erlang]
name=rabbitmq_erlang
baseurl=https://packagecloud.io/rabbitmq/erlang/el/8/$basearch
repo_gpgcheck=1
gpgcheck=1
enabled=1
# PackageCloud's repository key and RabbitMQ package signing key
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
       https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

[rabbitmq_erlang-source]
name=rabbitmq_erlang-source
baseurl=https://packagecloud.io/rabbitmq/erlang/el/8/SRPMS
repo_gpgcheck=1
gpgcheck=0
enabled=1
# PackageCloud's repository key and RabbitMQ package signing key
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
       https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

##
## RabbitMQ server
##

[rabbitmq_server]
name=rabbitmq_server
baseurl=https://packagecloud.io/rabbitmq/rabbitmq-server/el/8/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
# PackageCloud's repository key and RabbitMQ package signing key
gpgkey=https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey
       https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

[rabbitmq_server-source]
name=rabbitmq_server-source
baseurl=https://packagecloud.io/rabbitmq/rabbitmq-server/el/8/SRPMS
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

```

>它将从 PackageCloud 安装 RabbitMQ 及其 Erlang 依赖项

 - 4,更新yum包元数据~

```bash
yun update && yum upgrade`
yum -q makecache -y --disablerepo= '*' --enablerepo= 'rabbitmq_erlang' --enablerepo= 'rabbitmq_server' 
```

 - 5,安装依赖项

```bash
## 从标准操作系统存储库安装这些依赖项 
 yum install socat logrotate -y 
```

 - 6,安装 Erlang/OPT 和 RabbitMQ

```bash
## 安装 RabbitMQ 和零依赖 Erlang， 
## 忽略标准存储库提供的任何版本 ,因为比较旧
 yum install --repo rabbitmq_erlang --repo rabbitmq_server erlang rabbitmq-server -y 
```

**也可以使用Cloudsmith Yum Repository安装**

>Cloudsmith 跟package方式一样,也提供了一个带有 RabbitMQ 包的 Yum 存储库。

[参考链接](https://www.rabbitmq.com/install-rpm.html#cloudsmith)

检查erlang安装是否成功

```bash
[zzy@Gavin local]$ erl -version
Erlang (SMP,ASYNC_THREADS) (BEAM) emulator version 12.2.1
```

7,安装rabbitMQ服务器

[下载rabbitMQ~](https://www.rabbitmq.com/install-generic-unix.html#downloads)
上传到linux服务器  /usr/local 下

解压后重命名为rabbitmq(重命名非必要操作)
 sbin 目录包含服务器和 CLI 工具 脚本。

 - 8,启动RabbitMq

```bash
./sbin/rabbitmq-server
```
>不同于其他一些安装方法,rabbitmq 通用unix二进制构建 运行不需要 管理员权限

启动之后的提示-->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2c6dc9aa9ad4d1ab4ce1938ba97a75c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**分离模式启动rabbitmq**

要以“分离”模式启动服务器，请使用 rabbitmq-server-detached 。 这将运行 后台的节点进程。 

 - 9,停止rabbitmq服务

```bash
sbin/rabbitmqctl shutdown
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2115e4da0d5d472e803a7a6c7222304d.png)


>该命令 将等待节点进程停止。 如果目标节点没有运行， 它将退出并出现错误。 




 - 10,配置服务器

RabbitMQ 配置文件 位于 $RABBITMQ_HOME/etc/rabbitmq/rabbitmq.conf 是配置节点的主要方式。

可以 使用环境变量 来控制某些设置。 推荐的方法是使用 $RABBITMQ_HOME/etc/rabbitmq/rabbitmq-env.conf 文件。

安装后这些文件都不存在，因此必须先创建它们。 
配置文件主要有---->>

**rabbitmq.conf ---主配置文件 。 应该用于大多数设置。**

**advanced.config---无法表达的有限数量的设置 在新样式的配置格式中，例如 LDAP 查询 。 仅应在必要时使用。**

**rabbitmq-env.conf  （ 在win下为 rabbitmq-env.conf.bat ）
用于 环境变量 在一处设置与 RabbitMQ 相关的**


配置示例--->>rabbitmq.conf 


```conf
ssl_options.cacertfile  = /path/to/ca_certificate.pem
ssl_options.certfile = /path/to/server_certificate.pem
ssl_options.keyfile   = /path/to/server_key.pem
ssl_options.verify    = verify_peer
ssl_options.fail_if_no_peer_cert = true
listeners.tcp.default = 5673  #将改变 RabbitMQ 监听 的 AMQP 0-9-1 和 AMQP 1.0 客户端连接从 5672 到 5673。 
```

配置示例--->>advanced.config
```config
[
  {rabbit, [{ssl_options, [{cacertfile,           "/path/to/ca_certificate.pem"},
                           {certfile,             "/path/to/server_certificate.pem"},
                           {keyfile,              "/path/to/server_key.pem"},
                           {verify,               verify_peer},
                           {fail_if_no_peer_cert, true}]}]}
].
```

 - 11,配置文件位置

![在这里插入图片描述](https://img-blog.csdnimg.cn/f7abde803b0a403ba3ea65d4c9649978.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

环境变量可用于覆盖配置文件的位置：

```
 覆盖主配置文件位置 
RABBITMQ_CONFIG_FILE =/path/to/a/custom/location/rabbitmq.conf 

 # 覆盖高级配置文件位置 
RABBITMQ_ADVANCED_CONFIG_FILE =/path/to/a/custom/location/advanced.config 

 # 覆盖环境变量文件位置 
RABBITMQ_CONF_ENV_FILE =/path/to/a/custom/location/rabbitmq-env.conf 
 
```

rabbitmq.conf 和 advanced.config 更改将在节点重启后生效。

>如果 rabbitmq-env.conf 不存在，可以手动创建 在 RABBITMQ_CONF_ENV_FILE 变量指定的位置。 



### 插件管理
进入rabbitmq安装目录下的sbin目录

>cd $rabbitmqhome/rabbitmq/sbin

**查看插件~**

```bash
./rabbitmq-plugins list
#插件列表
Listing plugins with pattern ".*" ...
 Configured: E = explicitly enabled; e = implicitly enabled
 | Status: [failed to contact rabbit@localhost - status not shown]
 |/
[  ] rabbitmq_amqp1_0                  3.9.13
[  ] rabbitmq_auth_backend_cache       3.9.13
[  ] rabbitmq_auth_backend_http        3.9.13
[  ] rabbitmq_auth_backend_ldap        3.9.13
......................略..............................
[  ] rabbitmq_web_dispatch             3.9.13
[  ] rabbitmq_web_mqtt                 3.9.13
[  ] rabbitmq_web_mqtt_examples        3.9.13
[  ] rabbitmq_web_stomp                3.9.13
[  ] rabbitmq_web_stomp_examples       3.9.13

```

使得管理插件生效

```bash
 ./rabbitmq-plugins enable rabbitmq_management
 
[zzy@localhost sbin]$ sudo  ./rabbitmq-plugins enable rabbitmq_management
[sudo] password for zzy: 
Enabling plugins on node rabbit@localhost:
rabbitmq_management
The following plugins have been configured:
  rabbitmq_management
  rabbitmq_management_agent
  rabbitmq_web_dispatch
Applying plugin configuration to rabbit@localhost...
The following plugins have been enabled:
  rabbitmq_management
  rabbitmq_management_agent
  rabbitmq_web_dispatch
started 3 plugins.
```

 - **rabbitmq后台运行**
>./rabbitmq-server -detached

>[zzy@localhost sbin]$ ps aux |grep rabbitmq

![在这里插入图片描述](https://img-blog.csdnimg.cn/51b99d93e1fb4873a7d11bb195c398fc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

查看Web管理界面
首先开启端口 可访问
>sudo firewall-cmd --zone=public --add-port=15672/tcp --permanent

访问  ip+端口号(15672),注意是在虚拟机中访问;

如果是外部访问,则会失败

![在这里插入图片描述](https://img-blog.csdnimg.cn/1eb2b5b2d7a94ae296726ff8d857785b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

默人用户名和密码都是guest    


![在这里插入图片描述](https://img-blog.csdnimg.cn/b05c40d5826a452e8524f6192679f488.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>此时虚拟机外部还无法访问控制台

停止服务~
 >./rabbitmqctl stop_app
 >![在这里插入图片描述](https://img-blog.csdnimg.cn/f6ba97d0168049419b5026b53b207252.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
### 知识小结

回顾~
几个重要的步骤~
1,设置 密钥
2,安装依赖
3,先配置erlang~通过yum存储库安装~参考官网安装步骤

安装成功检验~~~ `erl -version`
3,安装rabbitmq


常用的命令~
围绕着rabbitmq安装文件夹下的sbin目录下的命令进行操作(这里所处位置为rabbitmqhome )

 `rabbitmq-server`    `rabbitmqctl`    `rabbitmq-plugins`

启动服务~ `sbin/rabbitmq-server`

关闭服务~ `sbin/rabbitmqctl  shutdown`

后台运行~ `sbin/rabbitmq-server   -detached`

查看插件~ `sbin/ rabbitmq-plugins  list`

启动插件~ `sbin/rabbitmq-plugins enable 插件名`

验证服务是否开启~`ps aux |grep rabbitmq`

登录web管理界面~`localhost:15672`

关闭插件~`sbin/rabbitmqctl stop_app`

## RabbitMq账户管理
一开始默认用户为guest,
添加用户~

首先启动rabbitmq

```bash
[zzy@Gavin sbin]$ sudo ./rabbitmqctl add_user zzy 1234
Adding user "zzy" ...
Done. Don't forget to grant the user permissions to some virtual hosts! See 'rabbitmqctl help set_permissions' to learn more.
[zzy@Gavin sbin]$
```
提醒我们要授权给新建的用户


新建用户标签~adminintrator
```bash
[zzy@Gavin sbin]$ sudo ./rabbitmqctl  set_user_tags zzy administrator
Setting tags for user "zzy" to [administrator] ...
[zzy@Gavin sbin]$
```

后授权

授权格式~

```bash
rabbitmqctl [--node <node>] [--longnames] [--quiet] set_permissions [--vhost <vhost>] <username> <conf> <write> <read>
```
执行结果~
```bash
[zzy@Gavin sbin]$ sudo ./rabbitmqctl  set_permissions zzy ".*" ".*" ".*"
#注意~“--vhost” 表示虚拟机 可选,默认是 "/"即本地虚拟机
#"." 表示所有配置文件,读与写权限都有

Setting permissions for user "zzy" in vhost "/" ...
[zzy@Gavin sbin]$
```


这时我们就可以通过管理员来使用外部访问rabbitmq管理界面了


>无法访问?
>需要开放端口15672,之后重启防火墙;

只有使用localhost访问rabbitmq时guest账户可用;
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed6e2de6a3234ee7b9ff04c84545e4d9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


外部访问~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/bd5e19af8e3845eeac5ca3ef2ece6d65.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
‎要应用于用户的以逗号分隔的标记列表。‎‎管理插件目前支持‎‎：‎

```
management
User can access the management plugin

policymaker
User can access the management plugin and manage policies and parameters for the vhosts they have access to.

monitoring
User can access the management plugin and see all connections and channels as well as node-related information.

administrator
User can do everything monitoring can do, manage users, vhosts and permissions, close other user's connections, and manage policies and parameters for all vhosts.
```

## RabbitMq的交换器（交换机）

交换器负责接收客户端传递过来的消息，并转发到对应的队列中。在RabbitMQ中支持四种交换器

![在这里插入图片描述](https://img-blog.csdnimg.cn/4892a97dfa714c199575e350f2c7ac6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### Direct Exchange
**Direct Exchange：直连交换器（默认）**
>默认会公平调度,所有接收者会依次从消息队列中获取值;
>信息发布者给那个队列发送消息,就会给该队列发送消息,而对该交换器绑定的其他队列没影响;

代码演示---->>
导入依赖~

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
            <version>2.6.4</version>
        </dependency>
```
配置文件~

```yml
spring:
  rabbitmq:
    username: zzy
    password: 1234
    host: 172.24.232.166
```
java代码~


配置队列~
>如果已经存在队列,可以不用配置类,也可以在rabbitmq管理界面提前配置好队列

```java
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
@Configuration
public class RabbitConfig {
    @Bean
    protected Queue queue(){

        Queue queue= new Queue("zzy");
        return  queue;
    }
}

```
发布消息
```java
import com.gaivn.ApplicationRabbitMq;
import org.junit.jupiter.api.Test;
import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

//作为信息的发布者
@SpringBootTest(classes = ApplicationRabbitMq.class)
public class RabbitTest {
    @Autowired
    private AmqpTemplate amqpTemplate;

    @Test
    void RabbitTest(){
        amqpTemplate.convertAndSend("zzy","hello world");
        System.out.println("success");
    }
}

```

在管理界面我们会看到Ready由0变为了1,
![在这里插入图片描述](https://img-blog.csdnimg.cn/617b7a49ac7744c1b4e90de1c0dd7e87.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

接下来我们取出这个数据

新建一个消费者模块

配置文件yml跟publisher一样,这里略过

```java
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;
@Component  //加注解,不然无法解析
public class RabbitMqConsumer {

    @RabbitListener(queues = "zzy")//订阅的队列
    public void Listened1(String msg){

        System.out.println("取出的消息1----"+msg);
    }
    @RabbitListener(queues = "zzy")
    public void Listened2(String msg){

        System.out.println("取出的消息2----"+msg);
    }
}

```


启动类~略
>启动消费者后会进程会一直与运行实施监听

![在这里插入图片描述](https://img-blog.csdnimg.cn/ffc3c47d957d4f85bffbb760f6d032d8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/f7f27584fbf94d5c8b492ecfef0b3fbe.png)
取出数据后 Ready 由1变为0,说明数据被取走了
![在这里插入图片描述](https://img-blog.csdnimg.cn/6e5372a98b6a473cbabf76d31fd69987.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


执行多次publisher后~结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/03b7cdf1921040c584c9c906d77f86b0.png)
>direct会进行公平调度。所有接受者依次从消息队列中获取值;

### Fanout Exchange

Fanout Exchange：扇形交换器
 >实际就是广播,通知队列中 第一个,队列中的其他成员收不到通知,依此类推,取走后下一个成员就成了第一个;



在上述代码的基础上做一些修改,
①,修改交换器为**Fanout Exchange;**
需要在配置类中配置一下
②,增加队列数量 为2个
③,增加消费者的数量 2个,每个绑定为6个

启动消费者之后,发布多次信息看执行结果~
配置类~

```java
import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.FanoutExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RabbitFanoutConfig {

    //准备两个队列

    @Bean
    protected Queue queue() {

        return new Queue("gavin");
    }


    @Bean
    protected Queue queue2() {

        return new Queue("zzy");
    }

//内置fanout交换器名称amq.fanout

    @Bean
    protected FanoutExchange fanoutExchange() {

        return new FanoutExchange("amq.fanout");
    }

 //将交换器和队列绑定

    @Bean
    protected Binding fanoutBinding(Queue queue, FanoutExchange fanoutExchange) {

        return BindingBuilder.bind(queue).to(fanoutExchange);
    }

    @Bean
    protected Binding fanoutBinding2(Queue queue2, FanoutExchange fanoutExchange) {

        return BindingBuilder.bind(queue2).to(fanoutExchange);
    }
}

```
>一个交换器需要绑定多个队列

消费者1

```java
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

@Component
public class RabbitMqConsumer {


    @RabbitListener(queues = "gavin")
    public void Listened4(String msg){

        System.out.println("gavin取出的消息1----"+msg);
    }
    @RabbitListener(queues = "gavin")
    public void Listened5(String msg){

        System.out.println("gavin取出的消息2----"+msg);
    }
    @RabbitListener(queues = "gavin")
    public void Listened6(String msg){

        System.out.println("gavin取出的消息3----"+msg);
    }
```
消费者2
```java
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

@Component
public class RabbitMqConsumer {

    @RabbitListener(queues = "zzy")
    public void Listened1(String msg){

        System.out.println("zzy取出的消息1----"+msg);
    }
    @RabbitListener(queues = "zzy")
    public void Listened2(String msg){

        System.out.println("zzy取出的消息2----"+msg);
    }
    @RabbitListener(queues = "zzy")
    public void Listened3(String msg){

        System.out.println("zzy取出的消息3----"+msg);
    }

}
```

管理界面,
![在这里插入图片描述](https://img-blog.csdnimg.cn/a97a4f5aed8344d789d6d34900fcee0a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

发布者~

```java
   @Test
    void fanoutTest2(){
        amqpTemplate.convertAndSend("amq.fanout","路由键暂时用不到","hello world");

        System.out.println("success");
    }
```

>每取走一次,则下一个就会成为第一个

![在这里插入图片描述](https://img-blog.csdnimg.cn/67980116542643ec87a7d3e09686abd3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - 3.Topic Exchange：主题交换器
  允许在路由键（RoutingKey）中出现**匹配规则。**

  **路由键的写法和包写法相同** com.msb.xxxx.xxx格式。

```
在绑定时可以带有下面特殊符号，中间可以出现:

* : 代表一个单词（两个.之间内容）

# : 0个或多个字符
```

>接收方依然是公平调度，同一个队列中内容轮换获取值。

代码实现---->>

配置类~

```java
import org.springframework.amqp.core.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RabbitFanoutConfig {

    //准备两个队列

    @Bean
    protected Queue queue() {

        return new Queue("gavin");
    }


    @Bean
    protected Queue queue2() {

        return new Queue("zzy");
    }

//内置topic交换器名称amq.topic

    @Bean
    protected TopicExchange topicExchange() {

        return new TopicExchange("amq.topic");
    }

 //将交换器和队列绑定

    @Bean
    protected Binding TopicBind1(Queue queue, TopicExchange topicExchange) {

        return BindingBuilder.bind(queue).to(topicExchange).with("com.gavin.*");//匹配路由规则
    }

    @Bean
    protected Binding TopicBind2(Queue queue2, TopicExchange topicExchange) {

        return BindingBuilder.bind(queue2).to(topicExchange).with("com.gavin.M#");
    }
}

```
逻辑跟direct方式类似,不过后面追加了条件----路由键,该路由键会匹配配置类中的路由规则,如果不匹配,则不会将消息发送给该消费者;
消费者沿用FanoutExchange中的代码;


~更改路由规则尝试一下在不同的路由规则下各个消费者能否接收到消息~

### Header Exchange

 **Header Exchange：首部交换器。**



默认交换器的名称~
![在这里插入图片描述](https://img-blog.csdnimg.cn/d2aa1acf1d1640d487cc2024b56e964f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)