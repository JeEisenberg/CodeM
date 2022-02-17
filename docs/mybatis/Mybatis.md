![](..\pic\logo2.png)

# Mybatis

Mybatis是一个持久层框架,要细致的说是一个版持久性的框架,通过简单的xml或者注解进行配置,使得数据库的操没有那么繁琐;

**什么是mybatis**

mybatis是一种挂逆行型映射框架,用于解决数据库操做时遇到的一些问题;

在学习mybatis时还接触到了一个全表映射的框架 hibernate也是用于数据库操做的一个框架,hibernate不要求熟练掌握sql,而是根据对应逻辑来动态生成sql,所以开发效率要高于mybatis,但是随着数据库中内容的增多以及多张表的存在,在进行多表关联查询时性能会比mybatis低,所以herbernate适用于不太复杂,且对性能要求不高的项目中;


## 简单的案例----数据库CURD

mybatis并不依赖于spring,只是他们通常一起使用,在实际开发中我们经常会遇到查询的功能,如果是通过jdbc进行开发,则代码量会很大,同时如果要修改查询的则要去修改整个程序的代码,非常不方便;

**查询用户**
在实际开发中，查询操作通常都会涉及单条数据的精确查询以及多条数据的模糊查询。

第一步------>>>新建一个maven工程

引入相应依赖---
lombok
mybatis
junit
mysql-connector
log4j

下面是依赖的代码,可以直接拿走

```xml
<dependencies>
<!--    自动资源管理，自动生成getters，setters，equals，hashCode和toString-->
    <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.22</version>
        <scope>provided</scope>
    </dependency>

    <!--测试Junit-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>compile</scope>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.27</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
</dependencies>
```

然后开始写代码了去实现想要的功能;
这里以图书商店的后台数据库为例

还需要准备一个数据表----->>>

```sql

DROP TABLE IF EXISTS `bookstore`;

CREATE TABLE `bookstore` (
  `BookId` int NOT NULL,
  `BookName` varchar(255) DEFAULT NULL,
  `BookPublish` varchar(255) DEFAULT NULL,
  `BookPrice` double(10,2) DEFAULT NULL,
  `BookKind` varchar(16) DEFAULT NULL,
  `BookCount` int DEFAULT NULL,
  PRIMARY KEY (`BookId`),
  KEY `name_index` (`BookName`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;



insert  into `bookstore`(`BookId`,`BookName`,`BookPublish`,`BookPrice`,`BookKind`,`BookCount`) values 
(1001,'java入门','清华出版社',56.80,'计算机',70),
(1002,'python深入','机械工业出版社',78.60,'计算机',130),
(1003,'前端html','清华出版社',88.80,'计算机',100),
(1004,'废都','张江出版社',25.70,'文学名著',100),
(1005,'官场现形记','青岛出版社',23.80,'文学名著',100),
(1006,'红楼梦','青岛出版社',36.80,'文学名著',100),
(1007,'母猪的产后护理','机械工业出版社',27.80,'实用技术',100),
(1008,'车辆维修大全','机械工业出版社',26.80,'实用技术',100),
(1009,'如何讨取富婆欢心富婆','开心出版社',125.00,'生活技能',100),
(1010,'JVM调优','北京科技出版社',68.90,'计算机',120),
(1011,'GO','南京邮电出版社',78.90,'计算机',120),
(1012,'Android入门','电子科技出版社',88.90,'计算机',120),
(1014,'果树种植学','西北农林出版社',128.90,'农业',120),
(1015,'笑话','上海出版社',48.90,'生活',120),
(1111,'MQ消息队列','北大出版社',98.90,'计算机',120);
```
第二步------->>>编写代码去实现功能
写一个实体类
看起来数据表中的字段有点多,别怕,前面不是引入了lombok吗,就是简化代码量的;

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.io.Serializable;
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Book implements Serializable {
    private Integer BookId;
    private String BookName;
    private String BookPublish;
    private Double BookPrice;
    private String BookKind;
    private Integer BookCount;
}

```
是不是很简洁,set/get方法,toString,hashcode等通过lombok的注解去实现而不用,如果你在使用过程中遇到了错误,那么可能你还没有开启lombok插件使其自动生效

开启方式---->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/26aca77711364a8db335a4cba4c4e98b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
第三步----->>>在resources文件夹下建立
mybatis全局配置文件mybatis-config.xml
在这之前还需要配置一个jdbc配置文件,当然你也可以直接将数据库连接要素写在mybatis-config.xml中

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <properties resource="jdbc.properties"/>

    <settings>
        <!--指定mybatis日志方式,如果不指定,自动查找处理-->
        <setting name="logImpl" value="LOG4J"/>
    </settings>

    <typeAliases>
        <!--包扫描起别名  类的短路径名首字母小写-->
        <package name="com.gavin.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc_driver}"/>
                <property name="url" value="${jdbc_url}"/>
                <property name="username" value="${jdbc_username}"/>
                <property name="password" value="${jdbc_password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--加载mapper映射文件-->
    <mappers>
        <mapper resource="com.gavin.mapper/BookMapper.xml"/>

    </mappers>
</configuration>
```
jdbc.properties
```xml
jdbc_driver=com.mysql.cj.jdbc.Driver
jdbc_url=jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
jdbc_username=gavin
jdbc_password=955945

```

如果想要打印日志还需要借助log4j或者log4j2,本次以log4j为例子
在resources文件夹下建立log配置文件


log4j.properties

```xml
log4j.rootLogger=info,stdout,logfile
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.SimpleLayout
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=d:/gavin.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d{yyyy-MM-dd   HH:mm:ss} %l %F %p %m%n
```

最后编写测试类----->

