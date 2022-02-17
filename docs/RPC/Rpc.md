![](..\pic\div\deer.gif)

# RPC

## 为什么要学习RPC?

这就要讲项目架构的历史渊源了,在以前互联网不发达的时候,数据访问的并发量不大,这时候的项目大多数是以单体架构为主;
>什么是单体架构?
## 单体架构
一个项目里面的全部代码实现全部的业务功能;

画图便于理解,这里以jd.com为例
![在这里插入图片描述](https://img-blog.csdnimg.cn/b43c5a8e1ae54bc69dd73a2c66f885a8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
当我们要访问业务时,这些业务实现的代码都打包在一个项目包里,这样的做法有优点,也有缺点;
优点:
1,部署简单----部署一个项目代码
2,成本低----只需要一个服务器
3,维护方便----只需维护一个总的项目,但是也是很麻烦的,如果业务之间耦合度大,就要对其他业务逻辑代码重新改写;

缺点也很明显----
1,修改一个业务逻辑或者业务逻辑发生故障,其他业务也需要下线;
2,当项目很复杂,用户访问量大的时候,处理效率低,一个服务器去处理这些会很困难,甚至会出现宕机;
>单架构的项目适合小型的互联网项目;

随着互联网的普及,单架构越来越难以满足高并发,高吞吐量的现实需求,分布式架构也成了人们争相使用的一种架构;

>什么是分布式架构呢?
## 分布式架构
分布式架构就是在原来单体架构额基础上将业务进行拆分(多模块/功能,降低各个业务之间的代码耦合度),然后部署到不同的服务器上;
同样以jd.com为例
![在这里插入图片描述](https://img-blog.csdnimg.cn/c545613e00f14aec9cd1e62c6158b504.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

分布式架构优缺点:
优点:
1,增强了系统的可用性,减少了因为某一个业务逻辑改动(故障)而影响其他业务逻辑的正常运行;
2,增加了模块的复用性,同时也更容易扩展业务逻辑;
3.整体系统的负载能力得到的增强;

缺点:
1,费钱
2,架构比较复杂,部署起来有些难度
3,一些关联业务之间的响应时间会有所延迟


说到了单体架构与分布式架构的特点,单体架构各个业务模块之间的交流会比较容易实现,分布式架构中业务模块之间的通信如何实现?

我们知道我们访问互联网用的是Http来实现的,这是PC与外网之间的通信,那在服务器之间的通信呢?

服务器之间的通信也可以使用Http协议，也可以使用RPC协议通信，也可以使用其他的通信方式实现,RPC更适合项目内部之间的通信;

## RFC协议
RPC在rfc 1831中收录 ，RPC即Remote Procedure Call,是一个远程过程调用协议

>RFC(Request For Comments) 是由互联网工程任务组(IETF)发布的文件集。文件集中每个文件都有自己唯一编号;。

[RPC~远程过程调用协议规范](https://datatracker.ietf.org/doc/rfc1831/)

RPC协议规定**允许互联网中一台主机程序调用另一台主机程序**，而无需对这个交互过程进行编程。在RPC协议中强调当A程序调用B程序中功能或方法时，A是不知道B中方法具体实现的。

举个例子:---

业务服务器A功能实现时需要服务器B中的某些数据,那么A就需要调用(发送请求)服务器B中的功能/方法,但是服务器A并不知道服务器B中的具体方法实现(只需要知道业务接口)

[分布式与集群](https://blog.csdn.net/weixin_54061333/article/details/122273967)

RPC是**上层协议**，底层可以基于TCP协议，也可以基于HTTP协议。一般我们说RPC都是基于RPC的具体实现，如：Dubbo框架。**从广义上讲只要是满足网络中进行通讯调用都统称为RPC**，甚至HTTP协议都可以说是RPC的具体实现，但是具体分析看来RPC协议要比HTTP协议更加高效，基于RPC的框架功能更多。

RPC协议是**基于分布式架构**而出现的，所以RPC在分布式项目中有着得天独厚的优势。

## RPC与HTTP

  **RPC：可以基于TCP协议，也可以基于HTTP协议.**
  1. 自定义具体实现可以**减少很多无用的报文**内容,使得**报文体积更小**,减少了带宽,也减少了服务器的压力; 
  2. 支持长连接;
  3. RPC可以基于**很多序列化方式**;
  4. 一般RPC框架都带有**注册中心**;
    类似于mvc,注册中心就像一个控制层,要找数据要找控制器,由控制器去跟别的服务器联系;
     ![在这里插入图片描述](https://img-blog.csdnimg.cn/cd129ca130a949c3ad5536c99ff80a5a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

  5. 绝大多数RPC框架都带有**负载均衡测量**;
  6. RPC框架一般都带有**丰富的服务治理**等功能，更**适合企业内部接口调用**;


 **HTTP：基于HTTP协议**
  7. HTTP 1.1 报文中很多内容都是无用的。如果是HTTP2.0以后和RPC相差不大，比RPC少的可能就是一些服务治理等功能;
  8. **三次握手四次挥手**;
  9. HTTP 主要是通过JSON，序列化和反序列效率更低;
  10. 链接方式都是**直连**;
  11. 一般都需要借助第三方工具来实现负载均衡测量 ,如ngnix;
 12. HTTP更适合**多平台之间相互调用**。

 **负载均衡**
RPC一般自带负载均衡;
![在这里插入图片描述](https://img-blog.csdnimg.cn/938c8fb10f4742469a3614a936be6540.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
简单的理解就是原来一个业务一个服务器,现在由于也无访问量大,并发数高,将该业务部署在多个服务器上,以减少单一服务器的负载;
![在这里插入图片描述](https://img-blog.csdnimg.cn/14c8870a517c44bd82977a8599f2290e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
公司之内内部的服务器之间传输数据---RPC,公司之外的一般用HTTP比较合适;

## RPC的实现
服务端
新建服务端

```java
package com.gavin;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class RpcController {
@RequestMapping("/show")
@ResponseBody
    public String Show(String hello){

    return "Hello"+hello;
    }
}

```
以上在底层帮我们实现了请求控制,
现在我们手写java代码来实现RPC
首先查找依赖--->>

![在这里插入图片描述](https://img-blog.csdnimg.cn/0163c7998d014154b9a78b0b70d19298.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
根据我的原则~使用新的不使用旧,旧版本在2007年不再维护了,同时阿帕奇在07年之后将httpclient升级为顶级项目;
```xml
 <dependency>
        <groupId>org.apache.httpcomponents</groupId>
        <artifactId>httpclient</artifactId>
        <version>4.5.13</version>
    </dependency>
```
**Get请求**
```java
package gavin;


import org.apache.http.Header;
import org.apache.http.HeaderElement;
import org.apache.http.HttpEntity;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import org.junit.jupiter.api.Test;

import java.io.IOException;
import java.net.URISyntaxException;

/** get方式发送请求得到响应
 * @author Gavin
 */
public class rpcGet {


    @Test
    void testGet(){
        //创建客户端
        CloseableHttpClient client = HttpClients.createDefault();
        //准备url
        URIBuilder uriBuilder = null;
        try {
            uriBuilder = new URIBuilder("http://127.0.0.1:8080/show");
//            请求参数
            uriBuilder.addParameter("hello","get123");
//            通过get方式发送出去
            HttpGet httpGet = new HttpGet(uriBuilder.build());
            System.out.println(httpGet.getURI());
//            客户端执行请求
            CloseableHttpResponse res = client.execute(httpGet);
//            执行后的结果放在entity里面~从这里面去取数据
            HttpEntity entity = res.getEntity();
         
            String result = EntityUtils.toString(entity, "utf-8");
            System.out.println(result);
        } catch (URISyntaxException e) {
            e.printStackTrace();
        } catch (ClientProtocolException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


```
**Post请求**
```java
package gavin;

import org.apache.http.HttpEntity;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.ArrayList;
import java.util.List;

/**post请求得到响应
 * @author Gavin
 */
public class rpcPost {
    public static void main(String[] args) {
//        创建httpclient
        CloseableHttpClient httpclient = HttpClients.createDefault();
//        创建post请求
        HttpPost httpPost = new HttpPost("http://127.0.0.1:8080/show");
//请求体
        List<NameValuePair> list =new ArrayList<>();
        list.add(new BasicNameValuePair("hello","get123"));
        try {
//            将信息放到entity中
            UrlEncodedFormEntity entity = new UrlEncodedFormEntity(list, "utf-8");
//            将请求放到post中
      httpPost.setEntity(entity);
//      处理请求,响应
            CloseableHttpResponse response = httpclient.execute(httpPost);
//            返回entity
            HttpEntity entity1 = response.getEntity();
            String result = EntityUtils.toString(entity1);
            System.out.println(result);
            response.close();
            httpclient.close();

        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (ClientProtocolException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}


```

但有时候我们请求的并不止一个参数,有时候会是一个对象,一个集合,这时候我们就需要将参数转换为对象或者集合


 - 把json字符串转换为对象

```java
ObjectMapper objectMapper = new ObjectMapper();
People peo = objectMapper.readValue(content, People.class);
```

 - 把json字符串转换为List集合

```java
ObjectMapper objectMapper = new ObjectMapper();
JavaType javaType = objectMapper.getTypeFactory().constructParametricType(List.class, People.class);
List<People> list = objectMapper.readValue(content, javaType);
```
大部请求为对象分情况下请求
在前面的基础上我们增加 请求时包含json对象
引入依赖以便将字符串转换为json对象

```xml
<dependency><groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.1</version>
</dependency>
```

控制层的代码-->>

```java
package com.gavin;
import com.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.List;

/**
 * @author Gavin
 */

@Controller
public class RpcController {


@RequestMapping("/show")
@ResponseBody
    public String Show(String hello){

    return "hello"+hello;
    }

    @RequestMapping("/showInfo")
    @ResponseBody
    public User ShowInfo(@RequestBody User user){

        return user;
    }

    @RequestMapping("/showAll")
    @ResponseBody
    public List ShowAll(@RequestBody List<User> userlist){
        System.out.println(userlist);
        return userlist;
    }

}

```

**请求包含json**

>把对象转换为json字符串
```java
package gavin;

import com.fasterxml.jackson.databind.JavaType;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.pojo.User;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.util.List;

/** 发送对象
 * @author Gavin
 */
public class rpcPostJsonData {
    public static void main(String[] args) {
//        创建httpclient
        CloseableHttpClient httpclient = HttpClients.createDefault();
//        post的地址
        HttpPost httpPost = new HttpPost("http://127.0.0.1:8080/showInfo");
        ObjectMapper objectMapper = new ObjectMapper();
        User user= new User("张三",18);

        try {
            String str = objectMapper.writeValueAsString(user);
            System.out.println(str);
            StringEntity stringEntity = new StringEntity(str, ContentType.APPLICATION_JSON);
            httpPost.setEntity(stringEntity);
            CloseableHttpResponse resp = httpclient.execute(httpPost);
            HttpEntity entity = resp.getEntity();
            String result = EntityUtils.toString(entity);
            System.out.println(result);
//将json字符串转换为对象
            User user1 = objectMapper.readValue(result, User.class);
            System.out.println(user1);
            resp.close();
            httpclient.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/fec8ab51c9bb498ea8a112b94f6ff772.png)
> 把json字符串转换为List集合

```java
package com.gavin;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JavaType;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.pojo.User;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

/** 发送对象
 * @author Gavin
 */
public class rpcPostJsonData {
    public static void main(String[] args) throws JsonProcessingException {
//        创建httpclient
        CloseableHttpClient httpclient = HttpClients.createDefault();
//        post的地址
        HttpPost httpPost = new HttpPost("http://127.0.0.1:8080/showAll");
        ObjectMapper objectMapper = new ObjectMapper();
        List<User> list = new ArrayList<>();
        User user = new User("张三1", 18);
        User user1 = new User("张三2", 19);
        User user2 = new User("张三3", 20);
        User user3 = new User("张三4", 21);
        list.add(user);
        list.add(user1);
        list.add(user2);
        list.add(user3);
        try {
            //将对象转换为json字符串
            String str = objectMapper.writeValueAsString(list);
            System.out.println(str);
            StringEntity stringEntity = new StringEntity(str, ContentType.APPLICATION_JSON);
//         将实体放入post

            httpPost.setEntity(stringEntity);
            //响应
            CloseableHttpResponse resp = httpclient.execute(httpPost);
            HttpEntity entity = resp.getEntity();

            String result = EntityUtils.toString(entity);
            System.out.println(result);
//            将json转换为对象
            
//将json字符串转换为对象
            List <User>list1 = objectMapper.readValue(result, List.class);

            JavaType javaType = objectMapper.getTypeFactory().constructParametricType(List.class, User.class);
            List<User>listUser = objectMapper.readValue(result, javaType);
            listUser.forEach(System.out::println);
            resp.close();
            httpclient.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7f18320035d848a0b76fb0efd5d442d7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

如果是异步,且跨域请求--->>

跨域：协议、ip、端口中只要有一个不同就是跨域请求。

	同源策略：浏览器默认只允许ajax访问同源(协议、ip、端口都相同)内容。
	
	解决同源策略：
	
	在控制器接口上添加@CrossOrigin。表示允许跨域。

ajax发送json数据的写法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>静态页面</title>
    <script type="text/javascript" src="../js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function () {
            $("#bt").click(function () {
               // alert("RPCDEMO");
                var info='[{"name":"李四","age":12},{"name":"王五","age":13}]';
                $.ajax({
                    url:'/showAll',
                    type:'post',
                    data:info,//发送数据JSON.stringify(info)

                    contentType:'application/json',/*请求体中内容类型*/
                    dataType:'json',/*响应内容类型*/

                    success:function(data){
                        for (var i = 0; i < data.length ;i++) {
                            alert(data[i].name+"--"+data[i].age);
                        }

                    }
                })
            })
        });
    </script>
</head>
<body>
<button id="bt">this is rpc page</button>

</body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/676c33fc41904445bedfe8bd507c290e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
跨域请求怎么实现呢?
[ajax跨域的实现方式](https://blog.csdn.net/weixin_54061333/article/details/122441195)
本次主要使用注解的方式完成跨域;

~@CrossOrigin
该注解下有很多属性----->>
```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface CrossOrigin {
    /** @deprecated */
    @Deprecated
    String[] DEFAULT_ORIGINS = new String[]{"*"};
    /** @deprecated */
    @Deprecated
    String[] DEFAULT_ALLOWED_HEADERS = new String[]{"*"};
    /** @deprecated */
    @Deprecated
    boolean DEFAULT_ALLOW_CREDENTIALS = false;
    /** @deprecated */
    @Deprecated
    long DEFAULT_MAX_AGE = 1800L;

    @AliasFor("origins")
    String[] value() default {};

    @AliasFor("value")
    String[] origins() default {};//允许可访问的域列表

    String[] originPatterns() default {};

    String[] allowedHeaders() default {};

    String[] exposedHeaders() default {};

    RequestMethod[] methods() default {};

    String allowCredentials() default "";

    long maxAge() default -1L;//准备响应前的缓存持续的最大时间（以秒为单位）
}

```

 - 可以为整个controller启用@CrossOrigin 

![在这里插入图片描述](https://img-blog.csdnimg.cn/b978e8d539c04c18b1aaeea2cc1320ae.png)
 - 也可以为某一个方法启用@CrossOrigin
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/6c4f34cf165d479b97e7d96fc3a730d8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
    跨域实现代码-->>
    准备两个springboot,一个端口8080,一个端口8090,
    然后

![在这里插入图片描述](https://img-blog.csdnimg.cn/3e003e352d1641bf9faa59751e963f57.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
可以发现没有启用跨域时,请求时会报错误
![在这里插入图片描述](https://img-blog.csdnimg.cn/fa0aa6f7fe8849c59e15cb618de883e5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

在controller上或者showAll方法上添加注解--->>
@CrossOrigin,
![在这里插入图片描述](https://img-blog.csdnimg.cn/17d9ebe925e84b33909f5eab702e0afb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

重启后再次测试--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/870f4069c8a04f84858d93a76b5372b9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
在允许跨域时,由于没有指定可以跨域的ip,所以默认是所有的ip,
![在这里插入图片描述](https://img-blog.csdnimg.cn/4d85caabc3b740da98183d50bbac5f58.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

也可以指定允许跨域的ip
>@CrossOrigin(origins = "http://localhost:8090")
>![在这里插入图片描述](https://img-blog.csdnimg.cn/e05c3903c5f94ea6b6b9bc6692d2e8e0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


## RMI实现RPC
 Java RMI，即 远程方法调用(Remote Method Invocation)，一种用于实现远程过程调用(RPC)(Remote procedure call)的Java API， 能直接传输序列化后的Java对象和分布式垃圾收集。它的实现依赖于Java虚拟机(JVM)，因此它仅支持从一个JVM到另一个JVM的调用。
![在这里插入图片描述](https://img-blog.csdnimg.cn/5cad7c2862a043e082bbfc73f87e3fa1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_18,color_FFFFFF,t_70,g_se,x_16)
在客户端和服务端之间还有一个注册空间,
![在这里插入图片描述](https://img-blog.csdnimg.cn/c024021235e94c908c774094e18bd7c3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_15,color_FFFFFF,t_70,g_se,x_16)

Registry(注册表)是放置所有**服务器对象的命名空间**。 每次服务端创建一个对象时，它都会使用**bind()或rebind()方法注册该对象**。 这些是使用称为绑定名称的**唯一名称注册**的。 

	要调用远程对象，客户端需要该对象的引用。即通过服务端绑定的名称从注册表中获取对象(lookup()方法)。

 **Remote**

	java.rmi.Remote 定义了此接口为远程调用接口。如果接口被外部调用，需要继承此接口。

```java
public interface Remote{}
```
远程调用还需要处理一个异常
**RemoteException**

	java.rmi.RemoteException
	
	继承了Remote接口的接口中，如果方法是允许被远程调用的，需要抛出此异常。

  **UnicastRemoteObject**

	java.rmi.server.UnicastRemoteObject
	
	此类实现了Remote接口和Serializable接口。
	
	自定义接口实现类除了实现自定义接口还需要继承此类。

 **LocateRegistry**

注册器,请求时会先发送到注册器,通过注册器映射找到对应的controller

	  java.rmi.registry.LocateRegistry
	  可以通过LocateRegistry在本机上创建Registry，
	  通过特定的端口就可以访问这个Registry。


测试代码-->>

```java
package com.rpc;

import java.rmi.Remote;
import java.rmi.RemoteException;

/**继承 Remote接口,同时接口中的方法要抛出一个异常
 * @author Gavin
 */
public interface RpcService extends Remote {
      String DemoRpc (String param) throws RemoteException;
}

```
service的实现类

```java
package com.rpc.impl;

import com.rpc.RpcService;

import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

public class RpcServiceImpl extends UnicastRemoteObject implements RpcService {
    /**
     * 构造方法访问权限
     * @throws RemoteException  同时要抛出异常
     */
    public RpcServiceImpl()throws RemoteException{

    }
    @Override
    public String DemoRpc(String param) throws RemoteException {
        return param+"rpc";
    }
}

```
>这里因为UnicastRemoteObject内的构造方法为protected 所以要整一个RpcServiceImpl构造方法并抛出异常RemoteException

**开启server**

三步走
```java
package com.server;

import com.rpc.RpcService;
import com.rpc.impl.RpcServiceImpl;

import java.net.MalformedURLException;
import java.rmi.AlreadyBoundException;
import java.rmi.Naming;
import java.rmi.RemoteException;
import java.rmi.registry.LocateRegistry;

public class RpcServer {
    public static void main(String[] args)  {
        try {
            //创建接口实例
            RpcService Rpc= new RpcServiceImpl();
            //在某个端口创建注册表
            LocateRegistry.createRegistry(8081);
            //绑定服务
            Naming.bind("rmi://localhost:8081/rpcService",Rpc);//第二个参数要和RpcService实例名一致;
            System.out.println("服务启动成功");

        } catch (RemoteException e) {
            e.printStackTrace();
        } catch (AlreadyBoundException e) {
            e.printStackTrace();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } finally {
        }
    }
}

```
测试--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/17e361d80cbd4b4e975e917afe7a9830.png)
**开启client**

```java
package com.client;

import com.rpc.RpcService;

import java.net.MalformedURLException;
import java.rmi.Naming;
import java.rmi.NotBoundException;
import java.rmi.Remote;
import java.rmi.RemoteException;

/**
 * @author Gavin
 */
public class RpcClient {
    public static void main(String[] args) {

        try {
            RpcService rpcService = (RpcService) Naming.lookup("rmi://localhost:8081/rpcService");
            String demoRpc = rpcService.DemoRpc("小明");
            System.out.println(demoRpc);
        } catch (NotBoundException e) {
            e.printStackTrace();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (RemoteException e) {
            e.printStackTrace();
        }

    }

}

```

这里有一个问题----client在请求获得返回值时要Remote,但是很显然不能返回接口而要返回一个接口实现类,因此为了简化直接将server中的rpcService复制过来,
> RpcService rpcService = (RpcService) Naming.lookup("rmi://localhost:8081/rpcService");


如果复制过来时的包名不一样就会产生一个绑定异常

```java
java.rmi.UnmarshalException: error unmarshalling return; nested exception is: 
	java.lang.ClassNotFoundException: com.rpc.RpcService (no security manager: RMI class loader disabled)
```

包结构---->>>![在这里插入图片描述](https://img-blog.csdnimg.cn/f07fa30d0b6d431185b43a3f0c357851.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



## RPC框架

<iframe  height=500  width=88%  src="../staticshow/picslide/index.html"  frameborder=0></iframe>