![](..\pic\logo2.png)

# 数据库操作

# 插入数据

**插入数据时候----**

> **同时插入多行记录的INSERT语句等同于多个单行插入的INSERT语句，但是多行的INSERT语句在处理过程中效率更高。因为MySQL执行单条INSERT语句插入多行数据比使用多条INSERT语句快，所以在插入多条记录时最好选择使用单条INSERT语句的方式插入。**

传统插入方式不在赘述,
向大部分数据库一样,mysql也支持将查询的列插入到某一张表中;

建表----插入----然后清空

```sql
mysql> select * from emp2;
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
14 rows in set (0.00 sec)

mysql> truncate emp2;
Query OK, 0 rows affected (0.11 sec)

mysql> select * from emp2;
Empty set (0.02 sec)
```

之后

```sql
-- 可以选择指定字段, --
mysql> insert into emp2  select  * from emp;
Query OK, 14 rows affected (0.04 sec)
Records: 14  Duplicates: 0  Warnings: 0

mysql> select * from emp2;
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     40 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     40 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
14 rows in set (0.00 sec)

```

# 查询数据

建表什么的不在赘述,

直接上活-----

```sql
mysql> select * from userinfo order by score desc ,height;
+------+----------+--------+-------+--------+
| id   | name     | gender | score | height |
+------+----------+--------+-------+--------+
| 1005 | 古力娜华 | 女     | 145.6 |  166.7 |
| 1002 | 李四     | 女     | 130.5 |  167.3 |
| 1001 | 张三     | 男     | 128.8 |  173.4 |
| 1009 | 李二狗   | 男     | 123.4 |  178.9 |
| 1004 | 李六     | 男     | 123.4 |  180.4 |
| 1010 | 王建国   | 男     | 120.9 |  166.4 |
| 1003 | 王五     | 男     | 111.7 |  177.8 |
| 1008 | Gavin    | 男     | 111.3 |  188.8 |
| 1006 | 阿依古丽 | 女     | 100.4 |  158.9 |
| 1007 | Jack     | 女     | 100.4 |  198.7 |
+------+----------+--------+-------+--------+
10 rows in set (0.00 sec)
```

**需求一----**
按照score降序height升序------

```sql
mysql> select * from userinfo order by score desc,height asc;
+------+----------+--------+-------+--------+
| id   | name     | gender | score | height |
+------+----------+--------+-------+--------+
| 1005 | 古力娜华 | 女     | 145.6 |  166.7 |
| 1002 | 李四     | 女     | 130.5 |  167.3 |
| 1001 | 张三     | 男     | 128.8 |  173.4 |
| 1009 | 李二狗   | 男     | 123.4 |  178.9 |
| 1004 | 李六     | 男     | 123.4 |  180.4 |
| 1010 | 王建国   | 男     | 120.9 |  166.4 |
| 1003 | 王五     | 男     | 111.7 |  177.8 |
| 1008 | Gavin    | 男     | 111.3 |  188.8 |
| 1006 | 阿依古丽 | 女     | 100.4 |  158.9 |
| 1007 | Jack     | 女     | 100.4 |  198.7 |
+------+----------+--------+-------+--------+
10 rows in set (0.00 sec)
```

**需求2-------**
按照性别分组,求出男女的平均身高,并要查看男女方所有人姓名;

 分析----要求的是平均身高,所以选择的主体目标是 select avg(height),然后再添加附加条件---

```sql
mysql> select avg(height) as '平均身高' ,group_concat(name) as '姓名'  from userinfo group by gender;
+-----------+------------------------------------+
| 平均身高  | 姓名                               |
+-----------+------------------------------------+
| 172.90000 | 李四,古力娜华,阿依古丽,Jack        |
| 177.61666 | 张三,王五,李六,Gavin,李二狗,王建国 |
+-----------+------------------------------------+
2 rows in set (0.00 sec)
```

**需求3----**
统计男女数量,并显示出男女姓名

```sql
mysql> select gender ,count(gender) as '人数',group_concat(name) as '姓名' from userinfo group by gender;
+--------+------+------------------------------------+
| gender | 人数 | 姓名                               |
+--------+------+------------------------------------+
| 女     |    4 | 李四,古力娜华,阿依古丽,Jack        |
| 男     |    6 | 张三,王五,李六,Gavin,李二狗,王建国 |
+--------+------+------------------------------------+
2 rows in set (0.00 sec)
```

> 查看字段的每条记录名称，可以在GROUP BY子句中使用GROUP_CONCAT()函数，将每个分组中各个字段的值显示出来。





> HAVING关键字与WHERE关键字都是用来过滤数据的，两者有什么区别呢？其中重要的一点是，HAVING在数据分组之后进行过滤来选择分组，而WHERE在分组之前来选择记录。另外，WHERE排除的记录不再包括在分组中。

对于所得结果数据之查看部分-----

