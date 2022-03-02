# KindEditor

## KindEditor简介

>KindEditor是一套开源的HTML可视化编辑器，主要用于让用户在网站上获得所见即所得编辑效果，兼容IE、Firefox、Chrome、Safari、Opera等主流浏览器。
>KindEditor是基于JavaScript的插件。里面包含了丰富的组件，如：多文件上传组件、富文本编辑框,使用KindEditor可以大大的降低页面开发难度;

<div  align="center" ><iframe   style="width: 648px; height: 502px;" src="https://static-1ea97ceb-5948-482f-9b54-1ba103bd1713.bspapp.com"></iframe></div>


## KindEditor特点

1. 体积小，加载速度快，但功能十分丰富。
2. 内置自定义range，完美地支持span标记。
3. 基于插件的方式设计，所有功能都是插件，增加自定义和扩展功能非常简单。
4. 修改编辑器风格很容易，只需修改一个CSS文件。
5. 支持大部分主流浏览器，比如IE、Firefox、Safari、Chrome、Opera。

[KindEditor下载](http://kindeditor.net/down.php)

## KindEditor的使用
新建一个项目~目录结构~

也可根据需求删除以下目录

    asp - ASP程序
    asp.net - ASP.NET程序
    php - PHP程序
    jsp - JSP程序
    examples - 演示文件

**上传文件~**

```js
// ASP
KindEditor.ready(function(K) {
        K.create('#textarea_id', {
                uploadJson : '../asp/upload_json.asp',
                fileManagerJson : '../asp/file_manager_json.asp',
                allowFileManager : true
        });
});
// ASP.NET
KindEditor.ready(function(K) {
        K.create('#textarea_id', {
                uploadJson : '../asp.net/upload_json.ashx',
                fileManagerJson : '../asp.net/file_manager_json.ashx',
                allowFileManager : true
        });
});
// JSP
KindEditor.ready(function(K) {
        K.create('#textarea_id', {
                uploadJson : '../jsp/upload_json.jsp',
                fileManagerJson : '../jsp/file_manager_json.jsp',
                allowFileManager : true
        });
});
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/14ead3c3a6164a74ba8e4d1b5afb4cef.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

导入依赖~

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.6.4</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>2.6.4</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
            <version>2.6.4</version>
        </dependency>

    </dependencies>

```
将下载的kindeditor解压,将解压后的文件拷贝到staic目录下(想什么jsp.asp,php什么的可以舍弃)

前端代码~

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="/js/kindeditor/themes/default/default.css"/>
    <script src="/js/kindeditor/kindeditor-all-min.js" rel="script" type="text/javascript"></script>
    <script src="/js/kindeditor/lang/zh-CN.js"rel="script" type="text/javascript"></script>


    <script type="text/javascript">
        KindEditor.ready(function(K) {
            var editor = K.editor({
                allowFileManager : true,
                 uploadJson:'upload'
            });
            K('#J_selectImage').click(function() {
                editor.loadPlugin('multiimage', function() {
                    editor.plugin.multiImageDialog({
                        clickFn : function(urlList) {
                            var div = K('#J_imageView');
                            div.html('');
                            K.each(urlList, function(i, data) {
                                div.append('<img src="' + data.url + '" width="80">');
                            });
                            editor.hideDialog();
                        }
                    });
                });
            });
        });
    </script>
</head>
<body>
<input type="button" id="J_selectImage" value="批量上传" />
<div id="J_imageView"></div>
</body>
</html>
```
后端代码~

```java
package com.gavin.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class FileController {

@RequestMapping("/")
    public String showIndex(){

    return "index";
    }
}

```
请求之后~

![在这里插入图片描述](https://img-blog.csdnimg.cn/9791f4f2c9f048c0b24bbff441499649.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

>可以使用IE内核的浏览器~搜狗(技术的落后呀)

点击上传~发送到的地址是这个,http://localhost:8090/upload?dir=image![在这里插入图片描述](https://img-blog.csdnimg.cn/06c81a9bf0f74e15ad86f87a97f10ed2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

然后该请求会返回一个json格式字符串~

```json
//返回格式(JSON) 
//成功时
{ 
        "error"  :  0 , 
        "url"  :  "http://www.example.com/path/to/file.ext" 
} 
//失败时
{ 
        "error"  :  1 , 
        "message"  :  “错误信息” 
}

```

这个url就是存放图片地址的url

![在这里插入图片描述](https://img-blog.csdnimg.cn/3de55ad8a6ee4d20a1f74c6bd103d620.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
接下来就是整合fastds和nginx完成文件的上传下载
编写后端代码~

```java
package com.gavin.service;

import org.springframework.web.multipart.MultipartFile;

import java.util.Map;

public interface Fileservice {

    Map<String,Object> upload(MultipartFile multipartFile);
}

```

```java
package com.gavin.service.impl;

import com.gavin.service.Fileservice;
import com.gavin.utils.FastDFSClient;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

import java.util.HashMap;
import java.util.Map;
@Service
public class FileServiceImp implements Fileservice {
    @Override
    public Map<String, Object> upload(MultipartFile multipartFile) {

        Map<String, Object> map = new HashMap<>();

        try {
            String[] result = FastDFSClient.uploadFile(multipartFile.getInputStream(), multipartFile.getOriginalFilename());
            //上传成功
            map.put("error", 0);//这楼里要和返回的json字符串匹配
            //url设置了nginx解析模板,所以这样写
            map.put("url", "http:192.168.135.145:8888/" + result[0] +"/"+ result[1]);
        } catch (Exception e) {
            e.printStackTrace();
        }
            //上传失败
        map.put("error", 1);
        map.put("message", "上传失败!");
        return map;
    }
}

```
控制层~

```java
package com.gavin.controller;
import com.gavin.service.Fileservice;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;
import java.util.Map;

@Controller
public class FileController {
@Autowired
    private Fileservice fileservice;
@GetMapping("/")
    public String showIndex(){

    return "index";
    }
    @PostMapping("/upload")
//    跟前端名name一致
    @ResponseBody//返会json格式
public Map<String,Object> upload(MultipartFile imgFile){
        Map<String, Object> upload = fileservice.upload(imgFile);
        return upload;
}


}

```
启动fastdfs,nginx以及springboot后测试正常;

[kindeditor的其他使用场景可以参考官网api;](http://kindeditor.net/demo.php)
