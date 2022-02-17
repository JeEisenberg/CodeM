![](..\pic\div\plant.gif)

# 数据的基本操作

# 日期函数

## 当前日期函数

**当前日期的函数**
curdate()与current_date() 作用一样
curdate()+0是将时间转化为数字;

```sql
mysql> select curdate() as date1,current_date() as date2,curdate()+0 as date3;
+------------+------------+----------+
| date1      | date2      | date3    |
+------------+------------+----------+
| 2021-08-30 | 2021-08-30 | 20210830 |
+------------+------------+----------+
1 row in set (0.00 sec)
```

## 当前时间函数

**当前时间的函数**

```sql
mysql> select curtime() as time1,current_time(),curtime()+0 as time2;
+----------+----------------+--------+
| time1    | current_time() | time2  |
+----------+----------------+--------+
| 15:17:14 | 15:17:14       | 151714 |
+----------+----------------+--------+
1 row in set (0.00 sec)

```

## 当前日期和时间函数

```sql
mysql> select current_timestamp() as time1,localtime(),now(),sysdate() as time2;
+---------------------+---------------------+---------------------+---------------------+
| time1               | localtime()         | now()               | time2               |
+---------------------+---------------------+---------------------+---------------------+
| 2021-08-30 15:22:59 | 2021-08-30 15:22:59 | 2021-08-30 15:22:59 | 2021-08-30 15:22:59 |
+---------------------+---------------------+---------------------+---------------------+
```

## 时间戳函数

既然是时间戳至少需要包含日期,---以返回‘1970-01-0100:00:00’GMT到现在的秒数

```sql
mysql> select unix_timestamp(),unix_timestamp(now()),unix_timestamp(current_time);
+------------------+-----------------------+------------------------------+
| unix_timestamp() | unix_timestamp(now()) | unix_timestamp(current_time) |
+------------------+-----------------------+------------------------------+
|       1630308333 |            1630308333 |                   1630308333 |
+------------------+-----------------------+------------------------------+
1 row in set (0.00 sec)
```

> UNIX_TIMESTAMP(date)若无参数调用，则返回一个UNIX时间戳（‘1970-01-01
> 00:00:00’GMT之后的秒数）作为无符号整数。其中，GMT（Green wich mean
> time）为格林尼治标准时间。若用date来调用UNIX_TIMESTAMP()，它会将参数值以‘1970-01-01 00:00:00’GMT后的秒数的形式返回。date可以是一个DATE字符串、DATETIME字符串、TIMESTAMP或一个当地时间的YYMMDD或YYYYMMDD格式的数字。

## 时间戳函数的反函数

既然时间可以转换为时间戳,也可以反过来---from_unixtime(数字);

```sql
mysql> select from_unixtime(1630308333);
+---------------------------+
| from_unixtime(1630308333) |
+---------------------------+
| 2021-08-30 15:25:33       |
+---------------------------+
1 row in set (0.00 sec)

```

## 标准时间和日期函数

UTC_TIME()----当前时区的时间
 UTC_DATE()--------------返回当前时区日期的时间

```sql
mysql> select UTC_TIME(),UTC_DATE(),UTC_TIME()+0,UTC_DATE()+0;
+------------+------------+--------------+--------------+
| UTC_TIME() | UTC_DATE() | UTC_TIME()+0 | UTC_DATE()+0 |
+------------+------------+--------------+--------------+
| 07:33:02   | 2021-08-30 |        73302 |     20210830 |
+------------+------------+--------------+--------------+
1 row in set (0.00 sec)
```

## 月份函数和获取月份名称函数

```sql
mysql> select month(1000),monthname(1000);
+-------------+-----------------+
| month(1000) | monthname(1000) |
+-------------+-----------------+
|          10 | October         |
+-------------+-----------------+
1 row in set (0.00 sec)

mysql> select month(now()),monthname(now());
+--------------+------------------+
| month(now()) | monthname(now()) |
+--------------+------------------+
|            8 | August           |
+--------------+------------------+
1 row in set (0.00 sec)


mysql> select month(20190304),monthname('2019,05,01');
+-----------------+-------------------------+
| month(20190304) | monthname('2019,05,01') |
+-----------------+-------------------------+
|               3 | May                     |
+-----------------+-------------------------+
1 row in set (0.00 sec)

```

## 获取星期几的函数

DAYNAME(d)----返回当前日期为周几的名称
DAYOFWEEK(d)-----返回当前为一周的第几天,(周日算第一天)
WEEKDAY(d)------返回当前为一周第几天,周一为第一天;

