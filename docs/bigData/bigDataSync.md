# BigData

﻿本次项目部分用到的技术栈~

## 技术栈

 - 技术栈

Springboot
 Solr
 Zookeeper
 Dubbo
 Mybatis
 Mysql
 RabbitMQ
SpringSecurity
后续还可添加redis
>这里并没有配置集群

## 准备工作

 - 准备工作~solr

导入数据库数据

相关配置~~
导入数据库配置~
![在这里插入图片描述](https://img-blog.csdnimg.cn/084ed5c815c74bbda9650108d29748ee.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
其他非关键配置略

 - **开启solr**

```bash
[root@Gavin bin]# ./solr  start -force
Waiting up to 180 seconds to see Solr running on port 8983 [|]
Started Solr server on port 8983 (pid=28807). Happy searching!

```
新建核心~查询
```json
{
  "responseHeader":{
    "status":0,
    "QTime":0,
    "params":{
      "q":"*:*",
      "indent":"true",
      "q.op":"OR",
      "_":"1648604421556"}},
  "response":{"numFound":4,"start":0,"numFoundExact":true,"docs":[
      {
        "loc":"NEW YORK",
        "dname":"ACCOUNTING",
        "id":"10",
        "_version_":1728686796315820032},
      {
        "loc":"DALLAS",
        "dname":"RESEARCH",
        "id":"20",
        "_version_":1728686796365103104},
      {
        "loc":"CHICAGO",
        "dname":"SALES",
        "id":"30",
        "_version_":1728686796366151680},
      {
        "loc":"BOSTON",
        "dname":"OPERATIONS",
        "id":"40",
        "_version_":1728686796367200256}]
  }}
```

 - **开启zookeeper**

```bash
[root@Gavin bin]# ./zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /usr/local/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
[root@Gavin bin]#
```

 

## 模块组装

 **新建工程**
 - **新建实体类模块pojo**

### Pojo模块

 Dept类
```java
public class Dept implements Serializable {

    private  Integer deptno;

    private String dname;

    private String loc;

    private static final long serialVersionUID = -1L;

    public Dept() {
    }

    public Dept(Integer deptno, String dname, String loc) {
        this.deptno = deptno;
        this.dname = dname;
        this.loc = loc;
    }


    public Integer getDeptno() {
        return deptno;
    }

    public void setDeptno(Integer deptno) {
        this.deptno = deptno;
    }

    public String getDname() {
        return dname;
    }

    public void setDname(String dname) {
        this.dname = dname;
    }

    public String getLoc() {
        return loc;
    }

    public void setLoc(String loc) {
        this.loc = loc;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "deptno=" + deptno +
                ", dname='" + dname + '\'' +
                ", loc='" + loc + '\'' +
                '}';
    }
}

```
User类
```java
public class User implements Serializable {
    private Integer id;

    private String name;

    private String pwd;

    private String beizhu;

    private static final long serialVersionUID = 11L;

    public User() {
    }

    public User(Integer id, String name, String pwd, String beizhu) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
        this.beizhu = beizhu;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    public String getBeizhu() {
        return beizhu;
    }

    public void setBeizhu(String beizhu) {
        this.beizhu = beizhu;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                ", beizhu='" + beizhu + '\'' +
                '}';
    }
}
```

对于数据库的curd,自然需要mybatis
### Mapper模块
  **mapper模块**

 - **依赖~**

```xml

    <dependencies>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.27</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.8</version>
        </dependency>


        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.2</version>
        </dependency>


        <dependency>
            <artifactId>Pojo</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>

```

 - **mapper接口~**

DeptMapper
```java
@Mapper
public interface DeptMapper {

    /**
     * @return
     */
    List<Dept> queryAll();

    /**
     * @param deptno
     * @return
     */
    Dept queryByDeptno(int deptno);

    /**
     * @param dept
     * @return
     */
    int insertDept(Dept dept);

    /**
     * 
     * @param dept
     * @return
     */
    int updateDept(Dept dept);

    /**
     * 
     * @param deptno
     * @return
     */
    int deleteDeptByDeptno(int deptno);
}

```
UserMapper
```java
@Mapper
public interface UserMapper {

    int deleteByPrimaryKey(Integer id);

    int insert(User record);

    User selectByPrimaryKey(Integer id);

    User SelectUserByName(String username);

    List<User> SelectUser();

    User SelectUserById(Integer id);
}
```

xml文件~

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gavin.mapper.DeptMapper">

    <sql id="baseColumn">
deptno ,dname ,loc
    </sql>
    <insert id="insertDept">
        insert into dept values ( #{dept.deptno} ,#{dept.dname} ,#{dept.loc})
    </insert>

    <update id="updateDept">
        update dept set dname = #{dept.dname} ,loc =#{dept.loc}  where deptno =#{dept.deptno}
    </update>

    <delete id="deleteDeptByDeptno">
        delete from  deptno where deptno = #{deptno}
    </delete>

    <select id="queryAll" resultType="com.gavin.pojo.Dept">
        select  <include refid="baseColumn"></include>  from dept
    </select>

    <select id="queryByDeptno" resultType="com.gavin.pojo.Dept">
        select  <include refid="baseColumn"></include>  from dept where deptno= #{deptno}
    </select>
</mapper>
```
application.yml

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/gavin
    username: gavin
    password: 955945
    druid:
      db-type: mysql
    type: com.alibaba.druid.pool.DruidDataSource
```

### ServiceAPI模块

 - **新建ServiceAPI模块**

**依赖~**

```xml
<dependencies>
    <dependency>
        <artifactId>Mapper</artifactId>
        <groupId>com.codem</groupId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    <dependency>
        <groupId>com.codem</groupId>
        <artifactId>Mapper</artifactId>
        <version>1.0-SNAPSHOT</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

接口~DubboDeptService
```java
public interface DubboDeptService {
    /**
     * @return
     */
    List<Dept> queryAll();

    /**
     * @param deptno
     * @return
     */
    Dept queryByDeptno(int deptno);

    /**
     * @param dept
     * @return
     */
    int insertDept(Dept dept);

    /**
     *
     * @param dept
     * @return
     */
    int updateDept(Dept dept);

    /**
     *
     * @param deptno
     * @return
     */
    int deleteDeptByDeptno(int deptno);


}

```
DubboUserServiceImpl
```java
@DubboService
public class DubboUserServiceImpl implements DubboUserService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public int deleteByPrimaryKey(Integer id) {
        return userMapper.deleteByPrimaryKey(id);
    }

    @Override
    public int insert(User record) {
        return userMapper.insert(record);
    }

    @Override
    public User selectByPrimaryKey(Integer id) {
        return userMapper.selectByPrimaryKey(id);
    }

    @Override
    public User SelectUserByName(String username) {
        return userMapper.SelectUserByName(username);
    }

    @Override
    public List<User> SelectUser() {
        return userMapper.SelectUser();
    }

    @Override
    public User SelectUserById(Integer id) {
        return userMapper.SelectUserById(id);
    }
}

```

### 服务提供者

 - ServiceProvider~服务提供者模块

**依赖~**

```xml

<dependencies>
    <dependency>
        <artifactId>ServiceAPI</artifactId>
        <groupId>com.codem</groupId>
        <version>1.0-SNAPSHOT</version>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>2.6.5</version>
    </dependency>

    <dependency>
        <groupId>org.apache.zookeeper</groupId>
        <artifactId>zookeeper</artifactId>
        <version>3.8.0</version>
    </dependency>

    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-spring-boot-starter</artifactId>
        <version>3.0.6</version>
    </dependency>

    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-registry-zookeeper</artifactId>
        <version>3.0.6</version>
    </dependency>
<!--zookeeper核心组件-->
    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-recipes</artifactId>
        <version>5.2.1</version>
    </dependency>
    <!--zookeeper核心组件-->
    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>5.2.1</version>
    </dependency>
<!--字节流等转换-->
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.11.0</version>
    </dependency>
<!--    http客户端  与RFC标准有关-->
    <dependency>
        <groupId>org.apache.httpcomponents</groupId>
        <artifactId>httpclient</artifactId>
        <version>4.5.13</version>
    </dependency>
</dependencies>
```
DubboDeptService实现类
```java
@DubboService
public class DubboDeptServiceImpl implements DubboDeptService {
  @Autowired
  private DeptMapper deptMapper;

    @Override
    public List<Dept> queryAll() {

        return deptMapper.queryAll();
    }

    @Override
    public Dept queryByDeptno(int deptno) {
        return deptMapper.queryByDeptno(deptno);
    }

    @Override
    public int insertDept(Dept dept) {
        return deptMapper.insertDept(dept);
    }

    @Override
    public int updateDept(Dept dept) {
        return deptMapper.updateDept(dept);
    }

    @Override
    public int deleteDeptByDeptno(int deptno) {
        return deptMapper.deleteDeptByDeptno(deptno);
    }
}

```
DubboUserServiceImpl
```java
@DubboService
public class DubboUserServiceImpl implements DubboUserService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public int deleteByPrimaryKey(Integer id) {
        return userMapper.deleteByPrimaryKey(id);
    }

    @Override
    public int insert(User record) {
        return userMapper.insert(record);
    }

    @Override
    public User selectByPrimaryKey(Integer id) {
        return userMapper.selectByPrimaryKey(id);
    }

    @Override
    public User SelectUserByName(String username) {
        return userMapper.SelectUserByName(username);
    }

    @Override
    public List<User> SelectUser() {
        return userMapper.SelectUser();
    }

    @Override
    public User SelectUserById(Integer id) {
        return userMapper.SelectUserById(id);
    }
}

