@[TOC](Redis)

# Redis简介
>Redis 是一个开源（BSD许可）的，**内存中的数据结构存储系统**，它可以用作**数据库、缓存和消息中间件**。 

>它支持多种类型的数据结构~~如 **字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询**。

 >Redis 内置了 复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。 

**Redis产生的原因~~**
目前市场主流都是使用关系型数据库,每次数据操作基本都要进行i/o操作,很费时间,为了提高程序运行效率,Redis~内存中的数据结构存储系统诞生了;

常见的Nosql数据库~
**memcached** ：键值对，内存型数据库，所有数据都在内存中。

**Redis**:和Memcached类似，还具备持久化能力,补偿了memcached这类key/value存储的不足

**HBase**：以列作为存储。~大数据使用较多

**MongoDB**：以Document做存储。~大数据使用较多

## Redis的特点~

Redis是以**Key-Value**形式进行存储的**NoSQL数据库**。

Redis是使用**C语言进行编写**的。

平时操作的数据都在内存中，**效率特高**，读的效率110000/s，写81000/s，所以多把Redis当做**缓存工具**使用。

缺点~
向大部分存在内存里的数据一样,一断电数据就没了,所以持久化到本地也是很有必要的;

## Redis的存储结构

Redis以**solt（槽）作为数据存储单元**，每个槽中可以存储N多个键值对。Redis中**固定具有16384个槽**。理论上可以实现一个槽是一个Redis。每个向Redis存储数据的key都会进行**crc16算法**得出一个值后对16384取余就是这个key存放的solt位置。同时通过Redis Sentinel提供高可用，通过Redis Cluster提供自动分区。



**crc算法~**

CRC-16/MODBUS 算法：
在CRC计算时只用8个数据位，起始位及停止位，如有奇偶校验位也包括奇偶校验位，都不参与CRC计算。
CRC计算方法是：
1、 加载一值为0XFFFF的16位寄存器，此寄存器为CRC寄存器。
2、 把第一个8位二进制数据（即通讯信息帧的第一个字节）与16位的CRC寄存器的相异或，异或的结果仍存放于该CRC寄存器中。
3、 把CRC寄存器的内容右移一位，用0填补最高位，并检测移出位是0还是1。
4、 如果移出位为零，则重复第三步（再次右移一位）；如果移出位为1，CRC寄存器与0XA001进行异或。
5、 重复步骤3和4，直到右移8次，这样整个8位数据全部进行了处理。
6、 重复步骤2和5，进行通讯信息帧下一个字节的处理。
7、 将该通讯信息帧所有字节按上述步骤计算完成后，得到的16位CRC寄存器的高、低字节进行交换
8、 最后得到的CRC寄存器内容即为：CRC校验码。

```c

void ModBusCRC16(ref byte[] cmd, int len)

{    ushort i, j, tmp, CRC16;
    CRC16 = 0xFFFF;             //CRC寄存器初始值
    for (i = 0; i < len; i++)
    {
        CRC16 ^= cmd[i];
        for (j = 0; j < 8; j++)
        {
            tmp = (ushort)(CRC16 & 0x0001);
            CRC16 >>= 1;
            if (tmp == 1)
            {
                CRC16 ^= 0xA001;    //异或多项式
            }
        }
    }
    cmd[i++] = (byte) (CRC16 & 0x00FF);
    cmd[i++] = (byte) ((CRC16 & 0xFF00)>>8);
}
```

## Redis的部署

 - **1,安装依赖**

像nginx一样,Redis也是C语言编写的,所以要安装C的依赖

