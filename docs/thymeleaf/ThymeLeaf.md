![](..\pic\div\shark.gif)

# Thymeleaf简介

> Thymeleaf是一个现代的服务器端 Java 模板引擎，适用于 Web 和独立环境。

>Thymeleaf 的主要目标是为您的开发工作流程带来优雅的自然模板— HTML 可以在浏览器中正确显示，也可以作为静态原型工作，从而在开发团队中实现更强的协作。

>Thymeleaf 具有 Spring Framework 模块、大量与您最喜爱的工具集成，以及插入您自己的功能的能力，是现代 HTML5 JVM Web 开发的理想选择 — 尽管它可以做的还有很多。

[更多描述请转官网](https://www.thymeleaf.org/)
---------------------摘自官网描述--------------------

## 入门

入门就是先整一个Thymeleaf程序

参照freemarker的整合,这里将freemarker的启动器换成Thymeleaf
依赖的变动就这么一点;

```xml
     <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
            <version>2.6.2</version>
        </dependency>


```
这里选择最新版本的依赖,如果出现一些其他问题,后面再去解决;

## freemarker与Thymeleaf的对比
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e696471e98d465d97c5dc99fc034b7f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 配置
[详细官网配置请见spring官网](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties.web)

![在这里插入图片描述](https://img-blog.csdnimg.cn/569cf4b916e24b13a282b04103a3e08f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
源码是这样写的:

```java
public class GTVGApplication {
          ...
    private final TemplateEngine templateEngine;
    ...
      
    public GTVGApplication(final ServletContext servletContext) {
        super();
        ServletContextTemplateResolver templateResolver = 
                new ServletContextTemplateResolver(servletContext);
        
        // HTML is the default mode, but we set it anyway for better understanding of code
        templateResolver.setTemplateMode(TemplateMode.HTML);
        // This will convert "home" to "/WEB-INF/templates/home.html"
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        // Template cache TTL=1h. If not set, entries would be cached until expelled
        templateResolver.setCacheTTLMs(Long.valueOf(3600000L));
        
        // Cache is set to true by default. Set to false if you want templates to
        // be automatically updated when modified.
        templateResolver.setCacheable(true);
        
        this.templateEngine = new TemplateEngine();
        this.templateEngine.setTemplateResolver(templateResolver);
        
        ...

    }

}
```
以上默认配置表示  thymeleaf使用UTF-8编码,
访问时前缀template,后缀为html,
即直接转发或重定向到这个文件即可;

直接访问这个文件不可以么?

![在这里插入图片描述](https://img-blog.csdnimg.cn/c7934a45060f407a9397cbc1947e3ecc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
还真不可以,这个跟freemarker一致,


> Thymeleaf在Spring Boot项目中放入到resources/templates中。这个文件夹中的内容是无法通过浏览器URL直接访问的（和WEB-INF效果一样），所有Thymeleaf页面必须先走控制器。

# 运行原理
![在这里插入图片描述](https://img-blog.csdnimg.cn/6bf64607f6e348ce96144ddb537132c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
模板解析器

```java
//模板解析器是实现来自 Thymeleaf API 的接口的对象，全限定名称为：org.thymeleaf.templateresolver.ITemplateResolver
public interface ITemplateResolver {
    ...
  
    /*
     * Templates are resolved by their name (or content) and also (optionally) their 
     * owner template in case we are trying to resolve a fragment for another template.
     * Will return null if template cannot be handled by this template resolver.
     */
    public TemplateResolution resolveTemplate(
            final IEngineConfiguration configuration,
            final String ownerTemplate, final String template,
            final Map<String, Object> templateResolutionAttributes);
}
```
为了处理数据,并适应国际化的需求,模板引擎在获得request,response,还需要获得请求来自的地区;

```java
public class HomeController implements IGTVGController {

    public void process(
            final HttpServletRequest request, final HttpServletResponse response,
            final ServletContext servletContext, final ITemplateEngine templateEngine)
            throws Exception {
        
        WebContext ctx = 
                new WebContext(request, response, servletContext, request.getLocale());
        
        templateEngine.process("home", ctx, response.getWriter());
        
    }

}
```
> 准备好上下文对象后，现在我们可以告诉模板引擎使用上下文处理模板（按其名称），并将其传递给响应编写器，以便可以将响应写入它;

## 展示数据
**友情提醒**----在展示数据之前还需要引入一个命名空间

```
<html xmlns:th="http://www.thymeleaf.org">
```
因为我们在表单中使用的这些非标准属性是 HTML5 规范所不允许的。


1,Thymeleaf中表达式必须依赖标签而不能单独使用
2,标准变量表达式一般在开始标签中,以 th开头
3,语法为: `<tag th:***="${key}"   ></tag>`
4,表达式中可以通过`${}`取出域中的值并放入标签的指定位置
5,`${}`在这里不能单独使用,必须在 **th:后面的双引号**里使用


### 小案例----->>

比如后端---->>
```java
@Controller
public class ShowController {
    @RequestMapping("/goodsinfo")
    public String showInfo(Map<String,Object>map){
        map.put("goodsname","小鸡炖蘑菇");
        return "sample";
    }
}
```
前端:

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

这道菜是<p>"${goodsname}"</p>
<input type="text" th:value=${goodsname}>
</body>
</html>
```
html文件中th并没有把规定标签,所以要引入th空间;
注:这里时idea自带的提示,官方建议是引入
 xmlns:th="http://www.thymeleaf.org,

![在这里插入图片描述](https://img-blog.csdnimg.cn/d40da27b3fec4b359bfb78ee3956557d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
>**data-th-**语法是在 HTML5 中编写自定义属性的标准方法,该语法无需开发人员使用任何命名空间名称;**

html源代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/4976db0e0fe04a1d9120c7340e8add12.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
@Controller
public class ShowController {
    @RequestMapping("/goodsinfo")
    public String showInfo(Map<String,Object>map){
        map.put("goodsname","小鸡炖蘑菇");
        map.put("username","wet");
        map.put("colorful","background-color: yellowgreen");
        return "sample";
    }
}

```
后端传过来的数据

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml" xmlns:color="http://www.w3.org/1999/xhtml"
      xmlns:style="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style type="text/css">

    </style>
</head>
<body>

这道菜是<p>"${goodsname}"</p><br>
<input type="text" th:value=${goodsname}><br>
<span style:background-color:red th:text="${goodsname}"></span>
<span th:style="${colorful}" th:text="${goodsname}"></span>
<button th:text="${goodsname}"></button>
<input type="text" name="name" th:value="${username}">

</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/000ba9b1a41648e286c40110fe933498.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
简单总结一下---->
Thymeleaf将后端的数据 用th:*来展现,且只能在前标签中才能生效;
基本书写格式----->>th:标签中属性名="${}"


如果为了更方便区分可以在属性前加 data-th-标签中属性名="$()"
这两种形式是完全等效的,自己习惯哪一种就用哪一种;
![在这里插入图片描述](https://img-blog.csdnimg.cn/f9cbbeb6d6c94124bf228b42c520e86b.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/db85a008db4a410aa47a23be98579daa.png)
以上内容均为白话形式,如有不当之处,还请参阅
[Thymeleaf官方文档](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#the-template-engine)

## th 操作属性
namespace-------th可以操作的属性
只要是html能识别的标签中的属性基本就可以;
![在这里插入图片描述](https://img-blog.csdnimg.cn/08714ac7525343129c2a33f964d104dd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
有一点不太友好的是----~~1999/xhtml的引入提示会没有提示;~~ 


案例--->>
前端传来一个Goods对象
```java
@RequestMapping("/one")
    public ModelAndView showone(Map<String, Object> map) {
    Goods goodsById = goodsService.findGoodsById(2);
    System.out.println(goodsById);

    map.put("goods", goodsById);
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("sample3");
        return modelAndView;
    }

}

```

```html
<h1>菜单信息</h1>
id:<span th:text="${goods.id}"></span></br>
goodsName:<span th:text="${goods.goodsname}"></span></br>
price:<span th:text="${goods.price}"></span></br>
goodsDesc:<span th:text="${goods.goodsdesc}"></span></br>


```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a6846b9559094bf999ab4f87e994e12a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 当返回的信息属性带有null时,thymeleaf并不会报错而freemarker会报错;


那如果返回对象为null时----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/a2cfb0ed54a8451db18f383fbe78b9dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
报异常
![在这里插入图片描述](https://img-blog.csdnimg.cn/6515a704a7354848885e048c424ffaaf.png)
所以----->>


```html
<h1>菜单信息</h1>
<div th:if="${goods}!=null">判断是否为空
id:<span th:text="${goods.id}"></span></br>
goodsName:<span th:text="${goods.goodsname}"></span></br>
price:<span th:text="${goods.price}"></span></br>
goodsDesc:<span th:text="${goods.goodsdesc}"></span></br>

</div>
```

**th:text**
文本文本只是在单引号之间指定的字符串。它们可以包含任何字符，但您应该使用 对其中的任何单引号进行转义。\'

<p>
  Now you are looking at a <span th:text="'working web application'">template file</span>.
</p>

```html
<p>home.welcome=Welcome to our <b>fantastic</b> grocery store!</p>
<p th:utext="${username}">Welcome to our grocery store!</p>
<p th:text="${username}">Welcome to our grocery store!</p>

```

前端显示结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/6ef5bdcdd7104ae6ade997e44e4fdb4d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
当我们用了text时,会替换事先做好的内容;

**数字文本**
数字文字就是：数字。

```html
<p>The year is <span th:text="2013">1492</span>.</p>
<p>In two years, it will be <span th:text="2013 + 2">1494</span>.</p>
```
**布尔文本**
true    false

```html
<div th:if="${user.isAdmin()} == false"> ...
```

在这个例子中，写在大括号外面，所以是ThymeLeaf照顾它。如果它写在大括号内，那将是OGNL / SpringEL引擎来负责处理,达到的效果是一样的
```html
<div th:if="${user.isAdmin() == false}">
```
true 与 false 只能在if中使用;
**空文本**

还可以使用文本：null

```html
<div th:if="${variable.something} == null"> 
```
**文本标记**

类似于html中的选择器 
数字，布尔和空文本实际上是文本标记的特殊情况。
这些标记允许在标准表达式中进行一些简化。它们的工作方式与文本文本 （） 完全相同，但它们只允许使用字母 （ 和 ）、数字 （）、方括号 （ 和 ）、点 （）、连字符 （） 和下划线 （）。所以没有空格，没有逗号等

它不需要围绕它们的任何引号。因此，我们可以这样做：

```html
<div th:class="content">...</div>
```

```html
<span data-th-class="content">hello</span>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/fb1e2bb23d7e43638d1b3238fd374020.png)
**附加文本**

文本，无论是文本还是计算变量或消息表达式的结果，都可以使用运算符轻松追加：+

```html
<span th:text="'The name of the user is ' + ${user.name}">
```
**文字替换**

文本替换允许轻松格式化包含变量值的字符串

这些替换必须用竖线 （） 包围，例如：|

```html
<span th:text="|Welcome to our application, ${user.name}!|">
```

这相当于：

```html
<span th:text="'Welcome to our application,' + ${user.name} + '!'">
```
例子
```html
<span data-th-text="|Hello,${#dates.format(today)}|"></span><br>

<span data-th-text="|Hello,${#dates.format(today,'yyyy-MM-dd')}|"></span>

<span data-th-text="'Hello,'+${#dates.format(today,'yyyy-MM-dd')}"></span>
<br>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/8730294eea194198bbba8993546df53a.png)

文本替换可以与其他类型的表达式结合使用：

```html
<span th:text="${onevar} + ' ' + |${twovar}, ${threevar}|">
```

```java
<span data-th-text="'Hello,'+${#dates.format(today,'yyyy-MM-dd')}+|world,${#dates.format(today,'yyyy-MM-dd')}|"></span>
<br>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d9299358867b4c1c81bdf2e7148205f0.png)

在文本替换中只允许使用变量/消息表达式


**设置属性值**

```html
<form action="subscribe.html">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" />
  </fieldset>
</form>
```
此模板开始时是y一个静态原型，而不是 Web 应用程序的模板,如果要实现动态交互,则需要给此表单添加action,
```html
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```
除了输入属性，更改设置它的标签的属性值要用到----th:attr

但是，如果我们想一次设置多个属性呢？XML 规则不允许您在标记中设置属性两次，因此将采用**逗号分隔的赋值列表，**

```html
<img src="../../images/gtvglogo.png" 
     th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```
给定所需的消息文件，这将输出：

```html
<img src="/gtgv/images/gtvglogo.png" title="Logo de Good Thymes" alt="Logo de Good Thymes" />
```

但是实际上我们会用到类似于 th:text th:title等类似简单的方式来实现,除非在thymeleaf中没有,那么才应该考虑用这种方式;

在thyme中的标签属性---->>>[为特定属性设置值](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes)

**预置和附加属性**

```html
<input type="button" value="Do it!" class="btn" th:attrappend="class=${' ' + cssStyle}" />
```


```html
<span type="button" value="thymeleaf" class="ith" th:classappend="tht">thymeleaf</span>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/042a6ab409de45a5850f38b5f3ded1dc.png)
这种预置和附加标签之间的区别---->>
```html

<span  class="ith" th:classappend="tht">thymeleaf1</span>
<hr>
<br>
<span  class="ith" th:attrappend="class=${'tht'}">thymeleaf2</span>
<br>
<hr>
<span  th:attrappend="class=${'ith'+'tht'}">thymeleaf3</span>

<p  th:styleappend="'color:#ccc'">thymeleaf4</p>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/5227fa67957a4fa18aa17607112e50dd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_17,color_FFFFFF,t_70,g_se,x_16)

classappend用于向元素添加 CSS 类或样式片段，而不会覆盖现有属性;
attrappend用于添加属性,并不会覆盖哦原来的属性而是在后面追加
styleappend用于追css样式
>**这些属性会将其评估结果附加（后缀）或前缀（前缀）到现有属性值。**


固定值布尔属性
HTML具有布尔属性的概念，这些属性没有值，并且一个属性的优先性意味着值是"真的"。在 XHTML 中，这些属性只取 1 个值，即它本身。

例如：checked

```html
<input type="checkbox" name="option2" checked /> <!-- HTML -->
<input type="checkbox" name="option1" checked="checked" /> <!-- XHTML -->
```

标准方言包含允许您通过评估条件来设置这些属性的属性，因此，如果计算为 **true，**则该属性将设置为其固定值，如果计算为 **false**，则不会设置该属性：

```html

<input type="checkbox" name="active" th:checked="${g.goodsname}" value="肥肠鱼"/>肥肠鱼
<input type="checkbox" name="active" th:checked="${false}" value="蒸熊掌"/>蒸熊掌
<hr>
<input type="checkbox" name="active" th:checked="${true}" />
<input type="checkbox" name="active" th:checked="${false}" />

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2089dab1433460db91ffb158d77ac3d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/6cd65eb1ad8044f9995c2d63dafa55db.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

# 标准表达式
```c
Simple expressions:
Variable Expressions: ${...}
Selection Variable Expressions: *{...}
Message Expressions: #{...}
Link URL Expressions: @{...}
Fragment Expressions: ~{...}
Literals
Text literals: , ,…'one text''Another one!'
Number literals: , , , ,…0343.012.3
Boolean literals: , truefalse
Null literal: null
Literal tokens: , , ,…onesometextmain
Text operations:
String concatenation: +
Literal substitutions: |The name is ${name}|
Arithmetic operations:
Binary operators: , , , , +-*/%
Minus sign (unary operator): -
Boolean operations:
Binary operators: , andor
Boolean negation (unary operator): , !not
Comparisons and equality:
Comparators: , , , (, , , ><>=<=gtltgele)
Equality operators: , (, ==!=eqne)
Conditional operators:
If-then: (if) ? (then)
If-then-else: (if) ? (then) : (else)
Default: (value) ?: (defaultvalue)
Special tokens:
No-Operation: _
```

```html
简单表达式：
变量表达式：${...}
选择变量表达式：*{...}
消息表达式：#{...}
链接网址表达式：@{...}
片段表达式：~{...}
文字
文本文本：，,...'one text''Another one!'
数字文字： ， ， ， ,...0343.012.3
布尔文字： ，truefalse
空文本：null
文字标记： ， ， ,...onesometextmain
文本操作：
字符串串联：+
文字替换：|The name is ${name}|
算术运算：
二元运算符： ， ， ， ， ，+-*/%
减号（一元运算符）：-
布尔运算：
二元运算符： ，andor
布尔否定（一元运算符）：，!not
比较和平等：
比较器： ， ， ， （ ， ， ，><>=<=gtltgele)
等运算符： ， （，==!=eqne)
条件运算符：
如果-那么：(if) ? (then)
如果-然后-否则：(if) ? (then) : (else)
违约：(value) ?: (defaultvalue)
特殊代币：
无操作：_
所有这些功能都可以组合和嵌套：
```

## 条件表达式

```html
  <tr th:each="goods ,g: ${goodsInfo}" th:class="${g.odd}? 'odd'">
        <td th:text="${goods.id}">Onions</td>
        <td th:text="${goods.goodsname}">2.41</td>
        <td data-th-text="${goods.price}"></td>
        <td th:text="${goods.price}>10? ${'九折'} : #{''}">yes</td>
        <td>
            <span th:text="${#lists.size(goodsInfo)}">评论--爆发虎</span> comment/s
            <a href="不跳了,直接返回json吧"
               th:href="@{/comments(GoodsId=${goods.id})}"
               th:if="${not #lists.isEmpty(goods.goodsname)}">view</a>
        </td>
    </tr>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/3c6209b4282b460ba675671dd5d7fba0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd65baa436de4a37888c25f7f914956e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

后端--->

```java
@ResponseBody
    @RequestMapping("/comments")
    public String comments(Integer  GoodsId){
        Goods goodsById = goodsService.findGoodsById(GoodsId);
        return goodsById.getGoodsname();
    }
```
th:if 不仅会计算布尔条件。它的功能不至于此，它将按照以下规则计算指定的表达式--->>
If value is not null:
If value is a boolean and is .true
If value is a number and is non-zero
If value is a character and is non-zero
If value is a String and is not “false”, “off” or “no”
If value is not a boolean, a number, a character or a String.
(If value is null, th:if will evaluate to false).

反向属性th:unless，我们可以在前面的示例中使用该属性，而不是在OGNL表达式中使用：th:if   th:unlessnot

```html
<a href="comments.html"
   th:href="@{/comments(prodId=${prod.id})}" 
th:unless="${#lists.isEmpty(prod.comments)}">view</a>
```
**th:switch**
还有一种方法可以使用Java中的开关结构的等效物有条件地显示内容：/属性集。th:switch    与th:case

```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
</div>
```

请注意，只要将一个属性计算为 ，同一开关上下文中的所有其他属性的计算结果为 。th:case true th:case false

默认选项指定为 ：th:case="*"

```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```
案例演示:

```html
<tr th:each="goods ,g: ${goodsInfo}" th:class="${g.odd}? 'odd'">
        <td th:text="${goods.id}">Onions</td>
        <td th:text="${goods.goodsname}">2.41</td>
        <td data-th-text="${goods.price}"></td>
        <td th:text="${goods.price}>10? ${'九折'} : #{''}">yes</td>
        
        <td th:switch="${goods.goodsname}"><span th:case="酸菜鱼">招牌菜</span></td>
        <td>
        如果菜名是酸菜鱼则显示招牌菜
            <span th:text="${#lists.size(goodsInfo)}">评论--爆发虎</span> comment/s
            <a href="comments.html"
               th:href="@{/comments(GoodsId=${goods.id})}"
               th:if="${not #lists.isEmpty(goods.goodsname)}">view</a>
        </td>
```

## 数据访问方式

第一种----->>
${person.father.name}
第二种----->>
${person['father']['name']}
第三种,如果是map映射
${person[father'].name}
还可以用
${personsArray[0].name}
还可以调用方法
${person.createCompleteName()}
${person.createCompleteNameWithSeparator('-')}

## 内置对象

```
#ctx: the context object.  上下文对象
#vars: the context variables. 上下文变量
#locale: the context locale.  位置
#request: (only in Web Contexts) the object.HttpServletRequest请求对象
#response: (only in Web Contexts) the object.HttpServletResponse响应对象
#session: (only in Web Contexts) the object.HttpSession session对象
#servletContext: (only in Web Contexts) the object.ServletContext  ServletContext对象
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/105a1451939e4d4cb4a0abc262430719.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这样就可以向jsp中那样,将信息存储在与域对象中然后在取出来,如果可以的话还可以在Thymeleaf中做一些数据处理



除了这些基本对象之外，Thymeleaf还将为我们提供一组实用程序对象，这些对象将帮助我们在表达式中执行常见任务。

```html
#execInfo: information about the template being processed.
#messages: methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
#uris: methods for escaping parts of URLs/URIs
#conversions: methods for executing the configured conversion service (if any).
#dates: methods for java.util.Date objects: formatting, component extraction, etc.
#calendars: analogous to #dates, but for java.util.Calendar objects.
#numbers: methods for formatting numeric objects.
#strings: methods for String objects: contains, startsWith, prepending/appending, etc.
#objects: methods for objects in general.
#bools: methods for boolean evaluation.
#arrays: methods for arrays.
#lists: methods for lists.
#sets: methods for sets.
#maps: methods for maps.
#aggregates: methods for creating aggregates on arrays or collections.
#ids: methods for dealing with id attributes that might be repeated
```
一些常用的内置对象案例
```html
<p data-th-text="${#lists.size(goodsInfo)}">
<p data-th-text="${#aggregates.avg(arrayDemo)}">
<p data-th-text="${#sets.isEmpty(set)}">

<p data-th-text="${#messages.msg('hello')}">
<p data-th-text="${#calendars.format(today,'dd MM yyyy' )}">
<p data-th-text="${#uris.escapePath('www.baidu.com')}">
<p data-th-text="${#dates.create(1990,10,19)}">
<p data-th-text="${#dates.format(today,'yyyy,MM,dd')}">
<p data-th-text="${#dates.year(today)}">
<p data-th-text="${#dates.month(today)}">
<p data-th-text="${#dates.day(today)}">

<p data-th-text="${#strings.startsWith('ABC','A')}"></p>
<p data-th-text="${#strings.endsWith('ABC','A')}"></p>
<p data-th-text="${#strings.isEmpty('ABC')}"></p>
<p data-th-text="${#strings.concat('ABC','D')}"/>
<p data-th-text="${#strings.contains('ABC','C')}"/>
<p data-th-text="${#strings.indexOf('ABC','C')}"/>
<p th:text="${#numbers.listFormatCurrency(arrayDemo)}"/>
   <hr>
<p data-th-text="${#numbers.formatDecimal(1084.87,1,'COMMA',2,'POINT')}"/>
1表示整数位至少保持一位(如果不够则补0),COMMA表示, point表示 .      
```
部分结果----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/1701acfb2f3849089de1df7539f285ab.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```html
request:<br/>
<span th:text="${#httpServletRequest.getAttribute('msg')}"></span><br/>
<span th:text="${#request.getAttribute('msg')}"></span><br/>
<span th:text="${msg}"></span><br/>
session:<br/>
<span th:text="${#httpSession.getAttribute('msg')}"></span><br/>
<span th:text="${#session.getAttribute('msg')}"></span><br/>
<span th:text="${session.msg}"></span><br/>
application:<br/>
<span th:text="${#servletContext.getAttribute('msg')}"></span><br/>
<span th:text="${application.msg}"></span><br/>

```

## 可选参数

可选参数

```c
Not only can variable expressions be written as ${...}, but also as *{...}.
```

```c
两种方式----
1,${}
2,*{}
星号语法作用 于所选对象上的表达式，而不是整个上下文上的表达式。
如果没有选定的对象，${}和*{}的操作完全相同。
```

例如----->>官方给出的案例

```html
 <div th:object="${session.user}">
    <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
    <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
    <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
  </div>
```
上述等同于

```html

<div>
  <p>Name: <span th:text="${session.user.firstName}">Sebastian</span>.</p>
  <p>Surname: <span th:text="${session.user.lastName}">Pepper</span>.</p>
  <p>Nationality: <span th:text="${session.user.nationality}">Saturn</span>.</p>
</div>
```


实际运用---->>

```html
<div th:object="${goods}">
<!--可以理解为在这个标签中定义了一个变量,就累兹于java中的 Person per= new Person("name",age) ,然后用一个变量来接收这个对象  Object obj=per;-->
    <p>id: <span th:text="*{id}">Sebastian</span></p>
    <!--这样就很自然的调用属性-->
    <p>goodsname: <span th:text="*{goodsname}">Pepper</span>.</p>
    <p>price: <span th:text="*{price}">Saturn</span>.</p>
    <p>price: <span th:text="${#object.price}">Saturn</span>.</p>
    <p>goodsdesc: <span th:text="*{goodsdesc}">Saturn</span>.</p>
    <p>goodsdesc: <span th:text="${goods.goodsdesc}">Saturn</span>.</p>

</div>

<span>
    <p>goodsdesc: <span th:text="*{goods.goodsdesc}">Saturn</span>.</p>
</span>
```
实际上如果没有定义`th:object`那么`*{}`与`${}`达到的效果是一样的;
# Thymeleaf遍历信息

 **list**

```html

<table id="GoodsTable" cellpadding="0px" cellspacing="0px">
    <tr>
        <th>菜品编号</th>
        <th>菜品名称</th>
        <th>菜品价格</th>
        <th>菜品描述</th>
    </tr>

    <tr th:each="goods:${goodsInfo}">
    <td th:text="${goods.id}"></td>
    <td th:text="${goods.goodsname}"></td>
    <td th:text="${goods.price}"></td>
    <td th:text="${goods.goodsdesc}"></td>
    </tr>>
</table>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/912c1a3f1fbd462b86bab184dfd8dae3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
直接取Map:
很多时候我们不存JavaBean而是将一些值放入Map中，再将Map存在Model中，我们就需要对Map取值，对于Map取值你可以`${Map名['key']}`来进行取值。也可以通过${Map名.key}取值，当然你也可以使用`${map.get('key')}`(java语法)来取值，完整代码如下：

```html
<h2>Map</h2>
<table>
    <tr>
        <td>place:</td>
        <td th:text="${map.get('place')}"></td>
    </tr>
    <tr>
        <td>feeling:</td>
        <td th:text="${map['feeling']}"></td>
    </tr>
</table>
```
遍历Map：
如果说你想遍历Map获取它的key和value那也是可以的，这里就要使用和List相似的遍历方法，使用th:each="item:${Map名}"进行遍历，在下面只需使用item.key和item.value即可获得值。完整代码如下：

```html
<h2>Map遍历</h2>
<table >
    <tr th:each="item:${map}">
        <td th:text="${item.key}"></td>
        <td th:text="${item.value}"></td>
    </tr>
</table>
```
案例--->>

```java
    @RequestMapping("/showA")
    public ModelAndView showA(Map<String, Object> map) {
        List<Goods> goods = goodsService.showGoods();
        ModelAndView modelAndView = new ModelAndView();
        Map <String,Goods> map1 = new HashMap<>(6);
        for (Goods g :
                goods) {
            map1.put(g.getId().toString(),g);
        }
        map.put("goodsMap",map1);
        modelAndView.setViewName("sample4");
        return modelAndView;
    }
    @RequestMapping("/removeGoods")
    public String removeGoods(Integer id){
        int i = goodsService.removeInfo(id);
        return "redirect:showA";
    }
```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>菜单</title>
    <style type="text/css">
        #GoodsTable {
            width: 80%;
            border: 1px solid blue;
            margin: 0px auto;
        }

        #GoodsTable th, td {
            border: 1px solid green;
            text-align: center;
        }

        #GoodsTable1 {
            width: 80%;
            border: 1px solid blue;
            margin: 0px auto;
        }

        #GoodsTable1 th, td {
            border: 1px solid green;
            text-align: center;
        }
        .content{
            background-color: red;
        }
    </style>
</head>
<body>



<table id="GoodsTable1" cellpadding="0px" cellspacing="0px">
    <tr>
        <th>键值</th>
        <th>菜品编号</th>
        <th>菜品名称</th>
        <th>菜品价格</th>
        <th>菜品描述</th>
        <th>优惠信息</th>
        <th>操作</th>
    </tr>
    <!--    <div th:if="${goodsInfo}!=null">&lt;!&ndash;如过为null则不显示&ndash;&gt;-->
<!--遍历出的元素,要遍历的数据-->
    <tr th:each="goods,k:${goodsMap}">
        <td th:text="${goods.key}">
        <td th:text="${goods.value.id}">
        <td th:text="${goods.value.goodsname}">
        <td th:text="${goods.value.price}">
        <td th:text="${goods.value.goodsdesc}">
        <td th:text="(${goods.value.goodsname}=='炸花生米')?'今日特价':''"></td>
        <td><a data-th-href="@{/removeGoods(id=${goods.value.id})}">删除</a></td>
    </tr>
    <!--    </div>-->

</table>

</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/b90381807856460c88925549906242f7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


小结---->>

```html
 <tr th:each="遍历出的每个变量:${后端传入的数据名}">
```
示例中u为迭代遍历。



> `th:each="element,status:${listinfo}"` 其中status表示迭代状态。
>
> 1,index:当前迭代器的索引 从0开始
>
> 2,count:当前迭代对象的计数 从1开始
>
> 3,size:被迭代对象的长度
>
> 4,even/odd:布尔值，当前循环是否是偶数/奇数 从0开始
>
> 5,first:布尔值，当前循环的是否是第一条，如果是返回true否则返回false
>
> 6,last:布尔值，当前循环的是否是最后一条，如果是则返回true否则返回false

```html

<table id="GoodsTable1" cellpadding="0px" cellspacing="0px">
    <tr>
        <th>索引</th>
        <th>序号</th>
        <th>总数</th>
        <th>偶数索引</th>
        <th>基数索引</th>
        <th>第一?</th>
        <th>最后?</th>
        <th>菜品编号</th>
        <th>菜品名称</th>
        <th>菜品价格</th>
        <th>菜品描述</th>
        <th>优惠信息</th>
    </tr>
<!--    <div th:if="${goodsInfo}!=null">&lt;!&ndash;如果为null则不显示&ndash;&gt;-->

        <tr th:each="goods,g:${goodsInfo}">
            <td th:text="${g.index}">
            <td th:text="${g.count}">
            <td th:text="${g.size}">
            <td th:text="${g.odd}">
            <td th:text="${g.even}">
            <td th:text="${g.first}">
            <td th:text="${g.last}">
            <td th:text="${goods.id}"></td>

            <td th:text="${goods.goodsname}"></td>
            <td th:text="${goods.price}"></td>
            <td th:text="${goods.goodsdesc}"></td>
            <td th:text="(${goods.goodsname}=='宫保鸡丁')?'今日特价':''" ></td>
        </tr>
<!--    </div>-->

</table>


```

![在这里插入图片描述](https://img-blog.csdnimg.cn/0df70feebb08411fa09bacbedfdce8b9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
放上官方给出的案例--->>

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all" 
          href="../../../css/gtvg.css" th:href="@{/css/gtvg.css}" />
  </head>

  <body>

    <h1>Product list</h1>
  
    <table>
      <tr>
        <th>NAME</th>
        <th>PRICE</th>
        <th>IN STOCK</th>
      </tr>
      <tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
      </tr>
    </table>
  
    <p>
      <a href="../home.html" th:href="@{/}">Return to home</a>
    </p>

  </body>

</html>
```
可以迭代的对象,
 - 任何实现的对象java.util.Iterable 
 - 任何实现 的对象。java.util.Enumeration
 - 任何实现的对象，其值将在迭代器返回时使用，而无需在内存中缓存所有值。
 - 任何实现java.util.Iterator的对象。迭代映射时，迭代变量将属于 类 。java.util.Mapjava.util.Map.Entry
 - 任何数组。
 - 任何其他对象都将被视为包含对象本身的单值列表。

## 运算符

### 算数运算符


```
算数运算符
<p data-th-text="1+1">
<p data-th-text="10-1">
<p data-th-text="1*1">
<p data-th-text="1/1">
<p data-th-text="10%1">
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6fa40bc443114414b7c170bf43d4cc14.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
某些算术运算也可用+-*/%

```
<div th:with="isEven=(${prodStat.count} % 2 == 0)">
```

请注意，这些运算符也可以应用于 OGNL 变量表达式本身（在这种情况下，将由 OGNL 而不是 Thymeleaf 标准表达式引擎执行）：

```
<div th:with="isEven=${prodStat.count % 2 == 0}">
```

请注意，其中一些运算符存在文本别名：div  即/ mod 即%


### 关系运算符
```
关系运算符
<p data-th-text="10>1">
<p data-th-text="10 gt 1">
<p data-th-text="1<0">
<p data-th-text="1 lt 0">
<p data-th-text="1 eq 1">
<p data-th-text="1==1">
略

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f1d63312222149a19a3a433b391c45eb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
XML 确定不应在属性值中使用 和 符号，因此应将它们替换为相应的符号
```html
<div th:if="${prodStat.count} &gt; 1">
<span th:text="'Execution mode is ' + ( (${execMode} == 'dev')? 'Development' : 'Production')">

更简单的替代方法可能是使用其中一些运算符存在的文本别名

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6611d342d1944e04af411a2830e2ca27.png)

### 逻辑运算符

```html
**逻辑运算符**
<!--<p data-th-text="10>1 and 1==0">
<p data-th-text="10 gt 1 && 1==9">
<p data-th-text="1<0 || 2 gt 1">
<p data-th-text="1 lt 0 or 10 > 9 ">
<p data-th-text="1 lt 0 or 10 ge 9 ">-->

**逻辑运算符**
<p data-th-if="10 > 1 and 1==0">2</p>
<p data-th-if="10 gt 1 and 1 eq 9">0</p>
<p data-th-if="1 < 0 or 2 gt 1">2</p>
<p data-th-if="1 lt 0 or 10 > 9 ">1</p>
<p data-th-if="1 lt 0 or 10 ge 9 ">1</p>
<div th:text="1>0 and 2<3"></div>
<div th:text="1>0 and 2>3"></div>
<div th:text="1>0 or 2<3"></div>
<div th:text="1>0 or 2>3"></div>
```
在最新版的thymeleaf中对&& 与 ||不太 友好,报错了
![在这里插入图片描述](https://img-blog.csdnimg.cn/6703921a6e8d44e582bec87579898326.png)

### 条件运算符

```
<div th:text="${1==1}?a:b"></div>

 <td th:text="(${goods.goodsname}=='宫保鸡丁')?'今日特价':''" ></td>
```
注意:
![在这里插入图片描述](https://img-blog.csdnimg.cn/70f13dba765f4c3a80a3ed5a245785f8.png)
像这种空的时候,字符串默认为'',如果是其他数据则是null

### 条件运算符
条件表达式旨在仅计算两个表达式中的一个，具体取决于计算条件的结果（条件本身是另一个表达式）。

让我们看一个示例片段（引入另一个属性修饰符， ）：th:class

```html
<tr th:class="${row.even}? 'even' : 'odd'">
  ...
</tr>
```
后端
```java
   map.put("ifU",true);
```

```html
<span data-th-text="${ifU}?'hello':'你好'"></span>
<!--前端结果-->
hello
```
条件表达式也可以使用括号嵌套：
```html
<tr th:class="${row.even}? (${row.first}? 'first' : 'even') : 'odd'">
  ...
</tr>
```

Else 表达式也可以省略，在这种情况下，如果条件为 false，则返回 null 值：
```html
<tr th:class="${row.even}? 'alt'">
  ...
</tr>
```
**默认值--->>>**
在freemarker中也有默认值的表达方式---->>>>

```html
${!''}
```
而在thymeleaf中,默认表达式是一种不带 then 部分的特殊条件值。它等效于某些语言（如Groovy）中存在的Elvis运算符，允许您指定两个表达式：如果计算结果不为null，则使用第一个表达式，但如果计算结果为null，则使用第二个表达式。
例如---->>

```html
<div th:object="${session.user}">
  ...
  <p>Age: <span th:text="*{age}?: '(no age specified)'">27</span>.</p>
</div>
```

如您所见，运算符是 ，我们在这里使用它来指定名称的默认值（在本例中为文本值），仅当计算结果为 null 时。因此，这等效于：?:*{age}

```html
<p>Age: <span th:text="*{age != null}? *{age} : '(no age specified)'">27</span>.</p>
```

与条件值一样，它们可以在括号之间包含嵌套表达式：

```html
<p>
  Name: 
  <span th:text="*{firstName}?: (*{admin}? 'Admin' : #{default.username})">Sebastian</span>
</p>

```

```html
<span data-th-text="${ifU}?'hello':'你好'"></span>
```


## 链接网址

URL是Web应用程序模板中的一等公民，Thymeleaf标准方言为它们提供了特殊的语法，即语法：@@{...}
有不同类型的网址：
**绝对网址---->>>**
绝对网址：http://www.thymeleaf.org

```html
<img data-th-src="@{https://pic1.zhimg.com/v2-d58ce10bf4e01f5086c604a9cfed29f3_r.jpg?source=1940ef5c}" width="100px" height="100px">
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7024beacdfb5451a9f110f1f9da02553.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**相对网址---->>**
页面相对：user/login.html
或者是上下文相对：
（将自动添加服务器中的上下文名称）/itemdetails?id=3
相对于服务器：（允许在同一服务器中调用另一个上下文（= 应用程序）中的 URL。~/billing/processInvoice
```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.ModelAndView;
@Controller
public class testController {
    @RequestMapping("/test/{name}")
    @ResponseBody
    public String test01( @PathVariable String name) {
        return name;
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/340f3ec5080140f2a471e36b66f90aef.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/4f80b9a948b54702be7679b56b0efed0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**传入多个参数**

```c
如果需要多个参数，这些参数将用逗号分隔：@{/order/process(execId=${execId},execType='FAST')}
URL 路径中也允许使用变量模板：@{/order/{orderId}/details(orderId=${orderId})}
```

```java
@Controller
public class testController {

    @RequestMapping("/test/{name}/{age}")
    @ResponseBody
    public String test01(@PathVariable String name, @PathVariable Integer age) {
        return name + age;
    }
}
//<a data-th-href="@{/test/张三/29}">点我呀</a>

```


>这些表达式的实际处理及其到将要输出的 URL 的转换由注册到正在使用的对象中的接口的实现完成。org.thymeleaf.linkbuilder.ILinkBuilderITemplateEngine

默认情况下，此接口的单个实现是类的注册，这对于脱机（非Web）和基于Servlet API的Web方案都足够了。其他方案（如与非 ServletAPI Web 框架的集成）可能需要链接生成器界面的特定实现。org.thymeleaf.linkbuilder.StandardLinkBuilder


除了上述方式还可以通过请求发或者响应重定向来实现另一个业务需求;
比如删除商品信息

**controller层**

```java
@RequestMapping("/showall")
    public ModelAndView showall(Map<String, Object> map) {
    Goods goodsById = goodsService.findGoodsById(1);
    map.put("goods",goodsById);

    List<Goods> goods = goodsService.showGoods();
    map.put("goodsInfo", goods);
    map.put("set",new HashSet<>());
    map.put("today", new Date());
    int[]i={1,2,3};
    map.put("arrayDemo",i);
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.setViewName("sample3");
    return modelAndView;
    }
    @RequestMapping("/removeInfo")
    public String removeInfo(Integer id){
        int i = goodsService.removeInfo(id);
        return "redirect:showall";
    }

```

```java
<table id="GoodsTable1" cellpadding="0px" cellspacing="0px">
    <tr>
        <th>索引</th>
        <th>序号</th>
        <th>总数</th>
        <th>偶数索引</th>
        <th>基数索引</th>
        <th>第一?</th>
        <th>最后?</th>
        <th>菜品编号</th>
        <th>菜品名称</th>
        <th>菜品价格</th>
        <th>菜品描述</th>
        <th>优惠信息</th>
        <th>操作</th>
    </tr>
    <!--    <div th:if="${goodsInfo}!=null">&lt;!&ndash;如过为null则不显示&ndash;&gt;-->
<!--遍历出的元素,要遍历的数据-->
    <tr th:each="goods,g:${goodsInfo}">
        <td th:text="${g.index}">
        <td th:text="${g.count}">
        <td th:text="${g.size}">
        <td th:text="${g.odd}">
        <td th:text="${g.even}">
        <td th:text="${g.first}">
        <td th:text="${g.last}">
        <td th:text="${goods.id}"></td>
        <td th:text="${goods.goodsname}"></td>
        <td th:text="${goods.price}"></td>
        <td th:text="${goods.goodsdesc}"></td>
        <td th:text="(${goods.goodsname}=='地三鲜')?'今日特价':''"></td>
        <td><a data-th-href="@{/removeInfo(id=${goods.id})}">删除</a></td>
    </tr>
    <!--    </div>-->
</table>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ad74ae76ef9142d380d500a465bd3dd8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
如果是请求转发,则地址栏上会发生变化
![在这里插入图片描述](https://img-blog.csdnimg.cn/503ddcb711ad43ae9ae43049a2a6a3e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

引入css

```html
 <link rel="stylesheet" th:href="@{index.css}">
```

引入JavaScript：

```html
 <script type="text/javascript" th:src="@{index.js}"></script>
```

小案例--->>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.gavin.mapper.GoodsMapper">
  <resultMap id="BaseResultMap" type="com.gavin.pojo.Goods">
    <id column="id" jdbcType="INTEGER" property="id" />
    <result column="goodsName" jdbcType="VARCHAR" property="goodsname" />
    <result column="price" jdbcType="FLOAT" property="price" />
    <result column="goodsDesc" jdbcType="VARCHAR" property="goodsdesc" />
  </resultMap>
  <sql id="Base_Column_List">
    id, goodsName, price, goodsDesc
  </sql>
  <select id="findGoods" resultType="com.gavin.pojo.Goods">
    select <include refid="Base_Column_List"></include> from goods
  </select>
  <select id="findOne" resultType="com.gavin.pojo.Goods">
    select <include refid="Base_Column_List"></include> from goods where id=#{param1} and goodsname= #{param2}
  </select>

  <delete id="removeInfo" >
    delete  from goods where id = #{param1} and goodsname= #{param2}
  </delete>
</mapper>
```
后端:----->>>

```java
    @RequestMapping("/showA")
    public ModelAndView showA(Map<String, Object> map) {
        List<Goods> goods = goodsService.showGoods();
        ModelAndView modelAndView = new ModelAndView();

        map.put("goodsInfo",goods);
        modelAndView.setViewName("sample4");
        return modelAndView;

    }

    @RequestMapping("/removeGoods")
    public String removeGoods(Integer id,String goodsname){
        int i = goodsService.removeInfo(id,goodsname);
        return "redirect:showA";
    }
    @ResponseBody
    @RequestMapping("/comments")
    public String comments(Integer  id,String goodsname){
        Goods goodsById = goodsService.findGoodsById(id,goodsname);
        return goodsById.getGoodsname();
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7e09d14cf18f486d9b7c4cb628ccd9a5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

这里实现了弹窗提醒,主要是运用了th:click方式来触发js函数;
超链接：

```html
<a th:href="@{index.html}">超链接</a>
```
>th:onclick 
>给元素绑定事件,单击事件并传递参数
>写法1:仅仅支持数字和布尔类型参数的传递,字符串不支持

```html
 <a href="javascript:viod(0)"  
 th:onclick="'del('+${emp.empno}+')'">删除</a>
```

写法2:支持数字和文本类型的参数传递

```html
<a href="javascript:void(0)" th:onclick="delEmp([[${emp.empno}]],[[${emp.ename}]])">删除</a>
```

## 文件模板

模板的标准存放位置/templates/footer.html

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <body>
  
    <div th:fragment="copy">
      &copy; 2022 The Good Thymes Virtual Grocery
    </div>
  
  </body>
  
</html>
```

## 内容模板

### 定义和引用片段

在我们的模板中，我们经常希望包含其他模板中的部分，如页脚，页眉，菜单等部分...
我们想要在某个模板中添加一些别的模板中的或者其资源中的信息,我们会用到
th:insert th:replace th:include

案例---我们要在页面底部添加一些公司以及认证的商标信息,如果在每一个页面中添加会比较繁琐,于是可以单独建一个页面来实现对该页面元素的引用
定义片段 :(页面名称footer)
```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

<body>

<div th:fragment="copy">
    &copy; 2022 The Good Thymes Virtual Grocery
</div>

</body>

</html>
```
**th:insert**插入
那么我们只需要在需要的页面中引入以下内容就可以达到效果

```html
<div th:insert="~{footer :: copy}"></div>

```
等效代码---->>>

```html
<div th:insert="footer :: copy"></div>
```
页面效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/9828ecba8c31473e8711c58f9bd7c0b5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

在这里有一个关键的----->>th:fragment

```html
 <div th:fragment="copy">
      &copy; 2011 The Good Thymes Virtual Grocery
    </div>
```
th:fragment 就像一个选择器,
首先定义一个模板 
**th:fragment="selector"**


```html
<span th:fragment="header">
    Happy New Year
</span>
```
当在别的页面引用的时候格式---->>

```
"~{templatename::selector}"
或者
"(templatename::selector)"
```

th:insert ="~{被引用的页面名即模板名::片段名}",
当然更简便的是
th:insert="(被引用的页面名::片段名)"
```html
<span th:insert="~{footer::header}"></span>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f83421f87d5b405db686d9229b953d17.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc1a4581c2334b66807e6f43ddfb3d7e.png)

```
~{::selector}"或插入来自同一模板的片段，匹配 。
如果在显示表达式的模板上找不到，则模板调用（插入）
堆栈将遍历到最初处理的模板（根），直到在某个级别匹配。
"~{this::selector}"selectorselector

这种片段方法的一大优点是，
可以将片段写入浏览器完全可显示的页面中，
具有完整甚至有效的标记结构，
同时仍然保留使Thymeleaf将它们包含到其他模板中的能力。
```

即使不定义 fragment片段,我们也可以完成插入/引用
例如:
footer.html
```html
<div id="copy-section">
  &copy; 2011 The Good Thymes Virtual Grocery
</div>
```

```html
<div th:insert="footer :: #copy-section"></div>
<!-- 等价于  <div th:insert="~{footer :: #copy-section}"></div> -->
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/8703a00f0ee44b3eab246f5f63a4f024.png)
这个就跟元素选择器很像了,通过其属性引用它，类似于CSS选择器：id
![在这里插入图片描述](https://img-blog.csdnimg.cn/1c13b89ba6be488584ab3921b40e5d56.png)
除此之外还有----->>    th:replace    th:include,
 **th:replace** 替换

```html
<div th:fragment="copy">
    &copy; 2022 The Good Thymes Virtual Grocery1
<div/>

<div id="copy">
    &copy; 2022 The Good Thymes Virtual Grocery2
<div/>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/3a98c2a9f4b6479ca81173e57c0c6db1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**th:include**只插入内容

```html
<span th:include="footer::#copy3">ThymeLeaf2</span>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2be32bbb30694cf1972e1476a68f76d7.png)

三种的对比
>th:insert是最简单的：它只会插入指定的片段作为其主机标记的主体。

>th:replace实际上将其主机标记替换为指定的片段。

>th:include与 类似，但不是插入片段，而是仅插入此片段的内容。

但是在thymeleaf3.0及以后的版本中不在建议这么食用这三种标签,所以了解即可;
>刚刚测试每修改一次就需要重启一些服务,比较繁琐,有一种方式是引入spring开发工具来实现修改之后的自动编译

**引入依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <version>2.6.3</version>
</dependency>

```
设置一下idea
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c927bf452f64601a9a09cb9aae4c822.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
设置registry
![在这里插入图片描述](https://img-blog.csdnimg.cn/296c7cf3929f44978897d7646e8aace0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/b073aa7e5f7447b3805273e3144a644a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


**片段的参数化**
为模板片段创建更像函数的机制，定义的片段可以指定一组参数：th:fragment

## 模板引擎对比

```c
jsp
优点：
1、功能强大，可以写java代码
2、支持jsp标签（jsp tag）
3、支持表达式语言（el）
4、官方标准，用户群广，丰富的第三方jsp标签库
缺点：
性能问题。不支持前后端分离

freemarker
FreeMarker是一个用Java语言编写的模板引擎，它基于模板来生成文本输出。FreeMarker与Web容器无关，即在Web运行时，它并不知道Servlet或HTTP。它不仅可以用作表现层的实现技术，而且还可以用于生成XML，JSP或Java 等。
目前企业中:主要用Freemarker做静态页面或是页面展示

优点：
1、不能编写java代码，可以实现严格的mvc分离
2、性能非常不错
3、对jsp标签支持良好
4、内置大量常用功能，使用非常方便
5、宏定义（类似jsp标签）非常方便
6、使用表达式语言
缺点：
1、不是官方标准
2、用户群体和第三方标签库没有jsp多


Thymeleaf
Thymeleaf是个XML/XHTML/HTML5模板引擎，可以用于Web与非Web应用。
Thymeleaf的主要目标在于提供一种可被浏览器正确显示的、格式良好的模板创建方式，因此也可以用作静态建模。你可以使用它创建经过验证的XML与HTML模板。相对于编写逻辑或代码，开发者只需将标签属性添加到模板中即可。接下来，这些标签属性就会在DOM（文档对象模型）上执行预先制定好的逻辑。Thymeleaf的可扩展性也非常棒。你可以使用它定义自己的模板属性集合，这样就可以计算自定义表达式并使用自定义逻辑。这意味着Thymeleaf还可以作为模板引擎框架。
优点：静态html嵌入标签属性，浏览器可以直接打开模板文件，便于前后端联调。springboot官方推荐方案。
缺点：模板必须符合xml规范
VUE: 前后端分离,最多,未来趋势
```
<center><b>持续更新中<b><center>





![](..\pic\div\plant.gif)