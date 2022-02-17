[![在这里插入图片描述](https://img-blog.csdnimg.cn/29cb4e8427a34489a3870281db3085a6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)](https://junit.org/junit5/)

# Junit5

描述:
JUnit 5 是 JUnit4的下一代产品。与以前版本的 JUnit 不同，JUnit 5 由来自三个不同子项目的几个不同模块组成。

```
JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage
```

目标是为 JVM 上的开发人员端测试创建最新的基础。这包括专注于Java 8及更高版本，以及支持许多不同风格的测试。
>springboot 2.2.0开始引入Junit5作为单元测试的默认库

在spring test中已经导入了关于junit5的相关依赖

![在这里插入图片描述](https://img-blog.csdnimg.cn/e10c70fb6cbe40ae85fe8ab06b9a2500.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>JUnit Jupiter,提供了Junit5的最新的编程模型,是Junit5 的核心,内部包含了一个测试引擎,用于在Junit Platform上运行

>JUnit Vintager: 提供了兼容Junit4/3 的测试引擎

## 注解

**Junit5中的注解与Junit4中的注解**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2143f056c44a44a290a79d5fab45771e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 常用的注解:
 **标准测试类**

```java
import static org.junit.jupiter.api.Assertions.fail;
import static org.junit.jupiter.api.Assumptions.assumeTrue;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class StandardTests {

    @BeforeAll
    static void initAll() {
    }

    @BeforeEach
    void init() {
    }

    @Test
    void succeedingTest() {
    }

    @Test
    void failingTest() {
        fail("a failing test");
    }

    @Test
    @Disabled("for demonstration purposes")
    void skippedTest() {
        // not executed
    }

    @Test
    void abortedTest() {
        assumeTrue("abc".contains("Z"));
        fail("test should have been aborted");
    }

    @AfterEach
    void tearDown() {
    }

    @AfterAll
    static void tearDownAll() {
    }

}
```
**生命周期演示**
```java

@SpringBootTest(classes = Demo351Application.class)
class Demo351ApplicationTests {
    @Autowired
    private IDeptService deptService;
    @Autowired
    private DeptMapper deptMapper;

@BeforeEach////每次执行之后
void contextBeforeEach(){
    System.out.println("beforeeach");
}
@AfterEach//每次执行之前
void contextAfterEach(){
    System.out.println("Aftereach");
}

@BeforeAll//所有方法执行之前,只执行一次
static void contextBeforeAll(){
    System.out.println("beforeall");
}
@AfterAll//所有方法执行之后,只执行一次
static void contextAfterAll(){
    System.out.println("afterall");
}
@RepeatedTest(2)//重复2次
@Timeout(value = 5,unit = TimeUnit.SECONDS)//超时设置
@DisplayName(value = "测试方法")//测试方法描述
@Test
public  void contestTest(){
    System.out.println("ContextTest");
    deptService.list(new QueryWrapper<Dept>().eq("deptno",10)).forEach(System.out::println);
}
@Disabled//设置该测试方法不可用
@Test
public void contexTest1(){
    System.out.println("测试方法描述");
}

```
注意--->>
**@AfterAll与@BeforeAll** 注解下的方法要为静态方法;

方法的执行顺序

```c
beforeall
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.6.3)
           main] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} inited
 _ _   |_  _ _|_. ___ _ |    _ 
| | |\/|_)(_| | |_\  |_)||_|_\ 
     /               |         
                        3.5.1 


beforeeach
ContextTest
Dept{deptno=10, dname=ACCOUNTING, loc=NEW YORK}
Aftereach


beforeeach
ContextTest
Dept{deptno=10, dname=ACCOUNTING, loc=NEW YORK}
Aftereach


beforeeach
ContextTest
Dept{deptno=10, dname=ACCOUNTING, loc=NEW YORK}
Aftereach

afterall
```
**显示名称**
可以没有该注解,但如果 
```java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("A special test case")
class DisplayNameDemo {

    @Test
    @DisplayName("Custom test name containing spaces")
    void testWithDisplayNameContainingSpaces() {
    }

    @Test
    @DisplayName("╯°□°）╯")
    void testWithDisplayNameContainingSpecialCharacters() {
    }

    @Test
    @DisplayName("😱")
    void testWithDisplayNameContainingEmoji() {
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/75d159e35787478690dbbbf2456c8772.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**Junit5对事务的支持**

```

@Transactional//测试时回滚
    @Test
    void standardAssertions() {
        boolean deptno = deptService.update(new Dept(10,"MybatisPlus","SD"),new UpdateWrapper<Dept>().eq("deptno", 10));


    }

```

## 断言机制

JUnit Jupiter附带了许多JUnit 4拥有的断言方法，并添加了一些非常适合与Java 8 lambdas一起使用的断言方法。所有 JUnit Jupiter 断言都是类中的方法。staticorg.junit.jupiter.api.Assertions

倘若没有断言机制,那么我们要想知道结果是否如预期,那么就需要通过在控制台打印来查看是否符合预期,这样当结果很多时,不便于观察;

**断言中的方法**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c28ef8de75c74fafb93ba1e93d3bd905.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**assertEquals**与**assertNotEquals**
```java
 
//@Transactional//测试时回滚
    @Test
    void standardAssertions() {
    List<Dept> list = deptService.list(new QueryWrapper<Dept>().eq("loc", "YT"));

    Assertions.assertEquals(50,list.get(0).getDeptno(),"存在该职员");//
    Assertions.assertNotEquals(10,list.get(0).getDeptno(),"不存在该职员");//

    }

    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/48a0ed8349ab46b19fea3d3ac0d63cd1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
以上是一个简单的断言,断言还可以组合着来
**assertAll**
```java
  // 组合断言
    @DisplayName("组合断言")
    @Test
    void GroupAssertions2() {
        List<Dept> list = deptService.list(new QueryWrapper<Dept>().eq("loc", "YT"));//50,60
        boolean boston = deptService.lambdaQuery().eq(Dept::getLoc, "BOSTON").list().get(0).getDeptno().equals(30);
        Assertions.assertAll("开头",()->Assertions.assertTrue(list.get(0).getDeptno().equals(50),"正确"),()->Assertions.assertFalse(deptService.lambdaQuery().eq(Dept::getLoc,"BOSTON").list().get(0).getDeptno().equals(30),"错误"));
            Assertions.assertAll("AssertAll",()-> Assertions.assertTrue(true && true),
                    ()-> Assertions.assertEquals(1,1));
    }
//就类似于链式
```



**assertSame**与**assertNotSame**
```java

@DisplayName("same与unsame")
@Test
void sameAssertion() {
        String str="MybatisPlus";
        String str1=new String("MybatisPlus");
        String str2="Mybatis"+"Plus";
        //Assertions.assertSame(str,str1,"我们不一样?");//false
        Assertions.assertSame(str,str2,"我们不一样?");//false
        Assertions.assertNotSame(str,str1,"我们一样?");//false
}

```
**assertNull**与**assertNotNull**
```java

    @DisplayName("null与notnull")
    @Test
    void nullAssertion() {
        List<Dept> list = deptService.list(new QueryWrapper<Dept>().eq("loc", "YTT"));//查无此值~但不是null
        List<Dept> list2 = deptService.list(new QueryWrapper<Dept>().eq("loc", "YT"));//查无此值~null
        list = null;
        Assertions.assertNull(list);
        Assertions.assertNotNull(list2);

    }


```
 **assertThrows**

```java
// 异常断言 认为应该会出现异常
    @DisplayName("异常断言")
    @Test
    public void testAssertException(){
        Assertions.assertThrows(ArithmeticException.class, ()->{ int i=100/0;}, "没有抛出异常");
    }
  @Test
    void exceptionTesting() {
        Exception exception = assertThrows(ArithmeticException.class, () ->
                Math.floorDiv(10, 0));
        assertEquals("/ by zero", exception.getMessage());
    }
```
断言的规则---
在同一个块里,上面那个断言不通过,下面的就不会执行,就像块里有异常一样;
```java
 @Test
    void dependentAssertions() {
        // Within a code block, if an assertion fails the
        // subsequent code in the same block will be skipped.
        assertAll("testdepend",
                () -> {
                    List<Dept> list = deptService.list(new QueryWrapper<Dept>().eq("loc", "YT"));
                    assertNotNull(list);//true

                    // Executed only if the previous assertion is valid.
                    assertAll("Manager",
                            () -> assertTrue(list.get(0).getDname().equals("Manager")),//false
                            () -> assertTrue(list.get(1).getDname().endsWith("er"))//true
                    );
                },

                () -> {
                    // Grouped assertion, so processed independently
                    // of results of first name assertions.
                    List<Dept> list = deptService.list(new QueryWrapper<Dept>().eq("deptno", "10"));
                    String loc = list.get(0).getLoc();
                    assertNotNull(loc);
//在同一个块里,上面那个断言不通过,下面的就不会执行
                    // Executed only if the previous assertion is valid.
                    assertAll("loc",
                            () -> assertTrue(loc.startsWith("N")),
                            () -> assertTrue(loc.endsWith("k"))
                    );
                }
        );
    }


```
**assertTimeout**


```java
> // 超时断言 判断有没有超时
>     @DisplayName("超时断言")
>     @Test
>     public void testAssertTimeOut(){
>         Assertions.assertTimeout(Duration.ofMillis(1000),()-> Thread.sleep(5000));
>     }

 @Test
    void timeoutNotExceededWithMethod() {
        // The following assertion invokes a method reference and returns an object.
      String actualGreeting = assertTimeout(ofNanos(1), MyUser::Greeting/*睡1000ms*/);
//        如果不超时,则正常返回结果,否则不返回结果
        assertEquals("Hello", actualGreeting);
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b1fde5472c0a4fce8aa55920b1a9df88.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/61669184904e408187b7aee59b4933eb.png)




 **快速失败**

```java
   // 快速失败
    @DisplayName("快速失败")
    @Test
    public void testFail(){
        if(true){
            Assertions.fail("测试 失败");
        }
    }
```

**假设**
JUnit Jupiter 附带了 JUnit 4 提供的假设方法的子集，并添加了一些非常适合与 Java 8 lambda 表达式和方法引用一起使用的假设方法。所有 JUnit Jupiter 假设都是类中的静态方法。org.junit.jupiter.api.Assumptions

**assumeTrue**与**assumeFalse**
```java
//    假设

    @Test
    void testOnlyOnCiServer() {

        assumeTrue("Hello".equals("Hello"));
        assumeTrue(1==1,
                () -> "Aborting test: not on developer workstation");
        // remainder of test
    }
//同理assumeFalse
```

**assumingThat**

假定某一个条件,然后执行下面的代码,这张方式一般用于触发指定条件
```java
@Test
    void testInAllEnvironments() {
        assumingThat("Start".equals("End"),
                () -> {
                    // perform these assertions only on the CI server,在CI开发环境中
                    assertEquals(2, Math.floorDiv(4, 2));
                });

        // perform these assertions in all environments//在所有环境中
        assertEquals(7, Math.max(6, 7));
    }
```
官方给出的案例--->>

```java
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assumptions.assumeTrue;
import static org.junit.jupiter.api.Assumptions.assumingThat;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class AssumptionsDemo {

    private final Calculator calculator = new Calculator();

    @Test
    void testOnlyOnCiServer() {
        assumeTrue("CI".equals(System.getenv("ENV")));
        // remainder of test
    }

    @Test
    void testOnlyOnDeveloperWorkstation() {
        assumeTrue("DEV".equals(System.getenv("ENV")),
            () -> "Aborting test: not on developer workstation");
        // remainder of test
    }

    @Test
    void testInAllEnvironments() {
        assumingThat("CI".equals(System.getenv("ENV")),
            () -> {
                // perform these assertions only on the CI server
                assertEquals(2, calculator.divide(4, 2));
            });

        // perform these assertions in all environments
        assertEquals(42, calculator.multiply(6, 7));
    }

}
```

>从JUnit Jupiter 5.4开始，也可以使用JUnit 4类中的方法进行假设。具体来说，JUnit Jupiter支持JUnit 4，以指示测试应该中止而不是标记为失败。org.junit.AssumeAssumptionViolatedException


## 条件测试

**禁用测试**

@Disabled可以在没有提供理由的情况下声明; 然而，对禁用测试类或测试方法的原因提供一个简短的解释;

```java
 @Disabled("Disabled until bug #42 has been resolved")
    @Test
    void testWillBeSkipped() {
        System.out.println("testWillBeSkipped执行了");
    }

    @Test
    void testWillBeExecuted() {
        System.out.println("testWillBeExecuted执行了");
    }
```
>禁用测试并不代表该测试不能够正常运行,而是为了标注该测试下会有一些问题,该注解用于在自动可追溯性等原因中存在问题进行跟踪;

**指定测试时的操作系统**
容器或测试可以通过 @EnabledOnOs和 @DisabledOnOs注释在特定操作系统上启用或禁用。
```java
 @Test
    @EnabledOnOs(WINDOWS)
    void onlyOnWINDOWS() {
        // ...
    }
```
注解参数值---->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/4fbfbbd040ed4f80b785287da3677812.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


**指定测试时的Java 运行时环境**
容器或测试可以通过 @EnabledOnJre@DisabledOnJre明确 运行时环境 （JRE） 也可以通过@EnabledForJreRange@DisabledForJreRange指定在 JRE 的特定版本上启用或禁用。该范围默认为下边框 （） 和较高边框 （），这允许使用半个开放范围。

```java
@Test
@EnabledOnJre(JAVA_8)
void onlyOnJava8() {
    // ...
}

@Test
@EnabledOnJre({ JAVA_9, JAVA_10 })
void onJava9Or10() {
    // ...
}

@Test
@EnabledForJreRange(min = JAVA_9, max = JAVA_11)
void fromJava9to11() {
    // ...
}

@Test
@EnabledForJreRange(min = JAVA_9)
void fromJava9toCurrentJavaFeatureNumber() {
    // ...
}

@Test
@EnabledForJreRange(max = JAVA_11)
void fromJava8To11() {
    // ...
}

@Test
@DisabledOnJre(JAVA_9)
void notOnJava9() {
    // ...
}

@Test
@DisabledForJreRange(min = JAVA_9, max = JAVA_11)
void notFromJava9to11() {
    // ...
}

@Test
@DisabledForJreRange(min = JAVA_9)
void notFromJava9toCurrentJavaFeatureNumber() {
    // ...
}

@Test
@DisabledForJreRange(max = JAVA_11)
void notFromJava8to11() {
    // ...
}
```
>主要是java版本不同会有一些差异,新特性或者性能上的改进

**系统属性条件**
容器或测试可以通过 @EnabledIfSystemProperty和 @DisabledIfSystemProperty注释根据 JVM 系统属性的值来启用或禁用。通过该属性提供的值将被解释为正则表达式。

```java
@Test
@EnabledIfSystemProperty(named = "os.arch", matches = ".*64.*")
void onlyOn64BitArchitectures() {
    // ...
}

@Test
@DisabledIfSystemProperty(named = "ci-server", matches = "true")
void notOnCiServer() {
    // ...
}
```
**环境变量条件**
容器或测试可以通过 @EnabledIfEnvironmentVariable和@DisabledIfEnvironmentVariable 注释根据基础操作系统的环境变量的值来启用或禁用。通过该属性提供的matches值将被解释为正则表达式。

```java
@Test
@EnabledIfEnvironmentVariable(named = "ENV", matches = "staging-server")
void onlyOnStagingServer() {
    // ...
}

@Test
@DisabledIfEnvironmentVariable(named = "ENV", matches = ".*development.*")
void notOnDeveloperWorkstation() {
    // ...
}
```
**自定义条件**
容器或测试可以根据方法通过 @EnabledIf和 @DisabledIf注释返回的布尔值来启用或禁用。该方法通过其名称提供给批注。如果需要ExtensionContext，条件方法可以采用 类型的单个参数。
```java
  @Test
    @EnabledIf("customCondition")
    void enabled() {
        System.out.println("尔曹身与名俱灭");
    }

    @Test
    @DisabledIf("customCondition")
    void disabled() {
        System.out.println("不废江河万古流");
    }
    //当该方法的返回值为true时执行
   static boolean customCondition() {
        return 1==1;
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e34158b31e6a448188502a479f1f310c.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ddb4e8029a0148e29416603f0993bf40.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/286e1a4081164431b038b09f2eafca1d.png)
>注:customCondition方法在测试类中时,会按照需要进行执行,跟是否是static修饰的方法无关;


当条件方法位于测试类之外。在这种情况下，必须通过其完全限定的名称引用它
例如:

```java

@SpringBootTest(classes = Demo351Application.class)
class Demo351ApplicationTests {
 @Test
    @EnabledIf("com.gavin.myTest#customCondition2")
    void enabled2() {
        System.out.println("尔曹身与名俱灭");
    }

    @Test
    @DisabledIf("com.gavin.myTest#customCondition2")
    void disabled2() {
        System.out.println("不废江河万古流");
    }
}
class myTest{
    //当该方法的返回值为true时执行
   static boolean customCondition2() {
        return 1==1;
    }
}
```
>注:这时候注解上要为**类的全限定路径#方法名(静态)**
>![在这里插入图片描述](https://img-blog.csdnimg.cn/077d4ee988ed4baeb3eb0ffbd1610955.png)


在类级别使用@EnabledIf 或@DisabledIf 时，条件方法必须始终为 。位于外部类中的条件方法也必须为static 。在任何其他情况下static，可以同时使用静态方法或实例方法。

**当测试类很多时怎么快速的查找呢?**

Junit5中提供了标签来解决这个问题,@Tag注解可以在类上声明也可以在测试方法上

```java
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;

@Tag("fast")
@Tag("model")
class TaggingDemo {

    @Test
    @Tag("taxes")
    void testingTaxCalculation() {
    }
}
```
**测试方法的执行顺序**
尽管有时候测试方法并不要求执行顺序,但是有的时候测试一个方法之前确实需要先做一些准备;
虽然@BeforeAll或者@BeforeEach,@AfterAll或者@fterEach提供了执行顺序,但是这是针对于所有测试方法的,能否开个小灶呢?

Junit5提供了方法的执行时顺序的支持
>若要控制测试方法的执行顺序，请使用测试类或测试接口进行批注，并指定所需的实现。您可以实现自己的自定义或使用以下内置实现之一。@TestMethodOrder   MethodOrderer    MethodOrderer  MethodOrderer

 - MethodOrderer.DisplayName：根据测试方法的显示名称按字母数字对测试方法进行排序（请参阅显示名称生成优先级规则)
 - MethodOrderer.MethodName：根据测试方法的名称和正式参数列表按字母数字对测试方法进行排序
 - MethodOrderer.OrderAnnotation：根据通过注释指定的值对测试方法进行数值排序@Order
 - MethodOrderer.Random：伪随机订购测试方法，并支持自定义种子的配置
 - MethodOrderer.Alphanumeric：根据测试方法的名称和正式参数列表按字母数字对测试方法进行排序;弃用以支持----已废弃,不测试了


![在这里插入图片描述](https://img-blog.csdnimg.cn/819a6a5b642649569931cd1953786690.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**根据OrderAnnotation排序**
测试代码--->>

```java
package com.gavin;

import org.junit.jupiter.api.*;
import org.springframework.boot.test.context.SpringBootTest;

import static org.junit.jupiter.api.Assertions.assertEquals;
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
@SpringBootTest
public class MyUser {

    public static String Greeting() throws InterruptedException {
        Thread.sleep(1000);
        return "Hello";
    }

static int result=100;
    @Test
    @Order(1)
    void nullValues() {
      --result;
        System.out.println("result0="+result);
        assertEquals(2, 1 + 1);
    }
  @Test
    @Order(3)
    void validValues() {
        --result;
        System.out.println("result2="+result);
        assertEquals(2, 1 + 1);
    }
    @Test
    @Order(2)
    void emptyValues() {
        --result;

        System.out.println("result1="+result);
        assertEquals(2, 1 + 1);
    }

  

}

```

当运行整个测试类时
![在这里插入图片描述](https://img-blog.csdnimg.cn/7c798bae99c7405896a7775e461bad6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_16,color_FFFFFF,t_70,g_se,x_16)



**根据DisplayName排序**
```java
package com.gavin;

import org.junit.jupiter.api.*;
import org.springframework.boot.test.context.SpringBootTest;

import static org.junit.jupiter.api.Assertions.assertEquals;
//@TestMethodOrder(MethodOrderer.OrderAnnotation.class)

//根据测试方法的显示名称按字母数字对测试方法进行排序
@TestMethodOrder(MethodOrderer.DisplayName.class)
@SpringBootTest
public class MyUser {

    public static String Greeting() throws InterruptedException {
        Thread.sleep(1000);
        return "Hello";
    }

static int result=100;
    @Test
    @Order(1)
    @DisplayName("first")
    void nullValues() {
      --result;
        System.out.println("result0="+result);
        assertEquals(2, 1 + 1);
    }

    @Test
    @Order(3)
    @DisplayName("Third")
    void validValues() {
        --result;
        System.out.println("result2="+result);
        assertEquals(2, 1 + 1);
    }
    @Test
    @Order(2)
    @DisplayName("Second")
    void emptyValues() {
        --result;
        System.out.println("result1="+result);
        assertEquals(2, 1 + 1);
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/849f27c9175a4344ac738c29f7a148d4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_16,color_FFFFFF,t_70,g_se,x_16)


**根据MethodName排序**
```java
package com.gavin;

import org.junit.jupiter.api.*;
import org.springframework.boot.test.context.SpringBootTest;

import static org.junit.jupiter.api.Assertions.assertEquals;

//根据测试方法的名称和正式参数列表按字母数字对测试方法进行排序
@TestMethodOrder(MethodOrderer.MethodName.class)//nve

@SpringBootTest
public class MyUser {

    public static String Greeting() throws InterruptedException {
        Thread.sleep(1000);
        return "Hello";
    }

static int result=100;
    @Test
    @Order(1)
    @DisplayName("first")
    void nullValues() {
      --result;
        System.out.println("result0="+result);
        assertEquals(2, 1 + 1);
    }

    @Test
    @Order(3)
    @DisplayName("Third")
    void validValues() {
        --result;
        System.out.println("result2="+result);
        assertEquals(2, 1 + 1);
    }
    @Test
    @Order(2)
    @DisplayName("Second")
    void emptyValues() {
        --result;

        System.out.println("result1="+result);
        assertEquals(2, 1 + 1);
    }
    @Test
    @Order(1)
    @DisplayName("four")
    void nullValues1() {
        --result;
        System.out.println("result4="+result);
        assertEquals(2, 1 + 1);
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/1837c4a8940b4a4696458a0eb014ca15.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_15,color_FFFFFF,t_70,g_se,x_16)
>注:MethodName将在 6.0 中删除

**Random排序**
```java
package com.gavin;
import org.junit.jupiter.api.*;
import org.springframework.boot.test.context.SpringBootTest;
import static org.junit.jupiter.api.Assertions.assertEquals;
//伪随机订购测试方法，并支持自定义种子的配置
@TestMethodOrder(MethodOrderer.Random.class)//nve

@SpringBootTest
public class MyUser {

    public static String Greeting() throws InterruptedException {
        Thread.sleep(1000);
        return "Hello";
    }

static int result=100;
    @Test
    @Order(1)
    @DisplayName("first")
    void nullValues() {
      --result;
        System.out.println("result0="+result);
        assertEquals(2, 1 + 1);
    }

    @Test
    @Order(3)
    @DisplayName("Third")
    void validValues() {
        --result;
        System.out.println("result2="+result);
        assertEquals(2, 1 + 1);
    }
    @Test
    @Order(2)
    @DisplayName("Second")
    void emptyValues() {
        --result;

        System.out.println("result1="+result);
        assertEquals(2, 1 + 1);
    }
    @Test
    @Order(1)
    @DisplayName("four")
    void nullValues1() {
        --result;
        System.out.println("result4="+result);
        assertEquals(2, 1 + 1);
    }

}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b8954d1be57a4a28a16f553c9401d9df.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## 嵌套测试

@Nested测试提供了更多功能来表达几组测试之间的关系。这种嵌套测试利用Java的嵌套类(内部类)，并促进对测试结构的分层;
先看官方的测试代码-->>

```java
package com.gavin;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertTrue;

import java.util.EmptyStackException;
import java.util.Stack;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
@DisplayName("A stack")
class TestingAStackDemo {
//声明  1,
    Stack<Object> stack;

    @Test
    @DisplayName("is instantiated with new Stack()")
    void isInstantiatedWithNew() {
        new Stack<>();
    }

    @Nested//嵌套
    @DisplayName("when new")
    class WhenNew {

        @BeforeEach
        void createNewStack() {
            stack = new Stack<>();
        }

        @Test
        @DisplayName("is empty")
        void isEmpty() {
            assertTrue(stack.isEmpty());
        }

        @Test
        @DisplayName("throws EmptyStackException when popped")
        void throwsExceptionWhenPopped() {
            assertThrows(EmptyStackException.class, stack::pop);
        }

        @Test
        @DisplayName("throws EmptyStackException when peeked")
        void throwsExceptionWhenPeeked() {
            assertThrows(EmptyStackException.class, stack::peek);
        }

        @Nested
        @DisplayName("after pushing an element")
        class AfterPushing {

            String anElement = "an element";

            @BeforeEach
            void pushAnElement() {
                stack.push(anElement);
            }

            @Test
            @DisplayName("it is no longer empty")
            void isNotEmpty() {
                assertFalse(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when popped and is empty")
            void returnElementWhenPopped() {
                assertEquals(anElement, stack.pop());
                assertTrue(stack.isEmpty());
            }

            @Test
            @DisplayName("returns the element when peeked but remains not empty")
            void returnElementWhenPeeked() {
                assertEquals(anElement, stack.peek());
                assertFalse(stack.isEmpty());
            }
        }
    }
}
```
执行外部的测试方法**isInstantiatedWithNew**
![在这里插入图片描述](https://img-blog.csdnimg.cn/7d85bf0747f54d54b04dddd176ff01a9.png)
>内部类中的测试方法一定要在内部类class上添加@Nested注解
>![在这里插入图片描述](https://img-blog.csdnimg.cn/d1c06d23c07c4363b152308257f280e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

执行内部类(儿子类)WhenNew中测试方法 **isEmpty**
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ac222628f9044b59400495a84e2a127.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
执行内部类(孙子类)AfterPushing测试方法**isNotEmpty**

![在这里插入图片描述](https://img-blog.csdnimg.cn/0db948daa6d44a1c98c9040bf334419b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
因此可以得出结论--->>
执行外部类中的测试方法,内部类信息不会被关联测试;
执行内部类中的测试方法,外部类会被加载;

比较直观的测试代码--->

```java
package com.gavin;
import org.junit.jupiter.api.*;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
@DisplayName("OuterClassInfo")
public class NestedTest {

    @BeforeAll
    @DisplayName("OuterBeforeAll")
    public static void OuterBeforeAll() {
        System.out.println("OuterBeforeAll");
    }

    @BeforeEach
    @DisplayName("OuterBeforeEach")
    public void OuterBeforeEach() {
        System.out.println("OuterBeforeEach");
    }

    @DisplayName("OuterTest")
    @Test
    public void OuterTest() {
        System.out.println("OuterTest");
    }

    @AfterEach
    @DisplayName("OuterAfterEach")
    public void OuterAfterEach() {
        System.out.println("OuterAfterEach");
    }

    @AfterAll
    @DisplayName("OuterAfterAll")
    public static void OuterAfterAll() {
        System.out.println("OuterAfterAll");
    }
@Nested
@DisplayName("InnerClassInfo")
        class NestedInner {


        @BeforeEach
        @DisplayName("InnerBeforeEach")
        public void InnerBeforeEach() {
            System.out.println("InnerBeforeEach");
        }

        @DisplayName("InnerTest")
        @Test
        public void InnerTest() {
            System.out.println("InnerTest");
        }

        @AfterEach
        @DisplayName("InnerAfterEach")
        public void InnerAfterEach() {
            System.out.println("InnerAfterEach");
        }


    }
}

```
执行整个外部类--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/cabd0202beb14e5e8e848aa697908be8.png)

测试结果
```java
OuterBeforeAll
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.6.3)
OuterBeforeEach
OuterTest
OuterAfterEach

OuterBeforeEach
InnerBeforeEach
InnerTest
InnerAfterEach
OuterAfterEach

OuterAfterAll

```
结论:
>当执行整个外部类时内部类也会被执行

执行外部类中的测试方法-->>**OuterTest**

```java
OuterBeforeAll
OuterBeforeEach
OuterTest
OuterAfterEach
OuterAfterAll
```
结论:
>执行外部类中的测试方法,内部类中的方法不会被执行

执行整个内部类中的测试方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/e4af1bc060c04016b91f48a0c708e481.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
OuterBeforeAll
OuterBeforeEach
InnerBeforeEach
InnerTest
InnerAfterEach
OuterAfterEach

OuterAfterAll
```
结论:
>执行内部类中的所有测试方法时,外部类的测试方法不会被加载

执行内部类中的测试方法-->>**InnerTest**
```java
OuterBeforeAll
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.6.3)
OuterBeforeEach
InnerBeforeEach
InnerTest
InnerAfterEach
OuterAfterEach
OuterAfterAll
```
结论:
>当执行内部类中的测试方法,外部类也会被加载,*Each方法会被执行,同时如果*All方法要想执行,就需要让方法变为静态方法;

## 构造函数和方法的依赖注入

>在之前的 JUnit 版本中，不允许测试构造函数或方法具有参数。

目前有三个内置的解析程序会自动注册。

如果构造函数或方法参数是 类型的 ，则将提供与当前容器或 test 相对应的实例作为参数的值。然后，可用于检索有关当前容器或测试的信息，如显示名称、测试类、测试方法和关联的标记。显示名称可以是技术名称（如测试类或测试方法的名称），也可以是通过 配置的自定义名称。

**TestInfo**

TestInfo 能获得displayname注解上的参数,测试类是谁,以及tag上的参数; 
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d519a4f07494cb7a39d7e3de33f9958.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
package com.gavin;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertTrue;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestInfo;

@DisplayName("TestInfo Demo")
class TestInfoDemo {
//TestInfo 用于注入关于当前测试的信息
    /*如果一个方法参数的类型是{@link TestInfo}， JUnit将提供
    TestInfo的一个实例，对应于当前测试或容器作为参数的值。
            */

//    TestInfoDemo(TestInfo testInfo) {
//        System.out.println("www"+testInfo.getDisplayName());//获得的是类上的注解
//        assertEquals("TestInfo Demo", testInfo.getDisplayName());
//    }

 TestInfoDemo(TestInfo testInfo,String name) {
        name=testInfo.getDisplayName();
     System.out.println("姓名--"+name);
        System.out.println("www"+testInfo.getDisplayName());//获得的是类上的注解
        assertEquals("TestInfo Demo", testInfo.getDisplayName());
    }

    @BeforeEach
    void init(TestInfo testInfo) {
        String displayName = testInfo.getDisplayName();//获得test方法上的注解
        System.out.println("beforeEach"+displayName);
//       assertTrue(displayName.equals("TEST 1") || displayName.equals("test2(TestInfo)"));
       assertTrue(displayName.equals("TEST 1") || displayName.equals("Test2"));
    }

    @Test
    @DisplayName("TEST 1")
    @Tag("my-tag")
    void test1(TestInfo testInfo) {
        System.out.println("Test"+testInfo.getDisplayName());//获得test方法上的注解
        assertEquals("TEST 1", testInfo.getDisplayName());
        assertTrue(testInfo.getTags().contains("my-tag"));
    }
@DisplayName("Test2")//不指定的话默认为方法名+(参数类型)---test2(TestInfo)
    @Test
    void test2(TestInfo testInfo) {
        System.out.println(testInfo.getDisplayName());
        testInfo.getTags().forEach(System.out::println);
//        获取与当前测试或容器相关的Class如果可用。
        System.out.println(testInfo.getTestClass().get().getName());
//        获取与当前测试或容器关联的Method
        System.out.println(testInfo.getTestMethod().get().getName());
    }

}
```
如果 RepeatedTest 或 @BeforeEach@AfterEach方法中的方法参数类型为 RepetitionInfo，则 将提供 RepetitionInfoParameterResolver的实例。 然后，可用于检索有关当前重复和相应 重复次数总数的信息。但请注意，在 的上下文之外不会注册

**RepetitionInfo**
用于获得测试执行的次数以及重复的次数
可用于检索有关当前重复和相应 重复次数总数的信息。但请注意，在 的上下文之外不会注册。
![在这里插入图片描述](https://img-blog.csdnimg.cn/4229c36f262c47f3a2cc05ba35536a7f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
package com.gavin;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertTrue;

import org.junit.jupiter.api.*;

@DisplayName("TestInfo Demo")
class TestInfoDemo {
 TestInfoDemo(RepetitionInfo repetitionInfo) {

     System.out.println(repetitionInfo.getCurrentRepetition());
     System.out.println(repetitionInfo.getTotalRepetitions());
 }
    @BeforeEach
    void init(TestInfo testInfo) {
        String displayName = testInfo.getDisplayName();//获得test方法上的注解
        System.out.println("beforeEach"+displayName);

    }

   
@DisplayName("Test2")//不指定的话默认为方法名+(参数类型)---test2(TestInfo)
    @RepeatedTest(2)
    void test2(TestInfo testInfo) {
        System.out.println(testInfo.getDisplayName());
        testInfo.getTags().forEach(System.out::println);
//        获取与当前测试或容器相关的Class如果可用。
        System.out.println(testInfo.getTestClass().get().getName());
//        获取与当前测试或容器关联的Method
        System.out.println(testInfo.getTestMethod().get().getName());
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/125788a8ad294686bc300102de0b73d8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
如果构造函数或方法参数是类型，则将提供 一个实例。可用于发布有关当前测试运行的其他数据;

**TestReport**

```java
package com.gavin;

import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestReporter;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.HashMap;
import java.util.Map;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotEquals;

@SpringBootTest
class TestReporterDemo {

    @Test
    void reportSingleValue(TestReporter testReporter) {
        testReporter.publishEntry("a status message");
    }

    @Test
    void reportKeyValuePair(TestReporter testReporter) {

        testReporter.publishEntry("a key", "a value");
    }

    @Test
    void reportMultipleKeyValuePairs(TestReporter testReporter) {
        Map<String, String> values = new HashMap<>();
        values.put("user name", "dk38");
        values.put("award year", "1974");

        testReporter.publishEntry(values);

    }
}
```

## 参数的支持
在所有以前的 JUnit 版本中，不允许测试构造函数或方法具有参数（至少在标准实现中不允许）。作为JUnit Jupiter的主要变化之一，测试构造函数和方法现在都允许具有参数。这允许更大的灵活性，并为构造函数和方法启用依赖关系注入。

测试代码-->>
```java
package com.gavin;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
@DisplayName("ParamTestStart")
public class ParamTest {

    @ParameterizedTest
    @ValueSource(strings = {"Hello","World"})
    public void ParamTest1(String str){
        System.out.println(str);
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6064de47811449949f2fa81902ed5a31.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>注解@ParameterizedTest 表明该测试方法可以添加参数;
>   @ValueSource(strings = {"Hello","World"})表明参数的类型和值;

参数的类型
```java
@ArgumentsSource(ValueArgumentsProvider.class)
public @interface ValueSource {
    short[] shorts() default {};

    byte[] bytes() default {};

    int[] ints() default {};

    long[] longs() default {};

    float[] floats() default {};

    double[] doubles() default {};

    char[] chars() default {};

    boolean[] booleans() default {};

    String[] strings() default {};

    Class<?>[] classes() default {};
}
```

## 参数化测试

参数化测试使得使用不同参数多次运行测试成为可能。它们的声明方式与常规方法相同，但改用注释。此外，必须声明至少一个将为每个调用提供参数的源，然后使用测试方法中的参数。

```java
 public static boolean isPalindrome(String str) {
        if(str==null||str.length()==0) {
            throw new RuntimeException("字符串为空");
        }
        int middle=(str.length()-1)/2;
        for(int i=0;i<=middle;i++) {
            if(str.charAt(i)!=str.charAt(str.length()-1-i)) {
                return false;
            }
        }
        return true;
    }
    @ParameterizedTest(autoCloseArguments=false)
    @ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
    void palindromes(String candidate) {//回文
        assertTrue(MyRandomParametersTest.isPalindrome(candidate));
        System.out.println("success");
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/3a4fe75b7da548a4a4a5af9d327ec7d0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

>使用参数化测试需要@ParameterizedTest

**使用参数**

参数化测试方法通常直接使用来自已配置源的参数，遵循参数源索引和方法参数索引之间的一对一关联。但是，参数化测试方法也可以选择将源中的参数聚合到传递给该方法的单个对象中;

**可自动关闭参数**
在为当前参数化测试调用调用方法和扩展后，实现（或扩展）的参数将自动关闭。
 >@ParameterizedTest(autoCloseArguments=false)

要防止发生这种情况，请将该属性设置为false 。如果实现的参数重用于同一参数化测试方法的多个调用，则必须为该方法添加注释，以确保该参数在调用之间不会关闭。autoCloseArguments@ParameterizedTestfalseAutoCloseable@ParameterizedTest(autoCloseArguments = false)

**@ValueSource**
@ValueSource是最简单的可能来源之一。它允许您指定单个文本值数组，并且只能用于为每个参数化测试调用提供单个参数;

@ValueSource支持以下类型的文本值。

- short
- byte
- int
- long
- float
- double
- char
- boolean
- java.lang.String
- java.lang.Class

测试代码--->

```java

    public static boolean isPalindrome(String str) {
        if(str==null||str.length()==0) {
            throw new RuntimeException("字符串为空");
        }
        int middle=(str.length()-1)/2;
        for(int i=0;i<=middle;i++) {
            if(str.charAt(i)!=str.charAt(str.length()-1-i)) {
                return false;
            }
        }
        return true;
    }


    @ParameterizedTest(autoCloseArguments=false)
    @ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
    void palindromes(String candidate) {//回文
        assertTrue(MyRandomParametersTest.isPalindrome(candidate));
        System.out.println("success");
    }

    @ParameterizedTest
    @ValueSource(classes = {Random.class,MyUser.class})
    void randomTest(Class obj){
        if(obj instanceof Object) {
            System.out.println(new Random().nextInt(10));
            System.out.println(new MyUser().toString());
        }
    }
    @ParameterizedTest
    @NullSource//有注解时,会将null也参加到参数上测试
    @EmptySource
//    @NullAndEmptySource两个的合集
    @ValueSource(strings = {"\n a","\b bb","  ", "空字符串"})
    void testWithNullSource(String str) {
        System.out.println(str.length());
        assertTrue(1==1);
    }

    @ParameterizedTest
    @ValueSource(ints = { 1, 2, 3 })//测试将会执行三次,1,2,3区别于@RepeatedTest(3)
    void testWithValueSource(int argument) {
        System.out.println(argument);
        assertTrue(argument > 0 && argument < 4);
    }

```
**@NullSource与@EmptySource**

@NullSource：为带批注的方法提供单个参数~null,不能用于具有基本类型的参数。

@EmptySource：为以下类型的参数的带注释方法提供单个空参数：基元数组、对象数组。

```java
   @ParameterizedTest
    @NullSource//有注解时,会将null也参加到参数上测试
    @EmptySource
//    @NullAndEmptySource两个的合集
    @ValueSource(strings = {"\n a","\b bb","  ", "空字符串"})
    void testWithNullSource(String str) {
        System.out.println(str.length());
        assertTrue(1==1);
    }

```
>参数化测试方法的两种变体都会导致六次调用：1 表示，1 表示空字符串，4 表示通过 提供的显式空字符串。

**@EnumSource**
@EnumSource提供了一种使用枚举常量的便捷方法

批注的属性是可选的。省略时，将使用第一个方法参数的声明类型。如果测试不引用枚举类型，它将失败。因此，在上面的示例中，该属性是必需的，因为方法参数被声明为 ，即 由 实现的接口，它不是枚举类型。将方法参数类型更改为 允许您从批注中省略显式枚举类型，
```java
    @ParameterizedTest
//    @EnumSource(ChronoUnit.class)//则会测试所有枚举值
    @EnumSource(names = { "DAYS", "HOURS" })//指定枚举值测试
    void testWithEnumSource(TemporalUnit argument) {
        System.out.println(argument);
    }


    @ParameterizedTest
//    @EnumSource(ChronoUnit.class)//则会测试所有枚举值
    @EnumSource(names = { "DAYS", "HOURS" })//names指定枚举值测试
    void testWithEnumSource2(ChronoUnit unit) {//相当注解中声明了枚举数据源TemporalUnit
//        assertTrue(EnumSet.of(ChronoUnit.DAYS, ChronoUnit.HOURS).contains(unit));
        System.out.println(unit);
    }
 @ParameterizedTest
//    @EnumSource(ChronoUnit.class)//则会测试所有枚举值
//    @EnumSource(names = { "DAYS", "HOURS" })//names指定枚举值测试
   @EnumSource//可以不指定,通过方法的第一个参数来确定
    void testWithEnumSource3(ChronoUnit unit) {//相当注解中声明了枚举数据源TemporalUnit
//        assertTrue(EnumSet.of(ChronoUnit.DAYS, ChronoUnit.HOURS).contains(unit));
        System.out.println(unit);
    }
```
>该注释还提供了一个可选属性，该属性允许对将哪些常量传递给测试方法进行细粒度控制。例如，可以从枚举常量池中排除名称或指定正则表达式，如以下示例所示。@EnumSourcemode

```java
@ParameterizedTest
@EnumSource(mode = EXCLUDE, names = { "ERAS", "FOREVER" })
void testWithEnumSourceExclude(ChronoUnit unit) {
    assertFalse(EnumSet.of(ChronoUnit.ERAS, ChronoUnit.FOREVER).contains(unit));
}
@ParameterizedTest
@EnumSource(mode = MATCH_ALL, names = "^.*DAYS$")
void testWithEnumSourceRegex(ChronoUnit unit) {
    assertTrue(unit.name().endsWith("DAYS"));
}
```
**@MethodSource**
@MethodSource引用测试类内的方法或外部类的一个或多个静态方法。

测试类中的工厂方法必须为static ，除非测试类注释为@TestInstance(Lifecycle.PER_CLASS) ;而外部类中的工厂方法必须始终为static 。此外，此类工厂方法不得接受任何参数。

```java
  @ParameterizedTest
    @MethodSource//如果不指定,则找同名的,静态方法
    void show(Integer str){
        System.out.println(str);
    }
public  static  Integer [] show(){
        Integer []result={1,2,3};
        return  result;
}

  @ParameterizedTest
    @MethodSource("stringProvider")
    void testWithExplicitLocalMethodSource(String argument) {
        assertNotNull(argument);
        System.out.println(argument);
    }

    static Stream<String> stringProvider() {
        return Stream.of("apple", "banana");
    }

```
如果参数化测试方法声明了多个参数，那么您需要返回实例或对象数组的集合、流或数组

```java
    @ParameterizedTest
    @MethodSource("range")
    void testWithRangeMethodSource(int argument) {
        assertNotEquals(9, argument);
    }

    static IntStream range() {
        return IntStream.range(0, 20).skip(10);
    }

    @ParameterizedTest
    @MethodSource("stringIntAndListProvider")
    void testWithMultiArgMethodSource(String str, int num, List<String> list) {
        assertEquals(5, str.length());
        assertTrue(num >=1 && num <=2);
        assertEquals(2, list.size());
    }

    static Stream<Arguments> stringIntAndListProvider() {
        return Stream.of(
                arguments("apple", 1, Arrays.asList("a", "b")),
                arguments("lemon", 2, Arrays.asList("x", "y"))
        );
    }

```
可以通过提供外部工厂方法名称来引用其完全限定的方法名称

```java
class ExternalMethodSourceDemo {

    @ParameterizedTest
    @MethodSource("example.StringsProviders#tinyStrings")
    void testWithExternalMethodSource(String tinyString) {
        // test with tiny string
    }
}
class StringsProviders {

    static Stream<String> tinyStrings() {
        return Stream.of(".", "oo", "OOO");
    }
}
```
**@CsvSource**与**@CsvFileSource**
@CsvSource允许您将参数列表表示为逗号分隔的值（即 CSV 文本）。通过 中的属性提供的每个字符串都表示一个 CSV 记录，并导致对参数化测试的一次调用。第一条记录可以选择用于提供 CSV 标头;

@CsvFileSource允许您使用类路径或本地文件系统中的逗号分隔值 （CSV） 文件。CSV 文件中的每条记录都会导致一次参数化测试调用。可以选择使用第一条记录来提供 CSV 标头。


```java
    @ParameterizedTest
    @CsvSource({"apple,1","mac ,2","'zzt,zzy',100","strawberry,1"})//书写格式", ,"
    void testCsv(String args,int rank){
        assertNotNull("mac");
        assertNotEquals(0,rank);
    }

    void testWithCsvSource(String fruit, int rank) {
        assertNotNull(fruit);
        assertNotEquals(0, rank);
    }

    @ParameterizedTest
    @CsvFileSource(resources = "/two-column.csv", numLinesToSkip = 1)
    void testWithCsvFileSourceFromClasspath(String country, int reference) {
        assertNotNull(country);
        assertNotEquals(0, reference);
    }

    @ParameterizedTest
    @CsvFileSource(files = "src/test/resources/two-column.csv", numLinesToSkip = 1)
    void testWithCsvFileSourceFromFile(String country, int reference) {
        System.out.println(country);
        assertNotNull(country);
        assertNotEquals(0, reference);
    }

    @ParameterizedTest(name = "[{index}] {arguments}")
    @CsvFileSource(resources = "/two-column.csv", useHeadersInDisplayName = true)
    void testWithCsvFileSourceAndHeaders(String country, int reference) {
        assertNotNull(country);
        assertNotEquals(0, reference);
    }

    @ParameterizedTest
    @ArgumentsSource(MyArgumentsProvider.class)//可以指定参数提供类
    void testWithArgumentsSource(String argument) {
        assertNotNull(argument);
    }

}
class MyArgumentsProvider implements ArgumentsProvider {//参数提供者~class,要实现ArgumentsProvider,覆写provideArguments方法;

    @Override
    public Stream<? extends Arguments> provideArguments(ExtensionContext context) {
        return Stream.of("apple", "banana").map(Arguments::of);
    }
```
不带引号的空值将始终转换为引用，而不管通过该属性配置的任何自定义值,除带引号的字符串外，默认情况下会修剪 CSV 列中的前导空格和尾随空格。可以通过将属性设置为 来更改此行为;
![在这里插入图片描述](https://img-blog.csdnimg.cn/d562df42c5d24079a8540d7a4bad965a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**@ArgumentsSource**
@ArgumentsSource可用于指定自定义的、可重用的 。请注意，必须将 的实现声明为顶级类或嵌套类

```java
@SpringBootTest
class mulove{
 @ParameterizedTest
    @ArgumentsSource(MyArgumentsProvider.class)
    void testWithArgumentsSource(String argument) {
        assertNotNull(argument);
    }

}
class MyArgumentsProvider implements ArgumentsProvider {

    @Override
    public Stream<? extends Arguments> provideArguments(ExtensionContext context) {
        return Stream.of("apple", "banana").map(Arguments::of);
    }
```

**@RepeatedTest重复测试**
JUnit Jupiter提供了通过注释方法并指定所需的重复总数来重复指定次数测试的能力。每次调用重复测试的行为都类似于执行常规方法，完全支持相同的生命周期回调和扩展。

```java
@DisplayName("Test2")//不指定的话默认为方法名+(参数类型)---test2(TestInfo)
    @RepeatedTest(value = 3 ,name=CURRENT_REPETITION_PLACEHOLDER)
    void test2(TestInfo testInfo) {
        System.out.println(testInfo.getDisplayName());
        testInfo.getTags().forEach(System.out::println);
//        获取与当前测试或容器相关的Class如果可用。
        System.out.println(testInfo.getTestClass().get().getName());
//        获取与当前测试或容器关联的Method
        System.out.println(testInfo.getTestMethod().get().getName());
    }
```
除了指定重复次数外，还可以通过注释的属性为每个重复配置自定义显示名称。此外，显示名称可以是静态文本和动态占位符组合的模式。当前支持以下占位符
{displayName}：方法的显示名称@RepeatedTest
![在这里插入图片描述](https://img-blog.csdnimg.cn/1878eb94f76949e7b772d109e1a8c7f5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

{currentRepetition}：当前重复次数
![在这里插入图片描述](https://img-blog.csdnimg.cn/0550e32cb0b847d8b150ad44e1d1a878.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


{totalRepetitions}：重复次数总数
![在这里插入图片描述](https://img-blog.csdnimg.cn/f513891d5e3b4b9d857bf16402caf821.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**组合模式--->>SHORT_DISPLAY_NAME---默认的模式**


![在这里插入图片描述](https://img-blog.csdnimg.cn/7828ecb981c444a9975f68ab38143910.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**组合模式----->>LONG_DISPLAY_NAME**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f2dda7195710442c88ba4320cfea399a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## 参数转换
Junit5中提供了参数的类型转换,最常见的是String转为别的类型的,
例如-->

```java
 @ParameterizedTest
    @ValueSource(strings = "SECONDS")
    void testWithImplicitArgumentConversion(ChronoUnit argument) {
        assertNotNull(argument.name());
    }
```
[更多参数转换详见JunitAPI](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests-argument-conversion)

## 参数聚合

```java
@ParameterizedTest
@CsvSource({
    "Jane, Doe, F, 1990-05-20",
    "John, Doe, M, 1990-10-22"
})
void testWithArgumentsAccessor(ArgumentsAccessor arguments) {
    Person person = new Person(arguments.getString(0),
                               arguments.getString(1),
                               arguments.get(2, Gender.class),
                               arguments.get(3, LocalDate.class));

    if (person.getFirstName().equals("Jane")) {
        assertEquals(Gender.F, person.getGender());
    }
    else {
        assertEquals(Gender.M, person.getGender());
    }
    assertEquals("Doe", person.getLastName());
    assertEquals(1990, person.getDateOfBirth().getYear());
}
//参数访问器的实例会自动注入到参数访问器类型的任何参数中
```
**自定义聚合器**
>要使用自定义聚合器，请实现接口并通过方法中兼容参数上的注释进行注册。然后，在调用参数化测试时，聚合的结果将作为相应参数的参数提供。请注意，必须将 的实现声明为顶级类或嵌套类

```java
@ParameterizedTest
@CsvSource({
    "Jane, Doe, F, 1990-05-20",
    "John, Doe, M, 1990-10-22"
})
void testWithArgumentsAggregator(@AggregateWith(PersonAggregator.class) Person person) {
    // perform assertions against person
}
public class PersonAggregator implements ArgumentsAggregator {
    @Override
    public Person aggregateArguments(ArgumentsAccessor arguments, ParameterContext context) {
        return new Person(arguments.getString(0),
                          arguments.getString(1),
                          arguments.get(2, Gender.class),
                          arguments.get(3, LocalDate.class));
    }
}
```
**@TestFactory**
JUnit Jupiter中还引入了一种全新的测试编程模型。这种新型测试是一种动态测试，由工厂方法在运行时生成，该方法用 注释为 。@TestFactory

```java
import static example.util.StringUtils.isPalindrome;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;
import static org.junit.jupiter.api.DynamicContainer.dynamicContainer;
import static org.junit.jupiter.api.DynamicTest.dynamicTest;
import static org.junit.jupiter.api.Named.named;

import java.util.Arrays;
import java.util.Collection;
import java.util.Iterator;
import java.util.List;
import java.util.Random;
import java.util.function.Function;
import java.util.stream.IntStream;
import java.util.stream.Stream;

import example.util.Calculator;

import org.junit.jupiter.api.DynamicNode;
import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.Named;
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.TestFactory;
import org.junit.jupiter.api.function.ThrowingConsumer;

class DynamicTestsDemo {

    private final Calculator calculator = new Calculator();

    // This will result in a JUnitException!
    @TestFactory
    List<String> dynamicTestsWithInvalidReturnType() {
        return Arrays.asList("Hello");
    }

    @TestFactory
    Collection<DynamicTest> dynamicTestsFromCollection() {
        return Arrays.asList(
            dynamicTest("1st dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("2nd dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        );
    }

    @TestFactory
    Iterable<DynamicTest> dynamicTestsFromIterable() {
        return Arrays.asList(
            dynamicTest("3rd dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("4th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        );
    }

    @TestFactory
    Iterator<DynamicTest> dynamicTestsFromIterator() {
        return Arrays.asList(
            dynamicTest("5th dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("6th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        ).iterator();
    }

    @TestFactory
    DynamicTest[] dynamicTestsFromArray() {
        return new DynamicTest[] {
            dynamicTest("7th dynamic test", () -> assertTrue(isPalindrome("madam"))),
            dynamicTest("8th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        };
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromStream() {
        return Stream.of("racecar", "radar", "mom", "dad")
            .map(text -> dynamicTest(text, () -> assertTrue(isPalindrome(text))));
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromIntStream() {
        // Generates tests for the first 10 even integers.
        return IntStream.iterate(0, n -> n + 2).limit(10)
            .mapToObj(n -> dynamicTest("test" + n, () -> assertTrue(n % 2 == 0)));
    }

    @TestFactory
    Stream<DynamicTest> generateRandomNumberOfTestsFromIterator() {

        // Generates random positive integers between 0 and 100 until
        // a number evenly divisible by 7 is encountered.
        Iterator<Integer> inputGenerator = new Iterator<Integer>() {

            Random random = new Random();
            int current;

            @Override
            public boolean hasNext() {
                current = random.nextInt(100);
                return current % 7 != 0;
            }

            @Override
            public Integer next() {
                return current;
            }
        };

        // Generates display names like: input:5, input:37, input:85, etc.
        Function<Integer, String> displayNameGenerator = (input) -> "input:" + input;

        // Executes tests based on the current input value.
        ThrowingConsumer<Integer> testExecutor = (input) -> assertTrue(input % 7 != 0);

        // Returns a stream of dynamic tests.
        return DynamicTest.stream(inputGenerator, displayNameGenerator, testExecutor);
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromStreamFactoryMethod() {
        // Stream of palindromes to check
        Stream<String> inputStream = Stream.of("racecar", "radar", "mom", "dad");

        // Generates display names like: racecar is a palindrome
        Function<String, String> displayNameGenerator = text -> text + " is a palindrome";

        // Executes tests based on the current input value.
        ThrowingConsumer<String> testExecutor = text -> assertTrue(isPalindrome(text));

        // Returns a stream of dynamic tests.
        return DynamicTest.stream(inputStream, displayNameGenerator, testExecutor);
    }

    @TestFactory
    Stream<DynamicTest> dynamicTestsFromStreamFactoryMethodWithNames() {
        // Stream of palindromes to check
        Stream<Named<String>> inputStream = Stream.of(
                named("racecar is a palindrome", "racecar"),
                named("radar is also a palindrome", "radar"),
                named("mom also seems to be a palindrome", "mom"),
                named("dad is yet another palindrome", "dad")
            );

        // Returns a stream of dynamic tests.
        return DynamicTest.stream(inputStream,
            text -> assertTrue(isPalindrome(text)));
    }

    @TestFactory
    Stream<DynamicNode> dynamicTestsWithContainers() {
        return Stream.of("A", "B", "C")
            .map(input -> dynamicContainer("Container " + input, Stream.of(
                dynamicTest("not null", () -> assertNotNull(input)),
                dynamicContainer("properties", Stream.of(
                    dynamicTest("length > 0", () -> assertTrue(input.length() > 0)),
                    dynamicTest("not empty", () -> assertFalse(input.isEmpty()))
                ))
            )));
    }

    @TestFactory
    DynamicNode dynamicNodeSingleTest() {
        return dynamicTest("'pop' is a palindrome", () -> assertTrue(isPalindrome("pop")));
    }

    @TestFactory
    DynamicNode dynamicNodeSingleContainer() {
        return dynamicContainer("palindromes",
            Stream.of("racecar", "radar", "mom", "dad")
                .map(text -> dynamicTest(text, () -> assertTrue(isPalindrome(text)))
        ));
    }

}
```

[Junit5新特性之并行执行](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parallel-execution)

[从 JUnit 4 迁移到Junit5](https://junit.org/junit5/docs/current/user-guide/#migrating-from-junit4)

[Junit5扩展模型](https://junit.org/junit5/docs/current/user-guide/#extensions)

![](..\pic\div\plant.gif)