```sql
mysql> select dayname(20210809)as t1,dayofweek(20210809),weekday(20211019);
+--------+---------------------+-------------------+
| t1     | dayofweek(20210809) | weekday(20211019) |
+--------+---------------------+-------------------+
| Monday |                   2 |                 1 |
+--------+---------------------+-------------------+
1 row in set (0.00 sec)
```

## 获取星期数的函数

week(20210809),表示返回当前日期为一年中的第几周,默认星期日为一周第一天;
week(20210809,1),表示返回当前日期为一年中的第几周,指定周一为一周开始;

 weekofyear(20210809),返回当前日期为一年中的第几周;

```sql
mysql> select week(20210809),week(20210809,1),weekofyear(20210809),weekofyear(20210809);
+----------------+------------------+----------------------+----------------------+
| week(20210809) | week(20210809,1) | weekofyear(20210809) | weekofyear(20210809) |
+----------------+------------------+----------------------+----------------------+
|             32 |               32 |                   32 |                   32 |
+----------------+------------------+----------------------+----------------------+
1 row in set (0.00 sec)

```

## 获取天数的函数

天数主要是是为了获得一年中的第几天,一个月中的第几天,一周的第几天

```sql
mysql> select dayofyear(20210909),dayofmonth(20210909),dayofweek(20100909);
+---------------------+----------------------+---------------------+
| dayofyear(20210909) | dayofmonth(20210909) | dayofweek(20100909) |
+---------------------+----------------------+---------------------+
|                 252 |                    9 |                   5 |
+---------------------+----------------------+---------------------+
```

## 获取年、季度、小时、分钟和秒钟的函数

之前提到了获取月份,日期,还可以单独获取其他时间的函数

```sql
mysql> select year(20220909),quarter(20210909),hour(now()),minute(20210909122334),second(now());
+----------------+-------------------+-------------+------------------------+---------------+
| year(20220909) | quarter(20210909) | hour(now()) | minute(20210909122334) | second(now()) |
+----------------+-------------------+-------------+------------------------+---------------+
|           2022 |                 3 |          15 |                     23 |             7 |
+----------------+-------------------+-------------+------------------------+---------------+
1 row in set (0.00 sec)
```

也非常的见名知意;

## 从时间中获取需要的信息

```sql
mysql> SELECT EXTRACT(YEAR FROM '2018-07-02' ) AS coll,
    -> EXTRACT(YEAR_MONTH FROM '2018-07-12 01:02:03') AS col2,
    ->  EXTRACT(DAY_MINUTE FROM '2018-07-12 01:02:03') AS col3;
+------+--------+--------+
| coll | col2   | col3   |
+------+--------+--------+
| 2018 | 201807 | 120102 |
+------+--------+--------+
1 row in set (0.00 sec)

mysql> SELECT EXTRACT(YEAR FROM now() ) AS coll,
    -> EXTRACT(YEAR_MONTH FROM now()) AS col2,
    ->  EXTRACT(DAY_MINUTE FROM now()) AS col3;
+------+--------+------+
| coll | col2   | col3 |
+------+--------+------+
| 2021 | 202108 | 1642 |
+------+--------+------+
1 row in set (0.00 sec)
```

## 时间和秒钟转换

TIME_TO_SEC(time)返回已转化为秒的time参数,
转换公式为：小时*3600+分钟*60+秒。

```sql
mysql> select time_to_sec(now());
+--------------------+
| time_to_sec(now()) |
+--------------------+
|              61403 |
+--------------------+
1 row in set (0.00 sec)

mysql> select sec_to_time(61403);
+--------------------+
| sec_to_time(61403) |
+--------------------+
| 17:03:23           |
+--------------------+
1 row in set (0.00 sec)


```

## 计算日期和时间的函数

> 计算日期和时间的函数有DATE_ADD()、ADDDATE()、DATE_SUB()、SUBDATE()、ADDTIME()、SUBTIME()和DATE_DIFF()。

