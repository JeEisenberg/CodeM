# ﻿ Redis

## Redis中的数据类型

key总是 string类型的

所以判断key类型的时候其实是判断的value类型

value的类型 有------

String（字符串  用法： 键  值），

Hash（哈希 类似Java中的 map  用法 键值对），

List（列表  用法：键 集合 不可以重复），

Set（集合 用法：键 集合 可以重复），

Zset（sorted set 有序集合 ),



但是redis在实际运用中远远不止这么些数据类型,为什么这么说呢?



实际中生产中的数据类型

运行一个的单机版的redis

 set key value 

设置 age 的值

    127.0.0.1:6379> set age  5
    OK
    127.0.0.1:6379> get age
    "5"

我们看一下set的用法

    127.0.0.1:6379> help set   
      SET key value [EX seconds|PX milliseconds] [NX|XX] [KEEPTTL]
      summary: Set the string value of a key
      since: 1.0.0
      group: string

那么这个name应该是什么类型的呢?

应该是字符串类型的;

    127.0.0.1:6379> type age
    string

type的用法

    127.0.0.1:6379> help type
    
      TYPE key
    
      summary: Determine the type stored at key
    
      since: 1.0.0
    
      group: generic


可不可以给字符串加上一个值?按照我们实际认为的那样~5是一个数值

    127.0.0.1:6379> incr age
    (integer) 6
    127.0.0.1:6379> get age
    "6"



但既然按照type命令,他告诉我们~5是一个字符串,那么可不可以在字符串后面追加内容?

    127.0.0.1:6379> append age 999
    (integer) 4
    127.0.0.1:6379> get age
    "6999"

这个时候我们在判断他的类型

    127.0.0.1:6379> type age
    string

好像一直都是string类型的;



在底层他是怎样编码的呢?

在 object命令下有一个encoding用于查看value底层编码

    127.0.0.1:6379>  help object
    
      OBJECT subcommand [arguments [arguments ...]]
      summary: Inspect the internals of Redis objects
      since: 2.2.3
      group: generic


一开始设置值的时候我们查看他的底层编码~

    127.0.0.1:6379> object encoding age
    "int"

但是在redis的五大数据类型中并没有int类型;

在append之后age的数据类型

    127.0.0.1:6379> object encoding age
    "raw"

再次执行 incr后age又变成了int

    127.0.0.1:6379> incr age
    (integer) 7000
    127.0.0.1:6379> object encoding age
    "int"

实际上redis拿到数据后是以二进制编码的方式存储的;

    127.0.0.1:6379> append age 中
    (integer) 7
    127.0.0.1:6379> get age
    "7000\xe4\xb8\xad"

一个汉字3个字节;

二进制存储有什么好处呢?

可以存任何数据类型,比如图片,视频或者任何序列化对象;

string类型的值最大能存储512MB;

另一个好处是能够做到二进制安全



二进制安全

什么是二进制安全?

举个例子~ 

设置一下price的值

    127.0.0.1:6379> set price 1
    OK
    127.0.0.1:6379> get price
    "1"

字符串长度是多少?

    127.0.0.1:6379> strlen price
    (integer) 1

strlen的用法

    127.0.0.1:6379> help strlen
    
      STRLEN key
      summary: Get the length of the value stored in a key
      since: 2.2.0
      group: string

这个时候我们用incrby 来实现一个增量9

    127.0.0.1:6379> incrby price 9
    (integer) 10

这个时候的长度是多少?

    127.0.0.1:6379> strlen price
    (integer) 2

在不同的语言中,数据类型所占的位宽有时候是不一样的,各个类型的变量长度是由编译器来决定的;

在java中 int占4字节,可能别的语言占用两个字节,这样就有可能发生字段溢出的错误;

发生的情形~

先做一个假设,redis中存储的value数据类型有int,1个int类型占4字节,编译器编译后value占4字节,某一个语言也用int数据去接这个数据,但是这个语言的int是占2字节的,这就会造成字段溢出;

所以在redis中  set age  5 value并不会按照数值型数据存储,而是以字节存储的;

当我们执行incr(或者其他增量命令时),首先会从内存中取出来先转换成数值类型,如果能够执行成功,则转换为甚至并执行增量,然后更新encoding编码;

    127.0.0.1:6379> set k1 12
    OK
    127.0.0.1:6379> incr k1
    (integer) 13
    127.0.0.1:6379> set hello aaa
    OK
    127.0.0.1:6379> incr hello
    (error) ERR value is not an integer or out of range



当我们执行append的时候底层是识别为raw,

    127.0.0.1:6379> append k1 12
    (integer) 4
    127.0.0.1:6379> type k1
    string
    127.0.0.1:6379> object encoding k1
    "raw"
    127.0.0.1:6379> incrby k1 12
    (integer) 1324
    127.0.0.1:6379> type k1
    string
    127.0.0.1:6379> object encoding k1
    "int"

再次执行增量之后,如果执行成功,则也会转为int;



redis存储数据~字节流的体现

以UTF-8编码

    127.0.0.1:6379> set name 中
    OK
    127.0.0.1:6379> type name
    string
    127.0.0.1:6379> object encoding name
    "embstr"
    127.0.0.1:6379> strlen name
    (integer) 3
    127.0.0.1:6379>





修改编码格式为GBK ~这个时候strlen的长度为2

    127.0.0.1:6379> set name 中
    OK
    127.0.0.1:6379> type name
    string
    127.0.0.1:6379> object encoding name
    "embstr"
    127.0.0.1:6379> strlen name
    (integer) 2

所以我们使用redis时会先将数据转为本客户端所在的字符编码格式然后再交给redis存储;



触发格式化

    [zzy@Gavin bin]$ ./redis-cli --raw


## Redis中的事务

在redis中有这样几个操作
**setnx 与setex**

```bash
[root@Gavin bin]# ./redis-cli -p 6379
127.0.0.1:6379> set k value
OK
127.0.0.1:6379> setnx k value1
(integer) 0
127.0.0.1:6379> get k
"value"
127.0.0.1:6379> setex k    100  value2
OK
127.0.0.1:6379> get k
"value2"

127.0.0.1:6379> ttl k
(integer) 98
```
**setnx与setex用法**
```bash
127.0.0.1:6379> help setnx
  SETNX key value
  summary: Set the value of a key, only if the key does not exist
  since: 1.0.0
  group: string

127.0.0.1:6379> help setex

  SETEX key seconds value  #先设置过期时间在设置值
  summary: Set the value and expiration of a key
  since: 2.0.0
  group: string
```

当批量操作时

