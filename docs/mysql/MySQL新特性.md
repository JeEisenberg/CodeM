![](..\pic\logo2.png)

# MySQL 8.0的部分新特性

## 原子操作

在java中我们也经常接触到原子性的概念,在mysql8.0中也引入了原子操作的概念---即要么操作成功,要么就回滚

> 在MySQL 8.0版本中，InnoDB表的DDL支持事务完整性，即DDL操作要么成功要么回滚。DDL操作回滚日志写入到data dictionary数据字典表mysql.innodb_ddl_log（该表是隐藏的表，通过show tables无法看到）中，用于回滚操作。通过设置参数，可将DDL操作日志打印输出到MySQL错误日志中。

**案例演示-------**

现在有几张表------
![在这里插入图片描述](https://img-blog.csdnimg.cn/37fd9dba89cb4f1f905ecea3c8c7d867.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)执行删除操作------

```sql
mysql> drop table testnum1,test;
ERROR 1051 (42S02): Unknown table 'gavin.test'
mysql> show tables;
+-----------------+
| Tables_in_gavin |
+-----------------+
| bonus           |
| dept            |
| emp             |
| emp2            |
| salgrade        |
| testnum         |
| testnum1        |
+-----------------+
7 rows in set (0.00 sec)
```

在mysql8.0之前------

```sql
mysql> drop table testnum1,test;
ERROR 1051 (42S02): Unknown table 'gavin.test'
mysql> show tables;
+-----------------+
| Tables_in_gavin |
+-----------------+
| bonus           |
| dept            |
| emp             |
| emp2            |
| salgrade        |
| testnum        |
+-----------------+
7 rows in set (0.00 sec)
```



虽然删除的时候出现了错误但是仍然将testnum1 给删除了;

## 自增变量的持久化

> 在MySQL 8.0之前，自增主键AUTO_INCREMENT的值如果大于max(primary
> key)+1，在MySQL重启后，会重置AUTO_INCREMENT=max(primary
> key)+1，这种现象在某些情况下会导致业务主键冲突或者其他难以发现的问题。

建表来感受以下8.0的新特性

![在这里插入图片描述](https://img-blog.csdnimg.cn/624533f4e1b544bbafb2f6c74b3067ff.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)插入数据------

![在这里插入图片描述](https://img-blog.csdnimg.cn/b9265b6af93f46a1b1747227fe2dc59b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**插入的数据按照升序自增字段升序排列的;**

接下来删除一个字段id=4的记录,然后再插入数据id=4的记录

![在这里插入图片描述](https://img-blog.csdnimg.cn/d6950a1f03c541aa884937ffb28f1486.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
关于自增变量持久化的验证----
删除id=6的字段,然后插入新记录
![在这里插入图片描述](https://img-blog.csdnimg.cn/8ddcd59768b0426ba99870c4fc1dcb57.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这是自增是从7开始的,而不是从6,
虽然删除了id为6的记录，但是再次插入空值时，并没有重用被删除的6，而是分配了7。

> **所以要想插入id=6的记录,只能自己指定;**



接着删除id=7 的字段后,重新启动数据库后再插入数据;
![在这里插入图片描述](https://img-blog.csdnimg.cn/cedd105a2f8f41e8bab135706c78c94f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
对比mysql8.0之前的,会发现重启数据库之后新插入的数据是从第一个缺省位开始的,比如下图-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/a29158cd09a34cc78068b11ce26284db.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)重启后再次插入数据----是从3开始的;

>  **MySQL8.0将自增主键的计数器持久化到重做日志中。每次计数器发生改变，都会将其写入重做日志中。如果数据库重启，InnoDB会根据重做日志中的信息来初始化计数器的内存值。为了尽量减小对系统性能的影响，计数器写入到重做日志时并不会马上刷新数据库系统。**

**自增并非一定从0开始;**

**自增是从第一条记录的的值开始的,即如果第一条记录的值为10,那么下一条记录是从10开始自增的;**

## 窗口函数

> 在MySQL 8.0版本中，新增了一个窗口函数，用它可以实现很多新的查询方式。窗口函数类似于**SUM()、COUNT()**那样的集合函数，但它并不会将多行查询结果合并为一行，而是将结果放回多行当中。也就是说，窗口函数是不需要  **GROUPBY的。**

表数据-----

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

对表中信息进行操作------按照薪资大小从大到小进行排序

```sql
mysql> select empno,ename ,sal,comm ,rank() over w as '薪资排名' from emp window w as ( ORDER BY sal+ifnull(comm,0) desc);
+-------+--------+---------+---------+----------+
| empno | ename  | sal     | comm    | 薪资排名 |
+-------+--------+---------+---------+----------+
|  7839 | KING   | 5000.00 |    NULL |        1 |
|  7788 | SCOTT  | 3000.00 |    NULL |        2 |
|  7902 | FORD   | 3000.00 |    NULL |        2 |
|  7566 | JONES  | 2975.00 |    NULL |        4 |
|  7698 | BLAKE  | 2850.00 |    NULL |        5 |
|  7654 | MARTIN | 1250.00 | 1400.00 |        6 |
|  7782 | CLARK  | 2450.00 |    NULL |        7 |
|  7499 | ALLEN  | 1600.00 |  300.00 |        8 |
|  7521 | WARD   | 1250.00 |  500.00 |        9 |
|  7844 | TURNER | 1500.00 |    0.00 |       10 |
|  7934 | MILLER | 1300.00 |    NULL |       11 |
|  7876 | ADAMS  | 1100.00 |    NULL |       12 |
|  7900 | JAMES  |  950.00 |    NULL |       13 |
|  7369 | SMITH  |  800.00 |    NULL |       14 |
+-------+--------+---------+---------+----------+
14 rows in set (0.00 sec)

```

```sql
--窗口函数的格式---
**select   字段名1,字段名2,....字段名n ,  窗口函数 over (partition by 字段 order by 字段 desc) as ranking from 表名 ;**
-- 或者--- 
select   字段名1,字段名2,....字段名n ,  窗口函数 over  窗口名 as ranking from 表名 window    窗口名 as order by 字段 desc;
 ---- 区别在于 over 中是否有参数,一般习惯上不给窗口命名;
```

根据上表查询每个部门的员工人数占总人数的百分比;

查询思路----就像数学计算一样,
先要统计每个部门人数-----  count(deptno)  group by(deptno)
然后求百分比------count(deptno)/sum(count(deptno)),
其次是按照要求进行显示------为了一看就明白

```sql
mysql> select DISTINCT deptno  as 部门 ,count(deptno) as '部门人数', count(deptno)/sum(count(deptno)) over() as rate from emp group by deptno;
+------+----------+--------+
| 部门 | 部门人数 | rate   |
+------+----------+--------+
|   10 |        3 | 0.2143 |
|   20 |        5 | 0.3571 |
|   30 |        6 | 0.4286 |
+------+----------+--------+
3 rows in set (0.00 sec)

mysql>
```

**窗口函数都有哪些呢?----**

1） 专用窗口函数，rank(), dense_rank, row_number等专用窗口函数。

2） 聚合函数，如sum(). avg(), count(), max(), min()等
窗口函数原则上只能写在select子句中,因为窗口函数是对where或者group by子句处理后的结果进行操作.

**group  by 与partition by 有什么区别-----** 
别着急先看案例再自己总结----

还是上表,我们来看-------
操作一-------按照部门分组,并按照薪资进行排序

> **PARTITION by DEPTNO ORDER BY (sal + ifnull( comm,0))表示按照员工部门进行分组,按照薪资进行排序;**

```sql
SELECT 
   DEPTNO 员工部门,
	empno 员工编号,
	ename 员工姓名,
	sal + ifnull( comm, 0 ) 员工薪资,
	rank ( ) over (PARTITION by DEPTNO ORDER BY (sal + ifnull( comm, 0 ))
	 ) AS 排名 
FROM
	emp;    -> emp;
+----------+----------+----------+----------+------+
| 员工部门 | 员工编号 | 员工姓名 | 员工薪资 | 排名 |
+----------+----------+----------+----------+------+
|       10 |     7934 | MILLER   |  1300.00 |    1 |
|       10 |     7782 | CLARK    |  2450.00 |    2 |
|       10 |     7839 | KING     |  5000.00 |    3 |
|       20 |     7369 | SMITH    |   800.00 |    1 |
|       20 |     7876 | ADAMS    |  1100.00 |    2 |
|       20 |     7566 | JONES    |  2975.00 |    3 |
|       20 |     7788 | SCOTT    |  3000.00 |    4 |
|       20 |     7902 | FORD     |  3000.00 |    4 |
|       30 |     7900 | JAMES    |   950.00 |    1 |
|       30 |     7844 | TURNER   |  1500.00 |    2 |
|       30 |     7521 | WARD     |  1750.00 |    3 |
|       30 |     7499 | ALLEN    |  1900.00 |    4 |
|       30 |     7654 | MARTIN   |  2650.00 |    5 |
|       30 |     7698 | BLAKE    |  2850.00 |    6 |
+----------+----------+----------+----------+------+
14 rows in set (0.00 sec)
```

```sql
SELECT
	DEPTNO 员工部门,
	empno 员工编号,
	ename 员工姓名,
	sal + ifnull( comm, 0 ) 员工薪资 
FROM
	emp 
GROUP BY
	deptno 
ORDER BY
	( sal + ifnull( comm, 0 ) );
+----------+----------+----------+----------+
| 员工部门 | 员工编号 | 员工姓名 | 员工薪资 |
+----------+----------+----------+----------+
|       20 |     7369 | SMITH    |   800.00 |
|       30 |     7499 | ALLEN    |  1900.00 |
|       10 |     7782 | CLARK    |  2450.00 |
+----------+----------+----------+----------+
3 rows in set (0.00 sec)
```

**其他窗口函数-----**
排名函数-----rank, dense_rank, row_number
聚合函数-----sum. avg, count, max, min等
我们来试验一下-----排名函数

```sql
mysql> SELECT deptno,empno,ename ,sal + ifnull ( comm, 0 ),dense_rank ( ) over ( PARTITION BY deptno ORDER BY (sal + ifnull ( comm, 0 )) DESC ) as 排名 FROM emp;
+--------+-------+--------+--------------------------+------+
| deptno | empno | ename  | sal + ifnull ( comm, 0 ) | 排名 |
+--------+-------+--------+--------------------------+------+
|     10 |  7839 | KING   |                  5000.00 |    1 |
|     10 |  7782 | CLARK  |                  2450.00 |    2 |
|     10 |  7934 | MILLER |                  1300.00 |    3 |
|     20 |  7788 | SCOTT  |                  3000.00 |    1 |
|     20 |  7902 | FORD   |                  3000.00 |    1 |
|     20 |  7566 | JONES  |                  2975.00 |    2 |
|     20 |  7876 | ADAMS  |                  1100.00 |    3 |
|     20 |  7369 | SMITH  |                   800.00 |    4 |
|     30 |  7698 | BLAKE  |                  2850.00 |    1 |
|     30 |  7654 | MARTIN |                  2650.00 |    2 |
|     30 |  7499 | ALLEN  |                  1900.00 |    3 |
|     30 |  7521 | WARD   |                  1750.00 |    4 |
|     30 |  7844 | TURNER |                  1500.00 |    5 |
|     30 |  7900 | JAMES  |                   950.00 |    6 |
+--------+-------+--------+--------------------------+------+
14 rows in set (0.00 sec)

mysql> SELECT deptno,empno,ename ,sal + ifnull ( comm, 0 ),rank ( ) over ( PARTITION BY deptno ORDER BY (sal + ifnull ( comm, 0 )) DESC ) as 排名 FROM emp;
+--------+-------+--------+--------------------------+------+
| deptno | empno | ename  | sal + ifnull ( comm, 0 ) | 排名 |
+--------+-------+--------+--------------------------+------+
|     10 |  7839 | KING   |                  5000.00 |    1 |
|     10 |  7782 | CLARK  |                  2450.00 |    2 |
|     10 |  7934 | MILLER |                  1300.00 |    3 |
|     20 |  7788 | SCOTT  |                  3000.00 |    1 |
|     20 |  7902 | FORD   |                  3000.00 |    1 |
|     20 |  7566 | JONES  |                  2975.00 |    3 |
|     20 |  7876 | ADAMS  |                  1100.00 |    4 |
|     20 |  7369 | SMITH  |                   800.00 |    5 |
|     30 |  7698 | BLAKE  |                  2850.00 |    1 |
|     30 |  7654 | MARTIN |                  2650.00 |    2 |
|     30 |  7499 | ALLEN  |                  1900.00 |    3 |
|     30 |  7521 | WARD   |                  1750.00 |    4 |
|     30 |  7844 | TURNER |                  1500.00 |    5 |
|     30 |  7900 | JAMES  |                   950.00 |    6 |
+--------+-------+--------+--------------------------+------+
14 rows in set (0.00 sec)

	
mysql> SELECT deptno,empno,ename ,sal + ifnull ( comm, 0 ),row_number ( ) over ( PARTITION BY deptno ORDER BY (sal + ifnull ( comm, 0 )) DESC ) as 排名 FROM emp;
+--------+-------+--------+--------------------------+------+
| deptno | empno | ename  | sal + ifnull ( comm, 0 ) | 排名 |
+--------+-------+--------+--------------------------+------+
|     10 |  7839 | KING   |                  5000.00 |    1 |
|     10 |  7782 | CLARK  |                  2450.00 |    2 |
|     10 |  7934 | MILLER |                  1300.00 |    3 |
|     20 |  7788 | SCOTT  |                  3000.00 |    1 |
|     20 |  7902 | FORD   |                  3000.00 |    2 |
|     20 |  7566 | JONES  |                  2975.00 |    3 |
|     20 |  7876 | ADAMS  |                  1100.00 |    4 |
|     20 |  7369 | SMITH  |                   800.00 |    5 |
|     30 |  7698 | BLAKE  |                  2850.00 |    1 |
|     30 |  7654 | MARTIN |                  2650.00 |    2 |
|     30 |  7499 | ALLEN  |                  1900.00 |    3 |
|     30 |  7521 | WARD   |                  1750.00 |    4 |
|     30 |  7844 | TURNER |                  1500.00 |    5 |
|     30 |  7900 | JAMES  |                   950.00 |    6 |
+--------+-------+--------+--------------------------+------+
14 rows in set (0.00 sec)



```

可以发现区别在于排序如果遇到重复值的时候处理方式不一样,

**rank()-----排名并列且一致,下一名在顺延(+1)的基础上排名要加上并列的记录数  如1,1,1,4

row_numbe()----不考虑并列,下一名顺延(+1),如 1,2,3,4

dense_rank()----排名并列且一致,下一名顺延(+1),如1,1,1,2**



再来试验一下聚合函数,以avg 为例子------
**各部门的平均工资,在求总的平均薪资**

```sql
SELECT DISTINCT
	( deptno ) AS 部门编号,
	ename 员工姓名,
	job 员工职位,
	sal + ifnull( comm, 0 ) AS 薪资,
	avg( sal + ifnull( comm, 0 ) ) over ( PARTITION BY deptno ORDER BY sal + ifnull( comm, 0 ) ) as 平均薪资
FROM
	emp;
	+----------+----------+-----------+---------+-------------+
| 部门编号 | 员工姓名 | 员工职位  | 薪资    | 平均薪资    |
+----------+----------+-----------+---------+-------------+
|       10 | MILLER   | CLERK     | 1300.00 | 1300.000000 |
|       10 | CLARK    | MANAGER   | 2450.00 | 1875.000000 |
|       10 | KING     | PRESIDENT | 5000.00 | 2916.666667 |
|       20 | SMITH    | CLERK     |  800.00 |  800.000000 |
|       20 | ADAMS    | CLERK     | 1100.00 |  950.000000 |
|       20 | JONES    | MANAGER   | 2975.00 | 1625.000000 |
|       20 | SCOTT    | ANALYST   | 3000.00 | 2175.000000 |
|       20 | FORD     | ANALYST   | 3000.00 | 2175.000000 |
|       30 | JAMES    | CLERK     |  950.00 |  950.000000 |
|       30 | TURNER   | SALESMAN  | 1500.00 | 1225.000000 |
|       30 | WARD     | SALESMAN  | 1750.00 | 1400.000000 |
|       30 | ALLEN    | SALESMAN  | 1900.00 | 1525.000000 |
|       30 | MARTIN   | SALESMAN  | 2650.00 | 1750.000000 |
|       30 | BLAKE    | MANAGER   | 2850.00 | 1933.333333 |
+----------+----------+-----------+---------+-------------+
14 rows in set (0.00 sec)
 	

```

> -----从该表中可以发现 avg ()函数的求平均的方式 第二个的值为(第一个+第二个)/2,第三个为(第一个+第二个+第三个)/3,以此类推,所以如果想要各部门平均值只需要看各部门最后一条记录-----
> 同理其他聚合函数也是如此;



**窗口函数的应用-----**
主要解决----排名问题,
topN问题,用limit 来筛选出前十名,
rank函数排名如果遇到一样的则保持排名一致,
如   -------
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a685024d25147c2850c08382e72d09f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

定了字段的数据类型之后，也就决定了向字段插入的数据内容;**

想要发现Mysql8全部新特性----->>[mysql8新特性](https://download.csdn.net/download/weixin_54061333/21633252)