```sql
mysql> select * from userinfo limit 4,3;
+------+----------+--------+-------+--------+-----------+
| id   | name     | gender | score | height | fruitname |
+------+----------+--------+-------+--------+-----------+
| 1005 | 古力娜华 | 女     | 145.6 |  166.7 | orange    |
| 1006 | 阿依古丽 | 女     | 100.4 |  158.9 | mongo     |
| 1007 | Jack     | 女     | 100.4 |  198.7 | banana    |
+------+----------+--------+-------+--------+-----------+
3 rows in set (0.00 sec)

mysql> select * from userinfo limit 4 offset 3;
+------+----------+--------+-------+--------+-----------+
| id   | name     | gender | score | height | fruitname |
+------+----------+--------+-------+--------+-----------+
| 1004 | 李六     | 男     | 123.4 |  180.4 | apple     |
| 1005 | 古力娜华 | 女     | 145.6 |  166.7 | orange    |
| 1006 | 阿依古丽 | 女     | 100.4 |  158.9 | mongo     |
| 1007 | Jack     | 女     | 100.4 |  198.7 | banana    |
+------+----------+--------+-------+--------+-----------+
4 rows in set (0.00 sec)

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/7ccb18d6b63641d0be6ba4314c665972.png)

小结-----

**查询数据**  
1,查询某写字段下所有信息---
 select 字段名1,字段名2,.......字段名n from 表名;
 2,查询表中所有信息 
 select * from 表名;
3,限制查询条件达到筛选目的---
select 字段名          from 表名  where 条件1 and/or 条件2

总体格式就是------
4,select    字段名 as 查询字段的临时字段名  from 表名 where 条件  group by 字段名 order by 字段名 limit 偏移量,记录数量;

其中可以穿插各种函数;

## MySQL聚合函数

**MySQL聚合函数**
![在这里插入图片描述](https://img-blog.csdnimg.cn/445cdad4d3b34c218cf3dd9c8c5b03b5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)COUNT()函数

COUNT()函数统计数据表中包含的记录行的总数，或者根据查询结果返回列中包含的数据行数。其使用方法有两种：

**

> ●　COUNT(*)计算表中总的行数，不管某列是否有数值或者为空值。
>  ●　COUNT(字段名)计算指定列下总的行数，计算时将忽略空值的行。

------

***空值被COUNT(字段名)函数忽略；
而在COUNT(*)函数所有记录都不忽略。**

**sum()函数再计算和的时候忽略null的行;**

## 联结查询

将多张表连结起来以便于查询数据,

### 內联结查询

#### 传统方式--where

将字段通过where进行筛选

```sql
mysql> select userinfo.id ,userinfo.name from userinfo where userinfo.id=usertest.id  ;
ERROR 1054 (42S22): Unknown column 'usertest.id' in 'where clause'
mysql> select userinfo.id ,userinfo.name from userinfo,usertest where userinfo.id=usertest.id  ;
+------+----------+
| id   | name     |
+------+----------+
| 1001 | 张三     |
| 1003 | 王五     |
| 1005 | 古力娜华 |
| 1007 | Jack     |
| 1008 | Gavin    |
| 1009 | 李二狗   |
| 1010 | 王建国   |
| 1011 | 玛丽亚   |
+------+----------+
8 rows in set (0.02 sec)

```

#### 新方式--inner join方式

```sql
mysql> select userinfo.id ,userinfo.name from userinfo inner join usertest on userinfo.id=usertest.id;
+------+----------+
| id   | name     |
+------+----------+
| 1001 | 张三     |
| 1003 | 王五     |
| 1005 | 古力娜华 |
| 1007 | Jack     |
| 1008 | Gavin    |
| 1009 | 李二狗   |
| 1010 | 王建国   |
| 1011 | 玛丽亚   |
+------+----------+
8 rows in set (0.00 sec)
```

**内连接（INNER JOIN）使用比较运算符进行表间某（些）列数据的比较操作，并列出这些表中与连接条件相匹配的数据行，组合成新的记录，也就是说，在内连接查询中，只有满足条件的记录才能出现在结果关系中。**

一般是建议使用內联结,有些情况下使用where 操作可能会影响性能;

#### 自联结查询

內联结除了可以跟别的表进行之外还可以自己跟自己联结------又叫自联结查询.

### 外联结查询

#### 左联结查询

　LEFT JOIN（左连接）：返回包括左表中的所有记录和右表中连接字段相等的记录。

```sql
mysql> select userinfo.name,userinfo.fruitname from userinfo left join usertest on userinfo.name =usertest.name ;
+----------+-----------+
| name     | fruitname |
+----------+-----------+
| 张三     | pear      |
| 李四     | apple     |
| 王五     | pear      |
| 李六     | apple     |
| 古力娜华 | orange    |
| 阿依古丽 | mongo     |
| Jack     | banana    |
| Gavin    | mongo     |
| 李二狗   | orange    |
| 王建国   | banana    |
| 玛丽亚   | NULL      |
| 穿耳戈   | NULL      |
+----------+-----------+
12 rows in set (0.02 sec)

ysql> select userinfo.name,userinfo.fruitname from userinfo left outer join usertest on userinfo.name =usertest.name ;
+----------+-----------+
| name     | fruitname |
+----------+-----------+
| 张三     | pear      |
| 李四     | apple     |
| 王五     | pear      |
| 李六     | apple     |
| 古力娜华 | orange    |
| 阿依古丽 | mongo     |
| Jack     | banana    |
| Gavin    | mongo     |
| 李二狗   | orange    |
| 王建国   | banana    |
| 玛丽亚   | peak      |
| 穿耳戈   | peak      |
+----------+-----------+
12 rows in set (0.02 sec)

```

**left join   等同于left outer join-----  可以看作省略outer**



#### 右联结查询

RIGHT JOIN（右连接）：返回包括右表中的所有记录和左表中连接字段相等的记录。

```sql
mysql> select userinfo.name,userinfo.fruitname from userinfo right join usertest on userinfo.name =usertest.name ;
+----------+-----------+
| name     | fruitname |
+----------+-----------+
| 张三     | pear      |
| 王五     | pear      |
| 古力娜华 | orange    |
| Jack     | banana    |
| Gavin    | mongo     |
| 李二狗   | orange    |
| 王建国   | banana    |
| 玛丽亚   | NULL      |
| NULL     | NULL      |
| 阿依古丽 | mongo     |
| 李六     | apple     |
+----------+-----------+
11 rows in set (0.00 sec)


mysql> select userinfo.name,userinfo.fruitname from userinfo right outer join usertest on userinfo.name =usertest.name ;
+----------+-----------+
| name     | fruitname |
+----------+-----------+
| 张三     | pear      |
| 王五     | pear      |
| 古力娜华 | orange    |
| Jack     | banana    |
| Gavin    | mongo     |
| 李二狗   | orange    |
| 王建国   | banana    |
| 玛丽亚   | peak      |
| NULL     | NULL      |
| 阿依古丽 | mongo     |
| 李六     | apple     |
+----------+-----------+
11 rows in set (0.00 sec)
```

#### 联结查询---內联结

就是再内联结条件上进一步细化得到的查询

```sql
mysql> select u1.name,u1.fruitname from userinfo as u1 inner join usertest as u2 on u1.id=u2.id ;
+----------+-----------+
| name     | fruitname |
+----------+-----------+
| 张三     | pear      |
| 王五     | pear      |
| 古力娜华 | orange    |
| Jack     | banana    |
| Gavin    | mongo     |
| 李二狗   | orange    |
| 王建国   | banana    |
| 玛丽亚   | peak      |
+----------+-----------+
8 rows in set (0.00 sec)