```

配置类~

```yml
spring:
  profiles:
    active: mybatis
dubbo:
  application:
    name: dubbo-provider
  registry:
    address: zookeeper://172.23.141.26:2181
    protocol: dubbo
```

启动类
```java
@EnableDubbo
@SpringBootApplication
@MapperScan("com.gavin.mapper")
public class ApplicationServiceProvider {

    public static void main(String[] args) {
        SpringApplication.run(ApplicationServiceProvider.class,args);
    }
}

```

### 服务订阅者

 - **ServiceConsumer服务订阅者模块**

依赖

```xml
  
    <dependencies>
        <dependency>
            <artifactId>ServiceAPI</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <artifactId>Mapper</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <artifactId>Pojo</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>2.6.5</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.6.5</version>
        </dependency>

        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.8.0</version>
        </dependency>

        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>3.0.6</version>
        </dependency>

        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-registry-zookeeper</artifactId>
            <version>3.0.6</version>
        </dependency>
        <!--zookeeper核心组件-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
            <version>5.2.1</version>
        </dependency>
        <!--zookeeper核心组件-->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>5.2.1</version>
        </dependency>
        <!--字节流等转换-->
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.11.0</version>
        </dependency>
        <!--    http客户端  与RFC标准有关-->
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.13</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
            <version>2.6.5</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
            <version>2.6.4</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.8</version>
        </dependency>
    </dependencies>
 <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-springsecurity5</artifactId>
            <version>3.0.3.RELEASE</version>
        </dependency>

