![](..\pic\div\shark.gif)

# JAVA实现JDBC

## 入门



了解历史----
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed707b3f425b41ce93d27cee3e9a0df5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 在以前如果想要java连结数据库需要编写不同的驱动(不同的连接地址)来完成对各自不同的数据库的连接,非常麻烦,于是考虑到java语言的快速发展以及连接数据库的方便性,各家数据库厂商找java语言开发人员商量如何用java快速连结自家数据库,
> 这一商量-----java说都要符合我制定的标准 jdbc 于是就有了java现系统中的数据库连结方式;

说了这么多,还是直接上图来的直观,在以前的老版本教程中
  连接数据库的方式需要需要配置文件,
  jdbc----- 数据库驱动厂商单独提供,
  另连接地址格式也不统一
如老教程中-----

```java
  public class ConnectJDBC{
         //驱动程序就是之前在classpath中配置的jdbc的驱动程序的jar包中
          public static final String DBDRIVER="oracle.jdbc.driver.OracleDriver";
        //连接地址是由各个数据库生产商单独提供的，所以需要单独记住
         public static final String DBURL="jdbc:oracle:thin:@localhost:1521:orcl";
```

  现在就比较简便了;
连接jdbc

```java
import java.sql.*;

public class test001 {
    public static void main(String[] args) throws SQLException {
        //加载依赖
        Driver driver = new com.mysql.cj.jdbc.Driver();
        //注册驱动
        DriverManager.registerDriver(driver);
        //得到连接----连接参数---连接地址,用户名和密码
        final String URL = "jdbc:mysql://localhost:3306/gavin";
        final String USER = "gavin";
        final String PASSWORD = "gavin";
        Connection connection = DriverManager.getConnection(URL, USER, PASSWORD);
        //创建描述以用来操作数据库
        Statement statement = connection.createStatement();
        //接着操作数据库--要准备sql语句
        String sql = "insert into test values(1,'张三',18);";
        //执行该语句
        int rows = statement.executeUpdate(sql);
        System.out.println("插入了===" + rows + "行数据");
        //释放资源
        statement.close();
        connection.close();
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/95a27f3276f14dd096f2750b33c6114c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
在操作过程中可能会遇到连接失败的情况,可能的原因是数据库URL的错误

```java
 /* * url:统一资源定位符 定位我们要连接的数据库的
        *   1协议         jdbc:mysql
        *   2IP          127.0.0.1/localhost
        *   3端口号       3306
        *   4数据库名字   mydb
        *   5参数
        *   协议://ip:端口/资源路径?参数名=参数值&参数名=参数值&....
        *   jdbc:mysql://127.0.0.1:3306/mydb
        * */
        String url="jdbc:mysql://127.0.0.1:3306/mydb?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai";
```

useSSL----表示是否用ssl加密传输=====false一般不选择加密

useUnicode=======表示用Unicode编码格式-------true

characterEncoding=====表示字符编码格式-----用UTF-8
serverTimeZone======表示时区-------国内用Asia/Shanghai

查看Driver的源码

![在这里插入图片描述](https://img-blog.csdnimg.cn/9c229c3ec8094ecf99d4c2a58c2cba55.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以发现只要Driver这个类加载到内存中静态代码块就帮助我们完成了驱动的注册;所以上述加载驱动的时候完全没有必要,只需要通过反射将该类加载到内存就可以了;

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class test002 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        Class aClass = Class.forName("com.mysql.cj.jdbc.Driver");//加载完毕接着就注册了
        final String URL = "jdbc:mysql://localhost:3306/gavin";
        final String USER = "gavin";
        final String PASSWORD = "gavin";
        Connection connection= DriverManager.getConnection(URL,USER,PASSWORD);
        String sql="update test set name='Gavin' where id=1 ;";
        Statement statement = connection.createStatement();
        int rows = statement.executeUpdate(sql);
        System.out.println("影响了----"+rows+"条数据");

        statement.close();
        connection.close();
    }
}

```

## JDBC批处理

JDBC MySql PreparedStatement executeBatch过慢的问题解决-

话不多说,先看源码-----

```sql
package com.test.curd;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestBatch {
    private static String driver ="com.mysql.cj.jdbc.Driver";
    private static String url="jdbc:mysql://127.0.0.1:3306/gavin?useUnicode=true&rewriteBatchedStatements=true&useServerPrepStmts=true";;
    private static String user="gavin";
    private static String password="955945";
    static long start =0;
    static long end =0;
    public static void main(String[] args) {
        testAddBatch();
    }
    // 定义一个方法,向部门表增加1000条数据
    public static void testAddBatch(){
        Connection connection = null;
        PreparedStatement preparedStatement=null;
        try{
            Class.forName(driver);
            connection = DriverManager.getConnection(url, user,password);
            String sql="insert into accounting1 values (DEFAULT,?,?,?,now());";
            preparedStatement = connection.prepareStatement(sql);//这里已经传入SQL语句
            //设置参数
          start = System.currentTimeMillis();
            for (int i = 1; i <= 10666; i++) {
                preparedStatement.setString(1, "Mary");
                preparedStatement.setString(2, "123456");
                preparedStatement.setDouble(3, 200.00d);
                preparedStatement.addBatch();// 将修改放入一个批次中
                if(i%1000==0){
                    preparedStatement.executeBatch();//1000条一提交
                    preparedStatement.clearBatch();// 清除批处理中的数据
                }
            }
            /*
             * 整数数组中的元素代表执行的结果代号
             * SUCCESS_NO_INFO -2
             * EXECUTE_FAILED  -3
             * */
            /*int[] ints = */
            preparedStatement.executeBatch();
            preparedStatement.clearBatch();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            end=System.currentTimeMillis();

            System.out.println(end-start);
        }
    }
}

```