```c
yum install -y gcc-c++ automake autoconf libtool make tcl 
```

 - **2,[官网下载redis](https://redis.io/download)**

 - **3,上传redis到linux服务器**

>可以使用rz命令或者xftp

 - **4,解压文件**

```c
tar -zxf redis-6.0.6.tar.gz
```

 - **5,编译安装Redis**
  进入redis解压后的目录  执行 ~`make`   进行编译
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/d888ea8ebd42421aa5e15038803e3ce4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
  编译结束后进行安装
>make install PREFIX=/usr/local/redis

![在这里插入图片描述](https://img-blog.csdnimg.cn/2c23f496675f4e3790a8f7feacbf0d4b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
redis安装就结束了;

启动redis
进入~`cd /usr/local/redis/bin` 
启动**redis-service**   ~`./redis-server`
![在这里插入图片描述](https://img-blog.csdnimg.cn/1db8f50fad954d268b1db749e877f2a2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



 - **6,配置redis**
  将配置文件/usr/local/redis-6.0.6/redis.conf 复制到/usr/local/redis/bin/

>cp /usr/local/redis-6.0.6/redis.conf   /usr/local/redis/bin/

修改配置文件

>默认情况下，Redis不会作为守护进程运行,当我们退出redis界面时,redis服务也会退出;

**将daemonize的值由no修改为yes**
在配置文件中有很多内容,如果找不到,可以直接搜索~ `/daemonize` 
![在这里插入图片描述](https://img-blog.csdnimg.cn/6fe48225c13f4833abf6bbd8d7268082.png)


 - **7,修改外部访问**

外部可访问ip,可以限制端口号,这里不做限制;
![在这里插入图片描述](https://img-blog.csdnimg.cn/bf41f3a89cbb4ca3b93ad87548e7dcca.png)

>  ~~~警告~~~如果计算机运行Redis是直接暴露在 internet，绑定到所有的接口是危险的，会暴露   给互联网上的每个人;举个例子。 默认情况下，我们取消注释跟随绑定指令，这将迫使Redis只听进入  IPv4 loopback接口地址(这意味着Redis将能够  只接受运行在同一台计算机上的客户端连接  运行)。   如果确定实例监听所有的接口 注释下面这行。
>  
>  ~~~

设置保护模式为no

> 保护模式是一层安全保护，为了避免这种情况的发生   Redis在互联网上开放的实例被访问和利用  当保护模式开启时，如果:

> 服务器没有明确绑定到一组地址~则使用 绑定指令。  

>没有设置密码 ~服务器只接受来自客户端的连接  

IPv4和IPv6环回地址是127.0.0.1和::1，并且来自Unix域套接字。
> 默认启用保护模式。 只有当你确定你想让其他主机的客户端连接到Redis  
> 即使没有配置认证，也没有一组特定的接口 使用"bind"指令显式列出。

![在这里插入图片描述](https://img-blog.csdnimg.cn/015b609cca2e4c0ba666775cfcce598b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 **8- 启动Redis**

配置结束后再次启动 redis

~加载配置文件启动
>  ./redis-server redis.conf

 

重启redis 服务
> ./redis-cli shutdown   关闭  

>./redis-server redis.conf   启动

启动客户端工具
>./redis-cli 


![在这里插入图片描述](https://img-blog.csdnimg.cn/114e880ab8314fe59ff15014939b3341.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


## Redis中的数据类型
 Redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，zset，hash等数据结构的存储，它还支持数据的备份，即master-slave模式的数据备份，同样Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。

        Redis支持的五大数据类型包括String（字符串  用法： 键  值），Hash（哈希 类似Java中的 map  用法： 键  键值对），List（列表  用法：键 集合 不可以重复），Set（集合 用法：键 集合 可以重复），Zset（sorted set 有序集合    用法： 键  值 值）

 - **String(字符串)**
  redis中的最**基本数据类型**,redis中的string 跟java中的字符串是有区别的,在redis中字符串是**二进制存储的**,可以**包含任何数据**,比如图片,视频或者任何序列化对象;
  string类型的**值最大能存储512MB;**

应用场景：  

>String是最常用的一种数据类型，普通的key/value存储都可以归为此类，value其实不仅是String，也可以是数字：比如想知道什么时候封锁一个IP地址(访问超过几次)。  

 - **Hash(哈希)**
  Redis hash 是一个**键值(key=>value)对集合**。

Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。
  使用场景：
  >存储、读取、修改用户属性  
  > >

 - **List(列表)**
  Redis 列表是简单的**字符串列表**，**按照插入顺序排序**。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

应用场景:
>利用List实现最新消息排行等功能:

> List的另一个应用就是消息队列，可以利用Lists的PUSH操作，将任务存在Lists中，然后工作线程再用POP操作将任务取出进行执行。  

 - **Set（集合）**
  Set是**string类型的无序集合**。

使用场景：
>共同好友、二度好友 


> 利用唯一性，可以统计访问网站的所有独立 IP 

 Redis set对外提供的功能与list类似是一个列表的功能，特殊之处在于**set是可以自动排重**的;
当需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口~~实现方式：  

    	set 的内部实现是一个 value永远为null的HashMap，实际就是通过计算hash的方式来快速排重的，这也是set能提供判断一个成员是否在集合内的原因。 


>除此之外,Redis还为集合提供了求交集、并集、差集等操作; 

 - **zset(sorted set：有序集合)**
  Redis zset 和 set 一样也是**string类型元素的集合**,且**不允许重复**的成员。不同的是每个元素都会**关联一个double类型的分数**。redis正是通过分数来为集合中的成员进行从小到大的排序。zset的成员是唯一的,但分数(score)却可以重复。

使用场景：
>带有权重的元素，比如一个游戏的用户得分排行榜 ;

>比较复杂的数据结构，一般用到的场景不算太多


## Redis常用命令
Redis命令手册~
 1.https://www.redis.net.cn/order/

  2.http://doc.redisfans.com/

>Redis的命令大部分是围绕key进行操作的;

### Key操作
进入redis安装目录,启动redis
先启动服务,然后启动客户端
![在这里插入图片描述](https://img-blog.csdnimg.cn/5c1919ebb70a4d258d2ac8e3ef432507.png)

设置key--value

 - **set key  value  与get key**
  set key   若key相同,则覆盖掉之前的,
  get key 若没有,返回 nli,
```c
127.0.0.1:6379> set name 扎根三
OK
127.0.0.1:6379> get name
"\xe6\x89\x8e\xe6\xa0\xb9\xe4\xb8\x89"
127.0.0.1:6379> set name gavin
OK
127.0.0.1:6379> get name
"gavin"
127.0.0.1:6379> set age 18
OK
127.0.0.1:6379> get age
"18"
127.0.0.1:6379> 

```
>1,set时候字符串可以加""也可以不加,
>2,汉字是以编码的形式存储的

 

 - **exists**

判断key是否存在。

语法：
exists key或者exists [key1 key2 ...... ]

返回值：存在返回数字，不存在返回0
```c
127.0.0.1:6379> exists name age
(integer) 2
127.0.0.1:6379> exists name
(integer) 1
127.0.0.1:6379> exists namea
(integer) 0

```

 

 - **expire**

设置key的过期时间，单位秒

语法：expire key 秒数

返回值：成功返回1，失败返回0
```c
127.0.0.1:6379> exists name
(integer) 1
127.0.0.1:6379> expire name 5
(integer) 1
127.0.0.1:6379> exists name
(integer) 0
```

 - **persist**
  移除 key 的过期时间，key 将持久保持。

```c
127.0.0.1:6379> expire class 60 #设置过期时间60s
(integer) 1
127.0.0.1:6379> ttl class
(integer) 58 #查看过期时间
127.0.0.1:6379> persist class
(integer) 1 # 设置永不过期
127.0.0.1:6379> ttl class
(integer) -1
```

 - **ttl**
  查看key的剩余过期时间
  语法：ttl key
  返回值：返回剩余时间，如果不过期返回-1
```c
127.0.0.1:6379> expire age 60
(integer) 1
127.0.0.1:6379> ttl age
(integer) 54
127.0.0.1:6379> set gender man
OK
127.0.0.1:6379> ttl gender
(integer) -1

```

 - **del**
    根据key删除键值对。
    语法：del key
    返回值：被删除key的数量 ,如果没删除(即没有该key),返回0

```c
127.0.0.1:6379> set name lisi
OK
127.0.0.1:6379> set age 19
OK
127.0.0.1:6379> del name age
(integer) 2
127.0.0.1:6379> 

```

 - **keys**
  查找所有符合给定模式( pattern)的 key 。
  语法  keys pattern
  返回值   array
  empty array ~~没有符合条件的

```c
127.0.0.1:6379> set name zha
OK

127.0.0.1:6379> set name2 dan
OK

127.0.0.1:6379> keys n
(empty array)
127.0.0.1:6379> keys n*
1) "name2"
2) "name"
127.0.0.1:6379> 
```

 - **rename**
  给 key重命名,
  语法  rename key newkey
  返回值   array
>可以随便你改名,如果重名了,则会覆盖掉之前的键值对
```c
127.0.0.1:6379> set name2 dan
OK
127.0.0.1:6379> rename name2 username 
OK
127.0.0.1:6379> get username
"dan"
127.0.0.1:6379> get name2
(nil)

```

 - **renamenx**

仅当newkey不存在时,才将key命名为 newkey
语法 renamenx key newkey

返回值 0修改失败,1修改成功

```c
127.0.0.1:6379> set 1 1
OK
127.0.0.1:6379> set 2 2
OK
127.0.0.1:6379> renamenx 2 1
(integer) 0
127.0.0.1:6379> 
```

 - **dump**
  序列化给定 key ，并返回被序列化的值。
```c
127.0.0.1:6379> dump 1
"\x00\xc0\x01\t\x00\xf6\x8a\xb6z\x85\x87rM"
127.0.0.1:6379> dump 2
"\x00\xc0\x02\t\x00_P\xe1p\xacR\x1dz"
127.0.0.1:6379> 
```
 - **randomkey**
  从当前数据库中随机返回一个 key 。

```c
127.0.0.1:6379> randomkey
"class"
127.0.0.1:6379> randomkey
"2"
```

### 字符串操作(String)
set get 略

 - **setnx**
  当且仅当key不存在时才新增。

   语法：setnx key value

   返回值：不存在时返回1，存在返回0
```c
127.0.0.1:6379> setnx class 18
(integer) 0
127.0.0.1:6379> setnx grade 89
(integer) 0
127.0.0.1:6379> setnx math  89
(integer) 1
```

 - **setex**
  设置key的存活时间，无论是否存在指定key都能新增，如果存在key覆盖旧值。同时必须指定过期时间。
>将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。
```c
127.0.0.1:6379> setex math 10 100
OK
127.0.0.1:6379>  get math
(nil)

```

 - **mset**

同时设置一个或多个 key-value 对。
语法格式 mset key value key value....
```c
127.0.0.1:6379> mset name lisi age 18 gender 29 graduate qdu 
OK

```

 - **mget**
  获取所有(一个或多个)给定 key 的值。
```c
127.0.0.1:6379> mget name age gender
1) "lisi"
2) "100009999999"
3) "29"
127.0.0.1:6379> 

```

 - **getrange**
    返回 key 中字符串值的子字符
     语法格式 getrange  key start end
>就是字符串的截取
```c
127.0.0.1:6379> getrange age 0 1
"18"
127.0.0.1:6379> getrange age -1 0
""
127.0.0.1:6379> getrange age 2 99
""

```
**value值的加减**

```c
127.0.0.1:6379> decr age
(integer) 17
127.0.0.1:6379> get age
"17"
127.0.0.1:6379> incrby age 10
(integer) 27
127.0.0.1:6379> decrby age 20
(integer) 7
127.0.0.1:6379> incrbyfloat age 10.0
"17"
127.0.0.1:6379> incr age 10
(integer) "18"
```

 - **setrange**
  用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。
  字符串值的替换,
  返回字符串的长度;
```c
127.0.0.1:6379> setrange age 5 8888
(integer) 9
127.0.0.1:6379> get age
"100008888"
127.0.0.1:6379> setrange age 5 9
(integer) 9
127.0.0.1:6379> get age
"100009888"
127.0.0.1:6379> setrange age 5 9999999
(integer) 12
127.0.0.1:6379> get age
"100009999999"
```
 - **append**

如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾。
返回字符串长度
```c
127.0.0.1:6379> append math 100
(integer) 7
127.0.0.1:6379> get math
"1000100"

```

### Hash表操作
Hash类型的值中包含多组field value。
![在这里插入图片描述](https://img-blog.csdnimg.cn/ecbc5d1276f146149bee70b3b9a641b0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **hset**
  将哈希表 key 中的字段 field 的值设为 value 
  类似于 像类中的属性设置值;
```c
127.0.0.1:6379> hset people name wangwu age 12
(integer) 2
127.0.0.1:6379> hget people name
"wangwu"

```

 - **hmset**

同时将多个 field-value (域-值)对设置到哈希表 key 中。

```c
127.0.0.1:6379> hmset fruit name apple color red 
OK

```

 - **hmget**

获取所有给定字段的值

```c
127.0.0.1:6379> hmget fruit name color
1) "apple"
2) "red"

```

 - **hgetall**
  获取在哈希表中指定 key 的所有字段和值
```c
127.0.0.1:6379> hgetall fruit
1) "name"
2) "apple"
3) "color"
4) "red"
```
 - **其他操作**

```c
127.0.0.1:6379> hlen fruit #获取哈希表中字段数量
(integer) 2
127.0.0.1:6379> hdel people age name
(integer) 2  #删除一个或多个hash表中字段
127.0.0.1:6379> hvals fruit#获取
1) "apple"
2) "red"
127.0.0.1:6379> hkeys fruit #获取hash表中的字段
1) "name"
2) "color"
127.0.0.1:6379> hsetnx fruit weight 500g
(integer) 1

```

### List(列表)操作
>list下标从0 开始
 - **lpush**
  将一个或多个值插入到列表头部
```c
127.0.0.1:6379> lpush list1 1 2 3 4
(integer) 4#返回list长度
127.0.0.1:6379> lpush flower zhizi rose
(integer) 2
```
 - **rpush**
  在列表中添加一个或多个值~尾部

```c
127.0.0.1:6379> rpush list1 7 8 9 10
(integer) 8#返回list长度
```

 - **llen**
  获取列表长度
```c
127.0.0.1:6379> llen list1
(integer) 7
```

 - **lrange**
  获取列表指定范围内的元素
```c
127.0.0.1:6379> lrange list1 5 10
1) "8"
2) "9"
3) "10"

```

 - **lrem**
  语法: lrem key count element
  删除列表中元素。count为正数表示从左往右删除的数量。负数从右往左删除的数量。
  返回值:删除的元素数量;
```c
127.0.0.1:6379> lrem list1 -2 2
(integer) 1

```

 - **ltrim**
  对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。


```c
127.0.0.1:6379> ltrim list1 1 6
OK #此处是下标的范围
```

 - **rpoplpush**

移除列表的最后一个元素，并将该元素添加到另一个列表并返回
```c
127.0.0.1:6379> rpoplpush list1 flower
"10"
```

 - **blpop**与**brpop**

**blpop** 
>移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。

**brpop**

>移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。
```c
127.0.0.1:6379> blpop list1  6 10
1) "list1"
2) "3"
127.0.0.1:6379> brpop list1 10
1) "list1"
2) "9"

```
### set(集合)操作

 - **sadd**
  像集合中添加内容,不允许重复

```c
127.0.0.1:6379> sadd  foods bread fish 
(integer) 2
```

 - **scard**
  获取集合的成员数
```c
127.0.0.1:6379> scard foods
(integer) 2
```
 - **sunion**
  返回所有给定集合的并集
```c
127.0.0.1:6379> sunion man person foods
1) "fish"
2) "man"
3) "child"
4) "woman"
5) "bread"
127.0.0.1:6379> 

```

 - **srandmember**

 返回集合中一个或多个随机元素
 语法:srandmemeber key count

```c
127.0.0.1:6379> srandmember man 2
1) "man"
2) "child"
127.0.0.1:6379> 
```
 - **smembers**
  返回集合中所有成员
```c
127.0.0.1:6379> smembers  man
1) "child"
2) "man"
3) "woman"

```

 - **sinter**
  返回给定集合的交集
```c
127.0.0.1:6379> sinter man person foods
(empty array)
127.0.0.1:6379> sinter person man
1) "man"
2) "woman"
```
 - **srem**
  移除集合中的一个或多个元素
```c
127.0.0.1:6379> srem man child
(integer) 1
127.0.0.1:6379> srem man man woman
(integer) 2 

```

 - **smove**
  将 member 元素从 source 集合移动到 destination 集合
  如果目标中已存在该元素则操作失败;
```c
127.0.0.1:6379> sadd man man woman
(integer) 2
127.0.0.1:6379> smove foods man woman
(integer) 0
127.0.0.1:6379> smove foods man fish
(integer) 1
```
 - **sdiff**
  返回集合差集
```c
127.0.0.1:6379> sdiff foods man
1) "bread"
127.0.0.1:6379> smembers foods
1) "bread"
127.0.0.1:6379> smembers  man
1) "fish"
2) "man"
3) "woman"
127.0.0.1:6379> 

```
 - **sismember**
  判断 member 元素是否是集合 key 的成员
```c
127.0.0.1:6379> sismember foods bread
(integer) 1
127.0.0.1:6379> 

```

 - **sdiffstore**
  返回给定所有集合的差集并存储在 destination 中

```c
127.0.0.1:6379> smembers lin
1) "lin"
2) "linlinlin"
3) "linlin"
127.0.0.1:6379> sadd yu lin linli l add
(integer) 4
127.0.0.1:6379> sdiffstore lin lin yu
(integer) 2
127.0.0.1:6379> smembers lin 
1) "linlinlin"
2) "linlin"
127.0.0.1:6379> 

```
同理由sinterstore和sunionstore

 - **sscan**
  迭代集合中的元素
  语法格式: sscan  key  
```c
127.0.0.1:6379> sscan lin 0 match l*
1) "0"
2) 1) "linlin"
   2) "linlinlin"
127.0.0.1:6379> sscan lin 1 match l*
1) "0"
2) 1) "linlinlin"

```

 - **spop**

移除并返回集合中的一个随机元素

```c
127.0.0.1:6379> spop lin 1
1) "linlinlin"

```

### 有序集合（Sorted Set）

 - **zadd**

向有序集合添加一个或多个成员(如果有该集合则添加,没有则创建)，或者更新已存在成员的分数
语法：zadd key score value score value
返回值：长度

```c
127.0.0.1:6379> zadd book 2 lisi 3 wangwu
(integer) 2
127.0.0.1:6379> zadd book 100 zhaosi 200 zhaoliu 300 lisi
(integer) 2 #已经存在lisi了
#查看元素

```

 - **zrange**
  返回区间内容，withscores表示带有分数
  zrange key 区间 [withscores]
  返回值:值列表
```c
127.0.0.1:6379> zrange book 0 -1 
1) "wangwu"
2) "zhaosi"
3) "zhaoliu"
4) "lisi"
127.0.0.1:6379> zrange book 0 -1  withscores
1) "wangwu"
2) "3"
3) "zhaosi"
4) "100"
5) "zhaoliu"
6) "200"
7) "lisi"
8) "300"

```

 - **zscore**
  返回有序集中，成员的分数值

```c
127.0.0.1:6379> zscore book wangwu  #zscore key value
"3"
```

 - **zrangebyscore**
  通过分数返回有序集合指定区间内的成员
>Redis Zrangebyscore 返回有序集合中指定分数区间的成员列表。有序集成员按分数值递增(从小到大)次序排列。

>具有相同分数值的成员按字典序来排列(该属性是有序集提供的，不需要额外的计算)。

>默认情况下，区间的取值使用闭区间 (小于等于或大于等于)，你也可以通过给参数前增加 ( 符号来使用可选的开区间 (小于或大于)。
```c

127.0.0.1:6379> zrangebyscore book -inf +inf withscores#整个有序集及成员的score值
1) "wangwu"
2) "3"
3) "zhaosi"
4) "100"
5) "zhaoliu"
6) "200"
7) "lisi"
8) "300"
127.0.0.1:6379> zrangebyscore book (1 500 #分数区间
1) "wangwu"
2) "zhaosi"
3) "zhaoliu"
4) "lisi"
127.0.0.1:6379> zrangebyscore book 10 1000  
1) "zhaosi"
2) "zhaoliu"
3) "lisi"

```

 - **zinterstore**
  计算给定的一个或多个有序集的交集，其中给定 key 的数量必须以 numkeys 参数指定，并将该交集(结果集)储存到 destination 。

默认情况下，结果集中某个成员的分数值是所有给定集下该成员分数值之和。
zinterstore destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]

```c
127.0.0.1:6379> zinterstore boo 2 book p
(integer) 3
127.0.0.1:6379> zrange boo 0 -1 withscores
1) "wangwu"
2) "23"
3) "zhaoliu"
4) "230"
5) "lisi"
6) "310"

```

 - **zscan**
  迭代有序集合中的元素（包括元素成员和元素分值）
```c
127.0.0.1:6379> zscan boo 0 match w* count 5
1) "0"
2) 1) "wangwu"
   2) "23"

```

 - **zrem**
  移除有序集合中的一个或多个成员

```c
127.0.0.1:6379> zrem boo wangwu
(integer) 1
127.0.0.1:6379> zrange boo 0 -1 withscores
1) "zhaoliu"
2) "230"
3) "lisi"
4) "310"
127.0.0.1:6379> 

```

 - **zrangebylex**

zrangebylex 返回指定成员区间内的成员，**按成员字典正序排序, 分数必须相同**。 在某些业务场景中,需要对一个字符串数组按名称的字典顺序进行排序时,可使用该命令;

```c
127.0.0.1:6379> zadd baba 1 a 1 b 1 c 1 d 
(integer) 4
127.0.0.1:6379> zrangebylex baba  - [b
1) "a"
2) "b"

```

  1. 分数必须相同! 如果有序集合中的成员分数有不一致的,返回的结果就不准。 成员字符串作为二进制数组的字节数进行比较。
  2. 默认是以ASCII字符集的顺序进行排列。如果成员字符串包含utf-8这类字符集的内容,就会影响返回结果,所以建议不要使用。 默认情况下,
  3. “max” 和 “min” 参数前必须加 “[” 符号作为开头。”[” 符号与成员之间不能有空格, 返回成员结果集会包含参数 “min”  和 “max” 。 
  4. “max” 和 “min” 参数前可以加 “(“ 符号作为开头表示小于, “(“    符号与成员之间不能有空格。返回成员结果集不会包含 “max” 和 “min” 成员。
  5. 可以使用 “-“ 和 “+” 表示得分最小值和最大值 “min” 和 “max” 不能反, “max” 放前面 “min”放后面会导致返回结果为空;
  6. 与ZRANGEBYLEX获取顺序相反的指令是ZREVRANGEBYLEX。 源码中采用C语言中 memcmp() 函数;
  7. 从字符的第0位到最后一位进行排序,如果前面部分相同,那么较长的字符串比较短的字符串排序靠后。


 - **其他命令**

```c
127.0.0.1:6379> zrank boo lisi # 返回有序集合指定成员的索引
(integer) 1
127.0.0.1:6379> zincrby boo 10 lisi #有序集合中对指定成员的分数加上增量 increment
"320"
127.0.0.1:6379> zrange boo 0 -1 withscores
1) "zhaoliu"
2) "230"
3) "lisi"
4) "320"
127.0.0.1:6379> zremrangebyscore boo 200 300  #移除有序集合中给定的分数区间的所有成员
(integer) 1
127.0.0.1:6379> zrange boo 0 -1 withscores
1) "lisi"
2) "320"
127.0.0.1:6379> zrange boo 0 -1  withscores
 1) "alili"
 2) "100"
 3) "ergouzi"
 4) "100"
 5) "wangwu"
 6) "100"
 7) "mazi"
 8) "102"
 9) "zhailiu"
10) "200"
11) "lisi"
12) "320"
127.0.0.1:6379> zrevrangebyscore boo 200 100 withscores
 1) "zhailiu"
 2) "200"
 3) "mazi"
 4) "102"
 5) "wangwu"
 6) "100"
 7) "ergouzi"
 8) "100"
 9) "alili"
10) "100"
127.0.0.1:6379> zrevrangebyscore boo 100 200 withscores
(empty array)

```
[更多Sorted Set命令](https://www.redis.net.cn/order/3616.html)

 

### Redis连接命令

 - **echo**
  打印字符串
```c
127.0.0.1:6379> echo hello
"hello"
```
 - **select**
  切换数据库
```c
127.0.0.1:6379> set db_number 0 # 为数据库分配编号 默认使用0 号数据库
OK
127.0.0.1:6379> select 1  #选中1 号数据库 
OK
127.0.0.1:6379[1]> get db_number #切换到了1 号数据库 Redis 现在的命令提示符多了个 [1]
(nil)
127.0.0.1:6379[1]> set db_number 1  
OK
127.0.0.1:6379[1]> get db_number 
"1"
127.0.0.1:6379[1]> select 3 
OK
127.0.0.1:6379[3]>  ##切换到了3 号数据库 Redis 现在的命令提示符多了个 [3]

127.0.0.1:6379[3]> zrange book 0 -1 #此时在数据库中找不到 book
(empty array)
127.0.0.1:6379[3]> select 0  #切换回数据库0
OK
127.0.0.1:6379> zrange book 0 -1  # 此时在数据库中找到了 book 
1) "wangwu"
2) "zhaosi"
3) "zhaoliu"
4) "lisi"

```

 - **ping**
  Ping 命令使用客户端向 Redis 服务器发送一个 PING ，如果服务器运作正常的话，会返回一个 PONG 。也可以自定义 message

>通常用于测试与服务器的连接是否仍然生效，或者用于测量延迟值。
```c
127.0.0.1:6379> ping  success
"success"
127.0.0.1:6379> ping
PONG
# 客户端和服务器连接不正常(网络不正常或服务器未能正常运行)
redis 127.0.0.1:6379> PING
Could not connect to Redis at 127.0.0.1:6379: Connection refused
```

 - **quit**
  关闭与当前客户端与redis服务的连接。
  如果还有未完成的事务则等待,一旦所有等待中的回复(如果有的话)顺利写入到客户端，连接就会被关闭。
```c
redis 127.0.0.1:6379> quit
```
 - **auth**
  检测给定的密码和配置文件中的密码是否相符~默认没有密码
```c
127.0.0.1:6379> auth password
(error) ERR AUTH <password> called without any password configured for the default user. Are you sure your configuration is correct?
127.0.0.1:6379> config requirepass "1234" 
OK
redis 127.0.0.1:6379> AUTH 1234
Ok
```

### Redis服务器命令

 - **flushdb**

清空数据库~刷新内存
```c
127.0.0.1:6379> zrange boo 0 -1 
1) "alili"
2) "ergouzi"
3) "wangwu"
4) "mazi"
5) "zhailiu"
6) "lisi"
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> zrange boo 0 -1 
(empty array)
127.0.0.1:6379> 

```

 - **save与bgsave**
  保存当前内存快照
>save 命令执行一个**同步保存**操作，将当前 Redis 实例的所有数据快照(snapshot)以 RDB 文件的形式保存到硬盘。
>bgsave 在后台异步保存当前数据库的数据到磁盘。

>bgsave 命令一个**异步保存**执行之后立即返回 OK ，然后 Redis fork 出一个新子进程，原来的 Redis 进程(父进程)继续处理客户端请求，而子进程则负责将数据保存到磁盘，然后退出。
>保存的文件~
>![在这里插入图片描述](https://img-blog.csdnimg.cn/c4621172555c4256b3d3633d83770801.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```c
127.0.0.1:6379> save 
OK
127.0.0.1:6379> bgsave
Background saving started
127.0.0.1:6379> lastsave  #返回最近一次 Redis 成功将数据保存到磁盘上的时间，以 UNIX 时间戳格式表示
(integer) 1646117542

```

 - **client kill**

关闭客户端连接

```c
127.0.0.1:6379> client kill 127.0.0.1:49742
OK

```

其他服务器命令
## Redis持久化策略
>Redis不仅仅是一个内存型数据库，还具备持久化能力。

在redis.conf配置文件中
![在这里插入图片描述](https://img-blog.csdnimg.cn/6d31b83b0fb04e1896129e795f1e69e9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)




>  将数据库保存到磁盘:  
>
>    save <seconds> <changes>  
>
>   保存数据库，如果给定的秒数和给定的 日志含义数据库发生写操作的次数。  


>  可以设置服务器配置的save选项，让服务器每隔一段时间自动执行一次BGSAVE命令，可以通过save选项设置多个保存条件，但只要其中任意一个条件被满足，服务器就会执行BGSAVE命令。
>
>   在900秒(15分钟)后，如果至少有一个按键改变  
>
>   在300秒(5分钟)后，如果至少改变10个按键  
>
>   在60秒后，如果至少改变10000个键  
>
>   注意:你可以通过注释掉所有的“save”行来完全禁用保存。  
>
>   也可以删除所有以前配置的保存  只执行save 不带参数

**持久化方式**
>redis持久化后的文件格式有两种一种是rdb(默认文件名为dump.rdb),另一种是aof(默认文件名为appendonly.aof)

#### rdb

rdb默认是开启的
![在这里插入图片描述](https://img-blog.csdnimg.cn/c4caecc890e048f297df39af8f0fc1a4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

优点~

该文件是一个紧凑文件,直接使用rdb文件就可以还原数据;

数据保存的操作由一个子进程负责,效率高于aof

缺点~
每次保存点之间导致redis不可意料的关闭，可能会丢失数据。

由于**每次保存数据都需要fork()子进程**，在数据量比较大时可能会比较耗费性能。

#### aof

aof默认是关闭的,需要手动开启,

![在这里插入图片描述](https://img-blog.csdnimg.cn/2402617cd19c4768b9866e14963eba8d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 AOF和RDB持久性可以同时启用而不会出现问题。  
如果AOF在启动时启用，Redis将加载AOF来进行数据恢复，即文件具有更好的耐用性保证。

优点~
相对rdb 数据会更加安全

缺点
相同数据,zof存储文件会大于rdb,同时数据恢复比rdb慢一些


## Redis主从复制
Redis支持**集群功能**。为了保证单一节点可用性，redis支持主从复制功能。每个节点有N个复制品（replica），其中一个复制品是主（master），另外N-1个复制品是从（Slave），也就是说**Redis支持一主多从。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/588f5c84cb7744be94a4b3657f8d2500.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_16,color_FFFFFF,t_70,g_se,x_16)
**主从的优点:**
像其他数据库一样,主从复制的优点~增强单一节点的健壮性,从而提高整个群集的稳定性;
>（Redis中当超过1/2节点不可用时，整个集群不可用）

从节点可以对主节点数据备份,提升了容错性,
**redis实现读写分离~**
主节点 用于写,从节点只能读以此来实现读写分离

### 主从搭建
1,关闭正在运行的redis客户端
2,新建文件夹~

```c
127.0.0.1:6379> quit  #退出客户端
[root@localhost bin]# ./redis-cli shutdown  #关闭客户端
[root@localhost bin]# mkdir /usr/local/replica #新建文件夹

```
3,将redis文件夹下的bin复制三份
作为master slave1 slave2

```c
[root@localhost redis]# cp -r /usr/local/redis/bin 
/usr/local/replica/master

[root@localhost redis]# cp -r /usr/local/redis/bin /usr/local/replica/slave1

[root@localhost redis]# cp -r /usr/local/redis/bin /usr/local/replica/slave2

[root@localhost redis]# 

```

4,修改从redis的配置文件

首先修改从redis   slave1的端口号  
>port 6380

然后让slave监听主Redis master 的行动

>replicaof 192.168.135.146  6379


5,然后启动 三个redis
启动前先关闭之前的redis

```c
[root@localhost bin]# ./redis-cli shutdown #说明之前已经关闭了
Could not connect to Redis at 127.0.0.1:6379: Connection refused

```

master slave1 slave2

在replica文件夹下新建一个startup.sh文件,用于三个redis服务依次开启,不用手动分几次开启;

```c
 [root@localhost ]#cd /usr/local/replica
[root@localhost replica]# touch startup.sh
[root@localhost replica]# ls
master  slave1  slave2  startup.sh
[root@localhost replica]# vi startup.sh 

```

startup.sh中添加如下内容

> cd /usr/local/replica/master/ ./redis-server redis.conf   cd
> /usr/local/replica/slave1 ./redis-server redis.conf   cd
> /usr/local/replica/slave2 ./redis-server redis.conf

执行startup.sh
```c
[root@localhost replica]# ./startup.sh
bash: ./startup.sh: Permission denied
[root@localhost replica]# 
```
权限不够
赋予权限~`chmod a+x startup.sh`

再次执行 startup.sh

```c
[root@localhost replica]# ./startup.sh
25648:C 01 Mar 2022 00:44:07.083 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
25648:C 01 Mar 2022 00:44:07.083 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=25648, just started
25648:C 01 Mar 2022 00:44:07.083 # Configuration loaded
25650:C 01 Mar 2022 00:44:07.118 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
25650:C 01 Mar 2022 00:44:07.118 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=25650, just started
25650:C 01 Mar 2022 00:44:07.118 # Configuration loaded
25656:C 01 Mar 2022 00:44:07.140 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
25656:C 01 Mar 2022 00:44:07.141 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=25656, just started
25656:C 01 Mar 2022 00:44:07.141 # Configuration loaded
[root@localhost replica]# 

```
查看启动状态~`ps aux|grep redis`

```c
[root@localhost replica]# ps aux |grep redis
root        4163  0.1  0.3  64372  6280 ?        Ssl  02:40   0:00 ./redis-server *:6379
root        4169  0.1  0.2 140152  4448 ?        Ssl  02:40   0:00 ./redis-server *:6380
root        4175  0.1  0.2 140152  4428 ?        Ssl  02:40   0:00 ./redis-server *:6381
root        4184  0.0  0.0  12136  1100 pts/0    S+   02:41   0:00 grep --color=auto redis

```
测试~打开主redis客户端

>cd/usr/local/replica/master
>
> ./redis-cli -p 6380

```c
[root@localhost replica]# cd master/
[root@localhost master]# ./redis-cli
127.0.0.1:6379> set name lisi
OK
127.0.0.1:6379> quit
[root@localhost master]# cd ../
[root@localhost replica]# cd slave1
[root@localhost slave1]# ./redis-cli -p 6380
127.0.0.1:6380> get name #获得key--value
"lisi"
127.0.0.1:6380> 
```
>但是这时候只有一个主redis,如果挂了,那么其他从不具备写的能力;

所以让一个从变成主，整个节点就可以继续工作。即使之前的主恢复过来也当做这个节点的从即可。

## Sentinel(哨兵)

Redis的哨兵就是帮助**监控整个节点**的，当节点主宕机等情况下，帮助重新选取主。

>Redis中哨兵支持单哨兵和多哨兵。

单哨兵是只要这个哨兵发现master宕机了，就直接选取另一个master。

多哨兵是根据我们设定，达到一定数量的哨兵认为master宕机后才会进行重新选取master

我们将主Redis进程杀掉之后~

```c
[root@localhost slave1]# ps aux|grep redis
root        4163  0.2  0.5  64372  9448 ?        Ssl  03:39   0:04 ./redis-server *:6379
root        4169  0.2  0.6 140152 11460 ?        Ssl  03:39   0:03 ./redis-server *:6380
root        4175  0.2  0.6 140152 11532 ?        Ssl  03:39   0:03 ./redis-server *:6381
root        4454  0.0  0.0  12136  1092 pts/0    S+   04:04   0:00 grep --color=auto redis
[root@localhost slave1]# kill -9 4163
[root@localhost slave1]# 

```
这时候我们进入从redis 客户端

```c
[root@localhost slave1]# ./redis-cli -p 6380
127.0.0.1:6380> info replication
# Replication
role:slave
master_host:192.168.135.145
master_port:6379
master_link_status:down
master_last_io_seconds_ago:-1
master_sync_in_progress:0
slave_repl_offset:2128
master_link_down_since_seconds:156
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:d7abcc2921dc048b18c2848e0feb2398df6d1b39
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:2128
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:2128
127.0.0.1:6380> 

```
杀掉主后，整个节点无法在写数据，从身份不会变化，主的信息还是以前的信息。
![在这里插入图片描述](https://img-blog.csdnimg.cn/d1ac5c6a7fcc4616bf435aad0ed73891.png)
这时候我们需要搭建多哨兵;

### 搭建哨兵环境

 - 1,新建文件夹用于承载哨兵环境

```c
[root@localhost slave1]# mkdir /usr/local/sentinel
```

 - 2,复制redis配置文件夹bin下所有文件 到sentinel

```c
# cp -r /usr/local/redis/bin/* /usr/local/sentinel
```

 - 3,之后将哨兵配置文件复制到 sentinel

进入redis的解压目录~复制配置文件到sentinel
>cd /usr/local/redis-6.0.6/
>[root@localhost redis-6.0.6]# cp sentinel.conf  /usr/local/sentinel/

 - 4,修改配置文件

>vi /usr/local/sentinel/sentinel.conf  

修改内容如下~
>port 26379

>daemonize yes

>logfile “/usr/local/sentinel/26379.log”

>sentinel monitor mymaster 192.168.93.10 6379 2

5,再复制两份sentinel.conf   分别命名为sentinel-26380.conf   sentinel-26381.conf   
修改端口 为 26380   26381
logfile 为26380.log  26381.log

启动主从

启动之前先杀掉之前存在的redis进程

```c
[root@localhost replica]# ps aux|grep redis
root        4169  0.4  0.6 140152 11600 ?        Ssl  03:39   0:19 ./redis-server *:6380
root        4175  0.4  0.6 140152 11468 ?        Ssl  03:39   0:19 ./redis-server *:6381
root        4493  0.6  0.5  64372  9436 ?        Ssl  04:09   0:17 ./redis-server *:6379
root        5069  0.0  0.0  12136  1040 pts/0    S+   04:57   0:00 grep --color=auto redis
[root@localhost replica]# kill -9 4169
[root@localhost replica]# kill -9 4175
[root@localhost replica]# kill -9 4493
[root@localhost replica]# 

```
启动主从

```c
[root@localhost replica]# ls
master  slave1  slave2  startup.sh
[root@localhost replica]# ./startup.sh
```

启动哨兵

```c
[root@localhost replica]# cd /usr/local/sentinel/
[root@localhost sentinel]# ls
dump.rdb         redis-check-rdb  redis-sentinel       sentinel-26381.conf
redis-benchmark  redis-cli        redis-server         sentinel.conf
redis-check-aof  redis.conf       sentinel-26380.conf
[root@localhost sentinel]# ./redis-sentinel sentinel.conf 
[root@localhost sentinel]# ./redis-sentinel sentinel-26380.conf 
[root@localhost sentinel]# ./redis-sentinel sentinel-26381.conf 
[root@localhost sentinel]# 

```
查看哨兵状态~`ps aux|grep sentinel`

![在这里插入图片描述](https://img-blog.csdnimg.cn/2a6242b0b8bf4c3d86e21afd301650b2.png)


查看日志

>cat 26379.log

杀死主redis进程~
然后进入原本是slave的redis客户端

![在这里插入图片描述](https://img-blog.csdnimg.cn/96be844d8a81427bbd9f21cef1526145.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
可以发现该节点变为了master,并且可以写入数据了
![在这里插入图片描述](https://img-blog.csdnimg.cn/d43396b454244b998aa5f5b2f0b57800.png)
进入 另一个salve节点  6381,不能写入,说明是从节点
![在这里插入图片描述](https://img-blog.csdnimg.cn/25baf269264d46bd8d420e2ed6cb7d70.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a32950e55c554ba5a798591fbcdfe0ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## Java操作redis

![在这里插入图片描述](https://img-blog.csdnimg.cn/cd3fadf3e88c47c898525a2bef3b5910.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

> Redis给Java语言提供了客户端API，称之为Jedis。

导入依赖

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.1.1</version>
</dependency>

```

### 单机版redis连接/操作

 - jedis对象

**构造Jedis对象~**连接redis
编写测试代码~

```java
 @Test
    void jedisDemo1() {
        try {
            Jedis jedis = new Jedis("192.168.135.145", 6379);
            jedis.set("fruit", "lisi");
            String fruit = jedis.get("fruit");
            System.out.println(fruit);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
lisi
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/349834c58b014b1ab8f2e6082bf89cf0.png)



异常解决方案~[jedis连接redis异常解决方案](https://blog.csdn.net/weixin_54061333/article/details/123229510)

 - jedis线程池

**构造JedisPool对象~**连接redis
```java
    @Test
    void jedisDemo2() {
    //提供连接池连接redis
        JedisPool jedisPool = new JedisPool("192.168.135.145", 6379);
        //获取资源
        Jedis jedis = jedisPool.getResource();
        Map<String, String> map = new HashMap();
        map.put("person", new Person("王五", 39).toString());
        jedis.hset("person", map);
        Map<String, String> map1 = jedis.hgetAll("person");
        for (Map.Entry<String, String> stringEntry : map1.entrySet()
        ) {
            System.out.println(stringEntry);

        }
    }
```

 - JedisPooled对象
    构造JedisPooled对象来快速操作redis

```java
 @Test
    void jedisDemo3() {

        JedisPooled jedisPool = new JedisPooled("192.168.135.145", 6379);
        jedisPool.sadd("hello", "world");
        Set<String> hello = jedisPool.smembers("hello");
        hello.forEach(System.out::println);
    }
```
这种方式底层是通过配置GenericObjectPoolConfig实现的;



```java
public class GenericObjectPoolConfig<T> extends BaseObjectPoolConfig<T> {
    public static final int DEFAULT_MAX_TOTAL = 8;
    public static final int DEFAULT_MAX_IDLE = 8;
    public static final int DEFAULT_MIN_IDLE = 0;
    private int maxTotal = 8;
    private int maxIdle = 8;
    private int minIdle = 0;

    public GenericObjectPoolConfig() {
    }
```

>Jedis实例实现大多数 Redis 命令。

[jedisAPI查看与下载](https://www.javadoc.io/doc/redis.clients/jedis/latest/redis/clients/jedis/Jedis.html)

### redis集群连接操作
开启集群

```c
[root@localhost bin]# ./startup.sh 
10147:C 01 Mar 2022 23:59:27.486 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
10147:C 01 Mar 2022 23:59:27.486 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=10147, just started
10147:C 01 Mar 2022 23:59:27.486 # Configuration loaded
10149:C 01 Mar 2022 23:59:27.498 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
10149:C 01 Mar 2022 23:59:27.498 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=10149, just started
10149:C 01 Mar 2022 23:59:27.498 # Configuration loaded
10155:C 01 Mar 2022 23:59:27.505 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
10155:C 01 Mar 2022 23:59:27.505 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=10155, just started
10155:C 01 Mar 2022 23:59:27.505 # Configuration loaded
10161:C 01 Mar 2022 23:59:27.517 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
10161:C 01 Mar 2022 23:59:27.517 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=10161, just started
10161:C 01 Mar 2022 23:59:27.517 # Configuration loaded
10167:C 01 Mar 2022 23:59:27.538 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
10167:C 01 Mar 2022 23:59:27.538 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=10167, just started
10167:C 01 Mar 2022 23:59:27.538 # Configuration loaded
10173:C 01 Mar 2022 23:59:27.550 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
10173:C 01 Mar 2022 23:59:27.550 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=10173, just started
10173:C 01 Mar 2022 23:59:27.550 # Configuration loaded
[root@localhost bin]# 

```
java代码连接redis集群

```java
//redis集群
    @Test
    void jedisDemo5() {
        HashSet<HostAndPort> hostAndPorts = new HashSet<>();
        hostAndPorts.add(new HostAndPort("192.168.135.145", 6789));
        hostAndPorts.add(new HostAndPort("192.168.135.145", 6790));
        hostAndPorts.add(new HostAndPort("192.168.135.145", 6791));
        hostAndPorts.add(new HostAndPort("192.168.135.145", 6792));
        hostAndPorts.add(new HostAndPort("192.168.135.145", 6793));
        hostAndPorts.add(new HostAndPort("192.168.135.145", 6794));
        JedisCluster jedisCluster = new JedisCluster(hostAndPorts);
        jedisCluster.set("name","codem");
        ystem.out.println(jedisCluster.get("name"))
        jedisCluster.sadd("book","java","html");
        System.out.println(jedisCluster.smembers("book"));


    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/272e5fea44cf4530b67a05ef66faf2f8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
[Jedis操作教程~](https://github.com/redis/jedis)

使用 Redis 模块
Jedis 为一些 Redis 模块提供支持，最著名的是 [RedisJSON](https://oss.redis.com/redisjson/) 和 [RediSearch](https://oss.redis.com/redisearch/)。

有关详细信息，请参阅 [RedisJSON Jedis](https://github.com/redis/jedis/blob/master/docs/redisjson.md) 快速入门。





## Spring整合redis

Spring Data 好处很方便操作对象类型。	把Redis不同值得类型放到一个opsForXXX方法中。

		opsForValue : String值
	
		opsForList : 列表List
	
		opsForHash: 哈希表Hash
	
		opsForZSet: 有序集合Sorted Set
	
		opsForSet : 集合


导入依赖
```xml
<!--redis启动器 其他不做描述--> 
              <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-redis</artifactId>
                <version>2.6.4</version>
            </dependency>
           
             <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.6.4</version>
            </dependency>

            <dependency>
                <groupId>org.junit.jupiter</groupId>
                <artifactId>junit-jupiter</artifactId>
                <version>5.8.2</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
                <version>2.6.4</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.2.2</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.22</version>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.28</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
                <version>2.6.4</version>
            </dependency>
 
```

跟之前的架构一样,
这里只展示关键代码~


定义一个redis配置类

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnection;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * @author Gavin
 */
@Configuration
public class redisConfigration {

    @Bean
   // redis模板 (传入一个redis连接工厂接口)
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory factory){
//        固定写法
//实例化有一个redis模板对象
        RedisTemplate<String,Object> redisTemplate= new RedisTemplate<String, Object>();

        redisTemplate.setConnectionFactory(factory);
        //将key序列化后传入  key是字符串
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //将值传入redis
        redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<Object>(Object.class));
       // 返回redis模板
        return redisTemplate;
    }
}
```
服务实现
```java
package com.gavin.service.impl;

import com.gavin.mapper.GoodsMapper;
import com.gavin.pojo.Goods;
import com.gavin.service.redisService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.stereotype.Service;

@Service
public class redisServiceImpl implements redisService {

    @Autowired
    private GoodsMapper goodsMappper;
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    @Override
    public Goods selectById(Integer id) {
//        先从redis中获取数据
        String key = "Goods:" + id;
        if (redisTemplate.hasKey(key)) {
            System.out.println("执行缓存查找");
            redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<Goods>(Goods.class));
            Goods goods = (Goods) redisTemplate.opsForValue().get(key);
            return goods;
        }
        System.out.println("执行了mysql");
        Goods goods = goodsMappper.selectByPrimaryKey(id);

        //将数据放入redis
        redisTemplate.opsForValue().set(key,goods);
        return goods;
    }
}

```
controller层

```java
import com.gavin.pojo.Goods;
import com.gavin.service.redisService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * @author Gavin
 */
@Controller
public class RedisController {
    @Autowired
    private redisService redisService;

@RequestMapping("/goods")
    public String selectById(Integer id, Model model) {

    Goods goods = redisService.selectById(id);
    model.addAttribute("goods",goods);
    return "index";
}
}

```
application.yml

```yml
spring:
    #配置数据源
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/gavin
    username: gavin
    password: 955945
   #单机版redis设置
  redis:
    host: 192.168.135.145
    port: 6379    
 #   redis集群配置
  redis:
    cluster:
      nodes: 192.168.135.145:6789,192.168.135.145:6790,192.168.135.145:6791,192.168.135.145:6792,192.168.135.145:6793,192.168.135.145:6794

mybatis:
  #mapper位置
  mapper-locations: classpath:/com.gavin.mapper/*.xml
  #实体类别名
  type-aliases-package: com.gavin.pojo

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d4a4e92a8e4249bcb6471c6418644ebd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



## Spring整合redis

Spring Data 好处很方便操作对象类型。	把Redis不同值得类型放到一个opsForXXX方法中。

		opsForValue : String值
	
		opsForList : 列表List
	
		opsForHash: 哈希表Hash
	
		opsForZSet: 有序集合Sorted Set
	
		opsForSet : 集合


导入依赖
```xml
<!--redis启动器 其他不做描述--> 
              <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-redis</artifactId>
                <version>2.6.4</version>
            </dependency>
           
             <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.6.4</version>
            </dependency>

            <dependency>
                <groupId>org.junit.jupiter</groupId>
                <artifactId>junit-jupiter</artifactId>
                <version>5.8.2</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
                <version>2.6.4</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.2.2</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.22</version>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.28</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
                <version>2.6.4</version>
            </dependency>
 
```

跟之前的架构一样,
这里只展示关键代码~


定义一个redis配置类

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnection;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * @author Gavin
 */
@Configuration
public class redisConfigration {

    @Bean
   // redis模板 (传入一个redis连接工厂接口)
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory factory){
//        固定写法
//实例化有一个redis模板对象
        RedisTemplate<String,Object> redisTemplate= new RedisTemplate<String, Object>();

        redisTemplate.setConnectionFactory(factory);
        //将key序列化后传入  key是字符串
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //将值传入redis
        redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<Object>(Object.class));
       // 返回redis模板
        return redisTemplate;
    }
}
```
服务实现
```java
package com.gavin.service.impl;

import com.gavin.mapper.GoodsMapper;
import com.gavin.pojo.Goods;
import com.gavin.service.redisService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.stereotype.Service;

@Service
public class redisServiceImpl implements redisService {

    @Autowired
    private GoodsMapper goodsMappper;
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    @Override
    public Goods selectById(Integer id) {
//        先从redis中获取数据
        String key = "Goods:" + id;
        if (redisTemplate.hasKey(key)) {
            System.out.println("执行缓存查找");
            redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<Goods>(Goods.class));
            Goods goods = (Goods) redisTemplate.opsForValue().get(key);
            return goods;
        }
        System.out.println("执行了mysql");
        Goods goods = goodsMappper.selectByPrimaryKey(id);

        //将数据放入redis
        redisTemplate.opsForValue().set(key,goods);
        return goods;
    }
}

```
controller层

```java
import com.gavin.pojo.Goods;
import com.gavin.service.redisService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * @author Gavin
 */
@Controller
public class RedisController {
    @Autowired
    private redisService redisService;

@RequestMapping("/goods")
    public String selectById(Integer id, Model model) {

    Goods goods = redisService.selectById(id);
    model.addAttribute("goods",goods);
    return "index";
}
}

```
application.yml

```yml
spring:
    #配置数据源
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/gavin
    username: gavin
    password: 955945
   #单机版redis设置
  redis:
    host: 192.168.135.145
    port: 6379    
 #   redis集群配置
  redis:
    cluster:
      nodes: 192.168.135.145:6789,192.168.135.145:6790,192.168.135.145:6791,192.168.135.145:6792,192.168.135.145:6793,192.168.135.145:6794

mybatis:
  #mapper位置
  mapper-locations: classpath:/com.gavin.mapper/*.xml
  #实体类别名
  type-aliases-package: com.gavin.pojo

```


![在这里插入图片描述](https://img-blog.csdnimg.cn/d4a4e92a8e4249bcb6471c6418644ebd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
返回list集合
```java
//list集合
    @Override
    public List<Goods> selectAllGoods() {
        String key="goodsInfo";
        if (redisTemplate.hasKey(key)){
            System.out.println("redis缓存中已找到");
            redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<List>(List.class));
            List<Goods> goodsInfo = (List<Goods>)redisTemplate.opsForValue().get("goodsInfo");
            return  goodsInfo;
        }
        System.out.println("执行了 mysql");
        List<Goods> goodsInfo = goodsMappper.selectAll();
        redisTemplate.opsForValue().set(key,goodsInfo);

        return goodsInfo;
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e4eaa2bff6ed43efa0405fd2c5e7f813.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)