```bash
127.0.0.1:6379> flushall
OK
127.0.0.1:6379> msetnx k value k2 value4
(integer) 1
127.0.0.1:6379> get k
"value"
127.0.0.1:6379> get k2
"value4"

```
这时候我们再次下面的操作
```bash
127.0.0.1:6379> msetnx k2 value2 k3 value4
(integer) 0
127.0.0.1:6379> get k3
(nil)
```
>这说明用msetnx同时设置多个k v时,如果有一个失败,则其他的也不会成功;

如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾。

返回字符串长度

    127.0.0.1:6379> append math 100
    (integer) 7
    127.0.0.1:6379> get math
    "1000100"

- setbit

    127.0.0.1:6379> help setbit
    
      SETBIT key offset value
    
      summary: Sets or clears the bit at offset in the string value stored at key
    #在
      since: 2.2.0
    
      group: string
    

对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。

根据值 value 是 1 或 0 来决定设置或清除位 bit。 当 key 不存在时会创建一个新的字符串。

当字符串不够长时，字符串的长度将增大，以确保它可以在offset位置存储值。

offset 参数需要大于等于0，并且小于 232 (bitmaps 最大 512MB)。

当值字符串变长时，添加的 bit 会被设置为 0。

    127.0.0.1:6379> setbit k 2 1
    (integer) 0
    127.0.0.1:6379> get k
    "m"
    
    127.0.0.1:6379> setbit m 2 1
    (integer) 0
    127.0.0.1:6379> get m
    " "
    #空格的二进制~00100000,当没有字符串时会新建一个字符串
    
    127.0.0.1:6379> setbit n  9 1
    (integer) 0
    127.0.0.1:6379> get n
    "\x00@"
    #设置时由于超过了一个字节 (8位),需要两个字节,不足的部分补0
    # 得到0000 0000 0100 0000   对应ascii中的@

- getbit

    127.0.0.1:6379> help getbit
    
      GETBIT key offset
      summary: Returns the bit value at offset in the string value stored at key
      since: 2.2.0
      group: string
    

使用案例~

    127.0.0.1:6379> set k M
    OK
    #M的二进制表示为~01001101
    127.0.0.1:6379> getbit k 2
    (integer) 0
    #从第 0 开始 偏移量为2  的为  0

- bitpos

返回字符串里面第一个被设置为1或者0的bit位。 

    127.0.0.1:6379> help bitpos
    
      BITPOS key bit [start] [end]
      summary: Find first bit set or clear in a string  #返回字符串里面第一个被设置为1或者0的bit位。 
      since: 2.8.7
      group: string


    127.0.0.1:6379> setbit n  9 1
    (integer) 0
    127.0.0.1:6379> get n
    "\x00@"
    127.0.0.1:6379> bitpos n 0 0 0
    (integer) 0
    127.0.0.1:6379> bitpos n 0 0 1
    (integer) 0
    127.0.0.1:6379> bitpos n 0 0 9
    (integer) 0


setbit/getbit等为操作有什么实际应用呢?

- setbit/getbit的实际应用

1,统计一段时间内的活跃用户

如果是普通方式,需要通过读取关系型数据库,经过IO操作,设计的内存会比较大(用户id,日期等),如果通过redis的位操作,内存使用量会大大减少;

首先,根据位操作可以实现 

```bash
#set key value;

#key映射为用户的id,接下来就是位操作了;

#一年有365天(366天),

#第一天登录则该位设置为1,否则设置为0
127.0.0.1:6379> setbit xm 1 1 #表示小明第一天登陆了
(integer) 0  
127.0.0.1:6379> setbit xm 100 1 #表示小明第100天登陆了
(integer) 0
#以此类推;这样我们可以通过不到50个字节来记录下用户的登录状态

#活跃用户判断~  临近双十一,近一周的活跃用户(只要双十一前的这两周登陆过就算是活跃用户)需要用到bitcount来统计一下
```


- bitcount

用法

    BITCOUNT key [start][end]

计算给定字符串中，被设置为 1 的比特位的数量。

一般情况下，给定的整个字符串都会被进行计数，通过指定额外的 start 或 end 参数，可以让计数只在特定的位上进行。

start 和 end 参数的设置和 GETRANGE 命令类似，都可以使用负数值：比如 -1 表示最后一个位，而 -2 表示倒数第二个位，以此类推。

不存在的 key 被当成是空字符串来处理，因此对一个不存在的 key 进行 BITCOUNT 操作，结果为 0 。

```
127.0.0.1:6379> help bitcount

  BITCOUNT key [start end]
  summary: Count set bits in a string
  since: 2.6.0
  group: string
```

所以统计双十一前两周的活跃用户只需要指定好相应的位用函数bitcount 统计即可(一个字节四位 )

    127.0.0.1:6379> bitcount xm -2 -1
    (integer) 4
    127.0.0.1:6379>



>位操作的好处~节省空间,速度快;


>我们来算一下记录的登陆的用户占用多少内存吧,每个用户50字节,4000万用户,2G左右,这样4000万用户的登录数据占2G内存,如果是redis集群,这样对单一服务器的压力会减少;



- 统计一段时间内的活跃用户

应用场景~双十一要搞3天,M要求在双十一期间给每一位登陆的用户发一个10块钱代金券应该发多少张代金券呢?

-----假如2亿用户中有僵尸用户,一人只能发一张;

还是通过setbit去操作;

上面是通过人去统计天数,现在要求通过天数来统计人,所以把key 和 value颠倒一下并做好用户与value位之间的映射 ;

比如 setbit 20221104 1  1   表示在20221104这一天有第一位用户xm登陆了(一定要做好用户和位之间的映射)

所以在统计 20221104到20221110这些天有多少用户 登陆了

通过bittop函数来实现

- bittop

    BITOP operation destkey key [key ...]

对一个或多个保存二进制位的字符串 key 进行位元操作，并将结果保存到 destkey 上。

operation 可以是 AND 、 OR 、 NOT 、 XOR 这四种操作中的任意一种：

- BITOP AND destkey key [key ...] ，对一个或多个 key 求逻辑并，并将结果保存到 destkey 。
- BITOP OR destkey key [key ...] ，对一个或多个 key 求逻辑或，并将结果保存到 destkey 。
- BITOP XOR destkey key [key ...] ，对一个或多个 key 求逻辑异或，并将结果保存到 destkey 。
- BITOP NOT destkey key ，对给定 key 求逻辑非，并将结果保存到 destkey 。

除了 NOT 操作之外，其他操作都可以接受一个或多个 key 作为输入。

处理不同长度的字符串

当 BITOP 处理不同长度的字符串时，较短的那个字符串所缺少的部分会被看作 0 。

空的 key 也被看作是包含 0 的字符串序列。

所以将20221104 到202221111这期间的key取or操作就能得到登录的这期间登陆的用户(并且已去重);

    127.0.0.1:6379> bitop or 20221111 20221104 20221105
    (integer) 39
    127.0.0.1:6379> bitcount 20221111 -2 -1
    (integer) 3