mysql> select u1.name,u1.fruitname from userinfo as u1 inner join usertest as u2 on u1.id=u2.id  and u1.id=1008;
+-------+-----------+
| name  | fruitname |
+-------+-----------+
| Gavin | mongo     |
+-------+-----------+
1 row in set (0.00 sec)
```

## 子查询

子查询很容易理解就是在查询的基础之前先进行一次条件筛选;
**有子查询时,先执行内层子查询，再执行外层查询，内层子查询的结果作为外部查询的比较条件。**



```sql
mysql> select userinfo.name ,userinfo.score from userinfo where userinfo.name in(select name from usertest);
+----------+-------+
| name     | score |
+----------+-------+
| 张三     | 128.8 |
| 王五     | 111.7 |
| 李六     | 123.4 |
| 古力娜华 | 145.6 |
| 阿依古丽 | 100.4 |
| Jack     | 100.4 |
| Gavin    | 111.3 |
| 李二狗   | 123.4 |
| 王建国   | 120.9 |
| 玛丽亚   |  NULL |
+----------+-------+
10 rows in set (0.02 sec)
```

子查询指一个查询语句嵌套在另一个查询语句内部的查询，这个特性从MySQL 4.1开始引入。在SELECT子句中先计算子查询，子查询结果作为外层另一个查询的过滤条件，查询可以基于一个表或者多个表。子查询中常用的**操作符有ANY（SOME）、ALL、IN、EXISTS**。子查询**可以添加到SELECT、UPDATE和DELETE语句中，而且可以进行多层嵌套。**子查询中**也可以使用比较运算符**，如**“<”“<=”“>”“>=”和“!=”**等。



### 带some/any 与all的子查询之间的区别

```sql
**原表数据-----**
mysql> select * from user01;
+--------+
| scores |
+--------+
|    111 |
|    112 |
|    118 |
|    120 |
|    121 |
|    108 |
+--------+
6 rows in set (0.00 sec)

mysql> select * from userinfo;
+------+----------+--------+-------+--------+-----------+
| id   | name     | gender | score | height | fruitname |
+------+----------+--------+-------+--------+-----------+
| 1001 | 张三     | 男     | 128.8 |  173.4 | pear      |
| 1002 | 李四     | 女     | 130.5 |  167.3 | apple     |
| 1003 | 王五     | 男     | 111.7 |  177.8 | pear      |
| 1004 | 李六     | 男     | 123.4 |  180.4 | apple     |
| 1005 | 古力娜华 | 女     | 145.6 |  166.7 | orange    |
| 1006 | 阿依古丽 | 女     | 100.4 |  158.9 | mongo     |
| 1007 | Jack     | 女     | 100.4 |  198.7 | banana    |
| 1008 | Gavin    | 男     | 111.3 |  188.8 | mongo     |
| 1009 | 李二狗   | 男     | 123.4 |  178.9 | orange    |
| 1010 | 王建国   | 男     | 120.9 |  166.4 | banana    |
| 1011 | 玛丽亚   | 男     |  NULL |   NULL | peak      |
| 1012 | 穿耳戈   | 男     |  NULL |   NULL | peak      |
+------+----------+--------+-------+--------+-----------+
12 rows in set (0.00 sec)
```

```sql
mysql> select name ,score from userinfo where score>any (select scores from user01);
+----------+-------+
| name     | score |
+----------+-------+
| 张三     | 128.8 |
| 李四     | 130.5 |
| 王五     | 111.7 |
| 李六     | 123.4 |
| 古力娜华 | 145.6 |
| Gavin    | 111.3 |
| 李二狗   | 123.4 |
| 王建国   | 120.9 |
+----------+-------+
8 rows in set (0.02 sec)

mysql> select name ,score from userinfo where score>some (select scores from user01);
+----------+-------+
| name     | score |
+----------+-------+
| 张三     | 128.8 |
| 李四     | 130.5 |
| 王五     | 111.7 |
| 李六     | 123.4 |
| 古力娜华 | 145.6 |
| Gavin    | 111.3 |
| 李二狗   | 123.4 |
| 王建国   | 120.9 |
+----------+-------+
8 rows in set (0.00 sec)
mysql> select name ,score from userinfo where score>all (select scores from user01);
+----------+-------+
| name     | score |
+----------+-------+
| 张三     | 128.8 |
| 李四     | 130.5 |
| 李六     | 123.4 |
| 古力娜华 | 145.6 |
| 李二狗   | 123.4 |
+----------+-------+
5 rows in set (0.00 sec)


```

可以发现some和any的效果是一样的;

**select name ,score from userinfo where score>some (select scores from user01);**
------some/any 表示 满足其中的一个条件即可,即userinfo.score 的值大于user01.scores中的最小值就算满足条件;

**select name ,score from userinfo where score>all (select scores from user01);**

------all表示满足userinfo.score 的值大于user01.scores中的所有值,即要求userinfo.score 的值大于user01.scores中的最大值

### 带(not) exists 的查询

```sql
mysql> select name ,score from userinfo where exists(select score from userinfo where score=150);
Empty set (0.02 sec)

mysql> select name ,score from userinfo where exists(select score from userinfo where score=100.4);
+----------+-------+
| name     | score |
+----------+-------+
| 张三     | 128.8 |
| 李四     | 130.5 |
| 王五     | 111.7 |
| 李六     | 123.4 |
| 古力娜华 | 145.6 |
| 阿依古丽 | 100.4 |
| Jack     | 100.4 |
| Gavin    | 111.3 |
| 李二狗   | 123.4 |
| 王建国   | 120.9 |
| 玛丽亚   |  NULL |
| 穿耳戈   |  NULL |
+----------+-------+
12 rows in set (0.00 sec)
```

可以发现exists的作用是如果exists后面的执行结果为true则执行前面的查询,否则不执行;
即由于不存在score=150 的记录结果为false,所以不执行查询,存在score=100.4的记录,结果为true所以执行查询;

**同理可以理解 not exsits ;**

```sql
mysql> select name ,score from userinfo where not exists(select score from userinfo where score=150);
+----------+-------+
| name     | score |
+----------+-------+
| 张三     | 128.8 |
| 李四     | 130.5 |
| 王五     | 111.7 |
| 李六     | 123.4 |
| 古力娜华 | 145.6 |
| 阿依古丽 | 100.4 |
| Jack     | 100.4 |
| Gavin    | 111.3 |
| 李二狗   | 123.4 |
| 王建国   | 120.9 |
| 玛丽亚   |  NULL |
| 穿耳戈   |  NULL |
+----------+-------+
12 rows in set (0.00 sec)
```

**EXISTS和NOT EXISTS的结果只取决于是否会返回行，而不取决于这些行的内容，所以这个子查询输入列表通常是无关紧要的。**

### 带(not) in的查询

```sql
mysql> select id, name ,fruitname from userinfo where id in(select id from usertest);
+------+----------+-----------+
| id   | name     | fruitname |
+------+----------+-----------+
| 1001 | 张三     | pear      |
| 1003 | 王五     | pear      |
| 1005 | 古力娜华 | orange    |
| 1007 | Jack     | banana    |
| 1008 | Gavin    | mongo     |
| 1009 | 李二狗   | orange    |
| 1010 | 王建国   | banana    |
| 1011 | 玛丽亚   | peak      |
+------+----------+-----------+
8 rows in set (0.00 sec)

