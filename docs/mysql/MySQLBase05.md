![](..\pic\div\plant.gif)



# mysql中的事务
## 事务定义

> **事务（Transaction）是用来维护数据库完整性的，它能够保证一系列的MySQL操作要么全部执行，要么全不执行。**

简单来讲就是几个相关联的操作(各关联之间的操作相互影响各自数据的结果)要么全部执行完,要么都不要执行;

**> 在现实生活中有一个很常用的案例-----

> **转账操作的要求--- 转账操作要保证转账端转账200元即余额减少200同时接收端要进行账户余额增加200的操作,**


 转账时理论中可能会出现以下几种情况---
(假设转账成功余额增加,转账失败余额减少)
> **1,转账端转账200元失败,接收端收到了转账款200元----转账端账户余额不变,接收端余额增加200;
> 2,转账端转账200元失败,接收端没有收到转账款200元----转账端账户余额不变,接收端余额不变;
> 3,转账端转账200元成功,接收端没有收到转帐款200元,转账端账户余额减少200,接收端余额不变; 
> 4,转账端转账200元成功,接收端收到了转帐款200元,转账端账户余额减少200,接收端余额增加200;;**

我们的需求不言而喻-----第二种和第四种情况符合我们的要求

当然有时候情况很多复杂,比如网上购物涉及到
购物者账户余额,商家账户余额,商家库存记录,订单记录等相关操作,这些操作要么全部被执行,要么就全不执行;
## mysql中条件处理与事务处理
### 条件处理---

> **特定条件需要特定处理。这些条件可以联系到错误以及子程序中的一般流程控制。定义条件是事先定义程序执行过程中遇到的问题，处理程序定义了在遇到这些问题时应当采取的处理方式，并且保证存储过程或函数在遇到警告或错误时能继续执行。这样可以增强存储程序处理问题的能力，避免程序异常停止运行。**

### 事务处理---

> **事务**（Transaction）指的是一个操作序列，该操作序列中的**多个操作要么都做**，**要么都不做**，
>
> **目前常用的存储引擎InnoDB支持事务处理机制，而MyISAM不支持。**

**事务特性**

原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability）。这四个特性简称为ACID特性。

**原子性----**

> **事务中的所有操作是不可再分的最小的逻辑执行体。事务对数据进行修改的操作序列，要么全部执行，要么全不执行。通常，某个事务中的操作都具有共同的目标，并且是相互依赖的。如果数据库系统只执行这些操作中的一部分，则可能会破坏事务的总体目标，而原子性消除了系统只处理部分操作的可能性。**

**一致性----**

事务执行的结果必须使数据库从一个一致性状态，变到另一个一致性状态。我减少,你增加,但是总额是不变的,一致性是通过原子性来保证的。

> **例如：在买米时，只有保证买的斤数和卖的斤数一致才能构成完整的构成此笔交易。也就是说我去超市买大米之前超市的大米有100斤,我买了10斤,大米超市还剩90斤,数据的总额都是100,即数据是匹配的。**

**隔离性----**

隔离性是指各个事务的执行互不干扰，任意一个事务的内部操作对其他并发的事务，都是隔离的。也就是说：并发执行的事务之间既不能看到对方的中间状态，也不能相互影响。

> **例如：超市有100斤米，我买10斤米,那超市大米减少10斤,我持有的大米增加了10斤,同时张三也去买米了,但他并不影响我买10斤大米这个交易,同时也并不影响我买大米导致超市大米减少10斤这个事实;**

**即其他的事务对于我买米产生的交易是不能产生任何影响的。**

**持久性----**

事务一旦提交，对数据所做的任何改变，都要记录到永久存储器中，通常是保存进物理数据库，即使数据库出现故障，提交的数据也应该能够恢复。但如果是由于外部原因导致的数据库故障，如硬盘被损坏，那么之前提交的数据则有可能会丢失。

> **例如:还是去超市买大米,当交易完成超市会给你单据,将该笔记录写入到超市的收银系统中,以方便日后公司对账和消费者维权**
**mysql 中的具体事务是指哪些?**

***默认一个dml语句是一个事务***
```sql
-- 建表,插入数据
create table acount(
id int PRIMARY key auto_increment,
uname varchar(8) not null,
balance double 
);
insert into acount values(null,'小丽',5000);
insert into acount values (null,'小刚',3000);
insert into acount values (null,'小花',8000);
```
**事务处理案例----**

模拟转账效果----

建立一个正常转账的过程-----
```sql
--- 建立一个存储过程----

-- 创建小丽给小刚转账的事务,没有异常来打断,
CREATE PROCEDURE LIzhuan (
IN Zid INT,
IN Zname VARCHAR ( 12 ),
IN Sid INT,
IN Sname VARCHAR ( 12 ),
IN num DOUBLE,
OUT result VARCHAR ( 20 ) 

) -- Zid转账账号,Zid转账姓名,Sid收款账号 ,Sname 收款姓名,num转账的金额
MODIFIES SQL DATA --  指定过程特性
BEGIN
IF
	Zid = 1 and Sid = 2 
	AND Sname = '小刚' AND Zname = '小丽' THEN-- 开始转账
	UPDATE acount 
	SET balance = balance - num 
WHERE
	id = Zid;
		UPDATE acount 
	SET balance = balance + num where id=Sid ;
else
select '转账失败' into result;
END IF;
SELECT
	* 
FROM
	acount;
SELECT 	'转账成功' INTO result;
END;

CALL Lizhuan (1,'小丽',2,'小刚', 200, @result);
select @result ;
```
**条件处理案例----**

建立一个有异常出现的转账过程----

```sql

-- 创建小丽给小刚转账的事务,有异常来打断
CREATE PROCEDURE LIzhuan1 (
IN Zid INT,
IN Zname VARCHAR ( 12 ),
IN Sid INT,
IN Sname VARCHAR ( 12 ),
IN num DOUBLE,
OUT result VARCHAR ( 20 ) 

) -- Zid转账账号,Zid转账姓名,Sid收款账号 ,Sname 收款姓名,num转账的金额
MODIFIES SQL DATA --  指定过程特性
BEGIN
declare mycondition condition for 1062 ;-- 重复异常
declare exit handler for mycondition  set @info='异常退出'; -- 在mysql中不支持undo处理方式,

IF
	Zid = 1 and Sid = 2 
	AND Sname = '小刚' AND Zname = '小丽' THEN-- 开始转账
	UPDATE acount 
	SET balance = balance - num 
WHERE
	id = Zid;
-- 加入异常---	
	insert into acount values(3,'小红',7000);
		UPDATE acount 
	SET balance = balance + num where id=Sid ;
else
select '转账失败' into result;
END IF;
SELECT
	* 
FROM
	acount;
SELECT 	'转账成功' INTO result;
END;
> Affected rows: 0
> 时间: 0.007s
--
-- 调用过程

CALL Lizhuan1 (1,'小丽',2,'小刚', 200, @result);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ea75cb249b6249bbb5a5d07ec0f37a1b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)所以上述方式并不满足事务的概念,那么该怎么让其有关联成为一个事务中的操作呢?----(上述操作为了加深存储过程操作的熟练度而写)

## 事务的开启,回滚,提交

**正常操作之后的回滚-----**

```sql
mysql> start transaction ;-- 开启事务 (在这之前已修改会初始的数据)
Query OK, 0 rows affected (0.00 sec)
--调用正常的存储过程----- 结果--
mysql> call lizhuan(1,'小丽',2,'小刚',200,@result);
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    4800 |
|  2 | 小刚  |    3200 |
|  3 | 小花  |    8000 |
+----+-------+---------+
3 rows in set (0.00 sec)
Query OK, 1 row affected (0.01 sec)
-- 回滚操作之后在查询表的信息--
mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from acount;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
|  2 | 小刚  |    3000 |
|  3 | 小花  |    8000 |
+----+-------+---------+
3 rows in set (0.00 sec)

```
**有异常时的回滚--**

```sql

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> call lizhuan1(1,'小丽',2,'小刚',200,@result);
Query OK, 0 rows affected (0.00 sec)

mysql> select * from acount;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    4800 |
|  2 | 小刚  |    3000 |
|  3 | 小花  |    8000 |
+----+-------+---------+
3 rows in set (0.00 sec)

mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from acount;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
|  2 | 小刚  |    3000 |
|  3 | 小花  |    8000 |
+----+-------+---------+
3 rows in set (0.00 sec)

mysql>
```

**----提交操作**

```sql
mysql> start transaction ;-- 开启事务 (在这之前已修改会初始的数据)
Query OK, 0 rows affected (0.00 sec)
--调用正常的存储过程----- 结果--
mysql> call lizhuan(1,'小丽',2,'小刚',200,@result);
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    4800 |
|  2 | 小刚  |    3200 |
|  3 | 小花  |    8000 |
+----+-------+---------+
3 rows in set (0.00 sec)
Query OK, 1 row affected (0.01 sec)


```
---- 这时用别的客户端来查看acount表数据![在这里插入图片描述](https://img-blog.csdnimg.cn/b90cfde97d3f444eadf02689b6664628.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)因为没提交,所以还没修改该数据;

提交-----

```sql
COMMIT;
select * from acount;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/1688aa0ffea848acb1a8f7ec7c4b28bf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)数据已修改;


在事务提交之前,操作的数据缓存的是缓存的数据,在提交之后才会将数据写入到磁盘之中;如果回滚,则也不会对磁盘中的数据产生修改;

## 高并发下的事务处理----
关于并发的问题
说到并发,就不得不说线程之间产生的一些小问题:

有以下这么几个概念---

**脏读----**

> 当一个事务正在访问数据并且对数据进行了修改，而这种修改还没有提交到数据库中，这时另外一个事务也访问了这个数据，然后使用了这个数据。因为这个数据是还没有提交的数据，那么另外一个事务读到的这个数据是“脏数据”，依据“脏数据”所做的操作可能是不正确的。

**简单来说就是一个事务读取了另一个事务还没提交时的数据;**


举个例子就是----
假设没有锁等保证数据的安全的前提下-----

小明要查询自己的银行账户,将需求发给银行的服务器,----->>>
服务器执行指令后查到银行小明账户里面有100块钱,------>>>>
还没等将结果返回到小明所在的客户端,服务器的CPU资源被另一个事务给抢走了(小刚往小明账户里转账100)-------->小刚往小明账户里转账100元执行之后,------>
线程又被小明的查询操作给抢走了,这时候缓存中存放的是'小明账户余额为200元'------->>>>>>>
狗血的剧情是小刚后悔了,撤回了对小明的转账,但是服务器这时候已经将信息返回给小明了,但对于小明得到的数据是不准确的,因此产生了脏读现象;

简化图是-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/1b29e2b3df344b86a0c8347b1e195cf9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**不可重复读----**

> （Unrepeatableread）:指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的数据是不一样的情况，因此称为不可重复读。

**简单来将就是一个事务在多次读取同一个数据但是出现结果不一致的现象;**
还是上面那个例子,这次时小刚确认转账了,当小明在小刚提交转账确认操作之前再次查询自己的账户余额时(假设没有别的事务来干预了),账户余额为100,但是在小刚提交确认转账之后查询结果为200;
简单的图解---
![在这里插入图片描述](https://img-blog.csdnimg.cn/62ba8ba06b4d4372960c31804fec8202.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**幻读----**

> （Phantom read）:幻读与不可重复读类似。它发生在一个事务（T1）读取了几行数据，接着另一个并发事务（T2）插入了一些数据时。在随后的查询中，第一个事务（T1）就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。

简单的就是一个事务A在读取多条记录时,另一个事务B并发的插入了一些数据,恰巧B插入的数据中的某些数据也满足事务A读取记录的条件,导致多次读取的数据记录数不一样;

举个例子----
有这么一个事务A,想要查询记录中id小于10的记录,
第一次查询到5条记录,这时事务B发起了操作,插入了id<10的2条记录,并提交了事务,当事务A 再次查询时,发现前后查询的记录条数不一致了;

简化图是----
![在这里插入图片描述](https://img-blog.csdnimg.cn/af51fc000e564e3abb672c214fa8b307.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
那怎么解决呢?


> 其实跟java中锁的原理是一样的;
> **对于脏读与不可重复读-----因为读取的是一条记录,所以用行锁就可以了;**
>
> **对于幻读,因为是读取的多条记录,所以要将整个表给锁住------用表锁;**


那我们就加锁不就完了,但是mysql中加锁不是我们要做的事情(因为底层源码给我们做加锁的操作针对不同的问题设置了不同的隔离级别),我们要做的是给事务设置合适的隔离级别就可以了;

## 隔离级别
事物的隔离级别有四种,

**READ UNCOMMITTED ---- 读未提交**

这种方式其实解决不了上面的任何一个问题,
因为仍然是可以读取未提交的数据,脏读\幻读\不可重复读问题没有得到解决;

> **但是这种隔离方式并非一无是处, 如果不涉及高并发,这种隔离方式的执行效率是最高的;**

**READ COMMITTED ---- 读已提交**

> **这种方式解决了脏读问题,因为读的都是已提交后的数据,但是没有解决不可重复读和幻读的问题;**



**REPEATABLE READ  ----可重复读**

**根据名字就可以知道,这种方式解决了不可重复读,同时也解决了脏读的问题,但没有解决幻读;**

考虑到性能和高并发,这是mysql默认的隔离级别;

**SERIALIZABLE  ----序列化**

>  **这种方式解决了脏读\幻读\不可重复读,但是这种方式效率极低;**


 查看mysql默认隔离级别;

```sql

mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
1 row in set (0.00 sec)
```
## 自定义事务隔离级别----
**自定义全局事务隔离级别**

```sql

mysql> set  @@transaction_isolation ='SERIALIZABLE';
Query OK, 0 rows affected (0.02 sec)

mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
1 row in set (0.00 sec)
```
**自定义当前事务隔离级别**

```sql

mysql> set session transaction isolation level read committed;
Query OK, 0 rows affected (0.00 sec)

mysql> select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| READ-COMMITTED          |
+-------------------------+
1 row in set (0.00 sec)
```

接着退出mysql服务器,再重新进入查询事务隔离级别
![在这里插入图片描述](https://img-blog.csdnimg.cn/1246a73461db46fc92381f4b7d0988ab.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


```sql
- 设置事务的隔离级别   （设置当前会话的隔离级别）
set session transaction isolation level read uncommitted;  
set session transaction isolation level read committed;  
set session transaction isolation level repeatable read;  
set session transaction isolation level serializable;  `
```

## 脏读问题的重现与解决
**问题重现--**

要重现脏读问题,就要将当前事务隔离界别设置为 读未提交 read uncommitted
原数据---
![在这里插入图片描述](https://img-blog.csdnimg.cn/493af67060ea473e80a25052421a7c1e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
开启两个事务,为了方便开启了两个客户端,

**客户端A**
```sql
-- 当前事务隔离界别设置为 读未提交 read uncommitted
mysql> set session transaction isolation level read uncommitted ;
Query OK, 0 rows affected (0.00 sec)
--开启事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)


```
客户端B
```sql
-- 当前事务隔离界别设置为 读未提交 read uncommitted
set session transaction isolation level read uncommitted
> OK
> 时间: 0s
-- 开启事务
start TRANSACTION
> OK
> 时间: 0s


```
客户端A-----查询acount表中小丽的数据

```sql
mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
+----+-------+---------+
1 row in set (0.00 sec)
```
客户端B----小刚向小丽账户转200元钱

```sql
update acount set balance=balance+200 where id=1;
update acount set balance=balance-200 where id=2;

```

客户端A---- 再次查询小丽的数据

```sql

mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5200 |
+----+-------+---------+
1 row in set (0.00 sec)
```
客户端B---- 撤销之前的操作
 ```sql
rollback;
select * from acount ;
 ```
表中真实的结果----
![在这里插入图片描述](https://img-blog.csdnimg.cn/de2539fc170c41c395538083925baadc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_15,color_FFFFFF,t_70,g_se,x_16)
所以就出现了脏读现象;

**脏读的解决方案----**

设置当前事物的隔离级别read committed
还是开启两个事务----A,B
客户端A-----查询小丽的数据

```sql

mysql> set session transaction isolation level read committed ;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
+----+-------+---------+
1 row in set (0.00 sec)
```
客户端B----小刚向小丽账户转200元钱

```sql
update acount set balance=balance+200 where id=1;
update acount set balance=balance-200 where id=2;
```
客户端A ----查询小丽的数据

```sql

mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
+----+-------+---------+
1 row in set (0.00 sec)

```
客户端B---- 回滚

```sql
rollback
> OK
> 时间: 0.004s
select * from acount ;
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/940d17aeb36741fabb63850d6c46d23f.png)
客户端A查询到的小丽数据一直是未提交以前的,所以解决了脏读的问题;

## 不可重复读问题的重现与解决

**问题重现----**

还是开启两个事务A,B

客户端A----查询小丽的数据
```sql

mysql> set session transaction isolation level read committed ;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
+----+-------+---------+
1 row in set (0.00 sec)

```
客户端B----小刚向小丽账户转200元钱

```sql
set session transaction isolation level read committed ;
start TRANSACTION;
update acount set balance=balance+200 where id=1
> Affected rows: 1
> 时间: 0.004s
update acount set balance=balance-200 where id=2
> Affected rows: 1
> 时间: 0s

```
客户端A----查询小丽的数据

```sql

mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
+----+-------+---------+
1 row in set (0.00 sec)
```

客户端B------提交事务

```sql
COMMIT
> OK
> 时间: 0.005s
```

客户端A----再次查询小丽数据
```sql
mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5200 |
+----+-------+---------+
1 row in set (0.00 sec)
```
发现read committed虽然解决了脏读问题,但是没有解决不可重复读问题;

**不可重复读的问题解决**

解决方案是将当前事物的隔离级别设置为 repeatable read
还是开启两个事务A,B
----还原为最初数据,
客户端A---- 查询小丽数据
```sql
mysql> set session transaction isolation level repeatable read ;
Query OK, 0 rows affected (0.00 sec)

mysql> start  transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
+----+-------+---------+
1 row in set (0.00 sec)

```
客户端B ------小刚向小丽账户转200元钱

```sql
set session TRANSACTION ISOLATION level REPEATABLE read ;
start TRANSACTION;

update acount set balance=balance+200 where id=1
> Affected rows: 1
> 时间: 0.004s
update acount set balance=balance-200 where id=2
> Affected rows: 1
> 时间: 0s


```
客户端A----查询小丽的数据

```sql

mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
+----+-------+---------+
1 row in set (0.00 sec)

```
客户端B-----提交事务

```sql
COMMIT
> OK
> 时间: 0.005s

```
客户端A 再次查询小丽数据-----

```sql

mysql> select * from acount where id=1;
+----+-------+---------+
| id | uname | balance |
+----+-------+---------+
|  1 | 小丽  |    5000 |
+----+-------+---------+
1 row in set (0.00 sec)
```
可以发现解决了不可重复读的问题;

小丽需要重新连接服务器或者当前事务结束再次查询才能查到余额增加了200;
![在这里插入图片描述](https://img-blog.csdnimg.cn/6467573bf8f34b46ad462125487a5efe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**幻读问题的重现与解决**

原表----(id设置为   id int )----初始数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/1380d1e3c617465ea27053e3abcf3fb2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


设置当前事务隔离级别为读未提交
开启两个事务A,B

客户端A------查询小于3的记录数

```sql
mysql> set session TRANSACTION ISOLATION level read uncommitted;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)
-- 查询第一次的结果

mysql> select id from acount where id<3;
+------+
| id   |
+------+
|    1 |
|    2 |
+------+
2 rows in set (0.00 sec)
```
客户端B---- 向表中插入数据并提交

```sql

set session transaction isolation level read uncommitted ;
start TRANSACTION;
 insert into acount values(1,null,null);
  insert into acount values(2,null,null);
	 insert into acount values(3,null,null);
	  insert into acount values(4,null,null);
	  COMMIT
> OK
> 时间: 0.003s
```
客户端A-----查询小于3的记录

```sql

mysql> select id from acount where id<3;
+------+
| id   |
+------+
|    1 |
|    2 |
|    1 |
|    2 |
+------+
4 rows in set (0.00 sec)
```
这时候发现小于3的数据增加了2条;



关于事务在高并发环境下并不一定说百分之百就能出现上述三种问题,这将球一定概率;
加了相应锁的情况下一定会避免该问题;

**幻读问题的解决----**

剩下一种是serializable 解决了上述三种问题;
将上述数据还原为初始数据

客户端A----

```sql

mysql> set session transaction isolation level serializable;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> select id from acount where id<3;
+------+
| id   |
+------+
|    1 |
|    2 |
+------+
2 rows in set (0.00 sec)

mysql> select id from acount where id<3;
+------+
| id   |
+------+
|    1 |
|    2 |
+------+
2 rows in set (0.00 sec)
```
客户端B-----

```sql
set session transaction isolation level SERIALIZABLE ;
start TRANSACTION;
 insert into acount values(1,null,null);

```
这时候客户端B的状态为阻塞状态-----

直至超时,出现错误提示,或者客户端A提交了事务客户端B插入数据才能得以继续执行;
```sql

mysql> insert into acount values(1,null,null);
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
mysql>   insert into acount values(2,null,null);
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
mysql>  insert into acount values(3,null,null);
Query OK, 1 row affected (3.67 sec)

mysql>   insert into acount values(4,null,null);
Query OK, 1 row affected (0.00 sec)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c38be44c0dea4194ba48aeb544f675c8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这是什么情况呢？

**当隔离级别为Serializable 时,用select时会使用到快照读,即查询没有问题**，但是在**insert时会进行当前读**，(或者update/delete时也会进行当前更改/删除)
在mysql中 快照读即事务开启时产生的序列版本号，当事务对数据表当前的数据select时，用到快照读，但是如果一个事务A最先对表进行了操作,如果事务B这时后也想对该表进行操作------**select 查询数据可以**,但是insert/ update / delete 这种语句时，系统会对insert/ update / delete 操作会进行加锁 (X锁)，不允许其他事务修改，但是能读取当前数据(当前读)即获取当前行的S锁，左边insert需要在右边事务释放锁之后才能进行insert/update操作(commit或者rollback)



事务A提交事务后(阻塞时间结束前)
在次查询数据,发现只有3,4 成功插入;
![在这里插入图片描述](https://img-blog.csdnimg.cn/4c3bbb2b1a274ac2b492be364b43f2e1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
当隔离级别设置为serializable时会严重影响mysql中表的操作效率,所以实际应用中选择RP(可重复读隔离级别),在此基础上添加以下锁来达到解决幻读的问题;



## 数据的几种导出方式

第一种  select  * from tablename [ where       condition] into outfile 目标文件夹下的某文件

```sql
mysql> select * from hxjy  into outfile 'C:/mysqlback/hxjy.txt' ;
Query OK, 11 rows affected (0.00 sec)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210605120312428.png)
第二种 是在cmd界面用mysqldump导出这种方式会导出 两个文件，一个txt，一个sql

```sql

C:\Users\Administrator>mysqldump -T C:\mysqlback   company hxjy -u root -p --fields-terminated-by='!
!!!'
Enter password: ******

C:\Users\Administrator>
```
这种方式不需要指定文件名，生成的文件也必须符合  secure-file-priv  的位置要求否则会报错

```sql
mysql> select * from hxjy into outfile 'C:\mysqlback\huixinjiaoyu.doc' ;
ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot execute this statement ---
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210605121040390.png)

第三种是用mysql          --execute ‘select 语句’  dbname >某位置之下的某文件          的方式生成

```sql

C:\Users\Administrator>mysql -u root -p --execute 'select * from hxjy' company> C:\HJXY.doc

```
这种方式好像只能生成在盘的根目录下，指定其他位置的话存到了 MySQL文件夹下，这是为啥呢？
删掉该文件夹在执行，提示系统找不到指定路径，有知道的伙伴可以在评论区回答一下，谢谢！
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060512254856.png)
```sql
C:\Users\Administrator>mysql -u root -p --execute 'select * from hxjy' company> C:/mysql/aiwo.txt
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210605122359137.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

以上就是三种方式的基本区别，前两种跟secure-file-priv 有关，第三种 比较特殊。

## mysql工作原理与存储引擎

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061207250323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)图片引自王英英老师的 《mysql 8从入门到精通》（这不算是侵权吧）