## Redis中的数据结构
说到数据结构,我们会想到------>>>>

链表----单向链表, 双向链表

在redis中我们可以再次接触一下数据结构;

```bash
127.0.0.1:6379> Lpush zzy a b c d
(integer) 4

127.0.0.1:6379> rpush zzy  e f g h
(integer) 7
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a0ba4041420f46438e7a8d801f835487.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
弹出的时候--->>

```bash
127.0.0.1:6379> lpop zzy
"d"
127.0.0.1:6379> rpop zzy
"h"
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/1dab58cf21034fe38d834993e4e3db37.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
所以我们的数据可以做到先进先出和先进后出---因为有两个出口;

另一个这个数据结构和java的list原理一样,所以有时候我们可以直接将数据存到redis的list里面,做好持久化,这样会大大加快读取速度;


## Redis中的对象存储

在java中我们对对象是很了解的,在redis中存储对象用什么合适?

比如 一个person对象,有name ,age ,gender

```bash
127.0.0.1:6379> hset person name zhangsan age 19 gender man
(integer) 3
```
我们可以针对对象做一些操作

**增加/修改属性**

```bash
127.0.0.1:6379> hmset person name lisi like PingPang
OK
127.0.0.1:6379> hgetall person
1) "name"
2) "lisi"
3) "age"
4) "19"
5) "gender"
6) "man"
7) "like"
8) "PingPang"
127.0.0.1:6379>
```
我们可以单独获取属性/属性值

```bash
127.0.0.1:6379> hkeys person
1) "name"
2) "age"
3) "gender"
4) "like"
127.0.0.1:6379> hvals person
1) "lisi"
2) "19"
3) "man"
4) "PingPang"
127.0.0.1:6379>
```
同时hash还有个跟字符串的命令类似的

-`hsetnx`

```bash
127.0.0.1:6379> help hsetnx

  HSETNX key field value
  summary: Set the value of a hash field, only if the field does not exist #只有在字段 field 不存在时，设置哈希表字段的值
  since: 2.0.0
  group: hash
127.0.0.1:6379> hsetnx person name wangwu
(integer) 0
```

## Redis中的正负索引

 - 1,获取对应位置的值

```bash
127.0.0.1:6379> lindex zzy 4
"f"
127.0.0.1:6379> lindex zzy -4
"a"
```

 - 2,遍历所有数据~

```bash
127.0.0.1:6379> lrange zzy 1 -1
1) "b"
2) "a"
3) "e"
4) "f"
5) "g"

```

 - 3,更新数据


利用索引更新数据
```bash
127.0.0.1:6379> lset zzy 1 A
OK
127.0.0.1:6379> lset zzy -1 F
OK
127.0.0.1:6379> lrange zzy 1 -1
1) "A"
2) "a"
3) "e"
4) "f"
5) "F"
```

 - 4-移除数据

有意思的一点是,移除数据~

```bash
127.0.0.1:6379> help lrem

  LREM key count element
  summary: Remove elements from a list
  since: 1.0.0
  group: list

```
移除多个数据

```bash
127.0.0.1:6379> lrem zzy 2 a
(integer) 1
```
有点像去重的感觉

但是两边都可以操作数据,是从哪边开始移除的呢?

```bash
127.0.0.1:6379> lpush code 1 a 2 a 3 a 4 a 5 a 6 a 7 a 8 a 9 a

(integer) 18
127.0.0.1:6379> lrem code 2 a
(integer) 2
127.0.0.1:6379> lrange code 0 -1
 1) "9"
 2) "8"
 3) "a"
 4) "7"
 5) "a"
 6) "6"
 7) "a"
 8) "5"
 9) "a"
10) "4"
11) "a"
12) "3"
13) "a"
14) "2"
15) "a"
16) "1"
127.0.0.1:6379> lrem code -2 a
(integer) 2
127.0.0.1:6379> lrange code 0 -1
 1) "9"
 2) "8"
 3) "a"
 4) "7"
 5) "a"
 6) "6"
 7) "a"
 8) "5"
 9) "a"
10) "4"
11) "a"
12) "3"
13) "2"
14) "1"
127.0.0.1:6379>
```
>当count>0 时从右面开始移动,反之从左面

插入数据~

```bash
127.0.0.1:6379> help linsert

  LINSERT key BEFORE|AFTER pivot element
  summary: Insert an element before or after another element in a list
  since: 2.2.0
  group: list
127.0.0.1:6379> linsert code before 2 a
(integer) 15
127.0.0.1:6379> lrange code 0 -1
 1) "9"
 2) "8"
 3) "a"
 4) "7"
 5) "a"
 6) "6"
 7) "a"
 8) "5"
 9) "a"
10) "4"
11) "a"
12) "3"
13) "a"
14) "2"
15) "1"
```

现在还有一个问题~因为有很多a 那如果往a之前或者之后插入数据,会是什么情况呢?

```bash
#已将数据恢复至新建状态后
127.0.0.1:6379> linsert code before a A
(integer) 18
127.0.0.1:6379> lrange code 0 -1
 1) "9"
 2) "A"
 3) "a"
 4) "8"
 5) "a"
 6) "7"
 7) "a"
 8) "6"
 9) "a"
10) "5"
11) "a"
12) "4"
13) "a"
14) "3"
15) "a"
16) "2"
17) "a"
18) "1"
127.0.0.1:6379> linsert code after a Q
(integer) 19
127.0.0.1:6379> lrange code 0 -1
 1) "9"
 2) "A"
 3) "a"
 4) "Q"
 5) "8"
 6) "a"
 7) "7"
 8) "a"
 9) "6"
10) "a"
11) "5"
12) "a"
13) "4"
14) "a"
15) "3"
16) "a"
17) "2"
18) "a"
19) "1"
```

>可以看到 
>>**从左面且不能从第二个之后追加**

## Redis中的阻塞队列

开启三个redis服务----非集群

 - blpop

```bash
  BLPOP key [key ...] timeout
  summary: Remove and get the first element in a list, or block until one is available
  since: 2.0.0
  group: list
```

```bash
127.0.0.1:6379> flushall
OK
```

客户端A先执行blpop命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/d43f076f7dbc486688fd6434bac78908.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
处于阻塞状态

