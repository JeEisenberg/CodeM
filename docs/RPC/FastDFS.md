# FastDFS

## FastDFS介绍

<div  align="center" ><iframe   style="width: 648px; height: 502px;" src="https://static-40147539-d8ed-4ebd-857f-0de35f75fd82.bspapp.com/"></iframe></div>

## 项目架构的演变

图片上传到哪个服务器上,其他项目需要通过http请求来从该服务器上获取图片;
这样存在的问题:
1,图片的存储相对分散
2,图片存储数量多的服务器在高并发情况下压力会很大;
>当访问图片时可能涉及到跨域的问题;
>![在这里插入图片描述](https://img-blog.csdnimg.cn/5757ca349a804a419c232620c4ce3ca6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


针对以上问题,需要单独搭建一个图片服务器,专门做图片存储和访问的;
>当我们访问网站时,会发现图片和业务服务器分离; ~csdn

![在这里插入图片描述](https://img-blog.csdnimg.cn/eae46ea669fc47d6b9da9590ea80a4ac.png)


## 图片处理服务器
**分布式文件存储系统**
通用分布式文件系统~lustre、MooseFS
优点:
>标准文件系统操作方式，对开发者门槛较低

缺点:
>系统复杂性较高，需要支持若干标准(POSIX标准)的文件操作，如：目录结构、文件读写权限、文件锁等。实际开发时的复杂性更高,对整体的系统性能有所影响;


**专门的分布式文件系统**

这种分布式问价系统是基于google File System的思想，**文件上传后不能修改**。需要使用**专有API**对文件进行**访问**，也可称作分布式文件存储服务。典型代表：MogileFS、FastDFS、TFS。

 优点:
>系统复杂性较低，不需要支持若干标准的文件操作，如：目录结构、文件读写权限、文件锁等，系统**比较简洁**。
>**系统整体性能较高**，因为**无需支持POSIX标准**，可以省去支持POSIX引入的环节，**系统更加高效**。

缺点:
>采用专有API，对开发者门槛较高（直接封装成工具类）

**Google FS 体系结构**
1,索引服务器,即文件地址存储服务器
2,存储服务器,文件存储服务器

文件分块存储,一个文件可以存储多份,至于存储到那个服务器上通常采用动态分配的形式;


这里要学习一下FastDFS,类似于google FS的一个轻量级分布式文件系统，纯C实现，支持Linux、FreeBSD、AIX等UNIX系统。


## FastDFS

![在这里插入图片描述](https://img-blog.csdnimg.cn/f0bb5745171949b7a6e4b2aface8ed77.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
 - FastDFS主要解决了**文件存储与高并发访问的问题**,文件的存取实现了**负载均衡,**实现了软件方式的磁盘阵列,可以使用廉价的硬盘存储解决方案(对硬件的要求不高);
 - FastDFS**只能通过Client API访问**，不支持POSIX访问方式。
 - FastDFS特别**适合大中型网站**使用，用来存储资源文件（如：图片、文档、音频、视频等等）

[FastDFS~下载](https://sourceforge.net/projects/fastdfs/)
[FastDFS~FAQ](http://bbs.chinaunix.net/thread-1920470-1-1.html)

## FastDFS的架构

![在这里插入图片描述](https://img-blog.csdnimg.cn/13b970a2383b45a499afb94adcbdb04d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)架构简述:
1,client在访问图片时回显访问tracker服务器去寻找图片的所在group和storage服务器的状态信息;

 - FastDFS只有两个角色~trackerserver与storage server, 不需要存储文件的索引信息;
 - 所有的服务器都是对等的,没有主从关系;
 - 存储服务器采取分组的形式,同组内的服务器上的文件完全相同,不同组的存储服务器之间不会相互通信;
 - 由storage server**主动向**tracker server**报告状态信息**，tracker server之间不会相互通信


## FastDFS环境搭建
FastDFS是用C写的,素以在搭建环境时要引入C的环境
> yum install -y make cmake gcc gcc-c++

 - 准备FastDFS运行时的函数库~

解压FastDFS的相关文件-->>libfastcommon-master.zip
该文件是FastDFS需要的公共C函数库
![在这里插入图片描述](https://img-blog.csdnimg.cn/1d81cdd69b9f4d5d8a9a201ba6f416a6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)**安装步骤~**

 - 1,执行脚本文件编译~   `./make.sh`

![在这里插入图片描述](https://img-blog.csdnimg.cn/1b88fdb353b8485ca37c6174c72e1c16.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - 2,执行脚本文件安装~  `./make.sh install`

![在这里插入图片描述](https://img-blog.csdnimg.cn/1619789268924e968a22535af1fbef9e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)使得函数库生效~创建软连接~
> ln -s /user/lib64/libfastcommon.so /usr/local/lib/libfastcommon.so
> ln -s /usr/local/lib64/libfdfsclient.so /usr/local/lib/libfdfsclient.so

 **安装FastDFS**
 - 1,上传FastDFS文件

>`cd/usr/local/`

>使用rz目录上传FastDFS文件

>解压文件 `tar -zxf FastDFS_v5.08.tar.gz`![ t](https://img-blog.csdnimg.cn/bd1a96f5920f44f4a04313d7c2efd334.png)

 - 2,进入解压后文件夹  `cd FastDFS/`

>编译 FastDFS    `./make.sh`
>安装FastDFS    `./make.sh install`


![在这里插入图片描述](https://img-blog.csdnimg.cn/eb3aab6328fc492b8c9b7927726eabaf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)安装后的文件夹解析~

 - /usr/bin  可执行文件所在的位置

 - /etc/fdfs  配置文件所在的位置
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/e5a2f47877f4426aaef49ece79f104d3.png)    

 - usr/lib64 主程序代码所在位置
 - /usr/include/fastdfs 包含一些插件组所在的位置
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/fd02cd017f284440aff35ec3e5e74489.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

**配置tracker**

 -1, 进入FastDFS配置文件夹~`cd /etc/fdfs`

新建配置文件cat tracker.conf,
>为了简便起见可以复制一份配置模板~`cp  tracker.conf.sample  tracker.conf`

 - 2,创建数据目录~

在FastDFS安装目录下创建数据存储目录~ `mkdir -p /usr/local/FastDFS/tracker`

 - 3,修改配置文件~tracker.conf,

这里主要修改数据存储的配置~剩余的用默认的配置;
默认端口:22122
![在这里插入图片描述](https://img-blog.csdnimg.cn/43f9916daa3a46aebdcadcff99a8b415.png)

```c
#the base path to store data and log files
base_path=/usr/local/FastDFS/tracker
```

 - 4,启动服务  ~`service fdfs_trackerd start`

```c
[root@localhost FastDFS]# service fdfs_trackerd start
Reloading systemd:                                         [  OK  ]
Starting fdfs_trackerd (via systemctl):                    [  OK  ]
[root@localhost FastDFS]# 

```

 - 5,查看服务状态~ `service fdfs_trackerd status`

```c
[root@localhost FastDFS]# service fdfs_trackerd status
● fdfs_trackerd.service - LSB: FastDFS tracker server
   Loaded: loaded (/etc/rc.d/init.d/fdfs_trackerd; generated)
   Active: active (running) since Thu 2022-02-24 18:20:01 PST; 1min 29s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 88913 ExecStart=/etc/rc.d/init.d/fdfs_trackerd start (code=exited, status=0/SUCCESS)
    Tasks: 7 (limit: 23528)
   Memory: 1.5M
   CGroup: /system.slice/fdfs_trackerd.service
           └─88920 /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf

Feb 24 18:20:01 localhost.localdomain systemd[1]: Starting LSB: FastDFS tracker server...
Feb 24 18:20:01 localhost.localdomain fdfs_trackerd[88913]: Starting FastDFS tracker server:
Feb 24 18:20:01 localhost.localdomain systemd[1]: Started LSB: FastDFS tracker server.

```
**关闭防火墙~**

   >service iptables stop

   >chkconfig iptables off

![在这里插入图片描述](https://img-blog.csdnimg.cn/b3b565226bfd49a2a935395bd21762ca.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


**配置storeage**

>storage可以和tracker不在同一台服务器上。

配置在两台服务器上,则另一台也需要安装fastDfs,配置方法跟前面一样,
这里为方便,将storage可以和tracker配置在一台服务器上;

 - 1,创建两个目录用于存放基础数据和日志文件

```c
[root@localhost FastDFS]# mkdir /usr/local/FastDFS/storage/base
[root@localhost FastDFS]# mkdir /usr/local/FastDFS/storage/store
```

 -2, 配置文件修改~

```c
 [root@localhost fdfs]# cd /etc/fdfs/
[root@localhost fdfs]# ls
client.conf.sample  storage.conf.sample  tracker.conf  tracker.conf.sample
[root@localhost fdfs]# cp storage.conf.sample  storage.conf
```

**做如下配置,其他配置保持默认**

```c
# the base path to store data and log files
# 保存storage server 基础数据内容和日志内容的目录。
base_path=/usr/local/FastDFS/storage/base

# store_path#, based 0, if store_path0 not exists, it's value is base_path
# 如果不指定该目录,则会按照base_path值配置
# 是用于保存FastDFS中存储文件的目录，就是要创建256*256个子目录的位置。
# the paths must be exist
store_path0=/usr/local/FastDFS/storage/store

# tracker_server can ocur more than once, and tracker_server format is
#  "host:port", host can be hostname or ip address
# 跟踪服务器的IP和端口。
tracker_server=192.168.135.145:22122 #这里是tracker服务器Ip+端口号
```

 - 3,启动storage服务

> service fdfs_storaged start


 - 4,查看服务状态

> service fdfs_storaged status

```c
[root@localhost fdfs]# service fdfs_storaged start
Starting fdfs_storaged (via systemctl):                    [  OK  ]
[root@localhost fdfs]# service fdfs_storaged status
● fdfs_storaged.service - LSB: FastDFS storage server
   Loaded: loaded (/etc/rc.d/init.d/fdfs_storaged; generated)
   Active: active (running) since Thu 2022-02-24 19:20:37 PST; 30s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 89537 ExecStart=/etc/rc.d/init.d/fdfs_storaged start (code=exited, status=0/SUCCESS)
    Tasks: 9 (limit: 23528)
   Memory: 71.1M
   CGroup: /system.slice/fdfs_storaged.service
           └─89544 /usr/bin/fdfs_storaged /etc/fdfs/storage.conf

Feb 24 19:20:37 localhost.localdomain systemd[1]: Starting LSB: FastDFS storage server...
Feb 24 19:20:37 localhost.localdomain fdfs_storaged[89537]: Starting FastDFS storage server:
Feb 24 19:20:37 localhost.localdomain systemd[1]: Started LSB: FastDFS storage server.
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/26cf7a3135e04279a0a720376ea940da.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - 安装总结~
  1. 安装FastDfS依赖的c语言的环境**libfastcommon-master.zip**
  2. 安装FastDfs
  3. tracker配置~配置文件并没有跟FastDfs安装目录在一起,而是在 etc/fdfs文件夹下,配置数据存放目录base_path
  4. storeage配置~可以跟tracker同服务器,也可以不同,配置base_path跟store_path已经tracker_server
  [资源打包下载](https://download.csdn.net/download/weixin_54061333/82303837)

## 文件上传下载

文件的上传下载图解~
**文件上传**
![在这里插入图片描述](https://img-blog.csdnimg.cn/d3ad48760d874e4297c2eb2bb19d6666.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

  1. 客户端访问Tracker
  2. Tracker返回**Storeage的ip和端口**
  3. 客户端直接访问storeage,**把文件和元数据发送过去**
  4. storerage返回文件存储的id,包含组名和文件名,组名默认为group1
>该id包含组名磁盘目录以及文件名

**代码实现**

新建maven项目,导入依赖

```xml
<dependencies>
    <dependency>
        <groupId>cn.bestwu</groupId>
        <artifactId>fastdfs-client-java</artifactId>
        <version>1.27</version>
    </dependency>
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>3.4</version>
    </dependency>
</dependencies> `
```

配置文件~fdfs_client.conf

```c
connect_timeout= 10
network_timeout= 30
charset= UTF-8
http.tracker_http_port = 8080
tracker_server = 192.168.135.145:22122   
```

导入工具类~FastDFSClient
可以自己封装也可以使用别人封装好的
FastDFS工具类~

```java
package com.gavin.utils;
import java.io.ByteArrayInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;

import org.apache.commons.lang3.StringUtils;
import org.csource.common.NameValuePair;
import org.csource.fastdfs.ClientGlobal;
import org.csource.fastdfs.StorageClient;
import org.csource.fastdfs.StorageClient1;
import org.csource.fastdfs.StorageServer;
import org.csource.fastdfs.TrackerClient;
import org.csource.fastdfs.TrackerServer;

/**
 * FastDFS分布式文件系统操作客户端.
 */
public class FastDFSClient {

    private static final String CONF_FILENAME = Thread.currentThread().getContextClassLoader().getResource("").getPath() + "fdfs_client.conf";

   // private static final String CONF_FILENAME = "D:\\Program Data\\idea2019Data\\FastDFSDemo1\\MyFastDFSdemo1\\src\\main\\resources\\fdfs_client.conf";

    private static StorageClient storageClient = null;

    /**
     * 只加载一次.
     */
    static {
        try {
            ClientGlobal.init(CONF_FILENAME);
            TrackerClient trackerClient = new TrackerClient(ClientGlobal.g_tracker_group);
            TrackerServer trackerServer = trackerClient.getConnection();
            StorageServer storageServer = trackerClient.getStoreStorage(trackerServer);
            storageClient = new StorageClient(trackerServer, storageServer);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     *
     * @param inputStream
     *    上传的文件输入流
     * @param fileName
     *    上传的文件原始名
     * @return
     */
    public static String[] uploadFile(InputStream inputStream, String fileName) {
        try {
            // 文件的元数据
            NameValuePair[] meta_list = new NameValuePair[2];
            // 第一组元数据，文件的原始名称
            meta_list[0] = new NameValuePair("file name", fileName);
            // 第二组元数据
            meta_list[1] = new NameValuePair("file length", inputStream.available()+"");
            // 准备字节数组
            byte[] file_buff = null;
            if (inputStream != null) {
                // 查看文件的长度
                int len = inputStream.available();
                // 创建对应长度的字节数组
                file_buff = new byte[len];
                // 将输入流中的字节内容，读到字节数组中。
                inputStream.read(file_buff);
            }
            // 上传文件。参数含义：要上传的文件的内容（使用字节数组传递），上传的文件的类型（扩展名），元数据
            String[] fileids = storageClient.upload_file(file_buff, getFileExt(fileName), meta_list);
            return fileids;
        } catch (Exception ex) {
            ex.printStackTrace();
            return null;
        }
    }

    /**
     *
     * @param file
     *            文件
     * @param fileName
     *            文件名
     * @return 返回Null则为失败
     */
    public static String[] uploadFile(File file, String fileName) {
        FileInputStream fis = null;
        try {
            // new NameValuePair[0];
            NameValuePair[] meta_list = null;
            fis = new FileInputStream(file);

            byte[] file_buff = null;
            if (fis != null) {
                int len = fis.available();
                file_buff = new byte[len];
                fis.read(file_buff);
            }

            String[] fileids = storageClient.upload_file(file_buff, getFileExt(fileName), meta_list);
            return fileids;
        } catch (Exception ex) {
            return null;
        }finally{
            if (fis != null){
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 根据组名和远程文件名来删除一个文件
     *
     * @param groupName
     *            例如 "group1" 如果不指定该值，默认为group1
     * @param remoteFileName
     *            例如"M00/00/00/wKgxgk5HbLvfP86RAAAAChd9X1Y736.jpg"
     * @return 0为成功，非0为失败，具体为错误代码
     */
    public static int deleteFile(String groupName, String remoteFileName) {
        try {
            int result = storageClient.delete_file(groupName == null ? "group1" : groupName, remoteFileName);
            return result;
        } catch (Exception ex) {
            return 0;
        }
    }

    /**
     * 修改一个已经存在的文件
     *
     * @param oldGroupName
     *            旧的组名
     * @param oldFileName
     *            旧的文件名
     * @param file
     *            新文件
     * @param fileName
     *            新文件名
     * @return 返回空则为失败
     */
    public static String[] modifyFile(String oldGroupName, String oldFileName, File file, String fileName) {
        String[] fileids = null;
        try {
            // 先上传
            fileids = uploadFile(file, fileName);
            if (fileids == null) {
                return null;
            }
            // 再删除
            int delResult = deleteFile(oldGroupName, oldFileName);
            if (delResult != 0) {
                return null;
            }
        } catch (Exception ex) {
            return null;
        }
        return fileids;
    }

    /**
     * 文件下载
     *
     * @param groupName 卷名
     * @param remoteFileName 文件名
     * @return 返回一个流
     */
    public static InputStream downloadFile(String groupName, String remoteFileName) {
        try {
            byte[] bytes = storageClient.download_file(groupName, remoteFileName);
            InputStream inputStream = new ByteArrayInputStream(bytes);
            return inputStream;
        } catch (Exception ex) {
            return null;
        }
    }

    public static NameValuePair[] getMetaDate(String groupName, String remoteFileName){
        try{
            NameValuePair[] nvp = storageClient.get_metadata(groupName, remoteFileName);
            return nvp;
        }catch(Exception ex){
            ex.printStackTrace();
            return null;
        }
    }

    /**
     * 获取文件后缀名（不带点）.
     *
     * @return 如："jpg" or "".
     */
    private static String getFileExt(String fileName) {
        if (StringUtils.isBlank(fileName) || !fileName.contains(".")) {
            return "";
        } else {
            // 不带最后的点
            return fileName.substring(fileName.lastIndexOf(".") + 1);     }
    }
}
```

测试代码~

```java
package com.gavin.client;

import org.csource.fastdfs.*;
public class FastDfsTest {
    public static void main(String[] args) {
        uploadfile();
    }


    private static void uploadfile() {
       //引入依赖
        // 加载配置文件。
        try {
            ClientGlobal.init("D:\\Program Data\\idea2019Data\\FastDFSDemo1\\MyFastDFSdemo1\\src\\main\\resources\\fdfs_client.conf");

            // new 一个tracker客户端对象
            TrackerClient trackerClient = new TrackerClient();
          
            TrackerServer trackerServer = null;
           //tracker客户端获得链接
            trackerServer = trackerClient.getConnection();            
            StorageServer storageServer = null;
          
            StorageClient storageClient = new StorageClient(trackerServer, storageServer);
            // 7、直接调用storageClient 对象方法上传文件即可。
            String[] strings = storageClient.upload_file("D:\\nvdi.png", "png", null);//该数组里包含文件所在位置和元数据信息;
            for (String string : strings) {
                System.out.println(string);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
运行结果~
group1为组名;
M00为磁盘
00为目录
wKiHkmIY1yeAG3hEAA_qkO7gJsA482.png为文件名
![在这里插入图片描述](https://img-blog.csdnimg.cn/693b92f274ff46c0985cda553b4a4766.png)
**实际存储位置~**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f6de6b26f503481a88b01c2157dfc33b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)


 - 上传文件的交互过程:

>1,客户端联系tracker服务器,tracker服务器返回一个可用的storaged的 ip+端口号
>2,客户端联系storage完成文件的上传


**文件下载**![在这里插入图片描述](https://img-blog.csdnimg.cn/42e253360ec4420393982de8bcf01db0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

 - 文件下载的交互过程:

>1,下载时客户端与tracker联系.获取文件标识(组名+文件名),并返回一台可用的storage
>2,客户端与storage联系下载文件;


代码实现~

```java
package com.gavin.client;
import com.gavin.utils.FastDFSClient;
import java.io.*;
import java.util.UUID;

/**
 * @author Gavin
 */
public class Myclient {
    public static void main(String[] args) {
        try {
            InputStream is = FastDFSClient.downloadFile("group1", "M00/00/00/wKiHkWIY1ZiARDEXAA_qkO7gJsA671.png");
            OutputStream os = new FileOutputStream(new File("D:/gavin.png"));
            int index = 0 ;
            while((index = is.read())!=-1){
                os.write(index);
            }
            os.flush();
            os.close();
            is.close();
        } catch (Exception e) {
        }
    }
}

```
运行结果~
![在这里插入图片描述](https://img-blog.csdnimg.cn/587683e0bfaa455cb8eb08d6bf6faced.png)
总结~
总体上文件的的上传下载比较容易实现,比较麻烦的时fastdfs的配置

## FastDFS配置详解

| **[FastDFS配置详解~](http://bbs.chinaunix.net/thread-1920470-1-1.html)** |
| ------------------------------------------------------------ |
| **[配置详解文件下载](https://download.csdn.net/download/weixin_54061333/82376007)** |





![](..\pic\div\plant.gif)