mysql> select id, name ,fruitname from userinfo where id in(1001,1002,1003,1004,1005);
+------+----------+-----------+
| id   | name     | fruitname |
+------+----------+-----------+
| 1001 | 张三     | pear      |
| 1002 | 李四     | apple     |
| 1003 | 王五     | pear      |
| 1004 | 李六     | apple     |
| 1005 | 古力娜华 | orange    |
+------+----------+-----------+
5 rows in set (0.00 sec)


```

**为了便于理解第二个做了一个简单的查询,只要id再in后面的这些数据中就筛选出来;**

not in 跟in正好相反

```sql
mysql> select id, name ,fruitname from userinfo where id not in(select id from usertest);
+------+----------+-----------+
| id   | name     | fruitname |
+------+----------+-----------+
| 1002 | 李四     | apple     |
| 1004 | 李六     | apple     |
| 1006 | 阿依古丽 | mongo     |
| 1012 | 穿耳戈   | peak      |
+------+----------+-----------+
4 rows in set (0.00 sec)
```

子查询的功能也可以通过连接查询完成，但是子查询使得MySQL代码更容易阅读和编写。

### 联结查询和子查询的比较

 用联结查询 完成  in 的  实现的效果-------

```sql
mysql> select u1.id,u1.name,u1.fruitname from userinfo as u1 inner join usertest as u2 on u1.id=u2.id;
+------+----------+-----------+
| id   | name     | fruitname |
+------+----------+-----------+
| 1001 | 张三     | pear      |
| 1003 | 王五     | pear      |
| 1005 | 古力娜华 | orange    |
| 1007 | Jack     | banana    |
| 1008 | Gavin    | mongo     |
| 1009 | 李二狗   | orange    |
| 1010 | 王建国   | banana    |
| 1011 | 玛丽亚   | peak      |
+------+----------+-----------+
8 rows in set (0.00 sec)


select id, name ,fruitname from userinfo where id in(select id from usertest);
+------+----------+-----------+
| id   | name     | fruitname |
+------+----------+-----------+
| 1001 | 张三     | pear      |
| 1003 | 王五     | pear      |
| 1005 | 古力娜华 | orange    |
| 1007 | Jack     | banana    |
| 1008 | Gavin    | mongo     |
| 1009 | 李二狗   | orange    |
| 1010 | 王建国   | banana    |
| 1011 | 玛丽亚   | peak      |
+------+----------+-----------+
8 rows in set (0.00 sec)

```

### 带比较运算符的子查询

只做演示不做赘述...............
比较符   “<”“<=”“=”“>=”和“!=”,'<=>'等等,

```sql
mysql> select id, name ,fruitname from userinfo where id not in(select id from usertest where length(name)<5);
+------+----------+-----------+
| id   | name     | fruitname |
+------+----------+-----------+
| 1001 | 张三     | pear      |
| 1002 | 李四     | apple     |
| 1003 | 王五     | pear      |
| 1004 | 李六     | apple     |
| 1005 | 古力娜华 | orange    |
| 1006 | 阿依古丽 | mongo     |
| 1008 | Gavin    | mongo     |
| 1009 | 李二狗   | orange    |
| 1010 | 王建国   | banana    |
| 1011 | 玛丽亚   | peak      |
| 1012 | 穿耳戈   | peak      |
+------+----------+-----------+
11 rows in set (0.00 sec)

mysql>
```

## 合并查询

**利用UNION关键字，可以给出多条SELECT语句，并将它们的结果组合成单个结果集。合并时，两个表对应的列数和数据类型必须相同。**

```sql
mysql> select name ,fruitname from userinfo union select name ,fruitname from usertest;
+----------+-----------+
| name     | fruitname |
+----------+-----------+
| 张三     | pear      |
| 李四     | apple     |
| 王五     | pear      |
| 李六     | apple     |
| 古力娜华 | orange    |
| 阿依古丽 | mongo     |
| Jack     | banana    |
| Gavin    | mongo     |
| 李二狗   | orange    |
| 王建国   | banana    |
| 玛丽亚   | peak      |
| 穿耳戈   | peak      |
| Gavin    | peak      |
| 李二狗   | mongo     |
| 王建国   | apple     |
| 玛丽亚   | orange    |
| 韩非     | apple     |
+----------+-----------+
17 rows in set (0.00 sec)


mysql> select name ,fruitname from userinfo union all select name ,fruitname from usertest;
+----------+-----------+
| name     | fruitname |
+----------+-----------+
| 张三     | pear      |
| 李四     | apple     |
| 王五     | pear      |
| 李六     | apple     |
| 古力娜华 | orange    |
| 阿依古丽 | mongo     |
| Jack     | banana    |
| Gavin    | mongo     |
| 李二狗   | orange    |
| 王建国   | banana    |
| 玛丽亚   | peak      |
| 穿耳戈   | peak      |
| 张三     | pear      |
| 王五     | pear      |
| 古力娜华 | orange    |
| Jack     | banana    |
| Gavin    | peak      |
| 李二狗   | mongo     |
| 王建国   | apple     |
| 玛丽亚   | orange    |
| 韩非     | apple     |
| 阿依古丽 | mongo     |
| 李六     | apple     |
+----------+-----------+
23 rows in set (0.00 sec)

