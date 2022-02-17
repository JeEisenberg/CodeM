![](..\pic\logo2.png)

# Maven

**Maven的安装\配置**

Maven 的主要目标是让开发人员在最短的时间内了解开发工作的所有状态。为了实现这一目标，马文涉及几个令人关切的领域：

使构建过程变得简单
提供统一的构建系统
提供高质量的项目信息
鼓励更好的发展做法



[去官网下载适合的maven](https://maven.apache.org/download.cgi)

这里用到的是3.8.3版本;

下载之后解压,将文件放到一个合适的目录,我是放在了
![在这里插入图片描述](https://img-blog.csdnimg.cn/ca11fb0b8cb3416cb4430924f2df1841.png)之后开始配置maven,maven跟java一样,都需要有一个path的环境变量----

注意---->>在配置maven之前一定要配置好java环境变量;

检查一下java环境变量----->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/8cd7cecca34c4513978d9990ed0e4365.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/1452505e4f864f9e8405168eae7274c4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **在配置maven时,你可以可建一个MAVEN_HOME,  path中引用MAVEN_HOME的方式也可以**

## 配置 maven的 环境变量
在path环境变量下添加maven配置,目标位maven的bin目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/12310e12678241ff9d01c48fa4d16d25.png)
配置完之后检查配配置是否成功-----
在控制输入  mvn -v,如果有反应,说明配置成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/95514fdbc4024507b0bfa35b8599b2b6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
虽然配置成功了,但是由于是第一次配置,还需要一些其他依赖文件,在控制台输入--

```xml
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

注意,这会在你本地用户文件夹下创建一个maven项目,找起来比较麻烦,所以建议自己指定一个文件夹来存放maven项目,


你也可以模仿我这么操做----

控制台进入D:\Program Files\mavenProjects文件夹,然后执行上方命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/63921a04d987499092ee328eb27cf4c3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这个操做会从官网下载一些依赖以及一些配置文件,执行成功会有 BUILD SUCCESS提示;
![在这里插入图片描述](https://img-blog.csdnimg.cn/e660e6ba460141f7a592acfe51ad8a4d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
同时检查

## maven项目目录----->>>

出现my-app说明操做成功,至此maven安装与配置就结束了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/cbb58acf9d4349ae89b50d6db4f0b69b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)生成目标创建了my-app名称目录。。

cd my-app
根据此目录，您将注意到以下标准项目结构。

my-app
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- mycompany
    |               `-- app
    |                   `-- App.java
    `-- test
        `-- java
            `-- com
                `-- mycompany
                    `-- app
                        `-- AppTest.java
目录包含项目源代码，目录包含测试源，文件是项目的项目对象模型（POM）

最后说一下如何让卸载maven,由于不是直接安装的maven,所以
卸载起来很简单------>>
1,环境变量中删除maven配置,
2,删除maven所在文件
3清理maven仓库-----即项目存放的地方


maven中一些常用的命令

构建项目

> mvn package
>

 **这需要在pom.xml所在文件夹下执行,要不然会提示找不到pom.xml文件错误;**

打包好之后控制台会提示----
![在这里插入图片描述](https://img-blog.csdnimg.cn/7270501728e2471ebdba6035673ff515.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这是我们打包好的jar文件位置所在D:\mavenProjects\my-app\target\my-app-1.0-SNAPSHOT.jar



用以下命令测试新编译和打包的 JAR：

> java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App

![在这里插入图片描述](https://img-blog.csdnimg.cn/d0fdc378d8074e2e88cacb868fb0c175.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/6c167d021a194ddf92e2b503a780ecbe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> mvn clean dependency:copy-dependencies package

此命令将清理项目、复制依赖项并打包项目（当然，执行所有阶段到包）。


生成站点
mvn site


## maven的使用
要了解maven是什么,首先得分析一下我们传统写java项目的时候 ,我们经常会导入一些依赖包,比如jdbc驱动,jstl标签库,log4j等等,将他们放在classpath中,虽然这些工作难度不大,但是比较繁琐;
maven针对这个问题专门位java项目的开发提供了一套管理和构建工具;

提供了一套**标准化的项目结构**；
提供了一套**标准化的构建流程**（**编译，测试，打包，发布**……）；
提供了一套**依赖管理**机制。
Maven项目结构
一个使用Maven管理的普通的Java项目，它的目录结构默认如下：

a-maven-project     ---项目名
├── pom.xml  -- 项目描述文件
├── src ---项目根目录
│   ├── main  
│   │   ├── java   ---java源码存放处
│   │   └── resources  ---资源文件
│   └── test   ---测试项目的目录
│       ├── java---java源码存放处
│       └── resources ---资源文件
└── target 编译,打包好的文件存放处



pom.xml文件分析
![](https://img-blog.csdnimg.cn/1e1220a3bbc3405ead1ecc0b21effb85.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)groupId类似于Java的包名，通常是公司或组织名称，artifactId类似于Java的类名，通常是项目名称，再加上version，一个Maven工程就是由groupId，artifactId和version作为唯一标识。我们在引用其他第三方库的时候，也是通过这3个变量确定。
![在这里插入图片描述](https://img-blog.csdnimg.cn/4268f3f660a44174b23e9dcd713d3742.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)通过上述3个变量，即可唯一确定某个jar包。Maven通过对jar包进行PGP签名确保任何一个jar包一经发布就无法修改。修改已发布jar包的唯一方法是发布一个新版本。

因此，某个jar包一旦被Maven下载过，即可**永久地安全缓存在本地。**


以上是maven本地加载的一些核心包名
,如果要使用的包,那么需要配置依赖-----

```xml
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>
```
使用<dependency>声明一个依赖后，Maven就会自动下载这个依赖包并把它放到classpath中。


那maven是从哪里下载我们需要的依赖包呢?
首先maven维护了一个仓库,基本上所有的依赖包都会上传到这个仓库,这个就不需要我们关心了;

那有一个小细节,我们怎么知道对应的 <groupId>  <artifactId>以及 <version>呢?

我们可以通过maven仓库 来搜索关键字 搜索得到上面三个信息---
[Maven Central Repository Search](https://search.maven.org/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/10579e418acd42eeaba71e80ea4b517d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)从maven的官方仓库下载一些依赖包会比较慢,一般是使用ALIYUN的仓库替代官方仓库
使用Maven镜像仓库需要一个配置，在用户主目录下进入.m2目录，创建一个settings.xml配置文件，内容如下：

```xml
<settings>
    <mirrors>
        <mirror>
            <id>aliyun</id>
            <name>aliyun</name>
            <mirrorOf>central</mirrorOf>
            <!-- 国内推荐阿里云的Maven镜像 -->
            <url>https://maven.aliyun.com/repository/central</url>
        </mirror>
    </mirrors>
</settings>
```
上述三个元素指向一个项目的特定版本，让 Maven 知道当在其软件生命周期中，我们正在处理哪个项目;

配置好后接下来就是maven的一些基础知识

## 包的依赖关系

在java中,maven解决的是有关包的依赖关系,
在maven中定义了几种依赖关系---->>>
complie   这说明编译时需要用到该jar包    例如 commons-logging

test  在测试环境下 需要用到该jar包  例如junit 
runtime  编译时不需要,但是运行时需要用到 例如 jdbc驱动
provided 编译时需要用到,但运行时由某个JDK或者某个服务器提供 例如 某个servlet

### complie

complie是最基本的也最常用 ,maven会将这种类型的依赖直接放入classpath,如果不声明则scope默认为complie

### test
测试时会用到,打包时不会放到项目中,比如junit的jar包
```xml
 <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```
### runtime
运行时才会依赖的jar包,比如数据库连接
```xml
<dependency>
<groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.27</version>
    <scope>runtime</scope>
</dependency>
```
### provided
编译和测试项目的时候需要该依赖，但在运行项目的时候，由于容器已经提供，就不需要Maven重复地引入一遍(如：servlet-api)

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.0</version>
    <scope>provided</scope>
</dependency>
```

### system
一些本地包可以这么引入
```xml
        <dependency>
            <!--注册自定义jar包-->
            <groupId>com.gavin.test</groupId>
            <artifactId>Maven_gavin</artifactId>
            <version>1.0-SNAPSHOT</version>
            <scope>system</scope>
            <systemPath>com.gavin.demo</systemPath><!--url -->
        </dependency>
```


### import
import范围只适用于pom文件中的<dependencyManagement>部分。表明指定的POM必须使用<dependencyManagement>部分的依赖。
注意：import只能用在dependencyManagement的scope里。


![在这里插入图片描述](https://img-blog.csdnimg.cn/a042df1e984a4c399e297a2ec0381ce6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

import用于<dependencyManagement>中,主要用于对版本的管理

指定一个maven项目中的pom.xml为父类pom,

![在这里插入图片描述](https://img-blog.csdnimg.cn/64b342acfd8f46b9a3ce5654059fb38e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 如果子类pom写上版本号,则会按照子类中的版本号走;

但是父类pom可以强制子类pom使用其指定的版本



maven会对开发版的项目中的一些依赖每次都会重新下载依赖,在maven中以SNAPSHOT结尾的版本号会被视为开发版本,这种SNAPSHOT版本只能用于内部私有的Maven repo，公开发布的版本不允许出现SNAPSHOT
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd9a37d082ee42b6a40c70a3b3a82336.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
在配置好maven环境时候,下载的资源中已经为我们准备好一个项目---myapp,我们可以通过这个来测试一下一些常用命令---


mvn -v    ----查看maven版本以及JDK和系统版本

mvn package   -----打包项目

java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App  ----  运行项目


运行项目的命令解析---  
 java -cp 

   target ----项目存放相对地址 ,也可以写项目绝对地址不过要把证地址准确性
my-app-1.0-SNAPSHOT.jar ---jar包名

com.mycompany.app----包名即 pom.xml中配置的groupId![](https://img-blog.csdnimg.cn/9a7ae777327544ef9b96a19d99a0de9f.png)

> <modelVersion>.POM 型号版本（始终为 4.0.0）。
>
> <groupId>.项目所属的团体或组织。通常表示为倒置域名。
>
> <artifactId>.要给项目图书馆文物起的名字（例如，其 JAR 或 WAR 文件的名称）。
>
> <version>.正在构建的项目版本。
>
> <packaging>- 应如何包装项目。JAR 文件包装的默认"jar"。使用"war"进行war文件包装。
>
>
> 实践出真知.....加油


maven中各个文件夹的含义--见名知意
![在这里插入图片描述](https://img-blog.csdnimg.cn/d0a963adbfc24abe86ac6e322804bcc4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## maven本地仓库的配置

将maven全局配置的文件按复制到本地仓库文件夹下
![在这里插入图片描述](https://img-blog.csdnimg.cn/c5e975f251cf41208765049db1fba03f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_19,color_FFFFFF,t_70,g_se,x_16)
在setings.xml文件夹下打开注释---
![在这里插入图片描述](https://img-blog.csdnimg.cn/28789c86144c49e3a59422d2b85de207.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## maven远程仓库的配置

> “远程”仓库可能是你的公司拥有的建立在文件或HTTP服务器上的内部仓库(不是Apache的那个中央仓库，而是你们公司的私服，你们自己在局域网搭建的maven仓库)，用来在开发团队间共享私有构件和管理发布的。

由于maven服务器在国外,国内下载一些依赖的时候速度会比较慢,这时候可以设置以下远程仓库为中央仓库的镜像仓库
国内一般推荐阿里云的镜像仓库


![在这里插入图片描述](https://img-blog.csdnimg.cn/d63fccac9a444327b7f7d1d7e1d69fa1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以指定多个镜像仓库,    


## 仓库优先级的问题---->>>

项目在查找依赖的时候,先从本地仓库查找,找不大的话,如果配置了镜像仓库则从镜像仓库找,再找不到就从中央仓库找;

## 配置maven项目编译运行环境配置
当我们电脑中有很多个JDK的时候,我们需要指定一个maven的运行和编译环境
![在这里插入图片描述](https://img-blog.csdnimg.cn/3e43c12b83ab4986a665ae698c4de004.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
小结---->>

> 一般我们在配置maven环境时只需要配置用户环境,全局环境一般不做修改;
> 同时我们只需要在用户配置中添加本地仓库,远程仓库以及JDK版本信息就可以了;


## 在Idea中创建maven工程


![在这里插入图片描述](https://img-blog.csdnimg.cn/60d890dafd564fa0a6bbd18b30698758.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
现阶段不选择模板而是建一个标准的maven工程
![在这里插入图片描述](https://img-blog.csdnimg.cn/33ba0e798e8745588fe2a846c674755b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

maven标准目录结构---

![在这里插入图片描述](https://img-blog.csdnimg.cn/be7af2aadc4542b4a6fd0c152db742ad.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 目录名字不可以随便改，因为maven进行编译或者jar包生成操作的时候，是根据这个目录结构来找的，你若轻易动，那么久找不到了。

除此之外还有一些其他文件夹,由于没有打包项目,还没有展示
下面打包项目-----

![在这里插入图片描述](https://img-blog.csdnimg.cn/29fd0dc157694f2e8e3ed93ff3232d15.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
打包结束后会生成一个target文件夹,用于存放项目打包后的文件;![在这里插入图片描述](https://img-blog.csdnimg.cn/6d992891523c4ec895b9a316e18d0af8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 这个跟我们在控制台上操做是一样的; 
> 1,控制台进入pom.xml文件所在文件夹,
>  2,执行打包命令 mvn package
> 3,打包结束后运行---- java -cp  命令 来测试打包后的文件是否可以运行


## maven中pom.xml的配置

基本的配置----->>
上面已经提到了
接下来需要实际操做---配置一些依赖



先做几个例子

![在这里插入图片描述](https://img-blog.csdnimg.cn/22e887c8d29b4036be03db09a1aeff61.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
配置依赖时爆红了,原因是我们没有更新依赖----



![在这里插入图片描述](https://img-blog.csdnimg.cn/4a2ac634d3514bf090b9650d29d5cca3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

更新后就会下载依赖![在这里插入图片描述](https://img-blog.csdnimg.cn/fff8481c34184c51807286ccd9fa5f23.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果不想每次手动同步,则需要设置一下让其自动同步------>>


![在这里插入图片描述](https://img-blog.csdnimg.cn/181eb46608294f31a98ba26013ebb51b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)`Maven警告解决：Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platfor`
在pom.xml中添加配置---->>>

```xml
<properties>
        <project.build.sourceEncoding>
            UTF-8
        </project.build.sourceEncoding>
 </properties>
```

## 添加自定义包的依赖
准备环境----->
一个自定义jar包
就用下面这个吧
![在这里插入图片描述](https://img-blog.csdnimg.cn/40aa61bb766c4b3aa441bb71e1380d21.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 依赖的特性与原则

### 依赖的传递性

新建一个maven 项目,
添加依赖------

![在这里插入图片描述](https://img-blog.csdnimg.cn/6240ba5c8b174ccbbc6243c7a86a99c5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)在添加依赖的时候,会将依赖的依赖也一同导入到项目中,即依赖的传递性;
导入的一些内容可以直接用;
![在这里插入图片描述](https://img-blog.csdnimg.cn/81f75c65ba5b498ebfb60f0f0ecdde51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
依赖关系----->>![在这里插入图片描述](https://img-blog.csdnimg.cn/bb129aac95714c40993fee54a47be5dc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


### 第一原则----依赖的最短路径原则

“最短路径优先”意味着项目依赖关系树中路径最短的版本会被使用。

   例如，假设A、B、C之间的依赖关系是A->B->C->D(2.0)  和A->E->(D1.0)，那么D(1.0)会被使用，因为A通过E到D的路径更短。
 解析---- 
 假如 A依赖B,B依赖C,C依赖D,D的版本为2.0版本
 同时A又依赖E,E依赖D,D的版本为1.0版本,则 D的版本最终被确认为1.0版本;


例如创建Spring项目时引入context依赖会自动引入相关依赖
![在这里插入图片描述](https://img-blog.csdnimg.cn/11831e17d87441b395dd7d91098ae73a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


### 第二原则----第一原则失效时
当第一原则失效时,即路径长短一致时,该怎么确定版本呢?

可以在依赖的时候做排除动作

![在这里插入图片描述](https://img-blog.csdnimg.cn/31af05174e0e41e29f8ca83b23d473b6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

不用关注版本号,因为通过groupId 和artifactId 就能定位到jar包

## 继承与聚合

### 继承关系：

如果A工程继承B工程，则代表A工程默认依赖B工程依赖的所有资源，且可以应用B工程中定义的所有资源信息。

被继承的工程（B工程）只能是POM工程。



    注意：在父项目中放在<dependencyManagement>中的内容时不被子项目继承，不可以直接使用
    
    放在<dependencyManagement>中的内容主要目的是进行版本管理。里面的内容在子项目中依赖时坐标只需要填写

   <group id>和<artifact id>即可。（注意：如果子项目不希望使用父项目的版本，可以明确配置version）。



父工程是一个POM工程：
![在这里插入图片描述](https://img-blog.csdnimg.cn/100d2f76af364fc9a6e8133ac044554f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



### 聚合关系
> 当我们开发的工程拥有2个以上模块的时候，每个模块都是一个独立的功能集合。比如某大学系统中拥有搜索平台，学习平台，考试平台等。开发的时候每个平台都可以独立编译，测试，运行。这个时候我们就需要一个聚合工程。

在创建聚合工程的过程中，总的工程必须是一个POM工程（Maven Project）（聚合项目必须是一个pom类型的项目，jar项目war项目是没有办法做聚合工程的），各子模块可以是任意类型模块（Maven Module）。

聚合的前提：继承。

聚合包含了继承的特性。

聚合时多个项目的本质还是一个项目。这些项目被一个大的父项目包含。且这时父项目类型为pom类型。同时在父项目的pom.xml中出现<modules>表示包含的所有子模块。
![在这里插入图片描述](https://img-blog.csdnimg.cn/638a1b4889cc4b30b2510e9b3c1ffedc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
聚合后生成的jar包目录结构----
![在这里插入图片描述](https://img-blog.csdnimg.cn/a6e980dc0a2f48aebbafd6abafe800e5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




## 插件的引入

### 编译器插件
当我们在一个工程中不想使用本地的jdk进行编辑,那么可以切换一下JDK的版本

![在这里插入图片描述](https://img-blog.csdnimg.cn/2115c1ae32ae4696be19cb303b887d17.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
### 资源拷贝插件
打包时只有放在resource的配置文件才会被打包
![在这里插入图片描述](https://img-blog.csdnimg.cn/843aee3aadaa4ab590727eec58089058.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




那如果想要打包非resource文件下的配置文件,那么就需要配置资源拷贝插件

```xml
 <!--        引入资源复制插件-->
        <resources>
            <resource>
                <directory>src/main/java/com/gavin/app</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/79f79b4d329e4b9ab97a7626c36da298.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)配置好以后，相应位置下的配置文件都会被打包了：

### tomcat插件
tomcat插件的引入需要在一个webapp工程中
新建一个maven工程,模板为webApp

![在这里插入图片描述](https://img-blog.csdnimg.cn/8f4bec5c1f8a4023bdfaaa9346587bea.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)之后在pom.xml中引入tomcat插件
在build下plugins plugin
```xml
<build>
    <plugins>
      <!-- 配置Tomcat插件 -->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
     <!-- 配置Tomcat监听端口 -->
          <port>8080</port>
     <!-- 配置项目的访问路径(Application Context) -->
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
```
怎么证明我们配置好了呢?
由于tomcat插件不会自动运行,我们需要手动开启
开启方式------>>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/2d8ed4e222364720aa1333dbc191c39b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)之后访问index.jsp

![在这里插入图片描述](https://img-blog.csdnimg.cn/00fa720cc39c4f8bb9ed2aeeacdfbf75.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

mvn 常用命令

 install

本地安装， 包含编译，打包，安装到本地仓库

编译 - javac

打包 - jar， 将java代码打包为jar文件

安装到本地仓库 - 将打包的jar文件，保存到本地仓库目录中。

❀ clean

清除已编译信息。

删除工程中的target目录。

 compile

只编译。 javac命令

❀ package

打包。 包含编译，打包两个功能。


install和package的区别：

package命令完成了项目编译、单元测试、打包功能，但没有把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库和远程maven私服仓库

install命令完成了项目编译、单元测试、打包功能，同时把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库，但没有布署到远程maven私服仓库

# 父POM

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>my-app</name>
  <url>http://www.example.com</url>
  <properties>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <repositories>
    <repository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
    </repository>
  </repositories>
  <pluginRepositories>
    <pluginRepository>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
    </pluginRepository>
  </pluginRepositories>
  <build>
    <sourceDirectory>D:\mavenProjects\my-app\src\main\java</sourceDirectory>
    <scriptSourceDirectory>D:\mavenProjects\my-app\src\main\scripts</scriptSourceDirectory>
    <testSourceDirectory>D:\mavenProjects\my-app\src\test\java</testSourceDirectory>
    <outputDirectory>D:\mavenProjects\my-app\target\classes</outputDirectory>
    <testOutputDirectory>D:\mavenProjects\my-app\target\test-classes</testOutputDirectory>
    <resources>
      <resource>
        <directory>D:\mavenProjects\my-app\src\main\resources</directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>D:\mavenProjects\my-app\src\test\resources</directory>
      </testResource>
    </testResources>
    <directory>D:\mavenProjects\my-app\target</directory>
    <finalName>my-app-1.0-SNAPSHOT</finalName>
    <pluginManagement>
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2-beta-5</version>
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.8</version>
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.5.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
    <plugins>
      <plugin>
        <artifactId>maven-clean-plugin</artifactId>
        <version>3.1.0</version>
        <executions>
          <execution>
            <id>default-clean</id>
            <phase>clean</phase>
            <goals>
              <goal>clean</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-resources-plugin</artifactId>
        <version>3.0.2</version>
        <executions>
          <execution>
            <id>default-testResources</id>
            <phase>process-test-resources</phase>
            <goals>
              <goal>testResources</goal>
            </goals>
          </execution>
          <execution>
            <id>default-resources</id>
            <phase>process-resources</phase>
            <goals>
              <goal>resources</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-jar-plugin</artifactId>
        <version>3.0.2</version>
        <executions>
          <execution>
            <id>default-jar</id>
            <phase>package</phase>
            <goals>
              <goal>jar</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.0</version>
        <executions>
          <execution>
            <id>default-compile</id>
            <phase>compile</phase>
            <goals>
              <goal>compile</goal>
            </goals>
          </execution>
          <execution>
            <id>default-testCompile</id>
            <phase>test-compile</phase>
            <goals>
              <goal>testCompile</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.22.1</version>
        <executions>
          <execution>
            <id>default-test</id>
            <phase>test</phase>
            <goals>
              <goal>test</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-install-plugin</artifactId>
        <version>2.5.2</version>
        <executions>
          <execution>
            <id>default-install</id>
            <phase>install</phase>
            <goals>
              <goal>install</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-deploy-plugin</artifactId>
        <version>2.8.2</version>
        <executions>
          <execution>
            <id>default-deploy</id>
            <phase>deploy</phase>
            <goals>
              <goal>deploy</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-site-plugin</artifactId>
        <version>3.7.1</version>
        <executions>
          <execution>
            <id>default-site</id>
            <phase>site</phase>
            <goals>
              <goal>site</goal>
            </goals>
            <configuration>
              <outputDirectory>D:\mavenProjects\my-app\target\site</outputDirectory>
              <reportPlugins>
                <reportPlugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-project-info-reports-plugin</artifactId>
                </reportPlugin>
              </reportPlugins>
            </configuration>
          </execution>
          <execution>
            <id>default-deploy</id>
            <phase>site-deploy</phase>
            <goals>
              <goal>deploy</goal>
            </goals>
            <configuration>
              <outputDirectory>D:\mavenProjects\my-app\target\site</outputDirectory>
              <reportPlugins>
                <reportPlugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-project-info-reports-plugin</artifactId>
                </reportPlugin>
              </reportPlugins>
            </configuration>
          </execution>
        </executions>
        <configuration>
          <outputDirectory>D:\mavenProjects\my-app\target\site</outputDirectory>
          <reportPlugins>
            <reportPlugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-project-info-reports-plugin</artifactId>
            </reportPlugin>
          </reportPlugins>
        </configuration>
      </plugin>
    </plugins>
  </build>
  <reporting>
    <outputDirectory>D:\mavenProjects\my-app\target\site</outputDirectory>
  </reporting>
</project>

```
<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/#为什么会有这个网站)

[<div   style="float:right;width:180px;heigh:20px"/>![](../pic/nextpage.png)</div>](/maven/MavenBase2.md#Maven目录结构)

</center>

<div align=center> <img src="\pic\div\rabbit2.gif" style="height:150px"/></div>