```
DeptServiceImpl
```java

@Controller
public class DeptUserController {
    @Autowired
    private DeptService deptService;
@PreAuthorize("hasAuthority('/insert')")
    @ResponseBody
    @RequestMapping("/addDept")
    public String AddDept(Dept dept) {
        int i = deptService.insertDept(dept);
        if (i != 1) {
            return "fail";
        }
        return "success";
    }

    @ResponseBody
    @RequestMapping("/queryDeptByDeptno")

    public Dept queryDeptByDeptno(int deptno) {

        return deptService.queryByDeptno(deptno);
    }

    @ResponseBody
    @RequestMapping("/queryDept")

    public List<Dept> queryDept() {

        return deptService.queryAll();
    }

    @PreAuthorize("hasAuthority('/delete')")
    @ResponseBody
    @RequestMapping("/deleteDeptnoByDeptno")
    public String deleteDeptnoByDeptno(int deptno) {
        int i = deptService.deleteDeptByDeptno(deptno);
        if (i != 1) {
            return "fail";
        }
        return "success";
    }
    @PreAuthorize("hasAuthority('/update')")
    @ResponseBody
    @RequestMapping("/updateDeptByDeptno")
    public String updateDeptByDeptno(Dept dept) {
        int i = deptService.updateDept(dept);
        if (i != 1) {
            return "fail";
        }
        return "success";
    }