执行时间------
![在这里插入图片描述](https://img-blog.csdnimg.cn/476b22a1dce445499ff0f85fce5991bd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以看到虽然开启了预编译和缓存,但是执行时间仍然很慢,原因就在于自动提交,看一下API中的说法-----

Newlycreated Connection objects are in auto-commit mode by default, which means thatindividual SQL statements are committed automatically when the statement iscompleted. To be able to group SQL statements intotransactions and commit them or roll them back as a unit, auto-commit must bedisabled by calling the method setAu ommit with false as its argument. Whenauto-commit is disabled, the user must call either the commit or rollbackmethod explicitly to end a transaction

> 翻译过来就是如果自动提交为true,则所有的sql都当作单个事务进行处理提交,如果设置为 false则视为事务一块提交(commit()或者回滚rollback(),所以第一种方式虽然设置了 缓存,预编译,但是还有自动提交没有设置;

```java
package com.test.curd;

import java.sql.*;

public class AdTest {

    //准备数据库连接的条件

    final static String URL = "jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&cachePrepStmts=true&reWriteBatchedStatements=true&useServerPrepStmts=true";
    final static String USER = "gavin";
    final static String PASSWORD = "955945";
    static long start =0;
    static long end =0;
    public static void main(String[] args) {
        addTest();
    }


    public static void addTest() {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            //Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection(URL, USER, PASSWORD);
            String sql = "insert into accounting values (DEFAULT,?,?,?,now());";
            start=System.currentTimeMillis();

            preparedStatement = connection.prepareStatement(sql);
            connection.setAu ommit(false);
            for (int i = 1; i <=10666; i++) {

                preparedStatement.setString(1, "Mary");
                preparedStatement.setString(2, "123456");
                preparedStatement.setDouble(3, 200.00d);

                //将上述语句加入到批处理
                preparedStatement.addBatch();
                if (i % 1000 == 0) {
                    connection.commit();
                    preparedStatement.executeBatch();

                    preparedStatement.clearBatch();
                }

            }
            preparedStatement.executeBatch();
            preparedStatement.clearBatch();
end=System.currentTimeMillis();
            System.out.println(end-start);
        } catch (Exception e) {
            e.printStackTrace();
           try {
                connection.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        } finally {
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
        }

    }

    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/875c980dfb084e2f880ff4b97286139b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)注意一点---------
一定要记住处理完之后要**提交**或者遇到异常要回滚！
如果不提交那么仅设置自动提交为true也是达不到效果的;

## JDBC事务处理

```java
package com.test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TransactionTest {
    //准备数据库连接的条件
    final static String URL = "jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&cachePrepStmts=true&reWriteBatchedStatements=true&useServerPrepStmts=true";
    final static String USER = "gavin";
    final static String PASSWORD = "955945";
    static long start = 0;
    static long end = 0;

    public static void main(String[] args) {
TransactionTest();
    }


    public static void TransactionTest() {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection(URL, USER, PASSWORD);
            String sql = "update acount set balance=balance+? where id=?;";

            start = System.currentTimeMillis();
//预编译
            preparedStatement = connection.prepareStatement(sql);
            //先不设置事务
            connection.setAu ommit(true);

            preparedStatement.setInt(1, 500);
            preparedStatement.setInt(2, 2);
            int rows = preparedStatement.executeUpdate();
if(rows==1){
    System.out.println("mary已经转账了");
}else{
    System.out.println("mary转账失败");
}

            int i=100/0; //制造异常

            preparedStatement.setInt(1, -500);
            preparedStatement.setInt(2, 2);
            int row1 = preparedStatement.executeUpdate();


            end = System.currentTimeMillis();
            System.out.println(end - start);
            if(row1==1){
                System.out.println("lili已经收到账了");
            }else{
                System.out.println("转账失败");
            }


        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("有异常发生了");
           /*try {
                connection.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }*/
        } finally {
          /*  try {
                connection.commit();
            } catch (SQLException e) {
                e.printStackTrace();
            }*/
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/3ab59bfd91fa4456b7ed6bdc99603d78.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)数据库结果----

![在这里插入图片描述](https://img-blog.csdnimg.cn/cfb341b060694f898d9a077177097044.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)由于没有开启事务异常也没有回滚处理,mary的balance并没有减少,但是lili的却增加了;



```java
package com.test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TransactionTest {
    //准备数据库连接的条件
    final static String URL = "jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&cachePrepStmts=true&reWriteBatchedStatements=true&useServerPrepStmts=true";
    final static String USER = "gavin";
    final static String PASSWORD = "955945";
    static long start = 0;
    static long end = 0;
   static  int rows=0;
   static  int row=0;
    public static void main(String[] args) {
TransactionTest();
    }


    public static void TransactionTest() {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection(URL, USER, PASSWORD);
            String sql = "update acount set balance=balance+? where id=?;";

            start = System.currentTimeMillis();
//预编译
            preparedStatement = connection.prepareStatement(sql);
            //先不设置事务
            connection.setAu ommit(false);

            preparedStatement.setInt(1, 500);
            preparedStatement.setInt(2, 2);
            rows = preparedStatement.executeUpdate();


            int i=100/0; //制造异常

            preparedStatement.setInt(1, -500);
            preparedStatement.setInt(2, 2);
            row = preparedStatement.executeUpdate();


            end = System.currentTimeMillis();
            System.out.println(end - start);


        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("有异常发生了");
           try {
                connection.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        } finally {
            try {
                connection.commit();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            if(rows==1&&row==1){
                System.out.println("转账成功");
            }else{
                System.out.println("转账失败");
            }
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0ac99744d38843b1be696f75976ca1a5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/3bf516d62d034d698c219a0db0d78d35.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这就是日常转账时候的操作,答题步骤是---

> **连接数据库, 
> 设置为不自动提交即 connection.setAu ommit(false); 
> 向数据库发送sql, 
> 异常处理处执行事务回滚
>  finnaly处选择执行;**

但是回滚的话,上面的是全部回滚的,如果发生异常数据并不是全部要回滚,比如插入数据的时候,
下面的一个案例就是这样,向表中插入10666条数据,在10005条数据的时候出现了异常,那么之前插入的数据如果全部撤回,不太合理,那怎么办呢?

设置回滚点---- 源码中很清楚了,不在赘述;

```sql
package com.test.curd;

import java.sql.*;
import java.util.LinkedList;

public class TestBatch {
    private static String driver ="com.mysql.cj.jdbc.Driver";
    final static String URL = "jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&cachePrepStmts=true&reWriteBatchedStatements=true&useServerPrepStmts=true";
    final static String USER = "gavin";
    final static String PASSWORD = "955945";
    static long start =0;
    static long end =0;
    static LinkedList<Savepoint>savepoints= new LinkedList<>();
    public static void main(String[] args) {
        testAddBatch();
    }
    // 定义一个方法,向部门表增加1000条数据
    public static void testAddBatch(){
        Connection connection = null;
        PreparedStatement preparedStatement=null;
        try{
            Class.forName(driver);
            connection = DriverManager.getConnection(URL, USER,PASSWORD);
            String sql = "insert into accounting1 values (DEFAULT,?,?,?,now());";
            preparedStatement = connection.prepareStatement(sql);//这里已经传入SQL语句
            //设置事务不自动提交
            connection.setAu ommit(false);

          start = System.currentTimeMillis();
            for (int i = 1; i <= 10666; i++) {
                preparedStatement.setString(1, "Mary");
                preparedStatement.setString(2, "123456");
                preparedStatement.setDouble(3, 200.00d);
                preparedStatement.addBatch();// 将修改放入一个批次中
                if(i%1000==0){
                    preparedStatement.executeBatch();//1000条一提交
                    //设置回滚点
                    Savepoint savepoint = connection.setSavepoint();
                    //将该混滚点加入到最 后一个节点
                    savepoints.addLast(savepoint);
                    preparedStatement.clearBatch();// 清除批处理中的数据

                }
                //制造异常以用于回滚
                if(i==10005){
                    throw  new RuntimeException();
                }
            }

            preparedStatement.executeBatch();
            preparedStatement.clearBatch();
            end=System.currentTimeMillis();
        }catch (Exception e){//捕捉到异常
            e.printStackTrace();
            //得到回滚点
            Savepoint last = savepoints.getLast();
            try {
                //在该回滚点处
                connection.rollback(last);
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        }finally {
            if(null!=connection) {
                try {
                    connection.commit();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(null!=preparedStatement){
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            System.out.println(end-start);
        }
    }
}

```

## JDBC连接池

我们在谈论**"**池**"什么时候是什么时间?我记得上一次是IO流的时候,建立缓冲池,加快读取速度/写入速度(实际是减少IO 操作次数)

那么什么时候会用到连接池呢???

> **连接池是创建和管理一个连接的缓冲池的技术，这些连接准备好被任何需要它们的线程使用。**

这就要从java工具类来讲,java功能工具类为我们的开发提供了便利,同理连接池正式基于这种理念进行设计的;

**对于大多数客户端请求，进行数据库的相关事务时，仅需要能够访问JDBC连接的 1 个线程。当客户端不需要处理事务时，那么这个连接就会闲置。
这样如果有很多客户端来请求,那么闲置的连接能不能被再次使用呢?
答案是可以的,这样就减少了资源的消耗(当客户端请求时,用已经闲置的连接就可以了),加快了客户端请求的速度;**

> **使用连接池技术能够降低资源消耗,使得cpu能过够更多的用于别的事务;**

下面来模拟一个连接缓冲池

**创建连接池**

**连接池的大体描述------**

假设这样一个场景,一所学校的选课系统,平时选课系统的并发量很少,只有学期开始选课的时候,并发量才上去了;
我记得当时我们学校(**别问哪个学校,反正不是你学校**)的限制是同时500人可以进行选课;一个人释放该链接的时候下一个人才能连接到选课系统;

下面开始干活-------

```java
public class MyConnection {
    //准备连接用的
    final static String URL = "jdbc:mysql://127.0.0.1:3306/gavin?";
    final static String USER = "gavin";
    final static String PASSWORD = "root";
    //准备一个池子用于盛放连接
    private static LinkedList<Connection> pool;
    //初始化大小
    private static int initSize = 5;
    //限制池子的最大大小
    private static int maxSize = 10;

    //加载该连接池类信息时就初始化了pool,pool中存放了5个连接实例
    static {
        //加载驱动
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //开辟空间用于初始化连接池
       pool = new LinkedList<Connection>();
       //往池子中添加5个连接实例
       for (int i = 0; i < initSize; i++) {
       Connection connection = initConnection();
        pool.addLast(connection);
            }
        }
        //初始化一个connection;
public static Connection initConnection(){
Connection connection = null;
    try {
        connection=DriverManager.getConnection(URL, USER, PASSWORD);
    } catch (SQLException e) {
        e.printStackTrace();
        System.out.println("异常");
    }
return connection;

}
    //用于获得连接的方法
    public static Connection getConnection() {
        //怎么获得呢?
        //当连接池中有连接的时候,直接从里面取用
        Connection connection;
        if (pool.size() != 0) {
            connection = pool.removeFirst();
            System.out.println("连接池中--"+connection.hashCode()+"被取走了");

        } else {//当连接池中没有的时候,创建一个
             connection = initConnection();
            System.out.println("连接池已空--"+connection.hashCode()+"被创建了");

        }
        return connection;
    }

    //用户还回去的方法
    public static void returnConnection(Connection connection){

//什么时候往回还,要求池子内少于五个的时候我才接收
        //检测
        if(null!=connection) {//不为空

                try {//检测
                    if (!connection.isClosed()) {//连接未关闭的话
                        //检测
                        //如果池子中少于10个则可以添加进去--始终保证最大化缓冲池
                        if (pool.size() <= maxSize) {
                            //向池中增加该连接
                            connection.setAu ommit(true);
                            pool.addLast(connection);//
                            System.out.println("归还的连接"+connection.hashCode()+"符合要求,归还成功");
                        }else{//如果归还后池子中连接数大于10,则将该链接关闭
                            System.out.println("池子中已满"+connection.hashCode()+"--将被关闭");
                            connection.close();

                        }
                    }else{//连接关闭
                        System.out.println("归还的连接不能使用,归还失败");
                    }
                } catch (SQLException e) {
                    e.printStackTrace();
                }

        }else{
            System.out.println("归还失败");
        }

    }

    public static void main(String[] args) {
        Connection connection = getConnection();
        Connection connection1 = getConnection();
        try {
            connection1.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        getConnection();
        getConnection();
        getConnection();
        getConnection();
        returnConnection(connection);
        returnConnection(null);
        returnConnection(connection1);
    }
}

```

测试结果如下-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/800dc51d8d0c492bad23d9c418e22382.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
要考虑连接池的健壮性,同时要考虑服务器的承压情况,灵活调整连接池中连接的数量;

**连接池的优化**

关于连接池的优化,当前代码有一个小小的问题,当我们要设置池的大小的之时,或者修改要连接的数据库时,我们就要停止代码的运行,然后修改,那么为了优化这一点就产生了相应的优化方案----

将这些数据单独妨碍一个配置文件中进行存放,如果哪一天要修改,在配置文件中修改就可以了;
关于配置文件的读取,Java中有一个专门的类用于读取配置文件,在util包下-----Properties
先来看一下该类---

**Properties类**

**public class Properties extends Hashtable<Object,​Object>**
是Hashtable 的子类,以键值对的方式存储数据;

> **该类表示一组恒久的属性。可以保存到流或从流加载。属性列表中的每个密钥及其相应值均为字符串。**

***此类是线程安全的：多个线程可以共享单个对象，而无需外部同步。Properties***

**构造方法**

**Properties()	
创建没有默认值的空属性列表。**

**Properties​(int initialCapacity)	
创建没有默认值的空属性列表，初始大小可容纳指定数量的元素，无需动态调整大小。**

**Properties​(Properties defaults)	
使用指定的默认值创建空属性列表。**

**常用方法**

![在这里插入图片描述](https://img-blog.csdnimg.cn/ffc8c484943e41e5b21bcb94f336c1e0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

可以试着建立一个jdbc.properties文件,

![在这里插入图片描述](https://img-blog.csdnimg.cn/e135849f383a40cab35330b37e2b1a5f.png)

> 文件内容------

![在这里插入图片描述](https://img-blog.csdnimg.cn/23fd045c7f1f40f9b5567b1e840f5913.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 文件内容格式----## 表示注释 
> **文件内内容均默认为文本格式, 具体化格式是 key=value**

建一个测试类-----

```java
package com.gavin.dao;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class TestProperties {
    public static void main(String[] args) {
        Properties properties= new Properties();//实力话配置实例
        /**
         * 怎么导入呢?
         * 获取配置文件位置,怎么或获取呢?
         * 通过当前类的字节码文件来获得一个输入流然后通过输入流将配置文件给加载进来
         * 注意文件路径的格式----    /相对路径(最好是相对路径)
         */
        InputStream resourceAsStream = TestProperties.class.getResourceAsStream("/jdbc.properties");
        try {
            properties.load(resourceAsStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
        String user = properties.getProperty("USER");
        System.out.println(user);
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/cbd87e94d7b54e9bb00da2b7e8fba2a7.png)

**配置文件读取时问题**

 **1. 获取的结果为null**
  原因-----代码中要获取的key与配置中的key名称不配写不匹配(大小写),不要有不该有的空格
![在这里插入图片描述](https://img-blog.csdnimg.cn/eec0fa754d6648acb65c023be8b53526.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

2,**Exception in thread "main" java.lang.NullPointerException: inStream parameter is null**
说明配置文件输入流为空-----
可能是配置文件名或者地址写错了
![在这里插入图片描述](https://img-blog.csdnimg.cn/d27883b620be4af0a49d6131c4d2c3d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**连接池的具体优化---**

```java
package com.gavin.dao;

import com.gavin.UtilTool;


import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.LinkedList;


public class MyConnection {

    //准备连接用的
    private static String DRIVER ;
   private static String URL ;
    private static String USER;
    private static String PASSWORD ;
    //准备一个池子用于盛放连接
    private static LinkedList<Connection> pool;
    //初始化大小
    private static int initSize ;
    //限制池子的最大大小
    private static int maxSize ;
    //加载该连接池类信息时就初始化了pool,pool中存放了5个连接实例
    static {
     UtilTool utool= new UtilTool("/jdbc.properties");
       DRIVER = utool.getProperties("DRIVER");
       URL = utool.getProperties("URL");
        USER = utool.getProperties("USER");
       PASSWORD= utool.getProperties("PASSWORD");
       initSize=Integer.parseInt(utool.getProperties("iniSize"));
        maxSize=Integer.parseInt(utool.getProperties("maxSize"));
        //加载驱动
        try {

            Class.forName(DRIVER);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //开辟空间用于初始化连接池
       pool = new LinkedList<Connection>();
       //往池子中添加5个连接实例
       for (int i = 0; i < initSize; i++) {
       Connection connection = initConnection();
        pool.addLast(connection);
            }
        }
        //初始化一个connection;
public static Connection initConnection(){
Connection connection = null;
    try {
        connection=DriverManager.getConnection(URL, USER, PASSWORD);
    } catch (SQLException e) {
        e.printStackTrace();
        System.out.println("异常");
    }
return connection;

}
    //用于获得连接的方法
    public static Connection getConnection() {
        //怎么获得呢?
        //当连接池中有连接的时候,直接从里面取用
        Connection connection;
        if (pool.size() != 0) {
            connection = pool.removeFirst();
            System.out.println("连接池中--"+connection.hashCode()+"被取走了");

        } else {//当连接池中没有的时候,创建一个
             connection = initConnection();
            System.out.println("连接池已空--"+connection.hashCode()+"被创建了");

        }
        return connection;
    }

    //用户还回去的方法
    public static void returnConnection(Connection connection){

//什么时候往回还,要求池子内少于五个的时候我才接收
        //检测
        if(null!=connection) {//不为空

                try {//检测
                    if (!connection.isClosed()) {//连接未关闭的话
                        //检测
                        //如果池子中少于10个则可以添加进去--始终保证最大化缓冲池
                        if (pool.size() <= maxSize) {
                            //向池中增加该连接
                            connection.setAu ommit(true);
                            pool.addLast(connection);//
                       System.out.println("归还的连接"+connection.hashCode()+"符合要求,归还成功");
                        }else{//如果归还后池子中连接数大于10,则将该链接关闭
                            System.out.println("池子中已满"+connection.hashCode()+"--将被关闭");
                            connection.close();

                        }
                    }else{//连接关闭
                        System.out.println("归还的连接不能使用,归还失败");
                    }
                } catch (SQLException e) {
                    e.printStackTrace();
                }

        }else{
            System.out.println("归还失败");
        }

    }

    public static void main(String[] args) {
        Connection connection = getConnection();
        Connection connection1 = getConnection();
        try {
            connection1.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        getConnection();
        getConnection();
        getConnection();
        getConnection();
        returnConnection(connection);
        returnConnection(null);
        returnConnection(connection1);
    }
}

```

查看,功能正常--说明配置文件成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/fcb114b0f6ec44dcb4b3bb21cd4ccf78.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
同时我们还可以查看配置文件和代码中相关参数的颜色

![在这里插入图片描述](https://img-blog.csdnimg.cn/5a81eafe0a834e0d87c2992835e085b2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## JDBC日志

先看图,代码没问题,没有报错(我用的是官网下载的最新log4j版本,jdk是16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9651755a74374878a12bb01b9a2f53f4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
运行后提示
java.lang.NoClassDefFoundError: org/apache/logging/log4j/Logger

错误原因找不到类的原因,在lib中添加log4j核心包就可以了,然后右键add as a library就可以了
![在这里插入图片描述](https://img-blog.csdnimg.cn/45a1baab24504311b9e2f3a6834f4c7d.png)

```java
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
D:/Milo/msb.txt
07:46:02.370 [main] FATAL com.gavin.Test01 - fatal
07:46:02.372 [main] ERROR com.gavin.Test01 - error`

```

还有一个问题,是官网下载的log4j配置结束后运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/b246f0970d6d47d28769eff9e39e39ee.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这可能是时配置文件和当前版本不太兼容的问题

换个Apache Log4j1.2jar的包

![在这里插入图片描述](https://img-blog.csdnimg.cn/2d27c5ddba0f4fe2964765810a45ffcb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
之所以会这样,也是现在maven流行的原因,以前的一些配置思路会有些变化;

后续maven配置会及时更新....



**日志文件的配置**

先来个配置文件----

```java
log4j.rootLogger=debug,stdout,logfile
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=d:/msb.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd   HH:mm:ss} %l %F %p %m%n
```

日志内容之外的要求----
**1,log4j的配置文件格式为 **.properties**,必须命名为
log4j.properties不然会读取配置文件失败;**

**2,标准格式时键值对来保存配置内容----**key=value**;**

**3,习惯将log4j.properties文件放在根目录下,因为默认情况下， 查找 日志配置文件是在CLASSPATH中查找名为log4j.properties的文件。**

**日志内容要求------**
**log4j.rootLogger**----->表示日志记录的级别以及记录方式

log4j.rootLogger=debug,stdout,logfile----->日志记录级别为debug,输出方式为两种stdout,logfile(两种方式需要自己指定;)

**日志记录级别----的优先级**

1. fatal:出现非常严重的错误事件,这些事件可能导致程序异常终止
2. error:虽有错误,但允许应用程序继续运行     
3. warn:运行环境潜藏着危害  
4. info:报告信息   
5. debug:细粒度的信息事件,对应于程序的调试;

每种级别包含他之上的级别;
即输出级别error包含fatal, debug包含上述所有级别

**log4j.appender.stdout =-------表示输出方式**

log4j.appender.stdout=org.apache.log4j.ConsoleAppender  ------->在控制台输出
log4j.appender.stdout.Target=System.err---->>>表示输出信息为系统error
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout----->>表示输出格式为简单的格式



**同理可以理解----**
log4j.appender.logfile=org.apache.log4j.FileAppender---->>输出位置在日志文件中
log4j.appender.logfile.File=d:/msb.log  ----->>指定日志文件位置
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout---->>>输出格式-----指定格式
log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd   HH:mm:ss} %l %F %p %m%n   ----->>>定义指定的格式

**log4j.properties语法:**

```xml
##Define the root logger with appender X
log4j.rootLogger = DEBUG, X
##Set the appender named X to be a File appender
log4j.appender.X=org.apache.log4j.FileAppender
##Define the layout for X appender
log4j.appender.X.layout=org.apache.log4j.PatternLayout
log4j.appender.X.layout.conversionPattern=%m%n
```

**log4j.appender.X.中只要保证X一致即可,并非一定要使用 stdout和logfile  不过见名知意是最好的;**

不过日常使用中一般不自己写而是直接到官网找到相关配置文件复制粘贴,然后改改就可以自己用了;
**将log4j.properties配置文件的输出记录到控制台。**

```xml
log4j.rootLogger=debug,stdout,logfile
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
```

**将log4j.properties配置文件的输出记录到日志文件。**

```xml
log4j.rootLogger=debug,logfile
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=d:/msb.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd   HH:mm:ss} %l %F %p %m%n
```

也可以同时记录,不过一般是按需所求,用户不需要看到某些错误信息,只给用户该看的就可以了;

再接触到xml之后,也可以用xml取完成配置文件-----

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true"
  xmlns:log4j="http://jakarta.apache.org/log4j/">
  <appender name="console" class="org.apache.log4j.ConsoleAppender">
      <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" 
      value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
      </layout>
  </appender>
  <root>
    <level value="DEBUG" />
    <appender-ref ref="console" />
  </root>
</log4j:configuration>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true"
  xmlns:log4j="http://jakarta.apache.org/log4j/">
 
  <appender name="file" class="org.apache.log4j.RollingFileAppender">
     <param name="append" value="false" />
     <param name="maxFileSize" value="10KB" />
     <param name="maxBackupIndex" value="5" />
     <!-- For Tomcat -->
     <param name="file" value="${catalina.home}/logs/my.log" />
     <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" 
      value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
     </layout>
  </appender>
 
  <root>
    <level value="ERROR" />
    <appender-ref ref="file" />
  </root>
 
</log4j:configuration>
```

最后整合一个标准的

```xml
# Root logger option
log4j.rootLogger=INFO, file, stdout
 
# Direct log messages to a log file
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=C:\\my.log
log4j.appender.file.MaxFileSize=10MB
log4j.appender.file.MaxBackupIndex=10
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
 
# Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true"
  xmlns:log4j="http://jakarta.apache.org/log4j/">
 
  <appender name="console" class="org.apache.log4j.ConsoleAppender">
      <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" 
      value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
      </layout>
  </appender>
 
  <appender name="file" class="org.apache.log4j.RollingFileAppender">
      <param name="append" value="false" />
      <param name="maxFileSize" value="10MB" />
      <param name="maxBackupIndex" value="10" />
      <param name="file" value="${catalina.home}/logs/my.log" />
      <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" 
      value="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n" />
      </layout>
  </appender>
 
  <root>
    <level value="DEBUG" />
    <appender-ref ref="console" />
    <appender-ref ref="file" />
  </root>
 
</log4j:configuration>
```

**日志记录的方式------**

```java
package com.gavin;
import org.apache.log4j.Logger;
import java.io.IOException;
public class Test01 {
    public static void main(String[] args) throws IOException {
        //Logger logger = Logger.getLogger("com.gavin.Test01");//类的全路径名
        Logger logger = Logger.getLogger(Test01.class);
    
       logger.fatal("fatal");
        logger.error("error");
        logger.warn("warn");
        logger.info("info");
        //logger.debug("debug");
        try {
            int result = 1 / 0;
        } catch (Exception e) {
            logger.error("程序运算错误", e);
        }
    }
}

```

注-----以上内容仅供学习参考使用;

# 数据库常见错误原因总结



1. Access denied for user 'gavin'@'localhost' (using password: YES)------用户名/密码输入错误
2. Communications link failure-----数据库连接错误,可能是URL配置错误;
3. Exception in thread "main" java.lang.ClassNotFoundException: com.mysql.jdbc2.Driver
   -----Driver配置错误,导入正确的Driver------com.mysql.cj.jdbc.Driver();
4. Exception in thread "main" java.sql.SQLException:
   No suitable driver found for jbdc:mysql://127.0.0.1:3306/stumgr
   ----可能也是URL错误;
5. Public Key Retrieval is not allowed---- 这种情况一般是在密码错的时候出现;
   产生的原因:
    数据库密码加密规则与当前不符,可以在连接中通过 ServerRSAPublicKeyFile 指定服务器的 RSA 公钥，或者AllowPublicKeyRetrieval=True参数以允许客户端从服务器获取公钥；

**jdbc执行查询操作-------**

```java
import java.sql.*;

public class test002 {
    private final static String URL = "jdbc:mysql://localhost:3306/gavin";
    private final static String USER = "gavin";
    private final static String PASSWORD = "955945";

    public static void main(String[] args) {
    searchT();
   }
    public static  void searchT(){
        ResultSet resultSet;
        Statement statement = null;
        Connection connection = null;
        int id;
        String name;
        int age;
        try {
            Class aClass = Class.forName("com.mysql.cj.jdbc.Driver");//加载完毕接着就注册了
            connection = DriverManager.getConnection(URL, USER, PASSWORD);//获得连接
            //查询语句
            String sql = "select * from test where id=6;";
            //获得执行器
            statement = connection.createStatement();
            //执行后返回结果
            resultSet = statement.executeQuery(sql);
            while (resultSet.next()) {
                id = resultSet.getInt("id");
                name = resultSet.getString("name");
                age = resultSet.getByte("age");
                System.out.println(id + "-----" + name + "------" + age);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (null != statement) {
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != statement) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }

}

```

将以上代码做一个汇总和优化----

```java
package com.test.jdbc;

import java.sql.*;

public class SearchTest {
    private final static String URL = "jdbc:mysql://localhost:3306/gavin?useSSL=false&useUnicode=true&characterEncode=UTF-8&serverTimezone=Asia/Shanghai";
    private final static String USER = "gavin";
    private final static String PASSWORD = "gavin";
    private static String SQL = "insert into test values(11,'吴哥',34),(23,'Gavin',19);";
    private static String sql = "select * from test ;";
    private static Connection connection = null;
    private static Statement statement = null;
    private static ResultSet resultSet = null;

    public static void main(String[] args) {
        createLink();// 创建连接
        try {
             statement = connection.createStatement();//执行器
            int rows = statement.executeUpdate(SQL);//执行
            System.out.println("影响了--" + rows + "行数据");
        int id;
        String name;
        int age;
        resultSet = statement.executeQuery(sql);


            while ( resultSet.next()) {
                id= resultSet.getInt("id");
                name = resultSet.getString("name");
                age= resultSet.getInt("age");
                System.out.println(id + "-----" + name + "-----" + age);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            closeLink(resultSet,statement, connection);
        }
    }

    static void closeLink(ResultSet resultSet,Statement statement, Connection connection) {
        if(null!= statement){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(null!= connection) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(null!= resultSet) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    public static void createLink() {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");//导入驱动方式二
            connection = DriverManager.getConnection(URL, USER, PASSWORD);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

**问题来了,如何将查到的数据返回给客户端呢????**
1,将查到的数据包装成类----
2,如果取用类中的数据---通过实例化对象来调用()set,get方法
3,既然能发送出去,就要实现序列化



下面开搞-----
**定义一个类-----**
定义以前的要求--------------------->>>>>>>>
1,类名要和表名一致,
2,类中属性要和字段名一致(最好是,因为见名知意)
3,类中属性要和字段属性相匹配,最好是用包装类来接收;
4,类中必须要有空参构造

```java
package com.test.jdbc.SendData;
import java.io.Serializable;
import java.util.Date;
public class EMP implements Serializable {
    private Integer empno;
  .................封装参数...........

    public EMP() {
    .........空参构造.........
    }
    public EMP(Integer empno, String ename, String job, Integer mgr, Date date, Double sal, Double comm, Integer deptno) {
      ..........................
    }
   .......................覆写tostring...............
    @Override
    public String toString() {
        return "EMP{" +
               ...........
                '}';
    }
}

```

封装完毕后,开始

```java
package com.test.jdbc.SendData;
import com.test.jdbc.SearchTest;
import javax.xml.crypto.Data;
import java.sql.*;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class empJdbc {
    private final static String URL = "jdbc:mysql://localhost:3306/gavin";
    private final static String USER = "gavin";
    private final static String PASSWORD = "955945";
    private static String sql="select * from emp;";
    static Connection connection=null;
    static Statement statement=null;
    static ResultSet resultSet=null;
    static List <EMP>list = new ArrayList<EMP>();
    public List searchAll(){
       try {
          connection = DriverManager.getConnection(URL, USER, PASSWORD);
            statement = connection.createStatement();
            resultSet = statement.executeQuery(sql);
               while(resultSet.next()){
              int empno=resultSet.getInt("EMPNO");
             ...........获取对应的字段然后装进emp对象中........
                   EMP emp=new EMP(empno,ename,job,mgr,hiredate,sal,comm,deptno);//
             list.add(emp);
           }
       } catch (SQLException e) {
           e.printStackTrace();
       }finally {
//释放资源
          SearchTest.closeLink(resultSet,statement,connection);//在SearchTest类中有释放资源的静态方法
       }
        return list;//返回值
   }
}

```

网络化-----发送接收...

```java
package com.test.jdbc.SendData;
--导入相关包中的类
import java.util.List;
............
public class Client {
    public static void main(String[] args) throws IOException, ClassNotFoundException {//有异常向外抛
        Socket client =null;
        client=new Socket("localhost",8888);//绑定客户端
        InputStream ops = client.getInputStream();//得到服务器发送过来的信息
        ObjectInputStream ois=new ObjectInputStream(ops);//由于是对象,所以用object接收
        List <EMP>list = (ArrayList<EMP>)ois.readObject();//传过来的是一个List对象
迭代方法---foreach
 for (EMP e:list
             ) {
            System.out.println(e);
        }      
        //释放资源
        ois.close();
        ops.close();
        client.close();
    }
}

```

```java
package com.test.jdbc.SendData;
--导入相关包中的类
import java.io.IOException;
..........
public class serverSocketC {
    public static void main(String[] args) throws IOException {
        ServerSocket server=null;
        server=new ServerSocket(8888);//服务器开启了8888端口
        Socket accept = server.accept();//服务器接收到客户端请求
        OutputStream ops = accept.getOutputStream();//服务器向外发送数据
        ObjectOutputStream oos= new ObjectOutputStream(ops);
        oos.writeObject(new empJdbc().searchAll());//发送数据

        //释放资源
        oos.close();
        ops.close();
        server.close();
    }
}

```

运行后结果-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/d42b5b35e8e64f10a018238a835d33ab.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# MySQL问题实录

## 1,ERROR 1044 (42000): Access denied

问题简述: mysql创建数据库失败，提示ERROR 1044 (42000): Access denied for user 'huixin'@'localhost' to database '。。

```sql
mysql> show databases ;

+--------------------+

| Database           |

+--------------------+

| information_schema |

+--------------------+

1 row in set (0.00 sec)

mysql> create database Mygod ;

ERROR 1044 (42000): Access denied for user 'huixin'@'localhost' to database 'mygod'

```



以上是我在写权限的时候报的错误，为啥会出现错误呢？



解决思路--
1，检查该用户的权限，是否开启创建数据库的权限
登录root 用户，检查 huixin 的Create_priv 权限是否为 N，如果是修改为N 
alter table user set Create_priv ='Y' where user='huixin' ;
2,退出root用户， 登录 用户huixin  创建数据库 即可

```sql
mysql> show databases ;
+--------------------+
| Database           |
+--------------------+
| company            |
| index_test         |
| information_schema |
| market             |
| mygod              |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
8 rows in set (0.00 sec)
```

## 2, you are not allowed to create user

问题简述:中 you are not allowed to create user with grant 的问题

先查看一下错误------

```sql
> 
mysql> grant select,insert on company.* to 'username'@'localhost' with grant opt
ion ;
ERROR 1410 (42000): You are not allowed to create a user with GRANT
mysql> grant select,insert on company.* to 'username'@'localhost' with grant opt
ion ;
ERROR 1410 (42000): You are not allowed to create a user with GRANT
```

排查原因--
1，查看root用户是否有grant_priv 权限，

```sql
mysql> select user ,grant_priv from user ;
+------------------+------------+
| user             | grant_priv |
+------------------+------------+
| mysql            | Y          |
| root             | Y          |
| huixin           | N          |
| mysql.infoschema | N          |
| mysql.session    | N          |
| mysql.sys        | N          |
| username         | N          |
+------------------+------------+
7 rows in set (0.00 sec)
```

发现有该权限，这说明不是权限的问题；继续排查下一个--
2，如果root用户仅在 localhost主机下去授权用户在别的主机上的权限也是不可以的，比如 ‘root'@'localhost’ 去授权别的主机用户  ‘username’@‘otherhost’ 也会报错，所以修改root用户的 host 为可以访问所有主机（打造一个真正的超级管理员），然后更新权限为拥有所有的权限，这里是必须要更新的，要不然还会报错，（对于为何要再次更新权限，我是这样理解的，只要更改了 user，host，或者authentication_string中的任何一个就要刷新权限）（对于为何要重新给 修改后的root用户重新赋予权限）因为没有刷新之前的记录的话新的权限没有覆盖

```sql
mysql> update user set host='%' where user='root' ;
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0
mysql> grant all privileges on *.* to 'root'@'%' ;
Query OK, 0 rows affected (0.00 sec)
```

3，刷新权限，授予权限

```sql
mysql> flush privileges ;
Query OK, 0 rows affected (0.00 sec)

mysql> grant select ,insert ,update on company.* to 'huixin_user'@'localhost' wi
th grant option ;#8.0之后的mysql不支持 授权的时候就进行用户创建，所以创建  之后才能授权;
ERROR 1410 (42000): You are not allowed to create a user with GRANT
mysql> grant select ,insert ,update on company.* to 'huixin'@'localhost' with gr
ant option ;
Query OK, 0 rows affected (0.00 sec)
```

由于huixin用户是之前创建好的，授权的时候又给他授权了grant_priv ，再次查看huiixn的权限

```sql
mysql> select user ,host,select_priv,insert_priv,update_priv,grant_priv from user where user='huiixn';
+------------------+-----------+-------------+-------------+-------------+------------+
| user             | host      | select_priv | insert_priv | update_priv | grant_priv |
+------------------+-----------+-------------+-------------+-------------+------------+
|        |
| huixin           | localhost | Y           | Y           | Y           | N          |
|        |
+------------------+-----------+-------------+-------------+-------------+------------+
7 rows in set (0.00 sec)

mysql>
```

以上就是解决的具体方式，整体思路如下：

> 登录root用户，
> 进入mysql数据库，
> 修改root 的主机名为可以访问所有主机，
> 重新更新root的所有权限
> 刷新权限
> 创建用户（如果之前创建过则不用再次创建）
> 授权给用户





## 3, 加密函数解密时为16进制数据

看代码：

```sql
`mysql>  select aes_decrypt(aes_encrypt('Iloveyou','1'),'1');
+--------------------------------------------------------------------------------------------+
| aes_decrypt(aes_encrypt('Iloveyou','1'),'1')                                               |
+--------------------------------------------------------------------------------------------+
| 0x496C6F7665796F75                                                                         |
+--------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

`解密之后为16进制数据，原因为系统编码方式（gbk）跟数据库采用的编码(utf8)方式不一样，
既然解密后为16进制数，那么把这个数转换成我想要的就可以了，mysql提供了 convert函数，用法如下
convert(进制数 using 编码格式）；
所以可以用如下方式解密得到我们想要的---

```sql
+------------------------------------------------------------------+
| convert(aes_decrypt(aes_encrypt('Iloveyou','1'),'1') using utf8) |
+------------------------------------------------------------------+
| Iloveyou                                                         |
+------------------------------------------------------------------+
1 row in set, 1 warning (0.00 sec)

mysql>  select  convert(aes_decrypt(aes_encrypt('woaini','1'),'1') using utf8);
+----------------------------------------------------------------+
| convert(aes_decrypt(aes_encrypt('woaini','1'),'1') using utf8) |
+----------------------------------------------------------------+
| woaini                                                         |
+----------------------------------------------------------------+
1 row in set, 1 warning (0.00 sec)

#尝试加密汉字，解密后出现null;
mysql> select convert(aes_decrypt(aes_encrypt('我爱你','1'),'1') using utf8);
+----------------------------------------------------------------+
| convert(aes_decrypt(aes_encrypt('我爱你','1'),'1') using utf8) |
+----------------------------------------------------------------+
| NULL                                                           |
+----------------------------------------------------------------+
1 row in set, 2 warnings (0.00 sec)
```

但是当加密汉字的时候解密后会出现null，所以我该考虑一下更改电脑编码格式了吧！

## 4,备份时找不到data文件夹

首先备份数据库的方式有多种，
1，mysqldump -u 用户名 -h 主机名 -p密码 数据库1名.表1名 数据库名.表2名   > 要备份到的路径\**.sql 
如果是备份数据库的全部--
mysqldump -u 用户名 -h 主机名 -p密码 --all-databases（也可以这样写all_databases）   > 要备份到的路径\**.sql 

2，如果要备份的全部数据库都是 myisam 引擎的话，用mysqlhotcopy（即mysql热拷贝），这样备份速度快，效率高

3，不过最常用的备份方式是直接拷贝mysql数据库文件，这样简单，快速有效，不过有几点要注意的
a,要想保持备份的一致性，备份前需要对相关表执行 locktables 操作，然后对表执行 flush tables 这样在复制数据库文件时可以允许用户查询数据库；   flush tables 语句是确保开始备份前将所有激活的索引页写入硬盘，当然也可以停止mysql服务在进行备份操作；

但是在拷贝数据库文件时发现找不到 data文件，这是怎么办么？

解决方式如下：
在cmd输入以下代码

```sql
C:\Users\Administrator>mysqld --initialize-insecure
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210603141047631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)这样就会找到data 文件夹，因为一开始安装的时候mysql将data文件夹隐藏了，需要手动在cmd界面刷新一下---即启用非安全模式。

 反推一下如何让data隐藏呢？  ---mysqld --initialize-secure 
以上就是解决方法，共同学习共同进步！

## 5,secure-file-pri option so it cannot execute this.

根据提示我们可以发现原因是secure-file-pri  的问题，我们查看一下在mysql中关于 该权限的一些信息，

~~~sql
#显示 参数--近似secure 的参数
show variables like “secure%” 
#secure-file-priv="C:\ProgramData\MySQL/MySQL Server 8.0\Uploads"

mysql> show variables like 'secure%' ;
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| secure_file_priv |       |
+------------------+-------+
#由于以上是修改之后的，所以显示value为空， 修改之前的value内容为"C:/ProgramData/MySQL/MySQL Server 8.0/Uploads" ;
1 row in set, 1 warning (0.01 sec)

接下来我们尝试修改  secure_file_priv 的value之，首先要找到该字段在哪张表里，由于是系统（全局）级的权限的字段，直接用set尝试修改它的值--

```sql
mysql>  set secure_file_priv ='' ;
ERROR 1238 (HY000): Variable 'secure_file_priv' is a read only variable
~~~

提示该权限为只读，不允许修改，那咋办？

网友提供的方法1--如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210604195319321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)也可以通过修改配置文件来修改
下面我整理一下第二种如何修改，尽可能把可能出现的问题进行全面列举，解决；
第一步--找到my.ini(或者my.conf)，  如果找不到则在文件系统中找到 C:\ProgramData\MySQL\MySQL Server 8.0 ，
 由于系统默认隐藏了ProgramData，所以需要 查看隐藏文件 win7的是  工具---文件夹选项---查看--显示隐藏的文件和驱动器，就可以找到了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210604195834569.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060420003864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)之后停止mysql服务，之后以记事本方式打开my.ini，修改secure_file_priv 的值为“”，如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021060420020258.png)
最好是注释掉，保存。
注意保存的时候一定要注意my.ini编码格式为ansi ，否则再次打开mysql服务会出错；
如果不会修改my.ini编码格式，可以先将my.ini修改为my.ini.bak，然后另存为，这时候会出现编码格式，修改为ansi
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210604200547537.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)此时重启mysql就不会报错了。

```
secure-file-priv的值有三种情况：
当secure_file_priv 的value值为“”或者干脆不写 (都表示为空，但不是null)时，表示可以将表导出。

演示第一种----代码如下`

​```sql
#关闭mysql服务，修改my.ini （配置文件）使得[secure-file-priv=    ] 之后开启mysql服务，登录
mysql> show variables like 'secure%' ;
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| secure_file_priv |       |
+------------------+-------+
1 row in set, 1 warning (0.01 sec)

mysql> use company ;
Database changed
mysql> select * from hxjy into  outfile 'C:\hxjy.doc' ;
Query OK, 11 rows affected (0.01 sec)
#之后去C盘根目录找不到hxjy.doc(这里只要是文本类文件就可以),搜索hxjy.doc 发现在 C:\ProgramData\MySQL\MySQL Server 8.0\Data 下，

```

这里要注意的是，如果为空，导出的时候如果将地址书写不符合要求了，也会提示导入成功，不过是将文件导入到了 C:\ProgramData\MySQL\MySQL Server 8.0\Data 位置下，（可能为了方便备份吧）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210605091533962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

第二种：
当secure_file_priv 的value只值为非null时，表示只可以导出到该路径下，如我的当前环境中的为C:/ProgramData/MySQL/MySQL Server 8.0/Uploads 文件夹下,注意路径的分隔符写法 "\"  这时才会导入到我们指定的位置，如果是"/"，我们来尝试一下会发生什么---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210605091932600.png)

```sql
mysql> show variables like 'secure%' ;
+------------------+---------------+
| Variable_name    | Value         |
+------------------+---------------+
| secure_file_priv | C:\mysqlback\ |
+------------------+---------------+
1 row in set, 1 warning (0.01 sec)
#由于我的地址分隔符是 '/',所以当我们地址按照'\'来写就会报错
mysql> use company ;
Database changed
mysql> select * from hxjy into outfile 'C:\mysqlback\hxjyya.doc';
ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot exec
ute this statement
#写成'/'就可以。
mysql> select * from hxjy into outfile 'C:/mysqlback/hxjyya.doc';
Query OK, 11 rows affected (0.00 sec)

```

```sql
#我设置了secure_file_priv="c:/mysqlback/hxjy.txt"
mysql> select * from hxjy into outfile 'c:\mysqlback\hxjy.txt' ;
ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot exec
ute this statement

```

当secure_file_priv 的value值指定为null时，则不允许导入和导出
就不演示了。



以上是主要步骤，如有其它问题欢迎提问；
共同学习共同进步！！！



## 6,could not open logs

首先很多刚学习数据库的伙伴在接触到 二进制日志恢复数据库数据的时候会遇到问题，先来分析一下为什么会出现这种错误提示？
不能打来日志文件，或者文件找不到

```sql
C:\Users\Administrator>mysqlbinlog --stop-datetime="2021-05-20 12:25:25" C:\ProgramData\MySQL\MySQL
Server 8.0\Data\HWL-2020-bin.000004 mysql -u root -p
Enter password: ******
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
mysqlbinlog: File 'C:\ProgramData\MySQL\MySQL' not found (OS errno 2 - No such file or directory)
ERROR: Could not open log file
SET @@SESSION.GTID_NEXT= 'AUTOMATIC' /* added by mysqlbinlog */ /*!*/;
DELIMITER ;
# End of log file
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;

```

这时候有两个地方会导致出现该错误--

> 输入文件地址的时候分隔符 mysql中地址的分隔符为"/" ，而不是windows平台下的“\”。

 另一个就是当我们安装mysql时一般会选择安装路径，大部分人会选择一路默认下去，许多老师也推荐此种方式，当然我也推荐。
 不过我们在安装完毕之后应该去修改 mysql数据库存放文件的默认存放地址--默认是--C:\ProgramData\MySQL\MySQL Server 8.0\Data
 但是你会发现，C:\ProgramData默认是隐藏文件，并且为只读
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608152855820.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)所以当我们用此路径下的日志文件去尝试恢复数据时，会出现不能打开log 的错误；

> 解决方法是--更换数据库文件存储位置----方法如下
> 停止mysql服务，然后修改配置文件

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210608153106172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)图中第一个是安装路径，第二个是数据库文件存放地址，修改为自己想要存放的位置即可；
 改完之后在此开启mysql服务，尝试用二进制文件恢复数据

```sql
C:\Users\Administrator>mysqlbinlog --stop-datetime="2021-06-05 12:25:25" D:/MySqlData/Data/HWL-2020-
bin.000061 mysql -u root -p
Enter password: ******
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
SET @@SESSION.GTID_NEXT= 'AUTOMATIC' /* added by mysqlbinlog */ /*!*/;
DELIMITER ;
# End of log file
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
```

以上就是解决方法。
共同学习共同进步，卷起来了！！

## 7,主从复制的异常解决

window平台下要想实现主从复制，首先得有主和从

1. 找到主从计算机（服务器）的IP地址，以便实现两个之间的联通；
2. 有了主从，主从上都要有mysql，版本最好一致，不然数据复制过程中会出现一些意外情况；
   **查看本机IP地址的方法**
   打开cmd 输入ipconfig/all
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210610093812300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)
3. 配置主服务器
   配置前检查一下数据库文件的存放地址；

```sql
Copyright (c) 2000, 2021, Oracle and/or its affiliates.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show variables like '%datadir%';
+---------------+--------------------+
| Variable_name | Value              |
+---------------+--------------------+
| datadir       | D:\MySqlData\Data\ |
+---------------+--------------------+
1 row in set, 1 warning (0.00 sec)
```

①开启binlog日志
找到配置文件所在位置 我的环境中是C:\ProgramData\MySQL\MySQL Server 8.0
关闭mysql服务。然后再修改my.ini
修改内容如下， 以记事本打开my.ini
[mysqld]之后添加如下内容

> log_bin="D:/MySqlData/binlog        #log的地址
> expire_logs_days =10 #log的删除天数
> max_binlog_size =100M #log文件最大的大小

另存为--my.ini（最好是另存为，同时可以检查该文件的另存为后的编码方式 --ansi）
开启mysql 服务并登录
检查二进制文件配置是否成功（log_bin 为 on则开启成功）

```sql
mysql> show variables like '%log_bin%' ;
+---------------------------------+--------------------------------------+
| Variable_name                   | Value                                |
+---------------------------------+--------------------------------------+
| log_bin                         | ON                                   |
| log_bin_basename                | D:\MySqlData\Data\HWL-2020-bin       |
| log_bin_index                   | D:\MySqlData\Data\HWL-2020-bin.index |
| log_bin_trust_function_creators | OFF                                  |
| log_bin_use_v1_row_events       | OFF                                  |
| sql_log_bin                     | ON                                   |
+---------------------------------+--------------------------------------+
6 rows in set, 1 warning (0.00 sec)
```

1. 在主机上配置 所需要的账户，（如果有的话，那就不需要配置了，这里顺带复习一下创建用户） 

```sql
mysql> create user 'user_log'@'%' identified by '123456' ;
Query OK, 0 rows affected (0.03 sec)
```

配置完毕再授予可复制从机权限（8.0之后在授权的时候创建用户会出错）

> 如下-----
> mysql> grant replication slave on *.* to 'user_l'@'%' identified by '123456' ;
> ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your
> MySQL server version for the right syntax to use near 'identified by '123456'' at line 1

```sql
mysql> grant replication slave on *.* to 'user_log'@'%' ;
Query OK, 0 rows affected (0.00 sec)
```

 5.配置主机信息
  ①退出mysql②并关闭主机的mysql服务③配置my.ini
  配置my.ini内容如下

> server-id=1 #服务器标识
> binlog-do-db=hxjy # 需要复制的数据库
> bin-ignore-db=mysql#忽略的数据库（即不需要复制的数据库）

   配置完毕后重启mysql服务，登录mysql，检查主机状态； 

```sql
mysql> show master status \G
*************************** 1. row ***************************
             File: HWL-2020-bin.000006
         Position: 689
     Binlog_Do_DB: hxjy
 Binlog_Ignore_DB: mysql
Executed_Gtid_Set:
1 row in set (0.00 sec)
```

1. 将master主机的数据备份出来，然后导入到slave从机中去     （如果导出错误请检查导出地址是否符合mysql安全要求）

> 登录数据库检查---secure_file_priv  的值
> mysql> show variables like '%secure%'
>   -> ;
> +--------------------------+---------------+
> | Variable_name            | Value         |
> +--------------------------+---------------+
> | require_secure_transport | OFF           |
> | secure_file_priv         | C:\mysqlback\ |
> +--------------------------+---------------+
> 2 rows in set, 1 warning (0.00 sec)

​                                                                                                                                                                       

```sql
1 row in set (0.00 sec)

mysql> exit
Bye

C:\Users\Administrator>mysqldump -u root -p company hxjy >C:/mysqlback/hxjy.txt
Enter password: ******
```

1. 配置slave从机的信息
   停止slave从机的mysql服务
   找到slave从机的my.ini 做如下修改
   [mysqld]
   default-character-set=utf8 #设置从机的默认编码
   log_bin="D:/mysqlback/binlog" #设置从机的二进制文件存放地址
   expire_logs_days=10
   max_binlog_size=100M
   [mysqld]#新添加的
   server_id=2#写在新添加的后面，如果有其他binlog配置可以注释掉
2. 重启slave 的mysql服务，登录slave， 关闭slave服务

```sql
mysql> stop slave ;
Query OK, 0 rows affected, 2 warnings (0.00 sec)
```

1. 设置slave从机实现复制的相关信息

```sql
mysql> change master to
    -> master_host='182.168.0.208' ,
    -> master_user='user_log',
    -> master_password='123456',
    -> master_log_file='HWL-2020-bin.000006',#要复制的二进制文件名
    -> master_log_pos=150;#偏移量
Query OK, 0 rows affected, 7 warnings (0.03 sec)

```

如果出现错误，那么检查一下主机的日志文件名与位置是否正确，

```sql
mysql> show master status \G
*************************** 1. row ***************************
             File: HWL-2020-bin.000006
         Position: 689
     Binlog_Do_DB: hxjy
 Binlog_Ignore_DB: mysql
Executed_Gtid_Set:
1 row in set (0.00 sec)
```

1. 验证阶段--在主机上数据库hxjy 下建立表，插入数据检查从机上数据是否同步过去了

## 8,读写分离 ,Proxy配置错误解决

mysql Proxy 是一个 客户端和服务端之间程序，可以实现读写分离，基本原理是--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210611171235983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)虽然一个mysql proxy 可以连接多个 客户端与数据库，但是有可能mysql proxy出现异常，所以最好是准备多个mysql proxy，以避免单点失效情况的出现。

1. mysql-proxy安装与配置。
   将下载好的MySQL Proxy 进行配置 ，下载地址是：https://downloads.mysql.com/archives/proxy/
    根据自己的平台进行 选择适合的版本，本次以windows平台为例
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210611171902195.png)下载后解压，修改解压后的文件接名为mysql-proxy-0.8.5，然后移动到mysql安装根目录下--

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210611172020779.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

1. 在cmd界面进行配置 mysql-proxy

```
C:\Users\Administrator>sc create "Proxy" DisplayName="MySQL Proxy" start="auto"  binPath="C:\Program
 Files\MySQL\mysql-proxy-0.8.5\bin\mysql-proxy-svc.exe  --proxy-backend-addresses=127.0.0.1:3306"

```

配置的时候出现提示---

```
 Files\MySQL\mysql-proxy-0.8.5\bin\mysql-proxy-svc.exe  --pr
描述:
        在注册表和服务数据库中创建服务项。
用法:
        sc <server> create [service name] [binPath= ] <optio

选项:
注意: 选项名称包括等号。
      等号和值之间需要一个空格。
 type= <own|share|interact|kernel|filesys|rec>
       (默认 = own)
 start= <boot|system|auto|demand|disabled|delayed-auto>
       (默认 = demand)
 error= <normal|severe|critical|ignore>
       (默认 = normal)
 binPath= <BinaryPathName>
 group= <LoadOrderGroup>
 tag= <yes|no>
 depend= <依存关系(以 / (斜杠) 分隔)>
 obj= <AccountName|ObjectName>
       (默认 = LocalSystem)
 DisplayName= <显示名称>
 password= <密码>

```

检查之后发现  选项名称包括等号。
等号和值之间需要一个空格。

> （程序猿要仔细仔细再仔细，有时候一个标点就要了老命了）

```
C:\Users\Administrator>sc create "Proxy" DisplayName= "MySQL Proxy" start= "auto"  binPath= "C:\Prog
ram Files\MySQL\mysql-proxy-0.8.5\bin\mysql-proxy-svc.exe  --proxy-backend-addresses=127.0.0.1:3306"

[SC] CreateService 成功

C:\Users\Administrator>

```

> 代码中sc 表示显示或者配置一下命令 
> create  “Proxy”  --创建proxy服务  
> Displayname=   --服务名（=与服务名之间有空格）
> start= "auto" --表示自动启动，
>
> | 手动启动            | 禁用                   |
> | ------------------- | ---------------------- |
> | start=DEMAND (手动) | start=DISABLED  (禁用) |
>
> ------

1. 开启mysql proxy 服务
   在cmd 界面输入 net start proxy 

```
C:\Users\Administrator>net start proxy
MySQL Proxy 服务正在启动 .
MySQL Proxy 服务已经启动成功。


```

| 开启某服务命令   | 关闭某服务命令  |
| ---------------- | --------------- |
| net start 服务名 | net stop 服务名 |

可以删除服务然后在重新配置一下，以便加深印象（程序猿要记住很多代码吗？？？）

> 删除服务是          sc delete 服务名

有的朋友会遇到一下问题--

```
C:\Users\Administrator>sc create "proxy"  displayname= "MySQL Proxy" start= "auto" binpath= "C:\Prog
ram Files\MySQL\mysql-proxy-0.8.5\bin\mysql-proxy-svc.exe  --proxy-backend-address=127.0.0.1:3306"
[SC] CreateService 成功

C:\Users\Administrator>net start proxy
MySQL Proxy 服务正在启动 .
MySQL Proxy 服务无法启动。

服务没有报告任何错误。

请键入 NET HELPMSG 3534 以获得更多的帮助。


```

出现以上错误的原因是 DisPlayName="*****"     不符合要求，应该写成MySQL Proxy， 删掉服务，重新配置启动就好了。

mysql-proxy配置参数含义如下---

------

> ●　--proxy-backend-addresses：该参数用来指定MySQL服务器的IP地址和端口号，如果代理多个服务器，可以用逗号分隔。
> ●　--proxy-read-only-backend-addresses：该参数用来指定只读服务器的IP地址和端口号，如果代理多个服务器，可以用逗号分隔。
> ●　--proxy-skip-profiling：该参数用来设置是否禁用查询性能分析。
> ●　--proxy-lua-script：该参数用来指定lua脚本文件。 ●　--daemon：采用daemon方式启动。
> ●　--admin-address：指定MySQL Proxy的管理端口。 ●　--proxy-address=：指定mMySQL
> Proxy的监听端口。
> 读者也可以通过mysql-proxy --help-all查看完整的参数含义。

接下还不能正式运行proxy 命令，还需要在cmd界面进入C:\Program Files\MySQL\mysql-proxy-0.8.5\bin 配置代理参数

```
C:\Users\Administrator>cd C:\Program Files\MySQL\mysql-proxy-0.8.5\bin

C:\Program Files\MySQL\mysql-proxy-0.8.5\bin>

```

配置代理参数--

```
C:\Program Files\MySQL\mysql-proxy-0.8.5\bin>mysql-proxy  --proxy-address= localhost:49710  --proxy-
backend-addresses=201.13.100.41:8008
2021-06-11 18:06:41: (critical) unknown option: localhost:49710

```

端口可以自己配置，如果想要在cmd下直接整mysql proxy 代码，要在path下配置该路径--
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210611181053138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)





## 9,缺少依赖

jdbc依赖文件-----也就是我们习惯叫的驱动文件,
下载完毕之后将该文件添加到classpath库中,

在idea中的方法是在模块中新建一个包----命名为lib 然后将下载好的依赖文件复制到该包中
![在这里插入图片描述](https://img-blog.csdnimg.cn/80e698e67a934a729f954fa3f84486ad.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
完成这一步之后算是准备工作做完了,
接着右键单击该依赖文件,选则add as libary,确认OK
![在这里插入图片描述](https://img-blog.csdnimg.cn/9400b5d3761a43a7889c51b348c4b9ac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
你会发现该依赖文件按右边多了一个箭头,带你开箭头会有相应的子包出现
![在这里插入图片描述](https://img-blog.csdnimg.cn/29ad2e94424f4d158703dcacbdf23b06.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

说明导入依赖文件成功;

那如何正确的移除该依赖文件呢????
直接删除不是正确的操作,因为这相当于没有告诉我们的程序------我把你的依赖文件删除了,那当我们再次运行该程序的时候就会出现错误;

正确移除方式------
单击 file菜单,找到project  structure 
![在这里插入图片描述](https://img-blog.csdnimg.cn/a547aad8da834ca39890acf77a4d3bd3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)modules 模块下的dependencies,选中当前依赖所在模块,然后依次点击   减号,  apply  ,ok;
![在这里插入图片描述](https://img-blog.csdnimg.cn/adb8860b15f04d659dcd9649fd3f9188.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然后就可以正常删除了;

## 10,unable to resolve table



先看问题所在------爆红但是并不是错误,可以不用管他,如果看着不舒服可以按照下面的步骤------

![在这里插入图片描述](https://img-blog.csdnimg.cn/bb49aa98bee2469d8e1e119ee5b9eac7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 根据提示说是要添加数据源,

![在这里插入图片描述](https://img-blog.csdnimg.cn/baa3c65058714b76894cad6352622417.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 单击打开database,单击+ 号,data source,选择mysql

![在这里插入图片描述](https://img-blog.csdnimg.cn/b221b90ef9794ebf8646c1814212c436.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)当然你也可以选择者驱动和数据源一块添加;

> 进入如下界面,**选择你当前在用的mysql对应的版本5.1之后的选mysql之前的选mysql5.1**

![在这里插入图片描述](https://img-blog.csdnimg.cn/b17ff01c81f549bfb5f80dd4085de417.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 接着单击 左上角    +   号, 选择 mysql,      general  输入Host ,user,password ,以及要连接的数据库database,
> 接着test connection 



![在这里插入图片描述](https://img-blog.csdnimg.cn/cf6e25a92b8243eb86cda4885d9f4685.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 可能会提示让你修改时区 severtimezone, 在advance 界面下找到 severtimezone,指定相应时区,一般是指定为
> Asia/Shanghai,



![在这里插入图片描述](https://img-blog.csdnimg.cn/3d1a970dd435437b861c6a0478ebc0b0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
应用后退出,即可看到  表名处   不报红了;![在这里插入图片描述](https://img-blog.csdnimg.cn/8edf8784cb9c4ac1b9b83400accbc8aa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 操作正确的话会看到该界面,说明连接数据库成功;

![在这里插入图片描述](https://img-blog.csdnimg.cn/04fec2a6d5f747c29c6ed8d39aef4a82.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



duplicated code fragment

作为一只程序元,英文基础不太好的话.......还好有那么多翻译软件,但打铁还需自身硬;
问题产生------
![在这里插入图片描述](https://img-blog.csdnimg.cn/d168d9efd2294b88b88d8c500b298877.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)虽然没报错但是看着难受呀-----虽然能运行

![在这里插入图片描述](https://img-blog.csdnimg.cn/a1b0214eb4374c55bb3cb91da9d982ce.png)
将finally中的代码块放在一个方法里就OK了
![在这里插入图片描述](https://img-blog.csdnimg.cn/ee156adcfeed41e38d1cfe331ba18d96.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## 11,连接SQL Server2014无法使用ssl加密与sqlserver建立连接

首先检查一下配置------
**第一步---->检查sql配置管理器**
在sqlserver网络配置   中查看 你电脑中存在的数据协议,查看右边 的TCP/IP是否开启,
![在这里插入图片描述](https://img-blog.csdnimg.cn/8502fe81a0514dc681813e39e75b1860.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**第二步----->右键TCP/IP,选择属性**
修改如下的值为是;
![在这里插入图片描述](https://img-blog.csdnimg.cn/c3fd45623cd843f98d99b6cb938091a7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)**第三步----->**

修改IPALL中TCP端口为1433;
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ed84a720f314f2aabb9ef9e03dcce98.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
做完以上工作,接着要在数据库连接的URL上添加一些属性来完成

不使用SSL加密和不在jvm中存储密码;
![在这里插入图片描述](https://img-blog.csdnimg.cn/66562576c119444184f0d38f6dbb1d2f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
正常运行结果-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/62607170362b4acca06d49dc3ee62735.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
有一个重要的也是很容易被遗漏的点-----改完之后要重启下sqlserver服务，不然改动不生效哦！！

[MySQL新特性](/mysql/MySQL新特性.md#mysql-80的部分新特性)

![](..\pic\div\weichatcode.png)