![在这里插入图片描述](https://img-blog.csdnimg.cn/e6a8cc48665f4052bc3d7424ee1d8429.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**date_add(时间,interval   要增加的值    增加的时间类型)
adddate(时间,interval   要增加的值    增加的时间类型)**

```sql
mysql> select date_add(now(),interval 1 year)as  time1,adddate(current_date(),interval 2 month) as time2;
+---------------------+------------+
| time1               | time2      |
+---------------------+------------+
| 2022-08-30 17:10:11 | 2021-10-30 |
+---------------------+------------+
1 row in set (0.00 sec)
```

有加就有减------
**date_sub(时间,interval   要减少的值    减少的时间类型)
subdate(时间,interval   要减少的值    增加的时间类型)**

```sql
mysql> select date_sub(curdate() ,interval 1 month)as col1,subdate(now() ,interval 1 month) as col2;
+------------+---------------------+
| col1       | col2                |
+------------+---------------------+
| 2021-07-30 | 2021-07-30 17:14:55 |
+------------+---------------------+
1 row in set (0.00 sec)

mysql> select date_sub(curdate() ,interval 1 month)as col1,subdate(now() ,interval 1 hour) as col2;
+------------+---------------------+
| col1       | col2                |
+------------+---------------------+
| 2021-07-30 | 2021-08-30 16:15:08 |
+------------+---------------------+
1 row in set (0.00 sec)
```

**加一个负值等于减,减一个负值等于加**----基本操作

date类型的操作可以操作到年\月\日\时间

**时间加减操作-----**

```sql
mysql> select time_add(now(),'12:12:12') as col,addtime(now(),'00:00:00')as col2;
ERROR 1305 (42000): FUNCTION test.time_add does not exist
mysql> select addtime(now(),'00:00:00')as col2;
+---------------------+
| col2                |
+---------------------+
| 2021-08-30 17:22:14 |
+---------------------+
1 row in set (0.00 sec)

mysql> select sub_time(now(),'00:38:00') as co1;
ERROR 1305 (42000): FUNCTION test.sub_time does not exist
mysql> select subtime(now(),'00:38:00') as co1;
+---------------------+
| co1                 |
+---------------------+
| 2021-08-30 16:45:11 |
+---------------------+
1 row in set (0.00 sec)
```

本来以为按照date的逻辑能有time_add,竟然没有.......好尴尬!!!

**返回两天之间的间隔天数**

datediff(date1,date2)返回 date1-date2的间隔天数;

```sql
mysql> select datediff('2019-09-09 12:12:12',now())as cltw;
+------+
| cltw |
+------+
| -721 |
+------+
1 row in set (0.00 sec)
```

**日期和时间格式化**
DATE_FORMAT(date,format)根据format指定的格式显示date值。
![在这里插入图片描述](https://img-blog.csdnimg.cn/da9ac9ddd45242e6a999ecdb87653282.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

这个玩意跟java日期转换方法类似-----,选定日期,指定格式

```sql
mysql>  SELECT DATE_FORMAT(now(), '%W %M %Y') AS coll,
    -> date_format(now() ,'%D %y %a %d %m %b %j') as coll2;
+--------------------+---------------------------+
| coll               | coll2                     |
+--------------------+---------------------------+
| Monday August 2021 | 30th 21 Mon 30 08 Aug 242 |
+--------------------+---------------------------+
1 row in set (0.00 sec)


mysql> SELECT DATE_FORMAT(now(), '%H:%i:%s') AS col3,
    -> DATE_FORMAT(now(), '%X %V') AS col4;
+----------+---------+
| col3     | col4    |
+----------+---------+
| 17:37:13 | 2021 35 |
+----------+---------+
1 row in set (0.00 sec)

mysql>

```

**除了日期转换还有时间转换函数**

```sql
mysql> select time_format('17:23:45','%H %K %H %I %L');
+------------------------------------------+
| time_format('17:23:45','%H %K %H %I %L') |
+------------------------------------------+
| 17 K 17 05 L                             |
+------------------------------------------+
1 row in set (0.00 sec)

mysql> select time_format('17:23:45','%H %k %h %I %l');
+------------------------------------------+
| time_format('17:23:45','%H %k %h %I %l') |
+------------------------------------------+
| 17 17 05 05 5                            |
+------------------------------------------+
1 row in set (0.00 sec)
```

**获取时间对应的显示格式----**

> GET_FORMAT(val_type,
> format_type)返回日期时间字符串的显示格式，val_type表示日期数据类型，包括DATE、DATETIME和TIME；format_type表示格式化显示类型，包括EUR、INTERVAL、ISO、JIS、USA。
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/3bb05d1d954f4d0c8e26fdb584e4cb62.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

本笔记的目的是为了方便程序员们查看基础知识,同时也是为了回顾一下之前学过的内容----深入;



**dayname()，dayofweek(),weekday()**

dayofweek()和weekday()返回的都是当前是第几个工作日，只是前者返回的是从1开始的，即一周的开始是周日，后者返回的是从0开始的，即一周的开始是周一。

dayname()，返回的是当前日期是第几个而工作日的英文名，

weekday更适合东方国家，dayofweek适合于西方国家；

同理我们也可以得到monthname()的用法；

```sql
mysql> select monthname(no
+------------------+
| monthname(now()) |
+------------------+
| June             |
+------------------+
1 row in set (0.00 sec)
```



# mysql中三目运算符

**java中三目运算符------ 表达式 ? a  : b** 还有印象吗????

话不多说,先操练起来----
**在mysql中三目运算符的形式----**

## if(表达式,a,b)

a,b不做类型限制

```sql
mysql> select if(1=1,'yes','no');
+--------------------+
| if(1=1,'yes','no') |
+--------------------+
| yes                |
+--------------------+
1 row in set (0.00 sec)

mysql> select if(0,'yes','no');
+------------------+
| if(0,'yes','no') |
+------------------+
| no               |
+------------------+
1 row in set (0.00 sec)

mysql> select if(null,'yes','no');
+---------------------+
| if(null,'yes','no') |
+---------------------+
| no                  |
+---------------------+
1 row in set (0.00 sec)
```

## ifnull(a,b)

如果a不为null,则返回a,否则返回b

```sql
mysql> select ifnull(1,2);
+-------------+
| ifnull(1,2) |
+-------------+
|           1 |
+-------------+
1 row in set (0.00 sec)

mysql> select ifnull(null,2);
+----------------+
| ifnull(null,2) |
+----------------+
|              2 |
+----------------+
1 row in set (0.00 sec)

mysql> select ifnull(null,null);
+-------------------+
| ifnull(null,null) |
+-------------------+
|              NULL |
+-------------------+
1 row in set (0.00 sec)
```

## nullif(a,b)

如果a=b则返回null,否则返回a;

```sql
mysql> select nullif(1,1),nullif(1,2),nullif('对','对'),nullif('对','错');
+-------------+-------------+-------------------+-------------------+
| nullif(1,1) | nullif(1,2) | nullif('对','对') | nullif('对','错') |
+-------------+-------------+-------------------+-------------------+
|        NULL |           1 | NULL              | 对                |
+-------------+-------------+-------------------+-------------------+
1 row in set (0.00 sec)
```

## CASE函数-----

case函数相当于java中的switch条件分支

CASE函数格式如下-----
CASE  某个条件    when 条件1  then  值1     when 条件2  then 值2 ..... else  '值n'  end ;

```sql
mysql> select case 3 when 1 THEN  'MONDAY' WHEN 2 THEN 'TUESDAY' WHEN 3 THEN 'WEDNESDAY' WHEN 4 THEN 'THIRSDAY'  ELSE 'OTHERDAY' END AS TEST;
+-----------+
| TEST      |
+-----------+
| WEDNESDAY |
+-----------+
1 row in set (0.00 sec)
```

> **一个CASE表达式的默认返回值类型是任何返回值的相容集合类型，但具体情况视其所在语境而定。如果用在字符串语境中，则返回结果为字符串。如果用在数字语境中，则返回结果为十进制值、实数值或整数值。**

```sql
SELECT
	empno 员工编号,
	ename 员工姓名,
	job,
CASE
	job 
	WHEN 'CLERK' THEN
	'店员' 
	WHEN 'sALE职位 ' THEN    -- 不区分大小写
	'销售' 
	WHEN 'manager' THEN
	'管理者' 
	WHEN 'analyst' THEN
	'分析师' 
	ELSE '负责人' 
	END  '岗位'
	
FROM
	emp;
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/359cd5159b404930891e7d332bab34d5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# 流程控制

## if()函数

先来最基本的两个函数----

```sql
mysql> select if(true,'hello','world');
+--------------------------+
| if(true,'hello','world') |
+--------------------------+
| hello                    |
+--------------------------+
1 row in set (0.00 sec)

mysql> select if(false,'hello','world');
+---------------------------+
| if(false,'hello','world') |
+---------------------------+
| world                     |
+---------------------------+
1 row in set (0.00 sec)

mysql>

```

## case 函数

```sql
select *,case  job
when 'clerk'   then  '收银员'
 when  'salesman' then '销售员'
 when 'analyst' then '市场分析师'
when 'manager'  then '小白领'
else '领导'
end
中文职位
from emp1 
;


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c485814c705c46228b7729cc710d8d34.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 流程控制里的if

```sql
create  PROCEDURE testmy(in name varchar(6),out countnum int)
begin
declare mycondition1 CONDITION for 1048 ;
declare mycondition2 condition for SQLSTATE '42000';
declare continue HANDLER for mycondition1 set @info='继续';
declare exit HANDLER for mycondition2 set @info='停止';
if name is null or name=''
then 
select * from emp ;
elseif name ='king' 
then 
select * from emp where ename='king';
else 
select * from emp where job='clerk';
end if ;
select FOUND_ROWS() into countnum;
end;;
> OK
> 时间: 0.027s
call testmy('king',@countnum);
+-------+-------+-----------+------+------------+---------+------+--------+
| EMPNO | ENAME | JOB       | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
+-------+-------+-----------+------+------------+---------+------+--------+
|  7893 | KING  | PRESIDENT | NULL | 1981-11-17 | 5000.00 | NULL |     10 |
+-------+-------+-----------+------+------------+---------+------+--------+
1 row in set (0.00 sec)

select @countnum;
+-----------+
| @countnum |
+-----------+
|         1 |
+-----------+
1 row in set (0.00 sec)


```

注意的点是 elseif在mysql中要合在一块写;

## 流程控制里的case

有两种方式---
第一种-------如下代码

```sql
delimiter ;;
-- 查询在这个工资以下的员工信息
CREATE procedure  mypro8(in sale int,out countnum int)
BEGIN
case 
when sale <=0  then update emp1 set salgrade ='已离职' where sal <=sale ;
when sale<= 1800 then update emp1 set salgrade='涨工资500' where sal <=sale ;
when sale <=3000 then update emp1 set salgrade='涨工资300' where sal <=sale ;
else UPDATE emp1 set salgrade ='维持原工资' where sal >3000;
end case;
end
> Affected rows: 0
> 时间: 0.03s

-- 调用
call mypro8(1800,@count)
> OK
> 时间: 0.025s

mysql> select * from emp1;---原表参考case函数里的图
+-------+--------+-----------+------+------------+---------+---------+--------+-----------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO | SALGRADE  |
+-------+--------+-----------+------+------------+---------+---------+--------+-----------+
|  7367 | NULL   | NULL      | NULL | NULL       |    NULL |    NULL |     40 | NULL      |
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 | 涨工资500 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 | 涨工资500 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 | 涨工资500 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 | NULL      |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 | 涨工资500 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 | NULL      |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 | NULL      |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 | NULL      |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 | 涨工资500 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 | 涨工资500 |
|  7893 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 | NULL      |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 | 涨工资500 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 | NULL      |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 | 涨工资500 |
+-------+--------+-----------+------+------------+---------+---------+--------+-----------+
15 rows in set (0.00 sec)

```

第二种就类似于case函数的形式,

~~~sql
CREATE procedure  mypro9(in works varchar(18),out countnum int)
BEGIN
case works  
when 'clerk'  then update emp1 set salgrade ='涨工资1000' where job=works ;
when 'salesman' then  update emp1 set salgrade='涨工资500' where job=works ;
when 'ANALYST' then update emp1 set salgrade='涨工资300' where  job=works ;
else UPDATE emp1 set salgrade ='维持原工资' where job=works;
end case;
end
> Affected rows: 0
> 时间: 0.027s

call mypro9('clerk',@countnum);
select * from emp1;

```sql

mysql> select * from emp1;
+-------+--------+-----------+------+------------+---------+---------+--------+------------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO | SALGRADE   |
+-------+--------+-----------+------+------------+---------+---------+--------+------------+
|  7367 | NULL   | NULL      | NULL | NULL       |    NULL |    NULL |     40 | NULL       |
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 | 涨工资1000 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 | NULL       |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 | NULL       |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 | NULL       |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 | NULL       |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 | NULL       |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 | NULL       |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 | NULL       |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 | NULL       |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 | 涨工资1000 |
|  7893 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 | NULL       |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 | 涨工资1000 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 | NULL       |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 | 涨工资1000 |
+-------+--------+-----------+------+------------+---------+---------+--------+------------+
15 rows in set (0.00 sec)
~~~



> **这里的CASE语句不能有ELSE NULL子句，并且用ENDCASE替代END来终止。

## LOOP语句

说到这我突然就想起了----benchmark(a,b)函数---将b重复执行a次

```sql
mysql> select BENCHMARK(50000000,md5('I love you'));
+---------------------------------------+
| BENCHMARK(50000000,md5('I love you')) |
+---------------------------------------+
|                                     0 |
+---------------------------------------+
1 row in set (20.68 sec)
```

这个时间是本地时间而不是到服务器的耗时

还有一个可以重复操作的函数----

```sql
select repeat('Iloveyou',6);
+--------------------------------------------------+
| repeat('Iloveyou',6)                             |
+--------------------------------------------------+
| IloveyouIloveyouIloveyouIloveyouIloveyouIloveyou |
+--------------------------------------------------+
1 row in set (0.00 sec)
```

说正题-----
**loop循环控制函数-----**

> **LOOP循环语句用来重复执行某些语句，与IF和CASE语句相比，LOOP只是创建一个循环操作的过程，并不进行条件判断。LOOP内的语句一直重复执行直到循环被退出（使用LEAVE子句），跳出循环过程;**

格式--

![在这里插入图片描述](D:/WebRepository/pic/mysql_pic/loop.png)

执行加法操作,求100之内的整数的和;

```sql
mysql> create function fun_add(num int) returns int
    -> reads sql data
    -> begin
    ->     declare i int default 0;
    ->     declare result int default 0;
    ->
    ->     myloop:loop
    ->         set i=i+1;
    ->         set result=i+result;
    ->         if i>=num
    ->         then
    ->         leave myloop;
    ->         end if;
    ->     end loop myloop;
    ->     return result;
    -> end ;;
Query OK, 0 rows affected (0.03 sec)

mysql> select fun_add(100);
    -> ;;
+--------------+
| fun_add(100) |
+--------------+
|         5050 |
+--------------+
1 row in set (0.00 sec)
```

每次登录mysql你怎么知道mysql的系统内部信息?这就需要知道mysql 内部的一些函数---



# 系统函数

**version()**

```sql
mysql> select version();
+-----------+
| version() |
+-----------+
| 8.0.26    |
+-----------+
1 row in set (0.00 sec)
```

**连接函数---查看当前连接的次数**

**connection_id()**

```sql
mysql> select connection_id();
+-----------------+
| connection_id() |
+-----------------+
|               9 |
+-----------------+
1 row in set (0.00 sec)
```

**查看当前用户连接信息**

```sql
processlist命令的输出结果显示了有哪些线程在运行，不仅可以查看当前所有的连接数，还可以查看当前的连接状态、帮助识别出有问题的查询语句等。
```

**show processlist \G**

```sql
mysql> show processlist \G
*************************** 1. row ***************************
     Id: 5
   User: event_scheduler
   Host: localhost
     db: NULL
Command: Daemon
   Time: 105079
  State: Waiting on empty queue
   Info: NULL
*************************** 2. row ***************************
     Id: 9
   User: Gavin
   Host: localhost
     db: test
Command: Query
   Time: 0
  State: init
   Info: show processlist
2 rows in set (0.00 sec)

mysql>
```

**查看所有用户连接信息------管理员**

**show full processlist \G**

```sql
mysql> show full processlist \G
*************************** 1. row ***************************
     Id: 5
   User: event_scheduler
   Host: localhost
     db: NULL
Command: Daemon
   Time: 105241
  State: Waiting on empty queue
   Info: NULL
*************************** 2. row ***************************
     Id: 9
   User: Gavin
   Host: localhost
     db: test
Command: Query
   Time: 0
  State: init
   Info: show full processlist
2 rows in set (0.00 sec)
```

每行信息代表的含义
 Id: 9--------  当前用户的ID(由系统分配)

   User: Gavin-----当前用户名(如果不是root，这个命令就只显示用户权限范围内的SQL语句。)

   Host: localhost------登陆的端口号,用于追踪用户和出现的问题

db: test------当前操作的数据库名
     
Command: Query-----当前执行的命令一般有（Sleep）、查询（Query）、连接（Connect）,系统线程会有 Daemon(守护)

   Time: 0------显示当前操作持续的时间

  State: init-------sql语句的状态,以查询为例，可能需要经过Copying to tmp table、Sortingresult、Sending data等状态才可以完成。
   Info: show full processlist-----显示当前操作的语句,如果是管理员账户还可以看到非管理员的操作语句;

**获取当前操作的数据库名**

DATABASE()和SCHEMA()函数返回使用utf8字符集的默认（当前）数据库名。

```sql
mysql> select database(),schema();
+------------+----------+
| database() | schema() |
+------------+----------+
| test       | test     |
+------------+----------+
1 row in set (0.00 sec)
```

**获取用户名**

```sql
mysql> select user() as user1,current_user as user2,current_user() as user3,system_user() as user4,session_user() as user5;
+-----------------+---------+---------+-----------------+-----------------+
| user1           | user2   | user3   | user4           | user5           |
+-----------------+---------+---------+-----------------+-----------------+
| Gavin@localhost | Gavin@% | Gavin@% | Gavin@localhost | Gavin@localhost |
+-----------------+---------+---------+-----------------+-----------------+
1 row in set (0.00 sec)
```

> **这几个函数返回当前被MySQL服务器验证的用户名和主机名组合。这个值符合确定当前登录用户存取权限的MySQL账户。一般情况下，这几个函数的返回值是相同的。**

查看系统编码格式

```sql
mysql> select charset('asfsf') as zifu1 ,charset(convert('abc' using latin1)) as zifu2, charset(version()) as zifu3;
+-------+--------+---------+
| zifu1 | zifu2  | zifu3   |
+-------+--------+---------+
| gbk   | latin1 | utf8mb3 |
+-------+--------+---------+
1 row in set (0.00 sec)
```

使用COLLATION()函数返回字符串排列方式

```sql
mysql> select collation('abc') as zifu1 ,collation(convert('abc' using latin1)) as zifu2, collation(version()) as zifu3;
+----------------+-------------------+-----------------+
| zifu1          | zifu2             | zifu3           |
+----------------+-------------------+-----------------+
| gbk_chinese_ci | latin1_swedish_ci | utf8_general_ci |
+----------------+-------------------+-----------------+
1 row in set (0.00 sec)
```

可以看到，使用不同字符集时字符串的排列方式不同。

**获取最后一个自动生成的ID值的函数**

这个有什么用呢?---查看当前数据表插入操作执行了几次了;

建表------

```sql
mysql> create table test01(
    -> id int(3) primary key auto_increment
    -> );
Query OK, 0 rows affected, 1 warning (0.05 sec)

mysql> show warnings;
+---------+------+------------------------------------------------------------------------------+
| Level   | Code | Message                                                                      |
+---------+------+------------------------------------------------------------------------------+
| Warning | 1681 | Integer display width is deprecated and will be removed in a future release. |
+---------+------+------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

插入数据----5次,每次插入一条

```sql
mysql> insert into test01 values(null);
Query OK, 1 row affected (0.01 sec)

mysql> insert into test01 values(null);
Query OK, 1 row affected (0.01 sec)

mysql> insert into test01 values(null);
Query OK, 1 row affected (0.01 sec)

mysql> insert into test01 values(null);
Query OK, 1 row affected (0.01 sec)

mysql> insert into test01 values(null);
Query OK, 1 row affected (0.00 sec)
```

查看数据----

```sql
mysql> SELECT * FROM test01;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  4 |
|  5 |
+----+
5 rows in set (0.00 sec)
```

查看最后一次插入的id

```sql
mysql> SELECT  LAST_INSERT_ID();
+------------------+
| LAST_INSERT_ID() |
+------------------+
|                5 |
+------------------+
1 row in set (0.00 sec)
```

继续插入数据----这次执行插入一次操作,同时插入4条记录---再查看最后一次插入的id

```sql
mysql> insert into test01 values(null),(null),(null),(null);
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM test01;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  4 |
|  5 |
|  6 |
|  7 |
|  8 |
|  9 |
+----+
9 rows in set (0.00 sec)

mysql> SELECT  LAST_INSERT_ID();
+------------------+
| LAST_INSERT_ID() |
+------------------+
|                6 |
+------------------+
1 row in set (0.00 sec)
```

可以发现最后一次插入的id跟记录条数无关,而是跟插入操作的次数有关;

如果此时向别的表插入数据-----

```sql
mysql> create table test02(
    -> id int primary key auto_increment
    -> );
Query OK, 0 rows affected (0.04 sec)

mysql> insert into test02 values(null);
Query OK, 1 row affected (0.01 sec)

mysql> insert into test02 values(null);
Query OK, 1 row affected (0.01 sec)

mysql> insert into test02 values(null);
Query OK, 1 row affected (0.00 sec)

mysql> select last_insert_id();
+------------------+
| last_insert_id() |
+------------------+
|                3 |
+------------------+
1 row in set (0.00 sec)
```

LAST_INSERT_ID是与数据表无关的，如果向表a插入数据后再向表b插入数据，那么LAST_INSERT_ID返回表b中的Id值。即每张表是各自管各自的;

@[ ](数据库中的加密)

# 数据加密

> ## 如何能让数据库比较安全呢?
>
> mysql中提供了一些列加密函数,加密函数主要用来对数据进行加密和界面处理，以保证某些重要数据不被别人获取。这些函数在保证数据库安全时非常有用

## MD5(str)加密函数

MD5(str)为字符串算出一个MD5 128比特校验和。该值以32位十六进制数字的二进制字符串形式返回，若参数为NULL，则会返回NULL。

```sql
mysql> select md5('weyui');
+----------------------------------+
| md5('weyui')                     |
+----------------------------------+
| 0752cfa48527ad63886b069b7b191c84 |
+----------------------------------+
1 row in set (0.00 sec)

```

## SHA(str)加密函数

SHA(str)从原明文密码str计算并返回加密后的密码字符串，当参数为NULL时，返回NULL。

```sql
mysql> select sha('weyui');
+------------------------------------------+
| sha('weyui')                             |
+------------------------------------------+
| 7769cfc05bc65a3a0468dd439c9e093869939005 |
+------------------------------------------+
1 row in set (0.00 sec)
```

## MD5(str)与SHA(str)加密方式对比

```sql
mysql> select md5('weyui'),sha('weyui');
+----------------------------------+------------------------------------------+
| md5('weyui')                     | sha('weyui')                             |
+----------------------------------+------------------------------------------+
| 0752cfa48527ad63886b069b7b191c84 | 7769cfc05bc65a3a0468dd439c9e093869939005 |
+----------------------------------+------------------------------------------+
1 row in set (0.00 sec)
```

从加密后的数据来看,SHA加密算法比MD5更加安全。

## SHA2(str, hash_length)加密函数

SHA2(str, hash_length)使用hash_length作为长度，加密str。hash_length支持的值为224、256、384、512和0。其中，0等同于256。

```sql
mysql> select sha2('weyui',256);
+------------------------------------------------------------------+
| sha2('weyui',256)                                                |
+------------------------------------------------------------------+
| 90a49f4112f54760730a32f1e866f058e476cf7c6c5bd728454e5796d572539d |
+------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> select sha2('weyui',0);
+------------------------------------------------------------------+
| sha2('weyui',0)                                                  |
+------------------------------------------------------------------+
| 90a49f4112f54760730a32f1e866f058e476cf7c6c5bd728454e5796d572539d |
+------------------------------------------------------------------+
1 row in set (0.00 sec)
```

随着加密算法的演变,加密的强度越来愈大,sha2()可以自定义加密的强度;

## 数据库加密的守门员

今天突然想到了一个点，就是我们再用一些图形化软件的时候在登陆mysql8.0版本的数据库时会遇到一个问题

在输入数据库账户密码时及时密码输入正确也会提示----- 2059错误，原因就在于加密的方式不一样，

好不容易找到了一个截图---这是navicat的------
![在这里插入图片描述](https://img-blog.csdnimg.cn/f5290a7be5c44eb9af61309fdec53da7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这是sqylog 的----
![在这里插入图片描述](https://img-blog.csdnimg.cn/a072b12a233847bd8dcb6d84f5012613.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)我们当时的操作是 修改数据库的加密规则为 mysql_native_password
，在修改之前我们先查看一下数据库的账户密码加密规则--默认的加密规则为caching_sha2_password

```sql
mysql> show variables like 'default_authentication_plugin';
+-------------------------------+-----------------------+
| Variable_name                 | Value                 |
+-------------------------------+-----------------------+
| default_authentication_plugin | caching_sha2_password |
+-------------------------------+-----------------------+
1 row in set, 1 warning (0.01 sec)

--- 再来看用户的密码加密方式
mysql> select host,user,plugin from mysql.user;
+-----------+------------------+-----------------------+
| host      | user             | plugin                |
+-----------+------------------+-----------------------+
| %         | gavin            | caching_sha2_password |
| localhost | mysql.infoschema | caching_sha2_password |
| localhost | mysql.session    | caching_sha2_password |
| localhost | mysql.sys        | caching_sha2_password |
| localhost | root             | caching_sha2_password |
+-----------+------------------+-----------------------+
5 rows in set (0.00 sec)
```

如何解决问题------ 修改加密的密码为经过--- mysql_native_password 加密的格式-

```sql
mysql> alter user   用户    identified with mysql_native_password by '密码' ;
Query OK, 0 rows affected (0.03 sec)
```

由于密码作为数据库加密的第一道防线，sha2 的加密强度比  mysql_native_password  的方式要强，所以我们在安装的时候就会推荐我们按照   sha2 的方式加密----
![在这里插入图片描述](https://img-blog.csdnimg.cn/6084bb843bfe482caf56f5ee5408cb1a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_14,color_FFFFFF,t_70,g_se,x_16)
翻了一下官方的源码-----
mysql8.0 的新特性----
caching_sha2_password as the Preferred Authentication Plugin

![在这里插入图片描述](https://img-blog.csdnimg.cn/7762a89cc3ae40cf97fe45b2fc69dfbc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



```sql
 /*This change applies only to new accounts created after installing or upgrading to MySQL 8.0 or higher. For accounts already existing in an upgraded installation, their authentication plugin remains unchanged. Existing users who wish to switch to caching_sha2_password can do so using the ALTER USER statement:*/

ALTER USER user
  IDENTIFIED WITH caching_sha2_password
  BY 'password';
```

根据官方给出的代码，我们改回 mysql_native_password 不难吧。

<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/mysql/MySQLBase02.md#基本数据类型)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/mysql/MySQLBase04.md#数据库操作)

</center>

![](..\pic\div\plant.gif)