    @RequestMapping("/showLogin")
    public String ShowLogin(){

    return "/showLogin";
    }
}

```
准备前端控器与前端资源进行测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/938ad31d91e04a4788f34a921aa7c7bc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

开始测试
dubbo
![在这里插入图片描述](https://img-blog.csdnimg.cn/3c7986c4fa9345d0b4ea0ce6f496da56.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a6fae0dfb59d477e813393f5f2d122fb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
继续测试新增dept

![在这里插入图片描述](https://img-blog.csdnimg.cn/3738fbbac7d54c1e8ade255bb2ee6f8d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9ea296944d0746abb7d22db922084b8c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8a20811e989b40248020716b79549cbc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


这里要注意_csrf

![在这里插入图片描述](https://img-blog.csdnimg.cn/5a67be5cf55143e7aa4c0893056ac0f1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
增加Dept前端界面

```html
<!DOCTYPE html>
<html lang="en"
      xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form method="post" action="/addDept" >
    部门号:<input  type="text" name="deptno"><br>
    部门名称:<input type="text" name="dname"><br>
    位置:<input type="text" name="loc"><br>
    <input type="hidden" th:value="${_csrf.token}" name="_csrf" th:if="${_csrf}"/>
    <input type="submit" value="增加">

</form>

</body>
</html>
```

Solr操作~勾选自动刷新
![在这里插入图片描述](https://img-blog.csdnimg.cn/d90334929c7246579ca83c0fffea1044.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/4a3bae7af03241ef856deff821e78fc0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 

### Solr模块

这里会用到@Field注解,所以原先基础的公共类Dept就不建议在原来基础上进行修改了,这里重新写一个类


先写接口,由于要将接口发布到zookeeper所以还是单独建一个接口模块,然后再建立solrData实体类模块



#### CommonData模块
即SolrData实体模块


依赖

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-solr</artifactId>
        <version>2.4.13</version>
    </dependency>
    
```
searchDept~实体类
```java
package com.gavin.entity;

import org.apache.solr.client.solrj.beans.Field;

import java.io.Serializable;

/**
 * @Description: UP up UP
 * @author: Gavin
 * @date:2022/3/31 14:26
 */
public class searchDept implements Serializable {

    @Field
    private Integer deptno;
    @Field
    private String dname;
    @Field
    private String loc;

    private static final long serialVersionUID = -1L;

    public searchDept() {
    }

    public searchDept(Integer deptno, String dname, String loc) {
        this.deptno = deptno;
        this.dname = dname;
        this.loc = loc;
    }

    public Integer getDeptno() {
        return deptno;
    }

    public void setDeptno(Integer deptno) {
        this.deptno = deptno;
    }

    public String getDname() {
        return dname;
    }

    public void setDname(String dname) {
        this.dname = dname;
    }

    public String getLoc() {
        return loc;
    }

    public void setLoc(String loc) {
        this.loc = loc;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "deptno=" + deptno +
                ", dname='" + dname + '\'' +
                ", loc='" + loc + '\'' +
                '}';
    }
}

```

#### SolrSearchData模块
依赖~