MySQL客户端登陆后，MySQLD开始初始化。

  1. 首先检查用户权限（用户名密码是否符合登陆要求）--其中初始化的时候有一些参数比如查询缓存，日志记录模块如果开启，这也会一同开启，当然存储引擎也会一同开启。
  2. 初始化过后是链接管理模块，监听客户端命令，将命令转发给进（线）程管理模块，由进程管理模块将命令转发给用户模块---这时候用户模块检查该用户的权限--比如select，insert等权限，如果无权限则返回没有权限的提示，有权限则会进一步将命令转发给命令解释器去将命令下放到各个不同的小模块中，如果是select 查询，就启用查询优化器，如果优化器缓存中有已经存在的结果集就返回给用户，如果没有就分发给相应的模块进行处理。
  3. 当命令执行完毕后，控制权都会还给链接现成模块（因为要I/O输入/出）
    以上如有不当之处还请评论区指出，我会第一时间修改。

接下来算是正题吧！！！
MySQL中最常用的两种存储引擎  myIsam、innodb，其他也会用到比如merge，memory
8.0将innodb作为默认存储引擎，我也是直接从8.0开始学的，我先总结一下innodb吧！

**innodb存储引擎的特点**

1，支持事务性操作，支持提交、回滚和 崩溃恢复能力--二进制日志和erro日志  
    	   对于innodb每一条SQL语言都默认封装成事务，自动提交，这样会影响速度，所以最好把多条SQL语言放在begin和commit之间，组成一个事务；      
2，支持外键约束以保证数据的一致性 ；
3，innodb专门为数据量大表而设计，CPU处理效率高（这点我还没体会到） ；
4，innodb支持表空间> 共享还是私有，这样生成的文件不一样， 私有空间.ibd,共享空间是.ibddata innodb 的数据文件中包含索引和数据； 
5，innodb是聚集索引，索引结构为BTree，数据文件和（主键）索引绑在一起的， 先查询到主键，然后再通过主键查询到数据，但是主键不应该过大；
6，innodb使用缓冲池来缓存索引和索引行数据。设置越大，需要的磁盘I/O就越少访问表中的数据。

>   InnoDB, unlike MyISAM, uses a buffer pool to cache both indexes and 
>   row data. The bigger you set this the less disk I/O is needed to  
>   access data in tables. On a dedicated database server you may set this
>   parameter up to 80% of the machine physical memory size. Do not set it
>   too large, though, because competition of the physical memory may  
>   cause paging in the operating system.  Note that on 32bit systems you 
>   might be limited to 2-3.5G of user level memory per process, so do not
>   set it too high.
>   

**MyISAM存储引擎的特点**

1，不支持事务和外键，但是插入和查询速度高；
2，数据和索引分开存储，表结构文件.frm 数据树即索引文件.myi，数据文件.myd ；
3，同样采用Btree存储结构，text和blob可以被索引，被索引的列值中可以出现Null；

**Memory存储引擎的特点**

1，索引为btree和hash；
2，以固定的长度记录数据，所以不支持blob和text；
3，被索引的列值中也可以出现Null；
4，表是共享的，因为memory表的内容被存放在内存中，当不需要memory时应该删除以免占用内存空间；

**Merge存储引擎的特点**

merge其实就是myisam的组合，一组myisam组合成merge，所以merge表本身没有数据，（创建merge表时只会创建两个较小的文件  .frm（数据结构）和.mrg（存放表的插入信息）
插入操作是通过insert_method完成的（first 和last来插入数据）


> 演示一下merge存储引擎--

```sql
mysql> create table one (id int primary key ,name varchar(128)) engine= myisam;
Query OK, 0 rows affected (0.01 sec)

mysql> create table two (id int primary key ,name varchar(128)) engine= myisam;
Query OK, 0 rows affected (0.01 sec)

mysql> create table three (id int primary key ,name varchar(128)) engine= merge  union=(one,two) ins
ert_method=first;
Query OK, 0 rows affected (0.01 sec)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210612083807822.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210612084021768.png)
mrg文件存放哪两张表以及插入方式（first或者last）

```sql

mysql> insert into one values(1001,'jack'),(1002,'mary');
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> insert into three values(1003,'tom'),(1004,'gavin');
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from three;
+------+-------+
| id   | name  |
+------+-------+
| 1001 | jack  |
| 1002 | mary  |
| 1003 | tom   |
| 1004 | gavin |
+------+-------+
4 rows in set (0.00 sec)

mysql> insert into two values(1005,'sam'),(1006,'king');
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from three;
+------+-------+
| id   | name  |
+------+-------+
| 1001 | jack  |
| 1002 | mary  |
| 1003 | tom   |
| 1004 | gavin |
| 1005 | sam   |
| 1006 | king  |
+------+-------+
6 rows in set (0.00 sec)

mysql> select * from one ;
+------+-------+
| id   | name  |
+------+-------+
| 1001 | jack  |
| 1002 | mary  |
| 1003 | tom   |
| 1004 | gavin |
+------+-------+
4 rows in set (0.00 sec)

mysql> select * from two ;
+------+------+
| id   | name |
+------+------+
| 1005 | sam  |
| 1006 | king |
+------+------+
2 rows in set (0.00 sec)
mysql>
```
当我们查询one 表和two内容时，我们发现插入three（merge引擎）的数据跑到了one表中，因为插入时制定了insert_method=first；当没有指定insert_method时向three表插入数据就会报错，因为系统不知道将数据插入到哪里；

# 数据结构--B+树



为什么要深入学习B+树,因为在学习mysql时候知道了innodb存储引擎和myisam存储引擎存储数据的结构用到了B+树,为什么要用B+树?
一旦打开了知识的大门,发现学习的内容还有很多,学无止境!!!



学习B+树之前先要学习B树,学习B树之前又要追溯到二叉树
别着急慢慢来,我是这么安慰我自己的!!
![在这里插入图片描述](https://img-blog.csdnimg.cn/a440509970c44d2a845db211db57088f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


## 二叉树

**什么是二叉树?**

简单来说二叉树是一种存储数据的结构,问我们之前学习的哈希表,链表,数组一样都是存储结构的一种;今天主要聊的是二叉树以及二叉树的变种;
[二叉树百科定义](https://baike.baidu.com/item/B%E6%A0%91/5411672?fr=aladdin#1)

**二叉树的几种形态**

  1. 空树;
  2. 只有根节点的树;
  3. 只有左子树;
  4. 只有右子树;
  5. 完全二叉树;

如下图所示---
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd622a9dab0c487894c8542f5c386ec9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)在普通二叉树的基础上衍生出了BST俗称二叉查找树


**BST是怎样插入数据的?**

以数据 10,6,4,7,3,8,2,9,1,5   按顺序插入二叉树为例
![在这里插入图片描述](https://img-blog.csdnimg.cn/8c0c809d6ef14c9d8f031547613690d5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**BST是怎样查找数据的?**
以上图数据为例,
查找数据5是怎样查找的呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/6fda1ad161934fb2af96c768560b7195.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)还是上图的数据如果是在线性表结构中是怎样查找的呢?

![在这里插入图片描述](https://img-blog.csdnimg.cn/00490cec707a471cb91ec98ab15cae12.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
对比分析,BST查找数据比线性表在一定程度上要好一些,但是如果是二叉树的极端情况(递减或者递增数据)----只有左子树或者右子树,那么二叉树就会退化成线性表结构;
如下数据1,2,3,4,5如果查找数据5
![在这里插入图片描述](https://img-blog.csdnimg.cn/c2763390a9da4e5489aaec208474900e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**基于以上原因,因此就有了基于BST的一些变种形式B树,AVL树,B+树,等;**
BST插入数据是比较繁琐的,不如线性表,但是查找效率要好于线性表(非极端情况下)

**BST是怎样删除数据的?**
假如要删除3,先找到3,然后删除,如果有子节点,则子节点来补位

![在这里插入图片描述](https://img-blog.csdnimg.cn/cbb7953f75b74ffdbe8a44b01447afed.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**线性表中可以通过下标来直接将元素删除;**
综上分析-----
> 如果插入/删除数据比较频繁,BST的插入/删除效率不如线性表,但实际情况中是有些数据录入之后很少修改,反而是查询的次数远远大于修改的次数,因此BST可应用的场合是插入不频繁但是查询频繁的场景;

总结----BST的特征:

  1. 任意结点（包括根结点）的左子树上的结点的值都比这个结点得值小。 
  2. 任意结点（包括根结点）的右子树上的结点的值都比这个结点得值大。
  3. 对一个二叉树进行中序遍历，如果是单调递增的，则可以说明这个树是二叉搜索树。



二叉树的遍历----四种方式

  1. 前序遍历----先访问根节点,在前序遍历左子树;
  2. 中序遍历----先访问左子树,然后访问根节点,再访问右子树;
  3. 后序遍历----先访问左子树,在访问右子树,最后访问根节点;
  4. 层序遍历-----从根节点往下一层一层访问;


代码实现----
先建立一颗二叉树

```javascript
package TreeB;

import java.util.LinkedList;

public class BinaryTreeD {
    public int data;
    public BinaryTreeD leftBTree;//左节点
    public BinaryTreeD rightBTree;//右节点

    //二叉树构造方法
    public BinaryTreeD(int data) {//这里应该是泛型,但为了简便只好先这样
        this.data = data;
    }

    //创建一个二叉树----为了简便指定为Integer 其实应该是泛型的
    /*
    将链表的数据加入到二叉树中
     */
    public static BinaryTreeD createBinaryTree(LinkedList<Integer> list) {
        BinaryTreeD binaryTreeD = null;
        //如果链表为空
        if (list == null || list.isEmpty()) {
            return null;//空树
        }//如果 不为空,则添加元素,首先要从list中取数据,
        Integer data = list.removeFirst();
        if (data != null) {//如果data不为null则添加,----因为Integer不像int,是有可能出现null值的
            //添加数据
            binaryTreeD=new BinaryTreeD(data);
            //下一个添加的元素左节点,右节点,都是这样操作,因此递归掉用就可以了,出口是list==null,return null;
            binaryTreeD.leftBTree=createBinaryTree(list);
            binaryTreeD.rightBTree=createBinaryTree(list);
        }

        return binaryTreeD;
    }
   //前序遍历二叉树   跟节点----左子树----右子树
    //因为要遍历,把树当作参数传进来
public  static void preOrderBinaryTree(BinaryTreeD binaryTreeD){
        //如果树为空
        if(binaryTreeD==null){
            return;

        }
        //递归方法,出口是binaryTreeD==null

        System.out.print(binaryTreeD.data+"\t");
    preOrderBinaryTree(binaryTreeD.leftBTree);
        preOrderBinaryTree(binaryTreeD.rightBTree);
}
//中序遍历     左子树----根----右子树
public static void midOrderBinaryTree(BinaryTreeD binaryTreeD){
    //如果树为空
    if(binaryTreeD==null){
        return;
    }
    //递归方法,出口是binaryTreeD==null
    midOrderBinaryTree(binaryTreeD.leftBTree);
    System.out.print(binaryTreeD.data+"\t");
    midOrderBinaryTree(binaryTreeD.rightBTree);
}
public static void bacOrderBinaryTree(BinaryTreeD binaryTreeD){
        //如果树为空
        if(binaryTreeD==null){
            return;
        }
        //递归方法,出口是binaryTreeD==null
        bacOrderBinaryTree(binaryTreeD.leftBTree);
  bacOrderBinaryTree(binaryTreeD.rightBTree);
        System.out.print(binaryTreeD.data+"\t");

    }

    public static void main(String[] args) {
        LinkedList<Integer>list= new LinkedList<>();
        list.add(10);
        list.add(6);
        list.add(4);
        list.add(7);
        list.add(3);
        list.add(8);
        list.add(2);
        list.add(9);
        list.add(1);
        list.add(5);
        BinaryTreeD binaryTree = createBinaryTree(list);
       preOrderBinaryTree(binaryTree);
        System.out.println();
       midOrderBinaryTree(binaryTree);
        System.out.println();
        bacOrderBinaryTree(binaryTree);
    }
}

```

H代表树高,N代表元素个数;

**来看BTS树查找数据的时间复杂度**
仅算最坏查找的时候(空树,查找啥数据????所以不算)
1,深度为1的BTS,即只有根节点,,查找次数为1,
2,深度为2的BTS,查找次数为2;
3,深度为3的BTS树,查找次数为3;
4,深度为4的BTS树,查找次数为4
以此类推,二叉查找树查找数据的时间复杂度为O(H);

**来看BTS树插入数据的时间复杂度**
仅算最坏插入的时候(空树插入就完了,不用判断是插入左边还是右边)
1,深度为1的BTS树.判断一次(即插入左边还是右边)
2,深度为2的BTS树,判断两次,(根节点一次和左节点(或者右节点)一次)
3,深度为3的BTS树,判断三次
以此类推,二叉查找树的插入数据的时间复杂度为OH);
**来看BTS树删除数据的时间复杂度**
删除数据是要先查找到该数据,所以查找(O(H))之后删除(1),所以时间复杂度也是O(H)


**由以上可看到BTS树的时间复杂度跟树的高/深度有关;**
但是------->极端情况下如下图的BTS树
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c8db8a89a7740bea8b8d3112f24ed43.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这时候BTS树就退化成了链表,这时候时间复杂度就成了O(N)-----N指元素个数,一般情况下树的高度H是小于元素个数N的;
但是当数据量足够多的时候------>趋于无穷大则可以看作是O(H)/O(N)=1;
所以当插入的数据量很大的时候,树的深度(高度)会非常大,于是二分查找树数据查询效率其实也不算高,于是就衍生出了基于BST的AVL树----平衡二叉查找树


## AVL树
**什么是AVL树呢?**

在计算机科学中，AVL树是最先发明的自平衡二叉查找树。**在AVL树中任何节点的两个子树的高度最大差别为1**，所以它也被称为高度平衡树。增加和删除可能需要通过一次或多次树旋转来重新平衡这个树。

特点:除了具备二叉查找树的特点,他还具有书中任何节点的两个子树的高度差最大差别为1,以避免二分查找树中的极端情况;
还是之前的数据----- 10,6,4,7,3,8,2,9,1,5   按顺序插入

![](..\pic\mysql_pic\21.png)

可以发现AVL树在插入相同数据的情况下树的高度变小了,那么树的高度变小了有什么好处呢???待会在说这个,先说一下AVL树的其他方面:

**AVL树失衡**

由上图可以发现AVL在树失衡的时候做了一些操作(旋转)使得树能过够保持一个平衡状态,那么我们来研究一下什么状态下树会失衡呢?

![在这里插入图片描述](https://img-blog.csdnimg.cn/f879419e4c284cc8af94fcc55d1e99ca.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)看图,以上是左子树失衡的两种情况,
特点是插入或者删除数据后使得根节点的左子树的高度比右子树高度高2,因此失衡了;
反过来讲就是怎么使得失衡的树保持平衡?
以做左子树失衡为例,要么删除元素使得根节点左子树高度减少以使得与右子树高度差不大于1(即减少根节点左子树高度),要么添加元素使得根节点右子树高度增加以使得与左子树高度差不大于1;
同理可以得到右子树的失衡的两种情况
右左子树失衡和右右子树失衡-----对称过来就可以了;

还是上图吧------

**AVL树中的旋转**

AVL树的旋转由树的失衡引起的,(上面四种树的失衡)
先看第一种左左子树失衡和右右子树失衡------>
所以可以总结为由于插入或者删除元素引起的树失衡

![](..\pic\mysql_pic\20.png)



剩余两种失衡
单纯的旋转并不能解决问题,需要进行两次旋转

![](..\pic\mysql_pic\22.png)

由此可以看到AVL树的插入或者删除都有可能通过旋转来调整来维持树的平衡
AVL树本质上还是一棵二叉搜索树，它的特点是： [1] 
1.本身首先是一棵二叉搜索树。
2.带有平衡条件：每个结点的左右子树的高度之差的绝对值（平衡因子）最多为1。
也就是说，AVL树，本质上是带了平衡功能的二叉查找树（二叉排序树，二叉搜索树）。

**二叉平衡树的实现代码**

  比较复杂,感兴趣你自己组找资源吧!

**AVL树的效率**

从插入数据上讲----
除了要比较大小,还要做旋转操作,因此插入效率上讲不如二叉排序树----BTS
从查找数据上讲---
AVL树避免了BTS中出现的极端情况,所以查找数据的最坏效率要比BTS树的最坏效率要高;

**AVL树的时间复杂性:O(logn)**

网上有一个题目是------AVL树能插入的最少节点数的复杂度;
假设一棵树的深度为h,根节点左子树高度为h-1,根节点右子树的高度h-2(也有可能是h-1但这是一颗完全平衡的AVL树,由于是最少节点,所以选择深度为h-2),

假设T(h)为高度为h的AVL树能放的最少节点,那么就有如下关系
T(h)=左子树节点+右子树节点+根节点
由此推出 T(h)>T(h-1)+T(h-2)-------->>>>>
由于高度减少2,则节点数量减半(由于式不完全的树,要是完全的树是高度每减少1,节点数量减半)------>>>>
左子树节点数------2^(h-1)      -1                                  
右子树为2^(h-2)-1,但是由于是最少节点,所以右子树为非完全的树,所以
就有了以下不等式
T(h)>2(T(h-2))
T(h)>2*i(T(h-2*i)
i=i/2
递推得到
T(h)>2^(h/2)
logT(h)>h/2
h<2logT(h)所以树的高度不超过2logT(h)



## B树

**什么是B树?**

B树(BalanceTree)也是二叉树的改进的一种,又叫多路平衡查找树,在AVL树的基础上又加了一些约束,B树的特点是:

（1）排序方式：所有节点关键字是按递增次序排列，并遵循左小右大原则；

（2）子节点数：非叶节点的子节点数>1，且<=M ，且M>=2，空树除外（注：M阶代表一个树节点最多有多少个查找路径，M=M路,当M=2则是2叉树,M=3则是3叉）；

（3）关键字数：枝节点的关键字数量大于等于ceil(m/2)-1个且小于等于M-1个（注：ceil()是个朝正无穷方向取整的函数 如ceil(1.1)结果为2);

（4）所有叶子节点均在同一层、叶子节点除了包含了关键字和关键字记录的指针外也有指向其子节点的指针只不过其指针地址都为null对应下图最后一层节点的空格子;      [-----来自百度百科](https://baike.baidu.com/item/B%E6%A0%91/5411672#1)

B树的图解------>>>>>
![定义自己去百度不在这里说了;](https://img-blog.csdnimg.cn/e46461ea1caf430698216afb5bb754dd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)B树和平衡二叉树稍有不同的是B树属于多叉树又名平衡多路查找树（查找路径不只两个），数据库索引技术里大量使用者B树和B+树的数据结构，
B树的阶数或者称为度数,看图找规律---可以看到跟之前的二叉树和AVL树不同的是B树的一个节点种可以存放的值可以不为一个,这样在存储数据量相同的的情况下B树的深度就减少了;

## B+树
**什么是B+树**

B+树是B树的一种变形形式，B+树上的叶子结点存储关键字以及相应记录的地址，叶子结点以上各层作为索引使用。一棵m阶的B+树定义如下:  
(1)每个结点至多有m个子女；
(2)除根结点外，每个结点至少有[m/2]个子女，根结点至少有两个子女； 
(3)有k个子女的结点必有k个关键字。
B+树的查找与B树不同，当索引部分某个结点的关键字与所查的关键字相等时，并不停止查找，应继续沿着这个关键字左边的指针向下，一直查到该关键字所在的叶子结点为止.[--------来自百度百科](https://baike.baidu.com/item/B+%E6%A0%91/7845683)

**B+树的每个节点可以包含更多的节点,这样做的原因是 为了降低属的高度,第二个原因是将数据范围变为多个区间,区间越多,检索数据越快;
非叶子节点存储key,叶子节点存储key和数据
叶子节点两两指针相互连接,符合磁盘的预读性特征,顺序查询的性能更高;**



下图为B树和B+树的图解![在这里插入图片描述](https://img-blog.csdnimg.cn/97d3b05300014fcf803e3b5ff62271e3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

接下来看一下各种树存储数据的方式图解-------

以10,6,4,7,3,8,2,9,1,5  为例看相同的数据在不同的树存放数据的区别

![](..\pic\mysql_pic\23.png)

可以看到在同样的数据量下,B树和B+树的树深小,这样有什么好处呢?
也是接上面的问题,


关于B树和B+树的区别放在数据库中去说明一下----

以三阶B树为例子-------
B树--------------->>>>>B树存储数据结构图

![](..\pic\mysql_pic\24.png)

可以看到B树的存储结构-----在一个磁盘块中存放了指针和key和value,

B+树------>>>>>>>>>B+树存储数据结构图

![](..\pic\mysql_pic\25.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/833828ef4e5145ae9dea8feb990c1fc4.png)

可以看到B+树的非叶子节点存储的是key和指针,在叶子节点中存放了数据,
我们可以做一个假设,但是再假设前我们要明白两点-----


**局部性原理-----**
空间局部性-----------即数据跟程序都有聚集成群的倾向,我们希望把具备某一些特征的数据存放在一起;
时间局部性---------即被访问过一次的数据下一次很有可能被在次访问;

**磁盘预读------**
磁盘预读------加入你想让电脑给你从一篇文章种查找字母"A",虽然电脑给你找到了字母A,但是实际上电脑并不是仅仅读取了字母A一个字符,而是很多内容------即每次读取磁盘获取数据并不是仅仅读取我们需要的数据而是读取一部分数据------具体来说就是内存和磁盘交互时会有一个叫页的单位,每次至少读取4K,可以时8K,16K,总之时4K的整数倍;

回到B树和B+树的数据结构图
假设提条数据data为1KB;key和指针不占空间
在mysql中 innodb存储引擎一次IO读取的数据大小为16KB;

那么问题来了,在B树中要查找数据70需要左多少次IO呢?

第一次内存中读入磁盘块1,70在56和88之间,于是第二次IO读取磁盘块3
70在66和77之间,第三次IO读取磁盘块8,在磁盘块8找到了数据;
三次IO,读取数据48KB;

那在这48KB中能存放多少数据呢?
16*16*16=256条数据,那么三次IO能查找的数据也只有256条,实际会少于256条,因为key和指针不可能不占空间;


如果上面换成是B+树,IO次数不变,变得是在B+树中非叶子节点存放的是key和指针,不存放数据,于是三次IO读取的数据量也是48KB,但是可以查找的数据量是多少呢?
假设一组key和指针占10字节,
那么可以查找到的数据量--------
1600*1024*1600*1024*16=4,294,967,296----千万级别的数据量了,

但问题的关键是key占的大小是不确定的,所以在设立索引的时候索引的长度也影响数据的能查找的量级,如varchar索引和int索引都能找到数据的树是推荐int的,尽量避免全文索引等;



再来一个小问题-----
在一个表中id为主键索引,是否一定要设置成自增呢?

最好设置成自增
原因----除了提高查找效率外还由一个重要的原因


![](..\pic\mysql_pic\26.png)



# 锁详解

mysql中也支持各种锁--今天来将脑海中的知识来整理一下（听说大厂面试可能会问到mysql中锁的机制问题，所以还是好好学习吧）

> 就像java中各种锁 轻量锁，偏向锁，重级锁--目前我还不太熟悉-------- 慢慢学习吧，我刚入门要学习的东西很多--谁让我踏上了程序员之路

 - 首先按照表来区分锁
 - innodb支持表锁，行锁；
 - myisam支持表锁；
 - dbd支持页面锁和表锁；
    表锁加锁快，开销小，不会出现死锁；锁定范围大，发生锁冲突的概率最高，并发度也是最低；
    行锁加锁慢，开销大，会出现死锁；锁定范围小，发生锁冲突概率小，并发度高；
    页面锁是介于表锁和行锁之间，范围介于两者之间，并发度一般；

由于mysql8.0默认引擎innodb，默认为行锁，所以先整理一下行锁----

***innodb引擎中的行锁***
行锁--
行锁分为共享锁和排他锁
共享锁---->又叫读锁，当一个线程获取读锁后，会阻塞其他用户对该行数据的写操作（即阻止其他事务的排他锁），但不会阻止其他用户对改行数据的读操作（共享锁）；
排他锁---->又叫写锁，当一个线程获取写锁之后，会阻塞其他用户对改行数据的读写操作

行锁的实现方式---
InnoDB行锁是给索引项加锁来实现的。所以只有通过索引项来操作数据才会有行锁，如果没有操作索引项 用的则是表锁；
默认情况下innodb用的是隐式加锁--

对于UPDATE、DELETE和INSERT语句，InnoDB会自动给涉及数据集加写锁--排他锁（X)因为要操作数据；
对于普通SELECT语句，InnoDB不会加任何锁，因为不需要操作数据，只是读取数据；
显示加锁的操作格式如下--

>     共享锁（S）：SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE 
>     排他锁（X) ：SELECT * FROM table_name WHERE ... FOR UPDATE
>
>     对于行锁用在事务的操作上，单纯为某一行数据加行锁没有太大意义（行锁是加在索引上的而不是数据上的）下面做一下演示：

```sql

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> desc huixin ;
+--------------+-------------+------+-----+---------+-------+
| Field        | Type        | Null | Key | Default | Extra |
+--------------+-------------+------+-----+---------+-------+
| user_id      | int         | NO   | PRI | NULL    |       |
| user_name    | varchar(64) | YES  |     | NULL    |       |
| basic_salary | int         | YES  |     | NULL    |       |
+--------------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)