这里要理解mybatis的工作流程才能更好的编写测试类--->>>
首先mybatis共工作时必备两个文件-------mybatis-config.xml(全局配置文件)和Mapper.xml(映射文件)当然文件名不一定非得是这个
mybatis正是通过这两个文件来解析并创建会话的;
每次会话都会创建一次sqlsessionFactory对象,然后通过工厂对象创建sqlsession对象,sqlsession对象用于接收请求,然后分析要执行哪些sql语句,最后传给executor去执行,执行的时候会用到statementHandler去对接数据库;
这就是mybatis的基本工作原理;
***看图----->>***
![在这里插入图片描述](https://img-blog.csdnimg.cn/31d078a4d3b040c3a9952d2d5430bcbb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
然后开始写测试代码,通过测试模块来完成测试的编写;



```java
public class Test1 {
    private  SqlSession sqlSession =null;
    @Before
    public void init(){
        InputStream resource =null;
        //载入配置文件
        try {
           resource= Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resource);
            sqlSession= build.openSession(true);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    @Test
    public void test(){

        List<Book> list = sqlSession.selectList("findAll");
        list.stream().forEach(System.out::println);
    }
    @After
    public void testClose(){
        sqlSession.close();
    }
}

```

运行结果---->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/81fb9346b56a467391c70621381f18f7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
有时候我们 会遇到一些错误,不要着急,要细心查看日志,看是哪里出错了;

增删查改其实答题原理是一样,都要注意mapper的路径问题;


好了接下来开始了解各个配置文件中的书写格式le

Mapper文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/70053bb008974b20aeb530af47136659.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



全局配置文件
mybatisconfig.xml

![在这里插入图片描述](https://img-blog.csdnimg.cn/5e78fec40df1460091353272b97e8702.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**实行模糊查询**
修改sql语句就可以了
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc5fcc2ee2094f0a9f9a69571863f8e9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)但是这样写无法防止sql注入,

查看有没有合适的方法预编译的,然而并没有,那只能通过字符串的拼接了,
就像这样
![在这里插入图片描述](https://img-blog.csdnimg.cn/392ab3e884794289b6a7555fdf7a0460.png)
**添加数据**

基本操做不变,就是sql语句和数据库操做的方法会有变动;
注意这次是插入数据,就不用写返回值了,因为insert标签下也没有resultType属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/14d5145d43db4c398502492cb0cf9eef.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果没去掉图片横线处的内容,会报错误,
![在这里插入图片描述](https://img-blog.csdnimg.cn/6a0a0d5f308a470cabe2e32a80f81fde.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
我也不知道为啥明明注释掉了,然后才发现少了一些东西.....很奇怪,错误总在不经意间出现;


**更新数据**

![在这里插入图片描述](https://img-blog.csdnimg.cn/3e058dec3f7c4397a8e13d601c19ed06.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**删除数据**
![在这里插入图片描述](https://img-blog.csdnimg.cn/d656b9715e44437faf0a741ca84b23c9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这样mybatis对于数据库的增删查改就结束了;
来做一个小小的总结-----------------<>>>
由以上操做基本也可以看出,mybatis的操做大致可以分为6步;
1,读取配置文件 -----mybatisconfig.xml与Mapper.xml
2加载配置文件并根据配置文件创建sqlsessionFactory
3,根据sqlsessionFactory创建SqlSession
4,由SqlSession负责联系数据库
5,关闭SqlSession


虽然表面上是SqlSession负责连接数据库,其实底层是由excutor和statemenHandler实现的



熟悉了工作流程之后需要深入学习一下

## MyBatis的核心对象-->

### SqlSessionFactory
SqlSessionFactory是单个数据库映射关系经过编译后的内存镜像，通过SqlSessionFactoryBuilder对象来构建,而SqlSessionFactory的实例由mybatis全局配置文件构建
所以
代码就可以写下来了,通过流来读取全局配置文件

Resources的getResourceAsStream方法来读取配置文件,然后通过SqlSessionFactoryBuilder的build来构建一个SqlSessionFactory对象;

![在这里插入图片描述](https://img-blog.csdnimg.cn/6617f75b97154e23a11a3bb286860284.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


> SqlSessionFactory对象是线程安全的，一旦被创建，在整个应用执行期间都会存在。如果多次创建同一个数据库的SqlSessionFactory，那么此数据库的资源将很容易被耗尽。所以在构建SqlSessionFactory实例时，建议使用单列模式,即只实例化一次

### SqlSession

sqlsession中封装了JDBC连接,是应用与数据库之间执行交互的一个单线程对象;

> SqlSession实例是**不能被共享的**，也是**线程不安全的**，因此其使用范围最好限定在**一次请求或一个方法**中，绝不能将其放在一个类的静态字段、实例字段或任何类型的管理范围中使用。使用完SqlSession对象之后，要**及时将它关闭**，通常可以将其放在finally块中关闭。

#### sqlsession中的常用方法

```java
  <T> T selectOne(String statement);
//该方法返回执行SQL语句查询结果的一个泛型对象。
  <T> T selectOne(String statement, Object parameter);
//该方法返回执行SQL语句查询结果的一个泛型对象。
  <E> List<E> selectList(String statement, Object parameter);
//该方法返回执行SQL语句查询结果的泛型对象的集合。
  <E> List<E> selectList(String statement, Object parameter);
//该方法返回执行SQL语句查询结果的泛型对象的集合。
  <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds);
  //rowBounds是用于分页的参数对象。该方法返回执行SQL语句查询结果的泛型对象的集合。

void select(String statement, Object parameter, ResultHandlerhandler);
//ResultHandler对象用于处理查询返回的复杂结果集，通常用于多表查询。

//还有insert,update,delete,commit以及rollback就不在此显示了

```

## MyBatis配置文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/9f0c15ddb58c42638502a18309d84160.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)mybatis配置属性顺序

configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）


```
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```





## mapper配置文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/2e484de5bfe04fff8e0a23a57b9fdad3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

错误演示----->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/b4ac314b74964267936bc2f40003b142.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
在说一下在mybatis中的全局配置文件中的一些小细节
比如上面案例开发中引入了一个外部配置文件jdbc.properties用于变量的一个引入,其实还有另一种操做,跟他很类似,但是不用在另外配置一个文件了;


那我们可以在mybatis配置文件中直接引入一个全局变量------->>


![在这里插入图片描述](https://img-blog.csdnimg.cn/98fdc07699324556b9be108978e3e8c0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
一般开发者能够还是习惯将四要素放在文件(jdbc.properties)外以尽可能降低耦合度;

但是这样就会来一个问题,如果两种方式都配置了,那么是按照谁来走呢?


实际测试后  发现**是按照外部配置的走的,原因是**

myabatis在读取配置时是
先读取内部的properties中的内容,
然后在读取外部resource引入的,如果同名了,
则读取的外部resource的值会覆盖掉内部读取的值;
所以临时修改数据库额连接时只要不同名就可以了;

> 如果一个属性在不只一个地方进行了配置，那么，MyBatis 将按照下面的顺序来加载：
>
> 首先读取在 properties 元素体内指定的属性。 然后根据 properties 元素中的 resource
> 属性读取类路径下属性文件，或根据 url 属性指定的路径读取属性文件，并覆盖之前读取过的同名属性。
> 最后读取作为方法参数传递的属性，并覆盖之前读取过的同名属性。 因此，通过方法参数传递的属性具有最高优先级，resource/url
> 属性中指定的配置文件次之，最低优先级的则是 properties 元素中指定的属性。



同时我们在代码中也可以指定实际要用到的数据库环境----->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/d47f88cefae442a0b5f11e72ef021eb7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 设置默认值

从 MyBatis 3.4.2 开始，你可以为占位符指定一个默认值。

在配置文件中启动默认值这种时候我们可以为某一个配置设置默认值----

```xml
  <property name="org.apache.ibatis.parsing.PropertyParser.enable-default-value" value="true"/>
```

比如我们在jdbc配置文件中只设置了三个值------>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/617a3a2e9908453685041ffbff916048.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 如果你在属性名中使用了 ":" 字符（如：db:username），或者在 SQL 映射中使用了 OGNL 表达式的三元运算符（如： ${tableName != null ? tableName : 'global_constants'}），就需要设置特定的属性来修改分隔属性名和默认值的字符。例如：



```xml
<properties resource="org/mybatis/example/config.properties">
  <!-- ... -->
  <property name="org.apache.ibatis.parsing.PropertyParser.default-value-separator" value="?:"/> <!-- 修改默认值的分隔符 -->
</properties>
```

```xml
<dataSource type="POOLED">
  <!-- ... -->
  <property name="username" value="${db:username?:ut_user}"/>
</dataSource>
```



##    # {} 与${}的区别

在编译ssql语句时我们常常会用到一些参数什么的,在编译时传统jdbc操作时我们习惯于用预编译的方式;

如果不用预编译的方式很可能发生sql注入,所以 # {} 与${}的区别 其实就是预编译和不预编译的区别,下面给一个案例-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/c9658ccfc80346e0b9aa88c174a6a66e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


## 设置自增----->

![在这里插入图片描述](https://img-blog.csdnimg.cn/afda5a86de224a09817cf11a8ea277e3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以单独配置,也可以在全局配置中进行配置;
![在这里插入图片描述](https://img-blog.csdnimg.cn/6d20af82c5a94e95999b5fea5352e59a.png)



# mybatis中配置文件详解

mybatis中的核心配置文件 一个是全局配置文件,另一个是映射文件;


首先来看一下全局配置文件------->>>>
全局配置文件
这是全局配置文件中各个属性的配置顺序,
```xml
<!ELEMENT configuration (properties?, settings?, typeAliases?, typeHandlers?, objectFactory?, objectWrapperFactory?, reflectorFactory?, plugins?, environments?, databaseIdProvider?, mappers?)>
```


configuration 在最外层,不必多说;

其次依次是properties---->>settings---->> typeAliases--->> typeHandlers--->> objectFactory--->> objectWrapperFactory--->>reflectorFactory--->> plugins--->> environments--->>databaseIdProvider--->> mappers 

## properties

```xml
<properties resource="jdbc.properties" >
    <property name="jdbc_url" value="jdbc:mysql://127.0.0.1:3306/gavin?serverTimezone=Asia/Shanghai"/>

</properties>
```
properties标签下可以引入外部配置,也可在标签俩面配置一些全局变量,但为了尽可能降低代码耦合度,还是建议直接在外部配置;

除了在配置文件中配置还可以在java代码中将配置作为参数传入,
mybatis提供了专门的方法----->>


```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, props);

// ... 或者 ...

SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, props);
```

但如果用多种方式都配置了,以哪个为准?
验证后得出的结论------>>>>
myabatis在读取配置时
先读取内部的properties中的内容,
然后在读取外部resource引入的,如果同名了,
则读取的外部resource的值会覆盖掉内部读取的值;
所以临时修改数据库的连接时只要将参数设置为不同名的就可以了;


**官方给出的答案---->>>>**
如果一个属性在不只一个地方进行了配置，那么，MyBatis 将按照下面的顺序来加载：
    首先读取在 properties 元素体内指定的属性。
    然后根据 properties 元素中的 resource 属性读取类路径下属性文件，或根据 url 属性指定的路径读取属性文件，并覆盖之前读取过的同名属性。
    最后读取作为方法参数传递的属性，并覆盖之前读取过的同名属性。

> **因此，通过方法参数传递的属性具有最高优先级，resource/url 属性中指定的配置文件次之，最低优先级的则是 properties 元素中指定的属性。**


可以给某些变量设置默认值了,默认这个功能是不开启的,需要手动开启----->>>

```xml
<properties resource="jdbc.properties" >
    <property name="jdbc_url" value="jdbc:mysql://127.0.0.1:3306/gavin?serverTimezone=Asia/Shanghai"/>
<!--    设置默认值-->
    <property name="org.apache.ibatis.parsing.PropertyParser.enable-default-value" value="true"/>
<!--三目运算符-->
<!--    <property name="org.apache.ibatis.parsing.PropertyParser.default-value-separator" value="?:"/> &lt;!&ndash; 修改默认值的分隔符 &ndash;&gt;-->
</properties>
```


> 小结-----关于properties属性的用法----->>
> 1,可以动态的去配置一些参数,方便代码的维护,减少代码耦合度;
> 2,如果参数配置重名了,那么通过方法传入的优先级最高,其次是resource引入的外配置文件,最后是properties内的配置信息;
> 3,可以开启默认值,实际中也很受用到,主要是用于数据库连接要素的配置等;


## settings
settings是MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为;

 一个配置完整的 settings 元素的示例如下：

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```
cacheEnabled 	 默认 	true
全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。 


lazyLoadingEnabled 		默认	false 
延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态。 

useGeneratedKeys 	默认 False 
允许 JDBC 支持自动生成主键，需要数据库驱动支持。如果设置为 true，将强制使用自动生成主键。尽管一些数据库驱动不支持此特性，但仍可正常工作（如 Derby）。 

defaultFetchSize 		无默认值
为驱动的结果集获取数量（fetchSize）设置一个建议值。此参数只可以在查询设置中被覆盖。 即在查询中也可以设置该参数来替代全局配置

localCacheScope 	 默认SESSION 
MyBatis 利用本地缓存机制（Local Cache）防止循环引用和加速重复的嵌套查询。 默认值为 SESSION，会缓存一个会话中执行的所有查询。 若设置值为 STATEMENT，本地缓存将仅用于执行语句，对相同 SqlSession 的不同查询将不会进行缓存。 	SESSION | STATEMENT 	
这个主要跟SqlSession 生命周期以及安全性有关;

logPrefix
 	指定 MyBatis 增加到日志名称的前缀。 	任何字符串 	
 	
logImpl 	指定 MyBatis 所用日志的具体实现，未指定时将自动查找。 	SLF4J | LOG4J | LOG4J2 | JDK_LOGGING | COMMONS_LOGGING | STDOUT_LOGGING | NO_LOGGING 	
跟日志记录有关

以上几个为常用的配置信息;

[各个配置的含义可以参考官方API](https://mybatis.net.cn/configuration.html#settings)

## typeAliases

类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

可以将类的全限定名简化为我们指定的名,一般为类名,
即在映射文件中我们输入Author即指的是domain.blog.Author

也可以指定一个包中的所有的pojo类,这时会默认用首字母小写的非限定类名来作为它的别名;

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

如果启用了注解,则会按照注解所示的名作为别名

```java
@Alias("author")
public class Author {
}
```

基本数据类型和其对应的包装类都有默认自己的简写,不需要再去自己指定;
他们的命名规律是
基本数据类型  别名为    _小写类名
他们的包装类  别名为     小写类名
int和Integer需要单独记忆
集合类型         别名为      小写类名

[查看API](https://mybatis.net.cn/configuration.html#typeAliases)

## typeHandlers
MyBatis 在设置预处理语句（PreparedStatement）中的参数或从结果集中取出一个值时， 都会用类型处理器将获取到的值以合适的方式转换成 Java 类型

![在这里插入图片描述](https://img-blog.csdnimg.cn/56fe835501ba4884a085e5690ca1fef2.png)
大部分是够用了,个别情况需要自己开发,mybatis也提供了相应的方式---->>
实现TypeHandler 接口， 或继承BaseTypeHandler

参考代码实现------>>>

```java
// ExampleTypeHandler.java
@MappedJdbcTypes(JdbcType.VARCHAR)
public class ExampleTypeHandler extends BaseTypeHandler<String> {

  @Override
  public void setNonNullParameter(PreparedStatement ps, int i, String parameter, JdbcType jdbcType) throws SQLException {
    ps.setString(i, parameter);
  }

  @Override
  public String getNullableResult(ResultSet rs, String columnName) throws SQLException {
    return rs.getString(columnName);
  }

  @Override
  public String getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
    return rs.getString(columnIndex);
  }

  @Override
  public String getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
    return cs.getString(columnIndex);
  }
}
```
自定义之后还需要再配置文件中注册以使其生效


```xml
<!-- mybatis-config.xml -->
<typeHandlers>
  <typeHandler handler="org.mybatis.example.ExampleTypeHandler"/>
</typeHandlers>
```

## objectFactory

每次 MyBatis 创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成实例化工作。 默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认无参构造方法，要么通过存在的参数映射来调用带有参数的构造方法。 如果想覆盖对象工厂的默认行为，可以通过创建自己的对象工厂来实现;--继承DefaultObjectFactory

```java
public class ExampleObjectFactory extends DefaultObjectFactory {
  public Object create(Class type) {
    return super.create(type);
  }
  public Object create(Class type, List<Class> constructorArgTypes, List<Object> constructorArgs) {
    return super.create(type, constructorArgTypes, constructorArgs);
  }
  public void setProperties(Properties properties) {
    super.setProperties(properties);
  }
  public <T> boolean isCollection(Class<T> type) {
    return Collection.class.isAssignableFrom(type);
  }}
```
自定义之后还需要再配置文件中注册以使其生效

ObjectFactory 接口很简单，它包含两个创建实例用的方法，一个是处理默认无参构造方法的，另外一个是处理带参数的构造方法的。 另外，setProperties 方法可以被用来配置 ObjectFactory，在初始化你的 ObjectFactory 实例后， objectFactory 元素体中定义的属性会被传递给 setProperties 方法。
```xml
<!-- mybatis-config.xml -->
<objectFactory type="org.mybatis.example.ExampleObjectFactory">
  <property name="someProperty" value="100"/>
</objectFactory>
```


## plugins
 MyBatis 允许你在映射语句执行过程中的某一点进行拦截调用。默认情况下，MyBatis 允许使用插件来拦截的方法调用包括：

    Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
    ParameterHandler (getParameterObject, setParameters)
    ResultSetHandler (handleResultSets, handleOutputParameters)
    StatementHandler (prepare, parameterize, batch, update, query)




参考拦截代码----->>>
实现原理就是代理模式

```java
@Intercepts({@Signature(
  type= Executor.class,
  method = "update",
  args = {MappedStatement.class,Object.class})})
public class ExamplePlugin implements Interceptor {
  private Properties properties = new Properties();
  public Object intercept(Invocation invocation) throws Throwable {
    // implement pre processing if need
    Object returnObject = invocation.proceed();
    // implement post processing if need
    return returnObject;
  }
  public void setProperties(Properties properties) {
    this.properties = properties;
  }
}
```

自定义之后还需要再配置文件中注册以使其生效

```xml
<plugins>
  <plugin interceptor="org.mybatis.example.ExamplePlugin">
    <property name="someProperty" value="100"/>
  </plugin>
</plugins>
```

## environments
MyBatis 可以配置成适应多种环境,以适应多种需求,比如连接多个数据库;
如果你想连接**两个数据库**，就需要创建**两个 SqlSessionFactory 实例**，每个数据库对应一个。而如果是三个数据库，就需要三个实例，

 每个数据库对应一个 SqlSessionFactory 实例
为了指定创建哪种环境，只要将它作为可选的参数传递给 SqlSessionFactoryBuilder 即可

```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, properties);
```

如果忽略了环境参数，那么将会加载默认环境，如下所示：

```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, properties);
```

```xml
<environments default="development">
        <environment id="mysql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc_driver}"/>
                <property name="url" value="${jdbc_url}"/>
                <property name="username" value="${jdbc_username}"/>
                <property name="password" value="${jdbc_password}"/>

<!--                <property name="三目运算符" value="${db:username?:ut_user}"/>-->

            </dataSource>
        </environment>
        <environment id="PostGreSql">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc_driver}"/>
                <property name="url" value="${jdbc_url}"/>
                <property name="username" value="${jdbc_username}"/>
                <property name="password" value="${jdbc_password:955945}"/>
            </dataSource>
        </environment>
    </environments>
```

>  环境可以随意命名，但务必保证默认的环境 ID 要匹配其中一个环境 ID。

### transactionManager
事务管理器,有两个可以选择----JDBC和MANAGED

如果选则JDBC,则使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域。

MANAGED – 这个配置几乎没做什么。它从**不提交或回滚**一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 **默认情况下它会关闭连接**。如果想复用连接则需要设置一下 closeConnection 属性设置为 false 来阻止默认的关闭行为

```xml
<transactionManager type="MANAGED">
  <property name="closeConnection" value="false"/>
</transactionManager>
```

> [如果使用 Spring + MyBatis，则没有必要配置事务管理器，因为 Spring 模块会使用自带的管理器来覆盖前面的配置。](https://mybatis.net.cn/configuration.html#environments)

### dataSource
数据源的配置,type有三个值可以选择,
[UNPOOLED|POOLED|JNDI]



UNPOOLED
这个数据源的实现会每次请求时打开和关闭连接

```xml
   <environment id="PostGreSql">
            <transactionManager type="JDBC"/>
            <dataSource type="UNPOOLED">
                <property name="driver" value="${jdbc_driver}"/>
                <property name="url" value="${jdbc_url}"/>
                <property name="username" value="${jdbc_username}"/>
                <property name="password" value="${jdbc_password:955945}"/>
                <property name="defaultTransactionIsolationLevel" value="DEFAULT"/>
            <property name="defaultNetworkTimeout" value="10"/>
<!--                给driver添加属性-->
                <property name="driver.encoding" value="UTF-8"/>
            </dataSource>
        </environment>
```


POOLED
这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间

```xml
 <environment id="mysql">
            <transactionManager type="MANAGED"/>
            <dataSource type="UNPOOLED">
                <property name="driver" value="${jdbc_driver}"/>
                <property name="url" value="${jdbc_url}"/>
                <property name="username" value="${jdbc_username}"/>
                <property name="password" value="${jdbc_password:955945}"/>
                <property name="defaultTransactionIsolationLevel" value="DEFAULT"/>
                <property name="defaultNetworkTimeout" value="10"/>
                <property name="poolMaximumActiveConnections" value="10"/><!--最大并发连接数 默认为10-->
                <property name="poolMaximumIdleConnections" value="5"/><!--任意时间可能存在的空闲连接数。-->
                <property name="poolMaximumCheckoutTime"
                          value="20000"/><!--在被强制返回之前，池中连接被检出（checked out）时间，默认值：20000 毫秒（即 20 秒）-->
                <property name="poolTimeToWait" value="20000"/><!--如果获取连接花费了相当长的时间，连接池会打印状态日志并重新尝试获取一个连接（避免在误配置的情况下一直失败且不打印日志），默认值：20000 毫秒（即 20 秒）。
-->

                <property name="poolMaximumLocalBadConnectionTolerance"
                          value="3"/> <!--作用于每一个尝试从缓存池获取连接的线程。 如果这个线程获取到的是一个坏的连接，那么这个数据源允许这个线程尝试重新获取一个新的连接，但是这个重新尝试的次数不应该超过 poolMaximumIdleConnections 与 poolMaximumLocalBadConnectionTolerance 之和。 默认值：3（新增于 3.4.5）-->

                <property name="poolPingQuery" value="NO PING QUERY SET"/><!--发送到数据库的侦测查询，用来检验连接是否正常工作并准备接受请求。默认是“NO PING QUERY SET”，这会导致多数数据库驱动出错时返回恰当的错误消息。
-->
                <property name="poolPingEnabled"
                          value="false"/><!--是否启用侦测查询。若开启，需要设置 poolPingQuery 属性为一个可执行的 SQL 语句（最好是一个速度非常快的 SQL 语句），默认值：false。-->
                <property name="poolPingConnectionsNotUsedFor"
                          value="0"/><!--配置 poolPingQuery 的频率。可以被设置为和数据库连接超时时间一样，来避免不必要的侦测，默认值：0（即所有连接每一时刻都被侦测 — 当然仅当 poolPingEnabled 为 true 时适用）。-->

            </dataSource>
        </environment>
```

### JNDI

为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的数据源引用。这种数据源配置只需要两个属性： 

initial_context – 这个属性用来在 InitialContext 中寻找上下文（即，initialContext.lookup(initial_context)）。这是个可选属性，如果忽略，那么将会直接从 InitialContext 中寻找 data_source 属性。


data_source – 这是引用数据源实例位置的上下文路径。提供了 initial_context 配置时会在其返回的上下文中进行查找，没有提供时则直接在 InitialContext 中查找。

可以通过添加前缀“env.”直接把属性传递给 InitialContext。比如：

    env.encoding=UTF8

这就会在 InitialContext 实例化时往它的构造方法传递值为 UTF8 的 encoding 属性。

你可以通过实现接口 org.apache.ibatis.datasource.DataSourceFactory 来使用第三方数据源实现：

```java
public interface DataSourceFactory {
  void setProperties(Properties props);
  DataSource getDataSource();
}
```

org.apache.ibatis.datasource.unpooled.UnpooledDataSourceFactory 可被用作父类来构建新的数据源适配器，比如下面这段插入 C3P0 数据源所必需的代码：

```java
import org.apache.ibatis.datasource.unpooled.UnpooledDataSourceFactory;
import com.mchange.v2.c3p0.ComboPooledDataSource;

public class C3P0DataSourceFactory extends UnpooledDataSourceFactory {

  public C3P0DataSourceFactory() {
    this.dataSource = new ComboPooledDataSource();
  }
}
```

为了令其工作，记得在配置文件中为每个希望 MyBatis 调用的 setter 方法增加对应的属性。 下面是一个可以连接至 PostgreSQL 数据库的例子：

```xml
<dataSource type="org.myproject.C3P0DataSourceFactory">
  <property name="driver" value="org.postgresql.Driver"/>
  <property name="url" value="jdbc:postgresql:mydb"/>
  <property name="username" value="postgres"/>
  <property name="password" value="root"/>
</dataSource>
```

### databaseIdProvider
根据不同的数据库厂商执行不同的语句，这种多厂商的支持是基于映射语句中的 databaseId 属性
 如果同时找到带有 databaseId 和不带 databaseId 的相同语句，则后者会被舍弃。


```xml
<databaseIdProvider type="DB_VENDOR" />
```

databaseIdProvider 对应的 DB_VENDOR 实现会将 databaseId 设置为 DatabaseMetaData#getDatabaseProductName() 返回的字符串。 由于通常情况下这些字符串都非常长，而且相同产品的不同版本会返回不同的值，你可能想通过设置属性别名来使其变短：

```xml
<databaseIdProvider type="DB_VENDOR">
  <property name="SQL Server" value="sqlserver"/>
  <property name="DB2" value="db2"/>
  <property name="Oracle" value="oracle" />
</databaseIdProvider>
```

在提供了属性别名时，databaseIdProvider 的 DB_VENDOR 实现会将 databaseId 设置为数据库产品名与属性中的名称第一个相匹配的值，如果没有匹配的属性，将会设置为 “null”。 在这个例子中，如果 getDatabaseProductName() 返回“Oracle (DataDirect)”，databaseId 将被设置为“oracle”。

你可以通过实现接口 org.apache.ibatis.mapping.DatabaseIdProvider 并在 mybatis-config.xml 中注册来构建自己的 DatabaseIdProvider：

```java
public interface DatabaseIdProvider {
  default void setProperties(Properties p) { // 从 3.5.2 开始，该方法为默认方法
    // 空实现
  }
  String getDatabaseId(DataSource dataSource) throws SQLException;
}
```

### mappers

既然 MyBatis 的行为已经由上述元素配置完了，我们现在就要来定义 SQL 映射语句了。 但首先，我们需要告诉 MyBatis 到哪里去找到这些语句。 在自动查找资源方面，Java 并没有提供一个很好的解决方案，所以最好的办法是直接告诉 MyBatis 到哪里去找映射文件。 你可以使用相对于类路径的资源引用，或完全限定资源定位符（包括 file:/// 形式的 URL），或类名和包名等。例如：

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>

<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>

<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>

<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

这些配置会告诉 MyBatis 去哪里找映射文件;

## 映射文件mapper详解

mapper映射文件----->>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org/DTD Mapper 3.0" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gavin.mapper">

  <!--  因为有了构造方法了就不用再这定义了
  <resultMap type="UserInfo" id="userData">
        <id property="id" column="f_id"/>
        <result property="name" column="f_name"/>
        <result property="birth" column="f_birth"/>
        <result property="salary" column="f_salary"/>
    </resultMap>-->

    <!-- id要与接口方法名相同 -->
    <!-- SQL语句中的参数名称（#{id}），要与java代码中的参数bean的数据字段相同，这里是UserInfo.id字段 -->
    <!-- type属性可省略 -->

    <sql id="allColumn" >BookId,BookName,BookPublish,BookPrice,BookKind,BookCount</sql>
    <sql id="partColumn" >BookName,BookPublish,BookPrice,BookKind,BookCount</sql>

    <select id="findAllBook" resultType="book">
select <include refid="allColumn"></include>
from bookstore
    </select>
<insert id="addBook" useGeneratedKeys="true" keyProperty="BookId" parameterType="book">
    insert into bookstore (<include refid="partColumn"></include>) values(#{BookName},#{BookPublish},#{BookPrice},#{BookKind},#{BookCount})
</insert>
    <update id="saleBook" parameterType="buyBook">
update bookstore set BookCount =BookCount - #{b_Count} where BookId = #{b_Id}
    </update>
</mapper>
```

通过配置mapper映射文件可以发现,mapper映射文件的顶级元素很少,但是也跟mybatis配置文件一样应该按照顺序进行定义;

cache   设置缓存的;
cache-ref引用缓存空间的;
resultMap设置结果集对象格式的;
sql定义sql语句中可重用元素的;
insert,select,update,delete CURD的标签;

**select标签--->>>>**

![select](https://img-blog.csdnimg.cn/0ddb25f84b1f4c419cb95dfecfd09b41.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> select 元素允许你配置很多属性来配置每条语句的行为细节。


```xml
<select
  id="selectPerson"
  parameterType="int" <!--参数类型-->
  
  resultType="hashmap"<!--结果集的类型-->
  resultMap="personResultMap"
  <!--对外部 resultMap 的命名引用,resultType 和 resultMap 之间只能同时使用一个。-->
  
  flushCache="false" <!--缓存刷新
-->
  useCache="true"使用缓存
  timeout="10" 超时等待
  fetchSize="256"每次取的数据的数量
  statementType="PREPARED"编译类型,可以是statement可以是prepareStatement或者 CallableStatement
  resultSetType="FORWARD_ONLY">
```

insert, update 和 delete实现非常接近：
```xml
<insert
  id="insertAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  keyProperty=""
  keyColumn=""
  useGeneratedKeys=""
  timeout="20">

<update
  id="updateAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">

<delete
  id="deleteAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">
```
useGeneratedKeys	（仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出**由数据库内部生成的主键**（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。

keyProperty	（仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用 getGeneratedKeys 的返回值或 insert 语句的 selectKey 子元素设置它的值，默认值：未设置（unset）。如果生成列不止一个，可以用逗号分隔多个属性名称。
keyColumn	（仅适用于 insert 和 update）设置生成键值在表中的列名，在某些数据库（像 PostgreSQL）中，当主键列不是表中的第一列的时候，是必须设置的。如果生成列不止一个，可以用逗号分隔多个属性名称。



sql
这个元素可以用来定义可重用的 SQL 代码片段，以便在其它语句中使用。 参数可以静态地（在加载的时候）确定下来，并且可以在不同的 include 元素中定义不同的参数值。比如：

```
<sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>
```

这个 SQL 片段可以在其它语句中使用，例如：

```
<select id="selectUsers" resultType="map">
  select
    <include refid="userColumns"><property name="alias" value="t1"/></include>,
    <include refid="userColumns"><property name="alias" value="t2"/></include>
  from some_table t1
    cross join some_table t2
</select>
```

也可以在 include 元素的 refid 属性或内部语句中使用属性值，例如：

```
<sql id="sometable">
  ${prefix}Table
</sql>

<sql id="someinclude">
  from
    <include refid="${include_target}"/>
</sql>

<select id="select" resultType="map">
  select
    field1, field2, field3
  <include refid="someinclude">
    <property name="prefix" value="Some"/>
    <property name="include_target" value="sometable"/>
  </include>
</select>
```

一句话就是sql 语句片段可在 CURD标签的各个位置使用;

在mybatis中如果遇到字段名和类中属性字段不匹配的情况,通常可以有两种解决方案------>>>


一个是在sql语句中添加as 别名

```xml
<select id="selectUsers" resultType="User">
  select
    user_id             as "id",
    user_name           as "userName",
    hashed_password     as "hashedPassword"
  from some_table
  where id = #{id}
</select>
```
另一个是在外部配置映射---->>>

```xml
<resultMap id="userResultMap" type="User">
  <id property="id" column="user_id" />
  <result property="username" column="user_name"/>
  <result property="password" column="hashed_password"/>
</resultMap>
```

然后在引用它的语句中设置 resultMap 属性就行了:

```xml
<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```
在mybatis数据库开发过程中,一开始是先写dao ,pojo然后通过映射文件来完成数据库的操作;

一个简单的案例是---->>>

![](C:\Users\Gavin\Pictures\Camera Roll\66.png)

看起来就这么多,如果你想看一下本教程,可以直接移步-------

[链接](https://github.com/JeEisenberg/daomapper.git)


## mybatis逆向工程

由结果导向配置文件,正常情况下是由我们根据数据库文件编写pojo实体类,而mabatis逆向工程是通过一个mybatis-genertator-core核心包来实现自动生成pojo的一种实现;

下面简单来说一下generrator的配置------>>>
首先是表头---->>>>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
```

表头没有什么好研究的,规定好了,
[配置文件下的基本规则内容](http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd)

配置生成器 

```xml
<generatorConfiguration >
   <properties resource="jdbc.properties"/>
<!--  resource：配置资源加载地址
  url：配置资源加载地址 
  两个选择一个,或者选择不引用直接在下面配置  -->
<context>
具体的配置内容
</context>
</generatorConfiguration>
```

context 标签的含义是生成一组对象的环境,唯一id必不可少

```xml
 <context id="mysql" defaultModelType="flat" targetRuntime="MyBatis3" introspectedColumnImpl="com.Gavin.Tools">
```

 defaultModelType用于指定生成对象的样式,有三个可选值---->>>>

>    1，conditional：如果某张表只有一个字段，则不会生成pojo实体
>
>    2，flat：所有内容（主键，blob）等全部生成在一个对象中；也是最常用的
>
>    3，hierarchical：主键生成一个XXKey对象(key class)，Blob等单独生成一个对象，其他简单属性在一个对象中(record class)
>


>
> **如果表中没有blob,则hierarchical 与flat的效果是一样的**

 targetRuntime:
1，MyBatis3：默认的值，生成基于MyBatis3.x以上版本的内容，包括XXXBySample；

 2，MyBatis3Simple：类似MyBatis3，只是不生成XXXBySample；


 introspectedColumnImpl：值为类全限定名，用于扩展MBG,即引入一个工具类用于扩展MBG




下面的配置基本是默认的,如果遇到某些情况可以修改一下,比如表字段与java关键字冲突时候;

```xml
      <!-- 自动识别数据库关键字，默认false，如果设置为true，根据SqlReservedWords中定义的关键字列表；
            一般保留默认值，遇到数据库关键字（Java关键字），使用columnOverride覆盖
         -->
        <property name="autoDelimitKeywords" value="false"/>
        <!-- 生成的Java文件的编码 -->
        <property name="javaFileEncoding" value="UTF-8"/>
        <!-- 格式化java代码 -->
        <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
        <!-- 格式化XML代码 -->
        <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>

        <!-- beginningDelimiter和endingDelimiter：指明数据库的用于标记数据库对象名的符号，比如ORACLE就是双引号，MYSQL默认是`反引号； -->
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>

```

关于注释的一些配置,当然可以自定义注释,要有类的实现,代码留作日后在讨论;
```xml
 <commentGenerator>
            <!-- 是否去除自动生成的注释 -->
            <property name="suppressAllComments" value="true"/>
            <!-- 生成注释是否带时间戳-->
            <property name="suppressDate" value="true"/>
            <!-- 生成的Java文件的编码格式 -->
            <property name="javaFileEncoding" value="utf-8"/>
            <!-- 格式化java代码-->
            <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
            <!-- 格式化XML代码-->
            <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>
        </commentGenerator>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/989dd7c6c57d42a3a37e15169e7571cf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

java数据库连接,在这里,开头generatorConfiguration可以引入一个外部文件,然后修改数据库时会很方便;这里并没有这么做,你知道就行;等案例的时候会采用外部配置的方式完成;
```xml
    <!-- 数据库连接驱动类,URL，用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://127.0.0.1:3306/gavin?useUnicode=true&amp;characterEncoding=UTF-8"
 userId="gavin" password="123456">
<!-- 这里面可以设置property属性，每一个property属性都设置到配置的Driver上 -->
            <property name="useSSL" value="true"/>
        </jdbcConnection>
        
```



 自定义数据库中数据处理类型,当然前提是需要有类型处理器的实现类或者继承类;

```xml
   <!-- java类型处理器
            用于处理DB中的类型到Java中的类型，默认使用JavaTypeResolverDefaultImpl；
            注意一点，默认会先尝试使用Integer，Long，Short等来对应DECIMAL和 NUMERIC数据类型；
        -->
        <javaTypeResolver type="org.mybatis.generator.internal.types.JavaTypeResolverDefaultImpl">
          <!--
          对一些数据类型进行很好的处理以便节省空间
 true：使用BigDecimal对应DECIMAL和 NUMERIC数据类型
false：默认,
scale>0;length>18：使用BigDecimal;
scale=0;length[10,18]：使用Long；
 scale=0;length[5,9]：使用Integer；
 scale=0;length<5：使用Short；
             -->
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>
```





生成实体类---pojo  生成的位置以及其他配置
```xml
<!-- 生成Domain模型：包名(targetPackage)、位置(targetProject) -->
<javaModelGenerator targetPackage="com.project.business.domain"
                    targetProject="D:/workSpace/project/src/main/java">
    <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
    <property name="enableSubPackages" value="true"/>
    <!-- 设置是否在getter方法中，对String类型字段调用trim()方法-->
    <property name="trimStrings" value="true"/>
</javaModelGenerator>
```
生成xml文件---mapper.xml

```xml
<!-- 生成xml映射文件：包名(targetPackage)、位置(targetProject) -->
        <sqlMapGenerator targetPackage="com.proj....ss.dao" targetProject="D:/workS....">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>
```


生成dao层接口



```xml
 <!-- 生成DAO接口：包名(targetPackage)、位置(targetProject) -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.....dao"
    targetProject="D:.....">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>
```


要操作的表----

```xml
    <!-- 要生成的表：tableName - 数据库中的表名或视图名，domainObjectName - 实体类名 -->
        <table tableName="tableName" domainObjectName="tableNameDO"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false">
  <!--这里也可以处理关键字冲突-->
<columnOverride column="id" javaType="Integer"/>
        </table>

```

当然,上面的配置内容不必全部都配置,挑选自己需要的就好了;


下面是我自己配置的

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<!-- 配置生成器 -->
<generatorConfiguration >
<!--引入外部配置 -->
    <properties resource="jdbc.properties"/>
    <!--    没有扩展的功能,所以不配置introspectedColumnImpl了-->
    <context id="mysql" defaultModelType="flat" targetRuntime="MyBatis3">

        <!--        全局配置-->
        <!--        自动划界关键字-->
        <property name="autoDelimitKeywords" value="false"/>
        <!-- 生成的Java文件的编码 -->
        <property name="javaFileEncoding" value="UTF-8"/>
        <!-- 格式化java代码 -->
        <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
        <!-- 格式化XML代码 -->
        <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>

        <!-- beginningDelimiter和endingDelimiter：指明数据库的用于标记数据库对象名的符号，比如ORACLE就是双引号，MYSQL默认是`反引号； -->
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>

        <!--注释操作,这里选择不生成注释-->
        <commentGenerator>
            <!--            局部配置-->
            <!-- 如果选择false,下面两个就可以不写-->
            <property name="addRemarkComments" value="true"/>
            <!-- 是否去除自动生成的注释 -->
            <property name="suppressAllComments" value="true"/>
            <!-- 生成注释是否带时间戳-->
            <property name="suppressDate" value="false"/>
        </commentGenerator>

        <!--   jdbc驱动配置-->
        <jdbcConnection driverClass="${jdbc_driver}"
                        connectionURL="${jdbc_url}"
                        userId="${jdbc_username}" password="${jdbc_password}">
            <property name="useSSL" value="false"/>
            <!-- 这里面可以设置property属性，每一个property属性都设置到配置的Driver上 -->
        </jdbcConnection>

        <!-- java类型处理器
            用于处理DB中的类型到Java中的类型，默认使用JavaTypeResolverDefaultImpl；
            注意一点，默认会先尝试使用Integer，Long，Short等来对应DECIMAL和 NUMERIC数据类型；
        -->
        <javaTypeResolver type="org.mybatis.generator.internal.types.JavaTypeResolverDefaultImpl">
            <!--
                true：使用BigDecimal对应DECIMAL和 NUMERIC数据类型
                false：默认,
                    scale>0;length>18：使用BigDecimal;
                    scale=0;length[10,18]：使用Long；
                    scale=0;length[5,9]：使用Integer；
                    scale=0;length<5：使用Short；
             -->
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>


        <!-- java模型创建器，是必须要的元素
            负责：1，key类（见context的defaultModelType）；2，java类；3，查询类
            targetPackage：生成的类要放的包，真实的包受enableSubPackages属性控制；
            targetProject：目标项目，指定一个存在的目录下，生成的内容会放到指定目录中，如果目录不存在，MBG不会自动建目录
         -->

        <!--生成实体类-->
        <javaModelGenerator targetPackage="com.gavin.pojo" targetProject="./src/main/java">
            <!--  for MyBatis3/MyBatis3Simple
                自动为每一个生成的类创建一个构造方法，构造方法包含了所有的field；而不是使用setter；
             -->
            <property name="constructorBased" value="false"/>

            <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
            <property name="enableSubPackages" value="true"/>

            <!-- for MyBatis3 / MyBatis3Simple
                是否创建一个不可变的类，如果为true，
                那么MBG会创建一个没有setter方法的类，取而代之的是类似constructorBased的类
             -->
            <property name="immutable" value="false"/>

            <!-- 设置一个父类对象，
                如果设置了这个根对象，那么生成的keyClass或者recordClass会继承这个类；在Table的rootClass属性中可以覆盖该选项
                注意：如果在key class或者record class中有root class相同的属性，MBG就不会重新生成这些属性了，包括：
                    1，属性名相同，类型相同，有相同的getter/setter方法；
             -->
            
            <property name="rootClass" value="com.gavin.pojo.BaseBook"/>
            <!-- 设置是否在getter方法中，对String类型字段调用trim()方法 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>


        <!-- 生成SQL map的XML文件生成器，
            注意，在Mybatis3之后，我们可以使用mapper.xml文件+Mapper接口（或者不用mapper接口），
                或者只使用Mapper接口+Annotation，所以，如果 javaClientGenerator配置中配置了需要生成XML的话，这个元素就必须配置
            targetPackage/targetProject:同javaModelGenerator
         -->
        <sqlMapGenerator targetPackage="com.gavin.mapper" targetProject="./src/main/resources">
            <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>


        <table tableName="bookstore" domainObjectName="Book"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false">
        </table>
    </context>
</generatorConfiguration>
```

下面写测试类以生成需要的数据----

```java
package GavinTest;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.exception.InvalidConfigurationException;
import org.mybatis.generator.exception.XMLParserException;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.io.File;
import java.io.IOException;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

public class test {
    public static void main(String[] args) {
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        String Cfg = "C:\\Users\\Gavin\\IdeaProjects\\comgavinreverse\\src\\main\\resources\\Generator-Config.xml";

        File configFile = new File(Cfg);
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = null;
        try {
            config = cp.parseConfiguration(configFile);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (XMLParserException e) {
            e.printStackTrace();
        }
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = null;
        try {
            myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        } catch (InvalidConfigurationException e) {
            e.printStackTrace();
        }
        try {
            myBatisGenerator.generate(null);
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}

```

在生成时出现了一个错误,-----
![在这里插入图片描述](https://img-blog.csdnimg.cn/de68357401984d1f9de3d77e244bc0a2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


原来是设置了继承了一个父类,
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6ee3c97f8ff47a88d6cbee683074916.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
如果设置了该项,那么父类中有的属性不会在子类中再去生成了;


同时我们还可指定全参构造方法是否生成;
![在这里插入图片描述](https://img-blog.csdnimg.cn/0a9943ff20cc4a4fb2702755fc8dbac7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


最后打印日志----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/b7635a30344b4d58aa12e38a140e3c01.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


配置文件可以使得不生成映射文件,但是这种方式会使得代码维护修改sql,就比较....
即通过如下配置---->>>>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/3a296c4dc61a4c2da426fe7752b7b161.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
一般是采用用混合的模式----即如下方式---
![在这里插入图片描述](https://img-blog.csdnimg.cn/4b19315c67d04b44a32de082064d78c1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

生成的目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/24016d77f04b4be58fba2a86fbb1eb63.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
源代码放在这里了------>>>>



[就是这里](https://gitee.com/varyu-program/reverse-mybatis.git)

对了,放一个标准的逆向工程配置文件供参考------->>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<!-- 配置生成器 -->
<generatorConfiguration >
    <properties resource="jdbc.properties"/>
    <!--    没有扩展的功能,所以不配置introspectedColumnImpl了-->
    <context id="mysql" defaultModelType="flat" targetRuntime="MyBatis3">

        <!--        全局配置-->
        <!--        自动划界关键字-->

        <property name="autoDelimitKeywords" value="false"/>
        <!-- 生成的Java文件的编码 -->
        <property name="javaFileEncoding" value="UTF-8"/>
        <!-- 格式化java代码 -->
        <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
        <!-- 格式化XML代码 -->
        <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>

        <!-- beginningDelimiter和endingDelimiter：指明数据库的用于标记数据库对象名的符号，比如ORACLE就是双引号，MYSQL默认是`反引号； -->
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>

        <!--注释操作,这里选择不生成注释-->
        <commentGenerator>
            <!--            局部配置-->
            <!-- 如果选择false,下面两个就可以不写-->
            <property name="addRemarkComments" value="true"/>
            <!-- 是否去除自动生成的注释 -->
            <property name="suppressAllComments" value="true"/>
            <!-- 生成注释是否带时间戳-->
            <property name="suppressDate" value="true"/>
        </commentGenerator>

        <!--   jdbc驱动配置-->
        <jdbcConnection driverClass="${jdbc_driver}"
                        connectionURL="${jdbc_url}"
                        userId="${jdbc_username}" password="${jdbc_password}">
            <property name="useSSL" value="false"/>
            <!-- 这里面可以设置property属性，每一个property属性都设置到配置的Driver上 -->
        </jdbcConnection>

        <!-- java类型处理器
            用于处理DB中的类型到Java中的类型，默认使用JavaTypeResolverDefaultImpl；
            注意一点，默认会先尝试使用Integer，Long，Short等来对应DECIMAL和 NUMERIC数据类型；
        -->
        <javaTypeResolver type="org.mybatis.generator.internal.types.JavaTypeResolverDefaultImpl">
            <!--
                true：使用BigDecimal对应DECIMAL和 NUMERIC数据类型
                false：默认,
                    scale>0;length>18：使用BigDecimal;
                    scale=0;length[10,18]：使用Long；
                    scale=0;length[5,9]：使用Integer；
                    scale=0;length<5：使用Short；
             -->
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>


        <!-- java模型创建器，是必须要的元素
            负责：1，key类（见context的defaultModelType）；2，java类；3，查询类
            targetPackage：生成的类要放的包，真实的包受enableSubPackages属性控制；
            targetProject：目标项目，指定一个存在的目录下，生成的内容会放到指定目录中，如果目录不存在，MBG不会自动建目录
         -->

        <!--生成实体类-->
        <javaModelGenerator targetPackage="com.gavin.pojo" targetProject="./src/main/java">
            <!--  for MyBatis3/MyBatis3Simple
                自动为每一个生成的类创建一个构造方法，构造方法包含了所有的field；而不是使用setter；
             -->
            <property name="constructorBased" value="true"/>

            <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
            <property name="enableSubPackages" value="true"/>

            <!-- for MyBatis3 / MyBatis3Simple
                是否创建一个不可变的类，如果为true，
                那么MBG会创建一个没有setter方法的类，取而代之的是类似constructorBased的类
             -->
            <property name="immutable" value="false"/>

            <!-- 设置一个父类对象，
                如果设置了这个根对象，那么生成的keyClass或者recordClass会继承这个类；在Table的rootClass属性中可以覆盖该选项
                注意：如果在key class或者record class中有root class相同的属性，MBG就不会重新生成这些属性了，包括：
                    1，属性名相同，类型相同，有相同的getter/setter方法；
             -->

            <property name="rootClass" value="com.gavin.pojo.BaseBook"/>
            <!-- 设置是否在getter方法中，对String类型字段调用trim()方法 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>



<!--        生成mapper-->
        <sqlMapGenerator targetPackage="com.gavin.mapper" targetProject="./src/main/resources">
            <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

<!--  生成mapper与Dao-->
        <javaClientGenerator targetPackage="com.gavin.dao" type="XMLMAPPER" targetProject="./src/main/java">
            <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
            <property name="enableSubPackages" value="true"/>

            <!-- 可以为所有生成的接口添加一个父接口，但是MBG只负责生成，不负责检查
            <property name="rootInterface" value=""/>
             -->
        </javaClientGenerator>

        <table tableName="bookstore" domainObjectName="Book"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false">
        </table>

    </context>

</generatorConfiguration>
```

## mapper开发规范

  1. Mapper接口的名和对应的Mapper.xml映射文件的名必须一致。
  2. Mapper.xml文件中的namespace与Mapper接口的类路径相同（即接口文件和映射文件需要放在同一个包中）。
  3. Mapper接口中的方法名和Mapper.xml中定义的每个执行语句的id相同。
  4. Mapper接口中方法的输入参数类型要和Mapper.xml中定义的每个SQL的parameterType的类型相同。
  5. Mapper接口方法的输出参数类型要和Mapper.xml中定义的每个SQL的resultType的类型相同。

# 动态 SQL

动态 SQL 是 MyBatis 的强大特性之一

> 在 MyBatis 之前的版本中，需要花时间了解大量的元素。借助功能强大的基于 OGNL 的表达式，MyBatis 3
> 替换了之前的大部分元素，大大精简了元素种类，现在要学习的元素种类比原来的一半还要少。


主要的动态sql标签-----
> 　<if>：判断语句，用于单条件分支判断。　<choose>(<when>、<otherwise>)：用于多条件分支判断。　<where>、<trim>、<set>：辅助元素，用于处理一些SQL拼装、特殊字符问题。　<foreach>：循环语句，常用于in语句等列举条件中。　<bind>：从OGNL表达式中创建一个变量，并将其绑定到上下文，常用于模糊查询的SQL中。


准备案例,
三步走----->>.
创建数据库,建立数据表,如果你之前就有了的话,可以直接拿来用;

建数据库,建立表,插入数据

```sql

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for bookstore
-- ----------------------------
DROP TABLE IF EXISTS `bookstore`;
CREATE TABLE `bookstore`  (
  `BookId` int(0) NOT NULL AUTO_INCREMENT,
  `BookName` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  `BookPublish` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  `BookPrice` double(10, 2) NULL DEFAULT NULL,
  `BookKind` varchar(16) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  `BookCount` int(0) NULL DEFAULT NULL,
  PRIMARY KEY (`BookId`) USING BTREE,
  INDEX `name_index`(`BookName`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 1112 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of bookstore
-- ----------------------------
INSERT INTO `bookstore` VALUES (1001, 'java入门', '清华出版社', 56.80, '计算机', 90);
INSERT INTO `bookstore` VALUES (1002, 'python深入', '机械工业出版社', 78.60, '计算机', 130);
INSERT INTO `bookstore` VALUES (1003, '前端html', '清华出版社', 88.80, '计算机', 100);
INSERT INTO `bookstore` VALUES (1004, '废都', '张江出版社', 25.70, '文学名著', 100);
INSERT INTO `bookstore` VALUES (1005, '官场现形记', '青岛出版社', 23.80, '文学名著', 100);
INSERT INTO `bookstore` VALUES (1006, '红楼梦', '青岛出版社', 36.80, '文学名著', 100);
INSERT INTO `bookstore` VALUES (1007, '母猪的产后护理', '机械工业出版社', 27.80, '实用技术', 100);
INSERT INTO `bookstore` VALUES (1008, '车辆维修大全', '机械工业出版社', 26.80, '实用技术', 100);
INSERT INTO `bookstore` VALUES (1009, '如何讨取富婆欢心富婆', '开心出版社', 125.00, '生活技能', 100);
INSERT INTO `bookstore` VALUES (1010, 'JVM调优', '北京科技出版社', 68.90, '计算机', 120);
INSERT INTO `bookstore` VALUES (1011, 'GO', '南京邮电出版社', 78.90, '计算机', 120);
INSERT INTO `bookstore` VALUES (1012, 'Android入门', '电子科技出版社', 88.90, '计算机', 120);
INSERT INTO `bookstore` VALUES (1014, '果树种植学', '西北农林出版社', 128.90, '农业', 120);
INSERT INTO `bookstore` VALUES (1015, '笑话', '上海出版社', 48.90, '生活', 120);
INSERT INTO `bookstore` VALUES (1111, 'MQ消息队列', '北大出版社', 98.90, '计算机', 120);
INSERT INTO `bookstore` VALUES (1222, 'java大数据', '清华出版社', 124.70, '计算机', 160);
INSERT INTO `bookstore` VALUES (1233, 'java集合', '电子科技出版社', 120.80, '计算机', 80);

SET FOREIGN_KEY_CHECKS = 1;

```
通过逆向工程生成所需要的数据---->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/fb809972aa0b448288fc9cf40a214774.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
[参考逆行工程----->>](https://blog.csdn.net/weixin_54061333/article/details/121660924)
接下来根据生成的数据开始编写sql

## if 
![在这里插入图片描述](https://img-blog.csdnimg.cn/a1b8a6d3b94d43e29854cc13fef76d43.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)先说一下查询目标----->>找到小于指定数量的书,另一个不确定的条件是书的名字,有可能有,也有可能没有;
当传入 模糊查询条件时,会返回相应的结果集,否则会返回 所有小于指定数量的的结果;

设想如果换成java代码来实现判断的话,有一些繁琐;
运行后----->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/b2d1a7e681674f0cb525cdc7eea31a2d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)使用动态 SQL 中的if最常见情景是根据条件包含 where 子句的一部中;

来查看一下生成的sql语句长什么样子
![在这里插入图片描述](https://img-blog.csdnimg.cn/2ed8b25006904c3f8476842e6ea711ec.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以看到,当传入的参数符合要求时才会执行if里面的sql条件,否则就不会;

if 代码片段
```xml
  <select id="selectBook" resultType="com.gavin.pojo.Book">
    select <include refid="Base_Column_List"></include>
    from bookstore where BookCount &lt;= #{bookcount}
    <if test="bookname !=null">
      and BookName like concat('%',#{bookname},'%')
    </if>
  </select>
```


有时候我们并不像所有的条件都是用,而是只要满足这里条件中的一个就可以了,或者是传入什么参数就用什么参数查找或者修改(就比如上条查询,如果传入的是出版社,那么就按出版社进行模糊查询若是种类,则按种类查询);
这个时候就要用到了choose来实现类似功能了;


## choose (when, otherwise)
![在这里插入图片描述](https://img-blog.csdnimg.cn/df561036bfa74cb08bed7e4e546c24fc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

运行结果![在这里插入图片描述](https://img-blog.csdnimg.cn/8adfdd03c4894e6a824e3962f5d1f375.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这就很类似于java中的switch和前端jstl中的choose

万一我们一个参数不传入的话?
我们还可以给他设置一个默认值------>>


![在这里插入图片描述](https://img-blog.csdnimg.cn/26486c50cbc54f84bdcc04ef44baaef4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)choose代码片段----->>


```xml
 <select id="selectBookBySth" resultType="book">
select <include refid="Base_Column_List"></include>

from bookstore where 1=1

<choose>
    <when test="bookname != null">
        and BookName like concat('%',#{bookname},'%')
    </when>
    <when test="bookpublish  != null">
        and BookPublish like concat('%',#{bookpublish},'%')
    </when>
    <when test="bookkind  != null" >
        and BookKind like concat('%',#{bookkind},'%')
    </when>
    <otherwise>
       and  BookPrice &gt;= 100
    </otherwise>
</choose>
    </select>
```

但是我们可能会遇到过,如果没有查询条件即上述choose中没有 1=1这个条件,那么当查询条件为null时就会出现 一下的一种情况

```sql
select * from bookstore where   and  BookPrice &gt;= 100
```

查询语句出错了,所以我在上面的choose案例中添加了 1=1这个恒成立的条件;

假设不加的话,那么当条件为null时该怎么处理呢?
mybatis提供了一个方法,就是在  查询条件时外部嵌套where标签

就像下面这个样子
![在这里插入图片描述](https://img-blog.csdnimg.cn/88cf4e0b2a404d96a90c91149b6eabfb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> where 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，where 元素也会将它们去除。


如果where元素还不能满足我们的而要求,那么还可通过自定义trim 元素来定制where标签实现我们想要的;

如上案例,和 where 元素等价的自定义 trim 元素



## trim (where, set)

**where**
由于我们是对where进行自定义的,所以prefix处写where
![在这里插入图片描述](https://img-blog.csdnimg.cn/2f2f2d7e5ae2487898cfac785c210499.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

trim---where代码片段

```xml
 <select id="selectBookBySth" resultType="book">
select <include refid="Base_Column_List"></include>
from bookstore
  <trim prefix="WHERE" prefixOverrides="AND |OR ">
      <choose>
        <when test="bookname != null">
          BookName like concat('%',#{bookname},'%')
        </when>
        <when test="bookpublish  != null">
          and BookPublish like concat('%',#{bookpublish},'%')
        </when>
        <when test="bookkind  != null" >
          and BookKind like concat('%',#{bookkind},'%')
        </when>
        <otherwise>
          and  BookPrice &gt;= 100
        </otherwise>
      </choose>
  </trim>
    </select>
```

><trim元素的作用是去除一些特殊的字符串，它的prefix属性代表的是语句的前缀（这里使用where来连接后面的SOL片段），而prefixOverrides属性代表的是需要去除的那些特殊字符串（这里定义了要去除SQL中的and）

**set标签用于动态更新**

![在这里插入图片描述](https://img-blog.csdnimg.cn/f52434c5f2d24242a826598cbaa06c19.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




比如我们并不想要更新某条记录的全部数据,那么我们就可通过set来动态的更新
例如想要更新BookId为1111的数据,更改内容为书价格;

![在这里插入图片描述](https://img-blog.csdnimg.cn/5b8edd16e5b1496492a29bc9659b5e87.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
但是我们有时候会要更改的数据不是固定的,这时候用set来解决




```xml


  <update id="updateByPrimaryKeySelective" parameterType="com.gavin.pojo.Book">
    update bookstore
    <set>
      <if test="bookname != null">
        BookName = #{bookname,jdbcType=VARCHAR},
      </if>
      <if test="bookpublish != null">
        BookPublish = #{bookpublish,jdbcType=VARCHAR},
      </if>
      <if test="bookprice != null">
        BookPrice = #{bookprice,jdbcType=DOUBLE},
      </if>
      <if test="bookkind != null">
        BookKind = #{bookkind,jdbcType=VARCHAR},
      </if>
      <if test="bookcount != null">
        BookCount = #{bookcount,jdbcType=INTEGER},
      </if>
    </set>
    where BookId = #{bookid,jdbcType=INTEGER}
  </update>
```

**与 set 元素等价的自定义 trim 元素：**

```xml
  <update id="updateByPrimaryKeySelective" parameterType="com.gavin.pojo.Book">
    update bookstore

    <trim prefix="SET" suffixOverrides=",">

      <if test="bookname != null">
        BookName = #{bookname,jdbcType=VARCHAR},
      </if>
      <if test="bookpublish != null">
        BookPublish = #{bookpublish,jdbcType=VARCHAR},
      </if>
      <if test="bookprice != null">
        BookPrice = #{bookprice,jdbcType=DOUBLE},
      </if>
      <if test="bookkind != null">
        BookKind = #{bookkind,jdbcType=VARCHAR},
      </if>
      <if test="bookcount != null">
        BookCount = #{bookcount,jdbcType=INTEGER},
      </if>
  </trim>

  where BookId = #{bookid,jdbcType=INTEGER}
  </update>
```
set 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号（这些逗号是在使用条件语句给列赋值时引入的）。



## foreach
动态 SQL 的另一个常见使用场景是对集合进行遍历（尤其是在构建 IN 条件语句的时候）;

例如我们想要在计算机和文学名著中中查找书价格小于100的,


先看foreach用法
![在这里插入图片描述](https://img-blog.csdnimg.cn/7f8451cf737f4aecab40422359a268f5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以将任何可迭代对象（如 List、Set 等）、Map 对象或者数组对象作为集合参数传递给 foreach。当使用可迭代对象或者数组时，index 是当前迭代的序号，item 的值是本次迭代获取到的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。


foreach代码片段


```xml
<select id="findBook" resultType="book">
  select <include refid="Base_Column_List"></include>

  from bookstore where BookKind in
  <foreach collection="list" item="bookkind" index="index" open="(" separator="," close=")">
    #{bookkind}
  </foreach>

</select>
```
在使用<foreach>时，最关键、最容易出错的就是collection属性，该属性是必须指定的，而且在不同情况下该属性的值是不一样的，主要有以下3种情况。　

如果传入的是单参数且参数类型是**一个数组或者List**的时候，collection属性值分别为**array、list（或collection）。**　

如果传入的参数**有多个**，就需要把它们封装成一**个Map**，当然单参数也可以封装成Map集合，这时collection属性值就为Map的键。　

如果传入的参数是**POJO包装类**，collection属性值就为该包装类中需要进行遍历的数组或集合的属性名。


## 在带注解的接口类中使用动态sql

![在这里插入图片描述](https://img-blog.csdnimg.cn/07a3cc11b2454b49b3bf05a8ccc62f7f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3bff9ece097849debffadae632485de6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

当查询语句比较复杂时像动态sql,看起来注解比较糟糕,需要处理单引号和双引号的问题,
而且代码也不美观;

如果像是 这样的sql语句   select * from bookstore ,这样看起来还行;


> 还有一点要注意的时,如果是传入的数组,那么在映射文件中就需要注意了,参数类型哪里写list,在遍历的时候collection处写array

![在这里插入图片描述](https://img-blog.csdnimg.cn/979df01718e7449584f9319cb84b3a35.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

刚刚想起,map是很重要的一个集合,不能落下,于是乎,将上面的查询改成map形式的


首先,在mapper层定义好查询方法---->>
查询某一种类中的书籍
![在这里插入图片描述](https://img-blog.csdnimg.cn/a1af5d6a120c4a6bb7628881eed246ef.png)




再然后是mapper映射文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/4dbfc66951a94ffc86f1dca2c84ead64.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

测试代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/ad498a2260e440a69b158df8cb63ea43.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**当多条件的复杂查询时可以用map来实现;**




说一下bind标签
bind 元素允许你在 OGNL 表达式以外创建一个变量，并将其绑定到当前的上下文。

例如这样:-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/6c0fd1516e68414a92368ad886b10f14.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/1ca03da0dbaf46f284a71a87fa8bdc0e.png)



## mybatis对多数据库的支持

如果配置了 databaseIdProvider，你就可以在动态代码中使用名为 “_databaseId” 的变量来为不同的数据库构建特定的语句。

```xml
<insert id="insert">
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    <if test="_databaseId == 'oracle'">
      select * from bookstore
    </if>
    <if test="_databaseId == 'db2'">
       select * from bookstore
    </if>
  </selectKey>
  insert into bookstore values (#{bookname}, #{bookprice})
</insert>
```



小结----->>>


我们在实际操做数据库的时候经常会遇到多个参数的时候,


**一个参数时很好解决,多个参数时,可以用集合list/map的形式去实现**;



*map key放字段, value 放条件*


## mybatis参数传递

基本数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/814feba55be64a679d2d9b47767523d5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


方式一----

用arg来传递参数,注意从0开始,0表示第一个
```xml
    <select id="findPartBook" resultType="book">
        select * from bookstore  where bookkind=#{arg0} and bookprice &lt;= #{arg1}
    </select>
```



方式二---

用param来传递参数,注意从1开始,1表示第一个
```xml
    <select id="findPartBook" resultType="book">
    select * from bookstore  where bookkind=#{param1} and bookprice &lt;= #{param2}
    </select>
```

还可以为参数设置别名
接口中方法名
```java
 List<Book> findPartBook(@Param("bookkind") String BookKind ,@Param("bookprice") Double BookPrice);
```

```xml
    <select id="findPartBook" resultType="book">
<!--        select * from bookstore  where bookkind=#{arg0} and bookprice &lt;= #{arg1}-->
<!--        select * from bookstore  where bookkind=#{param1} and bookprice &lt;= #{param2}-->
        select * from bookstore  where bookkind=#{bookkind} and bookprice &lt;= #{bookprice}
    </select>
```

方式三----
通过集合的形式传入,
接口方法--

```java
  List<Book> findPartBook(Map<String,Object> book1);
```

```xml
    <select id="findPartBook" resultType="book"  parameterType="map">
<!--        select * from bookstore  where bookkind=#{arg0} and bookprice &lt;= #{arg1}-->
<!--        select * from bookstore  where bookkind=#{param1} and bookprice &lt;= #{param2}-->
        select * from bookstore  where bookkind=#{bookkind} and bookprice &lt;= #{bookprice}
    </select>
```

用集合时情况也可以起别名;

```xml
 <select id="findPartBook" resultType="book"  parameterType="map">
<!--        select * from bookstore  where bookkind=#{arg0} and bookprice &lt;= #{arg1}-->
<!--        select * from bookstore  where bookkind=#{param1} and bookprice &lt;= #{param2}-->
        select * from bookstore  where bookkind=#{B.bookkind} and bookprice &lt;= #{B.bookprice}
    </select>
```
小结--->>


参数为基本类型------->>>
1,参数为基本参数类型,用arg 和param都可以;


2,如果给参数起了别名.那么arg的形式传入参数会失效;



参数为引用类型---->>
1参数为单个的引用类型,那么在传入参数时,要写对应的参数属性名;

2,参数为多个引用参数时也是;
例如---->>>

```java
   List<Book> findPartBook(@Param("B") Map<String,Object> book1,@Param("C") Map<String,Double>book2);

```

```xml
    <select id="findPartBook" resultType="book"  parameterType="map">
<!--        select * from bookstore  where bookkind=#{arg0} and bookprice &lt;= #{arg1}-->
<!--        select * from bookstore  where bookkind=#{param1} and bookprice &lt;= #{param2}-->
<!--        select * from bookstore  where bookkind=#{B.bookkind} and bookprice &lt;= #{C.bookprice}-->
        select * from bookstore  where bookkind=#{B.bookkind} and bookprice &lt;= #{C.bookprice}
    </select>

```

```java
Map<String,Object> map= new HashMap<>();
Map<String,Double> map1= new HashMap<>();
map.put("bookkind","计算机" );
map1.put("bookprice",100.0);
        List<Book> list = mapper.findPartBook(map,map1);
        for (Book book : list) {
            System.out.println(book);
        }
    }

```


![在这里插入图片描述](https://img-blog.csdnimg.cn/3b8c2dc89eff420683cc5c0e058f29ef.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

在不起别名的情况下---->> arg 和param都可以;

```java
  List<Book> findPartBook( Map<String,Object> book1, Map<String,Double>book2);

```

```xml
select * from bookstore  where bookkind=#{arg0.bookkind} and bookprice &lt;= #{arg1.bookprice}
```

```xml
select * from bookstore  where bookkind=#{param1.bookkind} and bookprice &lt;= #{param2.bookprice}
```
# Mapper找不到的问题



很多时候我们会遇到这个问题,看起来并没有什么错误,但是就是找不到
![在这里插入图片描述](https://img-blog.csdnimg.cn/3f511f57fbd746c38410884bca1695d5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
报错---java.io.IOException: Could not find resource 
![在这里插入图片描述](https://img-blog.csdnimg.cn/4208bd3017b54417a1314e414fb2da38.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

问题排查------>>
1,将路径换成绝对路径

![在这里插入图片描述](https://img-blog.csdnimg.cn/5c7510c869e84de39d72fe3dbd73475b.png)
依旧报错



2,检查打包好的文件,看看里面有没有配置文件,有时候idea会犯这个问题,

![在这里插入图片描述](https://img-blog.csdnimg.cn/a4c21076a40f4984a5205644def89994.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

里面没有,手动将配置文件添加到这里,但是依旧报错


![](https://img-blog.csdnimg.cn/2a8f7c43d29f474699e083edcca530e6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
按理说没有什么问题,但是百密一疏,问题出在pom配置文件上,,打包成pom,当然配置文件并不会被打包进target里了,改成jar问题就解决了;



![在这里插入图片描述](https://img-blog.csdnimg.cn/1e93d18666964f36941a988da26fd342.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



问题是得到了解决,但是由于这是一个父工程.如果改为jar则clean的时候又会出现异常

![在这里插入图片描述](https://img-blog.csdnimg.cn/34e2712d574449578c3492e79304e579.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




所以要养成一个习惯------新建的maven父工程之后,要在其下面新建一个子模块,以避免出现这样的错误发生;





排除真的是路径写错了的问题!!



@[TOC](mybatis多表查询)

# 多表查询

## 一对一查询

我们通常在查询数据库时一般是一个实体类对应一张表,但是如果查询的字段不在一张表中该怎么办呢?
先看一个案例吧,从头开始捋------>>
由于是对数据库进行操作,先搞一下数据库,



备好以下两张表,当然你也可用自己的其他表,


```sql
DROP TABLE IF EXISTS `dept`;

CREATE TABLE `dept` (
`DEPTNO` int(2) NOT NULL,
`DNAME` varchar(14) DEFAULT NULL,
`LOC` varchar(13) DEFAULT NULL,
PRIMARY KEY (`DEPTNO`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
INSERT INTO `dept` VALUES ('10', 'ACCOUNTING', 'NEW YORK');
INSERT INTO `dept` VALUES ('20', 'RESEARCH', 'DALLAS');
INSERT INTO `dept` VALUES ('30', 'SALES', 'CHICAGO');
INSERT INTO `dept` VALUES ('40', 'OPERATIONS', 'BOSTON');


DROP TABLE IF EXISTS `emp`;

CREATE TABLE `emp` (
`EMPNO` int(4) NOT NULL,
`ENAME` varchar(10),
`JOB` varchar(9),
`MGR` int(4),
`HIREDATE` date,
`SAL` int(7),
`COMM` int(7),
`DEPTNO` int(2),
PRIMARY KEY (`EMPNO`),
KEY `FK_DEPTNO` (`DEPTNO`),
CONSTRAINT `FK_DEPTNO` FOREIGN KEY (`DEPTNO`) REFERENCES `dept` (`DEPTNO`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;




INSERT INTO `emp` VALUES ('7369', 'SMITH', 'CLERK', '7902', '1980-12-17', '800', null, '20');
INSERT INTO `emp` VALUES ('7499', 'ALLEN', 'SALESMAN', '7698', '1981-02-20', '1600', '300', '30');
INSERT INTO `emp` VALUES ('7521', 'WARD', 'SALESMAN', '7698', '1981-02-22', '1250', '500', '30');
INSERT INTO `emp` VALUES ('7566', 'JONES', 'MANAGER', '7839', '1981-04-02', '2975', null, '20');
INSERT INTO `emp` VALUES ('7654', 'MARTIN', 'SALESMAN', '7698', '1981-09-28', '1250', '1400', '30');
INSERT INTO `emp` VALUES ('7698', 'BLAKE', 'MANAGER', '7839', '1981-05-01', '2850', null, '30');
INSERT INTO `emp` VALUES ('7782', 'CLARK', 'MANAGER', '7839', '1981-06-09', '2450', null, '10');
INSERT INTO `emp` VALUES ('7788', 'SCOTT', 'ANALYST', '7566', '1987-04-19', '3000', null, '20');
INSERT INTO `emp` VALUES ('7839', 'KING', 'PRESIDENT', null, '1981-11-17', '5000', null, '10');
INSERT INTO `emp` VALUES ('7844', 'TURNER', 'SALESMAN','7698', '1981-09-08', '1500', '0', '30');
INSERT INTO `emp` VALUES ('7876', 'ADAMS', 'CLERK', '7788', '1987-05-23', '1100', null, '20');
INSERT INTO `emp` VALUES ('7900', 'JAMES', 'CLERK', '7698', '1981-12-03', '950', null, '30');
INSERT INTO `emp` VALUES ('7902', 'FORD', 'ANALYST', '7566', '1981-12-03', '3000', null, '20');
INSERT INTO `emp` VALUES ('7934', 'MILLER', 'CLERK', '7782', '1982-01-23', '1300', null, '10');
 

```

新建一个maven模块或者新建一个maven工程都可以;
然后准备实体类和对应映射;
这么多字段,就不用手动去编写实体类了,通过mybatis逆向工程自动生成是一个不错的选择;


[逆向工程详解链接----->>>](https://blog.csdn.net/weixin_54061333/article/details/121660924)

基本架构就出来了
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6b5c2e8cd84469bb788d841a1fd8dde.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
初步检查一下映射文件,实体类路径是否匹配;

然后开始写mapper映射文件

先分析以一下要查询的数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/39891448b1d14d62a7ffc71431d756e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
发现并不在一个实体类上,那么怎么办?

新建一个实体类,作用是用于接收查询的数据
新实体类暂且叫EmpDept吧,同时还要配置映射文件


![在这里插入图片描述](https://img-blog.csdnimg.cn/c92f173699014f1482d0dfd1fa93222c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

根据要查询的数据来编写实体类并配置映射文件


你会发现,要原来的实体类和mapper有何用?
如果查询不同的数据,那么每次都要有一个新的实体类???
很显然是不可以的;


那怎吗办呢?

可不可以在一个实体里面引入另一个实体类?
这样就可以减少新建立一个实体类

不妨这么干----从员的角度出发,一个员工对应一个部门
在emp实体类中添加一个dept属性;


![在这里插入图片描述](https://img-blog.csdnimg.cn/aa6cd92de3034393bdcc687ab82b0f17.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这样我们减少一些不必要的类与映射

## 一对多查询

如果从部门的角度出发,一个部门对应好几个员工,那么我们的实体类该怎么修改呢?

首先更改dept实体中的属性----增加 emp属性

![在这里插入图片描述](https://img-blog.csdnimg.cn/647c24aa4393429d92bcac3b2d8a58f7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然后配置映射文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/873e6e09a72d4f2eb6ca8745a5e50c79.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

最后测试---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/84a937a3d90d444b8441feea9ec3ed86.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# 小结

一对一   resultmap用的是association 标签

> 在 association 元素中通常使用以下属性------>>>
> property：指定映射到实体类的对象属性。
> column：指定表中对应的字段（即查询返回的列名）。
>  javaType：指定映射到实体对象属性的类型。





一对多     resultmap用的是   collection 标签

> 在 collection 元素中通常使用以下属性。
> property：指定映射到实体类的对象属性。
> column：指定表中对应的字段（即查询返回的列名）。
> javaType：指定映射到实体对象属性的类型。
> select：指定引入嵌套查询的子 SQL 语句，该属性用于关联映射中的嵌套查询。



## 多对多查询
首先准备一个数据库然后建表,当然不用自己建,网上有很多可以拿过来练习的虚拟表格,拿来用就可以了'


下面是完整的sql数据,有些顺序需要自己调整,添加外加约束的表要放在后面创建;


```sql

SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for userinfo
-- ----------------------------
DROP TABLE IF EXISTS `userinfo`;
CREATE TABLE `userinfo`  (
  `uid` int(0) NOT NULL AUTO_INCREMENT,
  `uname` varchar(20) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `upwd` varchar(20) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`uid`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 7 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of userinfo
-- ----------------------------
INSERT INTO `userinfo` VALUES (1, '李二狗', '123');
INSERT INTO `userinfo` VALUES (2, '张化胡', '456');
INSERT INTO `userinfo` VALUES (3, '赵小红', '123');
INSERT INTO `userinfo` VALUES (4, '李晓明', '345');
INSERT INTO `userinfo` VALUES (5, '杨小胤', '123');
INSERT INTO `userinfo` VALUES (6, '谷小乐', '789');

SET FOREIGN_KEY_CHECKS = 1;

SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for orderinfo
-- ----------------------------
DROP TABLE IF EXISTS `orderinfo`;
CREATE TABLE `orderinfo`  (
  `oid` int(0) NOT NULL AUTO_INCREMENT,
  `ordernum` int(0) NULL DEFAULT NULL,
  `userId` int(0) NULL DEFAULT NULL,
  PRIMARY KEY (`oid`) USING BTREE,
  INDEX `userId`(`userId`) USING BTREE,
  CONSTRAINT `orderinfo_ibfk_1` FOREIGN KEY (`userId`) REFERENCES `userinfo` (`uid`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB AUTO_INCREMENT = 10 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of orderinfo
-- ----------------------------
INSERT INTO `orderinfo` VALUES (1, 20200107, 1);
INSERT INTO `orderinfo` VALUES (2, 20200806, 2);
INSERT INTO `orderinfo` VALUES (3, 20206702, 3);
INSERT INTO `orderinfo` VALUES (4, 20200645, 1);
INSERT INTO `orderinfo` VALUES (5, 20200711, 2);
INSERT INTO `orderinfo` VALUES (6, 20200811, 2);
INSERT INTO `orderinfo` VALUES (7, 20201422, 3);
INSERT INTO `orderinfo` VALUES (8, 20201688, 4);

SET FOREIGN_KEY_CHECKS = 1;

SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for ordersdetail
-- ----------------------------
DROP TABLE IF EXISTS `ordersdetail`;
CREATE TABLE `ordersdetail`  (
  `odid` int(0) NOT NULL AUTO_INCREMENT,
  `orderId` int(0) NULL DEFAULT NULL,
  `productId` int(0) NULL DEFAULT NULL,
  PRIMARY KEY (`odid`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 7 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of ordersdetail
-- ----------------------------
INSERT INTO `ordersdetail` VALUES (1, 1, 1);
INSERT INTO `ordersdetail` VALUES (2, 1, 2);
INSERT INTO `ordersdetail` VALUES (3, 1, 3);
INSERT INTO `ordersdetail` VALUES (4, 2, 3);
INSERT INTO `ordersdetail` VALUES (5, 2, 1);
INSERT INTO `ordersdetail` VALUES (6, 3, 2);

SET FOREIGN_KEY_CHECKS = 1;

SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for product
-- ----------------------------
DROP TABLE IF EXISTS `product`;
CREATE TABLE `product`  (
  `pid` int(0) NOT NULL AUTO_INCREMENT,
  `pname` varchar(25) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `price` double NULL DEFAULT NULL,
  PRIMARY KEY (`pid`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 5 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of product
-- ----------------------------
INSERT INTO `product` VALUES (1, 'JavaWeb', 128);
INSERT INTO `product` VALUES (2, 'C##', 138);
INSERT INTO `product` VALUES (3, 'Python', 132.35);

SET FOREIGN_KEY_CHECKS = 1;


```
分析表关系

![在这里插入图片描述](https://img-blog.csdnimg.cn/8f46cb40fb0d492495399f1f9ce35ae0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



![在这里插入图片描述](https://img-blog.csdnimg.cn/615fab1af7f94e638ce6411982960efd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




所以在userinfo实体类添加 orderinfo的集合,

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class UserInfo implements Serializable {
    private static final long serialVersionUID = 1L;

    private Integer uid;

    private String uname;

    private String upwd;

    private List<OrderInfo> orderInfo;

    
}
```
在orderinfo中添加orderdetail集合

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class OrderInfo implements Serializable {
    private static final long serialVersionUID = 1L;
    private Integer oid;
    private Integer ordernum;
    private Integer userid;
    //    一对一  一条订单 对应一条订单信息
  List  <OrdersDetail> ordersdetail;
//    //一条订单信息对商品    一对多
//    private List<Product>products;
    }
```


然后在orderdetail中添加productj集合属性,

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class OrdersDetail implements Serializable {
    private static final long serialVersionUID = 1L;
    private Integer odid;

    private Integer orderid;

    private Integer productid;
    //一条订单信息对商品    一对多
    private List<Product>products;


}
```
product类不做任何操作
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Product implements Serializable {
    private static final long serialVersionUID = 1L;
    private Integer pid;
    private String pname;
    private Double price;
    
}
```


接下来写mapper接口中的方法了;
简单写一个 一个用户下的所有订单信息;

从用户角度出发,所以  使用  userinfo的实体类作为入口


定义查询方法----->>>


```java
import com.gavin.pojo.UserInfo;
import java.util.List;
public interface UserInfoMapper {
List<UserInfo> findUserOrder (Integer uid);

}
```

开始搞mapper.xm中的映射;

先写sql语句,然后在构造resultMap返回值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gavin.mapper.UserInfoMapper">
 <resultMap id="userRef"    type="userInfo">
...............看下面

 </resultMap>
 <select id="findUserOrder"   resultMap="userRef" parameterType="int">
        SELECT
        u.*,

        o.*,

        od.*,

        p.*

        FROM
        userinfo u
    inner JOIN orderinfo o ON u.uid = o.userid
        inner   JOIN ordersdetail od ON o.oid = od.orderid
        inner   JOIN product p ON od.productid = p.pid
        <where>
            u.uid = #{uid}
        </where>


    </select>
</mapper>
```

自定义映射----

```xml
  <resultMap id="userRef"    type="userInfo">
        
        <!--        用户表-->
        <id property="uid" column="uid"/>
        <result property="uname" column="uname"/>
        <result property="upwd" column="upwd"/>


        <!--订单表-->
        <collection property="orderInfo" ofType="orderInfo">
            <id column="oid" property="oid"/>
            <result column="ordernum" property="ordernum"/>
            <result column="userid" property="userid"/>
            <!--订单信息表-->
            <collection property="ordersdetail" ofType="ordersDetail">
                <id column="odid" property="odid"/>
                <result column="orderid" property="orderid"/>
                <result column="productid" property="productid"/>

                <collection property="products" ofType="product">
                    <id property="pid" column="pid"/>
                    <result property="pname" column="pname"/>
                    <result property="price" column="price"/>
                </collection>
            </collection>
        </collection>

    </resultMap>
```



测试结果----->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/315329e71f1a436587fb06eaffa23ade.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


异常解决----查询时遇到异常

```c
  org.mybatis.spring.MyBatisSystemException: nested exception is org.apache.ibatis.exceptions.TooManyResultsException: Expected one result (or null) to be returned by selectOne(), but found: 3
```

查看异常原因---查询到多条数据,但是mybais要求只能显示一条,这说明方法的返回类型不匹配,尝试改为集合或者数组  然后测试,一般问题就是出现在这里;



自定义映射最主要的是嵌套的问题,以及实体类引入属性的问题

如果是非常复杂的映射,需要嵌套好几层;

**使用MyBatis的延迟加载在一定程度上可以降低运行消耗并提高查询效率,MyBatis默认没有开启延迟加载，需要在核心配置文件mybatis-config.xml中的<settings>元素内进行配置，具体配置方式如下。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/e0bb7f8b8a92484dbb248bba812d93af.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 在映射文件中，MyBatis关联映射的<association元素和<collection元素中都已默认配置了延迟加载属性，即默认属性fetchType="lazy"（属性fetchType="eager"表示立即加载），所以在配置文件中开启延迟加载后，无须在映射文件中再做配置。

# mybatis分页实现

准备一个分页工具文件----

```java
package com.gavin.util;

import com.gavin.pojo.UserInfo;

import java.util.List;

public class page {

    //    表数据
    private Integer dataCount;
    //每页显示多少条数据
    private Integer showData;
    //一共多少个页
    private Integer pageCount;
    //当前页
    private Integer pageIndex;

    //    当前页现实的集合信息
    private List<UserInfo> list;

    public page() {
    }

    public page(Integer dataCount, Integer showData, Integer pageCount, Integer pageIndex, List<UserInfo> list) {
        this.dataCount = dataCount;
        this.showData = showData;
        this.pageCount = pageCount;
        this.pageIndex = pageIndex;
        this.list = list;
    }

    public Integer getDataCount() {
        return dataCount;
    }

    public void setDataCount(Integer dataCount) {
        this.dataCount = dataCount;
    }

    public Integer getShowData() {
        return showData;
    }

    public void setShowData(Integer showData) {
        this.showData = showData;
    }

    public Integer getPageCount() {
//        总条数除以每页显示数据,能整除就取这个值,否则+1
        return this.dataCount % this.showData == 0 ? this.dataCount / this.showData : this.dataCount / this.showData + 1;

    }

/*    public void setPageCount(Integer pageCount) {
        this.pageCount = pageCount;
    }*/

    public Integer getPageIndex() {
        return pageIndex;
    }

    public void setPageIndex(Integer pageIndex) {
        this.pageIndex = pageIndex;
    }

    public List<UserInfo> getList() {
        return list;
    }

    public void setList(List<UserInfo> list) {
        this.list = list;
    }
}

```


测试后![在这里插入图片描述](https://img-blog.csdnimg.cn/595f0708595445be84a242a3c260eee6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


参数设置-----

查询所有记录数%每页显示的数,如果能整除,则 页数为记录数/每页显示数,否则加一,但是这里有一个坑,如果是数组或者集合,由于是下标为零开始,所以可以不用加1,需要灵活处理;


也不用这么麻烦,其实还有一个工具类----RowBounds用它就可以直接
直线分页;


```java
    @Test
    public void Test() {

        UserInfoMapper mapper = sqlSession.getMapper(UserInfoMapper.class);


        RowBounds rowBounds= new RowBounds(0,2);
        List<UserInfo> userInfos = mapper.selectBypage(rowBounds);
        for (UserInfo u :
                userInfos) {
            System.out.println(u.getUid() + "--" + u.getUname());
        }

    }
```

[源代码------>>>在这里](https://github.com/JeEisenberg/EmpDept.git)

在接触mybatis时我们会学到接触小知识,关于懒加载的一些知识点还是需要亲自去测试一下才能加深理解;

@[TOC](mybatis懒加载与缓存)

在这之前问我们都接触过关于时间和空间局部性原理,在这里不做多说;
接下来是案例的实际操作过程---->>
首先准备两个关联的表,还是熟悉的表,还是熟悉的数据---

商品信息与订单信息表

```sql
DROP TABLE IF EXISTS `product`;
CREATE TABLE `product`  (
  `pid` int(0) NOT NULL AUTO_INCREMENT,
  `pname` varchar(25) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `price` double NULL DEFAULT NULL,
  PRIMARY KEY (`pid`) USING BTREE,
  CONSTRAINT `qwe` FOREIGN KEY (`pid`) REFERENCES `ordersdetail` (`productId`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB AUTO_INCREMENT = 5 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of product
-- ----------------------------
INSERT INTO `product` VALUES (1, 'JavaWeb', 128);
INSERT INTO `product` VALUES (2, 'C##', 138);
INSERT INTO `product` VALUES (3, 'Python', 132.35);





SET FOREIGN_KEY_CHECKS = 1;
DROP TABLE IF EXISTS `ordersdetail`;
CREATE TABLE `ordersdetail`  (
  `odid` int(0) NOT NULL AUTO_INCREMENT,
  `orderId` int(0) NULL DEFAULT NULL,
  `productId` int(0) NULL DEFAULT NULL,
  PRIMARY KEY (`odid`) USING BTREE,
  INDEX `WER`(`productId`) USING BTREE,
  CONSTRAINT `WER` FOREIGN KEY (`productId`) REFERENCES `product` (`pid`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB AUTO_INCREMENT = 7 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of ordersdetail
-- ----------------------------
INSERT INTO `ordersdetail` VALUES (1, 1, 1);
INSERT INTO `ordersdetail` VALUES (2, 2, 2);
INSERT INTO `ordersdetail` VALUES (3, 3, 3);

SET FOREIGN_KEY_CHECKS = 1;

```


接口信息---->>


```java

package com.gavin.mapper;

import com.gavin.pojo.Ordersdetail;


public interface OrdersdetailDao {

  Ordersdetail selectOrderDetail (int oidd);
}
```

```java
package com.gavin.mapper;

public interface ProductDao {
   Product  selectProduct (int pid);

}
```

mapper映射

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gavin.mapper.ProductDao">

  <select id="selectProduct" resultType="com.gavin.pojo.Product" parameterType="int">
select * from product where pid =#{pid}
  </select>
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gavin.mapper.OrdersdetailDao">

  <resultMap id="OrdersDetailRef" type="ordersdetail">
    <id column="odid" property="odid"/>
    <result column="orderid" property="orderid"/>
    <result column="productid" property="productid"/>
    <association property="product" javaType="product" select="com.gavin.mapper.ProductDao.selectProduct" column="productid">
      <id property="pid" column="pid"/>
      <result property="pname" column="pname"/>
      <result property="price" column="price"/>
    </association>
  </resultMap>


  <select id="selectOrderDetail" resultMap="OrdersDetailRef" >
    select * from ordersdetail where odid =#{oidd}

  </select>
</mapper>
```



[解决sql语句爆红的小插曲](https://blog.csdn.net/weixin_54061333/article/details/121723032)



懒加载是对于多表查询时而言的,查询一张表时也会顺带查询关联的表----如果不开启懒加载;



所以在配置订单信息表映射是要注意一下;



在配置文件中设置一下懒加载的方式

![在这里插入图片描述](https://img-blog.csdnimg.cn/b6310338eeb34bc29780b9042b87770d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


开始测试---->>>>
```java
//测试懒加载
  @Test
    public void test() {
        OrdersdetailDao mapper = sqlSession.getMapper(OrdersdetailDao.class);
        Ordersdetail ordersdetail = mapper.selectOrderDetail(1);
        //System.out.println(ordersdetail);
        sqlSession.close();
//商品信息表
  /*  System.out.println("订单编号:"+ordersdetail.getOdid());
    System.out.println("订单号:"+ordersdetail.getOrderid());
    //商品表
    System.out.println("商品名---"+ordersdetail.getProduct().getPname());*/
    }

```



部分结果一----




> lazyLoadingEnabled	延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态。
> aggressiveLazyLoading	开启时，任一方法的调用都会加载该对象的所有延迟加载属性。 否则，每个延迟加载属性会按需加载（参考 lazyLoadTriggerMethods)。	**即只有在查询属性时积极懒加载才会执行sql;**



![在这里插入图片描述](https://img-blog.csdnimg.cn/5c1c1108d8c84324b4cf8860c184faad.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

# mybatis对缓存的支持

缓存是一种用空间换时间的一种方式,MyBatis 内置了一个强大的事务性查询缓存机制，默认情况下，只启用了本地的session缓存，即一级缓存,它仅仅对在一个session的数据进行缓存。 如果要启用全局的二级缓存，需要在你的mybatis配置文件以及 SQL 映射文件
中进行配置;

> 映射语句文件中的**所有 select 语句**的结果将会被缓存。

> 映射语句文件中的所有 **insert、update 和 delete 语句会刷新缓存**。

> 缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存。

> 缓存**不会定时进行刷新**（也就是说，没有刷新间隔）。

> 缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用。

> 缓存会被视为**读/写缓存**，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改。




mybatis提供了对缓存的支持,在mybatis配置文件中setting标签下提供了很多关于mybatis的配置,关于缓存的一些参数配置--->>

**catcheEnable**----
全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。
默认false

**localCacheScope**----
MyBatis 利用本地缓存机制（Local Cache）防止循环引用和加速重复的嵌套查询。 默认值为 **SESSION**，会缓存一个会话中执行的所有查询。 若设置值为 **STATEMENT**，本地缓存将仅用于执行语句，对相同 SqlSession 的不同查询将不会进行缓存。

一级缓存是基于perpetualCatche的hashMap本地缓存,其作用阈为Session,当session      flush或者close后缓存就被清空;

> mybatis默认开启了一级缓存 ,即在一个sqlsession中,执行相同的sql,那么第一次查询时会从数据据取数据,第二次查询时直接从缓存中取,当执行sql查询时中间发生了增删改操作,或者flush/close操作,那么sqlsession缓存会被清空

二级缓存作用域为namespace,可自定义存储源;


## mybatis一级缓存

**测试mybatis缓存------->>**

为方便起见,测试案例用的为上面的商品表;

```java
  public void test2() {
        ProductDao mapper = sqlSession.getMapper(ProductDao.class);
        Product product = mapper.selectProduct(1);
        System.out.println(product.getPid() + "--" + product.getPname() + "--" + product.getPrice());

        System.out.println("------分割线------------");

        Product product2 = mapper.selectProduct(1);
        System.out.println(product.getPid() + "--" + product.getPname() + "--" + product.getPrice());
       // sqlSession.close();
    }
```

在没有关闭session的情况下,查询同一条数据两次
中间没有增删改操作,也没有刷新和关闭操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/80442e38f0fd4a87a1c0c65db3c48048.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/be6a567ef57c4cbd895cb7494d7caad9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


在两次之间做增加数据操作----->>>

再插入数据


![在这里插入图片描述](https://img-blog.csdnimg.cn/e1251293773440fba1fe6d8c2bb25480.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



在操作中发现在插入数据时,如果没有提交,也会清除缓存,使得查询相同数据的sql语句执行两次,

如果不插入数据,在两次查询之间只增加一次commit操作,
![在这里插入图片描述](https://img-blog.csdnimg.cn/c7a5fde67e2146c7bd1ffb0ff05294b0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


下面我们在sql语句中配置----->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/86d71df724b74f6eb96e449f5a4bf4e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
在不同的session中查询同一数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/521487c43d774881abcdda7aa2cd74fe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)





小结----->>
**在相同的查询操作之间    刷新缓存,提交, 增删改操作都会使得一级缓存清空;**

**一级缓存作用范围在同一个session中;**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2b1a81cbe2504514a50dabca5ed10f91.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




要想在多个session中共享数据,那么就需要开启二级缓存;








## mybatis二级缓存
在全局配置文件中,二级缓存默认是开启的,要使其生效需要对每个Mapper进行配置;

在配置二级缓存之前,先要下载两个jar,在maven中直接引入就可以了


```xml
 <!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
        <dependency>
            <groupId>org.mybatis.caches</groupId>
            <artifactId>mybatis-ehcache</artifactId>
            <version>1.2.1</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/net.sf.ehcache/ehcache -->
        <dependency>
            <groupId>net.sf.ehcache</groupId>
            <artifactId>ehcache</artifactId>
            <version>2.10.9.2</version>
        </dependency>
```

之后在mybatis全局配置文件中开启 二级缓存--->>>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/a5e203d1e4774030b34cf353d64a7dea.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
但这还没结束,还需要在想要开启二级缓存的sql语句中开启二级缓存

```xml
<!--useCache 使得二级缓存生效  将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。-->
    <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
    <select id="selectProduct" resultType="com.gavin.pojo.Product" parameterType="int" flushCache="false" statementType="PREPARED" useCache="false">
select * from product where pid =#{pid}
  </select>
```


测试二级缓存-----
首先创建两个session

```xml
@Test
    public void test2() {
        SqlSession sqlSession = factory.openSession();
        SqlSession sqlSession1 = factory.openSession();
        ProductDao mapper = sqlSession.getMapper(ProductDao.class);
        ProductDao mapper1 = sqlSession1.getMapper(ProductDao.class);

        System.out.println(mapper.selectProduct(1));
        sqlSession.close();

        System.out.println(mapper1.selectProduct(1));

        sqlSession1.close();
    }
```
运行结果---->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/cfdb4262e320467fb438d06730cb0e8e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


注意:
如果没有在sql中开启useCache 即useCache =false,那么即使在全局配置中开了二级缓存,在sql查询时也不会开启二级缓存;

另一个,如果两此查询时在最后关闭了session,那么也会用两次sql穿语句;

例如

![在这里插入图片描述](https://img-blog.csdnimg.cn/00df12ce94c24283871ff917aba9b24e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

小结----
1,在全局配置文件中开启二级缓存之后,还需要在相应sql中再次开启 缓存才能生效,但是这还没完全开启,需要指定缓存的类型,即用什么类型的类取处理缓存;![在这里插入图片描述](https://img-blog.csdnimg.cn/240497611fa948e3845aa73aef4d6598.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

2,session关闭顺序会影响二级缓存的是否生效;

![在这里插入图片描述](https://img-blog.csdnimg.cn/210159b0a1a345febf9943dce43fca8b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


**如果不想让某条sql语句使用二级缓存---
可以在该sql配置   useCache="false"**

**类似问题-----.>>>如和让一级缓存失效?
可以在该sql配置   flushCache="true"**

**或者将本地缓存---一级缓存作用域设置为 statament**

```xml
 <setting name="localCacheScope" value="STATEMENT"/>**
```

小结----
***开启二级缓存的步骤***

1,开启二级缓存----可以显示的在mybatis全局配置文件按中声明,即使不声明,也默认为true---开启二级缓存

```xml
   <setting name="cacheEnabled" value="true"/>
```
2,在对应的映射文件中添加配置

cache有两种配置方式------->>>

第一种:----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed5f731781dc417f866a582ba3cfeaf5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这个时候Mybatis会按照默认配置创建一个Cache对象，
此时创建的是PerpetualCache对象,这种缓存方式最多只能存1024个元素

也可去指定缓存的对象类型

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```

第二种是通过引用的方式



![在这里插入图片描述](https://img-blog.csdnimg.cn/97988a445360408f9cc8520a299d5a63.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这个时候两个mapper共享一个缓存空间;


> 既然可以指定缓存类型,那么就可以自定义缓存,先不着急自定义;有那么多已经写好的优秀插件为什么还要自定义?

先不管那些,大体工作原理--------Cache接口, 通过Id来获得对用的缓存对象;
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b7b2247fd3d4217b48cefba5b98a6a8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
为了保证一对一的关系,缓存底层通过map来实现的;

找到其实现类-----有很多,
![在这里插入图片描述](https://img-blog.csdnimg.cn/41b1d51cbaa0473a9b0a650359440c71.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




如果要自定义,那么首先要了解缓存的是怎样工作的;

1, XMLMappedBuilder来 解析 Mapper 中的 缓存标签

2,通过 builderAssistant 对象来调用 addMappedStatement 方法，在设置 cache 信息到 MappedStatement 对象内;
通过逆向探索可以获得缓存对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/7238ccc565514fb28786271b4c091f40.png)
3,CachingExecutor 对象的 query 方法先从 MappedStatement 对象中 getCache() 获取缓存的对象，如果没有查到则到 BaseExecutor 中查询，走本地缓存逻辑,在查不到,就要从数据库中查找了


二级缓存一般是不建议开启的,因为在高并发情况下,很容易发生数据与数据库数据不一致的情况;
另一个由于二级缓存可以存储在内存中,也可以持久化到本地,因此需要实现序列化接口;





## Mybatis整合第三方缓存框架

大数据时代,要求系统在高并发下的性能要有所提高,mybatis自带缓存不适用与分布式缓存,所以要使用分布式缓存框架来对缓存实施数据管理;

上例提到的分布式框架缓存----ehcache是比较常用的一个,除此之外还有redis,memcache等;


> ehcache直接在jvm虚拟机中缓存，速度快，效率高；但是缓存共享麻烦，集群分布式应用不方便。

>redis是通过socket访问到缓存服务，效率比ecache低，比数据库要快很多，处理集群和分布式缓存方便，有成熟的方案。

>如果是单个应用或者对缓存访问要求很高的应用，用ehcache。
>如果是大型系统，存在缓存共享、分布式部署、缓存内容很大的，建议用redis。


## 分布式框架的使用----ehcache:
一种广泛使用的开源java分布式框架,主要面向缓存,
可已将缓存数据存放于内存和磁盘;


分布式框架的使用

首先在maven中引入依赖---

```xml
    <!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
        <dependency>
            <groupId>org.mybatis.caches</groupId>
            <artifactId>mybatis-ehcache</artifactId>
            <version>1.2.1</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/net.sf.ehcache/ehcache -->
        <dependency>
            <groupId>net.sf.ehcache</groupId>
            <artifactId>ehcache</artifactId>
            <version>2.10.9.2</version>
        </dependency>
```

然后在核心配置文件中开启二级缓存;
![在这里插入图片描述](https://img-blog.csdnimg.cn/37716d72680943428e804b0639f88edb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然后在映射文件中开启该缓存框架

![在这里插入图片描述](https://img-blog.csdnimg.cn/9117620be330458cb0f9b500aed71c2a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

再然后----由于二级缓存可以存在内存或者持久化的存在本地,,所以对应的实体类要实现序列化,这一步要看在实体类的开发过程中有有没有实现序列化,如果有,则这一步可以省略;


在然后配置ehcache的配置文件,文件名必须是ehcache.


ehcache的配置文件配置文件详解----->>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd" updateCheck="false">
    <diskStore path="D:\ehcache\temporary"/>
<!--    默认缓存策略-->
<!-- name 缓存名-->
<!--    maxElementsInMemory:缓存最大数目-->
<!--    maxElementsOnDisk：硬盘最大缓存个数。-->
<!--    eternal:对象是否永久有效，一但设置了，timeout将不起作用。-->
<!--    overflowToDisk:是否保存到磁盘，当系统当机时-->

<!--    timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。
仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。-->
    
<!--    timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。
最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。-->
    
<!--    diskPersistent：是否缓存虚拟机重启期数据 -->

<!--    diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。
             每个Cache都应该有自己的一个缓冲区。-->

<!--    diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。-->
   
<!--    memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时,
   Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。-->
<!--    clearOnFlush：内存数量最大时是否清除。-->
<!--    memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
    FIFO，first in first out，这个是大家最熟的，先进先出。
    LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。
    如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
    LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，
    当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。-->
    <defaultCache eternal="false" maxElementsInMemory="1000"
                  overflowToDisk="false" diskPersistent="false" timeToIdleSeconds="0"
                  timeToLiveSeconds="600" memoryStoreEvictionPolicy="LRU" />

    <cache name="FunctionCache" eternal="false"
           maxElementsInMemory="500" overflowToDisk="false" diskPersistent="false"
           timeToIdleSeconds="3600" timeToLiveSeconds="3600"
           memoryStoreEvictionPolicy="LRU">
    </cache>

    <cache name="RoleFunctionCache" eternal="false"
           maxElementsInMemory="500" overflowToDisk="false" diskPersistent="false"
           timeToIdleSeconds="3600" timeToLiveSeconds="3600"
           memoryStoreEvictionPolicy="LRU">
    </cache>

    <cache name="ModelDefCache" eternal="false"
           maxElementsInMemory="500" overflowToDisk="false" diskPersistent="false"
           timeToIdleSeconds="3600" timeToLiveSeconds="3600"
           memoryStoreEvictionPolicy="LRU">
    </cache>

    <cache name="SysNoConfigCache" eternal="false"
           maxElementsInMemory="500" overflowToDisk="false" diskPersistent="false"
           timeToIdleSeconds="3600" timeToLiveSeconds="3600"
           memoryStoreEvictionPolicy="LRU">
    </cache>

    <cache name="MesProdTimeCache" eternal="false"
           maxElementsInMemory="500" overflowToDisk="false" diskPersistent="false"
           timeToIdleSeconds="3600" timeToLiveSeconds="3600"
           memoryStoreEvictionPolicy="LRU">
    </cache>

</ehcache>

```

测试代码


```java

    @Test
    public void test4() {
        SqlSession sqlSession = factory.openSession();
        SqlSession sqlSession1 = factory.openSession();

        ProductDao mapper = sqlSession.getMapper(ProductDao.class);
        ProductDao mapper1 = sqlSession1.getMapper(ProductDao.class);

        Product product = mapper.selectProduct(1);
        System.out.println(product.hashCode());

       sqlSession.close();

        Product product1 = mapper1.selectProduct(1);

        System.out.println(product1.hashCode());

        sqlSession1.close();
    }
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/86be431b64df436aab3ded85f24b0638.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

同时我们在指定位置也可看到相应的文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/021d2085b5cb4ec5858befbba4b12cc8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_18,color_FFFFFF,t_70,g_se,x_16)

# mybatis主键自动回填

mybatis主键自动回填的实现对应的业务逻辑就是我们在添加数据时,知道表中的主键为自动递增,但是不知道递增到多少个了,但是我们还需要知道该表中的主键数据才能对另一张关联的表进行操作;


例如下面的案例----->>
添加书籍时候还需要对书籍折扣信息表进行更新,

```sql
DROP TABLE IF EXISTS `bookstore`;
CREATE TABLE `bookstore`  (
  `BookId` int(0) NOT NULL AUTO_INCREMENT,
  `BookName` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  `BookPublish` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  `BookPrice` double(10, 2) NULL DEFAULT NULL,
  `BookKind` varchar(16) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  `BookCount` int(0) NULL DEFAULT NULL,
  PRIMARY KEY (`BookId`) USING BTREE,
  INDEX `name_index`(`BookName`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 1234 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of bookstore
-- ----------------------------
INSERT INTO `bookstore` VALUES (1001, 'java入门', '清华出版社', 180.90, '计算机', 90);
INSERT INTO `bookstore` VALUES (1002, 'python深入', '机械工业出版社', 78.60, '计算机', 130);
INSERT INTO `bookstore` VALUES (1003, '前端html', '清华出版社', 88.80, '计算机', 100);
INSERT INTO `bookstore` VALUES (1004, '废都', '张江出版社', 25.70, '文学名著', 100);
INSERT INTO `bookstore` VALUES (1005, '官场现形记', '青岛出版社', 23.80, '文学名著', 100);
INSERT INTO `bookstore` VALUES (1006, '红楼梦', '青岛出版社', 36.80, '文学名著', 100);
INSERT INTO `bookstore` VALUES (1007, '母猪的产后护理', '机械工业出版社', 27.80, '实用技术', 100);
INSERT INTO `bookstore` VALUES (1008, '车辆维修大全', '机械工业出版社', 26.80, '实用技术', 100);
INSERT INTO `bookstore` VALUES (1009, '如何讨取富婆欢心富婆', '开心出版社', 125.00, '生活技能', 100);
INSERT INTO `bookstore` VALUES (1010, 'JVM调优', '北京科技出版社', 68.90, '计算机', 120);
INSERT INTO `bookstore` VALUES (1011, 'GO', '南京邮电出版社', 78.90, '计算机', 120);
INSERT INTO `bookstore` VALUES (1012, 'Android入门', '电子科技出版社', 88.90, '计算机', 120);
INSERT INTO `bookstore` VALUES (1014, '果树种植学', '西北农林出版社', 128.90, '农业', 120);
INSERT INTO `bookstore` VALUES (1015, '笑话', '上海出版社', 48.90, '生活', 120);
INSERT INTO `bookstore` VALUES (1111, 'MQ消息队列', '北大出版社', 88.90, '计算机', 120);
INSERT INTO `bookstore` VALUES (1222, 'java大数据', '清华出版社', 124.70, '计算机', 160);
INSERT INTO `bookstore` VALUES (1233, 'java集合', '电子科技出版社', 120.80, '计算机', 80);

DROP TABLE IF EXISTS `bookdiscount`;
CREATE TABLE `bookdiscount`  (
  `Id` int(0) NOT NULL AUTO_INCREMENT,
  `BookID` int(0) NULL DEFAULT NULL,
  `Bookdicount` double(4, 2) NULL DEFAULT NULL,
  PRIMARY KEY (`Id`) USING BTREE,
  INDEX `fk_bookid_bookstore`(`BookID`) USING BTREE,
  CONSTRAINT `fk_bookid_bookstore` FOREIGN KEY (`BookID`) REFERENCES `bookstore` (`BookId`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of bookdiscount
-- ----------------------------
INSERT INTO `bookdiscount` VALUES (1, 1001, 0.90);
INSERT INTO `bookdiscount` VALUES (2, 1002, 0.90);
INSERT INTO `bookdiscount` VALUES (3, 1003, 0.98);
INSERT INTO `bookdiscount` VALUES (4, 1004, 0.80);
INSERT INTO `bookdiscount` VALUES (5, 1005, 0.88);
INSERT INTO `bookdiscount` VALUES (6, 1006, 0.90);
INSERT INTO `bookdiscount` VALUES (7, 1007, 0.90);
INSERT INTO `bookdiscount` VALUES (8, 1008, 0.98);
INSERT INTO `bookdiscount` VALUES (9, 1009, 0.80);
INSERT INTO `bookdiscount` VALUES (10, 1010, 0.95);
INSERT INTO `bookdiscount` VALUES (11, 1011, 0.88);
INSERT INTO `bookdiscount` VALUES (12, 1012, 0.89);
INSERT INTO `bookdiscount` VALUES (13, 1014, 0.90);
INSERT INTO `bookdiscount` VALUES (14, 1015, 0.90);
INSERT INTO `bookdiscount` VALUES (15, 1111, 0.98);
INSERT INTO `bookdiscount` VALUES (16, 1222, 0.80);
INSERT INTO `bookdiscount` VALUES (17, 1233, 0.96);

```

对应的mapper--->>


```xml
   <sql id="allColumn">BookId,BookName,BookPublish,BookPrice,BookKind,BookCount</sql>
    <sql id="partColumn">BookName,BookPublish,BookPrice,BookKind,BookCount</sql>



    <insert id="addBook" useGeneratedKeys="true" keyProperty="BookId" >
        insert into bookstore (<include refid="allColumn"></include>)
        values(#{BookId},#{BookName},#{BookPublish},#{BookPrice},#{BookKind},#{BookCount})
    </insert>
```

测试方法----->>

```java
 @Test
    public void Test2() {
        sqlSession = factory.openSession();
        BookMapper mapper = sqlSession.getMapper(BookMapper.class);
        Book book = new Book();
        int i = mapper.addBook(book);
        sqlSession.commit();
        System.out.println("新增书籍编号为--" + book.getBookId());
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/cf49260458004bacb5beb10667c81496.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

当去掉useGeneratedKeys="true" keyProperty="BookId"  属性时,就不会将主键的信息添加到book信息上;


![在这里插入图片描述](https://img-blog.csdnimg.cn/30bc581aedd14d4d96a4c692a8c62184.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

另一种实现方式---->>

```xml
    <insert id="addBook"  >
        <selectKey order="AFTER" keyProperty="BookId" resultType="int">
            select @@identity
        </selectKey>
        insert into bookstore (<include refid="allColumn"></include>)
        values(#{BookId},#{BookName},#{BookPublish},#{BookPrice},#{BookKind},#{BookCount})
    </insert>

```

主要理解下insert与update中独有的三个属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/b9b22fbdfede440c93aae9a3ec74859f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
如果表中没有主键或者数据库不支持主键,那么 也可以这么干----->>
在插入数据之前可以设置一下主键;



```xml

    <insert id="addBook" parameterType="book" >
     <!--   <selectKey order="AFTER" keyProperty="BookId" resultType="int">
            select @@identity
        </selectKey>
        -->
        <selectKey order="BEFORE" keyProperty="BookId" resultType="int" >
           select if(max(BookId) is null ,1,max(BookId)+1) as newBookId from bookstore
        </selectKey>
        insert into bookstore (<include refid="allColumn"></include>)
        values(#{BookId},#{BookName},#{BookPublish},#{BookPrice},#{BookKind},#{BookCount})
    </insert>

```
在执行上述示例代码时，<selectKey>元素会首先运行，y因为制定了顺序,

order属性可以被设置为BEFORE或AFTER。如果设置为BEFORE，那么它会先执行<selectKey>元素中的配置来设置主键，再执行插入语句；如果设置为AFTER，那么它会先执行插入语句，再执行<selectKey>元素中的配置内容。



它会通过自定义的语句来设置数据表中的主键（如果BookStore表中没有记录，就将bookid设置为1，否则将ibookd的最大值加1作为新的值），然后调用插入语句。
![在这里插入图片描述](https://img-blog.csdnimg.cn/09b112306e794f1e96d04ee706bc480e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# Mybatis学习回顾 

碎碎念--

> 花了一点时间，终于把mybatis给整入门了---自学还是比较费时间的

话不多说哦--直接开干
先来张数据表
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620090423281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

> 建立表的时候可以参考多种方式，可以再cmd界面，也可以用其他工具怎么方便怎么来，初学者最好是在cmd界面因为可以练习sql语句；

整java代码--

> 因为要跟数据库中的表打交道，所以要先建立一个跟数据库列名和数据类型一样的，一个是为了方便写代码，另一个避免运行的时候会有数据类型不兼容的问题出现；

```java
package charpter0619.one.next;
public class Animal {
private Integer animal_ID;
private String animal_type;
private String animal_name;
private float animal_price;
private Integer animal_num;
public Integer getAnimal_ID() {
	return animal_ID;
}
public void setAnimal_ID(Integer animal_ID) {
	this.animal_ID = animal_ID;
}
public String getAnimal_type() {
	return animal_type;
}
public void setAnimal_type(String animal_type) {
	this.animal_type = animal_type;
}
public String getAnimal_name() {
	return animal_name;
}
public void setAnimal_name(String animal_name) {
	this.animal_name = animal_name;
}
public float getAnimal_price() {
	return animal_price;
}
public void setAnimal_price(float animal_price) {
	this.animal_price = animal_price;
}
public Integer getAnimal_num() {
	return animal_num;
}
public void setAnimal_num(Integer animal_num) {
	this.animal_num = animal_num;
}
@Override
public String toString() {
	return "Animal [animal_ID=" + animal_ID + ", animal_type=" + animal_type + ", animal_name=" + animal_name
			+ ", animal_price=" + animal_price + ", animal_num=" + animal_num + "]";
}

}
//类中要有属性--与数据库中表的列名相对应(包括列名和数据类型)，再就是setter和getter方法，还要覆写一个toString，还好eclipse提供了便捷的方式；
```

整mybatis--先整配置文件，在src根目录下新建log4j.properties文件,文件内容如下

```java
#全局日志配置
#Global logging configuration
log4j.rootLogger=ERROR, stout
#MyBatis logging configuration...
#这里指定了将某包下所有的日志记录级别记录为DEBUG
log4j.logger.charpter0619.one=DEBUG
#控制台输出
Console output...
log4j.appender.stout=org.apache.log4j.ConsoleAppender
log4j.appender.stout.layout=org.apache.log4j.PatternLayout
log4j.appender.stout.layout.ConversionPattern=%5p [%t] - %m%n
```

接口--

```java
package charpter0619.one;
import java.io.IOException;
//接口，定义涨价和降价的动物
public interface AnimalManger {
public void Transfer(String animalout,String animalin,float price );
public void FindAnimalById()throws IOException;
}
```

接口实现类--

```java
package charpter0619.one.next;

import java.io.IOException;
import java.io.InputStream;
import javax.annotation.Resource;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Controller;

import org.springframework.transaction.annotation.Transactional;

import charpter0619.one.AnimalManger;

@Controller("manageAimal") // 控制层
public class ManageAnaimal implements AnimalManger {
	@Resource(name = "jdbcTemplate") // 注入
	private JdbcTemplate jdbcTemplate;

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}
	@Resource(name = "animal") // 注入
	private Animal animal;
	public void setAnimal(Animal animal) {
		this.animal = animal;
	}
@Transactional//事务管理，
	@Override
	public void Transfer(String animalout, String animalin, float price) {// 赠送动物
		String sql = "update animalshop set animal_price=animal_price+? where animal_name=?";
		this.jdbcTemplate.update(sql, price, animalin);
		System.out.println(animalin + "--涨价成功");
		String parm = "update animalshop set animal_price=animal_price-? where animal_name=?";
		this.jdbcTemplate.update(parm, price, animalout);
		System.out.println(animalout + "--降价成功");

	}
//这个是接口实现方法--查找动物信息
@Override
public void FindAnimalById() throws IOException {
//	String sql ="select * from animalshop where animal_id=id ";
//	RowMapper<Animal> rowMapper=new BeanPropertyRowMapper<Animal>(Animal.class);
//	读取配置文件
	String resource="mybatis-config.xml";
	InputStream inputStream=Resources.getResourceAsStream(resource);
	//格局配置文件构建SqlSesionFactory实例
	SqlSessionFactory sqlSessionFactory=new SqlSessionFactoryBuilder().build(inputStream);
	//通过SqlSessionFactory获得SqlSession实例
	SqlSession sqlSession=sqlSessionFactory.openSession();
	//sqlSession执行映射文件中的sql并返回结果
	Animal animal=sqlSession.selectOne("chapter0619.mapper.AnimalMapper.FindAnimalById", 1001);
	//打印输出结果
System.out.println(animal.toString());
//关闭sqlSession
sqlSession.close();
}
}
```

接着配置其他xml文件
在src目录下新建一个包用于存放映射空间--在该包下建立  （类名+Mapper.xml例如本例子就是AnimalMapper.xml）--配置内容如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--约束文件-->
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 定义映射空间 -->
<mapper namespace="chapter0619.mapper.AnimalMapper">
	<!-- 根据动物编号来获取动物信息 -->
	<!--定义sql语句的参数名，参数类型以及返回值类型，这里返回值是Animal类型，这根前面提到的animal类型的属性要跟数据表中列一一对应-->
		<select id="FindAnimalById"  parameterType="Integer" resultType="charpter0619.one.next.Animal">
		select * from animalshop where animal_ID=#{id}
	</select>
	<!--#{}表示一个占位符，#{id}表示该占位符接收的参数名称为id-->
</mapper>
```

以上都配置好之后，要配置关于mybatis的配置文件，
在src目录下创建mybatis-config,xml配置文件，内容如下

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE configuration PUBLIC "-//mybatis.org/ /DTD Config 3.0/ /EN"

"http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

	<!--1.配置环境，默认的环境id为mysq1 -->

	<environments default="mysql">

		<!--1.2配置id为mysql的数据库环境 -->

		<environment id="mysql">

			<!--使用JDBC的事务管理 -->

			<transactionManager type="JDBC" />

			<!--数据库连接池 -->

			<dataSource type="POOLED">

				<property name="driver" value="com.mysql.cj.jdbc.Driver" />

				<property name="url"
					value="jdbc:mysql://localhost:3306/company?serverTimezone=UTC" />
				<property name="username" value="root" />
<!--这里依然要加serverTimezone=UTC，不然也会提示时区错误-->
				<property name="password" value="955945" />

				</dataSource>

			</environment>

	</environments>

	<!-- 2.配置Mapper的位置 -->

	<mappers>

		<mapper resource="chapter0619/mapper/AnimalMapper.xml" />

	</mappers>

</configuration>
```

测试类

```java
package charpter0619.one;
import java.io.IOException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class Test {
	public static void main(String[] args) throws IOException {
		ApplicationContext app = new ClassPathXmlApplicationContext("NewFile.xml");
		AnimalManger animal =(AnimalManger)app.getBean("managerAnimal");
		animal.Transfer("小刚","小花", 100.5f);
		animal.FindAnimalById();
	}

}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620100015977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620155716869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)这是文件目录



以上就是mybatis入门操作；
具体思路就是--
新建类，对应数据库中表中列名，在类中要写setter，getter方法以便获得数据，
配置xml
配置properties配置文件
写方法来查询数据
最后测试



# Mybatis问题实录

## Mapped Statements collection does not contain value

> 初学者学习mybatis总会遇到一些错误，当然mybatis现在改名为Ibatis了，不过不重要，

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620152420673.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)遇到问题怎么办？解决问题
先看错误原因，说是映射语句不包含值，为什么呢，因为错误中提示的信息围绕着findBookById 方法---经过检查是mapper.xml位置写的不对
一，检查mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- mybatis 配置约束 -->
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0/ /EN"

"http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- 配置信息如下 -->
<configuration>
	<environments default="mysql">
		<environment id="mysql">
			<transactionManager type="JDBC"></transactionManager>
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.cj.jdbc.Driver" />
				<property name="url"
					value="jdbc:mysql://localhost:3306/bookstore?serverTimezone=UTC" />
				<property name="username" value="root" />
				<property name="password" value="955945" />
			</dataSource>
		</environment>
	</environments>

	<mappers>
		<mapper resource="ceshi/BookMapper.xml" />
	</mappers>
</configuration>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620152843110.png)

> 这里是斜杠分割不是点号

二，
![在这里插入图片描述](https://img-blog.csdnimg.cn/202106201530174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153100287.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)

这两处都是点号分割；

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210620153918159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81NDA2MTMzMw==,size_16,color_FFFFFF,t_70)



总结在java中凡是带扩展名的都是 “/"(斜杠)进行分割，不带的就是点号进行分割

> 还有一点，我自己发现的---不知正确与否，先写下来---
> mapper文件如果放在根目录下，按理说直接写BookMapper就可以了，但是也会报上述错误或者找不到mapper，是不是只能单独找个包存放他？



<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/#为什么会有这个网站)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/tomcat/tomcat.md#tomcat的部署方式)

</center>

![](..\pic\div\plant.gif)