```

### 带 all的union 与不带的区别

由上述数据可以看到带 all的union 与不带的区别,不带all的会将才能重复数据剔除,而带all的则显示所有数据;

## 用正则表达式查询

正则表达式符号以及含义----
![在这里插入图片描述](https://img-blog.csdnimg.cn/6783c02c1a594ab3a8d68b3cfe1ad885.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

练习正则表达式-------
源数据表

```sql
mysql> select * from userinfo
    -> ;
+------+----------+--------+-------+--------+---------------+
| id   | name     | gender | score | height | fruitname     |
+------+----------+--------+-------+--------+---------------+
| 1001 | 张三     | 男     | 128.8 |  173.4 | pear          |
| 1002 | 李四     | 女     | 130.5 |  167.3 | apple         |
| 1003 | 王五     | 男     | 111.7 |  177.8 | pear          |
| 1004 | 李六     | 男     | 123.4 |  180.4 | apple         |
| 1005 | 古力娜华 | 女     | 145.6 |  166.7 | orange        |
| 1006 | 阿依古丽 | 女     | 100.4 |  158.9 | mongo         |
| 1007 | Jack     | 女     | 100.4 |  198.7 | banana        |
| 1008 | Gavin    | 男     | 111.3 |  188.8 | mongo         |
| 1009 | 李二狗   | 男     | 123.4 |  178.9 | orange        |
| 1010 | 王建国   | 男     | 120.9 |  166.4 | banana        |
| 1011 | 玛丽亚   | 男     |  NULL |   NULL | peark         |
| 1012 | 穿耳戈   | 男     |  NULL |   NULL | peark         |
| 1013 | 张三12   | 男     |  NULL |   NULL | grape         |
| 1014 | 李四12   | 男     |  NULL |   NULL | pineaple      |
| 1015 | 王五12   | 男     |  NULL |   NULL | plum          |
| 1016 | 李六12   | 男     |  NULL |   NULL | grape         |
| 1017 | 古力娜12 | 男     |  NULL |   NULL | potato        |
| 1018 | 阿依古12 | 男     |  NULL |   NULL | carrot        |
| 1019 | jasdk    | 男     |  NULL |   NULL | aubergine     |
| 1020 | Gavinqwe | 男     |  NULL |   NULL | cabbage       |
| 1021 | NULL     | 男     |  NULL |   NULL | celery        |
| 1022 | NULL     | 男     |  NULL |   NULL | white cabbage |
| 1023 | NULL     | 男     |  NULL |   NULL | lichee        |
| 1024 | NULL     | 男     |  NULL |   NULL | lotus nut     |
+------+----------+--------+-------+--------+---------------+
24 rows in set (0.00 sec)
```

### 匹配开始/结尾的字符

**以a开头的**

```sql
mysql> select * from userinfo where fruitname regexp '^a';
+------+-------+--------+-------+--------+-----------+
| id   | name  | gender | score | height | fruitname |
+------+-------+--------+-------+--------+-----------+
| 1002 | 李四  | 女     | 130.5 |  167.3 | apple     |
| 1004 | 李六  | 男     | 123.4 |  180.4 | apple     |
| 1019 | jasdk | 男     |  NULL |   NULL | aubergine |
+------+-------+--------+-------+--------+-----------+
3 rows in set (0.00 sec)
```

**以ge结尾的**

```sql
mysql> select * from userinfo where fruitname regexp 'ge$';
+------+----------+--------+-------+--------+---------------+
| id   | name     | gender | score | height | fruitname     |
+------+----------+--------+-------+--------+---------------+
| 1005 | 古力娜华 | 女     | 145.6 |  166.7 | orange        |
| 1009 | 李二狗   | 男     | 123.4 |  178.9 | orange        |
| 1020 | Gavinqwe | 男     |  NULL |   NULL | cabbage       |
| 1022 | NULL     | 男     |  NULL |   NULL | white cabbage |
+------+----------+--------+-------+--------+---------------+
4 rows in set (0.00 sec)
```

### 匹配任一字符

```sql
mysql> select * from userinfo where fruitname  regexp 'n.a'
    -> ;
+------+--------+--------+-------+--------+-----------+
| id   | name   | gender | score | height | fruitname |
+------+--------+--------+-------+--------+-----------+
| 1014 | 李四12 | 男     |  NULL |   NULL | pineaple  |
+------+--------+--------+-------+--------+-----------+
1 row in set (0.00 sec)
```

### 匹配多个字符

```sql
mysql> select * from userinfo where fruitname  regexp 'ap+';
+------+--------+--------+-------+--------+-----------+
| id   | name   | gender | score | height | fruitname |
+------+--------+--------+-------+--------+-----------+
| 1002 | 李四   | 女     | 130.5 |  167.3 | apple     |
| 1004 | 李六   | 男     | 123.4 |  180.4 | apple     |
| 1013 | 张三12 | 男     |  NULL |   NULL | grape     |
| 1014 | 李四12 | 男     |  NULL |   NULL | pineaple  |
| 1016 | 李六12 | 男     |  NULL |   NULL | grape     |
+------+--------+--------+-------+--------+-----------+
5 rows in set (0.00 sec)--匹配a紧跟其后至少有一个p--

mysql> select * from userinfo where fruitname  regexp '^ap+';
+------+------+--------+-------+--------+-----------+
| id   | name | gender | score | height | fruitname |
+------+------+--------+-------+--------+-----------+
| 1002 | 李四 | 女     | 130.5 |  167.3 | apple     |
| 1004 | 李六 | 男     | 123.4 |  180.4 | apple     |
+------+------+--------+-------+--------+-----------+
2 rows in set (0.00 sec)--匹配以a开头紧跟其后至少有一个p--