之后另一个客户端B也执行blpop命令,也处于阻塞状态
![在这里插入图片描述](https://img-blog.csdnimg.cn/e3df874b755740f7ac95395c3456af44.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


然后往 客户端C中的list  添加数据

```bash
127.0.0.1:6379> lpush name  y
(integer) 1
```
客户端A阻塞结束了
![在这里插入图片描述](https://img-blog.csdnimg.cn/64a0284557724fa3a2756db89958d5ba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>这里设置了 阻塞时间,如果是 0 会一直阻塞.如果设置为10 ,则阻塞10秒后会退出阻塞
>127.0.0.1:6379> blpop age  10
(nil)
(10.04s)
127.0.0.1:6379>

但是客户端B一直处于阻塞状态,这里可以看到第一个拿走了,第二个不知道name里面之前有过数据;


还有以b开头的阻塞命令
brpop ,Brpoplpush

 - brpop~略
 - Brpoplpush

Brpoplpush 命令从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。
![在这里插入图片描述](https://img-blog.csdnimg.cn/22c864c40ad841bdad0611fd82f22834.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/5fbe002d88914a4bb2fe397912716f19.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>如果一开始name1不存在,结果是什么?brpoplpush一直处于阻塞状态;


```bash
127.0.0.1:6379> help ltrim

  LTRIM key start stop
  summary: Trim a list to the specified range #删除两边的,保留trim区间的内容,(并不要求对称;)
  since: 1.0.0
  group: list

127.0.0.1:6379> rpush name a s d f g
(integer) 5
127.0.0.1:6379> ltrim name  3 3
OK
127.0.0.1:6379> lrange name 0 -1
1) "f"
```

## Redis中的集合set
这里有几个比较好玩的
在redis中有交集并集和差集

 - **sunion** 并集

```bash
127.0.0.1:6379> help sunion

  SUNION key [key ...]
  summary: Add multiple sets
  since: 1.0.0
  group: set
127.0.0.1:6379> sadd k1 1 2 3 4 5
(integer) 5
127.0.0.1:6379> sadd k2 3 4 5 6 7
(integer) 5
127.0.0.1:6379> sunion k1 k2   #并集
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
7) "7"
```

 - **sinter**交集

```bash
127.0.0.1:6379> help sinter

  SINTER key [key ...]
  summary: Intersect multiple sets
  since: 1.0.0
  group: set

127.0.0.1:6379> sinter  k1 k2
1) "3"
2) "4"
3) "5"
```

 - **sdiff**  差集

```bash
127.0.0.1:6379> help sdiff

  SDIFF key [key ...]
  summary: Subtract multiple sets
  since: 1.0.0
  group: set

127.0.0.1:6379> sdiff k1 k2
1) "1"
2) "2"
```

 - sunionstore ,sinterstore和sdiffstore

```bash
127.0.0.1:6379> sunionstore k3 k1 k2
(integer) 7
127.0.0.1:6379> sscan  k3 0
1) "0"
2) 1) "1"
   2) "2"
   3) "3"
   4) "4"
   5) "5"
   6) "6"
   7) "7"

127.0.0.1:6379> sdiffstore k4 k1 k2
(integer) 2
127.0.0.1:6379> sscan k4  0  
1) "0"
2) 1) "1"
   2) "2"
127.0.0.1:6379> sdiffstore k5 k2 k1
(integer) 2
127.0.0.1:6379> sscan k5  0
1) "0"
2) 1) "6"
   2) "7"
127.0.0.1:6379> sinterstore k6 k1 k2
(integer) 3

127.0.0.1:6379> sscan k6  0
1) "0"
2) 1) "3"
   2) "4"
   3) "5"
```

>注意差集的时候有方向性;

 - **srandmember**随机事件


```bash
127.0.0.1:6379> help srandmember

  SRANDMEMBER key [count]
  summary: Get one or multiple random members from a set
  since: 1.0.0
  group: set
127.0.0.1:6379> srandmember k1 1
1) "1"
127.0.0.1:6379> srandmember k1 2
1) "1"
2) "5"
127.0.0.1:6379> srandmember k1 3
1) "4"
2) "5"
3) "2"
```
有意思的事情是当集合中元素数量不够的时候怎么办?
随机数量位正数~取出一个去重的结果集

```bash
127.0.0.1:6379> srandmember k1 8
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
```
>当取正值的时候,随机出来的元素并不会重复,如果不够则有多少拿多少;


我们取负值~取出一个带重复的结果集,满足取得数量要求;
```bash
127.0.0.1:6379> srandmember k1 -2
1) "1"
2) "5"
127.0.0.1:6379> srandmember k1 -3
1) "1"
2) "3"
3) "5"
127.0.0.1:6379> srandmember k1 -5
1) "2"
2) "1"
3) "1"
4) "2"
5) "3"

127.0.0.1:6379> srandmember k1 0  #0则返回空集合
(empty array)


```
>当取负值的时候是有可能出现重复的值的;

 srandmember的实际应用~抽奖
比如有三个奖品,100个用户,
准备一个集合,并映射好集合元素与用户的关系,

从这100个用户里取3个,
由于用户数大于3
,所以可以`srandmember user   3` 
也可以  `srandmember user   -3`

无非就是中奖能否重复的问题;

但有时候抽奖是抽一个往外拿出一个,
这个时候用srandember就不太合适,应该用

 - **spop**随机弹出

```bash
127.0.0.1:6379> help spop

  SPOP key [count]
  summary: Remove and return one or multiple random members from a set
  since: 1.0.0
  group: set

127.0.0.1:6379> spop k1 3
1) "1"
2) "2"
3) "5"
```

## Redis中的有序集合sorted set
有了set为什么还要有一个有序的set?
>现实中并不中是只能按照一种方式进行排序的，所以在往有序集中添加元素时必须要给出权重(分数)；

比如有一篮子水果，你会按照什么顺序排？
名字？---->>>>>字典顺序
大小？------>>>>>数值
甜度？------>>>>>数值
等等

举个例子~

>权重一样时，按照字典顺序排序

```bash
127.0.0.1:6379> zadd fuit 1 apple
(integer) 1
127.0.0.1:6379> zadd fuit 1 orange
(integer) 1
127.0.0.1:6379> zadd fuit 1 tomato
(integer) 1
127.0.0.1:6379> zadd fuit 1 pear
(integer) 1
127.0.0.1:6379> zadd fuit 1 pine
(integer) 1
127.0.0.1:6379> zrange fuit 0 -1
1) "apple"
2) "orange"
3) "pear"
4) "pine"
5) "tomato"
```

当我们把权重1进行修改后，假设1 表示最甜的等级
，0 表示最不甜的等级

这时候按照权重进行排序
```bash
127.0.0.1:6379> zadd fruit 1 apple
(integer) 1
127.0.0.1:6379> zadd fruit 0.9 orange
(integer) 1
127.0.0.1:6379> zadd fruit 0.8 tomato
(integer) 1
127.0.0.1:6379> zadd fruit 0.7 pear
(integer) 1
127.0.0.1:6379> zadd fruit 0.6 pine
(integer) 1
127.0.0.1:6379> zrange fruit 0 -1
1) "pine"
2) "pear"
3) "tomato"
4) "orange"
5) "apple"
```

如果权重一样~字典顺序排序

```bash
127.0.0.1:6379> zadd fruit  0.6 banana
(integer) 1
127.0.0.1:6379> zrange fruit 0 -1
1) "banana"
2) "pine"
3) "pear"
4) "tomato"
5) "orange"
6) "apple"
```
这里我们也可以看到默认是按照升序排的，

所以sorted Set 有反转排序
`zrevrange`    `zrevrangebyscore`
```bash
127.0.0.1:6379> help zrevrange

  ZREVRANGE key start stop [WITHSCORES]
  summary: Return a range of members in a sorted set, by index, with scores ordered from high to low
  since: 1.2.0
  group: sorted_set

127.0.0.1:6379> zrevrange fruit 0 -1
1) "apple"
2) "orange"
3) "tomato"
4) "pear"
5) "pine"
6) "banana"

127.0.0.1:6379> help zrevrangebyscore

  ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]
  summary: Return a range of members in a sorted set, by score, with scores ordered from high to low
  since: 2.2.0
  group: sorted_set
127.0.0.1:6379> zrevrangebyscore fruit 1 0
1) "apple"
2) "orange"
3) "tomato"
4) "pear"
5) "pine"
6) "banana"
```
其他命令

```bash
127.0.0.1:6379> zrank fruit  apple  #返回元素的index
(integer) 5

127.0.0.1:6379> zrange fruit  2 3  #取出3 和4 两个
1) "pear"
2) "tomato"
127.0.0.1:6379>#如果取出3和4 但是要求3排在在后4 在前
#这样取可以吗?
127.0.0.1:6379> zrange fruit -4 -3
1) "pear"
2) "tomato"
#取出的结果依旧是正向排序的
#通过逆操作可以快速实现
127.0.0.1:6379> zrevrange fruit 2 3
1) "tomato"
2) "pear"

```

这有什么实际应用呢?
**榜单~**
以音乐榜单为例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/8083ad9e013c44c7b7443882ce8da56e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
排行榜实时在变化,我们就可以根据一个分数(可以按照多个维度去计算一个综合分数),
当一首歌的分数变化时,我们可以更新该分数或者在原来分数的基础上及进行加减操作;


```bash
127.0.0.1:6379> zadd music 100 ipad
(integer) 1
127.0.0.1:6379> zadd music  98 "love we lost"
(integer) 1
127.0.0.1:6379> zadd music  97 "what would you do"
(integer) 1
```
取喜力电音榜 前十的歌曲

```bash
127.0.0.1:6379> zrevrange music 0 10
1) "ipad"
2) "love we lost"
3) "what would you do"
127.0.0.1:6379>
```
后来榜单发生了变化,第二名的歌曲穿到到第一去了

```bash
127.0.0.1:6379> zrevrange music 0 10
1) "what would you do"
2) "ipad"
3) "love we lost"
127.0.0.1:6379>
```
>这种榜单会实时变化,而且redis是单进程的,不用考虑高并发的情况,不同的人的操作会按照顺序由单进程执行;

**跟set一样sortedset也支持交并差集的操作**

 - **zunionstore**取交集并将结果存在一个集合中
help
```bash
127.0.0.1:6379> help zunionstore

  ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight] [AGGREGATE SUM|MIN|MAX]
  summary: Add multiple sorted sets and store the resulting sorted set in a new key
  since: 2.0.0
  group: sorted_set

127.0.0.1:6379> zadd funmusic 100  取经路
(integer) 1
127.0.0.1:6379> zadd funmusic 98 岸
(integer) 1
127.0.0.1:6379> zadd funmusic 98 岸 95 陪你
(integer) 1
#跟music取并集
127.0.0.1:6379> zunionstore sortedmusic 2 funmusic music
(integer) 6
127.0.0.1:6379> zrange sortedmusic 0 -1
1) "\xe9\x99\xaa\xe4\xbd\xa0"
2) "love we lost"
3) "\xe5\xb2\xb8"
4) "ipad"
5) "\xe5\x8f\x96\xe7\xbb\x8f\xe8\xb7\xaf"
6) "what would you do"
```
如果集合中重名的怎么办?

```bash
127.0.0.1:6379> zadd funmusic 200 ipad
(integer) 1
127.0.0.1:6379> zunionstore sortedmusic 2 funmusic music
(integer) 6

127.0.0.1:6379> zrange sortedmusic 0 -1 withscores
 1) "\xe9\x99\xaa\xe4\xbd\xa0"
 2) "95"
 3) "love we lost"
 4) "98"
 5) "\xe5\xb2\xb8"
 6) "98"
 7) "\xe5\x8f\x96\xe7\xbb\x8f\xe8\xb7\xaf"
 8) "100"
 9) "what would you do"
10) "103"
11) "ipad"
12) "300"  #此时会合并,并将分数相加

127.0.0.1:6379> zunionstore sortedmusic2 2 funmusic1 music  #与一个不存在的集合(此时默认为空集)取交集
(integer) 3
127.0.0.1:6379> zrange sortedmusic2 0 -1 withscores
1) "love we lost"
2) "98"
3) "ipad"
4) "100"
5) "what would you do"
6) "103"
```
并集的时候带上权重会是什么样的结果呢?

>**带权重的话,回将分数乘以权重再相加**
```bash
127.0.0.1:6379> zunionstore my 2 music funmusic weights 1 0.5
(integer) 7
127.0.0.1:6379> zrange my 0 -1 withscores
 1) "\xe9\x99\xaa\xe4\xbd\xa0"
 2) "47.5"
 3) "\xe5\xb2\xb8"
 4) "49"
 5) "\xe5\x8f\x96\xe7\xbb\x8f\xe8\xb7\xaf"
 6) "50"
 7) "iPad"
 8) "60"
 9) "love we lost"
10) "98"
11) "what would you do"
12) "103"
13) "ipad"
14) "200"

```
并集中如果有重复的元素,可以取最大值(最小值甚至求和(默认))

当然也可以有很多玩法,比如取交集的时候保留一个


```bash
127.0.0.1:6379> zunionstore fun 2 music funmusic aggregate max
(integer) 6
127.0.0.1:6379> zrange fun 0 -1 withscores
 1) "\xe9\x99\xaa\xe4\xbd\xa0"
 2) "95"
 3) "love we lost"
 4) "98"
 5) "\xe5\xb2\xb8"
 6) "98"
 7) "\xe5\x8f\x96\xe7\xbb\x8f\xe8\xb7\xaf"
 8) "100"
 9) "what would you do"
10) "103"
11) "ipad"
12) "200"#这里取最大的一个

```

说完并集,在说交集

```bash
127.0.0.1:6379> zinterstore mymusic 2 funmusic music
(integer) 1
127.0.0.1:6379> zrange mymusic 0 -1
1) "ipad"
127.0.0.1:6379> zrange mymusic 0 -1 withscores
1) "ipad"
2) "300"  #此时交集会将分数相加
```

## Redis的I/O问题
我们知道redis是单线程的,当我们连接客户端的时候,每执行一个命令就会发生一次IO,当数据量/并发数很多的时候,会有阻塞情况发生,这样给人的感觉就很慢;

这个问题其实跟sql中执行批量urd一样,redis中提供了管道来进行批量提交;
在这之前我们先看一下外部的一个工具~~nc

执行`yum install nc `
```bash
======================================================================= Package        Architecture Version               Repository     Size
=======================================================================Installing:
 netcat         x86_64       1.218-4.fc35          updates        34 k
Installing dependencies:
 libbsd         x86_64       0.10.0-8.fc35         fedora        104 k
 libretls       x86_64       3.5.0-1.fc35          updates        42 k

Transaction Summary
=======================================================================Install  3 Packages
```
安装之后

连接redis客户端

```bash
[root@Gavin bin]# nc localhost 6379
keys *
*0
set name lisi
+OK    #ok 表示执行set 成功

keys *    #该命令表示查询key的信息
*1       # key的数量
$4       # value 的长度       
name     #value 的值   

```
redis客户端验证
```bash
127.0.0.1:6379> get name
"lisi"
127.0.0.1:6379>
```

批量提交数据~

```bash
[root@Gavin bin]# echo -e "set num 99\n incr num\n get num\n" |nc localhost 6379
+OK
:100
$3
100

```
每个命令之间要换行,如果是在win中执行换行符为 \r\n

```bash
127.0.0.1:6379> get num
"100"
127.0.0.1:6379>
```

redis中支持了pipe管道操作
先准备一个数据文件用于存放redis的批量命令

![在这里插入图片描述](https://img-blog.csdnimg.cn/59b2c18b00f840f9a95f3fb5834a2a78.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_18,color_FFFFFF,t_70,g_se,x_16)

```bash
[root@Gavin /]# cat /mnt/d/redisdemo.txt  | /usr/local/redis/bin/redis-cli --pipe
All data transferred. Waiting for the last reply...
Last reply received from server.
errors: 0, replies: 5
```

使用nc 检验一下

```bash
[root@Gavin /]# nc localhost 6379
keys *
*6
$3
ABC
$4
soft
$3
num
$5
hello
$4
name
$6
person

```

## Redis中的消息发布/订阅

![在这里插入图片描述](https://img-blog.csdnimg.cn/0bbe589ee00f4ee99a743b4e42fe02ba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **publish向频道发布消息**
>Publish 命令用于将信息发送到指定的频道
```bash
127.0.0.1:6379> help publish

  PUBLISH channel message
  summary: Post a message to a channel
  since: 2.0.0
  group: pubsub
127.0.0.1:6379> publish mychannel "hello World"
(integer) 0 #返回值为订阅该频道的数

```

 - **subscribe 订阅频道**

>Subscribe 命令用于订阅给定的一个或多个频道的信息。

```bash
127.0.0.1:6379> help subscribe

  SUBSCRIBE channel [channel ...]
  summary: Listen for messages published to the given channels
  since: 2.0.0
  group: pubsub

#订阅后的返回值为发布的信息
127.0.0.1:6379> subscribe mychannel
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "mychannel"
3) (integer) 1
1) "message"
2) "mychannel"
3) "hello"
1) "message"
2) "mychannel"
3) "\xe4\xbd\xa0\xe5\xa5\xbd\xe5\x91\x80"
1) "message"
2) "mychannel"
3) "hello world"

```

 - **unsubscribe退订频道**
>Unsubscribe 命令用于退订给定的一个或多个频道的信息。

```bash
127.0.0.1:6379> help UNSUBSCRIBE

  UNSUBSCRIBE [channel [channel ...]]
  summary: Stop listening for messages posted to the given channels
  since: 2.0.0
  group: pubsub

[root@Gavin bin]# nc localhost 6379
subscribe mychannel
*3
$9
subscribe
$9
mychannel
:1
*3
$7
message
$9
mychannel
$5
hello
unsubscribe mychannel
*3
$11
unsubscribe
$9
mychannel
:0
publish mychannel hello
:0   #暂时没有人订阅

```

>当发布消息时没有人订阅,那么消息就会进入黑洞(并不会存储在服务器上);

 - **psubscribe订阅给定模式的频道**
>psubscribe 命令订阅一个或多个符合给定模式的频道。


```bash
127.0.0.1:6379> help psubscribe

  PSUBSCRIBE pattern [pattern ...]
  summary: Listen for messages published to channels matching the given patterns
  since: 2.0.0
  group: pubsub
# 发布消息
127.0.0.1:6379> publish  ChinaNews news
(integer) 0
127.0.0.1:6379> publish  USANews news
(integer) 0
127.0.0.1:6379> publish  UKLNews news
(integer) 0
127.0.0.1:6379> publish  RussiaNews news
(integer) 0
127.0.0.1:6379> publish  UKNews news
(integer) 0

#fabuxiaoxi..........
127.0.0.1:6379> publish  UKLNews news
(integer) 1

127.0.0.1:6379> publish  UKLNews helloworld
(integer) 1
```
另一个客户端~

```bash
[root@Gavin bin]# nc localhost 6379
psubscribe *KL*
*3
$10
psubscribe
$4
*KL*
:1
*4
$8
#此时 UKLNews 频道已经发布消息
pmessage
$4
*KL*
$7
UKLNews
$4
news
$10
helloworld
```

>每个模式以 * 作为匹配符，比如 it* 匹配所有以 it 开头的频道( it.news 、 it.blog 、 it.tweets 等等)。 news.* 匹配所有以 news. 开头的频道( news.it 、 news.global.today 等等)，诸如此类。

 - **punsubscribe批量退定**

用法跟 psubscribe一样
```bash
  PUNSUBSCRIBE [pattern [pattern ...]]
  summary: Stop listening for messages posted to channels matching the given patterns
  since: 2.0.0
  group: pubsub

punsubscribe
$4
*KL*
:0


```
另一个客户端发布消息,返回订阅者数为0

```bash
127.0.0.1:6379> publish  UKLNews helloworld
(integer) 0
```

 - **pubsub查看订阅与发布系统状态**

```bash
127.0.0.1:6379> help pubsub

  PUBSUB subcommand [argument [argument ...]]
  summary: Inspect the state of the Pub/Sub subsystem
  since: 2.8.0
  group: pubsub
```

**PUBSUB CHANNELS ~ 列出当前活跃的频道**
>**这里不算 模式订阅的客户端**
>
```bash
127.0.0.1:6379> PUBSUB CHANNELS
(empty array)
```

>语法~ pubsub channels [pattern]列出当前的**活跃频道**。

活跃频道指的是那些至少有一个订阅者的频道， 订阅模式的客户端不计算在内。

pattern 参数是可选的：

如果不给出 pattern 参数，那么列出订阅与发布系统中的所有活跃频道。
如果给出 pattern 参数，那么只列出和给定模式 pattern 相匹配的那些活跃频道。
复杂度： O(N) ， N 为活跃频道的数量（对于长度较短的频道和模式来说，将进行模式匹配的复杂度视为常数）。

返回值： 一个由活跃频道组成的列表。

例如  模式订阅下
```bash
...........
psubscribe *UK*
*3
$10
psubscribe
$4
*UK*
:1
*4
$8
pmessage
$4
*UK*
$7
UKLNews
$4
news
#......开始订阅频道订阅
subscribe note
*3
$9
subscribe
$4
note
:2
*3
$7
message 
$4
note
$4
book #发布的消息
```
发布消息
```bash
[zzy@Gavin bin]$  nc localhost 6379
publish UKLNews news
:1
#.....之后新建频道发布消息
publish note book
:0  #暂时没有订阅者
publish note book
:1  #有订阅者了

```
查看活跃频道

```bash
#订阅模式下
127.0.0.1:6379> PUBSUB CHANNELS
(empty array)
#....再次查看活跃频道
127.0.0.1:6379> PUBSUB CHANNELS
1) "note"#一个由活跃频道组成的列表。

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/ecf271216e124019be03deea6cf18792.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **PUBSUB numsub [channel-1 ... channel-N]**

> 返回给定频道的订阅者数量， 订阅模式的客户端不计算在内。

**复杂度**： O(N) ， N 为给定频道的数量。
**返回值**： 一个多条批量回复（Multi-bulk reply），回复中包含给定的频道，以及频道的订阅者数量。 格式为：频道 channel-1 ， channel-1 的订阅者数量，频道 channel-2 ， channel-2 的订阅者数量，诸如此类。 回复中频道的排列顺序和执行命令时给定频道的排列顺序一致。 不给定任何频道而直接调用这个命令也是可以的， 在这种情况下， 命令只返回一个空列表。

```bash
#列出频道订阅数  模式订阅不算在内
127.0.0.1:6379>  PUBSUB numsub UKNews  
#参数只能是频道
1) "UKNews"
2) (integer) 0
127.0.0.1:6379> pubsub numsub
(empty array)
#不给定任何频道而直接调用这个命令也是可以的， 在这种情况下， 命令只返回一个空列表。

```

 - **PUBSUB NUMPAT返回订阅模式的数量。**

注意， 这个命令返回的不是订阅模式的客户端的数量， 而是客户端订阅的所有模式的数量总和。

复杂度： O(1) 。

返回值： 一个整数回复（Integer reply）。
```bash
127.0.0.1:6379>  PUBSUB numpat   #返回订阅模式的数量。
(integer) 0

#参数可以是模式,也可以是频道名
```

redis中消息发布与订阅有什么实际应用呢?
我们在网页中常常看到~~直播间,直播间消息快速刷新
![在这里插入图片描述](https://img-blog.csdnimg.cn/5cfec07560dc4bbc84269f466772e346.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
这里可以用redis实现~redis当用户登录直播平台并进入主播直播间时就订阅了一个resdis的频道 subscribe,同时用户发布消息时就会触发 publish  channel  message


![在这里插入图片描述](https://img-blog.csdnimg.cn/494a721e3dbd4f60a9b9855abba95200.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
发送过来消息如果想要保存三天的聊天记录 可以将时间作为key,消息作为value,稍微长一点的数据可以持久化到数据库中;

```bash
127.0.0.1:6379> zadd 20220402 2317 hello  #zadd 日期   时间 value
(integer) 1
```
我们在查看数据的时候

```bash
127.0.0.1:6379> zrange 20220402 0 -1
1) "hello"
2) "helloworld"
127.0.0.1:6379> zrangebyscore 20220402 0 2359
1) "hello"
2) "helloworld"
```

## Redis中的事务

在redis中也支持事务

![在这里插入图片描述](https://img-blog.csdnimg.cn/ebfcad433ad64c3b8bb146e4b701dbe1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **multi**

用于标记一个事务块的开始。

事务块内的多条命令会按照先后顺序被放进一个队列当中，最后由 EXEC 命令**原子性(atomic)**地执行。



 - **exec**

用于执行所有事务块内的命令。



```bash
127.0.0.1:6379> multi    # 标记事务开始
OK
127.0.0.1:6379> set num 1
QUEUED  # 多条命令按顺序入队
127.0.0.1:6379> incr num
QUEUED
127.0.0.1:6379> incr num
QUEUED
127.0.0.1:6379> type num
QUEUED
127.0.0.1:6379> object encoding num
QUEUED
127.0.0.1:6379> exec  #执行
1) OK   
2) (integer) 2
3) (integer) 3
4) string
5) "int"
```
**事务的体现**

 - **Watch**

Watch 命令 - 监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。

redis客户端 A
```bash
127.0.0.1:6379> watch num
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> incr num
QUEUED
127.0.0.1:6379> incr num
QUEUED
127.0.0.1:6379> incrby num 10
QUEUED
127.0.0.1:6379> exec
(nil)
127.0.0.1:6379> get num
"100"
127.0.0.1:6379>
```
另一个Redis客户端B在A没有提交执行时将num的值改了

![在这里插入图片描述](https://img-blog.csdnimg.cn/5e37bdea3df349e0b86a83da4087f91c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
redis客户端A 此时在执行exec,得到的结果为 nil
-->这时事务被取消了,前提是通过watch监视 key
```bash
127.0.0.1:6379> exec
(nil)
```

我们都知道redis是单进程,执行的时候一定会按照先后顺序执行,谁先到达就让谁先执行;

![在这里插入图片描述](https://img-blog.csdnimg.cn/cd286fd27f98441f85ea247bb09d56d5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```bash
127.0.0.1:6379> del age
(integer) 0
127.0.0.1:6379> del age
(integer) 1
```

 - **Discard**

用于取消事务，放弃执行事务块内的所有命令。

```bash
127.0.0.1:6379> watch num
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set name lisi
QUEUED
127.0.0.1:6379> discard
OK
```

 - **Unwatch **

用于取消 WATCH 命令对所有 key 的监视。



>**redis中的事务只支持撤销,并不支持回滚**

## Redis的模块

![在这里插入图片描述](https://img-blog.csdnimg.cn/2e77b0063bbe4edf81f55809585799e6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



>Redis 模块可以使用外部模块扩展 Redis 功能，快速实现新的 Redis 命令，其功能类似于内核内部可以执行的功能。

>Redis 模块是动态库，可以在启动时或使用 MODULE LOAD 命令加载到 Redis 中。Redis 以名为 的单个 C 头文件的形式导出 C API。

[Redis模块预编译下载~企业版](https://redis.com/redis-enterprise-software/download-center/modules/)


![在这里插入图片描述](https://img-blog.csdnimg.cn/8345e0121b5d48e99e30e706584b4a13.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>预编译模块linux系统版本比较少,如果不是这里的系统,还是自己在本地编译吧!

github上都是未编译的,可以下载下来自己编译;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a874fe57d4e94101838bc483596b8639.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>RediSearch 是一个 Redis 模块，为 Redis 提供**查询、二级索引和全文搜索**

>要使用 RediSearch，请先在 Redis 数据上声明索引。然后，您可以使用 RediSearch 查询语言来查询该数据。
RediSearch 使用**压缩的倒排索引**，以较低的内存占用量快速编制索引。

>RediSearch 索引通过提供精确短语匹配、模糊搜索和数字过滤以及许多其他功能来增强 Redis,但是redis对中文不太友好,但也有用武之地

redisSearch模块暂不演示 ----[仓库地址](https://github.com/RediSearch/RediSearch)

### RedisBloom入门
linux环境   

```bash
[root@Gavin local]# cat /proc/version
Linux version 5.10.102.1-microsoft-standard-WSL2 (oe-user@oe-host) (x86_64-msft-linux-gcc (GCC) 9.3.0, GNU ld (GNU Binutils) 2.34.0.20200220) #1 SMP Wed Mar 2 00:30:59 UTC 2022
[root@Gavin local]#
```

[从git上获取源码--](https://github.com/RedisBloom/RedisBloom.git)选择2.2版本

RedisBloom 模块提供四种数据结构：可扩展的 **Bloom 过滤器、布谷鸟过滤器、计数分钟草图和 top-k**。这些数据结构以完美的准确性换取了极高的内存效率，因此它们对于大数据和流应用程序特别有用。

Bloom 和 cuckoo 过滤器用于高度确定元素是否是集合的成员。

计数-分钟草图通常用于确定流中事件的频率。您可以查询计数-分钟草图，获取任何给定事件频率的估计值。

下载之后编译`make`

编译之前
![在这里插入图片描述](https://img-blog.csdnimg.cn/1dfc0ef39ab04baf8ee54f75ed9d7bc0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

编译之后多了个`redisbloom.so`

![在这里插入图片描述](https://img-blog.csdnimg.cn/87a9f82e08eb49148718ab1ce298fa14.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

布隆过滤器可以用于检索一个元素是否在一个集合中。它可以告诉你某种东西**一定不存在或者可能存在**。

当布隆过滤器说，某种东西存在时，这种东西可能不存在；当布隆过滤器说，某种东西不存在时，那么这种东西一定不存在。

布隆过滤器 
优点：A. 空间效率高，占用空间少 B. 查询时间短

 缺点：A. 有一定的误判率  B. 元素不能删除

 - **启动redis加载模块**
**两种方式**
1,在启动时加载模块

```bash
redis-server  -loadmodule /path/to/mymodule.so  redis.conf
```
2,启动后加载模块

```bash
[root@Gavin bin]# ./redis-server  redis.conf
26091:C 04 Apr 2022 22:41:59.959 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
26091:C 04 Apr 2022 22:41:59.959 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=26091, just started
26091:C 04 Apr 2022 22:41:59.959 # Configuration loaded
[root@Gavin bin]# ./redis-cli
127.0.0.1:6379> module load /usr/local/RedisBloom-2.2/redisbloom.so
OK
```

 - **列出所有加载的模块**

```bash
127.0.0.1:6379> module list
1) 1) "name"
   2) "bf"
   3) "ver"
   4) (integer) 20214