mysql> select user_id ,user_name from huixin where user_id=1002 for update ;
+---------+-----------+
| user_id | user_name |
+---------+-----------+
|    1002 | 赵四      |
+---------+-----------+
1 row in set (0.00 sec)

mysql> commit ;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> select user_id ,user_name from huixin where user_id=1002 lock in share mode ;
+---------+-----------+
| user_id | user_name |
+---------+-----------+
|    1002 | 赵四      |
+---------+-----------+
1 row in set (0.00 sec)

mysql> insert into huixin values(1008,'李二狗',4500);
Query OK, 1 row affected (0.00 sec)

mysql> update huixin set user_name='zhaosi' where user_id=1002 ;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```
用select 。。。。。where  [condition] lock in share mode  来获得共享锁，在查询时候确保该记录没有被其他用户用update或者delete所操作，不然查询的数据可能跟原表不能保持一致。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210612120256645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)以上是两个用户登陆数据库，由于不能同时触发命令所以也就无法触发错误提示，仅供了解，如果是不同的设备登陆，如果我在查询的时候加了行级锁，另一个设备2同时在修改我要查询的数据，则会阻塞设备2上对该数据的修改；
用select 。。。。。where  [condition]  lock in  share mode 来获得排他锁，在修改数据的时候不允许其他用户读和写该数据；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210612121301913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)以上是两个用户登陆数据库，由于不能同时触发命令所以也就无法触发错误提示，仅供了解，如果是不同的设备登陆，如果我在修改数据的时候加了行级锁，另一个设备同时在查看或者修改我要修改的数据，则会阻塞在设备2上对该数据的操作；
以上就是行锁的原理；
下面是表锁---

> innodb和myisam以及dbd都支持表锁，所以还是以innodb的表锁为例演示--

```sql

mysql> use company
Database changed
mysql> #这里是不上锁的表huixin;
mysql> desc huixin ;
+--------------+-------------+------+-----+---------+-------+
| Field        | Type        | Null | Key | Default | Extra |
+--------------+-------------+------+-----+---------+-------+
| user_id      | int         | NO   | PRI | NULL    |       |
| user_name    | varchar(64) | YES  |     | NULL    |       |
| basic_salary | int         | YES  |     | NULL    |       |
+--------------+-------------+------+-----+---------+-------+
3 rows in set (0.03 sec)

mysql> insert into huixin values(1011,'王二小',3600);
Query OK, 1 row affected (0.01 sec)

mysql> #以下是上表锁的huixin;
mysql> lock table huixin read ;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from huixin ;
+---------+-----------+--------------+
| user_id | user_name | basic_salary |
+---------+-----------+--------------+
|    1001 | 爱德华    |         2500 |
|    1002 | 赵四      |         2500 |
|    1003 | 王伟      |         2200 |
|    1004 | 宫本      |         2100 |
|    1005 | 黄淮      |         3000 |
|    1006 | 胡歌      |         5000 |
|    1007 | 张浩      |         6500 |
|    1008 | 李二狗    |         4500 |
|    1011 | 王二小    |         3600 |
+---------+-----------+--------------+
9 rows in set (0.00 sec)

mysql> insert into huixin values(1009,'郑中基',5620);
ERROR 1099 (HY000): Table 'huixin' was locked with a READ lock and can't be updated
mysql> #上读锁之后，表就处于只读状态，不能修改表中数据;
#解锁
mysql> lock table huixin write ;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into huixin values(1009,'郑中基',5620);
Query OK, 1 row affected (0.00 sec)
mysql> select * from huixin ;
+---------+-----------+--------------+
| user_id | user_name | basic_salary |
+---------+-----------+--------------+
|    1001 | 爱德华    |         2500 |
|    1002 | 赵四      |         2500 |
|    1003 | 王伟      |         2200 |
|    1004 | 宫本      |         2100 |
|    1005 | 黄淮      |         3000 |
|    1006 | 胡歌      |         5000 |
|    1007 | 张浩      |         6500 |
|    1008 | 李二狗    |         4500 |
|    1009 | 郑中基    |         5620 |
|    1011 | 王二小    |         3600 |
+---------+-----------+--------------+
10 rows in set (0.00 sec)
mysql>
```
***innodb间隙锁***
话不多说上代码---

```sql
#session1中，首先在huixin表中插入一列num，并设置为普通索引，（已经设置好了）这里不演示操作可自行操作
mysql> select * from huixin where user_id>1003 for update ;
+---------+-----------+--------------+-----+
| user_id | user_name | basic_salary | num |
+---------+-----------+--------------+-----+
|    1005 | ceshi     |         4556 |   5 |
|    1006 | test      |         4455 |   5 |
+---------+-----------+--------------+-----+
2 rows in set (0.00 sec)

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> begin ;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from huixin where num = 3 for update ;
+---------+-----------+--------------+-----+
| user_id | user_name | basic_salary | num |
+---------+-----------+--------------+-----+
|    1003 | sdsad     |         4242 |   3 |
+---------+-----------+--------------+-----+
1 row in set (0.00 sec)

mysql>
```

```sql
#在session2进行操作
mysql> use company
Database changed
mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> begin ;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into huixin values (1234 ,'test',7878,2);
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
mysql> insert into huixin values (1234 ,'test',7878,4);
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
mysql> insert into huixin values (1234 ,'test',7878,5);
Query OK, 1 row affected (0.00 sec)
```
我们可以发现在（1，3]和[3，5）之间上了间隙锁这也是为啥插不进去数据的原因。
总结--设计表的时候尽量避免间隙锁的产生，间隙锁属于行锁的范畴，如果num不属于索引，那么此锁就属于表所的范畴，在huixin表中插入任何数据都会出现阻塞。


***死锁***
死锁的产生是多个session去操作同一行数据，同时该行数据又被加锁了，演示代码如下--

```sql
#session1中
mysql> set@@au ommit =0; #关闭自动提交
Query OK, 0 rows affected (0.00 sec)

mysql> show variables like '%isolation%'; #查看隔离级别
+-----------------------+-----------------+
| Variable_name         | Value           |
+-----------------------+-----------------+
| transaction_isolation | REPEATABLE-READ |
+-----------------------+-----------------+
1 row in set, 1 warning (0.00 sec)

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> begin ;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from huixin where user_id =1111 for update ;
Empty set (0.00 sec)

mysql> insert into huixin values(1111,'ceshi',5200,100);
Query OK, 1 row affected (32.79 sec)

```

```sql
#session2中
mysql> set @@au ommit =0;
Query OK, 0 rows affected (0.00 sec)

mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> begin ;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from huixin where user_id=1111 for update ;
Empty set (0.00 sec)

mysql> insert into huixin values (1111,'ceshi',5200,100);
ERROR 1213 (40001): Deadlock found when trying to get lock; try restarting transaction
mysql>
```
可以发现多个session同时操作一行数据（必须是索引才是行锁的范畴），会产生死锁。
***死锁的解决方案***

首先要解决死锁问题，在程序的设计上，当发现程序有高并发的访问某一个表时，尽量对该表的执行操作串行化，或者锁升级，一次性获取所有的锁资源。

然后也可以设置参数innodb_lock_wait_timeout，超时时间，并且将参数innodb_deadlock_detect 打开，当发现死锁的时候，自动回滚其中的某一个事务。
***~~总结--~~ ***

> innodb存储引擎实现了行级锁，在锁定范围上更小，更精细，能实现更为复杂的操作但是所带来的性能损耗也会比表级锁很大，但是行锁在并发性能上的优势很明显。
> 当并发量很大的时候，innodb的整体性能要比myisam高，所以在使用何种引擎的时候要考虑锁的问题，在选择锁的时候应该考虑并发量是不是很大，合理使用锁，以下是使用行锁的建议---
>

  1. 尽量控制事务的大小，减少锁定资源量和锁定时长；
  2. 数据检索最好是通过索引，因为时间快性能较好（当然数据量小也会进行全表扫描），同时也可以避免升级成表锁--合理使用索引可以更好的提高性能；
  3. 尽可能减少在数据范围内的检索条件比如 检索条件 <50 要比 <100要好（这要求我们大致推算出数据的位置），避免间隙锁带来的负面影响而不能操作一些数据；
  4. 在合适的情况下尽量使隔离级别较小，同时最好是一次锁定所有的所需操作的资源；

不断学习共同进步！



# 数据的备份与恢复

关于mysql数据库备份的一些知识的总结，昨晚想了一下，在学习的过程中要不多总结才能记得更深刻，欢迎各位给他探讨共同进步。
首先说一下导出数据--
最常用的是直接复制mysql的data文件，这种方式简单粗暴，但是有一些注意事项；


> 1，这种方式不支持增量备份，所以复制之前要停掉mysql服务，如果某张表正在被用的话会复制失败。
> 2，mysql数据库的版本最好要保持一致，不同版本之间可能会出现一些问题，虽然我现在还没遇到。

再说一下CMD界面下的导出操作--
mysqldump
 一----首先导出全部数据库文件---
 ```sql

C:\Users\Administrator>mysqldump -u root -p team  >C:/mysqlback/playered.sql
Enter password: ******

C:\Users\Administrator>
 ```
这种方式导出的数据不包含数据库（即create database）的信息，

```sql

C:\Users\Administrator>mysqldump -u root -p  --databases team  >C:/mysqlback/pla.sql
Enter password: ******

C:\Users\Administrator>
```
这种包含创建数据库的信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606120009589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)以上是导出的文件内容，可以发现pla.sql包含create database 语句，

二---其次是可以单独导出某张表，

```sql

C:\Users\Administrator>mysqldump -u root -p   team player  >C:/mysqlback/players.sql
Enter password: ******

C:\Users\Administrator>
```
三----然后还以通过mysqldump 导出txt、html、xml 文件 

```sql

C:\Users\Administrator>mysqldump -T  C:/mysqlback -u root -p   team player
Enter password: ******

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606120506155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
这时导出的是一个txt和一个sql文件，并且和导出的表同名。
四---用mysql   --execute 导出 

```sql

C:\Users\Administrator>mysql -u root -p --html --vertical --execute="select * from player;" team >
C:/mysqlback.html
Enter password: ******

C:\Users\Administrator>mysql -u root -p --html --vertical --execute="select * from player;" team >
C:/mysqlback\play.html
Enter password: ******

C:\Users\Administrator>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/202106061416487.png)文件地址--C:\mysqlback\play.html
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606141709272.png)文件地址--C:\mysqlback.html           文件内容为---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606141936464.png)

```sql

C:\Users\Administrator>mysql -u root -p --xml --vertical --execute="select * from player;" team >  C
:/mysqlback\play.xml
Enter password: ******
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210606142136850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)四----下面是登陆数据库之后的导出操作

```sql
mysql> use team ;
Database changed
mysql> select * from player into outfile 'C:/mysqlback/playered.txt' ;
Query OK, 2 rows affected (0.00 sec)

```
以上就是数据的导出操作。注意一点的就是  

```sql
mysql> select * from player into outfile 'C:/mysql/playered.txt' ;
ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot exec
ute this statement
```
如果出现关于 --secure-file-priv 错误 可能的原因就是---到处的地址不符合 MySQL安全机制的要求，
另外我发现一点，如果用mysql   ---execute 导出数据，可以导出到盘的根目录，其他地方都可以，没有secure-file-priv的限制。

---------------------------------------------------------------------------------------------------------------------------------------
下面是倒入操作---
首先还是data目录的复制，然后登陆数据库，可能要修改登陆的用户，没实际操作过，这里也不演示了。
 二，通过source 倒入


```sql

mysql> use team ;
Database changed
mysql> delete from player ;
Query OK, 2 rows affected (0.01 sec)

mysql> select * from player ;
Empty set (0.00 sec)

mysql> source C:/mysqlback/play.xml ;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to y
MySQL server version for the right syntax to use near '<?xml version="1.0"?>

<resultset statement="select * from player" xmlns:xsi="ht' at line 1
mysql> source C:/mysqlback/play.txt
ERROR:
Failed to open file 'C:\mysqlback\play.txt', error: 2
mysql> source C:/mysqlback/player.sql
Query OK, 0 rows affected (0.00 sec)

mysql>

```

> source只能倒入sql文件，其他文件不好用


三---通过load data   infile倒入 （只能倒入txt 文件）

```sql
MySQL server version for the right syntax to use near 'table player' at line 1
mysql> load data infile 'C:/mysqlback/playered.txt' into table player ;
Query OK, 2 rows affected (0.00 sec)
Records: 2  Deleted: 0  Skipped: 0  Warnings: 0
```
```sql
四--通过mysqlimport 倒入
C:\Users\Administrator>mysqlimport -u root -p team C:/mysqlback/player.txt
Enter password: ******
team.player: Records: 2  Deleted: 0  Skipped: 0  Warnings: 0
```
这里有一定要注意的是，倒入的文件为文本文件，且文本文件 名跟表名一样，否则会提示表不存在。

```sql

C:\Users\Administrator>mysqlimport -u root -p team C:/mysqlback/playered.txt
Enter password: ******
mysqlimport: Error: 1146, Table 'team.playered' doesn't exist, when using table: playered
```
五，还有一种，通过mysql倒入，也不需要登陆数据库 （只支持sql文件，同source导入一样）

```sql
C:\Users\Administrator>mysql -u root -p team <C:/mysqlback/player.txt
Enter password: ******
ERROR 1064 (42000) at line 1: You have an error in your SQL syntax; check the manual that correspond
s to your MySQL server version for the right syntax to use near '1001   寮犱笁  5       鎭掓嘲闃?100
2       鏉庡洓  65      鎭掑畨' at line 1
C:\Users\Administrator>mysql -u root -p team <C:/mysqlback/player.sql
Enter password: ******
```

乱码问题需要修改一下控制台编码,这里就先这样吧!



以上就是数据的导出和导入。
其他附加的条件没有添加，感兴趣的可以自行添加；

#  读写分离

由于没有那么多主机，我只是大体捋顺一下思路-----

实现读写分离首先要有客户端，服务器（主库和从库）
大体配置如下---
| 主机类型 | 用途                |
| -------- | ------------------- |
| 主机1    | 客户端              |
| 主机2    | 服务端主库          |
| --       | --                  |
| 主机3    | 服务端 从库         |
| 主机4    | 提供mysql proxy服务 |
| --       | --                  |

  1. 在主机4上安装proxy

cmd下命令  ---sc create "Proxy" diasplayname= "MySQL Proxy"  start="auto" path= "C:/Program Files/MySQL/mysql-proxy-0.8.5/bin/mysql-proxy-svc.exe  --proxy-backend-addresses=127.0.0.1:3306" 

  2. 在主机4上配置代理参数，并设置与主机2 和主机3连接
    若主机2 为主库，主机3 为从库，配置参数如下
    先配置地址 --（要不然怎么连接！！！）
```
#配置主机3地址--
mysql-proxy  --proxy-backend-addresses=****:3306  #****表示主机3地址，端口号一般都是3306
```

```
#配置主机2 的地址
mysql-proxy  --proxy-read-only-backend-addresses=****:3306  #****表示主机2地址，端口号一般都是3306
```

  3. 配置完毕之后，在主机2，3 上创建用户，授予读写库的权限；
    grant select ,insert on *.*  to ‘用户名’@‘%’ 
  4. 服务端（主机1）连接从库（主机3）

```
#在主机1上  --4040为端口号
mysql -h  主机3地址  -u 用户名 -p 密码  4040
```

  5. 连接成功后，创建数据表  因为从库是可以读写的，主库是可以写的，所以在从库上不存在刚建的表。

 

 





#  mysqly优化

**如何优化查询效率**

问题引入----

首先我们想以下在一张数据表中mysql是怎样查找数据的?
假设有这么一张表students_info,字段名为  id 主键  ,name ,gender ,scor 等字段

这张表存放了学号从1-50000的学生信息,

接下来我想查询 id 为12345 的 学生信息,
查询语句------
Select  *  from  students_info  where id=12345;

这个时候mysql就会遍历整个表,一行一行的找,知道 id=12345 的时候,想想都比较费时间,

那怎么优化这种情况呢??
Mysql系统内部提供了一种方式------索引; 


> ***原理跟我们用的windows查找数据一样;在windows中如果开启了索引,则会在磁盘上开辟一块空间存储着文件目录,以便快速找出某个或多个的文件;***

所以我们要在id上创建索引,那么在查找id=12345 的学,直接在索引里生信息 mysql不需要任何扫描直接在索引里找到12345就可以得知这一行的位置;

**优化查询效率的方式----建立索引**

索引不仅存在与操作系统中,有些软件中也会提供对应的索引来方便查找功能;

> ***‘索引是为了方便我们查询不同的数据’--------一位IT大佬说的;***

在mysql中 索引是在存储引擎中实现的,由于mysql中有不同的索引类型,因此，每种存储引擎的索引都不一定完全相同，并且每种存储引擎也不一定支持所有索引类型。根据存储引擎定义每个表的最大索引数和最大索引长度。

> **所有存储引擎支持每个表至少16个索引，总索引长度至少为256字节。**
>

> **MySQL中索引的存储类型有两种，即BTREE和HASH，具体和表的存储引擎相关；
> MyISAM和InnoDB存储引擎只支持BTREE索引；
> MEMORY/HEAP存储引擎可以支持HASH和BTREE索引。**

**索引的优缺点**

优点----
>
>  **1,既然是索引,那就要求唯一性,通过创建唯一索引，可以保证数据库表中每一行数据的唯一性**
>
>
> **2,加快了查询效率---这也是创建索引的主要目的;**
>
> **3,在数据参考完整性方面可以加速表之间的连接,因为一般索引是设置在主键上的;**
>
> **4,在使用分组查询和排序时可以大大提高效率,减少查询和排序时间;**



凡事都有两面性,

缺点----

> **1,创建索引需要消耗时间,数据更新的频繁,那么索引据需要重新创建;**
> **2,索引文件也需要占用磁盘空间,随着数据的增多,索引文件也会增加(一般情况下----即非重复数据)如果有大量的索引，索引文件可能比数据文件更快达到最大文件尺寸。**
> **3,当对表中的数据进行增加、删除和修改的时候，索引也要动态地维护，这样就降低了数据的维护速度。**