mysql> select * from userinfo where fruitname  regexp 'a*k';
+------+--------+--------+-------+--------+-----------+
| id   | name   | gender | score | height | fruitname |
+------+--------+--------+-------+--------+-----------+
| 1011 | 玛丽亚 | 男     |  NULL |   NULL | peark     |
| 1012 | 穿耳戈 | 男     |  NULL |   NULL | peark     |
+------+--------+--------+-------+--------+-----------+
2 rows in set (0.00 sec)--匹配k前面有n个a(n最少为0)--
```

### 匹配字符串

多个字符串之间要用  |  间隔

```sql
mysql> select * from userinfo where fruitname  regexp 'an|ap';
+------+----------+--------+-------+--------+-----------+
| id   | name     | gender | score | height | fruitname |
+------+----------+--------+-------+--------+-----------+
| 1002 | 李四     | 女     | 130.5 |  167.3 | apple     |
| 1004 | 李六     | 男     | 123.4 |  180.4 | apple     |
| 1005 | 古力娜华 | 女     | 145.6 |  166.7 | orange    |
| 1007 | Jack     | 女     | 100.4 |  198.7 | banana    |
| 1009 | 李二狗   | 男     | 123.4 |  178.9 | orange    |
| 1010 | 王建国   | 男     | 120.9 |  166.4 | banana    |
| 1013 | 张三12   | 男     |  NULL |   NULL | grape     |
| 1014 | 李四12   | 男     |  NULL |   NULL | pineaple  |
| 1016 | 李六12   | 男     |  NULL |   NULL | grape     |
+------+----------+--------+-------+--------+-----------+
9 rows in set (0.00 sec)
```



# 视图

刚开始看这个词-----脑海中浮现左视图,右视图,俯视图........我试图打死你.......

> **数据库中的视图是一个虚拟表。同真实的表一样，视图包含一系列带有名称的行和列数据。行和列数据来自由定义视图查询所引用的表，并且在引用视图时动态生成。**

简单的说,视图是从一个或者多个表中导出的,与表很类似,但又不是,视图是一个虚拟表。在视图中用户可以使用SELECT语句查询数据，以及使用INSERT、UPDATE和DELETE修改记录。视图的功能可追溯到从MySQL 5.0,视图可以使用户操作方便，而且可以隐藏非用户相关数据,可以保障数据库系统的安全。

可以通过视图修改表数据,,同时如果表中数据有修改也可以反应到视图中;

## 视图的创建与使用

语法格式--
create view 视图名 as sql语句;![在这里插入图片描述](https://img-blog.csdnimg.cn/bb90de0e72ce4979b29e4d8cc777b186.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

> CREATE表示创建新的视图；
> REPLACE表示替换已经创建的视图；
> ALGORITHM表示视图选择的算法；
> view_name为视图的名称，
> column_list为属性列；
> SELECT_statement表示SELECT语句；
> WITH [CASCADED | LOCAL]CHECK OPTION参数表示视图在更新时保证在视图的权限范围之内。

> ALGORITHM的取值有3个，分别是UNDEFINED | MERGE | TEMPTABLE。其中，UNDEFINED表示MySQL将自动选择算法；MERGE表示将使用的视图语句与视图定义合并起来，使得视图定义的某一部分取代语句对应的部分；TEMPTABLE表示将视图的结果存入临时表，然后用临时表来执行语句。

```sql
-- 视图的创建
-- 建立一个职位为clerk的视图;
mysql> create view userinfo as select empno 员工编号,ename 员工姓名 ,job 职位 ,deptno 部门编号 from emp where job='clerk' ;
Query OK, 0 rows affected (0.04 sec)
-- 该表等同于
create view userinfo as select empno 员工编号,ename 员工姓名 ,job 职位 ,deptno 部门编号 from emp where job='clerk' ;
Query OK, 0 rows affected (0.04 sec)

-- 视图的使用

mysql> select * from userinfo;
+----------+----------+-------+----------+
| 员工编号 | 员工姓名 | 职位  | 部门编号 |
+----------+----------+-------+----------+
|     7369 | SMITH    | CLERK |       20 |
|     7789 | Gavin    | CLERK |       20 |
|     7876 | ADAMS    | CLERK |       20 |
|     7900 | JAMES    | CLERK |       30 |
|     7934 | MILLER   | CLERK |       10 |
+----------+----------+-------+----------+
5 rows in set (0.02 sec)

```

向视图中插入信息,删除数据

```sql
mysql> insert into userinfo values(1111,'GGGG','CLERK',10);
Query OK, 1 row affected (0.02 sec)

mysql> select * from userinfo;
+----------+----------+-------+----------+
| 员工编号 | 员工姓名 | 职位  | 部门编号 |
+----------+----------+-------+----------+
|     1111 | GGGG     | CLERK |       10 |
|     7369 | SMITH    | CLERK |       20 |
|     7789 | Gavin    | CLERK |       20 |
|     7876 | ADAMS    | CLERK |       20 |
|     7900 | JAMES    | CLERK |       30 |
|     7934 | MILLER   | CLERK |       10 |
+----------+----------+-------+----------+
6 rows in set (0.00 sec)
-- 查看原表中的数据---

mysql> select * from emp where empno=1111;
+-------+-------+-------+------+----------+------+------+--------+
| EMPNO | ENAME | JOB   | MGR  | HIREDATE | SAL  | COMM | DEPTNO |
+-------+-------+-------+------+----------+------+------+--------+
|  1111 | GGGG  | CLERK | NULL | NULL     | NULL | NULL |     10 |
+-------+-------+-------+------+----------+------+------+--------+
1 row in set (0.00 sec)
-- 这里要注意 视图中的字段
mysql> DELETE FROM USERINFO WHERE 员工编号=1111;
Query OK, 1 row affected (0.01 sec)

```

但是问题来了,如果只想插入工作为clerk的职员信息,即如果不是员工为clerk,那么在原表中也不能插入,那怎么办????
先来看上面没有限制要求的 视图-----能否插入非 clerk 员工

```sql
mysql> insert into userinfo values(3333,'PPPP','SALESMAN',30);
Query OK, 1 row affected (0.01 sec)

mysql> select * from userinfo;
+----------+----------+-------+----------+
| 员工编号 | 员工姓名 | 职位  | 部门编号 |
+----------+----------+-------+----------+
|     1111 | GGGG     | CLERK |       10 |
|     7369 | SMITH    | CLERK |       20 |
|     7789 | Gavin    | CLERK |       20 |
|     7876 | ADAMS    | CLERK |       20 |
|     7900 | JAMES    | CLERK |       30 |
|     7934 | MILLER   | CLERK |       10 |
+----------+----------+-------+----------+
6 rows in set (0.00 sec)

mysql> insert into userinfo values(6666,'PPPP','clerk',30);
Query OK, 1 row affected (0.02 sec)

