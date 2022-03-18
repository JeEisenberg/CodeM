![1645023143463](..\pic\div\zookeeper.jpg)

# ZooKeeper

## 特点

Apache ZooKeeper致力于开发和维护开源服务器，以实现高度可靠的分布式协调。

 - ZooKeeper是一项集中式服务，用于维护配置信息、命名、提供分布式同步和提供组服务。所有这些类型的服务都以某种形式由分布式应用程序使用;
 - ZooKeeper 的实施非常重视高性能、高可用性、严格有序的访问。ZooKeeper 的性能方面意味着它可以在大型分布式系统中使用。
 - ZooKeeper 允许分布式进程通过共享的分层命名空间相互协调,旨在通过一组称为集合体的主机进行复制。

![在这里插入图片描述](https://img-blog.csdnimg.cn/9ea8b62dd5fb4321920454dbb975e06d.png)
>组成 ZooKeeper 服务的服务器必须相互了解。它们在持久性存储中维护状态的内存映像，以及事务日志和快照。只要大多数服务器可用，ZooKeeper 服务就可用。

## 运行逻辑

客户端连接到单个 ZooKeeper 服务器。客户端维护 TCP 连接，通过该连接发送请求、获取响应、获取监视事件并发送心跳。如果与服务器的 TCP 连接中断，客户端将连接到其他服务器。

## 数据模型和分层命名空间

ZooKeeper 提供的命名空间与标准文件系统的命名空间非常相似。名称是由斜杠 （/） 分隔的路径元素序列。ZooKeeper 命名空间中的每个节点都由一个路径标识。

ZooKeeper 的分层命名空间
![在这里插入图片描述](https://img-blog.csdnimg.cn/c46e5a710e6c48f1b13db3178107ba84.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_12,color_FFFFFF,t_70,g_se,x_16)
这一点跟文件目录很像,ZooKeeper 命名空间中的每个节点都可以具有与其关联的数据以及子节点。

每当客户端数据被修改了,Zookeeper会及发现,因为**Znodes 维护一个统计结构**，其中包括**数据更改、ACL 更改和时间戳的版本号**，以允许**缓存验证和协调更新**。每次 znode 的数据发生更改时，**版本号都会增加**。每当客户端检索数据时,客户端都会收到数据的版本;就像浏览器缓存机制一样;

## 数据的读取与写入

存储在命名空间中每个 znode 上的数据以原子方式读取和写入。读取获取与 znode 关联的所有数据字节，写入将替换所有数据。每个节点都有一个访问控制列表 （ACL），用于限制谁可以执行哪些操作。

ZooKeeper也有**临时节点**的概念。只要创建 znode 的会话处于**活动状态**，这些 znode 就**一直存在**。会**话结束时**，将**删除 znode**。

## 条件更新与监视
客户端可以在 znode 上设置监视。当 znode 更改时，将触发并移除监视。当触发监视时，客户端会收到一个数据包，指出 znode 已更改。如果客户端与其中一个 ZooKeeper 服务器之间的连接断开，客户端将收到本地通知。
>3.6.0 中的新增功能：客户端还可以在 znode 上设置永久的递归监视，这些监视在触发时不会被删除，并且会触发已注册的 znode 以及任何子 znode 的更改递归。

**Zookeeper是如何完成复杂的服务?**

 - **顺序一致性** - 来自客户端的**更新**将按其**发送顺序应用**。 
 - **原子性** - 要么更新成功要么更新失败。无部分更新结果。
 - **单个系统映像** -客户端将看到相同的服务视图，而不管它连接到哪个服务器。即，客户端将永远不会看到系统的旧视图，即使客户端故障转移到具有相同会话的其他服务器。
 - **可靠性** - 应用更新后，它将从该时间持续存在，直到客户端覆盖更新。
 - **及时性** - 保证系统的客户端视图在**一定时间范围内是最新的**。

## zookeeper支持的操作
创建 ：在树中的某个位置创建一个节点

删除 ：删除节点

存在 ：测试某个位置是否存在节点

获取数据：从节点读取数据

设置数据：将数据写入节点

获取子节点 ：检索节点的子节点列表

同步 ：等待数据传播

## ZooKeeper Components

 ZooKeeper 服务的高级组件。除请求处理器外，构成 ZooKeeper 服务的**每个服务器都会复制**其每个**组件的副本**;
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/9672dba58f2141438f4834b9c289ee89.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_17,color_FFFFFF,t_70,g_se,x_16)
复制的数据库是**包含整个数据树的内存中数据库**。更新将记录到磁盘以实现可恢复性，并且在将写入应用于内存中数据库之前，将**序列化到磁盘。**---写入之前序列化到自盘

每个 ZooKeeper 服务器都为客户端提供服务。**客户端**只**连接到一台服务器以提交请求**。**读取请求**由每个服务器数据库的**本地副本提供服务**。**更改服务状态**的请求（写入请求）由**协议协议**处理。

作为协议协议的一部分，来自客户端的所有写入请求都将转发到称为**领导者的单个服务器**。其余的 ZooKeeper 服务器（称为关注者）接收来自领导者的消息建议，并就消息传递达成一致。消息传递层负责在失败时替换领导者，并将追随者与领导者同步。

ZooKeeper 使用自定义原子消息传递协议。由于消息传递层是原子的，因此 ZooKeeper 可以**保证本地副本永远不会发散**。当领导者收到写入请求时，它会计算要应用写入时系统的状态，并将其转换为捕获此新状态的事务。

>ZooKeeper 集合的配置使得领导者不允许来自客户端的连接。

## zookeeper的使用
[官网下载合适的zookeeper版本](https://zookeeper.apache.org/releases.html)

没有win和mac版本,所以要准备一个linux系统.可以是wsl也可以是虚拟机,这里以centos8进行操作;
将下载好的文件解压到 /usr/local 里,
>为什么要解压到这里,因为之前配置的所有开发环境都在这个文件夹里,为了方便管理;

更改文件夹名未 zookeeper(解压后的文件名太长,也可以不修改~个人习惯)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7f7f98cff12145ab963a9a4988ba6995.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

>注意权限问题

之后需要做一下配置----
在zookeeper文件夹下新建一个为文件夹 空文件data用于存放zookeeper运行后的数据

```c
[zzy@localhost zookeeper]$ sudo mkdir data
[zzy@localhost zookeeper]$ 
```

在conf 文件夹下新建配置文件zoo.cfg
配置如下

```c
tickTime=2000#检测信号间隔时间
dataDir=../data#存储内存中数据库快照的位置存储数据库更新的事务日志
clientPort=2181 #侦听客户端连接的端口
```
接下来启动zookeeper

> sudo  bin/zkServer.sh start

 启动失败,提示没有设置java_home
```c
[zzy@localhost local]$ cd zookeeper/
[zzy@localhost zookeeper]$ sudo bin/zkServer.sh start
Error: JAVA_HOME is not set and java could not be found in PATH.
```
检查java_home

>export

```c
[zzy@localhost zookeeper]$ export
declare -x CATALINA_HOME="/usr/local/tomcat"
declare -x DBUS_SESSION_BUS_ADDRESS="unix:path=/run/user/1000/bus"
declare -x HISTCONTROL="ignoredups"
declare -x HISTSIZE="1000"
declare -x HOME="/home/zzy"
declare -x HOSTNAME="localhost.localdomain"
declare -x JAVA_HOME="/usr/local/jdk"
declare -x JRE_HOME="/usr/local/jre"
declare -x LANG="en_US.UTF-8"
declare -x LESSOPEN="||/usr/bin/lesspipe.sh %s"
declare -x LOGNAME="zzy"

```
已经配置了java_home,这时候在zookeeper的bin目录下 vi 打开zkEnv.sh文件

手动配置JAVA_HOME
export JAVA_HOME="/usr/local/jdk
![在这里插入图片描述](https://img-blog.csdnimg.cn/8306b7e93fc44d479bfa7826b234a054.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
再次与运行zookeeper

```c
[zzy@localhost zookeeper]$ sudo bin/zkServer.sh start
[sudo] password for zzy: 
ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED

```

zookeeper使用log4j记录日志,对于长时间运行的生产系统，ZooKeeper 存储必须在外部进行管理（dataDir 和 logs）

**连接到zookeeper**

![在这里插入图片描述](https://img-blog.csdnimg.cn/b270b9ac23854c3d9e687d090bec278a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

这时候已经进入到zookeeper中了

![在这里插入图片描述](https://img-blog.csdnimg.cn/b9c39ea34af94607a97391d5c79aba03.png)

这个是时候我们需要掌握zookeeper中常用的命令了  
**ll与ls**
	ls -s /path

	-s 详细信息，

```c
[zk: 127.0.0.1:2181(CONNECTED) 2] ls -s
ls [-s] [-w] [-R] path
[zk: 127.0.0.1:2181(CONNECTED) 3] 

```

	-R 当前目录和子目录中内容都罗列出来

```c
[zk: 127.0.0.1:2181(CONNECTED) 1] ls -R /
/
/zookTest
/zookeeper
/zookeeper/config
/zookeeper/quota
[zk: 127.0.0.1:2181(CONNECTED) 2] 
```

	例如：ls -R / 显示根目录下所有内容


**create**
	create /path [data]

	[data] 包含内容
	
	创建指定路径信息


​	

```c

  [zk: 127.0.0.1:2181(CONNECTED) 3]
   create /zookTest mytest  #创建节点 并将节点关联mytest
Created /zookTest
[zk: 127.0.0.1:2181(CONNECTED) 4] ls /
[zookTest, zookeeper]
[zk: 127.0.0.1:2181(CONNECTED) 5] 
```

get

查看/验证数据是否与 znode 相关联
	get [-s] /path
```c

[zk: 127.0.0.1:2181(CONNECTED) 0] get /zookTest
mytest
```
	[-s] 详细信息
	
	查看指定路径下内容。

```c
[zk: 127.0.0.1:2181(CONNECTED) 3] get -s  /zookTest
mytest #关联的节点信息---即存放的数据
cZxid = 0x2 创建时zxid(znode每次改变时递增的事务id)
ctime = Mon Feb 14 20:00:52 PST 2022 #时间戳
mZxid = 0x2 #最近一次更新的zxid
mtime = Mon Feb 14 20:00:52 PST 2022 #最近一次更新的时间戳
pZxid = 0x2 #子节点zxid 这说明没有子节点
cversion = 0 #子节点更新次数,操作一次更新一次值
dataVersion = 0#子节点数据更新的次数
aclVersion = 0#节点acl(授权信息)的更新次数
ephemeralOwner = 0x0#如果该节点为ephemeral节点(临时，生命周期与session一样), ephemeralOwner值表示与该节点绑定的session id. 如果该节点不是ephemeral节点, ephemeralOwner值为0.
dataLength = 6 #节点数据字节数
numChildren = 0#子节点数量
[zk: 127.0.0.1:2181(CONNECTED) 4] 
```


**set**
	set /path [data]

	[data] 包含内容
	
	设置关联的数据信息,
```c
 set zookTest junk
Path must start with / character
[zk: 127.0.0.1:2181(CONNECTED) 5] set /zookTest junk
[zk: 127.0.0.1:2181(CONNECTED) 6] get -s  /zookTest
junk
cZxid = 0x2
ctime = Mon Feb 14 20:00:52 PST 2022
mZxid = 0x5
mtime = Mon Feb 14 21:48:50 PST 2022
pZxid = 0x2
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 0
[zk: 127.0.0.1:2181(CONNECTED) 7] 
zk: 127.0.0.1:2181(CONNECTED) 14] set /show 张三   #向节点存放信息
[zk: 127.0.0.1:2181(CONNECTED) 15] get /show  #查看节点信息
张三
[zk: 127.0.0.1:2181(CONNECTED) 16] delete /show
[zk: 127.0.0.1:2181(CONNECTED) 17] ls -s /
[zookeeper]
cZxid = 0x0

```

**delete**
delete /path
删除节点

```c
[zk: 127.0.0.1:2181(CONNECTED) 7] delete /zookTest
[zk: 127.0.0.1:2181(CONNECTED) 10] ls -s /
[zookeeper]
cZxid = 0x0
ctime = Wed Dec 31 16:00:00 PST 1969
mZxid = 0x0
mtime = Wed Dec 31 16:00:00 PST 1969
pZxid = 0x6
cversion = 1
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 1

```

## 向Zookeeper中注册内容
在zookeeper中创建一个节点

```c
create /show
Created /show
[zk: 127.0.0.1:2181(CONNECTED) 13] 
```

在节点中存放信息,然后通过java代码去访问zookeeper中的信息;


引入依赖

```xml
<dependency>
<groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.6.3</version>
</dependency>

```
>创建 ZooKeeper 对象时，还会创建两个线程：IO 线程和事件线程。所有 IO 都发生在 IO 线程上（使用 Java NIO）。

>所有事件回调都发生在事件线程上。

>会话维护（如重新连接到 ZooKeeper 服务器和维护检测信号）在 IO 线程上完成。同步方法的响应也在 IO 线程中处理。对异步方法和监视事件的所有响应都在事件线程上进行处理


**异步调用和观察程序回调**的所有完成都**将按顺序进行**，一次一个。调用方可以执行他们希望的任何处理，但在此期间不会处理任何其他回调。
回调不会阻止 IO 线程的处理或同步调用的处理。
**同步调用可能无法按正确的顺序返回**。如果在异步读取和同步读取之间对 /a 进行了更改，则客户端库将在同步读取的响应之前收到指示 /a 已更改的监视事件，但由于完成回调阻塞了事件队列，则在处理监视事件之前，同步读取将返回新值 /a。


java代码--->>

```java
package com.gavin.zoo;
import org.apache.zookeeper.*;
import org.junit.jupiter.api.Test;

import java.io.IOException;

public class ZookeeperTest {

    @Test
    void zooTest() throws Exception {
//        创建zookeeper
        ZooKeeper zooKeeper = new ZooKeeper("192.168.135.145:2181", 100000, watchedEvent -> System.out.println("获取了链接"));
//        创建节点,发送数据
        String content = zooKeeper.create("/show", "content".getBytes(), ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.EPHEMERAL_SEQUENTIAL);
        System.out.println("content"+content);
    }
}

```
运行后发现--->超时异常
![在这里插入图片描述](https://img-blog.csdnimg.cn/bee6cee3d507442cb2e6fc2c2a0a3a11.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

可能是防火墙阻止了访问该端口,需要在虚拟机中将该端口打开或者直接关闭防火墙

开放端口2181--->>

```c
[zzy@localhost ~]$  sudo firewall-cmd --zone=public --add-port=2181/tcp --permanent
```
或者暂时关闭防火墙

```c
[zzy@localhost ~]$ systemctl stop firewalld#暂时关闭
[zzy@localhost ~]$ systemctl status firewalld#查看防火墙状态
[zzy@localhost ~]$ systemctl disable firewalld#永久关闭


```
之后 尝试运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/80ecfbe401844fe09424f31f5dab9e16.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
在zookeeper中查看该信息

查看信息的时候发现不存在...

```c
[zk: 127.0.0.1:2181(CONNECTED) 7] get /show/gavin0000000005
Node does not exist: 
```

**设置临时节点----EPHEMERAL_SEQUENTIAL
临时节点


**修改为持久化节点--**
PERSISTENT_SEQUENTIAL--
持久节点
```c
[zk: 127.0.0.1:2181(CONNECTED) 6] get /show/gavin0000000006
ZooKeeperDemo
```


接下来是数据的获取

```java
package com.gavin.zoo;


import org.apache.zookeeper.*;
import org.junit.jupiter.api.Test;

import java.io.IOException;
import java.util.List;

public class ZookeeperTest {

    @Test
    void zooTest() throws Exception {
        ZooKeeper zooKeeper = new ZooKeeper("192.168.135.145", 10000, watchedEvent -> System.out.println("获取链接"));
//取数据--返回给定路径的节点的子节点列表。
        List<String> children = zooKeeper.getChildren("/show", false);
        for (String child : children) {
//            返回给定路径下节点的数据和统计信息。 节点路径,是否监视,节点状态
            byte[] result=zooKeeper.getData("/show/"+child,false,null);
            System.out.println(new String(result));
        }
        System.out.println("-----------------");
       children.forEach(System.out::println);

    }
}


```
![在这里插入图片描述](https://img-blog.csdnimg.cn/b86f858fff9847d6b22ecddfce2c3119.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


## RPC框架搭建
**前期代码的冗余性体现---->>>**
![在这里插入图片描述](https://img-blog.csdnimg.cn/e440790790914ba68fe93fcf53fe2cbc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
所以实际开发中是创建一个父项目,然后通过继承父项目来达到代码复用的目的;

创建父项目ParentDemo。
包含3个聚合子项目。

	pojo: service中需要的实体类
	
	service：包含被serviceimpl和consumer依赖的接口。
	
	serviceimpl:provider提供的服务内容
	
	consumer：消费者，调用服务内容。



依赖的引入
**父类**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
<!--父类-->
    <groupId>com.gavin</groupId>
    <artifactId>RpcDemoTar</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>pojo</module>
        <module>service</module>
        <module>provider</module>
        <module>consumer</module>
    </modules>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.apache.zookeeper</groupId>
                <artifactId>zookeeper</artifactId>
                <version>3.6.3</version>
            </dependency>
            <dependency>          <groupId>org.projectlombok</groupId>             <artifactId>lombok</artifactId>               <version>1.18.22</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```
**子类---pojo**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>RpcDemoTar</artifactId>
        <groupId>com.gavin</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>pojo</artifactId>
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.22</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
</project>
```

**子类--service**

子类service依赖于pojo

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>RpcDemoTar</artifactId>
        <groupId>com.gavin</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>service</artifactId>

<dependencies>
<!-- service   依赖于pojo-->
    <dependency>
        <artifactId>pojo</artifactId>
        <groupId>com.gavin</groupId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
</dependencies>
</project>
```
**子类provider**
依赖于service

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>RpcDemoTar</artifactId>
        <groupId>com.gavin</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>provider</artifactId>
    <dependencies>
        <dependency>
            <artifactId>service</artifactId>
            <groupId>com.gavin</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </dependency>
    </dependencies>
</project>
```
**子类--consumer**
依赖于service

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>RpcDemoTar</artifactId>
        <groupId>com.gavin</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <artifactId>consumer</artifactId>
    <dependencies>
        <dependency> <artifactId>service</artifactId>
            <groupId>com.gavin</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</project>
```
整体目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b149608b1a44dc4ab5b2245b69edd9f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

**pojo 代码**
养成一个好习惯---实现序列化接口
```java
package com.gavin.codem;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.io.Serializable;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User implements Serializable {
    private String name;
    private Integer age;
    private static final long serialVersionUID = -6849794470754667710L;

}

```

**service 代码**

```java
package com.gavin.codem;

import java.rmi.Remote;
import java.rmi.RemoteException;
import java.util.List;
/**
 * 继承Remote接口
 */
public interface MyUserService extends Remote {
    /**
     *  接口中的方法必须抛出RemoteException
     * @return 用户集合
     * @throws RemoteException
     */
    public List<User> showUserInfo( List<User> ListUserInfo) throws RemoteException;
}

```
**provider 代码**

MyUserService接口实现类
```java
package com.gavin.codem;

import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;
import java.util.List;
/**
 * 必须继承UnicastRemoteObject 以实现序列化的操作
 */
public class MyUserServiceImpl extends UnicastRemoteObject implements MyUserService {
    public MyUserServiceImpl() throws RemoteException {
    }
    public List<User> showUserInfo(List<User> ListUserInfo) throws RemoteException {
        return ListUserInfo;
    }
}

```
**providerRun**
服务开启与发布

```java
package com.gavin;
import com.gavin.codem.MyUserService;
import com.gavin.codem.MyUserServiceImpl;
import jdk.swing.interop.SwingInterOpUtils;
import org.apache.zookeeper.*;

import java.io.IOException;
import java.net.MalformedURLException;
import java.rmi.AlreadyBoundException;
import java.rmi.Naming;
import java.rmi.RemoteException;
import java.rmi.registry.LocateRegistry;

/**RMI server服务启动
 * @author Gavin
 */
public class ProviderRun {

    public static void main(String[] args) {
//RMI操作
        try {
            MyUserService myUserService= new MyUserServiceImpl();
//            开启监听端口
            LocateRegistry.createRegistry(8089);
//            绑定Url,之后要将该地址发送到zookeeper
            String url="rmi://localhost:8089/showUser";
            Naming.bind(url,myUserService);
            ///服务开启提醒
            System.out.println("服务已启动");

//            创建zookeeper
            ZooKeeper    zooKeeper= new ZooKeeper("192.168.135.145:2181", 1000, new Watcher() {
                @Override
                public void process(WatchedEvent event) {
                    System.out.println("获取链接");
                }
            });
//发布信息
            String result = zooKeeper.create("/userinfo/userinfo", url.getBytes(), ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
            System.out.println("注册成功");
        } catch (RemoteException e) {
            e.printStackTrace();
        } catch (AlreadyBoundException e) {
            e.printStackTrace();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (KeeperException e) {
            e.printStackTrace();
        }
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/3854ea9c0f6f4eab898a4283c6050681.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```c
[zk: 127.0.0.1:2181(CONNECTED) 14] ls -R /
/
/show
/userinfo
/zookeeper
/show/gavin0000000006
/show/gavin0000000007
/userinfo/userinfo
/zookeeper/config
/zookeeper/quota
[zk: 127.0.0.1:2181(CONNECTED) 15] get /userinfo/userinfo
rmi://localhost:8089/showUser
[zk: 127.0.0.1:2181(CONNECTED) 16] 

```
发布之后需要从zookeeper中取出来
**Consumer**

接口类

```java
package com.gavin.service;

import com.gavin.pojo.User;

import java.util.List;

/**
 * @author Gavin
 */
public interface UserService {
    List<User> show() ;
}

```
接口实现类,同时从zookeeper取出数据
```java
package com.gavin.service.impl;

import com.gavin.service.MyUserService;
import com.gavin.pojo.User;
import com.gavin.service.UserService;
import org.apache.zookeeper.KeeperException;
import org.apache.zookeeper.ZooKeeper;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.rmi.Naming;
import java.rmi.NotBoundException;
import java.util.List;

/**
 * @author Gavin
 */
@Service
public class UserServiceImpl implements UserService {

    @Override
    public List<User> show() {
//        创建zookeeper实例
        try {
            ZooKeeper zooKeeper = new ZooKeeper("192.168.135.145:2181", 100000, event -> System.out.println("获得了链接"));
//        获取节点信息地址
            byte[] data = zooKeeper.getData("/show/myUserService", false, null);
//这里返回的是
            System.out.println(new String (data));
//            在zookeeper中查找url,并自动创建代理对象
            MyUserService userService = (MyUserService) Naming.lookup(new String(data));

            List<User> show = userService.showUserInfo();
            return show;
        } catch (IOException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (KeeperException e) {
            e.printStackTrace();
        } catch (NotBoundException e) {
            e.printStackTrace();
        }

return null;
    }
}

```
开启springbootweb

```java
package com.gavin;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumerApplication.class,args);
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/b686b5fe8c804efdbb736c80ff48b1e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
小结---->>RMI的原理~~远程调用

还是以一张图进行分析

![在这里插入图片描述](https://img-blog.csdnimg.cn/b97acd5842b04e449ed1f98720dc643a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



## Watcher 机制

Zookeeper 允许客户端向服务端的某个 Znode 注册一个 Watcher 监听，当服务端的一些指定事件触发了这个 Watcher，服务端会向指定客户端发送一个事件通知来实现分布式的通知功能，然后客户端根据 Watcher 通知状态和事件类型做出业务上的改变。

工作机制：~以下三种情况都会发生watcher,session失效时也会发生watcher

（1）客户端注册 watcher

（2）服务端处理 watcher

（3）客户端回调 watcher



**[ZooKeeper Java 示例](https://zookeeper.apache.org/doc/current/javaExample.html)**

## 运行复制的 ZooKeeper
以独立模式运行 ZooKeeper 便于评估、一些开发和测试。但在生产中，您应该以复制模式运行 ZooKeeper。同一应用程序中的一组复制服务器称为仲裁，在复制模式下，**仲裁中的所有服务器都具有相同配置文件的副本。**


>对于复制模式，至少需要三台服务器，强烈建议您使用奇数台服务器。如果您只有两台服务器，那么您的情况是，如果其中一台出现故障，则没有足够的机器来形成多数仲裁。两台服务器本质上不如一台服务器稳定，因为有两个单点故障;

配置格式--->>

```xml
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=5
syncLimit=2
server.1=zoo1:2889:3889
server.2=zoo2:2887:3887
server.3=zoo3:2888:3888
```

## 其他配置

还有一些其他配置参数可以大大提高性能：

为了降低更新延迟，拥有一个专门的事务日志目录很重要。默认情况下，事务日志与数据快照和myid文件放在同一目录中。dataLogDir 参数指示用于事务日志的不同目录。

# 问题汇总---

## Zookeeper启动失败~

今天在启动zookeeper集群时,出现了一个问题---
Starting zookeeper ... FAILED TO START

![在这里插入图片描述](https://img-blog.csdnimg.cn/0eeb62c67323460587e21d28e38635ac.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
昨天还好好的,今天..
**查找原因~**
由于提示启动失败,一开始想的就是配置文件的问题,然后去查看配置文件,

```bash
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=5
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=2
dataDir=/usr/local/zookeeper/data
dataLogDir=/usr/local/zookeeper/logs
clientPort=2181
minSessionTimeout=16000
maxSessionTimeout=30000
4lw.commands.whitelist=mntr,conf,ruok
server.1=192.168.135.145:2888:3888
server.2=192.168.135.146:2888:3888
server.3=192.168.135.147:2888:3888
autopurge.snapRetainCount=3
autopurge.purgeInterval=1

```
没发现问题
>因为昨天配置完毕正常启动之后也没有改动,因此不是这里的问题;

那就查看日志文件,发现启动时因为jvm内存设置的过高,内存溢出了,
在zookeeper-env文件中将内存改小一些,(我直接把他给删了,简单粗暴~记得备份哦!)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ad4941a21e2a4da7810ac8a32f5524ff.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


网上有很多常见的答案,五花八门,毕竟每个人遇到的问题可能不一样,

>比较搞笑的是~说什么少了jar包的,让你重新下载安装,这我就很诧异了,怎么会少?莫非梦游删了?还是jar包收拾铺盖跑路了?
>
>遇到问题,先看看日志吧!