可以查看windows索引文件----windows.edb,一般在C:\ProgramData\Microsoft\Search\Data\Applications\Windows 下,会发现该索引的大小并不是一成不变的
![在这里插入图片描述](https://img-blog.csdnimg.cn/287953de85584749a906f234157a0b52.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
## 索引的分类---
Mysql中将索引分为-----

**1,普通索引**

普通索引是MySQL中的基本索引类型，允许在定义索引的列中插入重复值和空值。

**2,唯一索引**

唯一索引要求索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。主键索引是一种特殊的唯一索引，不允许有空值。

**3,单列索引**

单列索引即一个索引只包含单个列，一个表可以有多个单列索引。

**4,组合索引**

组合索引是指在表的多个字段组合上创建的索引，只有在查询条件中使用了这些字段的左边字段时，索引才会被使用。使用组合索引时遵循最左前缀集合。

**5,全文索引**

全文索引类型为FULLTEXT，在定义索引的列上支持值的全文查找，允许在这些索引列中插入重复值和空值。全文索引可以在CHAR、VARCHAR或者TEXT类型的列上创建。MySQL中只有MyISAM存储引擎支持全文索引。

**6,空间索引**

空间索引是对空间数据类型的字段建立的索引，MySQL中的空间数据类型有4种，分别是GEOMETRY、POINT、LINESTRING和POLYGON。MySQL使用SPATIAL关键字进行扩展，使得能够用创建正规索引类似的语法创建空间索引。创建空间索引的列，必须将其声明为NOT NULL，空间索引只能在存储引擎为MyISAM的表中创建。


## 索引的选择
**1,索引并非越多越好,一个表中如果有 大量的索引,会影响CURD 操作;**

**2,选择合适的索引,监狱索引的特点,所以索引一般选择操作没有那么频繁的字段,对于经常操作的字段不适合做索引;**

**3,对于数据量小或者字段有大量重复值的情况下就不用建立索引.**

> 例如有的字段就可以不用索引,比如性别,月份,星期等字段,本来字段不同的数据就很少,用不到索引,如果用了索引反而会得不偿失,降低查询速度;

**4,当唯一性时某字段的要求时,可指定为唯一索引以确保数据的完整性,以提高查询效率;**

**5,在频繁排序或者分组的列上建立索引组合,以更快的实现排序或者分组;**


## 创建索引的方式
1,创建表的时候与索引一块创建;

**方式为---------表级约束---->>>定义完字段后再定义**


格式---- 

```sql
create table 表名 (  
字段名1 数据类型1 约束1 ,
字段名2 数据类型2 约束2 ,
.......
index 索引别名 (索引列所在字段名)
);
---->>>索引别名可添加也可不添加,不添加则默认为(索引所在字段名)
```

**创建普通索引----**

```sql
mysql>  create table testS(
    -> col_id int  ,
    -> col_name varchar(6) ,
    -> col_sex char(1) default '男' check(col_sex='男'||col_sex='女'),
    -> index (col_id), primary key (col_id)
    ->  );
Query OK, 0 rows affected, 1 warning (0.03 sec)

-- 新版中中不建议使用|| ,而建议使用 or --
mysql> show warnings\G
*************************** 1. row ***************************
  Level: Warning
   Code: 1287
Message: '|| as a synonym for OR' is deprecated and will be removed in a future release. Please use OR instead 
1 row in set (0.00 sec)
```
插入数据-----查询数据

```sql
mysql> select * from tests2;
+--------+----------+---------+
| col_id | col_name | col_sex |
+--------+----------+---------+
|   1001 | null     | 男      |
|   NULL | null     | 男      |
|   1009 | 张三     | 男      |
+--------+----------+---------+
3 rows in set (0.00 sec)

-- 查看表结构 --

mysql> show create table testS \G
*************************** 1. row ***************************
       Table: testS
Create Table: CREATE TABLE `tests` (
  `col_id` int NOT NULL,
  `col_name` varchar(6) DEFAULT NULL,
  `col_sex` char(1) DEFAULT '男',
  PRIMARY KEY (`col_id`),
  KEY `col_id` (`col_id`),
  CONSTRAINT `tests_chk_1` CHECK (((`col_sex` = _gbk'??') or (`col_sex` = _gbk'?')))
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)

-- 查看索引是否正在使用 --
 
mysql> explain select * from tests2 where col_id=1009 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE   ## 查询类型 
        table: tests2   ## 表名
   partitions: NULL  ##  分区
         type: ref  ##本数据表与其他数据表之间的关联关系
possible_keys: col_id   ## 可选用的索引
          key: col_id    ##索引
      key_len: 5         ##索引长度
          ref: const     ## 关联关系中另一个数据表里的数据列名
         rows: 1   ## 数据行数
     filtered: 100.00 ## 返回结果的行数占需读取行数的百分比----
        Extra: NULL    ##有关信息
1 row in set, 1 warning (0.00 sec)

```
 删除索引后再次查询

```sql
alter table tests2  drop index col_id;

mysql> explain select * from tests2 where col_id=1009 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tests2
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 2
     filtered: 50.00
        Extra: Using where
1 row in set, 1 warning (0.00 sec)
```

> 可以发现删除索引后, filtered的值变为了50,所以可以发现有索引和没有索引的区别----没有索引会扫描整张表,有则直接再索引中取用就可以了;

>
> **普通索引为最基本的索引类型，没有唯一性之类的限制，其作用只是加快对数据的访问速度。**

2,alter table 的方式在表中创建索引;
```sql
alter table 表名 add index    索引别名 (索引所在字段名);
---->>>索引别名可添加也可不添加,不添加则默认为(索引所在字段名)
```

紧接上表tests2,由于删除了索引,这次正好用alter的方式添加----

```sql
mysql> alter table tests2 add index coc_index (col_id) ;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
mysql>--  再次删除索引----

mysql> drop index coc_index on tests2;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

```

3,create index的 方式创建索引;

```sql
create index 索引别名 on 表名(索引所在字段名);
```

再次创建索引----

```sql
mysql> CREATE INDEX coc_index  ON tests2(col_id);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
mysql>
```
查看表结构----

```sql
## MYSQL 中默认不区分大小写
mysql> show create table tESts2 \G
*************************** 1. row ***************************
       Table: tESts2
Create Table: CREATE TABLE `tests2` (
  `col_id` int DEFAULT NULL,
  `col_name` varchar(6) DEFAULT NULL,
  `col_sex` char(1) DEFAULT '男',
  KEY `coc_index` (`col_id`),
  CONSTRAINT `tests2_chk_1` CHECK (((`col_sex` = _utf8mb4'男') or (`col_sex` = _utf8mb4'女')))
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)
```
以上为普通索引的创建方式,唯一索引,全文索引,空间索引创建的方式----触类旁通

只说一下他们之间的区别
## 普通索引与唯一索引之间的区别
**唯一索引------->>>**

用唯一索引的主要原因是减少查询索引列操作的执行时间，尤其是对比较庞大的数据表。

它与前面的普通索引类似，不同的就是：**索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。**

以上创建的索引为单列索引,还可以在多个列上添加索引;


### 多列索引---
单列索引是在数据表中的某一个字段上创建的索引，一个表中可以创建多个单列索引。这里是指每个列都是单独的,互不影响;

以 create index的方式来添加多列索引
```sql
mysql> create index index_name  on tests2(col_name(3));
-- 这里**只有字符串可以设置索引长度**,索引长度要小于等于设定的字符串长度,由于建表时col_name 设置为  VARCHAR ( 6 ),所以索引长度最大就为6,

Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
### 组合索引---
以alter table 的方式来添加组合索引
```sql

mysql> drop index coc_index on tests2;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table tests2 add unique index un_index (col_id,col_name);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

--- 查看表信息 ----

mysql> show create table tests2 \G
*************************** 1. row ***************************
       Table: tests2
Create Table: CREATE TABLE `tests2` (
  `col_id` int DEFAULT NULL,
  `col_name` varchar(6) DEFAULT NULL,
  `col_sex` char(1) DEFAULT '男',
  UNIQUE KEY `un_index` (`col_id`,`col_name`),
  KEY `index_name` (`col_name`(3)),
  CONSTRAINT `tests2_chk_1` CHECK (((`col_sex` = _utf8mb4'男') or (`col_sex` = _utf8mb4'女')))
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)

```

组合索引可起几个索引的作用，但是使用时并不是随便查询哪个字段都可以使用索引，而是遵从“最左前缀”：利用索引中最左边的列集来匹配行，这样的列集称为最左前缀

**即遵循最左边的索引为基准,如果第一个数据一样,则比较第二个,第二个一样则比较第三个,**
假设由id、name和age 3个字段构成的索引，索引行中按id、name、age的顺序存放，索引可以搜索（id,name, age）、（id, name）或者id字段组合,但是不能越过 id去搜索name,age  一句话必须要包含字段id的组合

演示-----

```sql
CREATE TABLE tests6 (
id INT,
name VARCHAR ( 6 ),
sex CHAR ( 1 ) ,
age INT,
INDEX index_zuhe( id,name, age ) 
)
> OK
> 时间: 0.049s
---插入数据---
insert into tests6 values
(1001,'张三','男',18),
(1008,'张三三','男',19),
(1011,'李四思','女',28),
(1004,'张三风','男',18),
(1018,'张二蛋','男',19),
(1021,'李帅','女',28),
(1031,'张王','男',18),
(1058,'张大大','男',19),
(1061,'李小小','女',28)
> Affected rows: 9
> 时间: 0.029s

-- 查询数据 --

mysql> explain select * from tests6 where  id=1031 and name='张王'and age =18  \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tests6
   partitions: NULL
         type: ref
possible_keys: index_zuhe
          key: index_zuhe
      key_len: 37
          ref: const,const,const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)


mysql> explain select * from tests6 where  id=1031 and name='张王' \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tests6
   partitions: NULL
         type: ref
possible_keys: index_zuhe
          key: index_zuhe
      key_len: 32
          ref: const,const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)


mysql> explain select * from tests6 where age='18' and  name='张王' \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tests6
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 9
     filtered: 11.11
        Extra: Using where
1 row in set, 1 warning (0.00 sec)


```
可以发现当查询没有id 为索引的时候,就没有用到索引index_zuhe

### 全文索引----

FULLTEXT全文索引可以用于**全文搜索**。**只有MyISAM存储引擎支持FULLTEXT索引**，并且**只为CHAR、VARCHAR和TEXT列创建索引**。索引总是对整个列进行，不支持局部（前缀）索引。

创建全文索引-----

```sql

CREATE TABLE test07 ( id INT, NAME VARCHAR ( 6 ), age TINYINT, liketext  LONGTEXT, FULLTEXT INDEX qw_index ( liketext ) );

mysql> show create table test07 \G
*************************** 1. row ***************************
       Table: test07
Create Table: CREATE TABLE `test07` (
  `id` int DEFAULT NULL,
  `NAME` varchar(6) DEFAULT NULL,
  `age` tinyint DEFAULT NULL,
  `liketext` longtext,
  FULLTEXT KEY `qw_index` (`liketext`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)

```
有些书中说 innodb不支持全文索引,应该更新了,在mysql5.6之后就支持了,看官方文档给出的源码----
[官方源码链接](https://dev.mysql.com/doc/refman/8.0/en/fulltext-natural-language.html)
```sql
mysql> CREATE TABLE articles (
          id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
          title VARCHAR(200),
          body TEXT,
          FULLTEXT (title,body)
        ) ENGINE=InnoDB;
Query OK, 0 rows affected (0.08 sec)

mysql> INSERT INTO articles (title,body) VALUES
        ('MySQL Tutorial','DBMS stands for DataBase ...'),
        ('How To Use MySQL Well','After you went through a ...'),
        ('Optimizing MySQL','In this tutorial, we show ...'),
        ('1001 MySQL Tricks','1. Never run mysqld as root. 2. ...'),
        ('MySQL vs. YourSQL','In the following database comparison ...'),
        ('MySQL Security','When configured properly, MySQL ...');
Query OK, 6 rows affected (0.01 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM articles
        WHERE MATCH (title,body)
        AGAINST ('database' IN NATURAL LANGUAGE MODE);
+----+-------------------+------------------------------------------+
| id | title             | body                                     |
+----+-------------------+------------------------------------------+
|  1 | MySQL Tutorial    | DBMS stands for DataBase ...             |
|  5 | MySQL vs. YourSQL | In the following database comparison ... |
+----+-------------------+------------------------------------------+
2 rows in set (0.00 sec)
```

全文索引的用途-----

> FULLTEXT索引。全文索引非常适合于大型数据集，对于小的数据集，它的用处比较小。

### 空间索引---


```sql
mysql> create table test08 (
    -> located geometry not null,
    -> SPATIAL index spq_index (located)
    -> );
Query OK, 0 rows affected, 1 warning (0.07 sec)

-- 警告信息---添加SRID 参考  SRID是指数据的坐标系 --
mysql> show warnings \G
*************************** 1. row ***************************
  Level: Warning
   Code: 3674
Message: The spatial index on column 'located' will not be used by the query optimizer since the column does not have an SRID attribute. Consider adding an SRID attribute to the column.
1 row in set (0.00 sec)

mysql> DROP TABLE TEST08 ;
Query OK, 0 rows affected (0.14 sec)

mysql> create table test08 (
    -> located geometry not null srid 4456,
    -> SPATIAL index spq_index (located)
    -> );
Query OK, 0 rows affected (0.14 sec)

mysql>
```

## 查看表的索引信息--

```sql

mysql> show index from test08 \G
*************************** 1. row ***************************
        Table: test08    -- 创建索引的表
   Non_unique: 1 -- 索引为非唯一索引,0表示唯一,1表示非唯一
     Key_name: spq_index--- 索引名
 Seq_in_index: 1 -- 该索引所在在索引中的位置,单列索引为1,组合索引则显示处所在第几位;
  Column_name: located -- 定义 索引的列
    Collation: A   
  Cardinality: 0
     Sub_part: 32    -- 索引长度
       Packed: NULL 
         Null:      ---该字段是否可为空,空白表示不能为空
   Index_type: SPATIAL  -- 索引类型
      Comment:           --- 注解
Index_comment:      --- 索引注解
      Visible: YES  ----是否可见
   Expression: NULL  ----描述
1 row in set (0.04 sec)
```


## 删除索引
### 方式一-->>>>alter table    ..  drop index..

```sql

mysql> alter table test08 drop index spq_index ;
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
### 方式二-->>>>drop index  ..    on..

```sql
mysql>  drop index sqp_index on test08 ;
Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
**删除表中的列时，如果要删除的列为索引的组成部分，则该列也会从索引中删除。如果组成索引的所有列都被删除，则整个索引将被删除。**


## 指定降序索引
我们创建索引时默认的为升序,如果需要则可以指定为降序索引

```sql
mysql> create table test0010(
    -> id int ,
    -> name varchar(6),
    -> score int ,
    -> index (id ,score desc)
    -> );
Query OK, 0 rows affected (0.08 sec)
-----查看表的创建信息----

mysql> show create table test0010\G
*************************** 1. row ***************************
       Table: test0010
Create Table: CREATE TABLE `test0010` (
  `id` int DEFAULT NULL,
  `name` varchar(6) DEFAULT NULL,
  `score` int DEFAULT NULL,
  KEY `id` (`id`,`score` DESC)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.04 sec)
```


插入数据-----

```sql
delimiter ;;
create procedure test_insert ()
begin 
declare i int default 1;
while i<50000
do 
insert into test0010 (id,score) select RAND()*50000,RAND()*50000;
set i=i+1;
end while ;
commit ;
end ;;
delimiter ;
call test_insert() ;

-- 降序索引
mysql> explain select * from test0010 order by  id,score  desc limit 6 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test0010
   partitions: NULL
         type: index
possible_keys: NULL
          key: id
      key_len: 10
          ref: NULL
         rows: 6
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)
/* 降序索引只是对查询中特定的排序顺序有效，如果使用不当，反而查询效率更低。*/
-- 如以下查询方式  遍历了整张表,效率低---
mysql> explain select * from test0010 order by  id desc , score  desc limit 6 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: test0010
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 50537
     filtered: 100.00
        Extra: Using filesort
1 row in set, 1 warning (0.00 sec)

```

>
> **Using filesort是MySQL里一种速度比较慢的外部排序，如果能避免是最好的结果。多数情况下，管理员可以通过优化索引来尽量避免出现Using filesort，从而提高数据库执行速度。**

@[ ](mysql事务处理)

# 存储过程和函数
存储程序可以分为存储过程和函数。在MySQL中，创建存储过程和函数使用的语句分别是CREATE PROCEDURE和CREATE FUNCTION。使用CALL语句来调用存储过程，只能用输出变量返回值。函数可以从语句外调用（引用函数名），也能返回标量值。存储过程也可以调用其他存储过程。

> **一句话解释存储过程/函数----------------就是提前将我们常用的一些查询存储起来,用的时候直接通过别名调用,非常的方便;**
## 为什么要有存储过程/函数

> 当我们经常要查询某张表的信息的时候,不用每次都输入重复的代码,而是将该代码通过一点过方式存储起来,下次用的时候直接通过过程名调用即可;
> 使用存储过程将简化操作，减少冗余的操作步骤，同时，还可以减少操作过程中的失误，提高效率;

## 创建存储过程与调用存储过程

```sql
-- 格式----
delimiter ;;
create procedure  过程名([参数1,参数2,.......])
[存储过程特性]
begin -- 开始事务
过程内容
end ;; -- 结束事务
dilimiter ; -- 改回原来的结束符
-- 调用过程
call 过程名([参数1,参数2,....])
-- [] 内为可选内容


/*参数中的内容---- [in|out|inout] 参数名 参数类型
其中，IN表示输入参数，OUT表示输出参数，INOUT表示既可以输入也可以输出；param_name表示参数名称；type表示参数的类型，该类型可以是MySQL数据库中的任意类型*/

/* 存储过程/函数的特性---
●　LANGUAGE SQL：说明routine_body部分是由SQL语句组成的，
当前系统支持的语言为SQL。SQL是LANGUAGE特性的唯一值。
●　[NOT] DETERMINISTIC：指明存储过程执行的结果是否正确。
DETERMINISTIC表示结果是确定的。每次执行存储过程时，相同的输入会得到相同的输出。
NOT DETERMINISTIC表示结果是不确定的，相同的输入可能得到不同的输出。如果没有指定任意一个值，默认为NOT DETERMINISTIC。
●　{ CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIESSQL DATA }：指明子程序使用SQL语句的限制。CONTAINS SQL表明子程序包含SQL语句，但是不包含读写数据的语句；
NO SQL表明子程序不包含SQL语句；
READS SQL DATA说明子程序包含读数据的语句；
MODIFIES SQL DATA表明子程序包含写数据的语句。
默认情况下，系统会指定为CONTAINS SQL。
●　SQL SECURITY { DEFINER | INVOKER }：指明谁有权限来执行。
DEFINER表示只有定义者才能执行。
INVOKER表示拥有权限的调用者可以执行。默认情况下，系统指定为DEFINER。
●　COMMENT 'string'：注释信息，可以用来描述存储过程或函数。
*/
```

### 不带参数的存储过程
例如以下操作---------------->>>>
```sql
-- 创建存储过程-----
mysql> delimiter /  -- 更改结束符
mysql> create procedure search() -- 此存储过程没有参数，但是后面的()仍然需要
    -> begin
    -> select * from emp ;
    -> end /
Query OK, 0 rows affected (0.04 sec)
mysql> delimiter ;  -- 改回原来的结束符
-- 调用存储过程


mysql>  call search();
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7367 | NULL   | NULL      | NULL | NULL       |    NULL |    NULL |     40 |
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
15 rows in set (0.00 sec)

Query OK, 0 rows affected (0.10 sec)

```
练习------
-- 查询当月应发的总薪资 ,因为每个月都要发,所以可以设置为过程

```sql
mysql>  create procedure salaryAll()
    -> begin
    -> select sum(sal+ifnull(comm,0)) 当月应发总薪资 from emp;
     ->  end ;;
Query OK, 0 rows affected (0.01 sec)

mysql>

mysql> call salaryall();
+----------------+
| 当月应发总薪资 |
+----------------+
|       31225.00 |
+----------------+
1 row in set (0.02 sec)

```

### 带有输入参数的存储过程
-- 比如想要查询 名字中带有'A' 的员工信息

```sql
mysql> create procedure searchB(in name varchar(20))-- 设置参数 name
    ->    begin
    ->      if name is null ||name ='' then
    ->     select * from emp ;
    ->      else
    ->     select * from emp where ename like concat('%',name,'%');-- 字符拼接
    ->      end if;
    ->     end;;
Query OK, 0 rows affected, 1 warning (0.03 sec)

mysql> call searchB('A');;-- 传入参数
+-------+--------+----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB      | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+----------+------+------------+---------+---------+--------+
|  7499 | ALLEN  | SALESMAN | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7654 | MARTIN | SALESMAN | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER  | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER  | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7876 | ADAMS  | CLERK    | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK    | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
+-------+--------+----------+------+------------+---------+---------+--------+
7 rows in set (0.00 sec)

Query OK, 0 rows affected (0.09 sec)
```

## 带有输入输出参数的存储过程----

例如要查询某个职位员工的信息----

```sql
mysql> 
mysql>  delimiter ;;
CREATE PROCEDURE searchinfo ( IN jobs VARCHAR ( 20 ), OUT countnum INT )BEGINIFjobs IS NULL OR jobs = '' THENSELECT* FROMemp;ELSESELECT* FROMemp WHEREjob = jobs;END IF;select FOUND_ROWS() into countnum;END
;;
mysql>  delimiter ;
mysql> call searchinfo('clerk',@countnum) ;
+-------+--------+-------+------+------------+---------+------+--------+
| EMPNO | ENAME  | JOB   | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
+-------+--------+-------+------+------------+---------+------+--------+
|  7369 | SMITH  | CLERK | 7902 | 1980-12-17 |  800.00 | NULL |     20 |
|  7876 | ADAMS  | CLERK | 7788 | 1987-05-23 | 1100.00 | NULL |     20 |
|  7900 | JAMES  | CLERK | 7698 | 1981-12-03 |  950.00 | NULL |     30 |
|  7934 | MILLER | CLERK | 7782 | 1982-01-23 | 1300.00 | NULL |     10 |
+-------+--------+-------+------+------------+---------+------+--------+
4 rows in set (0.02 sec)

Query OK, 1 row affected (0.04 sec)

mysql> select @countnum ;
+-----------+
| @countnum |
+-----------+
|         4 |
+-----------+
1 row in set (0.00 sec)

```



## 创建存储函数与调用存储函数

```sql
-- 格式--
delimiter ;;
create function 函数名 ([参数1,参数2,......]) 
returns type 
[函数特性]
return 函数内容
;;

delimiter ;
 --调用
 select 存储函数名(参数....)
```

内容解析基本跟存储过程一样;

```sql
mysql> delimiter ;;
mysql> create function nameall()
    -> returns varchar(20)
       -> return ( select ename from emp where empno=7893)
    -> ;;
    -- 错误提示  没有函数特性来约束,
    --- 解决方式----
   -- 1,添加函数特性约束;
    -- 2 ,设置| log_bin_trust_function_creators 为ON状态
    
    
ERROR 1418 (HY000): This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)

mysql> show variables like '%trust%';
    -> ;;
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin_trust_function_creators | OFF   |
+---------------------------------+-------+
1 row in set, 1 warning (0.00 sec)
mysql> set global log_bin_trust_function_creators = 'on';
Query OK, 0 rows affected (0.00 sec)

mysql> delimiter ;;
mysql> create  function test()
    -> returns varchar(20) -- 函数返回值类型
    -> return (select ename from emp where empno=7893)-- 执行的操作
    -> ;;
