# Apache-Solr

![在这里插入图片描述](https://img-blog.csdnimg.cn/c2dff700e3f94456baf93a4b8072e628.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

# Solr简介

~在海量数据下，对MySQL或Oracle进行模糊查询或条件查询的效率是很低的;~

> Solr 是一个独立的企业搜索服务器，具有类似 REST 的 API。 你把 通过 JSON、XML、CSV 或 HTTP 上的二进制文件（称为“索引”）。 您通过 HTTP GET 查询它并接收 JSON、XML、CSV 或二进制结果。

常见的搜索解决方案~
>基于Apache Lucene（全文检索工具库）实现搜索

>基于谷歌API实现搜索和基于百度API实现搜索;

Solr 在使用 Lucene 搜索库并对此基础上进行了扩展。 

<div  align="center" ><iframe   style="width: 648px; height: 502px;" src="https://upos-sz-mirrorcos.bilivideo.com/upgcxcode/77/63/540706377/540706377-1-160.mp4?e=ig8euxZM2rNcNbRVhwdVhwdlhWdVhwdVhoNvNC8BqJIzNbfq9rVEuxTEnE8L5F6VnEsSTx0vkX8fqJeYTj_lta53NCM=&uipk=5&nbs=1&deadline=1646682329&gen=playurlv2&os=cosbv&oi=3073951711&trid=a7f19f9041dc4efb824c7e9e95efcc49T&platform=html5&upsig=357d34206d15969c20a77cf4cd3ff39c&uparams=e,uipk,nbs,deadline,gen,os,oi,trid,platform&mid=0&bvc=vod&nettype=0&bw=78029&orderid=0,1&logo=80000000"></iframe></div>



## 索引的类型
![在这里插入图片描述](https://img-blog.csdnimg.cn/93a1ab59493f465e9c03fdd592aa67fe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 正向索引
>正向索引：从文档内容到词组的过程。每次搜索的实收需要搜索所有文档，每个文档比较搜索条件和词组。

从文档到关键字,将文档抽出关键字,然后查找是匹配关键字;
### 反向索引
反向索引：是正向索引的逆向。建立词组和文档的映射关系。通过找到词组就能找到文档内容。（和新华字典找字很像）

从关键字到文档,将关键字与文档做映射处理;

1.搜索原理

	Solr能够提升检索效率的主要原因就是------
	分词和索引（通过反向索引)
	
	分词：会对搜索条件/存储内容进行分词，分成日常所使用的词语。
	
	索引：存储在Solr中内容会按照程序员的要求来是否建立索引。
	如果要求建立索引会把存储内容中关键字（分词）建立索引。


![在这里插入图片描述](https://img-blog.csdnimg.cn/46a259678bda4141b7575ff9de6a84bc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>既然solr是一个针对搜索的解决方案,那么它就要支持存储,实际应用中要把数据库中的内容添加到solr中进行索引的初始化,如果数据库中的数据修改了,还需要同步到solr中;


Solr中数据存储是存储在**Document对象**中，对象中可以包含的**属性和属性类型都定义在scheme.xml**中。如果需要**自定义属性或自定义属性类型**都需要**修改scheme.xml配置文件**。
从Solr5开始schema.xml更改名称为managed-scheme(没有扩展名)

## solr的安装部署
系统要求~Java 运行时环境 (JRE) 1.8 或更高版本。 

 - **1.下载solor**

下载可用的solr包~[Solr8.11.1 下载](https://solr.apache.org/downloads.html)  

这里使用linux安装包

![在这里插入图片描述](https://img-blog.csdnimg.cn/037a6ff39fdc4cf791febf5d12fbe51c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - **2.上传文件到linux服务器**

>可以使用rz命令或者其他可视化软件;

 - **3.解压文件到local文件夹下**

```c
cd /usr/local
tar-zxf solr-8.11.1.tgz
```

目录结构~
bin/----包含solr 和 solr.cmd 等几个重要的脚本，solr.cmd /solr.in.sh /是启动和停止 Solr 的首选工具,它们将使 Solr 的使用更加容易。
![在这里插入图片描述](https://img-blog.csdnimg.cn/f4e5272ad84b42bfbdc2155a91600da3.png)post/----用于将各种类型的内容发布到 Solr 服务器 ~对于 Windows（非 Cygwin）使用;
[发布命令案例~](https://solr.apache.org/guide/8_11/post-tool.html#using-the-binpost-tool)

solr.in.sh 和 solr.in.cmd
   > 分别是 linux 和 Windows 系统的属性文件。 Java、Jetty 和 Solr 的系统级属性在此处配置。 使用时可以覆盖bin/solr / bin/solr.cmd其中的许多设置 ，可在一个地方设置所有属性。 
   >  >
   >  > install_solr_services.sh
   >  > 此脚本在 linux 系统上用于将 Solr 安装为服务。

contrib/
    索尔的 contrib目录包括用于 Solr 特殊功能的附加插件。 
dist/
    这 dist目录包含主要的 Solr .jar 文件。 
doc/
    这 docs目录包含指向 Solr 的在线 Javadocs 的链接。 
example/
    这 example目录包含几种类型的示例，这些示例演示了各种 Solr 功能。 请参阅下面的 Solr 示例 此目录中内容的更多详细信息， 
server/
    该目录是 Solr 应用程序的核心所在。 此目录中的 README 提供了详细的概述
        Solr 的管理界面 ( server/solr-webapp)
        码头图书馆（ server/lib)
        日志文件 （ server/logs) 和日志配置 ( server/resources)
        示例配置集 ( server/solr/configsets) 

 - **4.修改配置文件**

进入solr的bin目录下~`cd   /usr/local/solr-8.11.1/bin`
修改solr.in.sh~ `vi solr.in.sh`

修改内容如下~

```c
#公共系统值的设置，在使用系统默认值时可能会导致操作损失。
#Solr可以使用多个进程和多个文件句柄。在现代操作系统上，离开的节省
#这些设置低是微不足道的，而后果可能是Solr不稳定。要关闭这些检查，请设置
#SOLR_ULIMIT_CHECKS=false在这里或作为您的配置文件的一部分。
#也可以在solr.in.sh或您的配置文件中设置不同的限制。
SOLR_ULIMIT_CHECKS= false

```

 - **5.启动solr**

Solr内嵌Jetty，直接启动即可。监听8983端口。
![在这里插入图片描述](https://img-blog.csdnimg.cn/7ac7a34b44eb45f0a32ad0e48800b0ae.png)solr不建议在root权限下启动,在root权限下启动需要强制执行

```c
[root@localhost bin]# ./solr start
*** [WARN] *** Your open file limit is currently 1024.  
 It should be set to 65000 to avoid operational disruption. 
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh
*** [WARN] ***  Your Max Processes Limit is currently 7026. 
 It should be set to 65000 to avoid operational disruption. 
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh
WARNING: Starting Solr as the root user is a security risk and not considered best practice. Exiting.
         Please consult the Reference Guide. To override this check, start with argument '-force'
[root@localhost bin]# 

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d24c5d6d6e9a45a7991b7107cc43244d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)启动之后

```c
Started Solr server on port 8983 (pid=16360). Happy searching!
                                                                         
```
[Solr 控制脚本](https://solr.apache.org/guide/8_11/solr-control-script-reference.html)

solr启动时会扫描主目录即  /usr/local/solr-8.11.1/server/solr

**Standalone Mode**  下的运行目录结构

```xml
<solr-home-directory>/
   solr.xml 
   core_name1/
      core.properties <!--每个核心配的置文件,可以为核心定义特定属性-->
      conf/
         solrconfig.xml
         managed-schema
      data/
   core_name2/
      core.properties
      conf/
         solrconfig.xml
         managed-schema
      data/
```
[solr服务器的配置~solr.xml](https://solr.apache.org/guide/8_11/solr-cores-and-solr-xml.html)

[每个核心的配置~core.properties](https://solr.apache.org/guide/8_11/defining-core-properties.html)

[配置 solrconfig.xml](https://solr.apache.org/guide/8_11/configuring-solrconfig-xml.html) 

[managed-schema字段属性配置](https://solr.apache.org/guide/8_11/documents-fields-and-schema-design.html)

data/包含低级索引文件的目录

**SolrCloud Mode**的运行目录结构



```xml
<solr-home-directory>/
   solr.xml
   core_name1/
      core.properties
      data/
   core_name2/
      core.properties
      data/
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b01448c11ae040c88af59a2c5a463e2b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

>SolrCloud 示例不包括 conf每个 Solr Core 的目录（所以没有 solrconfig.xml或架构文件）。 这是因为配置文件通常在 conf目录存储在 ZooKeeper 中，因此它们可以在集群中传播。 
>
> 

> 如果将 SolrCloud 与嵌入式 ZooKeeper 实例一起使用，您可能还会看到 zoo.cfg和 zoo.dataZooKeeper
> 配置和数据文件。 但是，如果您正在运行自己的 ZooKeeper 集成，您将在启动它时提供自己的 ZooKeeper 配置文件，并且Solr 中的副本将不会被使用;

 - **6.登录可视化界面**

在登陆之前检查一下端口8983是否开放了

```c
[root@localhost bin]# lsof -i:8983
COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
java    16360 root  156u  IPv6 294504      0t0  TCP *:8983 (LISTEN)
[root@localhost bin]# 

```
如果没有开放则使其开放

```c
[root@manentost bin]#  sudo firewall-cmd --zone=public --add-port=8983/tcp --perm
success
[root@localhost bin]# systemctl restart firewalld
```
访问linux服务器的8983端口~
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ee787676234400f812dea3814d043d0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)左侧有5个菜单。分别是：

	（1）Dashboard：面板显示Solr的总体信息。
	
	（2）Logging：日志
	
	（3）Core Admin：Solr的核心。类似于数据的Database
	
	（4）Java Perperties：所有Java相关属性。
	
	（5）Thread Dump：线程相关信息。
	
	（6）如果有Core，将显示在此处。


solr是用java编写的,在启动时可以[配置一下启动参数~](https://solr.apache.org/guide/8_11/solr-control-script-reference.html)

 -**7. 查看solr启动状态**

```c
[root@localhost bin]# ./solr status

Found 1 Solr nodes: 

Solr process 16360 running on port 8983
{
  "solr_home":"/usr/local/solr-8.11.1/server/solr",
  "version":"8.11.1 0b002b11819df70783e83ef36b42ed1223c14b50 - janhoy - 2021-12-14 13:50:55",
  "startTime":"2022-03-03T09:46:18.877Z",
  "uptime":"0 days, 0 hours, 10 minutes, 30 seconds",
  "memory":"106.9 MB (%20.9) of 512 MB"}


```

### solr配置示例
在启动时可以按照实例配置运行solr,官方提供了几个标准示例
可以运行的可用示例有：techproducts、dih、schemaless 和 cloud。
```c
./solr start -e <name>
```
cloud ：本示例在单台机器上启动一个 1-4 节点的 SolrCloud 集群。 选择后，交互式会话将开始引导您选择要使用的初始配置集、示例集群的节点数、要使用的端口以及要创建的集合的名称。 

>在 $SOLR_HOME/server/solr/configsets/下取配置一些参数

techproducts ：此示例以独立模式启动 Solr，并使用为包含在 $SOLR_HOME/example/exampledocs目录。

>使用的配置集可以在 $SOLR_HOME/server/solr/configsets/sample_techproducts_configs. 

dih ：此示例在启用 DataImportHandler (DIH) 和几个示例的情况下以独立模式启动 Solr dataconfig.xml为 DIH 支持的不同类型数据（例如数据库内容、电子邮件、RSS 提要等）预配置的文件。

>使用的配置集是为 DIH 定制的，位于 $SOLR_HOME/example/example-DIH/solr/conf. 

schemaless ：此示例使用托管模式以独立模式启动 Solr，如 SolrConfig 中的模式工厂定义 ，并提供非常最小的预定义模式。 Solr 将 无模式模式下 ，其中 Solr 将在模式中动态创建字段，并猜测传入文档中使用的字段类型。



>使用的配置集可以在 $SOLR_HOME/server/solr/configsets/_default. 

[其他配置示例~](https://solr.apache.org/guide/8_11/solr-control-script-reference.html#stop)


### 创建核心

按照给定的模板进行创建核心--这里以cloud为模板:

  1. 首先进入 solr的安装文件夹~`cd /usr/local/solr-8.11.1/`
  2. 创建核心~  bin/solr create -e cloud
  3. 模板配置如下

```c
#引导完成设置带有嵌入式 ZooKeeper 的简单 SolrCloud 集群的步骤
To begin, how many Solr nodes would you like to run in your local cluster? (specify 1-4 nodes) [2]: 
2 #默认值为2,该脚本最多支持启动 4 个节点
```
 这些节点将分别存在于一台机器上，但将使用不同的端口来模拟不同服务器上的操作。 

  4. 节点绑定到端口

```c
Ok, let's start up 2 Solr nodes for your example SolrCloud cluster.
Please enter the port for node1 [8983]: 
8983
Please enter the port for node2 [7574]: 
7574

```

 >该脚本将按顺序启动每个节点并向您显示它用于启动服务器的命令
 >第一个节点还将启动嵌入式zookeeper服务器(zookeeper绑定的端口号9983)

第一个节点的 Solr 主目录位于 example/cloud/node1/solr


启动集群中所有节点,脚本会提示您输入要创建的集合的名称： 

```c
 Please provide a name for your new collection: [gettingstarted]
```
建议的默认值为“gettingstarted”，脚本将提示您输入集合的配置目录的名称。 您可以选择 _default 或 sample_techproducts_configs 。 配置目录是从 server/solr/configsets/

[SolrCloud 参考配置](https://solr.apache.org/guide/8_11/getting-started-with-solrcloud.html)


### 创建自定义核心

如果没有使用示例配置启动 Solr，则需要创建一个核心才能进行索引和搜索。 您可以通过运行：

>bin/solr create -c <name>

这将创建一个使用数据驱动模式的核心，该模式在您将文档添加到索引时尝试猜测正确的字段类型。 

solr在安装完之后默认是没有创建核心的,要手动配置,或者启动该的时候按照给定的模板进行配置

  1. **在/usr/local/solr-8.11.1/server/solr 中新建文件夹  new_core**

```c
[root@localhost server]#  cd /usr/local/solr-8.11.1/server/solr
```

  2. **复制solr配置文件到核心文件夹new_core**
    将配置文件复制到自定义核心的文件夹new_core
```c
[root@localhost solr]# cp -r configsets/_default/conf/ new_core/
[root@localhost solr]# 


```

  3. **配置core信息**
    在可视化界面创建核心~

创建核心之前检查一下   
1,solr实例核心文件夹是否存在,并且在指定位置
2,data文件夹是否存在,如果不存在则要建立文件夹  `mkdir /usr/local/solr-8.11.1/server/solr/new_core`

然后添加核心
![在这里插入图片描述](https://img-blog.csdnimg.cn/006fce792b964dcb98a05d2f4d8a39ab.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
添加成功后的信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/5488283b84624d6ba155d733f17ca0f9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
之后可以在可视化界面操作了
![在这里插入图片描述](https://img-blog.csdnimg.cn/b64a097c97994b5ab4e8d0248867d0f7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### 分词的实现
**英文分词**
![在这里插入图片描述](https://img-blog.csdnimg.cn/cfb292d1c89d4247a9b41c83e6669280.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)可以看到英文分词是按照单词进行的~单词之间有空格哦!

**中文分词**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f2428f40a7484c5092a50a4301338e54.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
中文按照一个一个子蹦可就不得劲了,应该按照合理的词组进行拆分;
这里需要一个[分词插件~](https://mvnrepository.com/artifact/com.github.magese/ik-analyzer/8.5.0)

![在这里插入图片描述](https://img-blog.csdnimg.cn/e5bf48d3a0c548e9be63cebbe92ea425.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)将该jar包上传到下面的文件夹~
>/usr/local/solr/server/solr-webapp/webapp/WEB-INF/lib

修改配置文件


```c
vi /usr/local/solr-8.11.1/server/solr/new_core/conf/managed-schema
```

添加下面的内容

```xml
<field name="myfield" type="text_ik" indexed="true" stored="true" />
    <fieldType name="text_ik" class="solr.TextField">
            <analyzer type="index">
                    <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="false" conf="ik.conf"/>
                    <filter class="solr.LowerCaseFilterFactory"/>
            </analyzer>
            <analyzer type="query">
                    <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="true" conf="ik.conf"/>
                    <filter class="solr.LowerCaseFilterFactory"/>
            </analyzer>
    </fieldType>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f6c323bcce5f4b1cb01797dc9ae61250.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
配置之后要重启solr

```c
[root@localhost solr-8.11.1]# bin/solr stop -all
Sending stop command to Solr running on port 8983 ... waiting up to 180 seconds to allow Jetty process 18180 to stop gracefully.
[root@localhost solr-8.11.1]# bin/solr start -force
Waiting up to 180 seconds to see Solr running on port 8983 [-]  
Started Solr server on port 8983 (pid=21976). Happy searching!
                                                              
```
这时候可以找到myfiled属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/ea12b7b4c0d0430c85ae029df7f9d2b4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

再次分析一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/3f041bde440f4cceb27e990989cd5565.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### managed-schema的配置详解
**< fieldType/>**
>定义一个属性类型,在Solr中属性类型都是自定义的。在上面配置中name=”text_ik”为自定义类型。当某个属性取值为text_ik时IK Analyzer才能生效。



**< field/>**

>表示向Document中添加一个属性。

	常用属性：
	
		name: 属性名
	
		type:属性类型。所有类型都是solr使用<fieldType>配置的
	
		indexed: 是否建立索引
	
		stored: solr是否把该属性值响应给搜索用户。
	
		required：该属性是否是必须的。默认id是必须的。
	
		multiValued：如果为true，表示该属性为复合属性，此属性中包含了多个其他的属性。常用在多个列作为搜索条件时，把这些列定义定义成一个新的复合属性，通过搜索一个复合属性就可以实现搜索多个列。当设置为true时与< copyField source="" dest=""/>结合使用

stored属性用于设置是否把内容响应给搜索用户
搜索手机关键字时有的内容没有手机二字但是也显示出来了,这时候是因为stored设置为true

>solr存储的数据不仅仅只有索引数据,还可以存储其他用于展示的数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/248c141e6b0144d68d9802374d627f84.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

**< uniqueKey>**

	唯一主键，Solr中默认定义id属性为唯一主键。ID的值是不允许重复的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/ff145f32ea3b4731bbd240a8dbf25edd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)当索引内容重复时会报错;


**< dynamicField>**
动态属性
	名称中允许*进行通配。代表满足特定名称要求的一组属性。
 	 solr_*


原始配置如下~

```xml
    <field name="id" type="string" indexed="true" stored="true" required="true" multiValued="false" />
    <!-- docValues are enabled by default for long type so we don't need to index the version field  -->
    <field name="_version_" type="plong" indexed="false" stored="false"/>

    <!-- If you don't use child/nested documents, then you should remove the next two fields:  -->
    <!-- for nested documents (minimal; points to root document) -->
    <field name="_root_" type="string" indexed="true" stored="false" docValues="false" />
    <!-- for nested documents (relationship tracking) -->
    <field name="_nest_path_" type="_nest_path_" /><fieldType name="_nest_path_" class="solr.NestPathField" />

    <field name="_text_" type="text_general" indexed="true" stored="false" multiValued="true"/>
    <field name="myfield" type="text_ik" indexed="true" stored="true"/>
<fieldType name="text_ik" class="solr.TextField">
<analyzer type="index">
    <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="false" conf="ik.conf"/>
    <filter class="solr.LowerCaseFilterFactory"/>
</analyzer>
<analyzer type="query">
    <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="true" conf="ik.conf"/>
    <filter class="solr.LowerCaseFilterFactory"/>
</analyzer>
</fieldType>

```

## solr的常用命令

 - 启动和停止

```bash
bin/solr start [options] 

bin/solr start -help

bin/solr restart [options]

bin/solr restart -help
```
>使用启动/停止命令时，必须传递启动 Solr 时最初传递的所有参数。在后台，会启动停止请求，因此 Solr 将在再次启动之前停止。如果没有节点正在运行，重新启动将跳过停止的步骤，并继续启动Solr

 - 启动参数

-a "参数"

>可以启动时设置jvm的参数
```bash
bin/solr start -a "-Xdebug -
Xrunjdwp:transport=dt_socket, 
server=y,suspend=n,address=1044"

```

-c 
>即在SolrCloud模式下启动Solr，这也将启动Solr附带的嵌入式ZooKeeper实例。是bin/solr start -cloud的缩写


-d <目录>

>定义服务器目录，缺省为server 。覆盖此选项的情况并不常见。在同一主机上运行多个 Solr 实例时，更常见的情况是为每个实例使用相同的服务器目录$SOLR_HOME/server，并使用 -s 选项使用唯一的 Solr 主目录。


示例：`bin/solr start -d newServerDir`
默认是在server中启动
![在这里插入图片描述](https://img-blog.csdnimg.cn/5016b61e57784cc1bbccda143e441ea6.png)

-e  示例名
>使用示例配置启动 Solr。

 - 官方给的示例
 - [ ] **cloud**
 - [ ] **techproducts**
 - [ ] **dih**
 - [ ] **schemaless**


-h <主机名>
>使用定义的主机名启动 Solr。如果未指定，则假定为"本地主机"。

示例：`bin/solr start -h search.mysolr.com`

-m <内存大小>
>使用定义的值作为 JVM 的最小 （-Xms） 和最大 （-Xmx） 堆大小来启动 Solr。

示例：`bin/solr start -m 1g`

-p <port>

>在定义的端口上启动 Solr。如果未指定，则将使用"8983"。

示例：`bin/solr start -p 8983`

-v
>log4j 的日志记录级别DEBUG

示例：`bin/solr start -f -v`

-q
> log4j 的日志记录级别WARN

示例：`bin/solr start -f -q`

-V
>log4j 的日志记录级别DEBUG。

示例：bin/solr start -V

**-z <zookeeper端口号>**
使用定义的 ZooKeeper  ZK_HOST连接字符串启动 Solr。此选项仅与 -c 选项一起使用，以在 SolrCloud 模式下启动 Solr。如果未在 solr.in.cmd/ solr.in.sh中指定此选项，并且未提供此选项，则 Solr 将启动嵌入式 ZooKeeper 实例，并将该实例用于 SolrCloud 操作。
示例：`bin/solr start -c -z server1:2181,server2:2181`
![在这里插入图片描述](https://img-blog.csdnimg.cn/99cd5fe1317d467d9f859e7c13d79191.png)


启动示例~
bin/solr start -h localhost -p 8983 -d server -s solr -m 512m
解析~

>使用主机名为localhost 端口8983 服务器根目录 server  ,核心目录 solr jvm内存512M

 - 停止参数

-p <端口号>
>停止在给定端口上运行的 Solr。

示例：`bin/solr stop -p 8983`

-all
>停止所有正在运行的具有有效 PID 的 Solr 实例。

示例：`bin/solr stop -all`

-k <键值>
>用于防止无意中停止Solr的停止键;默认值为"solrrocks"。

示例：`bin/solr stop -k solrrocks`

## 数据的导入
>solr主要用于索引,手动导入数据比较麻烦,有一个自动导入的方式~
### 数据库数据导入插件

使用solr自带的Dataimport功能将数据库数据快速导入到solr中;
修改~`managed-schema` 用以给数据库数据配置是否需要索引,展示什么的;
这里以emp表为例(这里表名改为了emps)
**修改配置文件**
```c
[root@localhost conf]# vi managed-schema 
[root@localhost conf]# 
```

 - 1,将需要添加索引的字段配置在managed-schema 中'

> type---索引类型 
> indexed--是否启用索引
> stored---是否是展示数据 
> required-- 该属性是必须的
> multiValued---该属性是否是复合属性
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/eb9c38e4a934444a8bf13aa9759d339a.png)

 - 2,在solorconfig中添加数据处理器配置

```c
vi /usr/local/solr-8.11.1/server/solr/new_core/conf/solrconfig.xml
```
添加如下内容~用于配置数据导入的处理器

```xml
 <!-- 配置数据导入的处理器 -->
  <requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">
    <lst name="defaults">
	  <!--  加载data-config.xml  -->
      <str name="config">data-config.xml</str>
     </lst>
  </requestHandler>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5cb866a9daa43268ef543513fd70fa3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - 4.添加数据库配置

```c
cd /usr/local/solr-8.11.1/server/solr/new_core/conf
touch  data-config.xml
```
>新建data-config.xml文件,用于配置数据库

![在这里插入图片描述](https://img-blog.csdnimg.cn/b2f2dac54908410ab024777e5ade97cf.png)

配置内容如下~
```xml
<?xml version="1.0" encoding="UTF-8"?>
<dataConfig>
        <dataSource type="JdbcDataSource"   
                driver="com.mysql.jdbc.Driver"   
                url="jdbc:mysql://192.168.1.6:3306/gavin"   
                user="gavin"   
                password="955945"/>
        <document>
        <!--实体名-->
            <entity name="emps" query="SELECT empno,ename,job,mgr,sal,comm,deptno from emps">
                <!-- 
                 实现数据库的列和索引库的字段的映射
                 column 指定数据库的列表
                 name  指定索引库的字段名字，必须和schema.xml中定义的一样
                 -->
                 <field column="empno" name="empno"/>
                 <field column="ename" name="ename"/>
				         <field column="job" name="job"/>
				         <field column="mgr" name="mgr"/>
                 <field column="commm" name="comm"/>
				 <field column="deptno" name="deptno"/>
            </entity>
         </document>
</dataConfig>
```
配置过后要加入jar包去完成数据导入到solr
在dist文件夹下---->>
solr-dataimporthandler-8.11.1.jar         
solr-dataimporthandler-extras-8.11.1.jar 

```c
[root@localhost dist]# cd /usr/local/solr-8.11.1/dist/
[root@localhost dist]# ls
solr-analysis-extras-8.11.1.jar            solr-langid-8.11.1.jar
solr-analytics-8.11.1.jar                  solr-ltr-8.11.1.jar
solr-cell-8.11.1.jar                       solr-prometheus-exporter-8.11.1.jar
solr-core-8.11.1.jar                       solr-s3-repository-8.11.1.jar
solr-dataimporthandler-8.11.1.jar          solr-solrj-8.11.1.jar
solr-dataimporthandler-extras-8.11.1.jar   solr-test-framework-8.11.1.jar
solr-gcs-repository-8.11.1.jar             solr-velocity-8.11.1.jar
solr-jaegertracer-configurator-8.11.1.jar  test-framework
solrj-lib
```
>将solr-dataimporthandler-8.11.1.jar    和   solr-dataimporthandler-extras-8.11.1.jar复制到
>/usr/local/solr-8.11.1/server/solr-webapp/webapp/WEB-INF/lib 下

```c
[root@localhost dist]# cp solr-dataimporthandler-8.11.1.jar  /usr/local/solr-8.11.1/server/solr-webapp/webapp/WEB-INF/lib
[root@localhost dist]# cp solr-dataimporthandler-extras-8.11.1.jar  /usr/local/solr-8.11.1/server/solr-webapp/webapp/WEB-INF/lib
[root@localhost dist]# 
```
还有数据库驱动也需要放在这里


重启solr

```c
[root@localhost solr-8.11.1]# bin/solr stop -all  #停止后重启
Sending stop command to Solr running on port 8983 ... waiting up to 180 seconds to allow Jetty process 21976 to stop gracefully.
[root@localhost solr-8.11.1]# /bin/solr start -force

```
配置成功界面~
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c44fc01854d4d17bc3e54b10b954046.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
在最新的8.11.1版本的使用数据导入处理程序上传结构化数据存储已被弃用
![在这里插入图片描述](https://img-blog.csdnimg.cn/69a0be8a3d284966858c7fa92199d79a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### Post命令导入数据

导入数据的其他方式~使用 post命令

在导入数据之前先要建立数据集

>此工具旨在供探索 Solr 功能的新用户使用，而不能用作将文档索引到生产系统中的强大解决方案。

>bin/post -c gettingstarted example/films/films.json


这适用于不用给文档/集合添加索引的情形,如果要索引还是要配置相应的managed-schema 和data-config.xml


将文件扩展名为 的所有文档添加到在端口 上运行的 Solr 上的集合/核心

```bash
bin/post -c emps -p 8984 *.xml
```
发送 XML 参数以从 emps 中删除文档。

```bash
bin/post -c emps -d '<delete><id>42</id></delete>'
```

>**注意~在managed-schema中的id字段要与_root_一致**


## AdminUI中的Documents
对文档内容进行新增和修改

 - **XML 格式索引更新**

索引更新命令可以作为 XML 消息发送到更新处理程序，方法是 使用 或 。Content-type: application/xmlContent-type: text/xml

**添加文档**

用于添加文档的更新处理程序识别的 XML 架构非常简单：

该元素引入了另一个要添加的文档。<add>

该元素介绍构成文档的字段。<doc>

该元素显示特定字段的内容。<field>
![在这里插入图片描述](https://img-blog.csdnimg.cn/e541950005dc4ccaa5ead0ea570a5a86.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - [ ] <add>

如果存在则修改,不存在则新增
新增/修改

```xml
 <add><doc>
<field name="gid">88</field>
<field name="name">铁锅炖大鹅</field>
<field name="price">98</field>
<field name="desc">癞蛤蟆想吃</field>
</doc>
</add>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/de0ce591317f416bb3b1c0352a8d8b60.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - [ ] 删除

根据主键删除

```xml
<delete>
<id>b896d180-e4de-483a-aa03-63b71ea3c796</id>
</delete>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/63e0a9078bdd45efb4eb594c2edd217e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



根据条件删除
再次新增铁锅炖大鹅,然后删除
```xml
<delete>
	<query>name:铁锅炖大鹅</query>
</delete>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa4f602bb29a402fbd5c90597f4ce368.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - [ ] 分组操作

通过将命令与周围的元素分组，可以在单个 XML 文件中发布多个命令。<update>

```xml
<update>
  <add>
    <doc><!-- doc 1 content --></doc>
  </add>
  <add>
    <doc><!-- doc 2 content --></doc>
  </add>
  <delete>
    <id>0002166313</id>
  </delete>
</update>
```

 - [ ] 使用 curl 执行更新

>可以使用curl执行上述任何命令，使用其选项将 XML 消息追加到命令中，并生成 HTTP POST 请求。

例如：curl--data-binarycurl

```html
curl http://localhost:8983/solr/my_collection/update -H "Content-Type: text/xml" --data-binary '
<add>
  <doc>
    <field name="authors">Patrick Eagar</field>
    <field name="subject">Sports</field>
    <field name="dd">796.35</field>
    <field name="isbn">0002166313</field>
    <field name="yearpub">1982</field>
    <field name="publisher">Collins</field>
  </doc>
</add>'
```

也可以将document命令放在一个文件里

```html
curl http://localhost:8983/solr/my_collection/update -H "Content-Type: text/xml" --data-binary @myfile.xml
```
[JSON 格式索引更新](https://solr.apache.org/guide/8_11/uploading-data-with-index-handlers.html#json-formatted-index-updates)
[CSV 格式的索引更新](https://solr.apache.org/guide/8_11/uploading-data-with-index-handlers.html#csv-formatted-index-updates)

## AdminUI中的Query

![在这里插入图片描述](https://img-blog.csdnimg.cn/6875b2f78d5c47349a311fc8403f5c20.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

高亮的具体应用
打开狗东,搜索笔记本电脑,你会发现笔记本电脑有关的字段都是红色的;
![在这里插入图片描述](https://img-blog.csdnimg.cn/695e5eb88e134653b10103df9210ceb8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
使用solr客户端搜索野生黑木耳,关于野生黑木耳的都被高亮了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/2fae9df3068c452780b37003dbaf72b8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

## 使用solr客户端操作solr

首先导入依赖

```xml
<dependency>
    <groupId>org.apache.solr</groupId>
    <artifactId>solr-solrj</artifactId>
    <version>8.11.1</version>
</dependency>

```

 - **新增与修改**

编写代码~
```java
package com.gavin.Client;
import org.apache.solr.client.solrj.SolrServerException;
import org.apache.solr.client.solrj.impl.HttpSolrClient;
import org.apache.solr.common.SolrInputDocument;
import org.junit.jupiter.api.Test;

import java.io.IOException;
public class SolrClient {
    @Test
    void Test1() throws IOException, SolrServerException {
        //solr地址
        String url = "http://192.168.135.145:8983/solr/goods_shard1_replica_n1/";
//        建链接
        HttpSolrClient solrClient =  new HttpSolrClient.Builder(url).build();
//        实例化输入流
        SolrInputDocument inputDocument = new SolrInputDocument();
//        添加域/值
        inputDocument.addField("gid","3");
        inputDocument.addField("id","eed6ab26-018c-4fe5-bfd4-34b4de71c7f5");
        inputDocument.addField("name","黄花菜");
        inputDocument.addField("price","10");
        inputDocument.addField("desc","黄花");

        solrClient.add(inputDocument);
        solrClient.commit();
    }
}
```
当id相同时则修改,否则新增

![在这里插入图片描述](https://img-blog.csdnimg.cn/4a1f39a533e3480a852d1843fd3caea1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
 **add的重载方法~的使用**

```java

    private String url= "http://192.168.135.145:8983/solr/";
    @Test
    void add(){

        HttpSolrClient httpSolrClient = new HttpSolrClient.Builder(url).build();

        SolrInputDocument solrInputFields = new SolrInputDocument();
        solrInputFields.addField("gid",101);
        solrInputFields.addField("name","麻婆煮豆腐");
        solrInputFields.addField("price",26);
        solrInputFields.addField("desc","麻婆豆腐");

        try {
            UpdateResponse goods = httpSolrClient.add("dept", solrInputFields);
            UpdateResponse goods_shard1_replica_n1 = httpSolrClient.commit("goods_shard1_replica_n1");
            httpSolrClient.close();
        } catch (SolrServerException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
有时候我们在一个solr中有很多集合索引,这个时候我们定义地址时就不方便携带上集合的名字;

比如在一个和心中有多个集合--->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a954867e3fe409cad9c1db4e2f1cafa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_11,color_FFFFFF,t_70,g_se,x_16)

现在想要往dept中添加信息~
![在这里插入图片描述](https://img-blog.csdnimg.cn/93335015f2c34e9cbfa1c8ef84377710.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
    private String url= "http://192.168.135.145:8983/solr/";
    @Test
    void add(){

        HttpSolrClient httpSolrClient = new HttpSolrClient.Builder(url).build();

        SolrInputDocument solrInputFields = new SolrInputDocument();
        solrInputFields.addField("gid",102);
        solrInputFields.addField("name","麻婆煮豆腐1");
        solrInputFields.addField("price",261);
        solrInputFields.addField("desc","麻婆豆腐1");

        try {
            UpdateResponse goods = httpSolrClient.add("dept", solrInputFields);
            UpdateResponse goods_shard1_replica_n1 = httpSolrClient.commit("dept");
            httpSolrClient.close();
        } catch (SolrServerException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }


```
![在这里插入图片描述](https://img-blog.csdnimg.cn/76255795245b4a3480c4f24681da9fb9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


 - **删除操作**

```java
@Test
    void TestDelete() throws IOException, SolrServerException {
        String url = "http://192.168.135.145:8983/solr/goods_shard1_replica_n1/";
        //建立连接
        HttpSolrClient build = new HttpSolrClient.Builder(url).build();
        UpdateResponse updateResponse = build.deleteById("47915063-6342-45e1-81c0-582e18f1a5d9");
        build.commit();//如果没有提交,则不会删除
        System.out.println(updateResponse.getRequestUrl());
        build.close();
    }

```
黄花菜被删除
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ed5375f8bb946b4a125e02e1efcc2bd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
当核心中不止一个集合,则提交的时候需要指明提交到那个集合,不然会报错`Expected mime type application/octet-stream but got text/html. <html>`

```java
@Test
    void delete() throws IOException, SolrServerException {
    HttpSolrClient build = new HttpSolrClient.Builder(url).build();
    System.out.println(build.getBaseURL());
    build.deleteById("goods","6e9f3376-f263-437e-9db1-cf1d55440f37");
    build.commit("goods");
    build.close();

}
```

 - **查询操作**

```java

    @Test
    void TestQuery() throws IOException, SolrServerException {
        System.out.println("建立连接");
        HttpSolrClient httpSolrClient = new HttpSolrClient.Builder(url).build();

        System.out.println("实例化一个solr查询实例");
        SolrQuery solrQuery = new SolrQuery();
        System.out.println("先设置查询条件");
//        先设置查询条件
        SolrQuery query1 = solrQuery.setQuery("name:*鱼");
        //排序
        solrQuery.setSort("price", SolrQuery.ORDER.desc);
        //分页
        solrQuery.setStart(0);
        solrQuery.setRows(5);
        //设置高亮
        solrQuery.setHighlight(true);
        solrQuery.addHighlightField("name");
        solrQuery.setHighlightSimplePre("<font color='red'>");
        solrQuery.setHighlightSimplePost("</font>");


        System.out.println("开始查询");
        QueryResponse query2 = httpSolrClient.query(solrQuery);
        //查询到的记录
        SolrDocumentList results1 = query2.getResults();
//        从记录总可以取出数据
        System.out.println("查询到的数据有"+results1.toString());
        results1.forEach(System.out::println);
        System.out.println("查询到--" + results1.getNumFound() + "条记录");
//        遍历数据,以便使得高亮设置生效
        //高亮数据,返回map
        Map<String, Map<String, List<String>>> highlighting = query2.getHighlighting();

        for (SolrDocument doc : results1) {
            System.out.println(doc.get("id"));
            Map<String, List<String>> high = highlighting.get(doc.get("id"));
            List<String> name1 = high.get("name");
            if (name1!=null&&name1.size()>0){
                System.out.println(name1.get(0));
            }else{
                System.out.println(doc.get("name"));
            }
            System.out.println(doc.get("price"));
            System.out.println(doc.get("desc"));

        }

    }

```

查询结果~

![在这里插入图片描述](https://img-blog.csdnimg.cn/9be56bc6f7b54dbe82513710f5d5725f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

大体逻辑就是通过solrquery来封装查询条件,然后获得的结果将需要的字段取出

[**solr-solrj8的Api**](https://solr.apache.org/docs/8_1_0/solr-solrj/allclasses.html)

## Spring-Solr启动器
导入依赖~

```xml
  <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-solr</artifactId>
        <version>2.4.13</version>
    </dependency>
```
配置文件~

```yml
spring:
  data:
    solr:
      host: http://192.168.134.145:8983/solr
      zk-host: 192.168.135.145:2181,192.168.135.146:2181,192.168.135,147:2181
```
编写代码获取数据

>这里由spring去管理solr客户端,这样就不用每次都去手动创建solr客户端了;

**增加/修改1**
```java
 @Autowired
    private SolrTemplate solrTemplate;//运行时会产生一个template对象并注入,编译时不会

    @Override
    public String addDoc() {
        SolrInputDocument solrInputFields = new SolrInputDocument();
        solrInputFields.addField("id", 10086);
        solrInputFields.addField("gid", 1);
        solrInputFields.addField("name", "麻辣鸡丝");
        solrInputFields.addField("price", 100);
        solrInputFields.addField("desc", "麻辣");
        UpdateResponse goods = solrTemplate.saveDocument("goods", solrInputFields);
        int status = goods.getStatus();
        if (status == 0) {

            solrTemplate.commit("goods");
            return "success";
        }
//        solrTemplate.saveDocument()
        solrTemplate.rollback("goods");
        return "failed";

    }
```

配置文件

```yml
spring:
  data:
    solr:
      host: http://192.168.135.145:8983/solr
```
运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/5f6b082e649b46ad895a95b5ab731c11.png)
>这里是通过solrTemplate来提交的;
>其他操作跟之前的基本一致

**增加/修改2**
将对象整个放进去
```java
    @Override
    public String addBean() {
        goods goods=new goods(66,"紫菜蛋花汤",12,"热乎乎");
        UpdateResponse goods1 = solrTemplate.saveBean("goods", goods);
        if (goods1.getStatus()==0) {
            solrTemplate.commit("goods");
            return "success";
        }
        solrTemplate.rollback("goods");
        return "failed";
    }

```
>如果是将对象放进去,那么就需要在实体类中的属性上添加注解@field,否则不会讲对象添加到集合

![在这里插入图片描述](https://img-blog.csdnimg.cn/4b41df433a864b5c812441e7c46065cf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

结果~
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd74d26e80a340b6872a03c6819389b2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**同理就会有 saveDocuments与SaveBeans**

```
在这里插入代码片
```

 saveDocuments底层源码与SaveBeans底层源码
```java
@Override
public UpdateResponse saveBeans(String collection, Collection<?> beans, Duration commitWithin) {
	return execute(solrClient -> solrClient.add(collection, convertBeansToSolrInputDocuments(beans),
			getCommitWithinTimeout(commitWithin)));
}
@Override
	public UpdateResponse saveDocuments(String collection, Collection<SolrInputDocument> documents,
			Duration commitWithin) {
		return execute(solrClient -> solrClient.add(collection, documents, getCommitWithinTimeout(commitWithin)));
	}


```

**新增/修改3**
>根据唯一ID进行判断,重复则修改,没有则新增

部分更新~跟新增/修改一样的代码~

```java
    @Override
    public Integer updateGoods() {
        SolrInputDocument solrInputFields = new SolrInputDocument();
        solrInputFields.addField("id", 10086);
        solrInputFields.addField("gid", 9999);
        solrInputFields.addField("name", "麻辣小龙虾");
        solrInputFields.addField("price", 100);
        solrInputFields.addField("desc", "麻辣");

        UpdateResponse goods = solrTemplate.saveDocument("goods", solrInputFields);
        int status = goods.getStatus();
        if (status == 0) {

            solrTemplate.commit("goods");
            return 1;
        }
//        solrTemplate.saveDocument()
        solrTemplate.rollback("goods");
        return -1;


    }
```
删除就很简单了;
**删除**

```java
    @Override
 @Override
    public Integer deleteGoodsById(String id) {
        UpdateResponse goods = solrTemplate.deleteByIds("goods", id);
        solrTemplate.commit("goods");
        return goods.getStatus();
    }
```

**查询**

```java
    @Override
    public List<Goods> QuerySolr() {
//        Criteria就类似于 strinbuilder,可以append继续添加条件
        SimpleQuery simpleQuery = new SimpleQuery();
//        添加查询条件
        Criteria criteria = new Criteria("name");
        criteria.endsWith("汤");
        simpleQuery.addCriteria(criteria);
        simpleQuery.setOffset(1L);
        simpleQuery.setRows(5);
     /*   criteria.and("price");
        criteria.greaterThanEqual(10);*/

        ScoredPage<Goods> goods1 = solrTemplate.queryForPage("goods", simpleQuery, Goods.class);
        List<Goods> content1 = goods1.getContent();
        /*
//       Query query=new Query();
        *//**
         * query()参数
         * String collection, Query query, Class<T> clazz
         * query是一个接口,需要找实现类
         * SimpleQuery
         *//*
//默认一页显示10条
        Page<Goods> goods = solrTemplate.query("goods", simpleQuery, Goods.class);
        solrTemplate.commit("goods");
        if (goods.getTotalPages() == 0) {
            return null;
        }
        System.out.println("查询到" + goods.getTotalElements() + "条记录");

        List<Goods> content = goods.getContent();
//获取高亮数据*/
        return content1;
    }

```
**高亮显示**

```java

    @Override
    public List<Goods> HighLightDoods() {

        //首先是查询
        SimpleHighlightQuery simpleHighlightQuery = new SimpleHighlightQuery();
        //查询的字段
        Criteria criteria = new Criteria("desc");
        criteria.contains("辣");
        criteria.and("price");
        criteria.between(10, 200);
        //封装查询条件
        simpleHighlightQuery.addCriteria(criteria);

        //手动分页
        simpleHighlightQuery.setOffset(0L);
        simpleHighlightQuery.setRows(5);

//        排序
        //        
//        Sort sort= new Sort(Sort.Direction.DESC,"price");//private 访问权限,需要静态方法返回一个Sort实例

        Sort price = Sort.by(Sort.Direction.ASC, "price");
        simpleHighlightQuery.addSort(price);
//设置高亮
        HighlightOptions highlightOptions = new HighlightOptions();

        highlightOptions.addField("desc");

        highlightOptions.setSimplePrefix("<span color='red'>");

        highlightOptions.setSimplePostfix("</span>");
        //将高亮放入条件
        SolrDataQuery solrDataQuery = simpleHighlightQuery.setHighlightOptions(highlightOptions);

        HighlightPage<Goods> goods = solrTemplate.queryForHighlightPage("goods", simpleHighlightQuery, Goods.class);
        solrTemplate.commit("goods");
//        取出高亮部分
        List<HighlightEntry<Goods>> highlighted = goods.getHighlighted();

        List<Goods> list = new ArrayList<>();
//遍历
        for (HighlightEntry<Goods> highlightEntry : highlighted) {
//            得到highlight部分
            List<HighlightEntry.Highlight> highlights = highlightEntry.getHighlights();

            Goods entity = highlightEntry.getEntity();
//            高亮部分不止一个,再次遍历
            for (HighlightEntry.Highlight highlight : highlights) {
                //在字段desc上添加高亮
                if (highlight.getField().getName().equals("desc")) {
//                    如果满足则在字段前添加高亮
                    entity.setDesc(highlight.getSnipplets().get(0));

//                    将高亮字段找个集合装起来
                    list.add(entity);
                }
            }

        }
        return list;

    }
```
结果~
![在这里插入图片描述](https://img-blog.csdnimg.cn/577289fa052c4c9d8e81a6e4cc81baa4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>高亮显示的思路是~在查询结果的基础上取数高亮部分,将通过高亮条件触发高亮前缀和后缀;



使用zookeeper搭建一个dolr集群
![在这里插入图片描述](https://img-blog.csdnimg.cn/1827cbb0d2c840b184bd21890ed431a1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)





[具体操作~Solrcloud模式](https://blog.csdn.net/weixin_54061333/article/details/123291110)

## Solr~cloud模式

>将solr作为双节点集群(伪集群)启动;



### 以Solr~cloud模式启动solr


 - 1,进入solr的安装目录下~`cd /usr/local/solr-8.11.1/`
 - 2,启动命令: `./bin/solr start -e cloud`
 - 3,按照提示进行相应配置

>询问我们要运行多少个节点。请注意最后一行末尾的;默认的节点数2

![在这里插入图片描述](https://img-blog.csdnimg.cn/8f873a330c2e4d80a197b1c6d79901da.png)

>第一个节点默认设置端口为~8983,
>第二个节点将在其上运行的端口~7574

![在这里插入图片描述](https://img-blog.csdnimg.cn/a36b623133944eb3ba31a0e3978ddc1e.png)
设置好参数之后,会启动两个端口
![在这里插入图片描述](https://img-blog.csdnimg.cn/74d348b4192747c194109c208c46c536.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

>Solr 的两个实例已在两个节点上启动。由于我们是在SolrCloud模式下启动的，并且没有定义有关外部ZooKeeper集群的任何细节，因此Solr启动了自己的ZooKeeper并将两个节点连接到它。

 - 4,创建集合名字

>系统提示创建用于索引数据的集合,添加集合名字,名字可以随便起,用于区分其他集合;

![在这里插入图片描述](https://img-blog.csdnimg.cn/557b2f0dbf624ac79ed400f73f907141.png)

 - 5,索引分片

>将索引拆分成多少个分片,选择"2"（默认值）意味着我们将在两个节点之间相对均匀地拆分索引;
>
>![在这里插入图片描述](https://img-blog.csdnimg.cn/25735eabe2b7420097f63a3c9503cb5e.png)

 - 6,开启索引副本

>用于故障时索引的转移,默认也是2

![在这里插入图片描述](https://img-blog.csdnimg.cn/3addefed27c14d518b5de1f641e5a397.png)

 - 7,配置文件的选择

集合必须具有一个配置文件集
![在这里插入图片描述](https://img-blog.csdnimg.cn/de982d25cc704af990e5d43a985c8921.png)

>Solr现在将运行两个"节点"，一个在端口7574上，一个在端口8983上。有一个自动创建的集合，一个双分片集合，每个集合有两个副本。

![在这里插入图片描述](https://img-blog.csdnimg.cn/137b3cd8930346f48b648dd58a955541.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/be8964622db849dc8e09cfbf31851d2a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

此时在solr中还没有索引数据
,我们可以通过post命令来向solr中添加数据

```c
[root@localhost solr-8.11.1]# ls
bin          contrib  docs     licenses     LUCENE_CHANGES.txt  README.txt
CHANGES.txt  dist     example  LICENSE.txt  NOTICE.txt          server
[root@localhost solr-8.11.1]# bin/post -c techproducts example//exampledocs/*
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c815b0d033f4ed78b03396506e1ced1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


基本查询~

![在这里插入图片描述](https://img-blog.csdnimg.cn/03cea7a049704a5e84e3b182a32c9335.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**查询所有数据~**
![在这里插入图片描述](https://img-blog.csdnimg.cn/ecd154e94bc14ad4b10abbd412cc14d8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**查询需要的数据---**
比如查询名字中带有Game的
>name:*Game*,这会将查询限制在name字段
>![在这里插入图片描述](https://img-blog.csdnimg.cn/9ec85e8dd80c4df5a199544e58646755.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

**只返回的匹配的记录**
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ed0d0b71c7b4749991a3a01a7e2fa43.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

**短语搜索**

![在这里插入图片描述](https://img-blog.csdnimg.cn/11e7308f4cb641b98465962ae61452ad.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
[更多搜索形式](https://solr.apache.org/guide/8_11/searching.html)



删除数据集~

```c
bin/solr delete -c  数据集名
```


导入自定义数据~ `bin/post -c emps example/exampledocs/emps.json` 

```c
[root@localhost solr]# bin/post -c emps example/exampledocs/emps.json 
/usr/local/jdk/bin/java -classpath /usr/local/solr/dist/solr-core-8.11.1.jar -Dauto=yes -Dc=emps -Ddata=files org.apache.solr.util.SimplePostTool example/exampledocs/emps.json
SimplePostTool version 5.0.0
Posting files to [base] url http://localhost:8983/solr/emps/update...
Entering auto mode. File endings considered are xml,json,jsonl,csv,pdf,doc,docx,ppt,pptx,xls,xlsx,odt,odp,ods,ott,otp,ots,rtf,htm,html,txt,log
POSTing file emps.json (application/json) to [base]/json/docs
1 files indexed.
COMMITting Solr index changes to http://localhost:8983/solr/emps/update...
Time spent: 0:00:03.706
```
查询信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/241f0fa0015240c9b1ff3450b3a2b4a7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

**solr支持上传文件的格式**
xml,json,jsonl,csv,pdf,doc,docx,ppt,pptx,xls,xlsx,odt,odp,ods,ott,otp,ots,rtf,htm,html,txt,log

>以上是官方提供的一个模式,内嵌的zookeeper不提供任何故障转移,当solr关闭时,zookeeper也会被关闭,一旦关闭热河依赖于solr的分片都将无法被通信;
>
>大部分时间我们需要自定义启动模式;

### 启用外部zookeeper

**启用外部zookeeper**
>要使 ZooKeeper 服务处于活动状态，必须有大多数无故障的计算机可以相互通信。若要创建可以容忍 F 计算机故障的部署，应依靠部署 2xF+1 计算机。
>

**"通常不建议zookeeper超过 5个节点。**

>虽然看起来更多的节点提供了更大的容错和可用性，但在实践中，由于发生的节点间协调量，它的效率会降低。除非您有一个真正庞大的Solr集群（在1，000个节点的规模上），否则一般规则是保持为3，如果您有更大的集群，则可能保持在5。"


配置zookeeper的参数~
>这里用的zookeeper集群
>![在这里插入图片描述](https://img-blog.csdnimg.cn/17504608198a455daec98ec3799e2559.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

**server.id配置~**
之后在dataDir文件夹下创建 myid  文件内容为zookeeper服务器的id值 1

**同理server.2与server.3**

开启zookeeper集群,查看集群状态
![在这里插入图片描述](https://img-blog.csdnimg.cn/fc5e9b06ce4b45f2b26693edfc753461.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

此时可能会遇到Client port found: 2181. Client address: localhost. Client SSL: false.的异常

解决方案~
第一种,将2888端口与3888端口开放

第二种,关闭防火墙

>**在所有的zookeeper服务器上都要操作**

启动zookeeper之后需要启动solr,在启动之前需要配置一下solr启动参数

```bash
[root@localhost bin]# vi solr.in.sh 
```
将zookeeper  IP:端口号整合到solr的启动文件中;
![在这里插入图片描述](https://img-blog.csdnimg.cn/7c4b64f67821487d99f06a59907cbfd4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
开启solr后访问solr管理UI
![在这里插入图片描述](https://img-blog.csdnimg.cn/3eae5cd91dbb407094e75d15c07d4a28.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**!!!新建核心~这个适用于非cloud模式!!!**
在 $solrhome/server/solr/  下新建文件夹 new_core ,将 $solrhome/server/solr/ 下的配置文件复制到新建的核心文件夹下

```bash
cd /usr/local/solr/server/solr

[root@localhost solr]# cp -r configsets/_default/  ./new_core
[root@localhost solr]# ls 
configsets  new_core    solr.xml   zoo.cfg
filestore   README.txt  userfiles
```

开启solrcloud

```bash
[root@localhost solr]# bin/solr start -cloud -force
Waiting up to 180 seconds to see Solr running on port 8983 [/]  
Started Solr server on port 8983 (pid=60660). Happy searching!
```
在solr模式下,一个集合对应一个核心,因此创建一个集合就会创建一个核心
>创建集合可以使用adminUI或者使用命令~create

**create创建核心**
-c <核心/集合名>
要创建的核心或集合的名称（必填）。

示例：bin/solr create -c mycollection-

![在这里插入图片描述](https://img-blog.csdnimg.cn/1ec273a9f6224b73943f8ba7c9ffd776.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**adminUI创建 ~略**


检查核心状态~

```bash
 collection: emps slice: shard1 saw state=DocCollection(emps//collections/emps/state.json/5 )={
  "pullReplicas":"0",
  "replicationFactor":"1",
  "shards":{"shard1":{
      "range":"80000000-7fffffff",
      "state":"active",
      "replicas":{"core_node2":{
          "core":"emps_shard1_replica_n1",
          "node_name":"192.168.135.145:8983_solr",
          "base_url":"http://192.168.135.145:8983/solr",
          "state":"down",
          "type":"NRT",
          "force_set_state":"false",
          "leader":"true"}}}},
  "router":{"name":"compositeId"},
  "maxShardsPerNode":"1",
  "autoAddReplicas":"false",
  "nrtReplicas":"1",
  "tlogReplicas":"0"} with live_nodes=[]

```

向集合中导入数据~

```bash
[root@localhost solr]# bin/post  -c emp example/exampledocs/emp.json
```

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/50068b159e8d4c01a1cfc30a1cff9829.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
 查询数据~
![在这里插入图片描述](https://img-blog.csdnimg.cn/b97a4523e6da480280c5c9422d59e398.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)



## 问题汇总

**1,导入数据库数据失败/查不到数据?**


情景复现
:  dataimport--->>

```bash
Indexing completed. Added/Updated: 2 documents. Deleted 0 documents. (Duration: 01s)
Requests: 1 1/s, Fetched: 0 2/s, Skipped: 0 , Processed: 2 2/s
Started: less than a minute ago
```
1,无论怎么刷新依旧是0;
2,检查配置的Fieldname / FieldType 均正确

solr运行环境
:  linux环境~fedora35 ,JDK8
:  内核 5.0以上



>原因肯能是数据库的用户没有权限访问 表格


授权之后可以正常出结果了;

```sql
mysql> grant all on *.* to 'gavin'@'%' ;
Query OK, 0 rows affected (0.00 sec)

mysql>
```