127.0.0.1:6379>
```

 - **使用 RedisBloom 与redis-cli**

    **bf.add命令如果过滤器没有创建的时候，会自动创建，并且使用的默认参数**
```bash
127.0.0.1:6379> bf.add newFilter foo  #添加过滤词
(integer) 1  

127.0.0.1:6379> bf.exists newFilter foo #检查是否存在 相关词
(integer) 1
127.0.0.1:6379> bf.exists newFilter fool
(integer) 0

#多个一起操作
127.0.0.1:6379> bf.madd newfilter html python
1) (integer) 1
2) (integer) 1
127.0.0.1:6379> bf.mexists myfilter 1 2 3
1) (integer) 0
2) (integer) 0
3) (integer) 0
127.0.0.1:6379>
```

**配置期望错误率和初始容量**
bf.reserve指令，创建一个自定义过滤器，有三个参数, 
**filterName**： 过滤器名称
**error_rate**: 期望错误率，期望错误率越低，需要的空间就越大
 **capacity**：初始容量，当实际元素的数量超过这个初始容量的时候，误判率会上升。

创建
```bash
127.0.0.1:6379> bf.reserve myfilter1 0.3  1000
OK
127.0.0.1:6379> bf.reserve myfilter1 0.3  1000
(error) ERR item exists
127.0.0.1:6379>

