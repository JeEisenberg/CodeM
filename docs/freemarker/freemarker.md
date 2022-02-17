<div style="float:center"><img src="../pic/div/dog.gif"/>

# FreeMarker

FreeMarker 是一款 模板引擎： 即一种基于模板和要改变的数据， 并用来生成输出文本(HTML网页，电子邮件，配置文件，源代码等)的通用工具。 它不是面向最终用户的，而是一个Java类库，是一款程序员可以嵌入他们所开发产品的组件。


freemaker在前后端交互中起一个什么样的作用呢?

首先来看freemarker所处的位置,是前端和后端之间,后端发来的数据经过模板生成了静态页面,然后发送到前端解析器去解析
![在这里插入图片描述](https://img-blog.csdnimg.cn/40caf3984b0a455e90f77693ecec174f.png)

查看依赖关系,发现了springcontext依赖,这也验证了freemarker在上下文关系中的作用
![在这里插入图片描述](https://img-blog.csdnimg.cn/1ca0c1b589c8419f8e185df906d1e363.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
编写一个freemarker模板---->>
就是一个静态的模板,模板内容按照html格式写,之不顾哦文件类型(扩展名为ftlh)

```html
<!DOCTYPE html>

<html lang="en">
<meta charset="UTF-8">
<head>
    <title>Title</title>
</head>
<body>
this is a free-Marker-style-page</br>

姓名:${name}</br>
年龄:${age}
</body>
</html>

```
简单的后端代码--->>>

```java
@Controller
public class FreeMarkerController {
    @RequestMapping("/test01")
    public String test01(Map<String, Object> map) {
        map.put("name", "张三");
        map.put("age", 18);
        return "index";
    }

}

```

访问一下---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/46e420fbca3f41e88449eafbf35ac78a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

验证-----如果ftlh是一个像jsp那样的动态页面.那么修改ttlh中内容会在重新编译后才生效,看一下源代码,是一个静态页面的内容;

![在这里插入图片描述](https://img-blog.csdnimg.cn/8fcfe5eb59b1415287c62b0900362074.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> freemarker并不关心数据的来源，只是根据模板的内容，将数据模型在模板中显示并输出文件;

所以一个简单的FreeMarker案例就是这样

![在这里插入图片描述](https://img-blog.csdnimg.cn/12f8d81811dc49bbbf57b1bc3cc9ebaa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

>  模板和静态HTML是相同的，只是它会包含一些 FreeMarker 将它们变成动态内容的指令：

通常模板文件存放在服务器上,当有人来访问这个页面， FreeMarker将会介入执行，然后动态转换模板，用后端得到的数据内容替换模板中 ${} 的部分， 之后将结果作为一个静态资源发送到访问者的Web浏览器中。


![在这里插入图片描述](https://img-blog.csdnimg.cn/0b1ee5744839417db02a5d9693e936e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## FreeMaker的数据模型
在FreeMarker中又两种数据模型
第一种hash表形式

```
(root)
  |
  +- animals
  |   |
  |   +- mouse
  |   |   |   
  |   |   +- size = "small"
  |   |   |   
  |   |   +- price = 50
  |   |
  |   +- elephant
  |   |   |   
  |   |   +- size = "large"
  |   |   |   
  |   |   +- price = 5000
  |   |
  |   +- python
  |       |   
  |       +- size = "medium"
  |       |   
  |       +- price = 4999
  |
  +- message = "It is a test"
  |
  +- misc
      |
      +- foo = "Something"
```
访问形式----->>animals.mouse.price。


令一种形式---->>
就时将每个属性的名换成序号

```
(root)
  |
  +- animals
  |   |
  |   +- (1st)
  |   |   |
  |   |   +- name = "mouse"
  |   |   |
  |   |   +- size = "small"
  |   |   |
  |   |   +- price = 50
  |   |
  |   +- (2nd)
  |   |   |
  |   |   +- name = "elephant"
  |   |   |
  |   |   +- size = "large"
  |   |   |
  |   |   +- price = 5000
  |   |
  |   +- (3rd)
  |       |
  |       +- name = "python"
  |       |
  |       +- size = "medium"
  |       |
  |       +- price = 4999
  |
  +- misc
      |
      +- fruits
          |
          +- (1st) = "orange"
          |
          +- (2nd) = "banana"
```

访问形式 animal[0].price

上述存储单值的变量 (size, price, message 和 foo) 称为 scalars (标量)

## 标量的数据类型

字符串：上面带引号的那些就是

日期/时间: 可以是日期-时间格式(存储某一天的日期和时间)， 或者是日期(只有日期，没有时间)，或者是时间(只有时间，没有日期)。

布尔值：对应着对/错(是/否，开/关等值)类似的值。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2d29804e40cf45dc9e9145beed48b25e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
FreeMarker区别字符串，数字，布尔值和日期类型的值。比如， 字符串 "150" 看起来很像数字 150， 字符串只是字符的任意序列，不能将它用于计算目的，也不能和其它数字进行比较
数字:   500,注意带引号的500与不带引号的是完全不同的;

## freemarker其他基础知识

![在这里插入图片描述](https://img-blog.csdnimg.cn/397d4eee76bf4402be1b43753993edd9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/bf219e902e6a42f78111b32594b05b26.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![字符串插值是允许的](https://img-blog.csdnimg.cn/2fa548e199cb47818f0da91e39d7ec52.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/edc185de1fee43f8abd13afbe245b3ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


小结---->

> freeemarker数据模型 
> 1,是一种树形结构,访问时类似于hash表 
> 2,标量类型比较简单,字符串,数字,日期,布尔
> 3,如果是序列,则序列号从0开始

## 关于值域
值域也是序列，但它们由指定包含的数字范围所创建， 而不需指定序列中每一项。

**值域的表示形式**

1,start..end----------------->>包含结尾的,就像数学中的 [start,end]
2,start..!end或者start..<end----->>>不包含结尾的,[start,end)
3,start..*length----->>指定长度的值域
例如100..*2---->>100,101
4,start..  -------无右边界值域

> 但是处理(无右边界值域时要万分小心，处理所有项时， 会花费很长时间，直到内存溢出应用程序崩溃。 和限定长度的值域一样，当它们被切分时，
> 遇到切分后的序列或字符串结尾时，切分就结束了。
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/4c01db8de2b14e2b8ad76c1f03d6d3c5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**序列的拆分**

**最后就是学习FreeMarker的一些常用指令**

## FreeMarker的一些常用指令

**FTL标签与注释**

![在这里插入图片描述](https://img-blog.csdnimg.cn/b49550f62b1f403b87438a45317e8ecf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**if指令**

这里的if指令跟动态sql的很类似
关于if指令,随着案例一起学习----加深印象吧;


后端---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/119667f8a4644525858d6af86f71d4b0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


前端页面

```html
<!DOCTYPE html>

<html lang="en">
<meta charset="UTF-8">
<head>
    <title>Title</title>
</head>
<body>
this is a free-Marker-style-page</br>

${name}今年${age}岁了,是一个
<#if age &gt; 16 >
    成年人
<#else>
    未成年人
</#if>
That is free-marker-demo
</body>
</html>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7671122a765f401589cd451780cd95ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

前端源代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a6a664c149e401d8f1c759077fa6a1f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_17,color_FFFFFF,t_70,g_se,x_16)


if标签还可以这样用,通过模板来读取问文件中的数据;

```html
<#if userage &gt; age>
    ${name}比${username}大
<#else>
    ${name}比${username}小
</#if>
</br>
That is free-marker-demo
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4053b87d88034372abfd471c3c7552bf.png)

> 日期：日期变量可以存储和日期/时间相关的数据。 一共有三种变化：

>日期：精确到天的日期，没有时间部分。比如April 4, 2003。

>时间:精确到毫秒，没有日期部分。比如10:19:18 PM。

>日期-时间(有时也被称为"时间戳")，比如April 4,2003 10:19:18 PM。 有日期和时间两部分，时间部分的存储精确到毫秒。



在实际运用种需要明确时间的类型后或者告诉freemarker时间模板![在这里插入图片描述](https://img-blog.csdnimg.cn/f0a200a13bab45fb8ec01325f4cec35c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```html
<body>
${GoodDate?string("yyyy-MM-dd")}<#--指定一个模板-->
${GoodDate?date_if_unknown}<#--明确时间类型-->
${GoodDate?datetime_if_unknown}
</body>
</html>
```


小结--->>>
内建函数日期格式化
显示年月日:         ${today?date}
显示时分秒：      ${today?time}
显示日期+时间：${today?datetime}
自定义格式化：   ${today?string("yyyy年MM月")}
## freemarker中的算术运算符




if标签基本就是这样的

**list指令**

如果后端传过来的是集合,那么list就是必要的,

list遍历的格式--->>

```html
<#list ["apple", "banana", "orange"] as fruit>
    ${fruit}
</#list>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2f245e739c2844c8ac29beb00bd30500.png)

**list遍历集合---java中的list**

这就需要前面提到的数据模板的理论


后端返回商品信息后到前端,期间经过freemarker模板处理
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>菜单</title>
    <style type="text/css">
        #GoodsTable{
            width: 80%;
            border: 1px solid blue;
            margin: 0px auto;
        }
        #empTable th,td{
            border: 1px solid green;
            text-align: center;
        }
    </style>
</head>
<body>
<table id="GoodsTable" cellpadding="0px" cellspacing="0px">
    <tr>
        <th>菜品编号</th>
        <th>菜品名称</th>
        <th>菜品价格</th>
        <th>菜品描述</th>
    </tr>
    <#list GoodsMap as goods>
        <tr>
            <td>${goods.id}</td>
            <td>${goods.goodsname}</td>
            <td>${goods.price}</td>
            <td>${goods.goodsdesc}</td>
        </tr>
    </#list>
</table>
</body>
</html>
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/ad899d7ea65043a68194d369bc0bf464.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
前端页面代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/352f2d5ab61c422c9dcfc63fbf5bd176.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

**list遍历java中的map**

为了便于理解,还是用上面的Goods作为数据源

这次为每一个goods信息做一下变动,已商品编号作为key,商品信息作为value
后端---->>


```java
    @RequestMapping("/showInfo2")
    public ModelAndView ShowGoods2() {
        List<Goods> goods = goodsService.showGoods();
        ModelAndView modelAndView = new ModelAndView();
        Map<String, Goods> GoodsMap = new HashMap<>();
        for (Goods g :
                goods) {
            GoodsMap.put(g.getId().toString(), g);

        }
        modelAndView.addObject("GoodsMap", GoodsMap);
        modelAndView.setViewName("index2");
        return modelAndView;
    }

}
```
先看下前端怎么输出map
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ce8651472924922ae9368138d68e0f9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
试了一下,key必须为字符串,日期,布尔类型的数据,或者可以自动转换为这些类型的数据才可以,如果不是就会报错;

>  Expected a sequence or string or something automatically convertible to string (number, date or boolean)

![在这里插入图片描述](https://img-blog.csdnimg.cn/8aef8ab3fade4f0ea1139de583984d28.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>菜单</title>
    <style type="text/css">
        #GoodsTable{
            width: 80%;
            border: 1px solid blue;
            margin: 0px auto;
        }
        #empTable th,td{
            border: 1px solid green;
            text-align: center;
        }
    </style>
</head>
<body>


<table id="GoodsTable" cellpadding="0px" cellspacing="0px">
    <tr>
        <th>菜品编号</th>
        <th>菜品ID</th>
        <th>菜品名称</th>
        <th>菜品价格</th>
        <th>菜品描述</th>
    </tr>
    <#list GoodsMap?keys as goods>
        <tr>
            <td>${goods_index}</td>
            <td>${goods}</td>
            <td>${GoodsMap[goods].goodsname}</td>
            <td>${GoodsMap[goods].price}</td>
            <td>${GoodsMap[goods].goodsdesc}</td>
        </tr>
    </#list>
</table>
</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6635dc16d11d4793baba7c35a3ef7f76.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以发现 由于hashmap的特点,使得菜品ID顺序并不是按照原来的顺序显示的;
![在这里插入图片描述](https://img-blog.csdnimg.cn/f9b4682299364bf382f31b1551f88945.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 特殊数据的处理

什么是特殊数据?其实就是null值,当后端查询到的信息为null时,如果后端不做处理,而是让前端处理---->>
**freemarker不支持null。如果值为null会报错**


假如后端查询的数据为null
这里又分为几种情况
第一种----->>查询的数据为null,
![在这里插入图片描述](https://img-blog.csdnimg.cn/5edd083144ae47d9978200e3d3f64e81.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/9ef5f6f481da4d3faa2f479116722673.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

前端展示时会报错提示为null,需要有默认值之类的错误提示;
![在这里插入图片描述](https://img-blog.csdnimg.cn/0144a437bd9f429a9a701e5aef259f8e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
联想实际生活,既然没有那么就不展示了,
这里的逻辑就是if标签, GoodsMap??表示如果GoodsMa为null,那么就不会展示下面的内容

```html
    <#if GoodsMap??>
    <#list GoodsMap as goods>
        <tr>
            <td>${goods_index}</td>
            <td>${goods.id}</td>
            <td>${goods.goodsname}</td>
            <td>${goods.price}</td>
            <td>${goods.goodsdesc}</td>
        </tr>
    </#list>
    </#if>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f215556de9494da18cb4836c4afc8bcf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/67cfad648e2f4e6397988a59873a0c91.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




第二种是数据表中的某些数据为null
比如这下面的菜单表

![在这里插入图片描述](https://img-blog.csdnimg.cn/6974f8f308c74d9299779f03aa3267fb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

直接显示数据也会出现错误,

>freemarker.core.InvalidReferenceException: The following has evaluated
> to null or missing:
> ==> GoodsMap[goods].price  [in template "index2.ftlh" at line 37, column 19]





 那如何处理呢?
 需要在值的后加感叹号处理----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/e80c8b674cc540cbbf5604f1ab0e5288.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
前端显示---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/8af0ea368772435a9129fb39274fb205.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


前端代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/c7fa21b1953f4af38f8789ea42b582fb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

现在虽然对null做了处理不报错了,但是当为null时,需要做提示
试了一下这个

```
<td>${((GoodsMap[goods].goodsname!'无')</td>
```
发现没有结果,
![在这里插入图片描述](https://img-blog.csdnimg.cn/1c92f03963fe4bfe8edc46758d0dde9b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
于是乎以为是版本的问题,但是我试了其他版本.结果是一样的
忽然灵光一闪,可能是空字符串的原因吧

于是乎按照空字符串特意处理了一下


```<td>${((GoodsMap[goods].goodsname!'')?length &gt;0)?string((GoodsMap[goods].goodsname!''),"无")}</td>

```


![在这里插入图片描述](https://img-blog.csdnimg.cn/8b32f4540ecc4faca787f6fa8a6952bc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

所以----

> freemarker  ! 在处理null的时候会将其转为空字符串



小结--->>

> 1、判断某变量是否存在使用 “??” 用法为:variable??,如果该变量存在,返回true,否则返回false 例：为防止stus为空报错可以加上判断如下
> 2、缺失变量默认值使用 “!” 使用!要以指定一个默认值，当变量为空时显示默认值。例： ${name!''}表示如果name为空显示空字符串。如果是嵌套对象则建议使用（）括起来

## FreeMarker内置函数
freemarker中的对象
其实也不算是对象只是看起来符合json格式的字字符串

```html
<#assign person="{'name':'张三','age','28'}"/>
<#-- 转换为对象 -->
<#assign data=person?eval/>
姓名:${data.name}<br>
年龄:${data.age}
```
更多内置函数可以参考API
[freemarker中文文档](http://freemarker.foofun.cn/dgui_template_exp.html#dgui_template_exp_builtin)

以上内容可能比较胡乱,自己也是看着freemarker的中文API进行整理的,有些跳跃,在使用的时候发现有一些API在最新版本的spring框架中并不太好用;
[spring官方推荐thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-utility-objects)

<div style="float:center"><img src="..\pic\div\rabbit2.gif"/></div>