```xml
  <dependency>
        <artifactId>CommonData</artifactId>
        <groupId>com.codem</groupId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-solr</artifactId>
        <version>2.4.13</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.6.5</version>
    </dependency>

```
SearchService
```java

public interface SearchService {

    boolean insert(searchDept searchDept);
}


```
SearchServiceImpl

```java
@Service
public class SearchServiceImpl implements SearchService {

    @Autowired
    private SolrTemplate solrTemplate;
    @Override
    public boolean insert(searchDept searchDept) {
        // in fact to   add Dept
        UpdateResponse new_core = solrTemplate.saveBean("new_core", searchDept);
       //commit
        solrTemplate.commit("new_core");
        int status = new_core.getStatus();
        //根据返回码来判断是否添加成功
        if (status==0){
            return true;
        }
        return false;
    }
}

```

在这之前要注入一个类

```java
@Configuration
public class SolrConfig {

    @Autowired
    SolrClient solrClient;

    @Bean
    public SolrTemplate solrTemplate(){
        return new SolrTemplate(solrClient);
    }
}

```

关于solr的使用在下面这篇文章中已阐述
[Apache-Solr的部署与使用](https://blog.csdn.net/weixin_54061333/article/details/123255700)
[Solr~cloud模式](https://blog.csdn.net/weixin_54061333/article/details/123291110)

Solr控制层

```java
@Controller
public class SearchController {

    @Autowired
    private SearchService searchService;

    @ResponseBody
    @PostMapping("/addSolrDept")
    public boolean SolrAddDept(searchDept searchDept) {
        return searchService.insert(searchDept);


    }

    @RequestMapping("/addSolr")
    public String SolrAddPage() {

        return "addDept";


    }

}


```

前端页面~

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>


<form method="post" action="/addSolrDept">
    部门号:<input type="text" name="deptno"><br>
    部门名称:<input type="text" name="dname"><br>
    位置:<input type="text" name="loc"><br>
    <input type="submit" value="增加">

</form>

</body>
</html>
```
添加数据~
![在这里插入图片描述](https://img-blog.csdnimg.cn/4a27d9b284e9469d89c2c9c0fed63ad8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
返回结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/584ba70f071e4f9f8df0ba1e9d6f5f1b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
Solr中的数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/773dae6cdf404e0393effbdede672030.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
但是本地数据库中并没有该数据,这是因为solr可以存储数据,提供solr直接添加的数据并不会持久化到数据库中而是存在solr中的data文件夹中;
![在这里插入图片描述](https://img-blog.csdnimg.cn/adb4653a654547de9de0897c71f3a98c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

如果线稿持久化到数据库中,那么在调用一次增加数据库中的api即可;
这里通过java代码来操控浏览器

**httpclient客户端**
```java
package com.gavin.utils;

/**
 * @Description: UP up UP
 * @author: Gavin
 * @date:2022/3/31 16:22
 */
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

import org.apache.http.HttpStatus;
import org.apache.http.NameValuePair;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

public class HttpClientUtil {

    private HttpClientUtil() {
    }

    public static String doGet(String url, Map<String, String> param) {

        // 创建Httpclient对象
        CloseableHttpClient httpclient = HttpClients.createDefault();

        String resultString = "";
        CloseableHttpResponse response = null;
        try {
            // 创建uri
            URIBuilder builder = new URIBuilder(url);
            if (param != null) {
                for (String key : param.keySet()) {
                    builder.addParameter(key, param.get(key));
                }
            }
            URI uri = builder.build();

            // 创建http GET请求
            HttpGet httpGet = new HttpGet(uri);

            // 执行请求
            response = httpclient.execute(httpGet);
            // 判断返回状态是否为200
            if (response.getStatusLine().getStatusCode() == 200) {
                resultString = EntityUtils.toString(response.getEntity(), "UTF-8");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (response != null) {
                    response.close();
                }
                httpclient.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return resultString;
    }

    public static String doGet(String url) {
        return doGet(url, null);
    }

    public static String doPost(String url, Map<String, Object> param) {
        // 创建Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        CloseableHttpResponse response = null;
        String resultString = "";
        try {
            // 创建Http Post请求
            HttpPost httpPost = new HttpPost(url);
            // 创建参数列表
            if (param != null) {
                List<NameValuePair> paramList = new ArrayList<>();
                for (String key : param.keySet()) {
                    paramList.add(new BasicNameValuePair(key, (String) param.get(key)));
                }
                // 模拟表单
                UrlEncodedFormEntity entity = new UrlEncodedFormEntity(paramList);
                httpPost.setEntity(entity);
            }
            // 执行http请求
            response = httpClient.execute(httpPost);
            resultString = EntityUtils.toString(response.getEntity(), "utf-8");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (response != null) {
                    response.close();
                }
                httpClient.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return resultString;
    }

    public static String doPost(String url) {
        return doPost(url, null);
    }

    public static String doPostJson(String url, String json,String token_header) throws Exception {
        // 创建Httpclient对象
        CloseableHttpClient httpClient = HttpClients.createDefault();
        CloseableHttpResponse response = null;
        String resultString = "";
        try {
            // 创建Http Post请求
            HttpPost httpPost = new HttpPost(url);
            // 创建请求内容
            httpPost.setHeader("HTTP Method","POST");
            httpPost.setHeader("Connection","Keep-Alive");
            httpPost.setHeader("Content-Type","application/json;charset=utf-8");
            httpPost.setHeader("x-authentication-token",token_header);

            StringEntity entity = new StringEntity(json);

            entity.setContentType("application/json;charset=utf-8");
            httpPost.setEntity(entity);

            // 执行http请求
            response = httpClient.execute(httpPost);
            if (response.getStatusLine().getStatusCode() == HttpStatus.SC_OK) {
                resultString = EntityUtils.toString(response.getEntity(), "UTF-8");
            }
        } catch (Exception e) {
            throw e;
        } finally {
            try {
                if(response!=null){
                    response.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return resultString;
    }


}
```
有了这个,要想调用增加Dept的服务,就需要在元实体类上进行
在这时候在ServiceConsumer中添加
Solr实力类模块依赖
```xml
<dependency>
            <artifactId>CommonData</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>

```
然后修改一下服务订阅者中的代码~

```java
  //这里已新增到数据库为例
    @Override
    public int insertDept(Dept dept) {
      //  Random random = new Random();
        //dept.setDeptno(random.nextInt(100));
        int index = dubboDeptService.insertDept(dept);
        if (index == 1) {
            //数据同步到solr,调用 insert
            Map<String, Object> map=new HashMap<>();
            map.put("deptno",dept.getDeptno().toString());
            map.put("dname",dept.getDname());
            map.put("loc",dept.getLoc());
            String doPost = HttpClientUtil.doPost("http://localhost:8088/addSolrDept", map);//这里采用同步向solr中发送数据而不是直接用实例对象那个来调用方法来实现,其实底层是一样的;
            boolean result=Boolean.parseBoolean(doPost);
            System.out.println("result"+result);

            if(!result){
                //失败则删除数据

            }
        }

        return index;
    }
```

测试代码~

![在这里插入图片描述](https://img-blog.csdnimg.cn/605b4fed759847e4858a3e51e8328eba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

最后要用到RabbitMq继续测试了;

首先新建一个rabbit模块

配置类,这里用的是direct交换器
```java
@Configuration
public class SenderConfig {

    @Bean
    protected Queue SolrDataQueueSolrDataQueue(){
        return new Queue("SolrDataQueue");
    }
}

```
发布数据的类
```java
@Component
public class sendData {

    @Autowired
    private AmqpTemplate amqpTemplate;
    public void sendData(Object obj){
        amqpTemplate.convertAndSend("solrDataQueue",obj);
    }
}

```
然后修改一下服务订阅模块

添加依赖

```xml
  <dependency>
            <artifactId>RabbitMqData</artifactId>
            <groupId>com.codem</groupId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
```
修改后的部分代码
```java
@Service
public class DeptServiceImpl implements DeptService {
    @DubboReference
    private DubboDeptService dubboDeptService;

    @Override
    public List<Dept> queryAll() {
        return dubboDeptService.queryAll();
    }

    @Override
    public Dept queryByDeptno(int deptno) {
        return dubboDeptService.queryByDeptno(deptno);
    }

    @Autowired
    private sendData sendData;


    //保存到数据库中
    @Override
    public int insertDept(Dept dept) {
      //  Random random = new Random();
        //dept.setDeptno(random.nextInt(100));
        int index = dubboDeptService.insertDept(dept);
        if (index == 1) {

       //使用同步方式的solr
         /*   //数据同步到solr,调用 insert
            Map<String, Object> map=new HashMap<>();
            map.put("deptno",dept.getDeptno().toString());
            map.put("dname",dept.getDname());
            map.put("loc",dept.getLoc());
            String doPost = HttpClientUtil.doPost("http://localhost:8088/addSolrDept", map);
            boolean result=Boolean.parseBoolean(doPost);
            System.out.println("result"+result);

            if(!result){
                //失败则删除数据

            }*/

         //使用rabbitMQ方式
            searchDept sDept= new searchDept();
            BeanUtils.copyProperties(dept,sDept);
            sendData.sendData(sDept);

        }

        return index;
    }

    @Override
    public int updateDept(Dept dept) {
        return dubboDeptService.updateDept(dept);
    }

    @Override
    public int deleteDeptByDeptno(int deptno) {
        return dubboDeptService.deleteDeptByDeptno(deptno);
    }


}

```

之后开始测试
>使用rabbitMq是希望通过异步的方式来实现solr与数据库中数据同步


rabbitmq

![在这里插入图片描述](https://img-blog.csdnimg.cn/d9e8a0c094fa4a0e988fc866f6581957.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

说明已经建立了队列,消息还没有被取走;

然后由于还没有完善消息订阅者,所以在solr中并没有新增数据,在sql中已经有了数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/cf5337b4b99347a6a0f4779849430477.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
接下来完善消息订阅者

新建一个订阅者模块
配置文件

```yml
spring:
  rabbitmq:
    host: 172.23.141.26
    username: gavin
    password: 1234
```
取出队列中的对象;
```java

package com.gavin.syncData;

import com.gavin.entity.searchDept;
import com.gavin.utils.HttpClientUtil;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.util.HashMap;
import java.util.Map;

/**
 * @Description: UP up UP
 * @author: Gavin
 * @date:2022/4/1 16:52
 */

@Component
public class syncDept {

    //如果队列中有数据那么就同步过去
    @RabbitListener(queues = "SolrDataQueue")
    public void synData(Object obj) {



        Message message = (Message) obj;
        ByteArrayInputStream bis = null;
        
        ObjectInputStream ois = null;
        try {
            //从数据中取出对象--字节流数据
            bis=new ByteArrayInputStream(message.getBody());
           ois= new ObjectInputStream(bis);
//            取出之后强转
            Object oro = ois.readObject();
            searchDept dept=(searchDept)oro;

//通过httpclient发送到solr
            Map<String, Object> map=new HashMap<>();

            map.put("deptno",dept.getDeptno().toString());
            map.put("dname",dept.getDname());
            map.put("loc",dept.getLoc());
            String doPost = HttpClientUtil.doPost("http://localhost:8088/addSolrDept", map);
            boolean result=Boolean.parseBoolean(doPost);
            System.out.println("result"+result);
            //如果往solr中发送失败了,那么就在数据库中删除数据
            if(!result){

            }
        } catch (Exception ex) {
            ex.printStackTrace();
        }finally {
            if(ois!=null){
                try {
                    ois.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            } if(bis!=null){
                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

}


```
之后进行测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/ae4b81f741534d2da21d63bcb6d6022e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

[源码下载~](https://download.csdn.net/download/weixin_54061333/85065137)