```
>布隆过滤器的**error_rate越小**，需要的**存储空间就越大**，对于不需要过于精确的场景，error_rate设置稍大一点也可以。布隆过滤器的**capacity设置的过大**，会**浪费存储空间**，设置的**过小**，就会**影响准确率**，所以在使用之前一定要尽可能地精确估计好元素数量，还需要加上一定的冗余空间以避免实际元素可能会意外高出设置值很多。总之，error_rate和 capacity都需要设置一个合适的数值。




然后再开一个redis,端口为6380


```bash
127.0.0.1:6380> bf.add newfilter app
(error) ERR unknown command `bf.add`, with args beginning with: `newfilter`, `app`,
127.0.0.1:6380>

```
没有bf相关命令

### RedisBloom的应用

**redisbloom会解决一个问题------缓存穿透**

>业务请求中数据缓存中没有，DB中也没有，导致类似请求直接跨过缓存，反复在DB中查询，与此同时缓存也不会得到更新;


比如一家完全不涉及业务A的公司,用户在公司网站上一直搜,首先会从内存中找,内存没有往数据库中找,数据库中也没有,如果用户一直这样操作....

那有了redisbloom,将可以放在过滤器中,如果bloom过滤器中没有,那么数据一定不存在,如果过滤器中有,数据有可能也不存在;这是一种不影响性能的解决方案;

**黑名单校验**

将发送者地址作为key,只要发送者在黑名单中,就识别为垃圾邮件;

**web拦截器**

如果相同请求则拦截，防止重复被攻击。

用户第一次请求，将请求参数放入布隆过滤器中，当第二次请求时，先判断请求参数是否被布隆过滤器命中，从而提高缓存命中率。 
