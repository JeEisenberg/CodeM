![在这里插入图片描述](https://img-blog.csdnimg.cn/1b474160dcc2427d92ae3ce898773d99.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

# MybatisPlus

  1. MyBatis-Plus是MyBatis的强大增强工具。它为MyBatis提供了许多有效的操作。你可以从MyBatis无缝切换到MyBatis-Plus。
  2. MyBatis-Plus可以自动注入基本的SQL片段;
  3. MyBatis-Plus有许多有用的插件（例如代码生成器，自动分页，性能分析等);

依赖
```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus</artifactId>
    <version>latest-version</version>
</dependency>
```

## MybatisPlus的特性

**无侵入：**只做增强不做改变，引入它不会对现有工程产生影响;

**损耗小：**启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作;

**强大的 CRUD 操作：**内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求;

**支持 Lambda 形式调用：**通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错;

**支持主键自动生成：**支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题;

**支持 ActiveRecord 模式：**支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作;

**支持自定义全局通用操作：**支持全局通用方法注入（ Write once， use anywhere ）;

**内置代码生成器：**采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用;

**内置分页插件：**基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询;

**分页插件支持多种数据库：**支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库;

**内置性能分析插件：**可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询;

**内置全局拦截插件：**提供全表删除 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 数据库支持
![在这里插入图片描述](https://img-blog.csdnimg.cn/b9f10b0101ab49c39aa67eb31fe4bc38.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## 框架结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/e1e49e0ec4274cc4b96a26a1d0ef33eb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
mybatisplus是在mybatis的curd基础上及逆行增强,所以需要将mybatis中的SqlSessionFactory替换为调整 SqlSessionFactory 为 MyBatis-Plus 的 SqlSessionFactory
看一下xml中的配置--->>

```xml
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
</bean>
```
所以在0xml中声明sqlSessionFactory实际为mybatisPlus中的;
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ddbd9bc0a7c45219c17ab1576c0094f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
查看源码-->>

```java
 @Bean
    @ConditionalOnMissingBean
    public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
        MybatisSqlSessionFactoryBean factory = new MybatisSqlSessionFactoryBean();
        factory.setDataSource(dataSource);
        factory.setVfs(SpringBootVFS.class);
   .........................省略......................
        return factory.getObject();
    }
```

我们获取一下sqlSessionFactory的全限定名,

```java
package com.codem;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
//@MapperScan("com.codem.mapper")
public class MybatiplusApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(MybatiplusApplication.class, args);        System.out.println(context.getBean("sqlSessionFactory").getClass());
    }
}

```
**全限定名--->**
```java

class org.apache.ibatis.session.defaults.DefaultSqlSessionFactory
```
当dataSource作为参数传入MybatisPlus的SqlSessionFactory时,会做一系列处理-----即增强了之前的SqlSessionFactory;


**整个案例感受一下**
表还是之前的经典表emp与dept

创建一个springboot项目
**父依赖**
```xml
<parent>      <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

**mybatisplus启动器**

```xml
<!--mybatisplus-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
```
**mysql数据库配置**

```yml
spring:
  datasource:
    #    使用druid链接池
    type: com.alibaba.druid.pool.DruidDataSource
    #    数据库要素
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
    username: gavin
    password: 955945

```
**mybatis配置**

```xml
mybatis:
  type-aliases-package: com.gavin.pojo
  mapper-locations: classpath:com/gavin/mapper/*.xml #按照习惯来吧
```
>自动配置的内容
>MyBatis PlusAutoConfiguration配置类,MyBatisPlusProperties配置项前缀 mybatis-plus: ***就是对mybatis-plus的参数的设置

>SQLSessionFactory已经配置好
>mapperlocations 自动配置好的,默认值是classpath*:/mapper/**/*.xml  意为任意包路径下所有的mapper包下的xml文件

**实体类**

```java
package com.codem.pojo;
import java.io.Serializable;
import java.util.Date;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.apache.ibatis.annotations.Mapper;

/**
 * emp
 * @author 
 */

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Emp implements Serializable {
    private Integer empno;

    private String ename;

    private String job;

    private Integer mgr;

    private Date hiredate;

    private Integer sal;

    private Integer comm;

    private Integer deptno;

    private static final long serialVersionUID = 1L;
}
```
**测试类**

```java
package codem;

import com.codem.MybatiplusApplication;
import com.codem.mapper.EmpMapper;
import com.codem.pojo.Emp;
import org.junit.jupiter.api.Test;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest(classes = MybatiplusApplication.class)
//@MapperScan("com.codem.mapper")//扫描mapper所在的包,或者在每一个mapper上夹注解@Mapper
public class MybatiplusApplicationTests {
    @Autowired
 private EmpMapper empMapper;
    @Test
   public void contextLoads() {
        List<Emp> emps = empMapper.selectAll();
        emps.forEach(System.out::println);
    }

}

```
**mapper接口**

```java
package com.codem.mapper;

import com.codem.pojo.Emp;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;

@Mapper

public interface EmpMapper {

    List<Emp> selectAll();

}
```
结果--->>

```html
Emp(empno=7369, ename=SMITH, job=CLERK, mgr=7902, hiredate=Wed Dec 17 00:00:00 CST 1980, sal=800, comm=null, deptno=20)
Emp(empno=7499, ename=ALLEN, job=SALESMAN, mgr=7698, hiredate=Fri Feb 20 00:00:00 CST 1981, sal=1600, comm=300, deptno=30)
Emp(empno=7521, ename=WARD, job=SALESMAN, mgr=7698, hiredate=Sun Feb 22 00:00:00 CST 1981, sal=1250, comm=500, deptno=30)
```

看起来跟之前springbootmybatis没有什么太大的区别,但是这只是基础,
 MyBatis-Plus 提供了大量的个性化配置来满足不同复杂度的工程
Mybatisplus为我们提供了两个接口已简化我们对curd的编写

**BaseMapper<T> extends Mapper<T>**
```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package com.baomidou.mybatisplus.core.mapper;

import com.baomidou.mybatisplus.core.conditions.Wrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.core.toolkit.CollectionUtils;
import com.baomidou.mybatisplus.core.toolkit.ExceptionUtils;
import java.io.Serializable;
import java.util.Collection;
import java.util.List;
import java.util.Map;
import org.apache.ibatis.annotations.Param;

public interface BaseMapper<T> extends Mapper<T> {
    int insert(T entity);//插入

    int deleteById(Serializable id);//删除

    int deleteById(T entity);

    int deleteByMap(@Param("cm") Map<String, Object> columnMap);

    int delete(@Param("ew") Wrapper<T> queryWrapper);

    int deleteBatchIds(@Param("coll") Collection<?> idList);//批量删除

    int updateById(@Param("et") T entity);//更新

    int update(@Param("et") T entity, @Param("ew") Wrapper<T> updateWrapper);

    T selectById(Serializable id);//查找

    List<T> selectBatchIds(@Param("coll") Collection<? extends Serializable> idList);//批量查找

    List<T> selectByMap(@Param("cm") Map<String, Object> columnMap);

    default T selectOne(@Param("ew") Wrapper<T> queryWrapper) {
        List<T> ts = this.selectList(queryWrapper);
        if (CollectionUtils.isNotEmpty(ts)) {
            if (ts.size() != 1) {
                throw ExceptionUtils.mpe("One record is expected, but the query result is multiple records", new Object[0]);
            } else {
                return ts.get(0);
            }
        } else {
            return null;
        }
    }

    default boolean exists(Wrapper<T> queryWrapper) {
        Long count = this.selectCount(queryWrapper);
        return null != count && count > 0L;
    }

    Long selectCount(@Param("ew") Wrapper<T> queryWrapper);

    List<T> selectList(@Param("ew") Wrapper<T> queryWrapper);

    List<Map<String, Object>> selectMaps(@Param("ew") Wrapper<T> queryWrapper);

    List<Object> selectObjs(@Param("ew") Wrapper<T> queryWrapper);

    <P extends IPage<T>> P selectPage(P page, @Param("ew") Wrapper<T> queryWrapper);

    <P extends IPage<Map<String, Object>>> P selectMapsPage(P page, @Param("ew") Wrapper<T> queryWrapper);
}

```
还有另一个接口--->>

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package com.baomidou.mybatisplus.extension.service;

import com.baomidou.mybatisplus.core.conditions.Wrapper;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.core.toolkit.Assert;
import com.baomidou.mybatisplus.core.toolkit.CollectionUtils;
import com.baomidou.mybatisplus.core.toolkit.Wrappers;
import com.baomidou.mybatisplus.extension.conditions.query.LambdaQueryChainWrapper;
import com.baomidou.mybatisplus.extension.conditions.query.QueryChainWrapper;
import com.baomidou.mybatisplus.extension.conditions.update.LambdaUpdateChainWrapper;
import com.baomidou.mybatisplus.extension.conditions.update.UpdateChainWrapper;
import com.baomidou.mybatisplus.extension.kotlin.KtQueryChainWrapper;
import com.baomidou.mybatisplus.extension.kotlin.KtUpdateChainWrapper;
import com.baomidou.mybatisplus.extension.toolkit.ChainWrappers;
import com.baomidou.mybatisplus.extension.toolkit.SqlHelper;
import java.io.Serializable;
import java.util.Collection;
import java.util.List;
import java.util.Map;
import java.util.Objects;
import java.util.function.Function;
import java.util.stream.Collectors;
import org.springframework.transaction.annotation.Transactional;

public interface IService<T> {
    int DEFAULT_BATCH_SIZE = 1000;

    default boolean save(T entity) {//节点存储
        return SqlHelper.retBool(this.getBaseMapper().insert(entity));
    }

    @Transactional(
        rollbackFor = {Exception.class}//回滚
    )
    default boolean saveBatch(Collection<T> entityList) {
        return this.saveBatch(entityList, 1000);//存储,默认1000
    }

    boolean saveBatch(Collection<T> entityList, int batchSize);//节点存储自定义容量

    @Transactional(
        rollbackFor = {Exception.class}
    )
    default boolean saveOrUpdateBatch(Collection<T> entityList) {
        return this.saveOrUpdateBatch(entityList, 1000);
    }

    boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize);

    default boolean removeById(Serializable id) {
        return SqlHelper.retBool(this.getBaseMapper().deleteById(id));
    }

    default boolean removeById(Serializable id, boolean useFill) {
        throw new UnsupportedOperationException("不支持的方法!");
    }

    default boolean removeById(T entity) {
        return SqlHelper.retBool(this.getBaseMapper().deleteById(entity));
    }

    default boolean removeByMap(Map<String, Object> columnMap) {
        Assert.notEmpty(columnMap, "error: columnMap must not be empty", new Object[0]);
        return SqlHelper.retBool(this.getBaseMapper().deleteByMap(columnMap));
    }

    default boolean remove(Wrapper<T> queryWrapper) {
        return SqlHelper.retBool(this.getBaseMapper().delete(queryWrapper));
    }

    default boolean removeByIds(Collection<?> list) {
        return CollectionUtils.isEmpty(list) ? false : SqlHelper.retBool(this.getBaseMapper().deleteBatchIds(list));
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    default boolean removeByIds(Collection<?> list, boolean useFill) {
        if (CollectionUtils.isEmpty(list)) {
            return false;
        } else {
            return useFill ? this.removeBatchByIds(list, true) : SqlHelper.retBool(this.getBaseMapper().deleteBatchIds(list));
        }
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    default boolean removeBatchByIds(Collection<?> list) {
        return this.removeBatchByIds(list, 1000);
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    default boolean removeBatchByIds(Collection<?> list, boolean useFill) {
        return this.removeBatchByIds(list, 1000, useFill);
    }

    default boolean removeBatchByIds(Collection<?> list, int batchSize) {
        throw new UnsupportedOperationException("不支持的方法!");
    }

    default boolean removeBatchByIds(Collection<?> list, int batchSize, boolean useFill) {
        throw new UnsupportedOperationException("不支持的方法!");
    }

    default boolean updateById(T entity) {
        return SqlHelper.retBool(this.getBaseMapper().updateById(entity));
    }

    default boolean update(Wrapper<T> updateWrapper) {
        return this.update((Object)null, updateWrapper);
    }

    default boolean update(T entity, Wrapper<T> updateWrapper) {
        return SqlHelper.retBool(this.getBaseMapper().update(entity, updateWrapper));
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    default boolean updateBatchById(Collection<T> entityList) {
        return this.updateBatchById(entityList, 1000);
    }

    boolean updateBatchById(Collection<T> entityList, int batchSize);

    boolean saveOrUpdate(T entity);

    default T getById(Serializable id) {
        return this.getBaseMapper().selectById(id);
    }

    default List<T> listByIds(Collection<? extends Serializable> idList) {
        return this.getBaseMapper().selectBatchIds(idList);
    }

    default List<T> listByMap(Map<String, Object> columnMap) {
        return this.getBaseMapper().selectByMap(columnMap);
    }

    default T getOne(Wrapper<T> queryWrapper) {
        return this.getOne(queryWrapper, true);
    }

    T getOne(Wrapper<T> queryWrapper, boolean throwEx);

    Map<String, Object> getMap(Wrapper<T> queryWrapper);

    <V> V getObj(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);

    default long count() {
        return this.count(Wrappers.emptyWrapper());
    }

    default long count(Wrapper<T> queryWrapper) {
        return SqlHelper.retCount(this.getBaseMapper().selectCount(queryWrapper));
    }

    default List<T> list(Wrapper<T> queryWrapper) {
        return this.getBaseMapper().selectList(queryWrapper);
    }

    default List<T> list() {
        return this.list(Wrappers.emptyWrapper());
    }

    default <E extends IPage<T>> E page(E page, Wrapper<T> queryWrapper) {
        return this.getBaseMapper().selectPage(page, queryWrapper);
    }

    default <E extends IPage<T>> E page(E page) {
        return this.page(page, Wrappers.emptyWrapper());
    }

    default List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper) {
        return this.getBaseMapper().selectMaps(queryWrapper);
    }

    default List<Map<String, Object>> listMaps() {
        return this.listMaps(Wrappers.emptyWrapper());
    }

    default List<Object> listObjs() {
        return this.listObjs(Function.identity());
    }

    default <V> List<V> listObjs(Function<? super Object, V> mapper) {
        return this.listObjs(Wrappers.emptyWrapper(), mapper);
    }

    default List<Object> listObjs(Wrapper<T> queryWrapper) {
        return this.listObjs(queryWrapper, Function.identity());
    }

    default <V> List<V> listObjs(Wrapper<T> queryWrapper, Function<? super Object, V> mapper) {
        return (List)this.getBaseMapper().selectObjs(queryWrapper).stream().filter(Objects::nonNull).map(mapper).collect(Collectors.toList());
    }

    default <E extends IPage<Map<String, Object>>> E pageMaps(E page, Wrapper<T> queryWrapper) {
        return this.getBaseMapper().selectMapsPage(page, queryWrapper);
    }

    default <E extends IPage<Map<String, Object>>> E pageMaps(E page) {
        return this.pageMaps(page, Wrappers.emptyWrapper());
    }

    BaseMapper<T> getBaseMapper();

    Class<T> getEntityClass();

    default QueryChainWrapper<T> query() {
        return ChainWrappers.queryChain(this.getBaseMapper());
    }

    default LambdaQueryChainWrapper<T> lambdaQuery() {
        return ChainWrappers.lambdaQueryChain(this.getBaseMapper());
    }

    default KtQueryChainWrapper<T> ktQuery() {
        return ChainWrappers.ktQueryChain(this.getBaseMapper(), this.getEntityClass());
    }

    default KtUpdateChainWrapper<T> ktUpdate() {
        return ChainWrappers.ktUpdateChain(this.getBaseMapper(), this.getEntityClass());
    }

    default UpdateChainWrapper<T> update() {
        return ChainWrappers.updateChain(this.getBaseMapper());
    }

    default LambdaUpdateChainWrapper<T> lambdaUpdate() {
        return ChainWrappers.lambdaUpdateChain(this.getBaseMapper());
    }

    default boolean saveOrUpdate(T entity, Wrapper<T> updateWrapper) {
        return this.update(entity, updateWrapper) || this.saveOrUpdate(entity);
    }
}

```
在这个接口中也提供了一些方法,用于简化手写的curd以及批量提交/回滚

所以在自动给注入时我们service,或者mapper可以注入
![在这里插入图片描述](https://img-blog.csdnimg.cn/ee0b31fe5b5a47f3870d28f19b733252.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**接口实现类---ServiceImpl**
```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package com.baomidou.mybatisplus.extension.service.impl;

import com.baomidou.mybatisplus.core.conditions.Wrapper;
import com.baomidou.mybatisplus.core.enums.SqlMethod;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.baomidou.mybatisplus.core.metadata.TableInfo;
import com.baomidou.mybatisplus.core.metadata.TableInfoHelper;
import com.baomidou.mybatisplus.core.toolkit.Assert;
import com.baomidou.mybatisplus.core.toolkit.CollectionUtils;
import com.baomidou.mybatisplus.core.toolkit.GlobalConfigUtils;
import com.baomidou.mybatisplus.core.toolkit.ReflectionKit;
import com.baomidou.mybatisplus.core.toolkit.StringUtils;
import com.baomidou.mybatisplus.extension.service.IService;
import com.baomidou.mybatisplus.extension.toolkit.SqlHelper;
import java.io.Serializable;
import java.util.Collection;
import java.util.Map;
import java.util.Objects;
import java.util.function.BiConsumer;
import java.util.function.Consumer;
import java.util.function.Function;
import org.apache.ibatis.binding.MapperMethod.ParamMap;
import org.apache.ibatis.logging.Log;
import org.apache.ibatis.logging.LogFactory;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.SqlSessionUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.transaction.annotation.Transactional;

public class ServiceImpl<M extends BaseMapper<T>, T> implements IService<T> {
    protected Log log = LogFactory.getLog(this.getClass());
    @Autowired
    protected M baseMapper;
    protected Class<T> entityClass = this.currentModelClass();
    protected Class<M> mapperClass = this.currentMapperClass();

    public ServiceImpl() {
    }

    public M getBaseMapper() {
        return this.baseMapper;
    }

    public Class<T> getEntityClass() {
        return this.entityClass;
    }

    /** @deprecated */
    @Deprecated
    protected boolean retBool(Integer result) {
        return SqlHelper.retBool(result);
    }

    protected Class<M> currentMapperClass() {
        return ReflectionKit.getSuperClassGenericType(this.getClass(), ServiceImpl.class, 0);
    }

    protected Class<T> currentModelClass() {
        return ReflectionKit.getSuperClassGenericType(this.getClass(), ServiceImpl.class, 1);
    }

    /** @deprecated */
    @Deprecated
    protected SqlSession sqlSessionBatch() {
        return SqlHelper.sqlSessionBatch(this.entityClass);
    }

    /** @deprecated */
    @Deprecated
    protected void closeSqlSession(SqlSession sqlSession) {
        SqlSessionUtils.closeSqlSession(sqlSession, GlobalConfigUtils.currentSessionFactory(this.entityClass));
    }

    /** @deprecated */
    @Deprecated
    protected String sqlStatement(SqlMethod sqlMethod) {
        return SqlHelper.table(this.entityClass).getSqlStatement(sqlMethod.getMethod());
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    public boolean saveBatch(Collection<T> entityList, int batchSize) {
        String sqlStatement = this.getSqlStatement(SqlMethod.INSERT_ONE);
        return this.executeBatch(entityList, batchSize, (sqlSession, entity) -> {
            sqlSession.insert(sqlStatement, entity);
        });
    }

    protected String getSqlStatement(SqlMethod sqlMethod) {
        return SqlHelper.getSqlStatement(this.mapperClass, sqlMethod);
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    public boolean saveOrUpdate(T entity) {
        if (null == entity) {
            return false;
        } else {
            TableInfo tableInfo = TableInfoHelper.getTableInfo(this.entityClass);
            Assert.notNull(tableInfo, "error: can not execute. because can not find cache of TableInfo for entity!", new Object[0]);
            String keyProperty = tableInfo.getKeyProperty();
            Assert.notEmpty(keyProperty, "error: can not execute. because can not find column for id from entity!", new Object[0]);
            Object idVal = tableInfo.getPropertyValue(entity, tableInfo.getKeyProperty());
            return !StringUtils.checkValNull(idVal) && !Objects.isNull(this.getById((Serializable)idVal)) ? this.updateById(entity) : this.save(entity);
        }
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    public boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize) {
        TableInfo tableInfo = TableInfoHelper.getTableInfo(this.entityClass);
        Assert.notNull(tableInfo, "error: can not execute. because can not find cache of TableInfo for entity!", new Object[0]);
        String keyProperty = tableInfo.getKeyProperty();
        Assert.notEmpty(keyProperty, "error: can not execute. because can not find column for id from entity!", new Object[0]);
        return SqlHelper.saveOrUpdateBatch(this.entityClass, this.mapperClass, this.log, entityList, batchSize, (sqlSession, entity) -> {
            Object idVal = tableInfo.getPropertyValue(entity, keyProperty);
            return StringUtils.checkValNull(idVal) || CollectionUtils.isEmpty(sqlSession.selectList(this.getSqlStatement(SqlMethod.SELECT_BY_ID), entity));
        }, (sqlSession, entity) -> {
            ParamMap<T> param = new ParamMap();
            param.put("et", entity);
            sqlSession.update(this.getSqlStatement(SqlMethod.UPDATE_BY_ID), param);
        });
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    public boolean updateBatchById(Collection<T> entityList, int batchSize) {
        String sqlStatement = this.getSqlStatement(SqlMethod.UPDATE_BY_ID);
        return this.executeBatch(entityList, batchSize, (sqlSession, entity) -> {
            ParamMap<T> param = new ParamMap();
            param.put("et", entity);
            sqlSession.update(sqlStatement, param);
        });
    }

    public T getOne(Wrapper<T> queryWrapper, boolean throwEx) {
        return throwEx ? this.baseMapper.selectOne(queryWrapper) : SqlHelper.getObject(this.log, this.baseMapper.selectList(queryWrapper));
    }

    public Map<String, Object> getMap(Wrapper<T> queryWrapper) {
        return (Map)SqlHelper.getObject(this.log, this.baseMapper.selectMaps(queryWrapper));
    }

    public <V> V getObj(Wrapper<T> queryWrapper, Function<? super Object, V> mapper) {
        return SqlHelper.getObject(this.log, this.listObjs(queryWrapper, mapper));
    }

    /** @deprecated */
    @Deprecated
    protected boolean executeBatch(Consumer<SqlSession> consumer) {
        return SqlHelper.executeBatch(this.entityClass, this.log, consumer);
    }

    protected <E> boolean executeBatch(Collection<E> list, int batchSize, BiConsumer<SqlSession, E> consumer) {
        return SqlHelper.executeBatch(this.entityClass, this.log, list, batchSize, consumer);
    }

    protected <E> boolean executeBatch(Collection<E> list, BiConsumer<SqlSession, E> consumer) {
        return this.executeBatch(list, 1000, consumer);
    }

    public boolean removeById(Serializable id) {
        TableInfo tableInfo = TableInfoHelper.getTableInfo(this.getEntityClass());
        return tableInfo.isWithLogicDelete() && tableInfo.isWithUpdateFill() ? this.removeById(id, true) : SqlHelper.retBool(this.getBaseMapper().deleteById(id));
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    public boolean removeByIds(Collection<?> list) {
        if (CollectionUtils.isEmpty(list)) {
            return false;
        } else {
            TableInfo tableInfo = TableInfoHelper.getTableInfo(this.getEntityClass());
            return tableInfo.isWithLogicDelete() && tableInfo.isWithUpdateFill() ? this.removeBatchByIds(list, true) : SqlHelper.retBool(this.getBaseMapper().deleteBatchIds(list));
        }
    }

    public boolean removeById(Serializable id, boolean useFill) {
        TableInfo tableInfo = TableInfoHelper.getTableInfo(this.entityClass);
        if (useFill && tableInfo.isWithLogicDelete() && !this.entityClass.isAssignableFrom(id.getClass())) {
            T instance = tableInfo.newInstance();
            tableInfo.setPropertyValue(instance, tableInfo.getKeyProperty(), new Object[]{id});
            return this.removeById(instance);
        } else {
            return SqlHelper.retBool(this.getBaseMapper().deleteById(id));
        }
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    public boolean removeBatchByIds(Collection<?> list, int batchSize) {
        TableInfo tableInfo = TableInfoHelper.getTableInfo(this.entityClass);
        return this.removeBatchByIds(list, batchSize, tableInfo.isWithLogicDelete() && tableInfo.isWithUpdateFill());
    }

    @Transactional(
        rollbackFor = {Exception.class}
    )
    public boolean removeBatchByIds(Collection<?> list, int batchSize, boolean useFill) {
        String sqlStatement = this.getSqlStatement(SqlMethod.DELETE_BY_ID);
        TableInfo tableInfo = TableInfoHelper.getTableInfo(this.entityClass);
        return this.executeBatch(list, batchSize, (sqlSession, e) -> {
            if (useFill && tableInfo.isWithLogicDelete()) {
                if (this.entityClass.isAssignableFrom(e.getClass())) {
                    sqlSession.update(sqlStatement, e);
                } else {
                    T instance = tableInfo.newInstance();
                    tableInfo.setPropertyValue(instance, tableInfo.getKeyProperty(), new Object[]{e});
                    sqlSession.update(sqlStatement, instance);
                }
            } else {
                sqlSession.update(sqlStatement, e);
            }

        });
    }
}

```
通过实现类来实现接口,最终完成功能;

 当有mybatis_plus前缀时会自动去检查下面的配置,比如映射文件地址--->"classpath*:/mapper/**/*.xml"

![在这里插入图片描述](https://img-blog.csdnimg.cn/b50b3a5aad244f6e9e2a9f95e828843b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)  
mybatis与mybatis-plus基本配置对比
```yml

mybatis:
  type-aliases-package: com.codem.pojo
  mapper-locations: classpath:com/codem/mapper/*.xml #按照习惯来吧
#  mybatisplus在mybatis基础上进行强化,但并不是改变mybatis
mybatis-plus:
  type-aliases-package: com.codem.pojo
  mapper-locations: classpath:com/codem/mapper/*.xml #默认值 classpath*:/mapper/**/*.xml
```
mybatisPlus实现curd代码--->>
目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/3a7eb89efadc48ceb4450f043c511fb3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
mapper接口----->>

```java
package com.codem.mapper;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.codem.pojo.Emp;
public interface EmpMapper extends BaseMapper<Emp> {
}
```
service接口
```java
package com.codem.service;

import com.baomidou.mybatisplus.extension.service.IService;
import com.codem.pojo.Emp;
import org.apache.ibatis.annotations.Mapper;
public interface EmpService extends IService<Emp> {
}

```
service接口实现类

```java
package com.codem.service.imp;

import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.codem.mapper.EmpMapper;
import com.codem.pojo.Emp;
import com.codem.service.EmpService;
import org.springframework.stereotype.Service;

@Service
public class empServiceImpl extends ServiceImpl<EmpMapper, Emp> implements EmpService {
}

```
配置注解@SpringBootTest(classes = MybatiplusApplication.class),因为测试类也需要启动springboot环境；
所以在测试类之前还需要在springboot启动类上做一些配置--

```java
package com.codem;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
@MapperScan("com.codem.mapper")//扫描包而不是单个注解mapper
public class MybatiplusApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(MybatiplusApplication.class, args);
        String[] beanDefinitionNames = context.getBeanDefinitionNames();
        for (String definitionName : beanDefinitionNames) {
            System.out.println(definitionName);
        }
        System.out.println("---------------------");
        System.out.println(context.getBean("sqlSessionFactory").getClass());
    }

}

```
测试类---->>
```java
package codem;

import com.codem.MybatiplusApplication;
import com.codem.mapper.EmpMapper;
import com.codem.pojo.Emp;
import com.codem.service.EmpService;
import org.junit.jupiter.api.Test;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest(classes = MybatiplusApplication.class)
//@MapperScan("com.codem.mapper")
public class MybatiplusApplicationTests {
   /* @Autowired
 private EmpMapper empMapper;*/

    @Autowired
    EmpService empService;
    @Test
   public void contextLoads() {
        List<Emp> emps = empService.list();
//        List<Emp> emps = empMapper.selectAll();
        emps.forEach(System.out::println);
    }

}

```
>跟之前对比,对于一些基本的curd我们就不需要在xml配置文件中写对应的sql语句了,但是还是需要xml映射文件的;


[MyBatis-Plus 配置参数详解](https://www.mybatis-plus.com/config/#%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE)  

## MybatisPlus中的注解
mybatisplus注解所在的包

```java
mybatis-plus-annotation
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7c151fd941574872b0520d22ae47a916.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 常用的注解
**@TableName**
表注解
```java
//指定对应的表名,    指定映射 resultMap,可以理解为类构造对应mapper中的resultMap
//autoResultMap---自动构建resultMap

@TableName(value = "emp" ,resultMap ="empconstructor",autoResultMap = true)
public class Emp implements Serializable {
    
```
**@TableId**
主键注解
```java
//主键字段名, 主键--自增
   @TableId(value = "empno",type = IdType.AUTO)
    private Integer empno;
    //IdType.INPUT----insert前自行set主键值
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/058f374a42084f1c897c9047d76c89f0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
  **@TableField**
  属性/字段注解(非主键)
```java
    //属性名与字段名不一致时可以加注解使得其匹配,插入的策略---非空,还有update,where等
    @TableField(value = "ename",insertStrategy = FieldStrategy.NOT_NULL)
    private String ename;
```
>关于`jdbcType`和`typeHandler`以及`numericScale`的说明:
>numericScale只生效于 update 的sql. jdbcType和typeHandler如果不配合@TableName#autoResultMap = true一起使用,也只生效于 update 的sql. 对于typeHandler如果你的字段类型和set进去的类型为equals关系,则只需要让你的typeHandler让Mybatis加载到即可,不需要使用注解

![在这里插入图片描述](https://img-blog.csdnimg.cn/97ff4c7a07f849bf9f2a937cd128296e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/65be4ac732a6408486f2c0a10e7c67f3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


**@OrderBy**
排序,作用域字段/属性/注解上
```
//指定排列顺序优先级低于 wrapper 条件查询
    @OrderBy(asc = false)
//主键字段名,设置主键--自增
  @TableId(value = "empno",type = IdType.AUTO)
    private Integer empno;

```

[其他注解的使用API](https://www.mybatis-plus.com/guide/annotation.html)

## 代码自动生成

## 代码生成器（3.4.1+版本）

>AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

> MyBatis-Plus 从 3.0.3 之后移除了代码生成器与模板引擎的默认依赖，需要手动添加相关依赖：

引入代码自动生成的依赖(非最新版本)---->>

```xml
 <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.4.1</version>
        </dependency>
```
依赖分析
![在这里插入图片描述](https://img-blog.csdnimg.cn/ce6af89be9774b12a816c52d4b686471.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
MyBatis-Plus 支持 Velocity（默认）、Freemarker、Beetl，可以采用自定义模板引擎。

```xml
<!--        模板引擎-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

注意！如果您选择了非默认引擎，需要在 AutoGenerator 中 设置模板引擎。


[官方教程](https://www.mybatis-plus.com/guide/generator.html#%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B)

案例--->>
代码生成器代码

```java
package com;
import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
import com.baomidou.mybatisplus.core.toolkit.StringPool;
import com.baomidou.mybatisplus.core.toolkit.StringUtils;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class CodeGenerator {

    /**
     * <p>
     * 读取控制台内容
     * </p>
     */
    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        //设置生成代码的上一级目录
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotBlank(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        //设置代码生成的位置,默认在根目录下
        gc.setOutputDir(projectPath + "/demo/src/main/java");
        gc.setAuthor("codeM");
        gc.setOpen(false);
        // gc.setSwagger2(true); 实体属性 Swagger2 注解
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/gavin?useUnicode=true&useSSL=false&characterEncoding=utf8");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("gavin");
        dsc.setPassword("955945");
        mpg.setDataSource(dsc);

        // 包配置D:\Program Data\idea2019Data\mybatiplus\demo
        PackageConfig pc = new PackageConfig();
        pc.setModuleName(scanner("模块"));
        //设置java下的顶级目录
        pc.setParent("com");
        mpg.setPackageInfo(pc);

        // 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };

        // 如果模板引擎是 freemarker
        String templatePath = "/templates/mapper.xml.ftl";
        // 如果模板引擎是 velocity
        // String templatePath = "/templates/mapper.xml.vm";

        // 自定义输出配置
        List<FileOutConfig> focList = new ArrayList<>();
        // 自定义配置会被优先输出
        focList.add(new FileOutConfig(templatePath) {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
                return projectPath + "/src/main/resources/mapper/" + pc.getModuleName()
                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        /*
        cfg.setFileCreate(new IFileCreate() {
            @Override
            public boolean isCreate(ConfigBuilder configBuilder, FileType fileType, String filePath) {
                // 判断自定义文件夹是否需要创建
                checkDir("调用默认方法创建的目录，自定义目录用");
                if (fileType == FileType.MAPPER) {
                    // 已经生成 mapper 文件判断存在，不想重新生成返回 false
                    return !new File(filePath).exists();
                }
                // 允许生成模板文件
                return true;
            }
        });
        */
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);

        // 配置模板
        TemplateConfig templateConfig = new TemplateConfig();

        // 配置自定义输出模板
        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别
        // templateConfig.setEntity("templates/entity2.java");
        // templateConfig.setService();
        // templateConfig.setController();

        templateConfig.setXml(null);
        mpg.setTemplate(templateConfig);

        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
//        继承的父类
//        strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");
        strategy.setEntityLombokModel(true);
        strategy.setRestControllerStyle(true);
        // 公共父类
//        strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");
        // 写于父类中的公共字段
        strategy.setSuperEntityColumns("id");
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
        strategy.setControllerMappingHyphenStyle(true);
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }

}
```
目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/b794c742e4d94a18871b2d404346ea8a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
基本按照上面那个模板来就好,
这个模板的思路是---->>

  1. 首先获取指定的文件夹(scanner)
  2. 然后获取配置内容,
  3. 链接数据库 
  4. 定义模板
    mybatis支持的模板引擎---
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/91c90658dc574033b068b3785a841a29.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
    如果使用Thymeleaf,则需要自定义模板,
    这就需要继承
    com.baomidou.mybatisplus.generator.engine.AbstractTemplateEngine
    自定义代码模板

```java
//指定自定义模板路径, 位置：/resources/templates/entity2.java.ftl(或者是.vm)
//注意不要带上.ftl(或者是.vm), 会根据使用的模板引擎自动识别
TemplateConfig templateConfig = new TemplateConfig()
    .setEntity("templates/entity2.java");

AutoGenerator mpg = new AutoGenerator();
//配置自定义模板
mpg.setTemplate(templateConfig);

```

```java
//自定义属性注入
InjectionConfig injectionConfig = new InjectionConfig() {
    //自定义属性注入:abc
    //在.ftl(或者是.vm)模板中，通过${cfg.abc}获取属性
    @Override
    public void initMap() {
        Map<String, Object> map = new HashMap<>();
        map.put("abc", this.getConfig().getGlobalConfig().getAuthor() + "-mp");
        this.setMap(map);
    }
};
AutoGenerator mpg = new AutoGenerator();
//配置自定义属性注入
mpg.setCfg(injectionConfig);
entity2.java.ftl
自定义属性注入abc=${cfg.abc}

entity2.java.vm
自定义属性注入abc=$!{cfg.abc}
#字段其他信息查询注入
```

## 代码生成器（3.5.1+版本）
文件位置配置-->>

location.properties
```properties
jdbc_url=jdbc:mysql://127.0.0.1:3306/gavin?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
jdbc_username=gavin
jdbc_password=955945
location_path=zzy\\src\\main/\\java
mapper_path=zzy\\src\\main\\resources\\mapper
```
代码快速自动生成

```java
package com;

import com.baomidou.mybatisplus.generator.FastAutoGenerator;
import com.baomidou.mybatisplus.generator.config.OutputFile;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;
import org.apache.ibatis.io.Resources;

import java.io.IOException;
import java.io.InputStream;
import java.util.Collections;
import java.util.Properties;

/**
 * @author Gavin
 */
public class FastAutoGeneratorTest {
    public static void main(String[] args) throws IOException {

        InputStream FTT = Resources.getResourceAsStream("config/location.properties");
        Properties properties= new Properties();
        properties.load(FTT);
        String url = properties.getProperty("jdbc_url");
        String username = properties.getProperty("jdbc_username");
        String password = properties.getProperty("jdbc_password");
        String location_path = properties.getProperty("location_path");
        String mapper_path = properties.getProperty("mapper_path");
        FastAutoGenerator.create(url, username,password)
                .globalConfig(builder -> {
                    builder.author("codeM") // 设置作者
                            .enableSwagger() // 开启 swagger 模式
                            .fileOverride() // 覆盖已生成文件
                            .outputDir(location_path); // 指定输出目录
                })
                //D:\Program Data\idea2019Data\mybatiplus\zzy\src\main\java\com
                .packageConfig(builder -> {
                    builder.parent("com") // 设置父包名
                            .moduleName("gavin") // 设置父包模块名/ /zzy/src/main/resources/mapper
                            .pathInfo(Collections.singletonMap(OutputFile.mapperXml, mapper_path)); // 设置mapperXml生成路径
                })
                .strategyConfig(builder -> {
                    builder.addInclude("dept") // 设置表名----要生成实体
                            .addTablePrefix("t_", "c_"); // 设置过滤表前缀
                })
                .templateEngine(new FreemarkerTemplateEngine())  // 使用Freemarker引擎模板，默认的是Velocity引擎模板
                .execute();
    }

}
```
>注意仅适用 3.5.1 以上版本，对历史版本坑会存在不兼容问题）


记录遇到的问题---->>路径问题
如图所示,
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-6i9OGYRy-1644313641190)(https://img-mid.csdnimg.cn/release/static/image/mid/ask/155522313446148.png "#left")\]](https://img-blog.csdnimg.cn/538724c4b42744a1a4ec59bb42ab645a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

解决方法---把 / 改成 \(要转义)

![在这里插入图片描述](https://img-blog.csdnimg.cn/5261e4cd69674b8c97b81e9a01b5f05e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
目录结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/fb6ad2554e20404bb04c26f572340dce.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


交互式生成代码--->>

```java
package com;

import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.generator.FastAutoGenerator;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;
import com.baomidou.mybatisplus.generator.fill.Column;
import org.apache.ibatis.io.Resources;

import java.io.IOException;
import java.io.InputStream;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.Properties;

public class CodeGenerator {

    public static void main(String[] args) throws IOException {

        InputStream FTT = Resources.getResourceAsStream("config/location.properties");

        Properties properties = new Properties();
        properties.load(FTT);
        String url = properties.getProperty("jdbc_url");
        String username = properties.getProperty("jdbc_username");
        String password = properties.getProperty("jdbc_password");
        String location_path = properties.getProperty("location_path");
        String mapper_path = properties.getProperty("mapper_path");
        FastAutoGenerator.create(url, username,password)
                // 全局配置
                .globalConfig((scanner, builder) -> {builder.author(scanner.apply("请输入作者名称？")).fileOverride().outputDir(location_path);})
                // 包配置
                .packageConfig((scanner, builder) -> builder.parent(scanner.apply("请输入包全限定名？")))
                // 策略配置
                .strategyConfig((scanner, builder) -> builder.addInclude(getTables(scanner.apply("请输入表名，多个英文逗号分隔？所有输入 all")))
                        .controllerBuilder().enableRestStyle().enableHyphenStyle()
                        .entityBuilder().enableLombok().addTableFills(
                                new Column("create_time", FieldFill.INSERT)
                        ).build()) .templateEngine(new FreemarkerTemplateEngine())
                /*
                    模板引擎配置，默认 Velocity 可选模板引擎 Beetl 或 Freemarker
                   .templateEngine(new BeetlTemplateEngine())
                   .templateEngine(new FreemarkerTemplateEngine())
                 */
                .execute();


    }

    // 处理 all 情况
    protected static List<String> getTables(String tables) {
        return "all".equals(tables) ? Collections.emptyList() : Arrays.asList(tables.split(","));
    }
}

```
**数据库配置-->**

```
new DataSourceConfig.
    Builder("jdbc:mysql://127.0.0.1:3306/数据库","root","123456").build();
```

**全局配置**

```java
new GlobalConfig.Builder()
    .fileOverride()//覆盖已生成的文件
    .outputDir("/codem/zzy")//输出目录
    .author("codem")//作者
    .enableKotlin()//kotlin模式.默认false
    .enableSwagger()// swagger 模式.默认 false
    .dateType(DateType.TIME_PACK)//时间策略
    .commentDate("yyyy-MM-dd")//注释日期
    .build();
```
包配置

```java
new PackageConfig.Builder()
    parent("com.gavin.mybatisplus.samples.generator")//父包名
    .moduleName("sys")//模块名
    .entity("po")//实体类包名
    .service("service")//service包名
    .serviceImpl("service.impl")
    .mapper("mapper")
    .mapperXml("mapper.xml")
    .controller("controller")
    .other("other")
    .pathInfo(Collections.singletonMap(OutputFile.mapperXml, "D://"))//路径配置
    .build();
```
**模板配置**

```java
new TemplateConfig.Builder()
    .disable(TemplateType.ENTITY)
    .entity("/templates/entity.java")
    .service("/templates/service.java")
    .serviceImpl("/templates/serviceImpl.java")
    .mapper("/templates/mapper.java")
    .mapperXml("/templates/mapper.xml")
    .controller("/templates/controller.java")
    .build();
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a34bea1db6d043759f169be07e921e7e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**注入配置**

```java
new InjectionConfig.Builder()
    .beforeOutputFile((tableInfo, objectMap) -> {
    System.out.println("tableInfo: " + tableInfo.getEntityName() + " objectMap: " + objectMap.size());
    })
    .customMap(Collections.singletonMap("test", "baomidou"))
    .customFile(Collections.singletonMap("test.txt", "/templates/test.vm"))
    .build();
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/ee4ad3bb3662422fa49ed17e4363d3bb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
[**策略配置**](https://www.mybatis-plus.com/guide/generator-new.html#%E7%AD%96%E7%95%A5%E9%85%8D%E7%BD%AE-strategyconfig)

```java
new StrategyConfig.Builder()
    .enableCapitalMode()
    .enableSkipView()
    .disableSqlFilter()
    .likeTable(new LikeTable("USER"))
    .addInclude("t_simple")
    .addTablePrefix("t_", "c_")
    .addFieldSuffix("_flag")
    .build();
```
Entity策略配置
```java
new StrategyConfig.Builder()
    .entityBuilder()
    .superClass(BaseEntity.class)
    .disableSerialVersionUID()
    .enableChainModel()
    .enableLombok()
    .enableRemoveIsPrefix()
    .enableTableFieldAnnotation()
    .enableActiveRecord()
    .versionColumnName("version")
    .versionPropertyName("version")
    .logicDeleteColumnName("deleted")
    .logicDeletePropertyName("deleteFlag")
    .naming(NamingStrategy.no_change)
    .columnNaming(NamingStrategy.underline_to_camel)
    .addSuperEntityColumns("id", "created_by", "created_time", "updated_by", "updated_time")
    .addIgnoreColumns("age")
    .addTableFills(new Column("create_time", FieldFill.INSERT))
    .addTableFills(new Property("updateTime", FieldFill.INSERT_UPDATE))
    .idType(IdType.AUTO)
    .formatFileName("%sEntity")
    .build();
```
**Service策略配置**
```java
new StrategyConfig.Builder()
    .serviceBuilder()
    .superServiceClass(BaseService.class)
    .superServiceImplClass(BaseServiceImpl.class)
    .formatServiceFileName("%sService")
    .formatServiceImplFileName("%sServiceImp")
    .build();
```

**Controller策略配置**

```java
new StrategyConfig.Builder()
    .controllerBuilder()
    .superClass(BaseController.class)
    .enableHyphenStyle()
    .enableRestStyle()
    .formatFileName("%sAction")
    .build();
```
**Mapper策略配置**
```java
new StrategyConfig.Builder()
    .mapperBuilder()
    .superClass(BaseMapper.class)
    .enableMapperAnnotation()
    .enableBaseResultMap()
    .enableBaseColumnList()
    .cache(MyMapperCache.class)
    .formatMapperFileName("%sDao")
    .formatXmlFileName("%sXml")
    .build();
```

## CURD

### Service CRUD 接口
>通用 Service CRUD 封装IService (opens new window)接口，进一步封装 CRUD 采用 get 查询单行 remove 删除 list 查询集合 page 分页 前缀命名方式区分 Mapper 层避免混淆


>泛型 T 为任意实体对象

>建议如果存在自定义通用 Service 方法的可能，请创建自己的 IBaseService 继承 Mybatis-Plus 提供的基类
>对象 Wrapper 为 条件构造器


在自动生成代码的基础上去实现增删查改


```java
package gavin;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("gavin/mapper")//mapper扫描
public class ZzyApplication {

    public static void main(String[] args) {
        SpringApplication.run(ZzyApplication.class, args);
    }

}

```

**CURD方法**

 **save方法**

```java
// 插入一条记录（选择字段，策略插入）
boolean save(T entity);
// 插入（批量）
boolean saveBatch(Collection<T> entityList);
// 插入（批量）
boolean saveBatch(Collection<T> entityList, int batchSize);
```


```java
package com.zzy;

import com.baomidou.mybatisplus.core.toolkit.Wrappers;
import gavin.ZzyApplication;
import gavin.entity.Dept;
import gavin.service.DeptService;
import org.junit.jupiter.api.Test;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.ArrayList;
import java.util.List;

@SpringBootTest(classes = ZzyApplication.class)
class ZzyApplicationTests {
@Autowired
    private DeptService deptService;
    @Test
    void contextLoads() {
        List<Dept> list = deptService.list();
        list.forEach(System.out::println);
    }
//    带条件查询
    @Test
    void contextLoads2() {
        //插入数据
        deptService.save(new Dept(50,"Manager","YT"));
        //批量插入数据
List<Dept> list= new ArrayList<Dept>(2);
list.add(new Dept(60,"Manager","YT"));
list.add(new Dept(70,"Manager","YTT"));
list.add(new Dept(80,"Manager","YTTT"));
//批量提交,默认1000
deptService.saveBatch(list);
//批量插入数据
List<Dept> depts= new ArrayList<Dept>(2);
depts.add(new Dept(90,"Manager","YT"));
depts.add(new Dept(100,"Manager","YTT"));
depts.add(new Dept(110,"Manager","YTTT"));
depts.add(new Dept(120,"Manager","YTTTT"));

//批量提交,每次提交2个
deptService.saveBatch(depts,2);
        List<Dept> list1 = deptService.list();
        list1.forEach(System.out::println);
    }

}

```

**SaveOrUpdate**
1,该方法要求有TableId注解,如果没有TableId主键注解,则报错`com.baomidou.mybatisplus.core.exceptions.MybatisPlusException: error: can not execute. because can not find column for id from entity!`
 2,根据是否有主键值来决定是执行更新还是存储,如果有则更新,否则就存储,
3,如果带有条件构造器,那么满足条件的执行,否则不执行;

```java
// TableId 注解  存在更新记录，否插入一条记录
boolean saveOrUpdate(T entity);
// 根据updateWrapper尝试更新，否继续执行saveOrUpdate(T)方法
boolean saveOrUpdate(T entity, Wrapper<T> updateWrapper);

// 批量修改插入
boolean saveOrUpdateBatch(Collection<T> entityList);
// 批量修改插入
boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize);
```

实践代码--->>

```java
 @Test
    void contextLoads3() {
        //该方法要求有TableId注解,
        //根据是否有主键值来决定是执行更新还是存储,
        //如果有则更新,否则就存储,
       Dept dept = new Dept(180, "Manager", "黑龙江");
       deptService.saveOrUpdate(dept);
    }
      
```
原来数据--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/4aa3cb2a73c74bbb99e363e82a341693.png)
执行后;
![在这里插入图片描述](https://img-blog.csdnimg.cn/69e09205dd584fce95059fcccdfcc562.png)
带有条件构造器的部分代码

```java
        Dept dept1= new Dept(180, "Manager", "黑龙江齐齐哈尔");
        UpdateWrapper<Dept> wrappers= new UpdateWrapper();
        wrappers.ge("deptno",100);//相当于deptno>=100的,都更新了
        deptService.saveOrUpdate(dept1,wrappers);

```
结果---大于deptno>=100的都更新了
![在这里插入图片描述](https://img-blog.csdnimg.cn/74fdef8f55ab47a6ab000ea324189420.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
当没有大于100的纪录时,会将该值插入;


```java

//        批量修改插入
        List<Dept> list= new ArrayList<Dept>(2);
     list.add(new Dept(300,"Clerk","QD"));//存在
     list.add(new Dept(400,"Clerk","QQD"));//不存在
     list.add(new Dept(500,"Clerk","QDD"));//不存在
        deptService.saveOrUpdateBatch(list);
        //        批量修改插入
        List<Dept> list1= new ArrayList<Dept>(2);
     list.add(new Dept(3000,"Clerk","QD"));//存在
     list.add(new Dept(4000,"Clerk","QQD"));//不存在
     list.add(new Dept(5000,"Clerk","QDD"));//不存在
        deptService.saveOrUpdateBatch(list,2);
```
运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/c951eb7dc86e4cf4910b95e45de5f90d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**Remove**

```java
// 根据 entity 条件，删除记录
boolean remove(Wrapper<T> queryWrapper);
// 根据 ID 删除
boolean removeById(Serializable id);
// 根据 columnMap 条件，删除记录
boolean removeByMap(Map<String, Object> columnMap);
// 删除（根据ID 批量删除）
boolean removeByIds(Collection<? extends Serializable> idList);
```


实践代码-->

```java
 @Test
    void contextLoads4() {
        Dept dept = new Dept(180, "Manager", "黑龙江");
        deptService.removeById(dept);
        
        UpdateWrapper<Dept> wrappers= new UpdateWrapper();
        wrappers.ge("deptno",100);
        deptService.remove(wrappers);
        List<Dept> list= new ArrayList<Dept>(2);
        list.add(new Dept(300,"Clerk","QD"));//存在
        list.add(new Dept(400,"Clerk","QQD"));//不存在
        list.add(new Dept(500,"Clerk","QDD"));//不存在
            deptService.removeBatchByIds(list,2)   ; 
       deptService.removeById(90);//可以直接根据主键删除
      //可以根据map来删除
         Map<String ,Object>map = new HashMap();
        map.put("deptno",70);
        deptService.removeByMap(map);
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/82a5f589f3f94afea295ab15c3196ad8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**Update**

```java
// 根据 UpdateWrapper 条件，更新记录 需要设置sqlset
boolean update(Wrapper<T> updateWrapper);
// 根据 whereWrapper 条件，更新记录
boolean update(T updateEntity, Wrapper<T> whereWrapper);
// 根据 ID 选择修改
boolean updateById(T entity);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList, int batchSize);
```

跟上面remove类似,实践代码略

**Get**

```java
// 根据 ID 查询
T getById(Serializable id);
// 根据 Wrapper，查询一条记录。结果集，如果是多个会抛出异常，随机取一条加上限制条件 wrapper.last("LIMIT 1")
T getOne(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
T getOne(Wrapper<T> queryWrapper, boolean throwEx);
// 根据 Wrapper，查询一条记录
Map<String, Object> getMap(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
<V> V getObj(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```
略

**List**

```java
// 查询所有
List<T> list();
// 查询列表
List<T> list(Wrapper<T> queryWrapper);
// 查询（根据ID 批量查询）
Collection<T> listByIds(Collection<? extends Serializable> idList);

// 查询（根据 columnMap 条件）
Collection<T> listByMap(Map<String, Object> columnMap);
// 查询所有列表
List<Map<String, Object>> listMaps();
// 查询列表
List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper);
// 查询全部记录
List<Object> listObjs();
// 查询全部记录
<V> List<V> listObjs(Function<? super Object, V> mapper);
// 根据 Wrapper 条件，查询全部记录
List<Object> listObjs(Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询全部记录
<V> List<V> listObjs(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```

实践代码-->

```java
 @Test
    void contextLoad5() {
        QueryWrapper<Dept> wrappers= new QueryWrapper<>();
        Map<String ,Object>map = new HashMap();
        map.put("deptno",30);
// 查询（根据 columnMap 条件）
        List<Dept> list = deptService.listByMap(map);
        list.forEach(System.out::println);
/*
        Map<String ,Object>map1 = new HashMap();
        map.put("loc","YT");
        */
        System.out.println("------------------");
        List<Map<String, Object>> maps = deptService.listMaps(wrappers.gt("deptno",20));
        maps.forEach(System.out::println);
        
        deptService.listObjs(wrappers).forEach(System.out::println);
    }
```
**Page**

[分页插件](https://www.mybatis-plus.com/guide/page.html)

```java
// 无条件分页查询
IPage<T> page(IPage<T> page);
// 条件分页查询
IPage<T> page(IPage<T> page, Wrapper<T> queryWrapper);
// 无条件分页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page);
// 条件分页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page, Wrapper<T> queryWrapper);
```



分页---实现类

```java
public class Page<T> implements IPage<T> {
    private static final long serialVersionUID = 8545996863226528798L;
    protected List<T> records;
    protected long total;
    protected long size;
    protected long current;
    protected List<OrderItem> orders;
    protected boolean optimizeCountSql;
    protected boolean searchCount;
    protected boolean optimizeJoinOfCountSql;
    protected String countId;
    protected Long maxLimit;

```
实现分页功能

```java
  @Test
    void contextLoads6() {
        Page<Dept> pageB= new Page<Dept>(2,2);//可以通过构造方法直接指定分页情况
        //  page.setSize(2); //可通过方法设置属性
        Page<Dept> page = deptService.page(pageB);
        List<Dept> list = page.getRecords();
//        list.forEach(System.out::println);
        for (Dept dept : list) {
            System.out.println(dept);
        }
        System.out.println("---------------");
        Map<String ,Object> map=  new HashMap<>();
        map.put("deptno",60);
        Page< Map<String ,Object>> page1=new Page<>(2,2);
        Page<Map<String, Object>> mapPage = deptService.pageMaps(page1);
        mapPage.getRecords().forEach(System.out::println);
        
    }
```
运行结果--
![在这里插入图片描述](https://img-blog.csdnimg.cn/509a04ee321043b2a8670028aa7cd6d8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
发现并没有分页的效果,debug一下发现,没有limit条件,
![在这里插入图片描述](https://img-blog.csdnimg.cn/e65f6af1cc6a417990d0054a16210e00.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2b4b04b127814cf0b2a6a39493ab61b2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
原来随着mybatisplus的本本更迭,需要配置interceptor来配合实现分页
如果是3.4.1版本的mubatisplus
做如下配置
```java
@Configuration
@MapperScan("com.gavin.mapper")
public class MyInterceptorConfig {
       @Bean
       public PaginationInterceptor paginationInterceptor() {
           PaginationInterceptor paginationInterceptor = new PaginationInterceptor();                    paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
           return paginationInterceptor;
       }
```
最新版本有些改动----改成了PaginationInnerInterceptor

```java

@Configuration
@MapperScan("com.gavin.mapper")
public class MyInterceptorConfig {

    // 最新版

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b34cd2450894d819824e69deda08602.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**Count**

```
// 查询总记录数
int count();
// 根据 Wrapper 条件，查询总记录数
int count(Wrapper<T> queryWrapper);
```

略...


**Chain**

```java
#query
// 链式查询 普通
QueryChainWrapper<T> query();
// 链式查询 lambda 式。注意：不支持 Kotlin
LambdaQueryChainWrapper<T> lambdaQuery(); 
```

```java
  @Test
    void contextLoads7() {
        
        QueryChainWrapper<Dept> query = deptService.query()
        //        可以继续查询,这就类似于 stringbuffer.stringbuilder
       .groupBy("loc")
        .ge("deptno", 30)
         .in("deptno",40,50,60);
        BaseMapper<Dept> baseMapper = query.getBaseMapper();
        List<Dept> deptno = baseMapper.selectList(new QueryWrapper<Dept>().gt("deptno", 40));
        deptno.forEach(System.out::println);
        System.out.println("---------------");
        deptService.query().eq("loc","YT").list().forEach(System.out::println);
        System.out.println("---------------");
        LambdaQueryChainWrapper<Dept> dequery = deptService.lambdaQuery();
        dequery.eq(Dept::getDeptno,"YT").list().forEach(System.out::println);
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/73f042a46b264745965f773e88a36b17.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**update**
```java

// 链式更改 普通
UpdateChainWrapper<T> update();
// 链式更改 lambda 式。注意：不支持 Kotlin 
LambdaUpdateChainWrapper<T> lambdaUpdate();

// 示例：
update().eq("column", value).remove();
lambdaUpdate().eq(Entity::getId, value).update(entity);
```
略....同链式查询



### Mapper CRUD 接口

>通用 CRUD 封装BaseMapper (opens new window)接口，为 Mybatis-Plus 启动时自动解析实体表关系映射**转换为 Mybatis 内部对象注入容器**
>泛型 T 为任意实体对象
>参数 Serializable 为任意类型主键 Mybatis-Plus 不推荐使用复合主键约定每一张表都有自己的**唯一 id 主键**
>对象 Wrapper 为 条件构造器

**CURD方法**

**insert**

```java
// 插入一条记录
int insert(T entity);
```
实践代码..略


**Delete**

```java
// 根据 entity 条件，删除记录
int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);
// 删除（根据ID 批量删除）
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
// 根据 ID 删除
int deleteById(Serializable id);
// 根据 columnMap 条件，删除记录
int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
```
实践代码
```java
@Test
    void contextLoads8() {
deptMapper.delete(new QueryWrapper<Dept>().eq("deptno",80));
    }
```
**Update**

```java
// 根据 whereWrapper 条件，更新记录
int update(@Param(Constants.ENTITY) T updateEntity, @Param(Constants.WRAPPER) Wrapper<T> whereWrapper);
// 根据 ID 修改
int updateById(@Param(Constants.ENTITY) T entity);
```
略

**Select**

```java
// 根据 ID 查询
T selectById(Serializable id);
// 根据 entity 条件，查询一条记录
T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 查询（根据ID 批量查询）
List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
// 根据 entity 条件，查询全部记录
List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
// 查询（根据 columnMap 条件）
List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
// 根据 Wrapper 条件，查询全部记录
List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询全部记录。注意： 只返回第一个字段的值
List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 entity 条件，查询全部记录（并翻页）
IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询全部记录（并翻页）
IPage<Map<String, Object>> selectMapsPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
// 根据 Wrapper 条件，查询总记录数
Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```
略

mapper 层 选装件
说明:

选装件位于 com.baomidou.mybatisplus.extension.injector.methods 包下 需要配合Sql 注入器使用,案例(opens new window)
使用详细见源码注释(opens new window)

```java
#AlwaysUpdateSomeColumnById

int alwaysUpdateSomeColumnById(T entity);

#insertBatchSomeColumn

int insertBatchSomeColumn(List<T> entityList);

#logicDeleteByIdWithFill

int logicDeleteByIdWithFill(T entity);
```
> 用service层操作是推荐的,mapper层操作不太推荐;


Wappers是一个条件构造器,
AbstractWrapper

>QueryWrapper(LambdaQueryWrapper) 和 UpdateWrapper(LambdaUpdateWrapper) 的父类
>用于生成 sql 的 where 条件, entity 属性也用于生成 sql 的 where 条件
>注意: entity 生成的 where 条件与 使用各个 api 生成的 where 条件没有任何关联行为

更多内容请查看---->>>[Wappers的API](https://www.mybatis-plus.com/guide/wrapper.html)