Query OK, 0 rows affected (0.03 sec)
-- 调用存储函数--
mysql> delimiter ;
mysql> select test();
+--------+
| test() |
+--------+
| NULL   |----创建存储函数时empno 值写错了
+--------+
1 row in set (0.02 sec)
-- 将表中king的empno数据改为7893,在执行
mysql> select test();
+--------+
| test() |
+--------+
| KING   |
+--------+
1 row in set (0.00 sec)
-- 这里强调一下为了安全建议不要开启log_bin_trust_function_creators ,而是为存储函数添加存储特性来解决上述ERROR 1418 (HY000):问题

```
添加函数存储特性----

```sql

mysql> set global log_bin_trust_function_creators = 'off';
Query OK, 0 rows affected (0.00 sec)
-- 创建存储函数
mysql> delimiter ;;
mysql> create function find()
    ->    returns varchar(20)
    ->   READS SQL DATA
    ->      return (select empno from emp where ename='KING' )
    ->      ;;
-- 调用
mysql> select find();
+--------+
| find() |
+--------+
| 7893   |
+--------+
1 row in set (0.00 sec)
```
如果在存储函数中的return语句跟returns返回值类型不一样,那么返回值会被强制准换为恰当的类型
如果函数返回值为varchar(20) 但是return返回的是一个整数,那么该整数会被转换成varchar类型;

举个例子----

```sql

mysql> delimiter ;;
mysql> create function find2()
    ->    returns varchar(2)
    ->   READS SQL DATA
    ->      return (select empno from emp where ename='KING' )
    ->      ;;
Query OK, 0 rows affected (0.03 sec)

mysql> delimiter ;
mysql> select find2();
ERROR 1406 (22001): Data too long for column 'find2()' at row 2

mysql> delimiter ;;
mysql> create function find5()
    ->    returns  int(2)
    ->   READS SQL DATA
    ->      return (select empno from emp where ename='KING' )
    ->      ;;
Query OK, 0 rows affected, 1 warning (0.03 sec)

mysql> delimiter ;
mysql> select find5();
+---------+
| find5() |
+---------+
|    7893 |
+---------+
1 row in set (0.00 sec)


```

> **可以发现int被转换换成了字符串类型,由于长度要求为2个,所以会提示ERROR 1406 (22001):错误;但是如果为int类型的即使指定显示宽度也会将内容显示出来;**


小结----
> **指定参数为IN、OUT或INOUT只对PROCEDURE是合法的。（FUNCTION中总是默认为IN参数）。RETURNS子句只能对FUNCTION做指定，对函数而言这是强制的。它用来指定函数的返回类型，而且函数体必须包含一个RETURN value语句。**


## 变量的使用
变量可以在存储过程中声明并使用，这些变量的作用范围是在BEGIN…END程序中;
### 定义变量----
```sql
--语法格式--
declare 变量名 变量数据类型 [default 默认值];
-- 如果没有default 则默认为该数据类型的默认值--- int 为 0 字符串为 null,类似于java中的默认值;
-- 如果为同一类型的变量,则可以写在一起最后定义数据类型
declare a ,b c int;
-- 变量值除了可以被声明为一个常数外,还可以是一个表达式;
```
### 设置变量值
```sql
-- 格式----
set  变量名 =值
set a= 10,b=30; 
set c=a+b;
```
还可以将select 语句查询到的值赋值到变量上;

```sql
declare id int;
declare name varchar(8);
select empno ,ename  into id ,name  from emp where empno=7893;
```


## mysql中定义条件以及处理异常

### 为什么要定义条件???
定义条件的目的其实就像java中的try catch 取捕捉过程或者函数过程中可能遇到的异常;
在java中如果遇到异常而不去处理,那么程序会因为异常而中断,在mysql中也是;
举个例子------------------->>>>>

> **先定义一张表---->>**

```sql
mysql> create table mytest (
    -> id int  primary key ,
    -> name varchar (8) not null
    -> );
Query OK, 0 rows affected (0.03 sec)
```

> **创建过程批量插入数据----->>>**

```sql
-- 处理异常的过程--
mysql>  create procedure mytestin()
    ->      begin
    ->      declare mycondition condition for 1062;
    ->     declare continue handler for mycondition set @info='重复值';
    ->      insert into mytest values(1,'张三');
    ->      insert into mytest values(1,'李四');
    ->      insert into mytest values(2,'李四'),(3,'王五');
    ->     insert into mytest values(2,'李四'),(1,'赵六');
    -> end
    -> ;;
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> call mytestin();;
Query OK, 1 row affected (0.00 sec)
-- 不处理异常的过程
mysql>  create procedure mytestin1()
    ->      begin
    ->      insert into mytest values(10,'张三');
    ->      insert into mytest values(10,'李四');
    ->      insert into mytest values(20,'李四'),(30,'王五');
    ->     insert into mytest values(20,'李四'),(10,'赵六');
    -> end
    -> ;;
Query OK, 0 rows affected (0.01 sec)
    -> ;;

mysql> call mytestin1();;
ERROR 1062 (23000): Duplicate entry '10' for key 'mytest.PRIMARY'
ERROR 1062 (23000): Duplicate entry '10' for key 'mytest.PRIMARY'
mysql> delimiter ;

-- 查看数据------
mysql> select * from mytest ;
+----+------+
| id | name |
+----+------+
|  1 | 张三 |
|  2 | 李四 |
|  3 | 王五 |
| 10 | 张三 |
+----+------+
4 rows in set (0.00 sec)
```
可以发现由于主键的非空,唯一约束,所以没有做异常处理的会提示报错,这也导致后后面的本该正常插入的   insert into mytest values(20,'李四'),(30,'王五');也没法插入;
而做了异常处理的----(continue表示遇到该异常直接跳过,继续执行下条插入语句)会执行异常要求的内容;


### 定义条件的方式
**----使用declare 语句----**
```sql
declare 条件名 condition for 条件类型;
条件类型有两种表示方式---
-- 1, sqlstate 值  (该值并非随便的一个值而是跟系统底层的规定要符合)
declare 条件名 condition for sqlstate '42000';
-- 2,mysql_error_code 
declare 条件名 condition for 1148 ;
-- 1148 就是mysql_error_code所代表的值;
```
我们在写mysql语句时偶尔也会看到错误提示-----例如

```sql
ERROR 1062 (23000): Duplicate entry '10' for key 'mytest.PRIMARY'
```
sqlstate 后的值 为一个长度为5 的字符串' 42000'
my_error_code 的值为1062

### 定义处理层序的方式

```sql
--语法格式--
declare 处理类型 handler for  已经定义的条件名  set @info='定义的含义';
```

处理类型包括-:
continue---- 跳过错误(不处理错误)继续执行;
exit---- 遇到错误---->退出,也是默认的一种方式;
undo--- 遇到错误撤回之前额操作----可是在mysql中不支持该操作;

```sql
create procedure test1()
 begin
 declare insertin condition for 1062;
 declare undo handler for insertin set @info='重复值';
 insert into test001 values(10);
insert into test001 values(10);
 set@info='重复值1';
 end;;
> 1064 - You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'undo handler for insertin set @info='重复值';
   insert into test001 values(10' at line 4
> 时间: 0.001s
```

定义处理程序的几种方式-----


![在这里插入图片描述](https://img-blog.csdnimg.cn/29792716238a49c38a8e5ccb9971ae3d.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)总结起来就两种模式---->>>

> 直接定义------ 
> declare continue handler for  condition_name  set @info='异常解析';
>

> 先定义后调用---- 
> declare condition_name condition for mysql_error_code ;
> 或者--- declare condition_name condition for sqlstate  '42000'; 调用
> declare continue handler for condition_name set @info='异常解析';

两种方式选一种即可;



小练习----
为了简便起见建一张简单的表----

```sql

mysql> create table test(
    -> id int primary key ,
    -> name varchar(6),
    -> age tinyint not null
    -> );
Query OK, 0 rows affected (0.05 sec)
```
建立过程插入数据----

```sql
mysql>  create procedure mypro()
    ->      BEGIN
    ->    declare mycondition1 condition for 1048 ;
    ->    declare mycondition2 condition for 1062 ;
    ->    declare continue handler for mycondition1 set  @info='不能为空';
    ->     declare exit handler for mycondition2 set @info ='重复值出现,退出';
    ->    insert into test values(1,'张三',22);
    ->    insert into test values(2,'李四',26);
    ->     insert into test values(3,null,26);
    ->      insert into test values(4,'王五',28);
    ->     insert into test values(2,'王尔',46);
    ->     insert into test values(8,'王二尔',56);
    ->     end
    ->     ;;
```
查询插入的数据----

```sql
mysql> select * from test;
+----+------+-----+
| id | name | age |
+----+------+-----+
|  1 | 张三 |  22 |
|  2 | 李四 |  26 |
|  3 | NULL |  26 |
|  4 | 王五 |  28 |
+----+------+-----+
4 rows in set (0.00 sec)
```
可以看到在insert into test values(2,'王尔',46);时由于重复值出现,处理方式为退出,所以 insert into test values(8,'王二尔',56);并没有插入表格;



# 分区表
分区表中的分区就像是磁盘的分区一样,一块磁盘被分成C,D,E,F等,然后为这个分区分配不同的大小,但是分区表是一般不会指定大小的;


那么到底什么是分区表????
简单的将就是原本存储在一张表中的数据按某种逻辑进行拆分,这样可以将相关的数据放到一起;
分区表的应用场景----->>>
1,表非常大,以至于内存无法将全部的表数据放入,或者表数据只在最后部分有热点数据,其余均是历史数据;
2,分区表相对于整张表而言,更容易维护,因为对于分区表而言分区表的数据量要比整表的数据量少,对于批量删除数据可以清空分区表来达到快速删除的目的;
3,分区表可以分布在不同的物理设备上,从而搞笑的利用多个硬件设备;
4,可以使用分区表来彼岸某些特殊瓶颈,例如innodb的单个索引的互斥访问和ext3文件按系统的 inode锁竞争;
5,可以备份和恢复独立的分区;

## 分区表的约束
分区表的约束---->
一个表最多只能有1024个分区;
如果分区字段中有主键后者唯一索引,那么所有的主键和唯一索引列都必须包含进来;
分区表无法使用外键约束;

## 分区表的操作逻辑

select  锁住所有底层表,优化器先判断是否可以过滤部分分区,然后再访问各个分区的数据;
insert锁住所有底层分区,判断插入数据在哪个分区,在将记录写入表中;
delete先锁住多有底层表,确定数据所在分区,然后找到---删除
update 锁住 所有底层表,确定要更新的数据再哪个分区,取出数据并更新再判断更新的数据应该再哪个分区;再进行底层表的写入操作,并对源数据所在底层表进行删除操作;

有些操作是支持过滤的,如果where条件和分区表达式型匹配,则会过滤掉不符合的数据;

注意-----虽然CURD都会锁住所有底层表,但是并不是说mysql的存储引擎在处理数据的时候一定会这么做,如果存储引擎能实现行级锁,则会在分区层释放对应的表锁;
## 分区表的类型
MySQL支持的分区类型一共有四种：RANGE，LIST，HASH，KEY。其中，RANGE又可分为原生RANGE和RANGE COLUMNS，LIST分为原生LIST和LIST COLUMNS，HASH分为原生HASH和LINEAR HASH，KEY包含原生KEY和LINEAR HASH。

 

## 分区表的实现方式

主要是以下几种：

1. 基于RANGE
2. 基于RANGE COLUMNS
3. 基于LIST  
4. 基于LISTCOLUMNS
5. 基于HASH
6. 基于KEY

 **基于范围的分区**
```sql

--创建分区表
 CREATE TABLE ppp (id  INT,NAME VARCHAR(6) ,gender ENUM("男","女") )PARTITION BY RANGE(id)(
   PARTITION p0 VALUES LESS THAN (5),
    PARTITION p1 VALUES LESS THAN (8),
    PARTITION p2 VALUES LESS THAN (10),
  PARTITION p3 VALUES LESS THAN (15));
```
**上表根据store_id划分分区表------>>>范围划分
5以下,
5到8,
10到15,
15以上**
为了简单,实际工作中不会这么分的;
插入数据------>

```sql

mysql> delimiter ;
mysql> delimiter //
mysql> create procedure addData()
    -> begin
    -> declare i int ;
    -> set i=1 ;
    -> while i<15
    -> do
    -> insert into ppp (id ) values (i);
    -> set i =i+1 ;
    -> end while ;
    -> end //
Query OK, 0 rows affected (0.04 sec)

mysql> delimiter ;
mysql> call addData ;
Query OK, 1 row affected (0.10 sec)
```
解析数据------

```sql

mysql> explain select * from ppp \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: ppp
   partitions: p0,p1,p2,p3
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)

```
如果查找id小于5的数据----执行解析

```sql

mysql> explain select * from ppp  where id<5 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: ppp
   partitions: p0
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 4
     filtered: 33.33
        Extra: Using where
1 row in set, 1 warning (0.00 sec)
```
**可以发现存储引擎对数据做了过滤,不加载不需要的分区数据;**
可想而知,如果数据量很大的时候,筛选数据的效率会有一定提升;
再来建立一个带有索引的表

```java
mysql> start transaction ;
Query OK, 0 rows affected (0.00 sec)

mysql> delimiter //
mysql> create procedure addD()
    -> begin
    -> declare i int ;
    -> set i=0 ;
    -> while (i<15)
    -> do
    -> insert into ppp2 (id) values(i) ;
    -> set i=i+1 ;
    -> end while ;
    -> end //
Query OK, 0 rows affected (0.03 sec)

mysql> commit ;
    -> //
Query OK, 0 rows affected (0.00 sec)
```
分区表上是可以用到索引的
```sql

mysql> explain select * from ppp2  where id=10 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: ppp2
   partitions: p3
         type: const
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 4
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)
```
接下来继续插入数据-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/b40d994157c44a8fa5750487e9d2a990.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```sql

mysql> ALTER TABLE ppp2 add index pt_index (name);
Query OK, 0 rows affected (0.14 sec)
Records: 0  Duplicates: 0  Warnings: 0
--- 索引覆盖
mysql> explain select id ,name from ppp2 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: ppp2
   partitions: p0,p1,p2,p3
         type: index
possible_keys: NULL
          key: pt_index
      key_len: 27
          ref: NULL
         rows: 15
     filtered: 100.00
        Extra: Using index
1 row in set, 1 warning (0.00 sec)
---- 当查询条件与分表条件不匹配的时候,并不会过滤分区表而是进行全表扫描
mysql> explain select  id ,name from ppp2 where gender='男' \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: ppp2
   partitions: p0,p1,p2,p3
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 15
     filtered: 50.00
        Extra: Using where
1 row in set, 1 warning (0.00 sec)

```

极限值的设定

上表中有一个问题是当插入id=15的数据时是无法插入的,因为最大值被限定在14,解决方案是   将最后一个分区表的限制改为----- less than  maxvalue


关于分区表,可以根据id int 及进行分区,那么可以用字段为字符型数据来进行分区吗?显然是不一定可以,

基于range的分区表-----

```sql
create table pojo3 (id int ,name varchar(18),age int) partition by range  (name)(  partition p values less than("aa") ,partition p1 values less than ("tt") )
>error 1697 - VALUES value for partition 'p' must have type INT
> 时间: 0.001s

```
基于range的分区表 条件 处仅支持int 型数据,

既然这样,那可不可以
尝试 length(name)按照字符串长度来分表
建表语句---

```sql

mysql>  CREATE TABLE ppp3 (id  INT,NAME VARCHAR(66) ,gender ENUM("男","女") )PARTITION BY RANGE(length(name))(
    ->    PARTITION p0 VALUES LESS THAN (6),
    ->     PARTITION p1 VALUES LESS THAN (8),
    ->     PARTITION p2 VALUES LESS THAN (10),
    ->   PARTITION p3 VALUES LESS THAN (15));
ERROR 1564 (HY000): This partition function is not allowed
mysql>

```
发现不允许,那么就没有别的办法了,range分区表确实不允许;
基于range columns 的分区表------
```sql
	create table pojo2 (id int ,name varchar(18),age int) partition by range columns (name)(  partition p values less than("aa") ,partition p1 values less than ("tt") );
```
可以;


**分区表可以用字段为时间的数据进行分区----**
建表-----

```sql
CREATE TABLE ppp3 (id  INT,NAME VARCHAR(66) ,gender ENUM("男","女") ,birthday date )PARTITION BY RANGE(year(birthday))(
   PARTITION p0 VALUES LESS THAN (1990),
    PARTITION p1 VALUES LESS THAN (1995),
    PARTITION p2 VALUES LESS THAN (1996),
  PARTITION p3 VALUES LESS THAN (1997),
	PARTITION p4 VALUES LESS THAN MAXVALUE)