mysql> select * from userinfo;
+----------+----------+-------+----------+
| 员工编号 | 员工姓名 | 职位  | 部门编号 |
+----------+----------+-------+----------+
|     1111 | GGGG     | CLERK |       10 |
|     6666 | PPPP     | clerk |       30 |
|     7369 | SMITH    | CLERK |       20 |
|     7789 | Gavin    | CLERK |       20 |
|     7876 | ADAMS    | CLERK |       20 |
|     7900 | JAMES    | CLERK |       30 |
|     7934 | MILLER   | CLERK |       10 |
+----------+----------+-------+----------+
7 rows in set (0.00 sec)
```

在mysql的sql语句中对wherez子句后中加入  with check option 来限制只能添加where 限制的字段类型的数据 ,

```sql
-- create or replace 创建或替换
mysql> create or replace view userinfo as select empno 员工编号,ename 员工姓名 ,job 职位 ,deptno 部门编号 from emp where job='clerk' with check option ;
Query OK, 0 rows affected (0.04 sec)
-----插入数据
mysql> insert into userinfo values(7777,'QQQQ','SALESMAN',30);
ERROR 1369 (HY000): CHECK OPTION failed 'gavin.userinfo'
mysql>
mysql> insert into userinfo values(7777,'PPQQQQ','CLERK',30);
Query OK, 1 row affected (0.03 sec)

mysql> select * from userinfo;
+----------+----------+-------+----------+
| 员工编号 | 员工姓名 | 职位  | 部门编号 |
+----------+----------+-------+----------+
|     1111 | GGGG     | CLERK |       10 |
|     6666 | PPPP     | clerk |       30 |
|     7369 | SMITH    | CLERK |       20 |
|     7777 | PPQQQQ   | CLERK |       30 |
|     7789 | Gavin    | CLERK |       20 |
|     7876 | ADAMS    | CLERK |       20 |
|     7900 | JAMES    | CLERK |       30 |
|     7934 | MILLER   | CLERK |       10 |
+----------+----------+-------+----------+
8 rows in set (0.00 sec)
```

## 视图的作用

与直接从数据表中读取相比，视图有以下优点：
1．简单化
使得用户看到的就是他需要的,不需要过多的查询条件,因为实现就已经整好了;
2．安全性
通过视图,用户只能查询和修改他们所能见到的数据。数据库中的其他数据则既看不见也取不到。

3,还可以细化到具体的数据库对象上.
数据库授权命令可以使每个用户对数据库的检索限制到特定的数据库对象上，但不能授权到数据库特定行和特定的列上。

通过视图，用户可以被限制在数据的不同子集上：

1. 使用权限可被限制在基表的行的子集上。
2. 使用权限可被限制在基表的列的子集上。
3. 使用权限可被限制在基表的行和列的子集上。
4. 使用权限可被限制在多个基表的连接所限定的行上。
5. 使用权限可被限制在基表中的数据的统计汇总上。
6. 使用权限可被限制在另一视图的一个子集上，或是一些视图和基表合并后的子集上。

### 在单表上创建视图

```sql
mysql> create or replace view myview as select empno ,ename,job,deptno from emp where deptno =20;
Query OK, 0 rows affected (0.01 sec)

mysql> select * from myview;
+-------+-------+---------+--------+
| empno | ename | job     | deptno |
+-------+-------+---------+--------+
|  7369 | SMITH | CLERK   |     20 |
|  7566 | JONES | MANAGER |     20 |
|  7788 | SCOTT | ANALYST |     20 |
|  7789 | Gavin | CLERK   |     20 |
|  7876 | ADAMS | CLERK   |     20 |
|  7902 | FORD  | ANALYST |     20 |
+-------+-------+---------+--------+
6 rows in set (0.00 sec)
```

### 在多表上创建视图

```sql
mysql> create or replace view myview1 as select e.empno ,e.ename,e.deptno,e1.mgr,e1.ename '领导' from emp e inner join emp e1 on e.empno =e1.mgr ;
Query OK, 0 rows affected (0.01 sec)

mysql> select * from myview1;
+-------+-------+--------+------+--------+
| empno | ename | deptno | mgr  | 领导   |
+-------+-------+--------+------+--------+
|  7902 | FORD  |     20 | 7902 | SMITH  |
|  7698 | BLAKE |     30 | 7698 | ALLEN  |
|  7698 | BLAKE |     30 | 7698 | WARD   |
|  7698 | BLAKE |     30 | 7698 | MARTIN |
|  7566 | JONES |     20 | 7566 | SCOTT  |
|  7698 | BLAKE |     30 | 7698 | TURNER |
|  7788 | SCOTT |     20 | 7788 | ADAMS  |
|  7698 | BLAKE |     30 | 7698 | JAMES  |
|  7566 | JONES |     20 | 7566 | FORD   |
|  7782 | CLARK |     10 | 7782 | MILLER |
+-------+-------+--------+------+--------+
10 rows in set (0.00 sec)
```

### 查看视图

视图可以理解为一张虚拟表,也可以查看该视图的字段信息,创建信息

```sql
mysql> desc myview1;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| empno  | int         | NO   |     | NULL    |       |
| ename  | varchar(10) | YES  |     | NULL    |       |
| deptno | int         | YES  |     | NULL    |       |
| mgr    | int         | YES  |     | NULL    |       |
| 领导   | varchar(10) | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

mysql> show create view myview1\G
*************************** 1. row ***************************
                View: myview1
         Create View: CREATE ALGORITHM=UNDEFINED DEFINER=`gavin`@`%` SQL SECURITY DEFINER VIEW `myview1` AS select `e`.`EMPNO` AS `empno`,`e`.`ENAME` AS `ename`,`e`.`DEPTNO` AS `deptno`,`e1`.`MGR` AS `mgr`,`e1`.`ENAME` AS `领导` from (`emp` `e` join `emp` `e1` on((`e`.`EMPNO` = `e1`.`MGR`)))
character_set_client: gbk
collation_connection: gbk_chinese_ci
1 row in set (0.00 sec)
----  查看视图的状态
mysql> show table status like 'myview1' \G
*************************** 1. row ***************************
           Name: myview1 --  视图名
         Engine: NULL
        Version: NULL
     Row_format: NULL
           Rows: NULL
 Avg_row_length: NULL
    Data_length: NULL