> OK
> 时间: 0.158s
```
插入数据----
![在这里插入图片描述](https://img-blog.csdnimg.cn/65056d2fa2ce4cbb842cd93fcc7dbc6b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_18,color_FFFFFF,t_70,g_se,x_16)

执行解析----
```sql
mysql> explain select * from ppp3 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: ppp3
   partitions: p0,p1,p2,p3,p4
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 9
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)
```
说明分区设置没问题,

## 为分区表设置索引
唯一索引------

```sql
mysql> alter table ppp3  add  unique index un_index (id) ;
ERROR 1503 (HY000): A UNIQUE INDEX must include all columns in the table's partitioning function (prefixed columns are not considered).
```

> **要求是UNIQUE INDEX必须包含表分区函数中的所有列(不考虑带前缀的列)。**

即如果要添加唯一索引,那么只能是分区所在所有字段,上表为birthday,
执行----
```sql
mysql>  alter table ppp3 add unique index un_index (birthday) ;
Query OK, 0 rows affected (0.11 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

主键索引------

```sql
mysql> alter table ppp3 modify id int primary key ;
ERROR 1503 (HY000): A PRIMARY KEY must include all columns in the table's partitioning function (prefixed columns are not considered).
```

> **要求是PRIMARY KEY必须包含表分区函数中的所有列(不考虑带前缀的列)。**

```sql
mysql> alter table ppp3 add primary key (birthday);
Query OK, 0 rows affected (0.50 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

普通索引-----

```sql
mysql>  alter table ppp3 add index pt_index (id) ;
Query OK, 0 rows affected (37.94 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
普通索引的添加对字段没有特别的要求;

联合索引

```sql
mysql> alter table ppp3 add unique index un_index(id,name,birthday) ;
Query OK, 0 rows affected (0.11 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
为分区表添加唯一索引必须要包含分区时的所用的字段;

全文索引

一般也不使用;

## 基于RANGE的分区表
 分区字段的数据类型------整型int

```sql
CREATE TABLE ppp3 (id  INT,NAME VARCHAR(66) ,gender ENUM("男","女") ,birthday date )PARTITION BY RANGE(year(birthday))(
   PARTITION p0 VALUES LESS THAN (1990),
    PARTITION p1 VALUES LESS THAN (1995),
    PARTITION p2 VALUES LESS THAN (1996),
  PARTITION p3 VALUES LESS THAN (1997),
	PARTITION p4 VALUES LESS THAN MAXVALUE);
```

>
> 有些字段的数据类型可以通过函数来转换为整型-----比如上表中的birthday字段,通过year()转换为整型;

如果使用浮点型会怎样呢?-----不允许按浮点型分区;同时分区表中 分区条件也必须为int 类型数据; ![partition p values less than ()](https://img-blog.csdnimg.cn/222c12989aba4c7f9f38b7730a521d42.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
```sql
CREATE TABLE ppp5 (id  float primary key,NAME VARCHAR(66) ,gender ENUM("男","女") ,birthday date )PARTITION BY RANGE(id)(
   PARTITION p0 VALUES LESS THAN (1990),
    PARTITION p1 VALUES LESS THAN (1995),
    PARTITION p2 VALUES LESS THAN (1996),
  PARTITION p3 VALUES LESS THAN (1997),
	PARTITION p4 VALUES LESS THAN MAXVALUE)

> error 1659 - Field 'id' is of a not allowed type for this type of partitioning
> 时间: 0.054s
```
也可以用时间戳函数将时间转换为数字;
> **基于范围的分区,对于分区表达式,可以使用操作函数基于date,time,或者datetime列来返回一个整数值;**

## 基于LIST的分区
。这是分区的一个变体，它允许使用多个列作为分区键，并且用于除整数类型以外的数据类型列用作分区列：您可以使用字符串类型、日期和日期时间列。
```sql
 CREATE TABLE ppp10 (id int ,NAME VARCHAR(66) ,gender ENUM("男","女") ,birthday date )PARTITION BY LIST(id)(
   PARTITION p0 VALUES in  (1,3,5,7,9),
    PARTITION p1 VALUES in (2,4,6,8,10),
  	PARTITION p3 VALUES in (12,14,16,18,20));
```



```sql
CREATE TABLE ppp10 (id  float ,NAME VARCHAR(66) ,gender ENUM("男","女") ,birthday date )PARTITION BY LIST((YEAR(birthday))(
   PARTITION p0 VALUES in  (1990,1991,1992,1993),
    PARTITION p1 VALUES in (1994,1995,1996,1997,1998),
  	PARTITION p3 VALUES in (1999,2001,2002,2000))
> 1064 - You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '(
     PARTITION p0 VALUES in  (1990,1991,1992,1993),
      PARTITION p1 VALUES in' at line 1
```
**列表分区的应用**
在列表分区中，每个分区的定义和选择基于一组值列表中的列值的成员数，而不是一组连续值范围中的列值。
RANGEPARTITION BY LIST(expr)VALUES IN (value_list)
与按范围定义的分区不同，列表分区不需要按任何特定顺序申报---即列表的定义顺序不做排序要求;

应用场景------一家公司的代理商信息,同一区域的放在一张表中

```sql
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY LIST(store_id) (
    PARTITION pNorth VALUES IN (3,5,6,9,17),
    PARTITION pEast VALUES IN (1,2,10,11,19,20),
    PARTITION pWest VALUES IN (4,12,13,14,18),
    PARTITION pCentral VALUES IN (7,8,15,16)
);
```
这样，添加或丢弃与特定区域相关的记录就很容易。
例如，假设西部地区的所有代理商都出售给另一家公司。

在 MySQL 8.0 中，所有与在该地区商店工作的员工相关的行都可以通过查询删除，该查询的执行效率比等效的 DeletE语句要高得多。（使用时还会删除所有这些行，但也会从表的定义中删除分区;您需要使用语句来恢复表的原始分区方案。

**删除分区表------>>>>**

```sql
ALTER TABLE employees TRUNCATE PARTITION pWestDELETE FROM employees WHERE store_id IN (4,12,13,14,18);
ALTER TABLE employees DROP PARTITION pWestpWest
```

**添加分区表------>**

```sql
ALTER TABLE ... ADD PARTITION
```
练习----->
当插入数据是  in范围以外的----

```sql
mysql> CREATE TABLE h2 (
    ->   c1 INT,
    ->   c2 INT
    -> )
    -> PARTITION BY LIST(c1) (
    ->   PARTITION p0 VALUES IN (1, 4, 7),
    ->   PARTITION p1 VALUES IN (2, 5, 8)
    -> );
Query OK, 0 rows affected (0.11 sec)

mysql> INSERT INTO h2 VALUES (3, 5);
ERROR 1525 (HY000): Table has no partition for value 3
INSERT IGNORE into h2 VALUES (3, 5)
> Affected rows: 0
> 时间: 0s

mysql> select * from h2;
Empty set (0.00 sec)
```
可以看到当插入数据有不符合的时候是不会将数据插入到表中的,即使用了ignore;

```sql

mysql> INSERT IGNORE INTO h2 VALUES (2, 5), (6, 10), (7, 5), (3, 1), (1, 9);
Query OK, 3 rows affected, 2 warnings (0.03 sec)
Records: 5  Duplicates: 2  Warnings: 2
```

分析一下能插入到表中的数据-----
2 在p1分区,6不会插入,7在P0分区,3不插入,1在P0分区
所以插入的数据----

2,5
7,5
1,9

按照表中存放的顺序显示应该是------p0,p1
7,5
1,9
2,5
> 当使用单个插入语句将多个行插入单个InnoDB表时，InnoDB将语句视为单个事务，以便任何无与伦比值的存在会导致语句完全失败，因此不会插入任何行。
>
> 您可以使用关键字IGNORE来忽略此类型的错误。如果这样做，则不插入包含无与伦比的分区列值的行，但插入任何具有匹配值的行，并且不会报告任何错误：


## 基于range colunms的分区表
使用date或者datatime作为分区列

```sql
CREATE TABLE ppp8 (id  float ,NAME VARCHAR(66) ,gender ENUM("男","女") ,birthday date )PARTITION BY range COLUMNS(birthday)(
   PARTITION p0 VALUES LESS THAN ('1990-10-19'),
    PARTITION p1 VALUES LESS THAN ('1999-10-19'),
    PARTITION p2 VALUES LESS THAN ('2010-10-19'),
	PARTITION p3 VALUES LESS THAN MAXVALUE)
> OK
> 时间: 0.109s

```
**这种方式分区只接受列不接收表达式**

验证如下-----
```sql
mysql>  CREATE TABLE ppp9 (id  float ,
NAME VARCHAR(66) ,
gender ENUM("男","女") ,
birthday date )
PARTITION BY RANGE COLUMNS((YEAR(birthday))(  
 PARTITION p0 VALUES LESS THAN (1990),  
   PARTITION p1 VALUES LESS THAN (1993),   
    PARTITION p2 VALUES LESS THAN (1995), 
     PARTITION p3 VALUES LESS THAN (1997),
     PARTITION p4 VALUES LESS THAN MAXVALUE);

ERROR 1064 (42000): You have an error in your SQL syntax; 
check the manual that corresponds to your
 MySQL server version for the right syntax to use near 
 '(YEAR(birthday))(   
 PARTITION p0 VALUES LESS THAN (1990),   
  PARTITION p1 VALUES' at line 1
```
```sql
 CREATE TABLE rcx (
      a INT,
       b INT,
       c CHAR(3),
        d INT
    )
    PARTITION BY RANGE COLUMNS(a,d,c) (
       PARTITION p0 VALUES LESS THAN (5,10,'ggg'),
        PARTITION p1 VALUES LESS THAN (10,20,'mmm'),
      PARTITION p2 VALUES LESS THAN (15,30,'sss'),
       PARTITION p3 VALUES LESS THAN (MAXVALUE,MAXVALUE,MAXVALUE)
   );
```
 **范围列分区的应用--------range columns**
范围列分区类似于范围分区，但允许您使用**基于多个列值的范围定义分区**。此外，您可以**使用整数类型以外的类型列定义范围**。
RANGE COLUMNS不支持 使用除date或date time以外的日期或时间类型的分区列。同时分区条件不支持表达式;

**RANGE COLUMNS分区与RANGE分区在以下方式上有显著差异：**

**RANGE COLUMNS不接受表达式，只接受列名。**

**RANGE COLUMNS接受一个或多个列的作为分区字段**。

**RANGE COLUMNS分区基于（列值列表）之间的比较，而不是缩放值之间的比较。将行放置在分区中也基于列值列表之间的比较：**

**RANGE COLUMNS分区列不限于整数列;       
字符串、日期和日期时间列也可用作分区列**

```sql
CREATE TABLE members (
    firstname VARCHAR(25) NOT NULL,
    lastname VARCHAR(25) NOT NULL,
    username VARCHAR(16) NOT NULL,
    email VARCHAR(35),
    joined DATE NOT NULL
)
PARTITION BY RANGE COLUMNS(joined) (
    PARTITION p0 VALUES LESS THAN ('1960-01-01'),
    PARTITION p1 VALUES LESS THAN ('1970-01-01'),
    PARTITION p2 VALUES LESS THAN ('1980-01-01'),
    PARTITION p3 VALUES LESS THAN ('1990-01-01'),
    PARTITION p4 VALUES LESS THAN MAXVALUE
);
```
range columns主要用于按照时间来进行划分分区的操作,相比于range,
range  需要借助表达式来完成对于时间字段的区分并进行分区;

range可以借助  year(),month(),week()等函数.也可以借助 UNIX_TIMESTAMP()
**在 MySQL 8.0 中，不允许使用任何其他涉及TIMESTAMP值的表达方式。**


```sql
CREATE TABLE quarterly_report_status (
    report_id INT NOT NULL,
    report_status VARCHAR(20) NOT NULL,
    report_updated TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
PARTITION BY RANGE ( UNIX_TIMESTAMP(report_updated) ) (
    PARTITION p0 VALUES LESS THAN ( UNIX_TIMESTAMP('2008-01-01 00:00:00') ),
    PARTITION p1 VALUES LESS THAN ( UNIX_TIMESTAMP('2008-04-01 00:00:00') ),
    PARTITION p2 VALUES LESS THAN ( UNIX_TIMESTAMP('2008-07-01 00:00:00') ),
    PARTITION p3 VALUES LESS THAN ( UNIX_TIMESTAMP('2008-10-01 00:00:00') ),
    PARTITION p4 VALUES LESS THAN ( UNIX_TIMESTAMP('2009-01-01 00:00:00') ),
    PARTITION p5 VALUES LESS THAN ( UNIX_TIMESTAMP('2009-04-01 00:00:00') ),
    PARTITION p6 VALUES LESS THAN ( UNIX_TIMESTAMP('2009-07-01 00:00:00') ),
    PARTITION p7 VALUES LESS THAN ( UNIX_TIMESTAMP('2009-10-01 00:00:00') ),
    PARTITION p8 VALUES LESS THAN ( UNIX_TIMESTAMP('2010-01-01 00:00:00') ),
    PARTITION p9 VALUES LESS THAN (MAXVALUE)
);
```

**范围分区的应用-----------range**

按范围划分的表以每个分区包含**分区表达值位于给定范围内的行**的方式进行分区。范围应是**连续的，但不应重叠，**并且使用操作员进行定义
建立一张员工信息表,根据每位员工离开公司的年份进行划分分区------>>>>>
```sql
CREATE TABLE employees (
    id INT NOT NULL,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NOT NULL DEFAULT '9999-12-31',
    job_code INT,
    store_id INT
)
PARTITION BY RANGE ( YEAR(separated) ) (
    PARTITION p0 VALUES LESS THAN (1991),
    PARTITION p1 VALUES LESS THAN (1996),
    PARTITION p2 VALUES LESS THAN (2001),
    PARTITION p3 VALUES LESS THAN MAXVALUE
);
```
1,提高查找效率-----假如我要查询在1996年离职员工,那么就可以只加载P1分区(即过滤掉非P1分区)
2,如果要批量删除数据,比如删除1991年之前离职的员工信息-----
有两种操作方式-----
方式一-------DELETE FROM employees WHERE YEAR(separated) <= 1990
方式二----- alter table employees drop partition p0
 如果数据量很多的话,方式二删除分区的效率要高;
![在这里插入图片描述](https://img-blog.csdnimg.cn/73a2790346de4f5b947c4fbe8808a23d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)总结------->>>>
range -----分区字段------>>>单独的int 字段   ,借助函数的date和datetime字段  不过时间型的数据要借助函数转换成整数;
## range 与range columns对比
range 单字段,range columns多字段
```sql
CREATE TABLE r1 (
    a INT,
    b INT
)
PARTITION BY RANGE (a)  (
    PARTITION p0 VALUES LESS THAN (5),
    PARTITION p1 VALUES LESS THAN (MAXVALUE)
);
```
插入数据-----

```sql
INSERT INTO r1 VALUES (5,10), (5,11), (5,12);
```
查看数分布列

```sql
mysql> SELECT PARTITION_NAME,TABLE_ROWS
    ->      FROM INFORMATION_SCHEMA.PARTITIONS
    ->         WHERE TABLE_NAME = 'r1';
+----------------+------------+
| PARTITION_NAME | TABLE_ROWS |
+----------------+------------+
| p0             |          0 |
| p1             |          3 |
+----------------+------------+
2 rows in set (0.02 sec)

```


建表----->

```sql
CREATE TABLE rc1 (
    a INT,
    b INT
)
PARTITION BY RANGE COLUMNS(a, b) (
    PARTITION p0 VALUES LESS THAN (5, 12),
    PARTITION p3 VALUES LESS THAN (MAXVALUE, MAXVALUE)
);
```
插入数据

```sql
INSERT INTO rc1 VALUES (5,10), (5,11), (5,12);
```

分析数据的分布情况
第一组数据5,10 ,5应该放在p3分区,12应该放在p0分区,那问题来了该放在哪里呢?
先来看-----数据分布
```sql
mysql>
mysql> SELECT PARTITION_NAME,TABLE_ROWS
    ->      FROM INFORMATION_SCHEMA.PARTITIONS
    ->         WHERE TABLE_NAME = 'rc1';
+----------------+------------+
| PARTITION_NAME | TABLE_ROWS |
+----------------+------------+
| p0             |          2 |
| p3             |          1 |
+----------------+------------+
2 rows in set (0.02 sec)
```
p0分区有两个,p3分区有一个,
执行以下语句------>>>>
```sql

mysql> SELECT (5,10) < (5,12), (5,11) < (5,12), (5,12) < (5,12);
+-----------------+-----------------+-----------------+
| (5,10) < (5,12) | (5,11) < (5,12) | (5,12) < (5,12) |
+-----------------+-----------------+-----------------+
|               1 |               1 |               0 |
+-----------------+-----------------+-----------------+
1 row in set (0.00 sec)
```
可以看到对应的大小;
所以 5,10   5,11应该放在P0分区,5,12放在P3分区;
同理可以按照此方式来进行比较
![在这里插入图片描述](https://img-blog.csdnimg.cn/0e4bdda8a7744d6fa74d357e8ad2b6d4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## 基于hash 的分区表
将分区字段的这些列值进行hash计算,得出的数据会进行分配到相应的分区种(可以自己指定分区个数);
分区主要用于确保在预定数量的分区中均匀地分配数据。在范围或列表分区中，必须明确指定应存储给定列值或列值集的分区
**HASH PARTITION BY HASH (expr)PARTITIONS num其中num是一个正整数，表示要划分表的分区数。 其中expr部分要求返回值为一个整数**
```sql
	create table pojo4 (id int ,name varchar(18),age int,birthday date) partition by hash(year(birthday)) partitions 5
> OK
> 时间: 0.118s
create table pojo5 (id int ,name varchar(18),age int,birthday date) partition by hash(id) partitions 5
> OK
> 时间: 0.106s

```

```sql
## 使用时，存储引擎根据表达结果的模态确定要使用的num分区的哪个分区。换句话说，对于给定的表达前，存储记录的分区是分区编号N，其中。假设该表的定义如下，因此它有 4 个分区：PARTITION BY HASHN = MOD(expr, num)t1

CREATE TABLE t1 (col1 INT, col2 CHAR(5), col3 DATE)
    PARTITION BY HASH( YEAR(col3) )
    PARTITIONS 4;
## 如果将记录插入其值，则存储该记录的分区将确定如下：t1col3'2005-09-15'

MOD(YEAR('2005-09-01'),4)
=  MOD(2005,4)
=  1
```

## 基于key的分区
key分区字段必须有一列或多列包含整数值

KEY只取零或更多列名列表。用作分区键的任何列必须包含部分或表的所有主密钥（如果表有一个）。如果没有列名指定为分区键时，如果有列名称，则使用表的主键;
```sql
create table pojo6 (id int primary key ,name varchar(18),age int,birthday date) partition by LINEAR key(id) partitions 5
> OK
> 时间: 0.135s

```
key不指定的情况下要在表的结构中有唯一键或者主键,否则会出现错误

> ERROR 1488 (HY000): Field in list of fields for partition function not
> found in table
>

有主键
```sql
CREATE TABLE k1 (
    id INT NOT NULL PRIMARY KEY,
    name VARCHAR(20)
)
PARTITION BY KEY()
PARTITIONS 2;
```
有唯一键
```sql
CREATE TABLE k1 (
    id INT NOT NULL,
    name VARCHAR(20),
    UNIQUE KEY (id)
)
PARTITION BY KEY()
PARTITIONS 2;
```

可以按线性键划分表。下面是一个简单的示例：


```sql
CREATE TABLE tk (
    col1 INT NOT NULL,
    col2 CHAR(5),
    col3 DATE
)
PARTITION BY LINEAR KEY (col1)
PARTITIONS 3;
```

关键字对分区的影响与对分区的影响相同，分区编号使用两种算法而不是算术来推导







# 注入攻击

> SQL注入攻击是黑客对数据库进行攻击的常用手段之一。随着B/S模式应用开发的发展，使用这种模式编写应用程序的程序员也越来越多。
> SQL注入攻击属于数据库安全攻击手段之一，可以通过数据库安全防护技术实现有效防护，数据库安全防护技术包括：数据库漏扫、数据库加密、数据库防火墙、数据脱敏、数据库安全审计系统。
> SQL注入攻击会导致的数据库安全风险包括：刷库、拖库、撞库。

**注入攻击产生的根源**

> 由于在编写代码的时候，**没有对用户输入数据的合法性进行判断，使应用程序存在安全隐患**。用户可以提交一段数据库查询代码，根据程序返回的结果，获得某些他想得知的数据，这就是所谓的SQL Injection，即SQL注入。

**模拟sql注入攻击**

模拟环境是 有一个网站,有了一定量的用户数,
某一天用户张三登陆了该网站

该网站的后台数据库----
有这样一张表,
![在这里插入图片描述](https://img-blog.csdnimg.cn/2358e8f13e0747b1a3987a6a2f5622c9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)先来模拟一下张三登陆的过程;
**准备工作,建立一个实体类usertest,**

```java
package com.test.injection;
//实体类
public class Usertest {
    private Integer uid;
    private String useracount;
    private String userpassword;
    private Double usermoney;
//空参构造
    public Usertest() {
    }
//账户名--密码
    public Usertest(String useracount, String userpassword) {
        this.useracount = useracount;
        this.userpassword = userpassword;
    }
//账户-----余额

    public Usertest(String useracount, Double usermoney) {
        this.useracount = useracount;
        this.usermoney = usermoney;
    }

    //全部信息
    public Usertest(Integer uid, String useracount, String userpassword, Double usermoney) {
        this.uid = uid;
        this.useracount = useracount;
        this.userpassword = userpassword;
        this.usermoney = usermoney;
    }

    public Integer getUid() {
        return uid;
    }

    public void setUid(Integer uid) {
        this.uid = uid;
    }

    public String getUseracount() {
        return useracount;
    }

    public void setUseracount(String useracount) {
        this.useracount = useracount;
    }

    public String getUserpassword() {
        return userpassword;
    }

    public void setUserpassword(String userpassword) {
        this.userpassword = userpassword;
    }

    public Double getUsermoney() {
        return usermoney;
    }

    public void setUsermoney(Double usermoney) {
        this.usermoney = usermoney;
    }
    @Override
    public String toString() {
        return "Usertest{" +
                "uid=" + uid +
                ", useracount='" + useracount + '\'' +
                ", userpassword='" + userpassword + '\'' +
                ", usermoney=" + usermoney +
                '}';
    }


```
然后准备数据库连接的类---

```java
import java.sql.*;
import java.util.Scanner;

public class testInject {
    private final static String URL = "jdbc:mysql://localhost:3306/gavin";
    private final static String USER = "gavin";
    private final static String PASSWORD = "955945";
    static Connection connection=null;
    static Statement statement=null;
    static ResultSet resultSet=null;
    static Usertest user=null;// 有这么一个用户
    static String account ;
    static String password;
    //登录-----
    public static  Usertest getAccount(String usera,String pwd) throws Exception {
      Class.forName("com.mysql.cj.jdbc.Driver");
         connection = DriverManager.getConnection(URL, USER, PASSWORD);
         statement = connection.createStatement();
        String sql="select * from usertest where useraccout='"+usera+"' and userpassword ='"+pwd+"';" ;
        // System.out.println(sql);执行前查看sql语句
         resultSet = statement.executeQuery(sql);
         while (resultSet.next()){
            account= resultSet.getString("useraccout");
           password=resultSet.getString("userpassword");
           user= new Usertest(account,password);
         }
         closeLink(resultSet,statement,connection);
        return user;
    }
   public  static void closeLink(ResultSet resultSet,Statement statement, Connection connection) {
        if(null!=resultSet){
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
       if(null!=statement){
           try {
               statement.close();
           } catch (SQLException e) {
               e.printStackTrace();
           }
       }
       if(null!=connection){
           try {
               connection.close();
           } catch (SQLException e) {
               e.printStackTrace();
           }
       }
    }
    public static void main(String[] args) throws Exception {

        Scanner sc= new Scanner(System.in);
        System.out.println("请输入账号----");
        String name = sc.next();
        System.out.println("请输入密码----");
        String password=sc.next();

        System.out.println(  null!=getAccount(name,password)?"登录成功":"登陆失败");

    }
}

```
测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/74d98dd95a4c47c28e74d955d0793303.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)再次测试------

![在这里插入图片描述](https://img-blog.csdnimg.cn/e3ef576850ef4092aaeb7f3605b8d0a8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

可以发现即使密码输入的不正确,也登录进了系统;
这就是注入攻击,让我们来看看sql语句是怎样执行的,
将输入sql的语句打开,
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2ba0946bc71415f9fc7f4c944a4289d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以看到sql语句的执行时后面多了一个条件,m=m
这样就改变了sql的执行逻辑-----false  or true de 结果为 true

这种情况可以查看到数据库中的所有用户;

**防止sql注入攻击**

首先我们来看,问题产生的根源就是这条sql语句,由于是字符串拼接而成,那么使得我们在字符串末尾添加任何内容都成为了可能;
![在这里插入图片描述](https://img-blog.csdnimg.cn/d65dcd21289746668030f185e93e7594.png)
通过PrepareStatement预编译的执行器来执行该sql,
这样sql的格式就会发生改动;

```java
 String SQL="select * from usertest where useraccout= ?and userpassword = ?;" ;
```
其中   ? 表示占位符,
预编译的形式是先将sql语句进行预处理,然后执行的时候先将占位符代表的内容填充然后执行,

具体的代码改动---
![在这里插入图片描述](https://img-blog.csdnimg.cn/588fb0a980ab4935b44579414b5455d4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)运行结果-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e12c5d5f43f4229954a9ab9259fa4e4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/44beb1a87abc41ef8989a342aff84a38.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以看到注入攻击解决了,同时获取的sql语句也更加安全了;

 

# 索引对数据库的性能的影响

适当的索引能够提高查询效率,但是如果设置不当可能会适得其反,所以在设置索引的时候一定要谨慎.

**要说为数据库选择合适的索引其实是一项比较复杂的任务。**

如果索引列较少,则需要的磁盘空间和维护开销就相对较小。
如果索引较多,那么频繁的增删查改会使得索引文件大小快速增加。
所以索引的设置要尽可能的覆盖到更多的查询记录。

因此要多次尝试,结合性能分析-----performance_schema来比较个索引之间的查询效率，才能找到最有效,合适的的索引。

另一个索引的长度(字符串类型的数据可以设置索引长度)也会对数据库的性能产生影响,索引长度短的查询起来就快,但并非越短越好,

**------过短会使得数据查询不精确,
------过长查询效率又得不到有效提升还占用存储空间;**


## 优化字段存储----优化查询效率

除了建立索引还可以通过优化字段存储来优化查询效率,
首先我们要知道每个字段存储的数据类型之间的区别以及数据类型占用的空间;

### 数据类型的选择----简单就好

所以优化查询效率的一种方式-----在同等的展销效果下尽量减少数据的存储大小,比如int 类型的有tinyint ,smallint....等等,在创建表的时候尽量使用能满足需求的最小数据类型,一是能够节省空间,另一个能够加快查询效率;

一个很简单的场景,在人眼感知范围内 打开一张1M的图片和一张10M的图片那个更快?------答案很明显了;

**实际应用-----**
比如存储年龄,tinyint就可以满足了;

### 是什么类型的就用什么类型存储
int,date,char等类型的数据可以转为字符串存储,虽然可以但是不建议这么做,因为不同的数据类型的校对规则是不一样的,字符类的要更复杂一些,所以在查询的时候尽可能是什么类型就存储成什么类型;


实验数据----
有两张表,emp2和emp3,存储数据类型见表,存储数据一致;
```sql
mysql> desc emp2;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| EMPNO    | int         | NO   |     | NULL    |       |
| ENAME    | varchar(10) | YES  |     | NULL    |       |
| JOB      | varchar(9)  | YES  |     | NULL    |       |
| MGR      | int         | YES  |     | NULL    |       |
| HIREDATE | varchar(64) | YES  |     | NULL    |       |
| SAL      | double(7,2) | YES  |     | NULL    |       |
| COMM     | double(7,2) | YES  |     | NULL    |       |
| DEPTNO   | int         | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
8 rows in set (0.00 sec)

mysql> desc emp3;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| EMPNO    | int         | NO   |     | NULL    |       |
| ENAME    | varchar(10) | YES  |     | NULL    |       |
| JOB      | varchar(9)  | YES  |     | NULL    |       |
| MGR      | int         | YES  |     | NULL    |       |
| hiredate | date        | YES  |     | NULL    |       |
| SAL      | double(7,2) | YES  |     | NULL    |       |
| COMM     | double(7,2) | YES  |     | NULL    |       |
| DEPTNO   | int         | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
8 rows in set (0.03 sec)
```


下面来做一个实验----

![在这里插入图片描述](https://img-blog.csdnimg.cn/9fd8cbe14aad48a8abdce13c2e5593b9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以看到查询时间上的差别,varchar 比date耗时长一些;所以是什么数据类型就用什么数据类型存储;

### 尽量避免 null
在mysql中null=null是不成立的,null<=>null才成立;
而且在计算含有null 的列的时候可能会出现与预期结果不一致的情况,比如 有些函数将null排除在外不做计算,有的则返回null,

```sql
mysql> select ename 姓名, sal 底薪 ,comm 当月奖金 ,sal+comm 当月应发薪资  from emp;
+--------+---------+----------+--------------+
| 姓名   | 底薪    | 当月奖金 | 当月应发薪资 |
+--------+---------+----------+--------------+
| SMITH  |  800.00 |     NULL |         NULL |
| ALLEN  | 1600.00 |   300.00 |      1900.00 |
| WARD   | 1250.00 |   500.00 |      1750.00 |
| JONES  | 2975.00 |     NULL |         NULL |
| MARTIN | 1250.00 |  1400.00 |      2650.00 |
| BLAKE  | 2850.00 |     NULL |         NULL |
| CLARK  | 2450.00 |     NULL |         NULL |
| SCOTT  | 3000.00 |     NULL |         NULL |
| KING   | 5000.00 |     NULL |         NULL |
| TURNER | 1500.00 |     0.00 |      1500.00 |
| ADAMS  | 1100.00 |     NULL |         NULL |
| JAMES  |  950.00 |     NULL |         NULL |
| FORD   | 3000.00 |     NULL |         NULL |
| MILLER | 1300.00 |     NULL |         NULL |
+--------+---------+----------+--------------+
14 rows in set (0.00 sec)
```
以上结果当月薪资就返回了 null,但是现实中并不是完全不允许null的存在,遇到null需要所特殊处理-----

```sql

mysql> select ename 姓名, sal 底薪 ,comm 当月奖金 ,sal+ifnull(comm,0) 当月应发薪资  from emp;
+--------+---------+----------+--------------+
| 姓名   | 底薪    | 当月奖金 | 当月应发薪资 |
+--------+---------+----------+--------------+
| SMITH  |  800.00 |     NULL |       800.00 |
| ALLEN  | 1600.00 |   300.00 |      1900.00 |
| WARD   | 1250.00 |   500.00 |      1750.00 |
| JONES  | 2975.00 |     NULL |      2975.00 |
| MARTIN | 1250.00 |  1400.00 |      2650.00 |
| BLAKE  | 2850.00 |     NULL |      2850.00 |
| CLARK  | 2450.00 |     NULL |      2450.00 |
| SCOTT  | 3000.00 |     NULL |      3000.00 |
| KING   | 5000.00 |     NULL |      5000.00 |
| TURNER | 1500.00 |     0.00 |      1500.00 |
| ADAMS  | 1100.00 |     NULL |      1100.00 |
| JAMES  |  950.00 |     NULL |       950.00 |
| FORD   | 3000.00 |     NULL |      3000.00 |
| MILLER | 1300.00 |     NULL |      1300.00 |
+--------+---------+----------+--------------+
14 rows in set (0.00 sec)
```

**一般来说当索引列允许为null的时候,索引的存储空间比not null的存储空间要大,因为Null需要额外一个字节来存储,但是实际操作中索引
 not null 的性能比null的提高有限;**


### varchar 与char的食用场景

varchar 是可变存储,存储时需要额外一个字节来记录长度

**1,在存储字符串长度变化的数据时用varchar,
2,变更不频繁的字符串**

char 是定长,存储时删除两边的空格,如果长度不够,则填充空格;

**1,char 适合存储字符串长度波动不大的数据,比如加密算法中的md5生成的结果就适宜用char来存储;**
**2,变更频繁的字符串**

### text与blob的食用场景
text是长字符串
blob是二进制字符串

实际应用中并不会将这两类数据存放在数据库中,而是存放于文件系统中,数据库中存放的是该文件的地址;

原因是在数据库中检索这两类的数据效率太低了;


### timestamp与datetime的食用场景
1,timestamp 的时间范围要比datetime要少
2,timestamp-----即时间戳,包含UTC适合国际化场景,datetime不包含utc;


### 使用enum来代替字符串类型
应用场景---
1,性别
2,月份
3,周
等等
枚举的底层是通过键值对保存的;

注意一点----IP地址在数据库中并不是存储为字符串而是通过ip与数字准换函数将其转化为 整数存储----

```sql
mysql> select inet_aton('192.168.1.1') as "ip地址转整数" ,inet_ntoa(323223577) as '残缺的',inet_ntoa( inet_aton('192.168.1.1')) as '完美的';
+--------------+------------+-------------+
| ip地址转整数 | 残缺的     | 完美的      |
+--------------+------------+-------------+
|   3232235777 | 19.68.0.25 | 192.168.1.1 |
+--------------+------------+-------------+
1 row in set (0.00 sec)
```
数据库的优化主要分两方面，一方面是硬件层面的优化，另一方面是参数和语句方面的优化。
今天做一个小结，内容可能不全（也是我自己的理解，如有不得当之处还请及时指出，收到之后我会及时予以修正）

> 硬件层面


首先我们来看什么算是一台性能强悍的计算机----
首先是计算机（服务器）的配置不能太差 ，处理器--多核（貌似锐龙核多），磁盘读写速度--（固态>普通硬盘）。
由于现在的计算机大多是多核心并且支持并发任务处理的，（mysql也支持多线程） 因此想要提高对数据库的读写操作，因此更换一个读写速度更好的磁盘是一个选择。
其次如果是企业级别的最好是整个分布式的以提高并发处理能力。

> 参数层面
>
> 优化mysql查询语句，配置参数等 查询的时候优化查询语句 按查询速度排名先后 1，system  -   只有一行查询速度，且是系统表
> 2，const --只有一行的查询，查询条件涉及是primary key 或者unique 3，eq-ref联结查询
> ，查询条件涉及primary key 或者unique 4，ref联结查询，查询条件不涉及primary key 或者unique
> 5，联结查询，查询条件中有or，则两个条件必须都是primary key 或者unique，否则就会忽略primary key
> 或者unique 6，all 是最慢的，因为要全部扫描

> 
>
> 代码演示---表中数据如下
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210609114348249.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

```sql

mysql> desc hxjy ;
+-------------+-------------+------+-----+---------+-------+
| Field       | Type        | Null | Key | Default | Extra |
+-------------+-------------+------+-----+---------+-------+
| user_id     | int         | NO   | PRI | NULL    |       |
| user_name   | varchar(16) | YES  |     | NULL    |       |
| user_gender | char(4)     | YES  |     | NULL    |       |
| user_age    | tinyint     | YES  |     | NULL    |       |
| user_jobs   | varchar(32) | YES  |     | NULL    |       |
| user_salary | int         | YES  |     | NULL    |       |
+-------------+-------------+------+-----+---------+-------+
6 rows in set (0.00 sec)

mysql> explain select * from hxjy where  user_id =1011 ;
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+
----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows |
 filtered | Extra |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+
----------+-------+
|  1 | SIMPLE      | hxjy  | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |
   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+
----------+-------+
1 row in set, 1 warning (0.00 sec)

mysql>

```
扫描1行数据就得到我们要的结果，我们会发现type为const，
我们再来看一下条件中没有索引的查询

```sql

mysql> explain select * from hxjy where  user_name='赵' ;
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filt
ered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------------+
|  1 | SIMPLE      | hxjy  | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   12 |    1
0.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------------+
1 row in set, 1 warning (0.00 sec)
```

扫描12行数据，才得到我们的结果，我们也会发现type为all，
扫描数据小的要比扫描的多的快。
所以要设置合适的索引来提高查询速度

> 插入数据--- 由于插入数据时表要检查索引，唯一索引，键约束等 
> 1，禁用索引 
> 2，禁用唯一索引 
> 3，禁用外键约束

```sql
mysql> explain insert into hxjy values(4552,'蚂yi','男',24,'Worker',5600),(4575,'sdasd','女',23,'Wor
ker',5445),(3232,'sadasd','男',36,'Worker',4556);
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filt
ered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------+
|  1 | INSERT      | hxjy  | NULL       | ALL  | NULL          | NULL | NULL    | NULL | NULL |
NULL | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------+
1 row in set, 1 warning (0.00 sec)
```

```sql

mysql> alter table hxjy enable keys ;
Query OK, 0 rows affected, 1 warning (0.00 sec)
mysql> explain insert into hxjy values(475545,'蚂yi','男',24,'Worker',5600),(44521,'sdasd','女',23,'
Worker',5445),(34545,'sadasd','男',36,'Worker',4556);
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filt
ered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------+
|  1 | INSERT      | hxjy  | NULL       | ALL  | NULL          | NULL | NULL    | NULL | NULL |
NULL | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+-----
-----+-------+
1 row in set, 1 warning (0.00 sec)

```
可能是插入的数据量不够，所以显示不出时间上的区别，（或者又可以显示时间的方法，欢迎提点）
插入数据时如果是往新建的表中插入数据，可以不用禁用索引，因为索引是插入数据之后才建立的，如果表中已有数据，而且要插入大量数据，最好先禁用索引，适用于myisam和innodb

另Load data infile 导入数据要比插入数据快。


表的层面--
表中数据太多的时候对于经常查询的表的内容，可已将该内容拆到另一张表中，（如果要修改表数据可以建立触发条件来保持数据一致，或者建立视图）
用联结查询有时候不如直接建立一张新表快。这个看需要；

# performance_schema

带着疑问去学习-----performance_schema从何而来？
经过mysql的版本的更迭，在细节部分会逐渐完善，还记得之前怎样去查询语句的执行时间，IO的操作时间吗？

简单回顾一下----一张表，两个关键字（show profile/profiles）
![在这里插入图片描述](https://img-blog.csdnimg.cn/ba8428121aec4cc88d3423da11bf9264.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)剩下的不在写了，在mysql8.0中输入以上语句，返回的都是空的，以为8.0中默认没开启profiling,  开启方式-----set profiling =1;
并给出了警告提示

```sql

mysql> show profile for query 1 ;
Empty set, 1 warning (0.00 sec)
```
查看警告信息-----

```sql
-- deprecated  熟悉吧---被废弃的，将来可能会移除，非常符合oracle公司的风格 --
mysql> show warnings \G
*************************** 1. row ***************************
  Level: Warning
   Code: 1287
Message: 'SHOW PROFILE' is deprecated and will be removed in a future release. Please use Performance Schema instead
1 row in set (0.00 sec)
```

## 什么是 performance_schema?
来看英文翻译---
![在这里插入图片描述](https://img-blog.csdnimg.cn/eb0e6f42f5914fff8f3b42ecbbacf35f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)一改往日的性能查看模式（ show profiles ....)，**performance_schema是一个用于监控我们数据库的一个工具，类似于java的守护线程一样，在背后默默工作；**

> **默认情况下performance_schema 数据库时开启的状态，performance_schema在5.7.x及其以上版本中默认启用（5.6.x及其以下版本默认关闭，如果是5.6之前的版本，需要在配置文件中修改；），开启后几乎不会影响系统性能，因为他的优先级很低；**


performance_schema 作为MySQL中的基础数据库之一，

```sql

mysql> show databases ;
+--------------------+
| Database           |
+--------------------+
| gavin              |
| information_schema | -- 运行时主要关注关于元数据的一些信息，
| mysql              | --- 存放 相关基础数据---用户数据、权限等
| performance_schema |-- 运行时主要关注性能方面的信息---如 查询时间、io等相关资源的消耗 --
| sakila             |
| sys                |
| world              |
+--------------------+
7 rows in set (0.01 sec)
```
我们来看一下performance_schema表里面都有些啥-----

```sql

mysql> use performance_schema ;
Database changed
mysql> show tables;
+------------------------------------------------------+
| Tables_in_performance_schema                         |
+------------------------------------------------------+
| accounts ----账户表                                            |
| events_errors_summary_by_account_by_error   --事件错误         |
| 省略了很多行                                          |
| events_statements_current       --事务状态                     |
|     省略了很多行                                      |
| events_transactions_current                          |
|    省略了很多行                                       |
| events_waits_current          -- 等待                       |
|    省略了很多行                                       |
| file_instances                  --文件                     |
|  省略了很多行                                         |
+------------------------------------------------------+
110 rows in set (0.00 sec)
```
关于performance_schema数据库中的表看起来很多其实做一下分类也并不多
**1，可以从命名上看出来----有很多表名称 开头是一样的，**
如 ----- 关于等待事件的表
```sql

mysql> show tables like 'events_waits%';
+-----------------------------------------------+
| Tables_in_performance_schema (events_waits%)  |
+-----------------------------------------------+
| events_waits_current                          |
| events_waits_history                          |
| events_waits_history_long                     |
| events_waits_summary_by_account_by_event_name |
| events_waits_summary_by_host_by_event_name    |
| events_waits_summary_by_instance              |
| events_waits_summary_by_thread_by_event_name  |
| events_waits_summary_by_user_by_event_name    |
| events_waits_summary_global_by_event_name     |
+-----------------------------------------------+
9 rows in set (0.00 sec)
```

2，**看一下该数据库中的存储引擎----**
performance_schema既可以视为一张表，也可被视为存储引擎。如果该引擎可用，则应该在INFORMATION_SCHEMA.ENGINES表或SHOW ENGINES语句的输出中都可以看到它的SUPPORT值为YES，
```sql

mysql> show engines ;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
```

> 有很多数据引擎对-------管他呢， 如果要把这些表都整明白确实不容易，也没必要，我们常用的也就 innodb 和myisam,当然在表中你还会发现一个叫PERFORMANCE_SCHEMA 的引擎，这引擎跟 innodb 和myisam不一样，而跟memory一样不会持久化到磁盘里，而是存储到内存中的，退出数据库后相应信息也就消失了；


我们来理顺一下-----查询性能这些信息什么时候能查到？ 在数据库运行的时候查，查完就查完了，每次操作的语句可能不一样（我下次可能不这么查）所以没必要存放在磁盘中保留下来，对吧！

   那么**性能监控器的工作模式**--------就类似于Java中生产者消费者一样，你操作了数据库，那么数据库性能监控器就记录关于性能的数据放在内存中（以键值对的方式），你查的时候我给你展现出来，你不查我先放在那里，你走了（mysql服务器重启），我就把它销毁了；-----这就是TM流水账式的操作啊。。。。。

## 性能监控的完全体
默认情况下性能监控器是打开的状态，但是并不是每一项关于性能的细节都是开启状态，
我们随便选一张表来查看----啊不能随便选，要与设置开关相关的才行  即setup_开头的表

```sql
mysql> select * from setup_instruments where enabled ='no' or timed ='no';
+----------------------------------------------------------------------------+---------+-------+------------+------------+---------------------------------------------------------------------------------------------------------------------+
| NAME                                                                       | ENABLED | TIMED | PROPERTIES | VOLATILITY | DOCUMENTATION                                                                                                       |
+----------------------------------------------------------------------------+---------+-------+------------+------------+---------------------------------------------------------------------------------------------------------------------+
| wait/synch/mutex/pfs/LOCK_pfs_share_list                                   | NO      | NO    | singleton  |          1 | Components can provide their own performance_schema tables. This lock protects the list of such tables definitions. |
| wait/synch/mutex/sql/TC_LOG_MMAP::LOCK_tc                                  | NO      | NO    |            |          0 | NULL                                                                                                                |
| wait/synch/mutex/sql/MYSQL_BIN_LOG::LOCK_commit                            | NO      | NO    |            |          0 | NULL                                                                                                                |
| wait/synch/mutex/sql/MYSQL_BIN_LOG::LOCK_commit_queue                      | NO      | NO    |            |          0
省略。。。。。。。
445 rows in set (0.00 sec)
```
如果有需要的话 可以设置成 ON；

```sql

mysql> UPDATE setup_instruments SET ENABLED = 'YES', TIMED = 'YES' where name like 'wait%';
Query OK, 331 rows affected (0.01 sec)
Rows matched: 384  Changed: 331  Warnings: 0

mysql> UPDATE setup_consumers SET ENABLED = 'YES' where name like '%wait%';
Query OK, 3 rows affected (0.00 sec)
Rows matched: 3  Changed: 3  Warnings: 0
```

配置好后就可以详细查看性能信息了------

```sql
mysql> sELECT * FROM events_statements_current limit 1\G
*************************** 1. row ***************************
              THREAD_ID: 75     -- 线程ID
               EVENT_ID: 69       --事件ID
           END_EVENT_ID: 69       -- 结束事件-ID
             EVENT_NAME: statement/sql/show_create_table  -- 
                 SOURCE: init_net_server_extension.cc:96
            TIMER_START: 11513064061400000  ---开始时间
              TIMER_END: 11513064572900000  ---结束时间
             TIMER_WAIT: 511500000   ---等待时间
              LOCK_TIME: 245000000  -----锁占用的的时间
               SQL_TEXT: SHOW CREATE TABLE `sys`.`sys_config` -- sql语句
                 DIGEST:  --摘要   8f23b04fbcf94a77aadd8de74ef8d52927846e1eb61e11b754ac525e114ec81a
            DIGEST_TEXT: SHOW CREATE TABLE `sys` . `sys_config`
         CURRENT_SCHEMA: sys  -- 当前表
            OBJECT_TYPE: NULL
          OBJECT_SCHEMA: NULL
            OBJECT_NAME: NULL
  OBJECT_INSTANCE_BEGIN: NULL
            MYSQL_ERRNO: 0
      RETURNED_SQLSTATE: NULL
           MESSAGE_TEXT: NULL
                 ERRORS: 0
               WARNINGS: 0
          ROWS_AFFECTED: 0
              ROWS_SENT: 0
          ROWS_EXAMINED: 0
CREATED_TMP_DISK_TABLES: 0
     CREATED_TMP_TABLES: 0
       SELECT_FULL_JOIN: 0
 SELECT_FULL_RANGE_JOIN: 0
           SELECT_RANGE: 0
     SELECT_RANGE_CHECK: 0
            SELECT_SCAN: 0
      SORT_MERGE_PASSES: 0
             SORT_RANGE: 0
              SORT_ROWS: 0
              SORT_SCAN: 0
          NO_INDEX_USED: 0
     NO_GOOD_INDEX_USED: 0
       NESTING_EVENT_ID: 65
     NESTING_EVENT_TYPE: TRANSACTION   -- 当前事务类型
    NESTING_EVENT_LEVEL: 0
           STATEMENT_ID: 7822
1 row in set (0.00 sec)
```

# 查看性能信息常用的命令

## 哪类sql执行最多

**分析用户操作和需求**

```sql

mysql> select digest_text ,count_star ,first_seen ,last_seen from events_statements_summary_by_digest order by count_star desc \G
*************************** 1. row ***************************
digest_text: INSERT INTO `City` VALUES (...)
 count_star: 4079
 first_seen: 2021-09-03 18:18:06.959215
  last_seen: 2021-09-03 18:18:07.665518
*************************** 2. row ***************************
digest_text: INSERT INTO `CountryLanguage` VALUES (...)
 count_star: 984
 first_seen: 2021-09-03 18:18:07.800364
  last_seen: 2021-09-03 18:18:07.946546
*************************** 3. row ***************************
。。。。。。。。。。。。省略。。。。。。。。。。。。
```

## 查看sql平均响应时间

**查看查询语句的执行时间效率**

```sql
mysql> select  digest_text ,avg_timer_wait from events_statements_summary_by_digest order by count_star desc \G
*************************** 1. row ***************************
   digest_text: INSERT INTO `City` VALUES (...)
avg_timer_wait: 113300000
*************************** 2. row ***************************
   digest_text: INSERT INTO `CountryLanguage` VALUES (...)
avg_timer_wait: 97500000

省略。。。。。。。。。

```
## 哪类 sql 排序记录最多

**分析用户喜爱**

```sql
mysql> select  digest_text ,sum_sort_rows from events_statements_summary_by_digest order by count_star desc \G
----省略 。。。。。。。。。。。。。。。。。。。。。。。。。。
*************************** 513. row ***************************
  digest_text: SHOW CREATE TABLE `performance_schema` . `setup_consumers`
sum_sort_rows: 0
*************************** 514. row ***************************
  digest_text: SELECT SCHEMA ( )
sum_sort_rows: 0
*************************** 515. row ***************************
  digest_text: SELECT `empno` , `ename` , `job` , `sal` FROM `emp`
sum_sort_rows: 0
*************************** 516. row ***************************
  digest_text: SELECT `digest_text` , `count_star` , `first_seen` , `last_seen` FROM `events_statements_summary_by_digest` ORDER BY `count_star` DESC
sum_sort_rows: 0
*************************** 517. row ***************************
  digest_text: SELECT `digest_text` , `avg_timer_wait` FROM `events_statements_summary_by_digest` ORDER BY `count_star` DESC
sum_sort_rows: 516
517 rows in set (0.01 sec)
```

## 哪类sql扫描记录最多

```sql
 select  digest_text ,sum_rows_examined  from events_statements_summary_by_digest order by count_star desc \G
*************************** 516. row ***************************
      digest_text: SELECT `digest_text` , `count_star` , `first_seen` , `last_seen` FROM `events_statements_summary_by_digest` ORDER BY `count_star` DESC
sum_rows_examined: 0
*************************** 517. row ***************************
      digest_text: SELECT `digest_text` , `avg_timer_wait` FROM `events_statements_summary_by_digest` ORDER BY `count_star` DESC
sum_rows_examined: 1032
*************************** 518. row ***************************
      digest_text: SELECT `digest_text` , `sum_sort_rows` FROM `events_statements_summary_by_digest` ORDER BY `count_star` DESC
sum_rows_examined: 1034
518 rows in set (0.02 sec)
```
**哪类sql使用临时表最多**
```sql
mysql> select  digest_text ,sum_created_tmp_tables,sum_created_tmp_disk_tables  from events_statements_summary_by_digest order by count_star desc \G
sum_created_tmp_disk_tables: 0
*************************** 519. row ***************************
                digest_text: SELECT `digest_text` , `sum_rows_examined` FROM `events_statements_summary_by_digest` ORDER BY `count_star` DESC
     sum_created_tmp_tables: 0
sum_created_tmp_disk_tables: 0
*************************** 520. row ***************************
                digest_text: SELECT * FROM `performance_schema` . `events_statements_summary_by_digest` LIMIT ?, ...
     sum_created_tmp_tables: 0
sum_created_tmp_disk_tables: 0
*************************** 521. row ***************************
                digest_text: SHOW CREATE TABLE `performance_schema` . `events_statements_summary_by_digest`
     sum_created_tmp_tables: 0
sum_created_tmp_disk_tables: 0
521 rows in set (0.01 sec)
```

> summary表提供所有事件的汇总信息。该组中的表以不同的方式汇总事件数据（如：按用户，按主机，按线程等等）。

```sql
 select  digest_text ,sum_created_tmp_tables,sum_created_tmp_disk_tables  from events_statements_summary_by_digest order by count_star desc limit 1  \G
*************************** 1. row ***************************
                digest_text: INSERT INTO `City` VALUES (...)
     sum_created_tmp_tables: 0
sum_created_tmp_disk_tables: 0
1 row in set (0.10 sec)

mysql>
```

>_current表中每个线程只保留一条记录，且一旦线程完成工作，该表中不会再记录该线程的事件信息，
>

```sql
mysql> SELECT * FROM events_waits_current limit 1\G
*************************** 1. row ***************************
            THREAD_ID: 14
             EVENT_ID: 10612
         END_EVENT_ID: 10612
           EVENT_NAME: wait/synch/mutex/innodb/dblwr_mutex
               SOURCE: buf0dblwr.cc:396
          TIMER_START: 14100886997260266
            TIMER_END: 14100886997830206
           TIMER_WAIT: 569940
                SPINS: NULL
        OBJECT_SCHEMA: NULL
          OBJECT_NAME: NULL
           INDEX_NAME: NULL
          OBJECT_TYPE: NULL
OBJECT_INSTANCE_BEGIN: 2774696303920
     NESTING_EVENT_ID: NULL
   NESTING_EVENT_TYPE: NULL
            OPERATION: lock
      NUMBER_OF_BYTES: NULL
                FLAGS: NULL
1 row in set (0.00 sec)


```
   **EVENT_NAME: wait/synch/mutex/innodb/dblwr_mutex表示   
线程ID为14的线程正在等待innodb存储引擎的dblwr_mutex锁，这是innodb存储引擎的一个互斥锁，等待时间为569940皮秒**


> _history表中记录每个线程已经执行完成的事件信息，但每个线程的只事件**信息只记录10条，再多就会被覆盖掉**

```sql
SELECT THREAD_ID,EVENT_ID,EVENT_NAME,TIMER_WAIT FROM events_waits_history ORDER BY THREAD_ID limit 2;
+-----------+----------+----------------------------------------------------+------------+
| THREAD_ID | EVENT_ID | EVENT_NAME                                         | TIMER_WAIT |
+-----------+----------+----------------------------------------------------+------------+
|        14 |     9551 | wait/synch/mutex/innodb/log_limits_mutex           |      75026 |
|        14 |     9552 | wait/synch/mutex/innodb/buf_pool_flush_state_mutex |     291410 |
+-----------+----------+----------------------------------------------------+------------+
2 rows in set (0.02 sec)
```

等等主要是搞懂几表存放的信息，首先是表名，然后是表的内容；

小结------
 1，开启性能监控细节的表--------setup_****有关的表 
 2，性能监控一般不会影响系统性能，
3，提高性能主要是从以下两个方面着手-----
减少执行次数----频率，
减少操作数量-----数量



真对性能优化的指标,市面上已经有很多软件可以查看数据库的性能数据-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/e01d39dc0d9d489f827af883232f1bd4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/e351e8ddd09c4444a55e0a58cda08d51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)所以只要了解大概就可以了,我是说非运维工程师;



# 优化查询效率--直方图
抛砖引玉

来我们先想一下,怎么才能优化查询效率,先从数据库是如何工作的来入手-----

 程序员能做的就是在分析器和优化器上面动手;
![](..\pic\mysql_pic\19.png)

换句话来讲,就是用怎样让用户能够快速找到想要的数据------
对于数据表,我们可以设立适当的索引来完成对数据表的内容快速查询,但是对于那些没有用索引的表我们怎样来加快查询速度呢?
对于那些没有建立索引的表,程序执行器不知道-------
1,该表有多少条记录?
2,每一列的值的分布情况,因为没有索引,所以会采取遍历整张表的方式来查找数据,效率自然就低了;

假如有这样一张表 person_info,里面存放的数据
name---姓名
birthday----生日
age----年龄

我想要查找
1-----10到18岁 的人
2------20到40岁的人
语句如下------
select  name 姓名 ,age 年龄 from  person_info where  age between 10 and 18 ;
select  name 姓名 ,age 年龄 from  person_info where  age between 20 and 40 ;

> 由于没有索引,优化器会按照一般方式进行查询----遍历 如果数据表中 年龄在10-18的人远远多于20-40 的人,那么第二个查询的时间会多于第一个,因为优先查到10-18的人的概率比20-40 的大;

有杠精就来找事了,如果是20-40 的人分布在表头呢???------你可以走了,慢走不送!

要么加快数据处理速度即整一个强大的cpu-------硬刚,
要么对数据的分布有一个大致了解------数学图形学之统计

> 还记得数学中的正态分布吗?面对海量数据的情况,我们在找到中间位置的数据概率比两边的数据的概率要大的多,所以如果数据表中没有索引,那么适合给该表整一个直方图,以告诉优化器在各个年龄段的分布情况;

**统计直方图**

MySQL中统计直方图在8.0之后新增的功能,主要有两种直方图---- 等高直方图和等宽直方图-------啥,这都不知?你可以去百度呀...........

简单来说这两种图可以让 优化器知道数据的分布情况,以便有的放矢的去查找数据;

先来数据-----

```sql
mysql> select * from emp;
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
14 rows in set (0.02 sec)
```
**问题一-------查看薪资sal的分布情况**

在sal字段上建立直方图

```sql

mysql> analyze table emp update histogram on sal with 50 buckets ;
+-----------+-----------+----------+------------------------------------------------+
| Table     | Op        | Msg_type | Msg_text                                       |
+-----------+-----------+----------+------------------------------------------------+
| gavin.emp | histogram | status   | Histogram statistics created for column 'SAL'. |
+-----------+-----------+----------+------------------------------------------------+
1 row in set (0.03 sec)

mysql> analyze table emp update histogram on sal ,comm ;
+-----------+-----------+----------+-------------------------------------------------+
| Table     | Op        | Msg_type | Msg_text                                        |
+-----------+-----------+----------+-------------------------------------------------+
| gavin.emp | histogram | status   | Histogram statistics created for column 'COMM'. |
| gavin.emp | histogram | status   | Histogram statistics created for column 'SAL'.  |
+-----------+-----------+----------+-------------------------------------------------+
2 rows in set (0.03 sec)


```

> **再次创建直方图时，将会将上一个直方图重写。
> 如果不指定buckets则默认为100;**





**统计直方图的创建方式-----**

```sql
analyze table  表名  update histogram   on 字段名     with  数字   buckets;
-- 数字的取值范围1-1024,默认值是100。设置buckets值时，可以尝试设置低一些，如没有满足要求，可以再往上增大。因为我们也不知道数据的具体分布情况;--
```

```sql
mysql> analyze table emp update histogram on sal ,comm ;
+-----------+-----------+----------+-------------------------------------------------+
| Table     | Op        | Msg_type | Msg_text                                        |
+-----------+-----------+----------+-------------------------------------------------+
| gavin.emp | histogram | status   | Histogram statistics created for column 'COMM'. |
| gavin.emp | histogram | status   | Histogram statistics created for column 'SAL'.  |
+-----------+-----------+----------+-------------------------------------------------+
2 rows in set (0.03 sec)
```

对于不同的数据集合，buckets的值取决于以下几个因素：
●　这列有多少不同的值,如果相同的值很多也没有必要设置直方图;
●　数据的分布情况,只能自己尝试,就像直方图中设置间距一样
●　需要多高的准确性,越小准确性与高,但是效率上会有所牺牲



**创建什么样的直方图呢??**

> 直方图的共同点是，它们都将数据分到了一系列的buckets中去。MySQL会自动将数据划到不同的buckets中，也会**自动决定**创建哪种类型的直方图。

**直方图的删除方式----**

analyze table emp drop histogram on 字段名;

```sql
mysql> analyze table emp drop histogram on  sal,comm ;
+-----------+-----------+----------+-------------------------------------------------+
| Table     | Op        | Msg_type | Msg_text                                        |
+-----------+-----------+----------+-------------------------------------------------+
| gavin.emp | histogram | status   | Histogram statistics removed for column 'comm'. |
| gavin.emp | histogram | status   | Histogram statistics removed for column 'sal'.  |
+-----------+-----------+----------+-------------------------------------------------+
2 rows in set (0.03 sec)

```

直方图的优点-----
1,为优化器提供数据支持-----数据分布情况
2,直方图不像索引那样增删查改都需要更新索引,除非你自己去修改,不然就当作是固定数据存放在那里,所以对mysqlCURD没有性能影响.



> 建立直方图的时候，MySQL服务器会将所有数据读到内存中，然后在内存中进行操作，包括排序。如果对一个很大的表建立直方图，可能会需要将几百兆的数据都读到内存中,考虑到系统的计算机的性能开销,为了规避这种风险，MySQL会根据给定的histogram_generation_max_mem_size的值计算该将多少行数据读到内存中。

```sql

mysql> show variables like 'histogram_generation_max_mem_size%';
+-----------------------------------+----------+
| Variable_name                     | Value    |
+-----------------------------------+----------+
| histogram_generation_max_mem_size | 20000000 |
+-----------------------------------+----------+
1 row in set, 1 warning (0.00 sec)
```

当然可以根据需要自己去设定该值的大小----
set histogram_generation_max_mem_size=10000;


建好直方图之后再去查询数据,效率可能会有提高,如果没有提高,那么就需要尝试修改 bufkets 的值来不断优化;



**mysql优化查询效率面面观**



话不多说,直接开搞------

从表中数据的查询开始---
看一下表的创建语句--------------------->>>>>

```java

mysql> show create table emp2 \G
*************************** 1. row ***************************
       Table: emp2
Create Table: CREATE TABLE `emp` (
  `EMPNO` int NOT NULL,
  `ENAME` varchar(10) DEFAULT NULL,
  `JOB` varchar(9) DEFAULT NULL,
  `MGR` int DEFAULT NULL,
  `HIREDATE` date DEFAULT NULL,
  `SAL` double(7,2) DEFAULT NULL,
  `COMM` double(7,2) DEFAULT NULL,
  `DEPTNO` int DEFAULT NULL,
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.00 sec)
```
在下面的内容开始之前,先说一下sql中的数据访问类型效率排序-------->>>>>>>>
system>const>eq_ref>ref>fulltext>ref_or_null>index_merge>unique_subquery>index_subquery>range>index>all;

别着急,先一个一个分析

从效率低的开始------
all------->>>>

```sql

mysql> explain select * from emp2\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE    -- 查询类型
        table: emp2       -- 表名     
   partitions: NULL
         type: ALL       --访问类型 
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14        -- 扫描行数
     filtered: 100.00    --过滤,100.00表示未过滤
        Extra: NULL
1 row in set, 1 warning (0.00 sec)

```

> **使用all这种数据访问方式,表示扫描全表数据,当表中数据很多时一般不建议扫描全表,实际应用中大都也不用这种方式;**

对比以下方式----index

```sql

mysql> alter table emp2 add index pt_index (empno);
Query OK, 0 rows affected (0.06 sec)
Records: 0  Duplicates: 0  Warnings: 0


mysql> explain select  empno from emp2  \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: index
possible_keys: NULL
          key: pt_index
      key_len: 4
          ref: NULL
         rows: 14
     filtered: 100.00
        Extra: Using index
1 row in set, 1 warning (0.00 sec)

-- 未设置主键S索引之前
mysql>  explain select  * from emp2 order by empno desc  \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 100.00
        Extra: Using filesort
1 row in set, 1 warning (0.00 sec)

--- 普通索引,并不带排序的功能,添加主键索引之后
mysql> alter table emp2 modify empno int primary key ;
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0


mysql> explain select  * from emp2 order by empno desc  \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: index
possible_keys: NULL
          key: PRIMARY
      key_len: 4
          ref: NULL
         rows: 14
     filtered: 100.00
        Extra: Backward index scan
1 row in set, 1 warning (0.00 sec)
```

> **index这种数据扫描方式比all要好一些,因为用到了索引,
> 一般有两种情况----->>>需要的数据在索引中就可以找到,另一个是通过索引进行排序,要求为主键索引或者唯一索引,只有普通索引的话也是all;**

以上两种方式筛选的数据还是很多,在此基础上做一些条件筛选一减少数据的展现量,确保数据的命中率;

range------->>>>>

```sql
mysql> explain select * from emp2 where empno>7800 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: range
possible_keys: PRIMARY,pt_index
          key: PRIMARY
      key_len: 4
          ref: NULL
         rows: 6
     filtered: 100.00
        Extra: Using where
1 row in set, 1 warning (0.00 sec)


mysql>  explain select * from emp2 where sal>7800 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 33.33
        Extra: Using where
1 row in set, 1 warning (0.00 sec)
```

> **筛选条件-----如果是主键索引或者其他索引所在字段,则数据访问类型为range否则可能仍然为all,
> 筛选条件 的操作主要关键字有 >    ,< ,between and ,is null,like ,in(),any(),some()  等等**


**所以可以小结以下,查询数据时尽量通过主键或者索引所在字段作为where的条件部分,依赖可以缩小数据范围,另一个可以提高查询效率;**

index_subquery------>>>>

> **这种数据访问方式现在很少出现,因为随着mysql优化器的优化(性能改进),这种方式会被mysql优化器优化为eq_ref类型;**

unique_subquery------>>>

> **这种数据访问模式跟index_subquery类似,只是索引对应的时唯一索引;**

index_merge---------->>>>>>

```sql
-- 添加索引
mysql> alter table emp2 drop  index lh_index ;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table emp2 add  unique index un_index (empno) ;
Query OK, 0 rows affected (0.06 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table emp2 add   index pt_index (ename) ;
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> explain select * from emp2 where empno=7369 or ename='king'\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: index_merge
possible_keys: PRIMARY,un_index,pt_index
          key: PRIMARY,pt_index
      key_len: 4,43
          ref: NULL
         rows: 2
     filtered: 100.00
        Extra: Using union(PRIMARY,pt_index); Using where
1 row in set, 1 warning (0.00 sec)

```
Using union(PRIMARY,pt_index); Using where----表示出现了索引合并优化，通常是将多个索引字段的范围扫描合并为一个。包括单表中多个索引的交集，并集以及交集之间的并集，但不包括跨多张表和全文索引。

 index merge这种数据访问方式可以使得我们可以使用多个索引同时进行扫描，然后将结果进行合并,但是这就会产生以下几种结果集-----
 1,取交集------即 索引1字段的条件 and   索引2字段的条件 ,如果出现这种情况也就说明我们的索引建立的并不合理,可以通过建立符合索引来进行优化-----


```sql
mysql> alter table emp2 drop index un_index ;
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table emp2 drop index pt_index ;
Query OK, 0 rows affected (0.08 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table emp2 add  unique index un_index  (empno,ename);
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0



mysql> explain  select * from emp2 where empno=7839 and ename='king' \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: const
possible_keys: PRIMARY,un_index
          key: PRIMARY
      key_len: 4
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)
```

**> 对比两种索引的扫描方式,第一种两个单独的索引,需要扫描两次(rows:2)然后取交集得到的结果第二种方式是扫描一次就可以得到结果,所以对于多索引字段如果费有必要的话(一般索引是不需要太多)可以设置联合索引代替 多索引的;**

ref_or_null---------->>>>>

对于某个字段即需要关联条件也需要查null值的情况会选用这种方式;这种方式跟ref类似,如果表中数据很少的话会进行全表扫描-----

```sql

mysql> explain select * from emp2  where empno=7844 or ename is null \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: ALL
possible_keys: PRIMARY,no_index
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 16.43
        Extra: Using where
1 row in set, 1 warning (0.00 sec)
```
ref------->>>>
使用非唯一性索引进行数据的查找

```sql
mysql> explain select * from emp2 e ,dept2 d where e.deptno=d.deptno \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: d
   partitions: NULL
         type: ALL
possible_keys: ptt_index
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 4
     filtered: 100.00
        Extra: NULL
*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: e
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 10.00
        Extra: Using where; Using join buffer (hash join) --- 使用连接缓存
2 rows in set, 1 warning (0.00 sec)
```
这种方式如果数据量少也会进行全表扫描

eq_ref----------------->>>>>
这种方式表示使用了唯一索引进行数据查找
```sql
mysql> explain select * from emp2  e ,emp e1 where e.empno=e1.empno \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: e
   partitions: NULL
         type: ALL
possible_keys: PRIMARY,empno
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 100.00
        Extra: NULL
*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: e1
   partitions: NULL
         type: eq_ref
possible_keys: PRIMARY,no_index
          key: PRIMARY
      key_len: 4
          ref: gavin.e.empno
         rows: 1
     filtered: 100.00
        Extra: NULL
2 rows in set, 1 warning (0.00 sec)
```
 const----->>>表示这个表之多有一个匹配记录


```sql
mysql> explain select * from emp2  where empno=7839\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: const
possible_keys: PRIMARY,empno --  表中存在的索引
          key: PRIMARY ------实际用到的索引
      key_len: 4
          ref: const -- 至多有一条记录则显示const,若非唯一,则显示哪一列用到了索引
         rows: 1    -- 扫描的行数
     filtered: 100.00
        Extra: NULL --额外的信息
1 row in set, 1 warning (0.02 sec)
```
extra处的值可以有以下取值--------------
using filesort ----->>>>
说明MySQL无法使用索引进行排序,只能利用排序算法进行排序,会消耗一部分额外的资源;

```sql
mysql> explain select * from emp2 order by sal \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 100.00
        Extra: Using filesort
1 row in set, 1 warning (0.00 sec)
```
using temporary---------------->>>>使用临时表
查询完毕后删除临时表
```sql

mysql> explain select ename ,count(*) from emp2  where deptno = 10 group by ename \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 10.00
        Extra: Using where; Using temporary  --使用where 进行扫描,同时建立临时表
1 row in set, 1 warning (0.00 sec)
```
using index------------------>>>
表名数据再索引字段就可以取到
```sql
ysql> explain select empno from emp2  \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: emp2
   partitions: NULL
         type: index
possible_keys: NULL
          key: empno
      key_len: 4
          ref: NULL
         rows: 14
     filtered: 100.00
        Extra: Using index
1 row in set, 1 warning (0.00 sec)

```
using  join buffer ------>使用连接缓存
```sql
mysql> explain select * from emp2 e ,dept2 d where e.deptno=d.deptno \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: d
   partitions: NULL
         type: ALL
possible_keys: ptt_index
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 4
     filtered: 100.00
        Extra: NULL
*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: e
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 14
     filtered: 10.00
        Extra: Using where; Using join buffer (hash join) --- 使用连接缓存
2 rows in set, 1 warning (0.00 sec)
```
 no matching row in const table------
 表示没有匹配到数据


```sql

mysql> explain select * from emp2  where empno =9999 \G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: NULL
   partitions: NULL
         type: NULL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: NULL
     filtered: NULL
        Extra: no matching row in const table
1 row in set, 1 warning (0.00 sec)
```



关于mysql优化查询效率主要有以下几个方面


1,查询时尽量避免使用全字段查询
2,筛选条件----where 处尽量选择带有索引的字段(如果能查到的话)
3,设置索引时 唯一索引,主键索引的查询效率要好于普通索引;
4,多字段索引一般是用联合索引替代



# 范式的定义
  范式作为一个数据库级别的术语,它是指关系数据库中的关系要满足的一定要求,------即不同的范式;

**范式的分类----**

   范式有六种----
   第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、Boyce-Codd范式（BCNF）、第四范式（4NF）和第五范式（5NF）。

**啥是范式**

说了这么半天,范式反映在数据表层面,举个简单的例子,
某超市管理系统中有这样一张表,
production_name---商品名
price ----零售价
provide-----供应商
![在这里插入图片描述](https://img-blog.csdnimg.cn/784df65668fe4334a8ee2e880659c2bb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果该张表用于存储商品价格,那么此时想要删除某一个供应商,那么就要删除对应的商品价格信息,可能该超市改供应商的产品卖光了只有别的供应商的同类产品,那么怎么办呢?

于是这里就涉及到了范化,在创建一个数据库的过程中,范化是将某表转化为一些表的过程,这使得在查询数据库中的数据结果更加准确,同时增加和删除表中的数据对别的表以及字段影响降到最小;
范化的目的并不是简单的将表拆分而是将拆分的表通过键来关联起来,每张表的列都是不可再分的最小数据单元;
于是我们可以将上表拆分成-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/e9d3fc3b544441c8a420c43be3eb22ce.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)两张表通过商品字段进行关联;
这时候删除供应商并不会删除商品的价格信息.

### 第一范式
所谓第一范式就是指数据库表的每一列都不能在分割了,
例如 姓名字段 ,在有些数据库中会将该字段拆分成姓 和 名,
就像上表一样,在查询数据的时候不需要额外的拆分处理;

> **总结一句话就是,第一范式要求字段的数据的原子性;**

举个例子体会一下吧------以下信息由excel制作

![在这里插入图片描述](https://img-blog.csdnimg.cn/e1335f1efb6b456997e5eeeaedcc5185.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)### 第二范式
第二范式是在第一范式的基础上进行的,通过主键依赖来进行判断表中应该存在的字段;
比如---
在一家培训机构中系统中有这样一张表-------
主键----教师姓名与班级作为主键,一个老师最多能交一个班级,

![在这里插入图片描述](https://img-blog.csdnimg.cn/222aeb8acc3c4a6ab7230984c55d050e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
通过主键能分析出的信息----
主键----姓名与班级,
性别跟教师有关-----即性别依赖于教师,
编号跟教师有关----即编号依赖于教师,
授课课时依赖于授课开始时间与结束时间,
课时费依赖于授课课时和教师等级,教师等级依赖于教师即课时费间接依赖于教师,
于是就分析出该表有数据不依赖于主键和间接依赖于主键的信息,所以按照第二范式的要求该表是不符合的,
所以可以做如下修改:
1,删除主键,
2,拆分表格使其符合第二范式要求;
![在这里插入图片描述](https://img-blog.csdnimg.cn/674abefc5b21449281407cf942d669b4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)要求表中的字段属性完全依赖于主关键字,如果出现部分依赖或者不依赖的字段,则需要将他们拆分到另一张表中


> **总结-----非主属性要要完全依赖于主键;**

举个很容易理解的案例----
![在这里插入图片描述](https://img-blog.csdnimg.cn/8e35c78a0ddf4cb781ad50b138858e21.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)





### 第三范式
减少数据冗余,上表中没有必要在第二张表中在添加教师姓名,性别等字段,因为通过关联就可以找到这两种属性;

一般在设计表的时候考虑到范式的前三各阶段;



### 反范式
啥是反范式?
听名字就知道跟范式反着来,你不让我干这事,我偏干;
举个例子就是上表中我要查询教师的课时费,需要将两张表连结起来查,反范式就是我只需要查询右半部分的表,但是右半部分信息不全,只好补全数据,这样只通过查询一张表来获得想要的查询数据;

反范式一定程度上提高了数据的冗余,但是提高了查询效率------多表查询效率低;

<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/mysql/MySQLBase04.md#数据库操作)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/mysql/MySQLBase06.md#JAVA实现JDBC)

</center>



![](..\pic\div\plant.gif)