Max_data_length: NULL
   Index_length: NULL
      Data_free: NULL
 Auto_increment: NULL
    Create_time: 2021-09-08 08:39:56
    Update_time: NULL
     Check_time: NULL
      Collation: NULL
       Checksum: NULL
 Create_options: NULL
        Comment: VIEW ---- 说明这是一张视图,
1 row in set (0.00 sec)
// 其他信息都为null说明这是一张虚拟表
-- 再来查看表的状态

mysql> show table status like 'emp'\G
*************************** 1. row ***************************
           Name: emp
         Engine: InnoDB
        Version: 10
     Row_format: Dynamic
           Rows: 21
 Avg_row_length: 780
    Data_length: 16384
Max_data_length: 0
   Index_length: 16384
      Data_free: 0
 Auto_increment: NULL
    Create_time: 2021-09-03 19:13:24
    Update_time: 2021-09-07 21:34:53
     Check_time: NULL
      Collation: utf8mb4_0900_ai_ci
       Checksum: NULL
 Create_options:
        Comment:
1 row in set (0.00 sec)

-- 正常表 会包含很多信息


```

**从查询的结果来看，视图里的信息包含了存储引擎、创建时间等，Comment信息为空，这就是视图和表的区别。**

在元信息表里也可以查看视图的信息

```sql
mysql> select * from information_schema.views  where table_name= 'myview1' \G
*************************** 1. row ***************************
       TABLE_CATALOG: def
        TABLE_SCHEMA: gavin
          TABLE_NAME: myview1
     VIEW_DEFINITION: select `e`.`EMPNO` AS `empno`,`e`.`ENAME` AS `ename`,`e`.`DEPTNO` AS `deptno`,`e1`.`MGR` AS `mgr`,`e1`.`ENAME` AS `领导` from (`gavin`.`emp` `e` join `gavin`.`emp` `e1` on((`e`.`EMPNO` = `e1`.`MGR`)))
        CHECK_OPTION: NONE -- 检查信息
        IS_UPDATABLE: YES   -- 视图权限   可更新
             DEFINER: gavin@%  -- 定义者--gavin
       SECURITY_TYPE: DEFINER   --- 安全类型 --- 定义者所有
CHARACTER_SET_CLIENT: gbk
COLLATION_CONNECTION: gbk_chinese_ci
1 row in set (0.00 sec)
```

### 修改视图字段

上面的代码中已经出现了一种方式------
方式一-----

```sql
--格式--
create or replace view   视图名 as .....
mysql> create or replace view myview as select empno ,ename,job,deptno from emp where deptno =20;
```

方式二-----

```sql
alter view  视图名 as ......

mysql> alter view myview as select * from emp where deptno =10 ;
Query OK, 0 rows affected (0.01 sec)

```

## 更新视图与删除数据

视图中的数据与表中的数据操作方法一致

```sql
-- 查看修改之前的数据
mysql> select * from myview ;
+-------+--------+-----------+------+------------+---------+------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
+-------+--------+-----------+------+------------+---------+------+--------+
|  1111 | GGGG   | CLERK     | NULL | NULL       |    NULL | NULL |     10 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 | NULL |     10 |
|  7893 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 | NULL |     10 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 | NULL |     10 |
+-------+--------+-----------+------+------------+---------+------+--------+
4 rows in set (0.00 sec)
```

**修改和删除数据**

```sql
mysql> select * from myview ;
+-------+--------+-----------+------+------------+---------+------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM | DEPTNO |
+-------+--------+-----------+------+------------+---------+------+--------+
|  1111 | GGGG   | CLERK     | NULL | NULL       |    NULL | NULL |     10 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 | NULL |     10 |
|  7893 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 | NULL |     10 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 | NULL |     10 |
+-------+--------+-----------+------+------------+---------+------+--------+
4 rows in set (0.00 sec)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5474a723e0c43f2a5c6a39262784f51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)对视图myview更新/删除数据后，基本表emp的内容也更新

## 删除视图

```sql
mysql> drop view  myview ;
Query OK, 0 rows affected (0.01 sec)
```

## 视图更新/删除数据失败是什么原因???

当视图中包含有如下内容时，视图的更新操作将不能被执行：
（1）视图中不包含基表中被定义为非空的列。
（2）在定义视图的SELECT语句后的字段列表中使用了数学表达式。
（3）在定义视图的SELECT语句后的字段列表中使用聚合函数。
（4）在定义视图的SELECT语句中使用了DISTINCT、UNION、TOP、GROUP BY或HAVING子句。

```sql
-- 建立视图
mysql> create or replace view myview  as select  empno ,group_concat(ename) ,job ,avg(ifnull(sal,0)+ifnull(comm,0))  from emp group by deptno ;
Query OK, 0 rows affected (0.03 sec)
-- 查看视图
mysql> select * from myview;
+-------+------------------------------------------------------------+----------+-----------------------------------+
| empno | group_concat(ename)                                        | job      | avg(ifnull(sal,0)+ifnull(comm,0)) |
+-------+------------------------------------------------------------+----------+-----------------------------------+
|  1111 | GGGG,KING,MILLER                                           | CLERK    |                       2516.666667 |
|  7369 | SMITH,JONES,SCOTT,Gavin,ADAMS,FORD                         | CLERK    |                       2312.500000 |
|  2222 | PPPP,PPPP,PPPP,ALLEN,WARD,MARTIN,BLAKE,PPQQQQ,TURNER,JAMES | SALESMAN |                       1160.000000 |
|  7367 | NULL                                                       | NULL     |                          0.000000 |
+-------+------------------------------------------------------------+----------+-----------------------------------+
4 rows in set (0.00 sec)
```

**修改数据--**

```sql
ERROR 1146 (42S02): Table 'gavin.view' doesn't exist
mysql> update  myview set job='salesman' where empno=7367;
ERROR 1288 (HY000): The target table myview of the UPDATE is not updatable
mysql> delete from myview where empno=7367;
ERROR 1288 (HY000): The target table myview of the DELETE is not updatable

```

<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/mysql/MySQLBase03.md#数据的基本操作)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/mysql/MySQLBase05.md#mysql中的事务)

</center>

![](..\pic\div\deer.gif)